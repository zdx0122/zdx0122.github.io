---
layout: post
title:  "【开源】访问任意URL并截图"
date:   2024-10-30
categories: SpringBoot Java Playwright
tags:  SpringBoot Java Playwright
author: i.itest.ren
---

* content
{:toc}

之前看到国外有一个访问URL自动给截图的产品，看上去很简单，所以就编码实现了下，果然很简单，而且用chatgpt帮忙写了大部分的代码






## 背景
之前看到国外有一个访问URL自动给截图的产品，看上去很简单，所以就编码实现了下，果然很简单，而且用chatgpt帮忙写了大部分的代码

## 演示
demo演示：[https://screenshot.itest.ren/api/screenshot?url=http://i.itest.ren](https://screenshot.itest.ren/api/screenshot?url=http://i.itest.ren)

替换掉url参数后的value为你想截图的网站域名即可

![自动截图](https://img.1024996.xyz/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20241031172626.png)

---

## 实现
1. 最核心的内容，就是使用playwright的headless模式进行访问URL并截图：

```java
public CompletableFuture<File> takeScreenshot(String url) {
    // 异步执行截图任务，提交到线程池
    return CompletableFuture.supplyAsync(() -> {
        // 创建一个新的页面
        Page page = browser.newPage();
        page.navigate(url);
 
        page.waitForLoadState(NETWORKIDLE);
 
        String modifiedUrl = url.replaceAll("https?://", "");
        // 生成截图文件
        Path screenshotPath = Paths.get("screenshot-" + modifiedUrl + "-"+ System.currentTimeMillis() + ".png");
        page.screenshot(new Page.ScreenshotOptions().setPath(screenshotPath));
 
        // 关闭页面
        page.close();
 
        return screenshotPath.toFile();
    }, executorService);
}
```

2. 之后使用Controller暴露出来SpringBoot的服务

```java
@GetMapping("/api/screenshot")
public CompletableFuture<ResponseEntity<byte[]>> getScreenshot(@RequestParam String url) {
    // 异步调用 ScreenshotService
    return screenshotService.takeScreenshot(url).thenApply(screenshot -> {
        try {
            // 将截图文件转换为字节数组
            byte[] imageBytes = Files.readAllBytes(screenshot.toPath());
 
            // 设置响应头为 image/png
            HttpHeaders headers = new HttpHeaders();
            headers.add(HttpHeaders.CONTENT_TYPE, "image/png");
 
            // 返回截图数据
            return new ResponseEntity<>(imageBytes, headers, HttpStatus.OK);
        } catch (IOException e) {
            return new ResponseEntity<>(HttpStatus.INTERNAL_SERVER_ERROR);
        }
    });
}
```


## 代码开源
完整代码已开源，可自行搭建部署：[https://github.com/zdx0122/playwright-screenshot-api](https://github.com/zdx0122/playwright-screenshot-api)

欢迎交流和提idea，可迭代开发