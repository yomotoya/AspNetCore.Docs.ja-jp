---
title: "ASP.NET Core MVC と Visual Studio for Mac の概要"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio の概要"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 51c0477c40885d3aaaa7562f8baba0a94cb4f920
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>ASP.NET Core MVC と Visual Studio for Mac の概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。 

[!INCLUDE[consider RP](../../includes/razor.md)]

このチュートリアルには 3 つのバージョンがあります。

* macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app/start-mvc)
* Linux、macOS、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルには、[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降が必要です。

以下をインストールします。

- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降
- [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Web アプリの作成

Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。

![macOS の新しいソリューション](../first-web-api-mac/_static/sln.png)

**[.NET Core アプリ]、[ASP.NET Core]、[Web アプリ]、[次へ]** の順に選択します。

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/1.png)

プロジェクトに **MvcMovie** という名前を付けて、**[作成]** を選択します。

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/2.png)

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。 Visual Studio で [Kestrel](xref:fundamentals/servers/index#kestrel) が開始し、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。

![新しいプロジェクトでのブラウザー](start-mvc/b1.png)

* アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 アプリを実行する際には、別のポート番号が表示されます。
* **[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。

既定のテンプレートには、**[Home]、[About]**、**[Contact]** のリンクがあります。 上のブラウザーの画像には、これらのリンクが表示されていません。 ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。

![新しいプロジェクトでのブラウザー](start-mvc/b2.png)

このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。

>[!div class="step-by-step"]
[次へ](adding-controller.md)  
