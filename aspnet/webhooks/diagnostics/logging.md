---
uid: webhooks/diagnostics/logging
title: "ログ記録 ASP.NET Webhook |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET Webhook でのログ記録を行う方法です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET の Webhook のログ記録

Microsoft ASP.NET の Webhook は、問題やエラーを報告する方法として、ログ記録を使用します。 既定でログが書き込まれるを使用して[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)を使用している可能性がある管理[トレース リスナー](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)などの他のログ ストリーム。

Azure Web アプリと Web アプリケーションを展開するときに、ログが自動的に取得され、と共に、他の管理されていることができます[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)ログ記録します。 詳細については、「 [Azure App service web アプリの診断ログ記録有効にする](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

さらに、ログはから直接取得できます」の説明に従って、Visual Studio 内[Visual Studio を使用して Azure App service web アプリのトラブルシューティングを行う](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)です。

## <a name="redirecting-logs"></a>ログをリダイレクトします。

ログを記述する代わりに[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)など、ログ マネージャーに直接ログインできる代替のログ記録の実装を提供することは[Log4Net](http://logging.apache.org/log4net/)と[NLog](http://nlog-project.org/). 実装を提供するだけ[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)して任意の依存関係の挿入エンジンに登録し、Microsoft ASP.NET Webhook によってピックアップを取得します。 参照してください[ASP.NET Web API 2 で依存性の注入](https://www.asp.net/web-api/overview/advanced/dependency-injection)詳細についてはします。
