---
title: "Visual Studio Code を使用する ASP.NET Core の Razor ページの概要"
author: rick-anderson
description: "Visual Studio Code を使用する ASP.NET Core の Razor ページの概要"
keywords: "ASP.NET Core,Razor ページ,スキャフォールディング,Entity Framework Core,EF,EF Core,データベース,mac,macOS,Visual Studio Code,Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Visual Studio Code を使用する ASP.NET Core の Razor ページの概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。 ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。

## <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

端末から、次のコマンドを実行します。

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

上のコマンドでは [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。 ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。

![ホームまたはインデックス ページ](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>プロジェクトを開く

Ctrl + C キーを押して、アプリケーションをシャットダウンします。

Visual Studio Code (VS Code) から、**[ファイル]、[フォルダーを開く]** の順に選択し、*RazorPagesMovie* フォルダーを選択します。

- "ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を 選択します。
- [There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。

### <a name="launch-the-app"></a>アプリの起動

Ctrl + F5 キーを押して、デバッグを行わずにアプリを開始します。 または、**[デバッグ]** メニューの **[デバッグなしで開始]** を選択します。

次のチュートリアルでは、プロジェクトにモデルを追加します。 

>[!div class="step-by-step"]
[次: モデルの追加](xref:tutorials/razor-pages-vsc/model)  
