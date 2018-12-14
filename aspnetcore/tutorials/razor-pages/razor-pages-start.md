---
title: ASP.NET Core の Razor ページの概要
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: このチュートリアル シリーズでは、ASP.NET Core で Razor ページを使用する方法を示します。 モデルの作成、Razor ページのコードの生成、Entity Framework Core と SQL Server を使用したデータ アクセス、検索機能の追加、入力検証の追加、および移行を使用したモデルの更新の方法について説明します。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861629"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>チュートリアル: ASP.NET Core の Razor ページの概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。

このアプリでは、映画タイトルのデータベースを管理します。 以下の方法について説明します。

> [!div class="checklist"]
> * Razor Pages Web アプリを作成する。
> * モデルを追加してスキャフォールディングする。
> * データベースを使用する。
> * 検索と検証を追加する。

最後には、映画タイトル項目を管理および表示できるアプリが完成します。

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに **RazorPagesMovie** という名前を付けます。 コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。
 ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)

* ドロップダウン リストで **[ASP.NET Core 2.2]** を選択してから、**[Web アプリケーション]** を選択します。

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.2.png)

  次のスターター プロジェクトが作成されます。

  ![ソリューション エクスプローラー](razor-pages-start/_static/se2.2.png)

* **Ctrl + F5** キーを押して、デバッガーなしで実行します。

  Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図では、ポート番号は 5001 です。 アプリを実行する際には、別のポート番号が表示されます。

  **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。
* ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。
* 次のコマンドを実行します。

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * "**ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、  **[はい]** を選択します

  * `dotnet new webapp -o RazorPagesMovie`: *RazorPagesMovie*  フォルダーに新しい Razor Pages プロジェクトを作成します。
  * `code -r RazorPagesMovie`: Visual Studio Code で *RazorPagesMovie.csproj* プロジェクト ファイルを読み込みます。

### <a name="launch-the-app"></a>アプリの起動

* **Ctrl + F5** キーを押して、デバッガーなしで実行します。

  Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動され、`http://localhost:5001` に移動されます。 アドレス バーには、`example.com` などではなく、`localhost:port:5001` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。

  **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

端末から、次のコマンドを実行します。

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

上のコマンドでは [.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。 ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。

## <a name="open-the-project"></a>プロジェクトを開く

Ctrl + C キーを押して、アプリケーションをシャットダウンします。

Visual Studio から、**[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。

### <a name="launch-the-app"></a>アプリの起動

**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。 Visual Studio は [Kestrel](xref:fundamentals/servers/kestrel) を開始し、ブラウザーを起動して、`http://localhost:5001` に移動します。

<!-- End of VS tabs -->

---

* **[同意する]** を選択し、追跡に同意します。 このアプリでは個人情報は追跡されません。 テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  次の図は、追跡に同意した後のアプリを示しています。

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>プロジェクトのファイルとフォルダー

次の表に、プロジェクトのファイルとフォルダーをリストします。 チュートリアルのこの段階では、*Startup.cs* ファイルを理解することが最も重要です。 以下に示す各リンクをレビューする必要はありません。 リンクは、プロジェクトのファイルまたはフォルダーについて詳しい情報が必要な場合の参照として提供されているものです。

| ファイルまたはフォルダー              | 目的 |
| ----------------- | ------------ |
| *wwwroot* | 静的ファイルが含まれています。 [静的ファイル](xref:fundamentals/static-files)に関するページを参照してください。 |
| *ページ* | [Razor ページ](xref:razor-pages/index)のフォルダー。 |
| *appsettings.json* | [構成](xref:fundamentals/configuration/index) |
| *Program.cs* | ASP.NET Core アプリを[ホスト](xref:fundamentals/host/index)します。|
| *Startup.cs* | サービスと要求パイプラインを構成します。 [スタートアップ](xref:fundamentals/startup)に関するページを参照してください。|

### <a name="the-pages-folder"></a>Pages フォルダー

*_Layout.cshtml* ファイルは共通の HTML 要素 (スクリプトとスタイルシート) を含み、アプリケーションのレイアウトを設定します。 たとえば、**[RazorPagesMovie]**、**[ホーム]**、または **[プライバシー]** をクリックすると、同じ要素が表示されます。 共通の要素には、ウィンドウの下部のヘッダーと上部のナビゲーション メニューが含まれます。 詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。

*_ViewImports.cshtml* ファイルには、各 Razor ページにインポートされた Razor ディレクティブが含まれています。 詳細については、「[Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)」 (共有ディレクティブのインポート) を参照してください。

*_ViewStart.cshtml* では、*_Layout.cshtml* ファイルを使用するように Razor ページの `Layout` プロパティを設定します。 詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。

*_ValidationScriptsPartial.cshtml* ファイルでは、[jQuery](https://jquery.com/) 検証スクリプトへの参照が提供されます。 チュートリアルの後半で `Create` および `Edit` ページを追加するときは、*_ValidationScriptsPartial.cshtml* ファイルが使用されます。

次の目的で `Index`、`Error`、`Privacy` ページが用意されています。

* `Index`: アプリを起動します。
* `Error`: エラー情報を表示します。
* `Privacy`: サイトのプライバシー ポリシーに関する詳細情報を指定します。

このチュートリアルでは、前のページは使用されません。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>F7 キーを使用して Razor ページと PageModel を切り替える

F7 キーを押すと、Razor ページ (*\*.cshtml* ファイル) と C# ファイル (*\*.cshtml.cs*) が切り替えられます。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

規約により、Razor Page (*\*.cshtml* ファイル) と関連する `PageModel` のルート ファイル名は同じです。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

規約により、Razor Page (*\*.cshtml* ファイル) と関連する `PageModel` のルート ファイル名は同じです。

---

> [!div class="step-by-step"]
> [次: モデルの追加](xref:tutorials/razor-pages/model)