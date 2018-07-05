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
# <a name="aspnet-webhooks-logging"></a>ASP.NET Webhook のログ記録

Microsoft ASP.NET Webhook を問題と問題を報告する方法として、ログ記録を使用します。 使用して既定でログが書き込まれます[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)を使用して管理できます[トレース リスナー](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)などの他の任意のログ ストリーム。

ログが自動的にピックアップし、と共にその他の管理することができます、Azure Web アプリとして Web アプリケーションをデプロイするときに[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)ログ記録します。 詳細については、「 [Azure App Service で web アプリに対して診断ログ有効にする](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

さらに、ログはから直接取得できます」の説明に従って、Visual Studio 内で[Visual Studio を使用して Azure App Service で web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)します。

## <a name="redirecting-logs"></a>ログをリダイレクトします。

ログを記述する代わりに[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)、ログ マネージャーに直接などにログインできるログ記録の代替実装を提供することは[Log4Net](http://logging.apache.org/log4net/)と[NLog](http://nlog-project.org/). 単の実装を提供[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)お好きな依存関係の注入エンジンに登録して、Microsoft ASP.NET Webhook によってピックアップを取得します。 参照してください[ASP.NET Web API 2 の依存関係挿入](https://www.asp.net/web-api/overview/advanced/dependency-injection)詳細についてはします。
