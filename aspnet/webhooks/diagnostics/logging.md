---
uid: webhooks/diagnostics/logging
title: ASP.NET Webhook のログ記録 |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook でのログ記録を行う方法。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 65e4d49474034406be835eb31378c81ba0706da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828456"
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="0768d-103">ASP.NET Webhook のログ記録</span><span class="sxs-lookup"><span data-stu-id="0768d-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="0768d-104">Microsoft ASP.NET Webhook を問題と問題を報告する方法として、ログ記録を使用します。</span><span class="sxs-lookup"><span data-stu-id="0768d-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="0768d-105">使用して既定でログが書き込まれます[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)を使用して管理できます[トレース リスナー](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)などの他の任意のログ ストリーム。</span><span class="sxs-lookup"><span data-stu-id="0768d-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="0768d-106">ログが自動的にピックアップし、と共にその他の管理することができます、Azure Web アプリとして Web アプリケーションをデプロイするときに[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)ログ記録します。</span><span class="sxs-lookup"><span data-stu-id="0768d-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="0768d-107">詳細については、「 [Azure App Service で web アプリに対して診断ログ有効にする](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="0768d-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="0768d-108">さらに、ログはから直接取得できます」の説明に従って、Visual Studio 内で[Visual Studio を使用して Azure App Service で web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)します。</span><span class="sxs-lookup"><span data-stu-id="0768d-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="0768d-109">ログをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0768d-109">Redirecting Logs</span></span>

<span data-ttu-id="0768d-110">ログを記述する代わりに[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)、ログ マネージャーに直接などにログインできるログ記録の代替実装を提供することは[Log4Net](http://logging.apache.org/log4net/)と[NLog](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="0768d-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="0768d-111">単の実装を提供[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)お好きな依存関係の注入エンジンに登録して、Microsoft ASP.NET Webhook によってピックアップを取得します。</span><span class="sxs-lookup"><span data-stu-id="0768d-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="0768d-112">参照してください[ASP.NET Web API 2 の依存関係挿入](https://www.asp.net/web-api/overview/advanced/dependency-injection)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="0768d-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
