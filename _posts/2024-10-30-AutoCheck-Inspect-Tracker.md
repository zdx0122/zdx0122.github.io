---
layout: post
title:  "自动化巡检系统"
date:   2024-10-30
categories: SpringBoot Java TestNg Playwright 自动化巡检
tags:  SpringBoot Java TestNg  Playwright 自动化巡检
author: i.itest.ren
---

* content
{:toc}

为方便“及时”感知H5页面服务是否存在稳定性问题，所以搭建一个基础巡检系统，用于检测H5页面是否正常，主动性感知页面异常从而快速处理问题，从而减少对线上服务的影响范围和时长。

基于`Testng + Playwright + SpringBoot + Vue + Ant-Design`，代码已开源在：[https://github.com/TheCoolQATeam/online-inspection-tracker](https://github.com/TheCoolQATeam/online-inspection-tracker)






## 出发点：遇到的问题、巡检必要性
- 页面白页
- 页面样式飞掉
- 页面无响应

遇到上述问题，有众多原因：
- 人为操作不当：配置文件错误、代码错误等
- 依赖服务异常
- 机房线路问题：阿里云崩了、腾讯云崩了等
- 机器原因：降配、磁盘不足等导致
- Nginx负载问题
- waf误封禁
- 域名被运营商封禁
- cdn原因
- 。。。

> 如图，今年的热搜：
![崩了](https://img.1024996.xyz/output.png)


- 内部的119反馈群中，某某城市页面打不开、崩了之类的反馈
- boss：技术团队在干什么，这么严重的问题都没发现

遇到上述情形，我想没有任何一个QA会轻松吧，都是泪

故，基于这些痛点，搭建一套自动化巡检系统的必要性不用多说吧

---

## 实现方案
整体使用Testng + Playwright + SpringBoot + Vue + Ant-Design搭建自动化巡检系统，有以下优势：
- 轻量：基于SpringBoot和VUE，使用前后端开发模式，便于把测试用例、数据报告等进行落库和前端展示，非常轻量，易于搭建
- 快速：基于Playwright，使用Playwright请求页面、截取页面元素、页面截图、性能分析、网络请求资源分析等，非常高效快速
- 稳定：基于Testng，使用Testng的断言、参数化、Listener监听、失败重试、数据报告等功能，易于上手

代码已开源在：[https://github.com/TheCoolQATeam/online-inspection-tracker](https://github.com/TheCoolQATeam/online-inspection-tracker)

## 主要巡检功能及核心源码

### 页面主要元素检测
请求页面后，对页面进行截图，检测页面的title是否正常，正常则代表页面HTML已进行渲染，否则代表页面HTML未渲染，页面无法正常访问。

```java
    @Test(description = "遍历页面可用状态", dataProvider = "HtmlData")
    public void testHtmlServiceability(int id, String htmlinfo, String title, String url, String dingKey, String wechatKey, String feishuKey) throws FileNotFoundException, UnknownHostException {
        page.navigate(url);
        long currentTimeMillis = System.currentTimeMillis();
        // 获取当前工作目录
        String userDir = System.getProperty("user.dir");
        String titleCleanInvalid = StrUtil.replace(StrUtil.replace(FileNameUtil.cleanInvalid(title), " ", "_"), "\t", "");
        String imageName = titleCleanInvalid.concat("_").concat(Long.toString(currentTimeMillis));
        logger.info("基准值地址"+imageName);
        // 使用String的concat()方法拼接路径
        String imagePath = userDir.concat(File.separator).concat("online-images").concat(File.separator).concat(imageName).concat(".png");
        page.waitForLoadState(LoadState.NETWORKIDLE); // 资源下载完毕
        page.screenshot(new Page.ScreenshotOptions().setPath(Paths.get(imagePath)));
        Assert.assertEquals(page.title(), title);
```

测试用例利用TestNg参数化功能从数据库中进行获取和转化为参数化类型，实现数据驱动；使用mybatis从数据库取数据
```java
    @DataProvider
    public Object[][] HtmlData() {
        List<OnlinesPatrol> onlinesPatrols = onlinesPatrolMapper.selectDate();
        if (onlinesPatrols == null) {
            return null;
        }
        Object[][] pageData = new Object[onlinesPatrols.size()][7];
        for (int i = 0; i < onlinesPatrols.size(); i++) {
            OnlinesPatrol onlinesPatrol = onlinesPatrols.get(i);
            pageData[i][0] = onlinesPatrol.getId();
            pageData[i][1] = onlinesPatrol.getHtmlinfo();
            pageData[i][2] = onlinesPatrol.getTitle();
            pageData[i][3] = onlinesPatrol.getUrl();
            pageData[i][4] = onlinesPatrol.getDingKey();
            pageData[i][5] = onlinesPatrol.getWechatKey();
            pageData[i][6] = onlinesPatrol.getFeishuKey();
        }
        return pageData;
    }
```

### 视觉回归

首先，把第一次请求的页面截图作为基准值，

然后，后续的每次请求的页面截图与基准值进行对比，如果存在差异，则代表页面有异常，异常则会发送钉钉、企微、飞书等报警，需要及时处理。

图片对比的阈值设置为60%，即如果两张图片的相似度低于60%，则认为两张图片有差异，代表页面有异常。
实际使用中，经过大量测试，可以确定该阈值可以满足需求。

```java
        OnlinesPatrol onlinesPatrol = onlinesPatrolMapper.selectByPrimaryKey(id);
        if (onlinesPatrol != null) { // 若无基准值
            if (onlinesPatrol.getDatumAddress() == null) {
                onlinesPatrol.setDatumAddress(imageName);
                onlinesPatrol.setDatumCreatetime(new Date());
                onlinesPatrolMapper.updateByPrimaryKey(onlinesPatrol);
            } else {
                String pic1 = imagePath;  // 本次图片
                logger.info("图片1的地址"+pic1);
                // 基准值图片
                String pic2 = userDir.concat(File.separator).concat("online-images").concat(File.separator).concat(onlinesPatrol.getDatumAddress()).concat(".png");//线上运行获取图片地址
                logger.info("图片2的地址"+pic2);
                String result = null;
                try {
                    result = imageComp.compareImage(pic2, pic1);
                } catch (MalformedURLException e) {
                    e.printStackTrace();
                }
                int xiangsi = Integer.parseInt(result);
                if (xiangsi > 60) {
                    Assert.assertTrue(true);
                    logger.info("图片对比相似率大于60:" + xiangsi);
                } else {
                    String ip = InetAddress.getLocalHost().getHostAddress();
                    logger.info("服务器IP地址：" + ip);
                    String picUrl = "http://" + ip + ":9091/patrol/onlines/images?imageName=" + imageName;
                    DingUtil.sendMsgPic(url, id, picUrl, title, dingKey);
                    WechatUtil.sendMsgPic(url, id, picUrl, title, wechatKey);
                    FeishuUtil.sendMsgPic(url, id, picUrl, title, feishuKey);
                    logger.info("图片对比相似率小于60:" + xiangsi);
                }
            }
```

### 性能分析
性能分析有两块，一块是计算测试用例的执行时间，另一块是通过playwright执行js脚本`window.performance.timing`计算页面加载时间

```java
// 获取性能数据
        Object performanceResult = page.evaluate("() => {\n" +
                "const timing = window.performance.timing; \n" +
                "return  JSON.stringify(timing.toJSON()); \n" +
                "}");
```

### 网络请求资源分析
playwright的`onRequest`方法可以监听到所有的请求，通过判断请求的url等信息是否是正常请求，如果不是正常请求，则发送报警。
```java
        noLoginPage.onRequest((request) -> {
            try {
                getTestUrl(idForEnv, request.url());
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
```

### 定时巡检
定时功能有很多方案，本次开源项目使用`ScheduledExecutorService`，较为轻量

设置定时执行间隔为5分钟进行巡检一次，每天会巡检288次。

## 开源
项目开源地址：https://github.com/TheCoolQATeam/online-inspection-tracker

欢迎一起迭代维护，记得star