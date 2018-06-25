---
title: ASP.NET と ASP.NET Core の選択
author: rick-anderson
description: ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297231"
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
|[Razor ページ](xref:razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。 [MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api)、[SignalR](xref:signalr/introduction) についても参照してください。|[Web フォーム](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/)、または [Web ページ](/aspnet/web-pages)を使います。|
|コンピューターごとに複数のバージョン|コンピューターごとに 1 つのバージョン|
|Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します|Visual Studio で C#、VB、または F# を使って開発します|
|ASP.NET より高いパフォーマンス|よいパフォーマンス|
|[.NET Framework または .NET Core ランタイムを選択します](/dotnet/articles/standard/choosing-core-framework-server)|.NET Framework ランタイムを使います|

## <a name="aspnet-core-scenarios"></a>ASP.NET Core のシナリオ

* [Razor ページ](xref:razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。
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
