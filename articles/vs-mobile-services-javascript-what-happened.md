<properties 
	pageTitle="通过使用 Visual Studio 连接服务将移动服务添加到 Javascript 应用时会发生什么情况？" 
	description="描述 Visual Studio 中的 Azure 移动服务项目发生了什么情况" 
	services="mobile-services" 
	documentationCenter="" 
	authors="patshea123" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.date="09/23/2015" 
	wacn.date="11/27/2015"/>

# 当我使用连接的 Visual Studio 服务添加 Azure 移动服务时，我的 Javascript 项目会发生什么情况？

> [AZURE.SELECTOR]
> - [Getting Started](/documentation/articles/vs-mobile-services-javascript-getting-started)
> - [What Happened](/documentation/articles/vs-mobile-services-javascript-what-happened)

###我的项目发生了什么情况？

#####添加了 NuGet 包

已安装 **WindowsAzure.MobileServices.WinJS** NuGet 包，包括 `js\MobileServices.js` 文件中的 Azure 移动服务库。
  
#####移动服务的连接字符串值 

在 `services\mobileServices\settings` 文件夹中，已生成包含 **MobileServiceClient** 的新 JavaScript (.js) 文件，其中包含选定的移动服务的应用程序 URL 和应用程序密钥。


#####添加了对 default.html 的引用

已将对 `MobileServices.js` 的引用和设置文件添加到 `default.html`。


#####添加了连接服务文件

在服务文件夹中，添加了连接服务配置文件。



 

<!---HONumber=82-->