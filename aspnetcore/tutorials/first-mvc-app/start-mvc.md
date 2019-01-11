---
title: ASP.NET Core MVC の概要
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: ASP.NET Core MVC の概要について説明します。
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: cfce3b5792a5d0673bae5ddbba9e2d4d515a6279
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381804"
---
# <a name="get-started-with-aspnet-core-mvc"></a>ASP.NET Core MVC の概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

このチュートリアルでは、ASP.NET Core の MVC Web アプリの構築の基礎について説明します。

このアプリでは、映画タイトルのデータベースを管理します。 以下の方法について説明します。

> [!div class="checklist"]
> * Web アプリを作成する。
> * モデルを追加してスキャフォールディングする。
> * データベースを使用する。
> * 検索と検証を追加する。

最後には、映画データを管理および表示できるアプリが完成します。

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。  現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>Web アプリの作成

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

**[新しいプロジェクト]** ダイアログで次のように設定します。

* 左側のウィンドウで、**[.NET Core]** を選択します。
* 中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** を選択します。
* プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。
* **[OK]** を選択します。

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project2-21.png)

**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。

* バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.2]** を選択します
* **[Web アプリケーション (モデル ビュー コントローラー)]** を選択します
* **[OK]** を選択します。

![[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。 プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。 これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。

**Ctrl + F5** キーを押して、デバッグ以外のモードでアプリを実行します。

* Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図で、ポート番号は 5000 です。 ブラウザーの URL は `localhost:5000` を示します。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。
* **[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* **[IIS Express]** ボタンを選択することで、アプリをデバッグできます。

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

このチュートリアルは VS Code の知識があることを前提としています。 詳細については、[VS Code の概要](https://code.visualstudio.com/docs)、および [Visual Studio Code のヘルプ](#visual-studio-code-help)に関するページを参照してください。

* [統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。
* ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。
* 次のコマンドを実行します。

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * "**ビルドとデバッグに必要な資産が 'MvcMovie' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、  **[はい]** を選択します

  * `dotnet new mvc -o MvcMovie`: *MvcMovie* フォルダー内に新しい ASP.NET Core MVC プロジェクトを作成します。
  * `code -r MvcMovie`:Visual Studio Code で *MvcMovie.csproj* プロジェクト ファイルを読み込みます。

### <a name="launch-the-app"></a>アプリの起動

* **Ctrl + F5** キーを押して、デバッガーなしで実行します。

  Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動され、`http://localhost:5001` に移動されます。 アドレス バーには、`example.com` などではなく、`localhost:port:5001` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。

  **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[ファイル]**、**[新しいソリューション]** の順に選択します。

  ![macOS の新しいソリューション](~/tutorials/first-web-api-mac/_static/sln.png)

* **[.NET Core アプリ]** > **[ASP.NET Core]** > **[ASP.NET Core Web アプリ (MVC)]** > **[次へ]** の順に選択します。

  ![macOS の [新しいプロジェクト] ダイアログ](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* **[Configure your new ASP.NET Core Web API]\(新しい ASP.NET Core Web API を構成する\)** ダイアログ ボックスで、既定の**ターゲット フレームワーク** **.NET Core 2.2* を受け入れます。

* プロジェクトに **MvcMovie** という名前を付けて、**[作成]** を選択します。

### <a name="launch-the-app"></a>アプリの起動

**[実行]** > **[デバッグなしで開始]** の順に選択してアプリを起動します。 Visual Studio for Mac によって [Kestrel](xref:fundamentals/servers/index#kestrel) サーバーが開始され、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。

* アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 アプリを実行する際には、別のポート番号が表示されます。
* **[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。

---  
<!-- End of VS tabs -->

* **[同意する]** を選択し、追跡に同意します。 このアプリでは個人情報は追跡されません。 テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。

  ![ホームまたはインデックス ページ](start-mvc/_static/privacy.png)

  次の図は、追跡に同意した後のアプリを示しています。

  ![ホームまたはインデックス ページ](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。

> [!div class="step-by-step"]
> [次へ](adding-controller.md)  
