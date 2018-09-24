---
title: ASP.NET Core MVC と Visual Studio の概要
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio の概要について説明します。
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011707"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>ASP.NET Core MVC と Visual Studio の概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

このチュートリアルには 3 つのバージョンがあります。

* macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)

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
* **[Web Application(Model-View-Controller)]** を選択します
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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Visual Studio Community 2017 をインストールします。 コミュニティ ダウンロードを選択します。 Visual Studio 2017 をインストールしている場合は、この手順をスキップします。

* [Visual Studio 2017 ホーム ページのインストーラー](https://www.visualstudio.com/)

インストーラーを実行し、次のワークロードを選択します。

* **ASP.NET と Web 開発** (**[Web & Cloud]\(Web とクラウド\)** の下)
* **.NET Core クロスプラットフォームの開発** (**[他のツールセット]** の下)

![**ASP.NET と Web の開発ツール** (**[Web & Cloud]\(Web とクラウド\)** の下)](start-mvc/_static/web_workload.png)

![**.NET Core クロスクロスプラットフォームの開発** (**[他のツールセット]** の下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Web アプリの作成

Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

**[新しいプロジェクト]** ダイアログで次のように設定します。

* 左側のウィンドウで、**[.NET Core]** をタップします。
* 中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。
* プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。
* **[OK]** をタップします

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。

* バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します
* **[Web Application(Model-View-Controller)]** を選択します
* **[OK]** をタップします。

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。

* バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 1.1]** をタップします
* **[Web アプリケーション]** をタップします
* 既定の **[No Authentication]\(認証なし\)** のままにします
* **[OK]** をタップします。

![新しい ASP.NET Core Web アプリ](start-mvc/_static/p3.png)

---

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
