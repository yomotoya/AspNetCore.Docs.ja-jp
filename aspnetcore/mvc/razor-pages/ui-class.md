---
title: ASP.NET Core のクラス ライブラリの再利用可能 Razor UI
author: Rick-Anderson
description: クラス ライブラリで再利用可能 Razor UI を作成する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>ASP.NET Core で Razor クラス ライブラリ プロジェクトを使用し、再利用可能 UI を作成します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor ビュー、ページ、コントローラー、ページ モデル、データ モデルは、Razor クラス Library (RCL) に組み込むことができます。 RCL はパッケージ化し、再利用できます。 アプリケーションには RCL を含めることができます。また、それに含まれるビューやページをオーバーライドできます。 Web アプリと RCL の両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。

この機能には [!INCLUDE[](~/includes/2.1-SDK.md)] が必要です。

[!INCLUDE[](~/includes/2.1.md)]

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="create-a-class-library-containing-razor-ui"></a>Razor UI を含むクラス ライブラリを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。
* **ASP.NET Core 2.1** 以降が選択されていることを確認します。
* **[Razor クラス ライブラリ]**、**[OK]** の順に選択します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

コマンドラインから `dotnet new razorclasslib` を実行します。 例:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

詳細については、「[dotnet new](/dotnet/core/tools/dotnet-new)」を参照してください。

------
Razor ファイルを RCL に追加します。

RCL コンテンツは *[区分]* フォルダーに入れることをお勧めします。 


## <a name="referencing-razor-class-library-content"></a>Razor クラス ライブラリ コンテンツを参照する

RCL は次によって参照できます。

* NuGet パッケージ。 「[Creating NuGet packages](/nuget/create-packages/creating-a-package)」 (NuGet パッケージの作成)、「[dotnet add package](/dotnet/core/tools/dotnet-add-package)」、「[Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)」 (NuGet パッケージの作成と公開) を参照してください。
* *{ProjectName}.csproj*. 「[dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)」を参照してください。

### <a name="partial-files-access-in-the-rcl"></a>RCL の部分ファイル アクセス

RCL の外のコンテンツについては、ASP.NET Core ランタイムは RCL の部分ファイルを検索しません。

たとえば、サンプル ダウンロードでは、*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分ビューは *WebApp1\Pages\About.cshtml* で参照**できません**。 ただし、RCL *RazorUIClassLib/* のページは *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* にアクセス**できます**。

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>チュートリアル: Razor クラス ライブラリ プロジェクトを作成し、Razor ページ プロジェクトから使用する

作成しなくても、[完全なプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples)をダウンロードしてテストできます。 サンプル ダウンロードには、プロジェクトのテストを簡単にする追加のコードやリンクが含まれています。 GitHub の問題に関するフィードバックは[こちら](https://github.com/aspnet/Docs/issues/6098)で扱っています。ダウンロード サンプルと段階的指示の違いについてコメントを投稿できます。

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

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。
* アプリに **RazorUIClassLib** という名前を付けます。
* **ASP.NET Core 2.1** 以降が選択されていることを確認します。
* **[Razor クラス ライブラリ]**、**[OK]** の順に選択します。

Razor ページ Web アプリの作成:

* **ソリューション エクスプローラー**で、ソリューションを右クリックし、**[追加]**、**[新しいプロジェクト]** の順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。
* アプリに **WebApp1** という名前を付けます。
* **ASP.NET Core 2.1** 以降が選択されていることを確認します。
* **[Web アプリケーション]**、**[OK]** の順に選択します。

### <a name="add-razor-files-and-folders-to-the-project"></a>Razor のファイルとフォルダーをプロジェクトに追加します。

* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* という名前が付いた Razor 部分ビュー ファイルを追加します。
* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* のマークアップを次のコードに変更します。

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* WebApp1 プロジェクトから *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml* に *_ViewStart.cshtml* ファイルをコピーします。

  Razor ページ プロジェクトのレイアウトを使用するには、[viewstart](xref:mvc/views/layout#running-code-before-each-view) ファイルが必要です。

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

Razor ページを更新します。

* *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* のマークアップを次のコードに変更します。

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* のマークアップを次のコードに変更します。

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` は部分ビュー (`<partial name="_Message" />`) を使用するために必要です。 `@addTagHelper` ディレクティブを含める代わりに、*_ViewImports.cshtml* ファイルを追加できます。 例:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

ビューのインポートについては、「[共有ディレクティブのインポート](xref:mvc/views/layout#importing-shared-directives)」を参照してください。

* クラス ライブラリをビルドし、コンパイラ エラーがないことを確認します。

``` CLI
dotnet build RazorUIClassLib
```

ビルド出力には、*RazorUIClassLib.dll* と *RazorUIClassLib.Views.dll* が含まれています。 *RazorUIClassLib.Views.dll* には、コンパイル済みの Razor コンテンツが含まれています。

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Razor ページ プロジェクトから Razor UI ライブラリを使用します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。
* **ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[ビルド依存関係]**、**[プロジェクトの依存関係]** の順に選択します。
* **WebApp1** の依存関係として **RazorUIClassLib** を選択します。
* **ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[追加]**、**[参照]** の順に選択します。
* **[参照マネージャー]** ダイアログで、**[RazorUIClassLib]**、**[OK]** の順に選択します。

アプリを実行します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Razor ページ Web アプリと、Razor ページ アプリと Razor クラス ライブラリを含むソリューション ファイルを作成します。

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Web アプリをビルドし、実行します。

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a>テスト WebApp1

Razor UI クラス ライブラリが使用されていることを確認します。

* `/MyFeature/Page1` を参照します。

## <a name="override-views-partial-views-and-pages"></a>ビュー、部分ビュー、ページのオーバーライド

Web アプリと Razor クラス ライブラリの両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。 たとえば、*WebApp1/Areas/MyFeature/Pages/Page1.cshtml* を WebApp1 に追加すると、WebApp1 の Page1 は、Razor クラス ライブラリの Page1 に優先します。

サンプル ダウンロードの *WebApp1/Areas/MyFeature2* の名前を *WebApp1/Areas/MyFeature* に変更し、優先設定をテストします。

*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分ビューを *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml* ビューにコピーします。 新しい場所を示すようにマークアップを更新します。 アプリをビルドして実行し、アプリの部分ビューが使用されていることを確認します。
