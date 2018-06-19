---
title: ASP.NET と ASP.NET Core の選択
author: rick-anderson
description: ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094436"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>ASP.NET と ASP.NET Core の選択

Windows Server を対象とするエンタープライズ Web アプリから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。

## <a name="aspnet"></a>ASP.NET

ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ グレードのサーバー ベース Web アプリを構築するために必要なすべてのサービスを提供します。

## <a name="framework-selection"></a>フレームワークの選択

次の表で、どのフレームワークが最もニーズに適しているかをご確認ください。

| ASP.NET Core | ASP.NET |
|---|---|
|Windows、macOS、Linux が対象|Windows が対象|
|[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。 [MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api)、[SignalR](xref:signalr/introduction) についても参照してください。|[Web フォーム](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/)、または [Web ページ](/aspnet/web-pages)を使います。|
|コンピューターごとに複数のバージョン|コンピューターごとに 1 つのバージョン|
|Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します|Visual Studio で C#、VB、または F# を使って開発します|
|ASP.NET より高いパフォーマンス|よいパフォーマンス|
|[.NET Framework または .NET Core ランタイムを選択します](/dotnet/articles/standard/choosing-core-framework-server)|.NET Framework ランタイムを使います|

## <a name="aspnet-core-scenarios"></a>ASP.NET Core のシナリオ

* [Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。
* [Web サイト](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [リアルタイム](xref:signalr/index)

## <a name="aspnet-scenarios"></a>ASP.NET のシナリオ

* [Web サイト](/aspnet/mvc)
* [API](/aspnet/web-api)
* [リアルタイム](/aspnet/signalr)

## <a name="resources"></a>リソース

* [ASP.NET の概要](/aspnet/overview)
* [ASP.NET Core の概要](xref:index)
