---
title: ASP.NET Core MVC と Visual Studio の概要
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio の概要について説明します。
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862201"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>ASP.NET Core MVC と Visual Studio の概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

このチュートリアルには 3 つのバージョンがあります。

* macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)

> [!NOTE]
> ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。  現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。

## <a name="install-visual-studio-and-net-core"></a>Visual Studio と .NET Core のインストール

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Web アプリの作成

Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

**[新しいプロジェクト]** ダイアログで次のように設定します。

* 左側のウィンドウで、**[.NET Core]** をタップします。
* 中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。
* プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。
* **[OK]** をタップします

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project2-21.png)

**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。

* バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.1]** を選択します。
* **[Web アプリケーション (モデル ビュー コントローラー)]** を選択します
* **[OK]** をタップします。

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。 プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。 これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。

**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)

* Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図で、ポート番号は 5000 です。 ブラウザーの URL は `localhost:5000` を示します。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。
* **[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* **[IIS Express]** ボタンをタップしてアプリをデバッグできます

![IIS Express](start-mvc/_static/iis_express.png)

既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。 上のブラウザーの画像には、これらのリンクが表示されていません。 ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。

このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a>Web アプリの作成

Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

**[新しいプロジェクト]** ダイアログで次のように設定します。

* 左側のウィンドウで、**[.NET Core]** をタップします。
* 中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。
* プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。
* **[OK]** をタップします

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project2.png)

**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。

* バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します
* **[Web Application(Model-View-Controller)]** を選択します
* **[OK]** をタップします。

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project22.png)

Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。 プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。 これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。

**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)

* Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図で、ポート番号は 5000 です。 ブラウザーの URL は `localhost:5000` を示します。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。
* **[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* **[IIS Express]** ボタンをタップしてアプリをデバッグできます

![IIS Express](start-mvc/_static/iis_express.png)

既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。 上のブラウザーの画像には、これらのリンクが表示されていません。 ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。

このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。

::: moniker-end

> [!div class="step-by-step"]
> [次へ](adding-controller.md)  
