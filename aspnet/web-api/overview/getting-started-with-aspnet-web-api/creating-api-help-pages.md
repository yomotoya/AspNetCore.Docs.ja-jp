---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: ASP.NET Web API のヘルプ ページの作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 3e8a0e6c4555e2b8d880118ca499b801ac8be63e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811945"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>ASP.NET Web API のヘルプ ページの作成
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

Web API を作成するときに多くの場合のヘルプ ページを作成すると便利他の開発者は、API を呼び出す方法を認識できるようにします。 すべてのドキュメントを手動で作成する可能性がありますが、できるだけ多くの自動生成することをお勧めします。

この作業を容易にするには、ASP.NET Web API は、実行時のヘルプ ページの自動生成するライブラリを提供します。

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>API ヘルプ ページを作成します。

インストール[ASP.NET および Web Tools 2012.2 の Update](https://go.microsoft.com/fwlink/?LinkId=282650)します。 この更新プログラムは、Web API プロジェクト テンプレートに、ヘルプ ページを統合します。

次に、新しい ASP.NET MVC 4 プロジェクトを作成し、Web API プロジェクト テンプレートを選択します。 プロジェクト テンプレートの作成という名前の API の例のコント ローラーを`ValuesController`します。 テンプレートには、API のヘルプ ページも作成します。 すべてのヘルプ ページのコード ファイルは、プロジェクトの [区分] フォルダーに配置されます。

![](creating-api-help-pages/_static/image2.png)

アプリケーションを実行すると、ホーム ページには、API ヘルプ ページへのリンクが含まれています。 ホーム ページからは、相対パスは、/Help が。

![](creating-api-help-pages/_static/image3.png)

このリンクでは、API の概要ページに表示されます。

![](creating-api-help-pages/_static/image4.png)

このページの MVC ビューは、Areas/HelpPage/Views/Help/Index.cshtml で定義されます。 レイアウト、概要、タイトル、スタイル、およびなどを変更するには、このページを編集することができます。

ページの主要部分では、コント ローラーでグループ化された Api のテーブルです。 テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイス。 (説明しますこのインターフェイスについては後で。)新しい API コント ローラーを追加すると、実行時に、テーブルが自動的に更新します。

"API"の列には、HTTP メソッドと相対 URI が表示されます。 「説明」列には、各 API のドキュメントが含まれています。 最初に、ドキュメントは、プレース ホルダー テキストだけです。 次のセクションでは、XML のコメントからドキュメントを追加する方法を紹介します。

各 API では、例の要求および応答の本文を含む、詳細な情報をページにリンクがあります。

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>既存のプロジェクトに追加のヘルプ ページ

ヘルプ ページは、NuGet パッケージ マネージャーを使用して既存の Web API プロジェクトに追加できます。 このオプションは、"Web API"テンプレートよりも、別のプロジェクト テンプレートから開始するは便利です。

**ツール**メニューの **ライブラリ パッケージ マネージャー**、し、**パッケージ マネージャー コンソール**します。 [パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)ウィンドウで、次のコマンドのいずれかを入力します。

**C#** アプリケーション。 `Install-Package Microsoft.AspNet.WebApi.HelpPage`

**Visual Basic**アプリケーション。 `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

C# と Visual basic の 2 つのパッケージがあります。 プロジェクトに一致する 1 つを使用してください。

このコマンドは、必要なアセンブリをインストールし、(領域/HelpPage フォルダーにあります) のヘルプ ページの MVC ビューを追加します。 ヘルプ ページへのリンクを手動で追加する必要があります。 URI は、/Help です。 Razor ビューで、リンクを作成するには、次のように追加します。

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

また、領域を登録することを確認してください。 Global.asax ファイルで次のコードを追加、**アプリケーション\_開始**メソッドが存在しない場合。

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>API のドキュメントを追加します。

既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。 使用することができます[XML ドキュメント コメント](https://msdn.microsoft.com/library/b2s063f7.aspx)ドキュメントを作成します。 この機能を有効にするには、ファイル領域/HelpPage/アプリを開く\_Start/HelpPageConfig.cs、次の行をコメント解除します。

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

XML ドキュメントを有効にするようになりました。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**プロパティ**します。 選択、**ビルド**ページ。

![](creating-api-help-pages/_static/image6.png)

**出力**、確認**XML ドキュメント ファイル**します。 編集ボックスで、次のように入力します。"アプリ\_Data/XmlDocument.xml"。

![](creating-api-help-pages/_static/image7.png)

コードを次に、開く、 `ValuesController` /Controllers/ValuesControler.cs で定義されている API コント ローラー。 コント ローラーのメソッドには、いくつかのドキュメントのコメントを追加します。 例えば:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> ヒント: メソッド上の行にキャレットを配置する 3 つのスラッシュを入力すると、Visual Studio が自動的に挿入する XML 要素。 空白を入力します。


今すぐビルドし、もう一度アプリケーションを実行し、ヘルプ ページに移動します。 API テーブル内のドキュメント文字列が表示されます。

![](creating-api-help-pages/_static/image8.png)

ヘルプ ページは、実行時に、XML ファイルから文字列を読み取ります。 (アプリケーションを展開するときに必ず XML ファイルをデプロイする。)

## <a name="under-the-hood"></a>内部的には

ヘルプ ページがの上に構築された、**ための ApiExplorer**クラスは、Web API フレームワークの一部です。 **ための ApiExplorer**クラスは、ヘルプ ページを作成するため、原材料を提供します。 各 api では、**ための ApiExplorer**が含まれています、 **ApiDescription** API を記述します。 このため、"API"は、HTTP メソッドと相対 URI の組み合わせとして定義されます。 たとえば、異なる Api がいくつか次に示します。

- /Api/Products を取得します。
- GET /api/Products/{id}
- Api/製品を投稿します。

コント ローラーのアクションは、複数の HTTP メソッドをサポートしている場合、**ための ApiExplorer**各メソッドを個別の API として扱われます。

API を非表示にする、**ための ApiExplorer**、追加、 **ApiExplorerSettings**属性を設定しアクション*IgnoreApi*を true にします。

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

コント ローラー全体を除外する、コント ローラーにこの属性を追加することもできます。

ドキュメント文字列を取得するための ApiExplorer クラス、 **IDocumentationProvider**インターフェイス。 ヘルプ ページ ライブラリは、既に説明したように**IDocumentationProvider** XML ドキュメントの文字列からドキュメントを取得します。 コードは/Areas/HelpPage/XmlDocumentationProvider.cs にあります。 取得できますドキュメントの別のソースを記述して、独自**IDocumentationProvider**します。 結び付けるを呼び出し、 **SetDocumentationProvider**で定義された拡張機能メソッド**HelpPageConfigurationExtensions**

**ための ApiExplorer**を自動的に呼び出す、 **IDocumentationProvider**各 api のドキュメント文字列を取得するインターフェイス。 それらを保存、**ドキュメント**のプロパティ、 **ApiDescription**と**ApiParameterDescription**オブジェクト。

## <a name="next-steps"></a>次の手順

方法は、次のとおりのヘルプ ページに限定されません。 実際、**ための ApiExplorer**ヘルプ ページを作成するのには限定されません。 Yao Huang Lin はすぐ概説するいくつかの優れたブログの投稿を書き込まれます。

- [ASP.NET Web API ヘルプ ページへの単純なテスト クライアントの追加](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [ASP.NET Web API ヘルプ ページ自己ホスト型サービスで作業を行う](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [ヘルプ ページ (またはクライアント) の ASP.NET Web API のデザイン時の生成](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [高度なヘルプ ページのカスタマイズ](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
