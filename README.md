﻿
<!-- TOC -->

- [智图网客户端系统](#智图网客户端系统)
- [Zoomla!逐浪CMS卓越出品](#zoomla逐浪cms卓越出品)
    - [精品网页项目](#精品网页项目)
        - [禁用右键:](#禁用右键)
        - [解决跨域资源请求问题](#解决跨域资源请求问题)
        - [调式模式配置](#调式模式配置)
        - [白屏](#白屏)

<!-- /TOC -->


# 智图网客户端系统

> 基于cefsharp



<p align="center">
  <a href="http://www.z01.com/">
    <img src="https://code.z01.com/img/zoomla_logo.svg" width="600">
  </a>
</p>
<br>


# Zoomla!逐浪CMS卓越出品


项目实际运行效果：
https://zoomla.gitee.io/laoweb

## 精品网页项目

Zoomla!逐浪CMS：中文业界alexa排名第一的CMS系统|专注.net与windows平台企业级研发，集成内容管理、webfont、商城、店铺、黄页、教育、考试、3D、三维全景、混合现实、CRM、ERP、OA、论坛、贴吧等为一体，打造国内高端的CMS产品典范。

官网：www.z01.com

免费下载：www.z01.com/mb

视频教程：www.z01.com/mtv

模板资源：www.z01.com/mb

逐浪字库： http://f.ziti163.com

zico中文图标库：http://ico.z01.com


QQ交流群号：
[![加入QQ群](https://img.shields.io/badge/一群-541450128-blue.svg?style=for-the-badge&logo=appveyor)](https://jq.qq.com/?_wv=1027&k=5qIayyX)  [![加入QQ群](https://img.shields.io/badge/二群-541450128-blue.svg?style=for-the-badge&logo=appveyor)](https://jq.qq.com/?_wv=1027&k=5Ephzpq)   [![加入QQ群](https://img.shields.io/badge/三群-601781959-blue.svg?style=for-the-badge&logo=appveyor)](https://jq.qq.com/?_wv=1027&k=50a28BK) 


官方QQ客服：
[![官方QQ客服1](https://img.shields.io/badge/官方QQ客服1-524979923-red.svg?style=for-the-badge&logo=appveyor)](http://wpa.qq.com/msgrd?v=3&uin=745151353&site=qq&menu=yes)  [![官方QQ客服2](https://img.shields.io/badge/官方QQ客服2-1799661890-red.svg?style=for-the-badge&logo=appveyor)](http://wpa.qq.com/msgrd?v=3&uin=1799661890&site=qq&menu=yes) 

###  禁用右键:
```
ChromiumWebBrowser.MenuHandler = new ContextMenuHandler();
/// <summary>
/// 内容菜单事件处理
/// </summary>
public class ContextMenuHandler : IContextMenuHandler
{
    public void OnBeforeContextMenu(IWebBrowser chromiumWebBrowser, IBrowser browser, IFrame frame, IContextMenuParams parameters, IMenuModel model)
    {
        model.Clear();
    }
    public bool OnContextMenuCommand(IWebBrowser chromiumWebBrowser, IBrowser browser, IFrame frame, IContextMenuParams parameters, CefMenuCommand commandId, CefEventFlags eventFlags)
    {
        return false;
    }
    public void OnContextMenuDismissed(IWebBrowser chromiumWebBrowser, IBrowser browser, IFrame frame)
    {
        //throw new NotImplementedException();
    }
    public bool RunContextMenu(IWebBrowser chromiumWebBrowser, IBrowser browser, IFrame frame, IContextMenuParams parameters, IMenuModel model, IRunContextMenuCallback callback)
    {
        //throw new NotImplementedException();
        return false;
    }
}
```

### 解决跨域资源请求问题
```
//方式一：实际测试过有效，缺点是会多一个文件夹
CefSettings.CefCommandLineArgs.Add("--disable-web-security", "");
CefSettings.CefCommandLineArgs.Add("--user-data-dir", "C:\\MyChromeDevUserData");

//方式二：实际测试过有效，比上面的方式好
ChromiumWebBrowser.BrowserSettings.WebSecurity = CefState.Disabled;
```
### 调式模式配置
客户端自主禁用
```
# global.config中配置下面节点
<Dev>true</Dev>
```

底层配置全局无法开启调式
```
# 注释 E\ZL_Photo\BrowserHandler\KeyboardHandler.cs line19-21
if (ConfigHelper.globalConfig.Dev && type == KeyType.RawKeyDown && windowsKeyCode == 123)
        {
            chromiumWebBrowser.ShowDevTools();
        }
```

### 白屏
CEFSharp加载完页面后会出现黑屏或者白屏现象，应该是渲染失败造成的，解决办法是初始化结束后刷新一次
```
 Browser.IsBrowserInitializedChanged += new EventHandler<CefSharp.IsBrowserInitializedChangedEventArgs>((s, a) =>
 {
    Browser.Refresh();
 });
```