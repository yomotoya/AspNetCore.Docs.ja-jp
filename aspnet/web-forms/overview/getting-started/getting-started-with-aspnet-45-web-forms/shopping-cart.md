---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: ショッピング カート |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: a8e96da7737cdf649575711a464c4f7726cb6ded
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="shopping-cart"></a>ショッピング カート
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、ショッピング カートを Wingtip Toys サンプルの ASP.NET Web フォーム アプリケーションに追加するために必要なビジネス ロジックについて説明します。 このチュートリアルでは、前のチュートリアルでは、データ項目と詳細を表示」を上に構築され、Wingtip Toy ストア チュートリアル シリーズの一部です。 このチュートリアルを完了したときに、サンプル アプリのユーザーは追加、削除、および、ショッピング カートに製品を変更できるようになります。

## <a name="what-youll-learn"></a>学習する内容。

1. Web アプリケーションのショッピング カートを作成する方法。
2. ショッピング カートにアイテムを追加するユーザーを有効にする方法です。
3. 追加する方法、 [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction)ショッピング カートの詳細を表示するコントロール。
4. 計算され、注文の合計を表示する方法。
5. 削除し、ショッピング カートにアイテムを更新する方法です。
6. ショッピング カート カウンターを含める方法です。

## <a name="code-features-in-this-tutorial"></a>このチュートリアルではコードの機能:

1. Entity Framework Code First
2. データの注釈
3. 厳密に型指定のデータ コントロール
4. モデル バインド

## <a name="creating-a-shopping-cart"></a>ショッピング カートを作成します。

このチュートリアル シリーズの前半は、ページと、データベースから製品データを表示するコードを追加します。 このチュートリアルで作成管理製品をショッピング カートのユーザーが購入に興味を示しています。 参照して、登録またはログインしていない場合でも、ショッピング カートにアイテムを追加することができます。 ショッピング カートのアクセスを管理するユーザーに割り当てます、一意`ID`を初めてカート ショッピング ユーザーにアクセスするときに、グローバル一意識別子 (GUID) を使用します。 これを格納する`ID`ASP.NET セッション状態を使用します。

> [!NOTE] 
> 
> ASP.NET セッション状態は、期限切れになる、ユーザーが、サイトをユーザーに固有の情報を格納する便利な場所です。 セッション状態の誤用はパフォーマンスに影響を持つ大規模なサイトで、デモンストレーションのために、セッション状態の動作の使用が点灯します。 Wingtip Toys サンプル プロジェクトでは、ここで、セッション状態は、サイトをホストする web サーバーでインプロセスに格納されて、外部プロバイダーを使用しないセッション状態を使用する方法を示します。 アプリケーションの複数のインスタンスを提供する大規模なサイト、または別のサーバーで、アプリケーションの複数のインスタンスを実行するサイトの使用を検討**Windows Azure キャッシュ サービス**です。 このキャッシュ サービスは、外部 web サイトには、インプロセス セッション状態を使用しての問題を解決する分散キャッシュ サービスを提供します。 詳細については、「 [ASP.NET セッション状態は Windows Azure Web サイトを使用する方法](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider)です。


### <a name="add-cartitem-as-a-model-class"></a>モデル クラスとして CartItem を追加します。

このチュートリアル シリーズの前半作成することで、分類と製品データのスキーマを定義した、`Category`と`Product`内のクラス、*モデル*フォルダーです。 ここで、ショッピング カートのスキーマを定義する新しいクラスを追加します。 このチュートリアルの後半へのデータ アクセスを処理するクラスを追加する、`CartItem`テーブル。 このクラスでは、追加、削除、および、ショッピング カートにアイテムを更新するビジネス ロジックを提供します。

1. 右クリックし、*モデル*フォルダーと選択**追加** - &gt; **新しい項目の**します。 

    ![ショッピング カートの新しい項目の追加](shopping-cart/_static/image1.png)
2. **[新しい項目の追加]** ダイアログ ボックスが表示されます。 選択**コード**、し、**クラス**です。 

    ![ショッピング カート - 新しいアイテム ダイアログ ボックスを追加](shopping-cart/_static/image2.png)
3. この新しいクラスの名前を*CartItem.cs*です。
4. **[追加]** をクリックします。  
   新しいクラス ファイルがエディターで表示されます。
5. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem`クラスには、ショッピング カートに追加された各製品を定義するスキーマが含まれています。 このクラスは、このチュートリアル シリーズの前半で作成した他のスキーマ クラスに似ています。 慣例により、Entity Framework Code First 期待されるプライマリ キーを`CartItem`テーブルには、どちらかになります`CartItemId`または`ID`です。 ただし、コードがそのデータ注釈を使用して、既定の動作を上書き`[Key]`属性。 `Key` ItemId プロパティの属性を指定する、`ItemID`プロパティは、主キー。

`CartId`プロパティを指定します、`ID`を購入するアイテムに関連付けられているユーザーのです。 このユーザーを作成するコードを追加する`ID`ユーザーがショッピング カートにアクセスするときにします。 これは、`ID`も、ASP.NET セッション変数として格納されます。

### <a name="update-the-product-context"></a>製品のコンテキストを更新します。

追加するだけでなく、`CartItem`クラス、エンティティ クラスを管理して、データベースへのデータ アクセスを提供するデータベース コンテキストのクラスを更新する必要があります。 これを行うには追加、新しく作成された`CartItem`モデル クラスを`ProductContext`クラスです。

1. **ソリューション エクスプ ローラー**を検索して開く、 *ProductContext.cs*ファイルで、*モデル*フォルダーです。
2. 強調表示されたコードを追加、 *ProductContext.cs*ファイルの次のようにします。  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

このチュートリアル シリーズのコードで説明したように、 *ProductContext.cs*ファイルを追加、`System.Data.Entity`名前空間、Entity Framework のすべてのコア機能にアクセスできるようにします。 この機能には、クエリ、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除する機能が含まれています。 `ProductContext`クラスは、新たに追加するアクセス権を追加`CartItem`モデル クラス。

### <a name="managing-the-shopping-cart-business-logic"></a>ショッピング カートのビジネス ロジックを管理します。

次に作成、`ShoppingCart`クラスの新しい*ロジック*フォルダーです。 `ShoppingCart`クラスへのデータ アクセスは、処理、`CartItem`テーブル。 クラスは、追加、削除、および、ショッピング カートにアイテムを更新するビジネス ロジックも含まれます。

追加する、ショッピング カート ロジックを次の操作を管理する機能が含まれます。

1. ショッピング カートにアイテムを追加します。
2. ショッピング カートからアイテムの削除
3. ショッピング カート ID を取得します。
4. ショッピング カートから項目を取得します。
5. ショッピング カートのすべての項目の量の合計
6. ショッピング カートのデータの更新

買い物カゴのページ (*後*)、ショッピング カートのデータにアクセスする、ショッピング カート クラスを一緒に使用されます。 ユーザーがショッピング カートに追加するすべての項目は、買い物カゴのページが表示されます。 さらに、ショッピング カートのページとクラス、ページを作成します (*AddToCart.aspx*) 製品をショッピング カートに追加します。 コードを追加、 *ProductList.aspx*ページおよび*ProductDetails.aspx*ページへのリンクを提供する、 *AddToCart.aspx*  ページで、ユーザーを追加できるようにショッピング カートに製品です。

次の図は、ユーザーがショッピング カートに製品を追加するときに発生する基本的なプロセスを示します。

![ショッピング カートのショッピング カートに追加します。](shopping-cart/_static/image3.png)

ユーザーがクリックしたとき、 **Add To Cart**いずれかのリンク、 *ProductList.aspx*ページまたは*ProductDetails.aspx*  ページで、アプリケーションに移動、 *AddToCart.aspx*ページし、自動的に、*後*ページ。 *AddToCart.aspx*ページは製品に追加を選択、ショッピング カート ShoppingCart クラスのメソッドを呼び出しています。 *後*ページ ショッピング カートに追加されている製品が表示されます。

#### <a name="creating-the-shopping-cart-class"></a>ショッピング カート クラスを作成します。

`ShoppingCart`クラスは、モデル (Models フォルダー)、(ルート フォルダー) のページおよびロジック (ロジック フォルダー) を明確に区別があるように、アプリケーション内の別のフォルダーに追加されます。

1. **ソリューション エクスプ ローラー**を右クリックし、 **WingtipToys**プロジェクトし、選択**追加**-&gt;**新しいフォルダー**. 新しいフォルダーの名前を付けます*ロジック*です。
2. 右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。
3. という新しいクラス ファイルを追加*ShoppingCartActions.cs*です。
4. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart`メソッドにより、製品に基づいて、ショッピング カートに含まれる個々 の製品`ID`です。 製品は、カートに追加またはカートが既にその製品の項目を含んでいる場合、数量がインクリメントされます。

`GetCartId`メソッドを返します、カート`ID`ユーザー。 カート`ID`ユーザーがショッピング カートに入っている項目を追跡するために使用します。 ユーザーが既存のカート`ID`、新しいカート`ID`それらに作成されました。 ユーザーがカート登録済みのユーザーとしてサインインしているかどうかは`ID`がユーザー名に設定されています。 ただし、ユーザーがカートに署名されていない場合`ID`が一意の値 (GUID) に設定します。 GUID により、セッションに基づく、各ユーザーの 1 つしかをカートが作成されます。

`GetCartItems`メソッド ショッピング カート項目、ユーザーの一覧を返します。 このチュートリアルの後半で項目を表示する、カート ショッピング カートを使用して、モデル バインディングが使用されることを確認は、`GetCartItems`メソッドです。

### <a name="creating-the-add-to-cart-functionality"></a>カートに追加機能の作成

名前付き処理ページを作成する、前述のように、 *AddToCart.aspx*ユーザーのショッピング カートに新製品の追加に使用されます。 このページが呼び出す、`AddToCart`メソッドで、`ShoppingCart`先ほど作成したクラスです。 *AddToCart.aspx*ページを必要とする製品`ID`に渡されます。 この製品`ID`呼び出しに使用される、`AddToCart`メソッドで、`ShoppingCart`クラスです。

> [!NOTE] 
> 
> 分離コードを変更する (*AddToCart.aspx.cs*) のこのページでは、ページの UI ではなく (*AddToCart.aspx*)。


#### <a name="to-create-the-add-to-cart-functionality"></a>カートに追加を作成する機能。

1. **ソリューション エクスプ ローラー**を右クリックし、 **WingtipToys**プロジェクトで、をクリックして**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 標準の新しいページ (Web フォーム) をという名前のアプリケーションに追加*AddToCart.aspx*です。 

    ![ショッピング カートの Web フォームの追加](shopping-cart/_static/image4.png)
3. **ソリューション エクスプ ローラー**を右クリックし、 *AddToCart.aspx*ページし、をクリックして**コードの表示**です。 *AddToCart.aspx.cs*分離コード ファイルがエディターで開きます。
4. 既存のコードを置き換える、 *AddToCart.aspx.cs*に次のコードの分離。   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

ときに、 *AddToCart.aspx*ページが読み込まれる、製品`ID`クエリ文字列から取得されます。 次に、ショッピング カート クラスのインスタンスが作成され、呼び出しに使用するが、`AddToCart`このチュートリアルで先ほど追加したメソッドにします。 `AddToCart`メソッドに含まれている、 *ShoppingCartActions.cs*ファイルを選択した製品をショッピング カートに追加または選択した製品の製品の数量をインクリメントするためのロジックが含まれています。 製品を追加する場合は、製品をショッピング カートに追加されていない、`CartItem`データベースのテーブルです。 製品は既にショッピング カートに追加されており、ユーザーが、同じ製品のアイテムを追加、製品の数量が大きくで、`CartItem`テーブル。 ページが最後に、リダイレクト、*後*ユーザーがカート内の項目の更新された一覧に表示されます、次の手順で追加するページ。

既に触れましたが、ユーザー`ID`特定のユーザーに関連付けられている製品を識別するために使用します。 これは、`ID`内の行を追加、`CartItem`表に、ユーザーがショッピング カートに商品を追加するたびにします。

### <a name="creating-the-shopping-cart-ui"></a>ショッピング カート UI を作成します。

*後*ページ、ユーザーがショッピング カートに追加する製品が表示されます。 追加、削除、および、ショッピング カートにアイテムを更新する機能も提供されます。

1. **ソリューション エクスプ ローラー**を右クリックして**WingtipToys**をクリックして**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択すると、マスター ページを含む新しいページ (Web フォーム) を追加**マスター ページを使用して Web フォーム**です。 新しいページの名前を付けます*後*です。
3. 選択**Site.Master**に新しく作成されたマスター ページをアタッチする*.aspx*ページ。
4. *後* ページで、既存のマークアップを次のマークアップに置き換えます。   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*後*ページが含まれています、 **GridView**という名前のコントロール`CartList`です。 このコントロールでは、モデル バインディングを使用して、ショッピング カートのデータをデータベースからバインドを**GridView**コントロール。 設定すると、`ItemType`のプロパティ、 **GridView**制御、データ バインディング式`Item`で使用できるは、コントロールとコントロールのマークアップが厳密に型指定します。 詳細を選択できますで既に述べたようこのチュートリアルのシリーズ、`Item`オブジェクトの IntelliSense を使用します。 設定するデータを選択するモデル バインドを使用するデータ コントロールを構成するのには`SelectMethod`コントロールのプロパティです。 上記のマークアップでは、設定、`SelectMethod`の一覧を返す GetShoppingCartItems メソッドを使用して`CartItem`オブジェクト。 **GridView**データ コントロールを選択し、ページのライフ サイクルの適切なタイミングで、メソッドを呼び出し、返されたデータを自動的にバインドします。 `GetShoppingCartItems`メソッドを追加する必要があります。

#### <a name="retrieving-the-shopping-cart-items"></a>ショッピング カートのアイテムを取得します。

次に、コードを追加、 *ShoppingCart.aspx.cs*分離コードを取得し、ショッピング カート UI を作成します。

1. **ソリューション エクスプ ローラー**を右クリックし、*後*ページし、をクリックして**コードの表示**です。 *ShoppingCart.aspx.cs*分離コード ファイルがエディターで開きます。
2. 既存のコードを次のコードに置き換えます。  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

、上記のように、`GridView`データの制御呼び出し、`GetShoppingCartItems`ページ ライフで適切なタイミングでメソッドが順番に表示して、返されたデータを自動的にバインドします。 `GetShoppingCartItems`メソッドのインスタンスを作成、`ShoppingCartActions`オブジェクト。 呼び出すことによって、カートにアイテムを返す次に、コードがそのインスタンスを使用して、`GetCartItems`メソッドです。

### <a name="adding-products-to-the-shopping-cart"></a>ショッピング カートに商品を追加します。

ときにいずれか、 *ProductList.aspx*または*ProductDetails.aspx*ページが表示されたら、ユーザーは、リンクを使用して、ショッピング カートに製品を追加することになります。 リンクをクリックすると、アプリケーション ページに移動する、処理という*AddToCart.aspx*です。 *AddToCart.aspx*ページが呼び出す、`AddToCart`メソッドで、`ShoppingCart`このチュートリアルで先ほど追加したクラスです。

ここに追加、**をカートに追加**両方へのリンク、 *ProductList.aspx*ページおよび*ProductDetails.aspx*ページ。 このリンクは、製品、`ID`データベースから取得されます。

1. **ソリューション エクスプ ローラー**を検索してという名前のページを開く*ProductList.aspx*です。
2. 黄色で強調表示のマークアップを追加、 *ProductList.aspx*ページ全体のページが次のように表示されるようにします。  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>ショッピング カートのテスト

ショッピング カートに製品を追加する方法を表示するアプリケーションを実行します。

1. **F5** キーを押してアプリケーションを実行します。  
 プロジェクトが、データベースを再作成した後、ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**車**ナビゲーション メニューのカテゴリ。  
 *ProductList.aspx* 「自動車」カテゴリに含まれる製品のみが表示されたページが表示されます。 

    ![ショッピング カートの自動車](shopping-cart/_static/image5.png)
3. クリックして、**をカートに追加**最初の製品の横にあるリンク (変換 car) を一覧表示します。   
 *後*ページが表示されたら、ショッピング カートの選択範囲を表示します。 

    ![ショッピング カートのカート](shopping-cart/_static/image6.png)
4. その他の製品を選択して表示**平面**ナビゲーション メニューのカテゴリ。
5. クリックして、**をカートに追加**表示されている最初の製品の横にあるリンクします。  
 *後*ページには、追加の項目が表示されます。
6. ブラウザーを閉じます。

### <a name="calculating-and-displaying-the-order-total"></a>計算し、注文の合計を表示します。

追加の製品をショッピング カートに追加するだけでなく、`GetTotal`メソッドを`ShoppingCart`クラスし、買い物カゴのページで注文金額の合計を表示します。

1. **ソリューション エクスプ ローラー**を開き、 *ShoppingCartActions.cs*ファイルで、*ロジック*フォルダーです。
2. 次の追加`GetTotal`に黄色で強調表示されているメソッド、`ShoppingCart`クラス、クラスは次のように表示されるようにします。   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

最初に、`GetTotal`メソッドは、ユーザーのショッピング カートの ID を取得します。 そのメソッドでは、カートに各製品の製品の数量が製品価格を乗算することによって合計カートの内容を取得します。

> [!NOTE] 
> 
> 上記のコードでは、null 許容型を使用して"`int?`"です。 Null 許容型では、すべての値の基になる型として、また、null 値を表すことができます。 詳細については、「 [null 許容型を使用して](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx)です。


### <a name="modify-the-shopping-cart-display"></a>ショッピング カート表示を変更します。

次のコードを変更、*後*ページから呼び出すこと、`GetTotal`メソッドと上の合計を表示、*後*ページが読み込まれるページします。

1. **ソリューション エクスプ ローラー**を右クリックし、*後*ページし、選択**コードの表示**です。
2. *ShoppingCart.aspx.cs*ファイル、更新、`Page_Load`黄色で強調表示されている次のコードを追加することによってハンドラー。   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

ときに、*後*ページが読み込まれる、ショッピング カート オブジェクトをロードし、呼び出すことによって、ショッピング カートの合計を取得、`GetTotal`のメソッド、`ShoppingCart`クラスです。 ショッピング カートの内容が空の場合、メッセージが表示が表示されます。

### <a name="testing-the-shopping-cart-total"></a>ショッピング カートの合計金額をテストします。

できましていないのみに追加する方法、製品、ショッピング カートを表示するアプリケーションを実行しますが、ショッピング カートの合計金額を表示できます。

1. **F5** キーを押してアプリケーションを実行します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**車**ナビゲーション メニューのカテゴリ。
3. クリックして、 **Add To Cart**最初の製品の横にあるリンクします。   
 *後*ページには、注文の合計が表示されます。 

    ![ショッピング カートのカートの合計金額](shopping-cart/_static/image7.png)
4. その他の製品の一部 (たとえば、平面) をカートに追加します。
5. *後*ページには、追加したすべての製品の更新後の合計が表示されます。 

    ![ショッピング カートの複数の製品](shopping-cart/_static/image8.png)
6. ブラウザー ウィンドウを閉じるには、実行中のアプリを停止します。

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>ショッピング カートに更新プログラムとチェック アウトのボタンの追加

追加、ショッピング カートを変更するユーザーを許可するのに、**更新**ボタンおよび**チェック アウト**買い物カゴのページをクリックします。 **チェック アウト**ボタンは、このチュートリアルのシリーズに進むまでは使用されません。

1. **ソリューション エクスプ ローラー**を開き、*後* ページで、web アプリケーション プロジェクトのルートです。
2. 追加する、**更新**ボタンをクリックし、**チェック アウト** ボタンをクリックして、*後*ページで、既存のマークアップを黄色で強調表示するマークアップを追加で示すように、次のコード:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

ユーザーがクリックしたとき、**更新** ボタン、`UpdateBtn_Click`イベント ハンドラーが呼び出されます。 このイベント ハンドラーでは、次の手順で追加するコードを呼び出します。

次に含まれるコードを更新することができます、 *ShoppingCart.aspx.cs*カート項目と呼び出しをループするファイル、`RemoveItem`と`UpdateItem`メソッドです。

1. **ソリューション エクスプ ローラー**を開き、 *ShoppingCart.aspx.cs* web アプリケーション プロジェクトのルートにあるファイルです。
2. 黄色で強調表示されている次のコード セクションを追加、 *ShoppingCart.aspx.cs*ファイル。   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

ユーザーがクリックしたとき、**更新**のボタンでは、*後* ページで、UpdateCartItems メソッドが呼び出されます。 UpdateCartItems メソッドは、ショッピング カート内の各項目を更新する値を取得します。 UpdateCartItems メソッドを呼び出して、`UpdateShoppingCartDatabase`メソッド (追加され、次の手順で説明) を追加するか、ショッピング カートから項目を削除します。 ショッピング カートへの更新を反映するように、データベースを更新したら、 **GridView**買い物カゴのページで呼び出すことによってコントロールが更新された、`DataBind`のメソッド、 **GridView**です。 また、買い物カゴのページで注文の合計量は項目の更新されたリストを反映するように更新されます。

### <a name="updating-and-removing-shopping-cart-items"></a>更新やショッピング カートの項目を削除

*後* ページで、アイテムの数量を更新し、項目を削除するためのコントロールの追加確認できます。 これで、これらのコントロール動作を構成するコードを追加します。

1. **ソリューション エクスプ ローラー**を開き、 *ShoppingCartActions.cs*ファイルで、*ロジック*フォルダーです。
2. 黄色で強調表示されている次のコードを追加、 *ShoppingCartActions.cs*クラス ファイル。   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase`から呼び出されるメソッド、`UpdateCartItems`メソッドを*ShoppingCart.aspx.cs*  ページで、更新するか、ショッピング カートから項目を削除するためのロジックが含まれています。 `UpdateShoppingCartDatabase`メソッドは、ショッピング カート リスト内のすべての行を反復処理します。 ショッピング カートのアイテムが削除されるマークされてまたは数量が 1 より小さい場合、`RemoveItem`メソッドが呼び出されます。 ショッピング カート項目がチェックされるそれ以外の場合、更新プログラムの場合、`UpdateItem`メソッドが呼び出されます。 ショッピング カートのアイテムが削除されたか、更新、データベースの変更は保存されます。

`ShoppingCartUpdates`ショッピング カートのすべての項目を保持する構造体が使用されます。 `UpdateShoppingCartDatabase`メソッドを使用、`ShoppingCartUpdates`構造を更新または削除する必要がかどうか、項目のいずれかあるを判断します。

次のチュートリアルでは使用して、`EmptyCart`製品を購入した後、ショッピングを消去するメソッドがカートです。 ここでは、使用するが、`GetCount`に追加したメソッド、 *ShoppingCartActions.cs*ファイル、ショッピング カート内の項目の数を調べることです。

### <a name="adding-a-shopping-cart-counter"></a>ショッピング カート カウンターの追加

ユーザーに、ショッピング カートにアイテムの合計数を表示するに追加するカウンターを*Site.Master*ページ。 このカウンターは、ショッピング カートへのリンクとしても機能します。

1. **ソリューション エクスプ ローラー**を開き、 *Site.Master*ページ。
2. 黄色で示したナビゲーション セクションに次のように表示されるように、ショッピング カート カウンターのリンクを追加することで、マークアップを変更します。  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. 分離コードを次に、更新、 *Site.Master.cs*次のように、黄色で強調表示コードを追加することによってファイル。  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

ページは HTML としてレンダリングされる前に、`Page_PreRender`イベントが発生します。 `Page_PreRender`ハンドラー、ショッピング カートの合計数が呼び出すことによって決まりますが、`GetCount`メソッドです。 返される値に追加、`cartCount`スパンのマークアップに含まれる、 *Site.Master*ページ。 `<span>`タグにより、正常にレンダリングされる内部の要素。 サイトの任意のページが表示されたら、ショッピング カートの合計金額が表示されます。 ユーザーには、ショッピング カートの合計金額、ショッピング カートを表示するクリックもできます。

## <a name="testing-the-completed-shopping-cart"></a>完了したショッピング カートのテスト

今すぐアプリケーションを追加する方法を表示、削除、および更新アイテムは、ショッピング カートで実行できます。 ショッピング カートの合計金額、ショッピング カート内のすべての項目の総コストが反映されます。

1. **F5** キーを押してアプリケーションを実行します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**車**ナビゲーション メニューのカテゴリ。
3. クリックして、 **Add To Cart**最初の製品の横にあるリンクします。   
 *後*ページには、注文の合計が表示されます。
4. 選択**平面**ナビゲーション メニューのカテゴリ。
5. クリックして、 **Add To Cart**最初の製品の横にあるリンクします。
6. 3 に、ショッピング カート内の最初の項目の数量を設定し、選択、**アイテムの削除**2 番目の項目のチェック ボックスをオンします。<a id="a"></a>
7. クリックして、**更新**買い物カゴのページを更新および新しい注文の合計を表示するボタンをクリックします。 

    ![ショッピング カートの買い物カゴの更新](shopping-cart/_static/image9.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、Wingtip Toys Web フォーム サンプル アプリケーションのショッピング カートを作成しました。 このチュートリアルでは、Entity Framework Code First、データ注釈、厳密に型指定されたデータ コントロール、およびモデル バインディングを使用しています。

ショッピング カートには、追加、削除、およびユーザーが注文書の選択項目の更新がサポートしています。 ショッピング カートの項目を表示する方法を習得買い物カゴの機能を実装するだけでなく、 **GridView**を制御し、注文の合計を計算します。

## <a name="addition-information"></a>追加情報

[ASP.NET セッション状態の概要](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [前へ](display_data_items_and_details.md)
> [次へ](checkout-and-payment-with-paypal.md)
