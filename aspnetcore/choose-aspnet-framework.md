---
title: ASP.NET と ASP.NET Core の選択
author: rick-anderson
description: ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: e6ac9f54ef623895b81eea33d90791e5f0ad5120
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>ASP.NET と ASP.NET Core の選択

Windows Server を対象とするエンタープライズ Web アプリから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。

## <a name="aspnet"></a>ASP.NET

ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ クラスのサーバー ベース Web アプリを構築するために必要なすべてのサービスを提供します。

## <a name="which-one-is-right-for-me"></a>どちらが適していますか?

| ASP.NET Core | ASP.NET |
|---|---|
|Windows、macOS、Linux が対象|Windows が対象|
|[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。 [MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api)、[SignalR](xref:signalr/introduction) についても参照してください。|[Web フォーム](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、または [Web ページ](/aspnet/web-pages)を使います|
|コンピューターごとに複数のバージョン|コンピューターごとに 1 つのバージョン|
|Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します|Visual Studio で C#、VB、または F# を使って開発します|
|ASP.NET より高いパフォーマンス|よいパフォーマンス|
|[.NET Framework または .NET Core ランタイムを選択します](/dotnet/articles/standard/choosing-core-framework-server)|.NET Framework ランタイムを使います|

## <a name="aspnet-core-scenarios"></a>ASP.NET Core のシナリオ

<!-- update link to Razor Pages mvc movie series when done -->
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
