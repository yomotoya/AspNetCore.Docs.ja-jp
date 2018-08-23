---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL ルーティング |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: d9f0779d560d6ec7796a16dc2996b959dd171c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833319"
---
<a name="url-routing"></a>URL ルーティング
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、URL ルーティングをサポートするために、Wingtip Toys のサンプル アプリケーションを変更します。 ルーティングは、検索エンジンによってサポートされているより、覚えやすくわかりやすい Url を使用する web アプリケーションを使用します。 このチュートリアルでは、「メンバーシップと管理」の前のチュートリアルに基づいており、Wingtip Toys のチュートリアル シリーズのパートです。

## <a name="what-youll-learn"></a>学習内容。

- ASP.NET Web フォーム アプリケーション用のルートを登録する方法。
- Web ページにルートを追加する方法。
- ルートをサポートするために、データベースからデータを選択する方法。

## <a name="aspnet-routing-overview"></a>ASP.NET ルーティングの概要

URL ルーティングを使用すると、そのまま使用するアプリケーションを構成する物理ファイルにマップされていない Url を要求します。 要求 URL は、ユーザーが web サイトのページの検索に、ブラウザーに入力 URL だけです。 ルーティングを使用して、検索エンジン最適化 (SEO) に役立つことができ、意味的意味のあるユーザーになっている Url を定義します。

既定では、Web フォーム テンプレートが含まれます[ASP.NET Friendly Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)します。 基本的なルーティング作業の多くを使用して実装*フレンドリな Url*します。 ただし、このチュートリアルでは、カスタマイズされたルーティング機能を追加します。

URL ルーティングをカスタマイズする前に、Wingtip Toys のサンプル アプリケーションは、次の URL を使用して製品にリンクできます。

`https://localhost:44300/ProductDetails.aspx?productID=2`

URL ルーティングをカスタマイズするには、Wingtip Toys のサンプル アプリケーションに URL を読みやすいを使用して、同じ製品にリンクされます。

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>ルート

ルートは、ハンドラーにマップされている URL パターンです。 ハンドラーは、Web フォーム アプリケーションで .aspx ファイルなどの物理ファイルを指定できます。 ハンドラーは、要求を処理するクラスもできます。 ルートを定義するには、URL パターン、ハンドラー、および必要に応じて、ルートの名前を指定することで、ルート クラスのインスタンスを作成します。

アプリケーションに追加することで、ルートを追加する、`Route`オブジェクトを静的な`Routes`のプロパティ、`RouteTable`クラス。 ルートのプロパティは、`RouteCollection`アプリケーションのすべてのルートを格納するオブジェクト。

### <a name="url-patterns"></a>URL パターン

URL パターンは、リテラル値と変数のプレース ホルダー (URL パラメーターと呼ばれます) に含めることができます。 リテラルとプレース ホルダーは、スラッシュで区切られた URL のセグメントである (`/`) 文字。

Web アプリケーションへの要求が行われたときに、URL はセグメントとプレース ホルダーに解析され、変数の値が、要求ハンドラーを提供します。 このプロセスは、クエリ文字列内のデータを解析および要求ハンドラーに渡される方法に似ています。 どちらの場合も、変数の情報は、URL に含まれ、キー/値ペアの形式でハンドラーに渡されます。 クエリ文字列の場合、キーと値の両方が、URL には。 ルートの場合、キーは、URL パターンで定義されているプレース ホルダー名と値のみが url に含めています。

URL パターンは、中かっこで囲み、プレース ホルダーを定義する (`{`と`}`)。 セグメントで 1 つ以上のプレース ホルダーを定義できますが、プレース ホルダーをリテラル値で区切る必要があります。 たとえば、`{language}-{country}/{action}`は有効なルート パターンです。 ただし、`{language}{country}/{action}`リテラル値またはプレース ホルダーの間の区切り記号がないために、有効なパターンではありません。 そのため、ルーティングと、国のプレース ホルダーの値からの言語のプレース ホルダーの値を分離する場所が決定することはできません。

### <a name="mapping-and-registering-routes"></a>マッピングとルートを登録します。

Wingtip Toys のサンプル アプリケーションのページへのルートを含めることができます、前に、アプリケーションの起動時に、ルートを登録する必要があります。 ルートを登録するには、変更、`Application_Start`イベント ハンドラー。

1. **ソリューション エクスプ ローラー**、Visual Studio の検索して開く、 *Global.asax.cs*ファイル。
2. 黄色で強調表示されているコードを追加、 *Global.asax.cs*ファイルの次のようにします。   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wingtip Toys のサンプル アプリケーションの起動時、ときに呼び出す、`Application_Start`イベント ハンドラー。 このイベント ハンドラーの最後に、`RegisterCustomRoutes`メソッドが呼び出されます。 `RegisterCustomRoutes`メソッドを呼び出して各ルートを追加する、`MapPageRoute`のメソッド、`RouteCollection`オブジェクト。 ルートを定義するには、ルート URL と物理的な URL とルート名を使用します。

最初のパラメーター ("`ProductsByCategoryRoute`") は、ルートの名前です。 ルートを呼び出す必要がある場合に使用されます。 2 番目のパラメーター ("`Category/{categoryName}`") フレンドリ交換コードに基づいて動的なことができる URL を定義します。 生成されるデータに基づいたリンクを使用して、データ コントロールを作成する場合は、このルートを使用します。 ルートが表示されます。

[!code-csharp[Main](url-routing/samples/sample2.cs)]

ルートの 2 番目のパラメーターには、中かっこで指定された、動的な値が含まれています (`{ }`)。 ここで、`categoryName`は適切なルーティング パスを決定するために使用する変数です。

> [!NOTE] 
> 
> **Optional**
> 
> 移動することによって、コードを管理しやすい検索可能性があります、`RegisterCustomRoutes`メソッドを別のクラスにします。 *ロジック*フォルダーを個別に作成`RouteActions`クラス。 上移動`RegisterCustomRoutes`からメソッド、 *Global.asax.cs*ファイルを新しい`RoutesActions`クラス。 使用して、`RoleActions`クラスおよび`createAdmin`メソッドを呼び出す方法の例として、`RegisterCustomRoutes`からメソッド、 *Global.asax.cs*ファイル。


お気付きかもしれません、`RegisterRoutes`メソッドの呼び出しを使用して、`RouteConfig`の先頭にあるオブジェクト、`Application_Start`イベント ハンドラー。 この呼び出しは、既定のルーティングを実装するために行われます。 Visual Studio の Web フォーム テンプレートを使用してアプリケーションを作成したときに既定のコードとしては含まれています。

## <a name="retrieving-and-using-route-data"></a>取得して、ルート データを使用します。

前述のように、ルートを定義することができます。 追加したコード、`Application_Start`内のイベント ハンドラー、 *Global.asax.cs*ファイルが定義可能なルートを読み込みます。

### <a name="setting-routes"></a>ルートの設定

ルートでは、追加のコードを追加する必要があります。 このチュートリアルでは、取得するモデル バインドを使用します、`RouteValueDictionary`データ コントロールからのデータを使用してルートを生成するときに使用されるオブジェクト。 `RouteValueDictionary`オブジェクトは、特定の製品カテゴリに属する製品名の一覧が格納されます。 データとルートに基づいて、各製品のリンクが作成されます。

#### <a name="enable-routes-for-categories-and-products"></a>カテゴリと製品のルートを有効にします。

次に、使用するアプリケーションを更新、`ProductsByCategoryRoute`各製品カテゴリのリンクを含めることが適切なルートを決定します。 更新しても、 *ProductList.aspx*ページに各製品のルーティング リンクが含まれます。 リンクが表示されます、変更前に、と同様に、リンクは URL ルーティングを使用してようになりました。

1. **ソリューション エクスプ ローラー**、オープン、 *Site.Master*ページがまだ開いていない場合。
2. 更新プログラム、 **ListView**という名前のコントロール"`categoryList`"黄色で強調表示の変更により、そのため、マークアップように表示されます。   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. **ソリューション エクスプ ローラー**、オープン、 *ProductList.aspx*ページ。
4. 更新プログラム、`ItemTemplate`の要素、 *ProductList.aspx*ページ マークアップが次のように表示されるように、黄色で強調表示されている更新プログラムを使用します。   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. 分離コードを開く*ProductList.aspx.cs*黄色で強調表示されている次の名前空間を追加します。  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. 置換、`GetProducts`分離コードのメソッド (*ProductList.aspx.cs*) を次のコード。   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>製品の詳細のコードを追加します。

ここでは、分離コードを更新 (*ProductDetails.aspx.cs*) の*ProductDetails.aspx*ルート データを使用するページ。 注意して、新しい`GetProduct`メソッドもユーザーがブックマーク リンク、古い非対応でないルーティングの URL を使用してケースのクエリ文字列値を受け取ります。

1. 置換、`GetProduct`分離コードのメソッド (*ProductDetails.aspx.cs*) を次のコード。   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>アプリケーションの実行

更新されたルートを表示するようになりましたアプリケーションを実行することができます。

1. キーを押して**F5** Wingtip Toys のサンプル アプリケーションを実行します。  
 ブラウザーが開きを示しています、 *Default.aspx*ページ。
2. をクリックして、**製品**ページの上部にあるリンクです。  
 すべての製品が表示されます、 *ProductList.aspx*ページ。 (実際のポート番号を使用して) 次の URL がブラウザーの表示されます。  
    `https://localhost:44300/ProductList`
3. 次に、クリックして、**自動車**ページの上部にあるカテゴリのリンク。  
 Cars のみが表示されます、 *ProductList.aspx*ページ。 (実際のポート番号を使用して) 次の URL がブラウザーの表示されます。  
    `https://localhost:44300/Category/Cars`
4. 最初の自動車の名前を含むリンクは、ページに表示 をクリックします ("**変換可能な自動車**")、製品の詳細を表示します。  
 (実際のポート番号を使用して) 次の URL がブラウザーの表示されます。  
    `https://localhost:44300/Product/Convertible%20Car`
5. 次に、ブラウザーに (実際のポート番号を使用して) 次の非ルーティング URL を入力します。  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 コードでは、ユーザーがブックマーク リンクを持つケースのクエリ文字列を含む URL は認識します。

## <a name="summary"></a>まとめ

このチュートリアルでは、カテゴリおよび製品用のルートを追加しました。 モデル バインディングを使用するデータ コントロールとルートを統合する方法を学習しました。 次のチュートリアルでは、グローバル エラー処理を実装します。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET Friendly Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Azure App Service へのメンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET Web フォーム アプリをデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [前へ](membership-and-administration.md)
> [次へ](aspnet-error-handling.md)
