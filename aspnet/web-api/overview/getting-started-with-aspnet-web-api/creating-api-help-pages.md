---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: ASP.NET Web API のヘルプ ページの作成 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28037904"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>ASP.NET Web API のヘルプ ページの作成
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

Web API を作成するときに便利ですヘルプ ページを作成する他の開発者が API を呼び出す方法を知ること。 すべてのドキュメントを手動で作成する可能性がありますが、できるだけ多くの自動生成することをお勧めします。

この作業を容易にするには、ASP.NET Web API は、実行時のヘルプ ページの自動生成するライブラリを提供します。

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>API のヘルプ ページを作成します。

インストール[ASP.NET および Web ツール 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)です。 この更新プログラムは、Web API プロジェクト テンプレートに、ヘルプ ページを統合します。

次に、新しい ASP.NET MVC 4 プロジェクトを作成し、Web API プロジェクト テンプレートを選択します。 プロジェクト テンプレートの作成という例 API コント ローラー`ValuesController`です。 テンプレートには、API のヘルプ ページも作成されます。 すべてのヘルプ ページのコード ファイルは、プロジェクトの [区分] フォルダーに配置されます。

![](creating-api-help-pages/_static/image2.png)

アプリケーションを実行するときに、ホーム ページには、API のヘルプ ページへのリンクが含まれています。 ホーム ページからは、相対パスは、/Help がします。

![](creating-api-help-pages/_static/image3.png)

このリンクでは、API の概要ページに表示されます。

![](creating-api-help-pages/_static/image4.png)

このページの MVC ビューは、Areas/HelpPage/Views/Help/Index.cshtml で定義されます。 レイアウト、概要、タイトル、スタイル、およびなどを変更するには、このページを編集することができます。

ページのメイン部分は、コント ローラーでグループ化された Api のテーブルです。 テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイスです。 (詳しく説明このインターフェイスは後でします。)新しい API コント ローラーを追加すると、実行時に、テーブルが自動的に更新します。

"API"列には、HTTP メソッドと相対 URI が一覧表示します。 「説明」列には、各 API のドキュメントが含まれています。 最初に、ドキュメントは、プレース ホルダー テキストだけです。 次のセクションで説明する XML のコメントからドキュメントを追加する方法です。

各 API では、例の要求および応答の本文を含む、詳細な情報をページにリンクがあります。

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>既存のプロジェクトへのヘルプ ページの追加

既存の Web API プロジェクトにヘルプ ページを追加するには、NuGet パッケージ マネージャーを使用します。 このオプションは、"Web API"テンプレートよりも別のプロジェクト テンプレートから開始する便利です。

**ツール**メニューの **ライブラリ パッケージ マネージャー**、し、 **Package Manager Console**です。 [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)ウィンドウで、次のコマンドのいずれかを入力します。

**C#** アプリケーション。 `Install-Package Microsoft.AspNet.WebApi.HelpPage`

**Visual Basic**アプリケーション。 `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

C# と Visual Basic 用の 2 つのパッケージがあります。 プロジェクトに一致するものを使用することを確認してください。

このコマンドは、必要なアセンブリをインストールし、(領域/HelpPage フォルダーにあります) のヘルプ ページの MVC ビューを追加します。 ヘルプ ページへのリンクを手動で追加する必要があります。 URI は、/Help です。 Razor ビュー内のリンクを作成するには次の手順を追加します。

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

また、領域を登録することを確認してください。 Global.asax ファイル内に次のコードを追加、**アプリケーション\_開始**メソッドが存在しない場合。

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>API のドキュメントを追加します。

既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。 使用することができます[XML ドキュメント コメント](https://msdn.microsoft.com/library/b2s063f7.aspx)ドキュメントを作成します。 この機能を有効にするには、ファイル領域/HelpPage/アプリを開いて\_Start/HelpPageConfig.cs し、次の行のコメントを解除します。

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

XML ドキュメントを有効にするようになりました。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。 選択、**ビルド**ページ。

![](creating-api-help-pages/_static/image6.png)

**出力**、確認**XML ドキュメント ファイル**です。 編集ボックスで、次のように入力します。"アプリ\_Data/XmlDocument.xml"です。

![](creating-api-help-pages/_static/image7.png)

次に、コードを開き、 `ValuesController` /Controllers/ValuesControler.cs で定義されている API コント ローラー。 コント ローラーのメソッドをいくつかのドキュメントのコメントを追加します。 例えば:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> ヒント: メソッド上の行にキャレットを配置し、3 つのスラッシュを入力すると、Visual Studio が自動的に挿入 XML 要素です。 空欄に入力することができます。


ここでビルドおよびアプリケーションをもう一度を実行し、ヘルプ ページに移動します。 API の表に、ドキュメント文字列が表示されます。

![](creating-api-help-pages/_static/image8.png)

ヘルプ ページは、実行時に、XML ファイルから文字列を読み取ります。 (アプリケーションを展開するときに必ず、XML ファイルを展開します。)

## <a name="under-the-hood"></a>実際には

ヘルプ ページがの上に構築された、 **ApiExplorer**クラスは、Web API フレームワークの一部です。 **ApiExplorer**クラスは、ヘルプ ページを作成するため、原材料を提供します。 各 api **ApiExplorer**が含まれています、 **ApiDescription**を API について説明します。 このため、"API"は、HTTP メソッドと相対 URI の組み合わせとして定義されます。 たとえば、一部の個別の Api を次に示します。

- /Api/Products を取得します。
- GET /api/Products/{id}
- /Api 製品を投稿します。

コント ローラーのアクションは、複数の HTTP メソッドをサポートしている場合、 **ApiExplorer**各メソッド distinct API として扱われます。

API を非表示にする、 **ApiExplorer**、追加、 **ApiExplorerSettings**属性をアクションとセットに*IgnoreApi* true に設定します。

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

この属性は、全体のコント ローラーを除外する、コント ローラーに追加することもできます。

ApiExplorer クラスからのドキュメントの文字列を取得する、 **IDocumentationProvider**インターフェイスです。 先ほど見たようヘルプ ページ ライブラリにより、 **IDocumentationProvider** XML ドキュメントの文字列からのドキュメントを取得します。 コードは/Areas/HelpPage/XmlDocumentationProvider.cs にあります。 取得できますドキュメントの別のソースを記述して、独自**IDocumentationProvider**です。 関連付けることを呼び出し、 **SetDocumentationProvider**で定義された拡張メソッド**HelpPageConfigurationExtensions**

**ApiExplorer**に自動的に呼び出して、 **IDocumentationProvider**各 API のドキュメントの文字列を取得するインターフェイスです。 保存、**ドキュメント**のプロパティ、 **ApiDescription**と**ApiParameterDescription**オブジェクト。

## <a name="next-steps"></a>次の手順

ここに表示されるヘルプ ページに制限はないです。 実際には、 **ApiExplorer**ヘルプ ページの作成に制限はありません。 Yao Huang Lin はすぐを検討するためにいくつかの優れたブログをポストを書き込まれます。

- [ASP.NET Web API のヘルプ ページへの単純なテスト クライアントの追加](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [ASP.NET Web API のヘルプ ページを自己ホスト型サービスで作業を行う](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [ヘルプ ページ (またはクライアント) の ASP.NET Web API のデザイン時の生成](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [高度なヘルプ ページのカスタマイズ](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
