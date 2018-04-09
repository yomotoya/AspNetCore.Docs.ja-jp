---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL ルーティング |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: a195b36517bcae4bbeaf43fe7386e7787fd00212
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="url-routing"></a>URL ルーティング
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、URL ルーティングをサポートするために、Wingtip Toys サンプル アプリケーションを変更します。 ルーティングは、わかりやすい、覚えやすく検索エンジンによってより適切にサポートされている Url を使用して、web アプリケーションを使用します。 このチュートリアルでは、前のチュートリアルでは、「メンバーシップと管理」を上に構築され、Wingtip Toys チュートリアル シリーズの一部です。

## <a name="what-youll-learn"></a>学習する内容。

- ASP.NET Web フォーム アプリケーションのルートを登録する方法。
- Web ページへのルートを追加する方法。
- ルートをサポートするためにデータベースからデータを選択する方法です。

## <a name="aspnet-routing-overview"></a>ASP.NET ルーティングの概要

URL ルーティングを使用すると、そのまま使用するアプリケーションを構成する物理ファイルにマップされていない Url を要求します。 要求 URL は、ユーザーが、web サイトでページを検索する際にブラウザーに入力 URL だけです。 ルーティング定義に使用する Url をユーザーに意味のある意味であるし、検索エンジン最適化 (SEO) に役立つことができます。

既定では、Web フォーム テンプレートに含まれる[ASP.NET Friendly Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)です。 使用して基本的なルーティング作業の多くを実装する*フレンドリな Url*です。 ただし、このチュートリアルでは、カスタマイズされたルーティング機能を追加します。

URL ルーティングをカスタマイズする前に、Wingtip Toys のサンプル アプリケーションは、次の URL を使用して、製品にリンクできます。

`https://localhost:44300/ProductDetails.aspx?productID=2`

URL ルーティングをカスタマイズすると、Wingtip Toys のサンプル アプリケーションは、読みやすい URL を使用して、同じ製品にリンクします。

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>ルート

ルートは、ハンドラーにマップされている URL パターンです。 ハンドラーは、Web フォーム アプリケーションで .aspx ファイルなどの物理ファイルを指定できます。 ハンドラーは、要求を処理するクラスもできます。 ルートを定義するのには、URL パターン、ハンドラー、および必要に応じて、ルートの名前を指定して、ルート クラスのインスタンスを作成します。

アプリケーションに追加することによって、ルートを追加する、 `Route` 、静的なオブジェクト`Routes`のプロパティ、`RouteTable`クラスです。 ルートのプロパティは、`RouteCollection`アプリケーションのすべてのルートを格納するオブジェクト。

### <a name="url-patterns"></a>URL パターン

URL パターンには、リテラル値と変数のプレース ホルダー (URL パラメーターと呼ばれる) を含めることができます。 リテラルとプレース ホルダーがスラッシュで区切られますが、URL のセグメントにあります (`/`) 文字です。

Web アプリケーションに要求が行われたときに、URL はセグメントとプレース ホルダーに解析され、変数の値が要求ハンドラーを提供します。 このプロセスは、クエリ文字列内のデータを解析および要求ハンドラーに渡される方法に似ています。 どちらの場合、変数の情報が URL に含まれ、キー/値ペアの形式でハンドラーに渡されます。 クエリ文字列の場合、キーと値の両方が、URL には。 ルートの場合、キーは、URL パターンで定義されているプレース ホルダー名と値のみが URL には。

URL パターンは、中かっこで囲み、プレース ホルダーを定義する (`{`と`}`)。 セグメントに複数のプレース ホルダーを定義できますが、プレース ホルダーをリテラル値で区切る必要があります。 たとえば、`{language}-{country}/{action}`は有効なルート パターン。 ただし、`{language}{country}/{action}`リテラル値またはプレース ホルダーの間の区切り記号がないために、有効なパターンではありません。 そのため、ルーティング、国のプレース ホルダーの言語のプレース ホルダーの値から値を分離する場所を特定することはできません。

### <a name="mapping-and-registering-routes"></a>マッピングとルートを登録します。

Wingtip Toys のサンプル アプリケーションのページへのルートを含めることができます、前に、アプリケーションの起動時に、ルートを登録する必要があります。 ルートを登録するには、変更、`Application_Start`イベント ハンドラー。

1. **ソリューション エクスプ ローラー**Visual Studio での検索して開く、 *Global.asax.cs*ファイル。
2. 黄色で強調表示のコードを追加、 *Global.asax.cs*ファイルの次のようにします。   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wingtip Toys サンプル アプリケーションの起動時は、呼び出し、`Application_Start`イベント ハンドラー。 このイベント ハンドラーの最後に、`RegisterCustomRoutes`メソッドが呼び出されます。 `RegisterCustomRoutes`メソッドを呼び出して各ルートは追加、`MapPageRoute`のメソッド、`RouteCollection`オブジェクト。 ルートを定義するには、ルート名、ルート URL および物理的な URL を使用します。

最初のパラメーター ("`ProductsByCategoryRoute`") のルートの名前を指定します。 ルートを呼び出すことが必要な場合に使用されます。 2 番目のパラメーター ("`Category/{categoryName}`")、わかりやすい動的なことができる URL に基づく置換コードを定義します。 データ コントロールとそのデータに基づいて生成されるリンクを作成する場合は、このルートを使用します。 ルートが表示されます。

[!code-csharp[Main](url-routing/samples/sample2.cs)]

ルートの 2 番目のパラメーターには、中かっこで指定された、動的な値が含まれています (`{ }`)。 ここで、`categoryName`適切なルーティング パスを決定するために使用される変数です。

> [!NOTE] 
> 
> **Optional**
> 
> 移動することによって、コードの管理が容易に見つけることがあります、`RegisterCustomRoutes`別のクラスにメソッドです。 *ロジック*フォルダーごとに別々`RouteActions`クラスです。 上移動`RegisterCustomRoutes`メソッドから、 *Global.asax.cs*を新しいファイル`RoutesActions`クラスです。 使用して、`RoleActions`クラスおよび`createAdmin`メソッドを呼び出す方法の例として、`RegisterCustomRoutes`メソッドから、 *Global.asax.cs*ファイル。


お気付き、`RegisterRoutes`メソッドの呼び出しを使用して、`RouteConfig`の先頭にあるオブジェクト、`Application_Start`イベント ハンドラー。 この呼び出しが既定のルーティングを実装しようとしています。 Visual Studio の Web フォーム テンプレートを使用してアプリケーションを作成したときに既定のコードとして含まれています。

## <a name="retrieving-and-using-route-data"></a>取得して、ルート データを使用します。

前述のように、ルートを定義することができます。 追加したコード、`Application_Start`内のイベント ハンドラー、 *Global.asax.cs*ファイルは、定義可能なルートを読み込みます。

### <a name="setting-routes"></a>ルートの設定

ルートでは、追加のコードを追加する必要があります。 このチュートリアルでは、取得するモデル バインドを使用して.、`RouteValueDictionary`データ コントロールからのデータを使用してルートを生成するときに使用されるオブジェクト。 `RouteValueDictionary`オブジェクトが製品の特定のカテゴリに属する製品名の一覧に含まれます。 データ ルートに各製品のリンクが作成されます。

#### <a name="enable-routes-for-categories-and-products"></a>分類と製品のルートを有効にします。

次に、使用するアプリケーションを更新します、`ProductsByCategoryRoute`に含める各製品カテゴリのリンクの適切なルートを決定します。 更新することもあります、 *ProductList.aspx*ページに各製品のルーティング リンクが含まれています。 リンクが表示されます、変更前に、と同様に、リンク、URL ルーティングが使用されるようになりました。

1. **ソリューション エクスプ ローラー**を開き、 *Site.Master*ページがまだ開いていない場合。
2. 更新プログラム、 **ListView**という名前のコントロール"`categoryList`"黄色で強調表示、変更のため、マークアップように表示されます。   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. **ソリューション エクスプ ローラー**を開き、 *ProductList.aspx*ページ。
4. 更新プログラム、`ItemTemplate`の要素、 *ProductList.aspx*マークアップが次のように表示されるように、黄色で強調表示されている更新プログラムを含むページ。   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. コード ビハインドを開く*ProductList.aspx.cs*黄色で強調されている次の名前空間を追加します。  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. 置換、`GetProducts`分離コードのメソッド (*ProductList.aspx.cs*) を次のコード。   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>詳細については製品コードを追加します。

ここで、分離コードを更新 (*ProductDetails.aspx.cs*) の*ProductDetails.aspx*ルート データを使用するページ。 注意して、新しい`GetProduct`メソッドも古い非対応、非ルーティングの URL を使用して、ユーザーがブックマークが付けられたリンクを持つ場合、クエリ文字列の値を受け取ります。

1. 置換、`GetProduct`分離コードのメソッド (*ProductDetails.aspx.cs*) を次のコード。   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>アプリケーションの実行

今すぐ更新済みのルートを表示するアプリケーションを実行することができます。

1. キーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. クリックして、**製品**ページの上部にリンクします。  
 すべての製品が表示されます、 *ProductList.aspx*ページ。 ブラウザーの (使用するポート番号を使用して) 次の URL が表示されます。  
    `https://localhost:44300/ProductList`
3. 次をクリックして、**車**ページの上部にあるカテゴリのリンク。  
 Cars のみが表示されます、 *ProductList.aspx*ページ。 ブラウザーの (使用するポート番号を使用して) 次の URL が表示されます。  
    `https://localhost:44300/Category/Cars`
4. 最初の車の名前を含むリンクがページに表示 をクリックします ("**変換可能な車**") 製品の詳細を表示します。  
 ブラウザーの (使用するポート番号を使用して) 次の URL が表示されます。  
    `https://localhost:44300/Product/Convertible%20Car`
5. 次に、ブラウザーに (使用するポート番号を使用して) 次の非ルーティング URL を入力します。  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 コードでは、ユーザーがブックマークが付けられたリンクを持っている場合、クエリ文字列を含む、URL をまだ認識します。

## <a name="summary"></a>まとめ

このチュートリアルでは、分類と製品のルートを追加しました。 モデル バインディングを使用するデータ コントロールとルートを統合する方法について学習しました。 次のチュートリアルでは、グローバル エラー処理を実装します。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET Friendly Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Azure App Service のメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [前へ](membership-and-administration.md)
> [次へ](aspnet-error-handling.md)
