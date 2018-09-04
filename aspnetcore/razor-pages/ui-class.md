---
title: ASP.NET Core のクラス ライブラリの再利用可能 Razor UI
author: Rick-Anderson
description: クラス ライブラリで再利用可能 Razor UI を作成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "42909650"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>ASP.NET Core で Razor クラス ライブラリ プロジェクトを使用し、再利用可能 UI を作成します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor ビュー、ページ、コントローラー、ページ モデル、[ビュー コンポーネント](xref:mvc/views/view-components)、データ モデルは、Razor クラス ライブラリ (RCL) に組み込むことができます。 RCL はパッケージ化し、再利用できます。 アプリケーションには RCL を含めることができます。また、それに含まれるビューやページをオーバーライドできます。 Web アプリと RCL の両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。

この機能が必要です。 [!INCLUDE[](~/includes/2.1-SDK.md)]

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="create-a-class-library-containing-razor-ui"></a>Razor UI を含むクラス ライブラリを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。
* ライブラリに名前を付け ("RazorClassLib" など)、**[OK]** を選択します。 生成されたビュー ライブラリとファイル名の競合を避けるため、ライブラリ名の末尾が `.Views` ではないことを確認します。
* **ASP.NET Core 2.1** 以降が選択されていることを確認します。
* **[Razor クラス ライブラリ]**、**[OK]** の順に選択します。

Razor クラス ライブラリでは、次のプロジェクト ファイルがあります。

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

コマンドラインから `dotnet new razorclasslib` を実行します。 例えば:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

詳細については、「[dotnet new](/dotnet/core/tools/dotnet-new)」を参照してください。 生成されたビュー ライブラリとファイル名の競合を避けるため、ライブラリ名の末尾が `.Views` ではないことを確認します。

------
Razor ファイルを RCL に追加します。

ASP.NET Core テンプレートは、RCL コンテンツが前提としています。、*領域*フォルダー。 参照してください[RCL ページ レイアウト](#afs)でコンテンツを公開する RCL を作成する`~/Pages`なく`~/Areas/Pages`します。

## <a name="referencing-razor-class-library-content"></a>Razor クラス ライブラリ コンテンツを参照する

RCL は次によって参照できます。

* NuGet パッケージ。 「[Creating NuGet packages](/nuget/create-packages/creating-a-package)」 (NuGet パッケージの作成)、「[dotnet add package](/dotnet/core/tools/dotnet-add-package)」、「[Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)」 (NuGet パッケージの作成と公開) を参照してください。
* *{ProjectName}.csproj*. 「[dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)」を参照してください。

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>チュートリアル: Razor クラス ライブラリ プロジェクトを作成し、Razor ページ プロジェクトから使用する

作成しなくても、[完全なプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)をダウンロードしてテストできます。 サンプル ダウンロードには、プロジェクトのテストを簡単にする追加のコードやリンクが含まれています。 GitHub の問題に関するフィードバックは[こちら](https://github.com/aspnet/Docs/issues/6098)で扱っています。ダウンロード サンプルと段階的指示の違いについてコメントを投稿できます。

### <a name="test-the-download-app"></a>ダウンロード アプリをテストする

完全なアプリをダウンロードしておらず、チュートリアル プロジェクトを作成する場合、[次のセクション](#create-a-razor-class-library)に進んでください。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio で *.sln* ファイルを開きます。 アプリを実行します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

*cli* ディレクトリのコマンド プロンプトから、RCL と Web アプリをビルドします。

``` CLI
dotnet build
```

*WebApp1* ディレクトリに移動し、アプリを実行します。

``` CLI
dotnet run
```
------

[テスト WebApp1](#test) の指示に従ってください。

## <a name="create-a-razor-class-library"></a>Razor クラス ライブラリの作成

このセクションでは、Razor クラス ライブラリ (RCL) が作成されます。 Razor ファイルが RCL に追加されます。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

RCL プロジェクトの作成:

* Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。
* アプリの名前を付けます**RazorUIClassLib** > **OK**します。
* **ASP.NET Core 2.1** 以降が選択されていることを確認します。
* **[Razor クラス ライブラリ]**、**[OK]** の順に選択します。
* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* という名前が付いた Razor 部分ビュー ファイルを追加します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

コマンド ラインから次を実行します。

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

上のコマンドでは以下の操作が行われます。

* `RazorUIClassLib` Razor クラス ライブラリ (RCL) が作成されます。
* Razor _Message ページが作成され、RCL に追加されます。 `-np` パラメーターによって、`PageModel` なしでページが作成されます。
* [viewstart](xref:mvc/views/layout#running-code-before-each-view) ファイルが作成され、RCL に追加されます。

(次のセクションで追加される) Razor ページ プロジェクトのレイアウトを使用するには、viewstart ファイルが必要です。

------

### <a name="add-razor-files-and-folders-to-the-project"></a>Razor のファイルとフォルダーをプロジェクトに追加します。

* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* のマークアップを次のコードに変更します。

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* のマークアップを次のコードに変更します。

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` は部分ビュー (`<partial name="_Message" />`) を使用するために必要です。 `@addTagHelper` ディレクティブを含める代わりに、*_ViewImports.cshtml* ファイルを追加できます。 例えば:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

ビューのインポートについては、「[共有ディレクティブのインポート](xref:mvc/views/layout#importing-shared-directives)」を参照してください。

* クラス ライブラリをビルドし、コンパイラ エラーがないことを確認します。

``` CLI
dotnet build RazorUIClassLib
```

ビルド出力には、*RazorUIClassLib.dll* と *RazorUIClassLib.Views.dll* が含まれています。 *RazorUIClassLib.Views.dll* には、コンパイル済みの Razor コンテンツが含まれています。

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Razor ページ プロジェクトから Razor UI ライブラリを使用します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Razor ページ Web アプリの作成:

* **ソリューション エクスプローラー**で、ソリューションを右クリックし、**[追加]**、**[新しいプロジェクト]** の順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。
* アプリに **WebApp1** という名前を付けます。
* **ASP.NET Core 2.1** 以降が選択されていることを確認します。
* **[Web アプリケーション]**、**[OK]** の順に選択します。

* **ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。
* **ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[ビルド依存関係]**、**[プロジェクトの依存関係]** の順に選択します。
* **WebApp1** の依存関係として **RazorUIClassLib** を選択します。
* **ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[追加]**、**[参照]** の順に選択します。
* **[参照マネージャー]** ダイアログで、**[RazorUIClassLib]**、**[OK]** の順に選択します。

アプリを実行します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Razor ページ Web アプリと、Razor ページ アプリと Razor クラス ライブラリを含むソリューション ファイルを作成します。

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

Web アプリをビルドし、実行します。

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>テスト WebApp1

Razor UI クラス ライブラリが使用されていることを確認します。

* `/MyFeature/Page1` を参照します。

## <a name="override-views-partial-views-and-pages"></a>ビュー、部分ビュー、ページのオーバーライド

Web アプリと Razor クラス ライブラリの両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。 たとえば、*WebApp1/Areas/MyFeature/Pages/Page1.cshtml* を WebApp1 に追加すると、WebApp1 の Page1 は、Razor クラス ライブラリの Page1 に優先します。

サンプル ダウンロードの *WebApp1/Areas/MyFeature2* の名前を *WebApp1/Areas/MyFeature* に変更し、優先設定をテストします。

*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分ビューを *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml* ビューにコピーします。 新しい場所を示すようにマークアップを更新します。 アプリをビルドして実行し、アプリの部分ビューが使用されていることを確認します。

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>RCL ページ レイアウト

RCL コンテンツは、web アプリのページのフォルダーの一部である場合と同様に参照するには、次のファイル構造で RCL プロジェクトを作成します。

* *RazorUIClassLib/ページ*
* *RazorUIClassLib/ページ/共有*

たとえば、 *RazorUIClassLib/ページ/共有*2 つの部分的なファイルが含まれています *_Header.cshtml*と *_Footer.cshtml*。 <partial>タグに追加できる *_Layout.cshtml*ファイル。 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```
