---
title: "Mac、Linux、Windows の ASP.NET Core MVC の概要"
author: rick-anderson
description: "Mac、Linux、Windows の ASP.NET Core MVC と Visual Studio Code の概要"
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 82ce5bbc695b190759ac2c05cdceebb5f7854eb7
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/23/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Mac、Linux、Windows の ASP.NET Core MVC の概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、[Visual Studio Code](https://code.visualstudio.com) (VS Code) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。 このチュートリアルは VS Code の知識があることを前提としています。 詳細については、[VS Code の概要](https://code.visualstudio.com/docs)、および [Visual Studio Code のヘルプ](#visual-studio-code-help)に関するページを参照してください。 [!INCLUDE[consider RP](../../includes/razor.md)]

このチュートリアルには 3 つのバージョンがあります。

* macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>VS Code と .NET Core のインストール

このチュートリアルには、[.NET Core 2.0.0 SDK](https://dot.net/core) 以降が必要です。 ASP.NET Core 1.1 バージョンについては、[この PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) をご覧ください。

以下をインストールします。

* [.NET Core 2.0.0 SDK](https://dot.net/core) 以降。
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-web-app-with-dotnet"></a>dotnet での Web アプリの作成

端末から、次のコマンドを実行します。

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Visual Studio Code (VS Code) で *MvcMovie* フォルダーを開き、*Startup.cs* ファイルを選択します。

- "'MvcMovie' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を 選択します。
- [There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。

!["'MvcMovie' に作成とデバッグに必要な資産がありません。 追加しますか?" という警告メッセージが表示された VS Code [今後このメッセージを表示しない]、[後で行う]、[はい] と [情報] - [未解決の依存関係があります] - [復元] - [閉じる]](../web-api-vsc/_static/vsc_restore.png)

**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。

![実行中のアプリ](../first-mvc-app/start-mvc/_static/1.png)

VS Code で [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが起動し、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:5000` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。

既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。 上のブラウザーの画像には、これらのリンクが表示されていません。 ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。

![右上のナビゲーション アイコン](../first-mvc-app/start-mvc/_static/2.png)

このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。

## <a name="visual-studio-code-help"></a>Visual Studio Code ヘルプ

- [はじめに](https://code.visualstudio.com/docs)
- [デバッグ](https://code.visualstudio.com/docs/editor/debugging)
- [統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [ショートカット キー](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac ショートカット キー](https://go.microsoft.com/fwlink/?linkid=832143)
  - [Linux ショートカット キー](https://go.microsoft.com/fwlink/?linkid=832144)
  - [Windows ショートカット キー](https://go.microsoft.com/fwlink/?linkid=832145)

>[!div class="step-by-step"]
[次 - コントローラーの追加](adding-controller.md)
