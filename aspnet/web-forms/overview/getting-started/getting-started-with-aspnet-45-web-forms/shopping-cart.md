---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: ショッピング カート |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 22253b8708efd9ce505c9fbeb9cb2e942588b37d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375779"
---
<a name="shopping-cart"></a>ショッピング カート
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、ショッピング カートを Wingtip Toys のサンプルの ASP.NET Web フォーム アプリケーションに追加するために必要なビジネス ロジックについて説明します。 このチュートリアルでは、前のチュートリアルでは、データ項目と詳細を表示」に基づいており、Wingtip 玩具店のチュートリアル シリーズのパートです。 このチュートリアルを完了すると、ときに、サンプル アプリのユーザーは追加、削除、およびショッピング カートに製品を変更することになります。

## <a name="what-youll-learn"></a>学習内容。

1. Web アプリケーションのショッピング カートを作成する方法。
2. ショッピング カートにアイテムを追加するユーザーを有効にする方法。
3. 追加する方法、 [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction)ショッピング カートの詳細を表示するコントロール。
4. 計算され、注文の合計を表示する方法。
5. 削除し、ショッピング カートのアイテムを更新する方法。
6. ショッピング カートのカウンターを含める方法です。

## <a name="code-features-in-this-tutorial"></a>このチュートリアルではコードの機能:

1. エンティティ フレームワーク コード ファースト
2. データの注釈
3. 厳密に型指定されたデータ コントロール
4. モデル バインド

## <a name="creating-a-shopping-cart"></a>ショッピング カートの作成

このチュートリアル シリーズで前のページと、データベースから製品データを表示するコードを追加します。 このチュートリアルでは、製品を管理するショッピング カートを作成しますユーザーが購入に関心があること。 参照して、登録またはログインしていない場合でも、ショッピング カートにアイテムを追加することができます。 ショッピング カートへのアクセスを管理するユーザーに割り当てます一意`ID`初めてカート、ショッピング、ユーザーにアクセスするときに、グローバル一意識別子 (GUID) を使用します。 これを格納する`ID`ASP.NET セッション状態を使用します。

> [!NOTE] 
> 
> ASP.NET セッション状態は、期限切れ後、ユーザーが、サイトのままになるユーザー固有の情報を格納する便利な場所です。 セッション状態の不正使用はパフォーマンスに影響を持つ大規模なサイトで、セッションの状態の動作もデモンストレーションの目的で使用するライトです。 Wingtip Toys のサンプル プロジェクトでは、セッション状態が、サイトをホストする web サーバーでインプロセスでストアドは、外部のプロバイダーを使用しないセッション状態を使用する方法を示します。 アプリケーションの複数のインスタンスを提供する大規模なサイト、または異なるサーバーで、アプリケーションの複数のインスタンスを実行するサイトについては、使用を検討して**Windows Azure キャッシュ サービス**します。 このキャッシュ サービスでは、外部 web サイトには、インプロセス セッション状態を使用しての問題を解決する分散キャッシュ サービスを提供します。 詳細については、「 [ASP.NET セッション状態で、Windows Azure Web サイトを使用する方法](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider)します。


### <a name="add-cartitem-as-a-model-class"></a>モデル クラスとして CartItem を追加します。

このチュートリアル シリーズで前に作成して、カテゴリおよび製品のデータのスキーマを定義した、`Category`と`Product`クラス、*モデル*フォルダー。 ショッピング カートのスキーマを定義する新しいクラスを追加します。 データ アクセスを処理するクラスを追加する、このチュートリアルの後半で、`CartItem`テーブル。 このクラスでは、追加、削除、およびショッピング カートのアイテムを更新するビジネス ロジックを提供します。

1. 右クリックし、*モデル*フォルダーと選択**追加** - &gt; **新しい項目の**します。 

    ![ショッピング カート - 新しい項目](shopping-cart/_static/image1.png)
2. **[新しい項目の追加]** ダイアログ ボックスが表示されます。 選択**コード**、し、**クラス**します。 

    ![ショッピング カート - 新しい項目 ダイアログ ボックスの追加](shopping-cart/_static/image2.png)
3. この新しいクラスの名前を*CartItem.cs*します。
4. **[追加]** をクリックします。  
   新しいクラス ファイルがエディターで表示されます。
5. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem`クラスには、ユーザーがショッピング カートに追加する各製品を定義するスキーマが含まれています。 このクラスは、このチュートリアル シリーズで作成した他のスキーマ クラスに似ています。 慣例により、Entity Framework Code First いるものと想定の主キー、`CartItem`テーブルには、どちらかになります`CartItemId`または`ID`します。 ただし、コードはデータ注釈を使用して、既定の動作を上書き`[Key]`属性。 `Key` ItemId プロパティの属性を指定する、`ItemID`プロパティは、プライマリ キーです。

`CartId`プロパティを指定します、`ID`を購入するアイテムに関連付けられているユーザーの。 このユーザーを作成するコードを追加します`ID`ユーザーがショッピング カートにアクセスします。 これは、`ID`も、ASP.NET セッション変数として格納されます。

### <a name="update-the-product-context"></a>製品のコンテキストを更新します。

追加するだけでなく、`CartItem`クラス、エンティティ クラスは、管理し、データベースへのデータ アクセスを提供する、データベース コンテキスト クラスを更新する必要があります。 これには、追加すると、新しく作成された`CartItem`モデル クラスを`ProductContext`クラス。

1. **ソリューション エクスプ ローラー**を検索して開く、 *ProductContext.cs*ファイル、*モデル*フォルダー。
2. 強調表示されたコードを追加、 *ProductContext.cs*ファイルの次のようにします。  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

コードで、このチュートリアル シリーズで説明したよう、 *ProductContext.cs*ファイルを追加、`System.Data.Entity`名前空間、Entity Framework のすべてのコア機能にアクセスするようにします。 この機能には、クエリ、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除する機能が含まれています。 `ProductContext`クラスを新しく追加したアクセス権を追加します`CartItem`モデル クラス。

### <a name="managing-the-shopping-cart-business-logic"></a>ショッピング カートのビジネス ロジックを管理します。

次に作成、`ShoppingCart`クラスの新しい*ロジック*フォルダー。 `ShoppingCart`クラスへのデータ アクセスの処理、`CartItem`テーブル。 クラスは、追加、削除、およびショッピング カートのアイテムを更新するビジネス ロジックも含まれます。

追加するショッピング カートのロジックは、次の操作を管理する機能が含まれます。

1. ショッピング カートにアイテムを追加
2. ショッピング カートから項目を削除します。
3. ショッピング カート ID を取得します。
4. ショッピング カートから項目を取得します。
5. ショッピング カートのアイテムの量の合計
6. ショッピング カートのデータを更新しています

ショッピング カート ページ (*後*)、ショッピング カート データにアクセスするショッピング カート クラスをまとめて使用されます。 ユーザーがショッピング カートに追加するすべてのアイテムがショッピング カート ページに表示されます。 さらに、ショッピング カートのページとクラス、ページを作成します (*AddToCart.aspx*) 製品をショッピング カートに追加します。 コードを追加、 *ProductList.aspx*ページと*ProductDetails.aspx*ページへのリンクを提供する、 *AddToCart.aspx*ページのユーザーを追加できるようにショッピング カートに製品です。

次の図は、ユーザーがショッピング カートに商品を追加するときに発生する基本的なプロセスを示します。

![ショッピング カートのショッピング カートに追加します。](shopping-cart/_static/image3.png)

ユーザーがクリックすると、 **Add To Cart**いずれかのリンク、 *ProductList.aspx*ページまたは*ProductDetails.aspx*  ページで、アプリケーションは、に移動されます*AddToCart.aspx*ページし、自動的に、*後*ページ。 *AddToCart.aspx*ページは ShoppingCart クラスでメソッドを呼び出すことによって、ショッピング カートに製品を選択するを追加するされます。 *後*ページ ショッピング カートに追加されている製品が表示されます。

#### <a name="creating-the-shopping-cart-class"></a>ショッピング カート クラスを作成します。

`ShoppingCart`クラスは、モデル (モデル フォルダー)、ページ (ルート フォルダー)、およびロジック (ロジック フォルダー) を明確に区別があるように、アプリケーションで別のフォルダーに追加されます。

1. **ソリューション エクスプ ローラー**を右クリックし、 **WingtipToys**順に選択して**追加**-&gt;**新しいフォルダー**. 新しいフォルダーの名前*ロジック*します。
2. 右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。
3. という名前の新しいクラス ファイルを追加*ShoppingCartActions.cs*します。
4. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart`メソッドにより、製品に基づいて、ショッピング カートに含まれる個々 の製品`ID`します。 製品は、カートに追加またはカートの内容には、その製品の項目が既に含まれています、数量が増加します。

`GetCartId`カートの内容を返します`ID`ユーザー。 カート`ID`ユーザーがショッピング カートに入っている項目を追跡するために使用します。 ユーザーが既存のカートを持たない場合`ID`、新しいカート`ID`に作成されます。 ユーザーがカート、登録済みユーザーとしてサインインしているかどうかは`ID`ユーザー名に設定されます。 ただし、ユーザーがカートに署名がない場合`ID`一意の値 (GUID) に設定されます。 GUID はによってその 1 つだけのカートがセッションに基づく、ユーザーごとに作成されます。

`GetCartItems`メソッドは、ショッピング カートのアイテムのユーザーの一覧を返します。 このチュートリアルで後でモデル バインドを使用すると、ショッピング カートを使用して、カートのアイテムを表示することを確認は、`GetCartItems`メソッド。

### <a name="creating-the-add-to-cart-functionality"></a>カートに追加機能の作成

前述のように、という処理ページを作成します*AddToCart.aspx*使用される新しい製品をユーザーのショッピング カートに追加します。 このページが呼び出す、`AddToCart`メソッドで、`ShoppingCart`作成したクラス。 *AddToCart.aspx*ページを予想する製品`ID`渡されます。 この製品`ID`を呼び出すときに使用される、`AddToCart`メソッドで、`ShoppingCart`クラス。

> [!NOTE] 
> 
> 分離コードを変更する (*AddToCart.aspx.cs*) のこのページでは、ページの UI ではなく (*AddToCart.aspx*)。


#### <a name="to-create-the-add-to-cart-functionality"></a>カートに追加を作成する機能。

1. **ソリューション エクスプ ローラー**を右クリックし、 **WingtipToys**プロジェクトで、をクリックして**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 標準的な新しいページ (Web フォーム) という名前のアプリケーションを追加*AddToCart.aspx*します。 

    ![ショッピング カートの Web フォームの追加](shopping-cart/_static/image4.png)
3. **ソリューション エクスプ ローラー**を右クリックし、 *AddToCart.aspx*ページし、クリックして**コードの表示**します。 *AddToCart.aspx.cs*分離コード ファイルが、エディターで開かれます。
4. 既存のコードを置き換える、 *AddToCart.aspx.cs*に次のコード ビハインド。   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

ときに、 *AddToCart.aspx*ページの読み込み、製品`ID`クエリ文字列から取得されます。 次に、ショッピング カート クラスのインスタンスが作成され、呼び出しに使用されるが、`AddToCart`このチュートリアルで先ほど追加したメソッド。 `AddToCart`に含まれているメソッド、 *ShoppingCartActions.cs*ファイルで、選択した製品をショッピング カートに追加または選択した製品の製品の数量をインクリメントするためのロジックが含まれています。 製品を追加する場合は、製品をショッピング カートに追加されていない、`CartItem`データベースのテーブル。 商品の数量を増分するショッピング カートに製品は既に追加されて、ユーザーが、同じ製品の他の項目を追加、`CartItem`テーブル。 最後に、ページのリダイレクトを*後*カート内の項目の最新の一覧を表示、次の手順で追加するページ。

既に触れましたが、ユーザー`ID`特定のユーザーに関連付けられている製品を識別するために使用します。 これは、`ID`内の行を追加、`CartItem`表に、ユーザーがショッピング カートに商品を追加するたびにします。

### <a name="creating-the-shopping-cart-ui"></a>ショッピング カートの UI を作成します。

*後*ページ、ユーザーが買い物カゴに追加する製品が表示されます。 追加、削除、およびショッピング カートのアイテムを更新する機能も提供されます。

1. **ソリューション エクスプ ローラー**、右クリックして**WingtipToys**、 をクリックして**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 新しいページを追加 (Web フォーム) を選択して、マスター ページを含む**マスター ページを使用して Web フォーム**します。 名前を新しいページ*後*します。
3. 選択**Site.Master**を新しく作成されたマスター ページをアタッチする *.aspx*ページ。
4. *後* ページで、既存のマークアップを次のマークアップに置き換えます。   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*後*ページが含まれています、 **GridView**という名前のコントロール`CartList`します。 このコントロールでは、モデル バインドを使用して、データベースからのショッピング カートのデータをバインドする、 **GridView**コントロール。 設定すると、`ItemType`のプロパティ、 **GridView**制御、データ バインディング式`Item`で使用できるは、コントロールとコントロールのマークアップが厳密に型指定されました。 このチュートリアル シリーズで前に述べた、に応じて選択の詳細、 `Item` IntelliSense を使用してオブジェクトします。 モデル バインドを使用してデータを選択するデータ コントロールを構成するに設定する、`SelectMethod`コントロールのプロパティ。 上記のマークアップでは、設定、`SelectMethod`の一覧を返す GetShoppingCartItems メソッドを使用して`CartItem`オブジェクト。 **GridView**データ コントロールがページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、返されるデータを自動的にバインドします。 `GetShoppingCartItems`メソッドを追加する必要があります。

#### <a name="retrieving-the-shopping-cart-items"></a>ショッピング カートのアイテムを取得します。

次に、コードを追加、 *ShoppingCart.aspx.cs*分離コードを取得し、ショッピング カート UI を設定します。

1. **ソリューション エクスプ ローラー**を右クリックし、*後*ページし、クリックして**コードの表示**します。 *ShoppingCart.aspx.cs*分離コード ファイルが、エディターで開かれます。
2. 既存のコードを次のコードに置き換えます。  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

上記で説明したように、`GridView`データの制御呼び出し、`GetShoppingCartItems`メソッドが適切な時点で、ページのライフ サイクルし、返されるデータを自動的にバインドします。 `GetShoppingCartItems`メソッドのインスタンスを作成、`ShoppingCartActions`オブジェクト。 コードがそのインスタンスを使用して呼び出すことによって、カートにアイテムを返す、`GetCartItems`メソッド。

### <a name="adding-products-to-the-shopping-cart"></a>ショッピング カートに商品を追加します。

ときにいずれか、 *ProductList.aspx*または*ProductDetails.aspx*ページが表示されたら、ユーザーは、リンクを使用して、ショッピング カートに製品を追加することになります。 アプリケーションがという名前の処理のページに移動、リンクをクリックしたときに*AddToCart.aspx*します。 *AddToCart.aspx*ページが呼び出す、`AddToCart`メソッドで、`ShoppingCart`このチュートリアルで先ほど追加したクラス。

ここに追加、**カートに追加**両方へのリンク、 *ProductList.aspx*ページと*ProductDetails.aspx*ページ。 このリンクは、製品、`ID`データベースから取得されます。

1. **ソリューション エクスプ ローラー**を検索してという名前のページを開く*ProductList.aspx*します。
2. 黄色で強調表示のマークアップを追加、 *ProductList.aspx*ページ全体のページが次のように表示されるようにします。  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>ショッピング カートのテスト

アプリケーションを実行して、ショッピング カートに製品を追加する方法を参照してください。

1. **F5** キーを押してアプリケーションを実行します。  
 プロジェクトが、データベースを再作成した後、ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**自動車**カテゴリのナビゲーション メニュー。  
 *ProductList.aspx* "Cars"カテゴリに含まれる製品のみを示すページが表示されます。 

    ![ショッピング カート - 自動車](shopping-cart/_static/image5.png)
3. をクリックして、**カートに追加**最初の製品の横にあるリンク (変換 car) を一覧表示します。   
 *後*ページが表示されます、ショッピング カート内の選択範囲を表示します。 

    ![ショッピング カートのカート](shopping-cart/_static/image6.png)
4. その他の製品を選択して表示**平面**カテゴリのナビゲーション メニュー。
5. をクリックして、**カートに追加**最初に表示されている製品の横にあるリンクです。  
 *後*ページには、追加の項目が表示されます。
6. ブラウザーを閉じます。

### <a name="calculating-and-displaying-the-order-total"></a>計算して注文の合計を表示します。

追加製品をショッピング カートに追加すると、だけでなく、`GetTotal`メソッドを`ShoppingCart`クラスし、ショッピング カート ページで、合計注文金額を表示します。

1. **ソリューション エクスプ ローラー**、オープン、 *ShoppingCartActions.cs*ファイル、*ロジック*フォルダー。
2. 次の追加`GetTotal`に黄色で強調表示されているメソッド、`ShoppingCart`クラス、クラスは次のように表示されるように。   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

まず、`GetTotal`メソッドは、ユーザーのショッピング カートの ID を取得します。 その後、メソッドでは、カート内の各製品の製品の数量の製品価格を掛けることによって合計カートの内容を取得します。

> [!NOTE] 
> 
> 上記のコードは、null 許容型を使用して"`int?`"。 Null 許容型では、基になる型とも null 値をすべての値を表すことができます。 詳細については、「 [null 許容型を使用して](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx)います。


### <a name="modify-the-shopping-cart-display"></a>ショッピング カートの表示を変更します。

次のコードを変更、*後*を呼び出すページ、`GetTotal`メソッドと合計を表示、*後*ページが読み込まれるページします。

1. **ソリューション エクスプ ローラー**を右クリックし、*後* ページ**コードの表示**します。
2. *ShoppingCart.aspx.cs*ファイルで、更新、`Page_Load`黄色で強調表示されている次のコードを追加することでハンドラー。   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

ときに、*後*ページが読み込まれると、呼び出すことによって、ショッピング カート合計を取得し、ショッピング カート オブジェクトの読み込み、`GetTotal`のメソッド、`ShoppingCart`クラス。 ショッピング カートの内容が空の場合は、それに対応する、メッセージが表示されます。

### <a name="testing-the-shopping-cart-total"></a>ショッピング カート合計は、のテスト

できましていないのみを追加する方法、製品、ショッピング カートを表示するアプリケーションを実行しますが、ショッピング カート合計を確認できます。

1. **F5** キーを押してアプリケーションを実行します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**自動車**カテゴリのナビゲーション メニュー。
3. をクリックして、 **Add To Cart**最初の製品の横にあるリンクです。   
 *後*ページには、注文の合計が表示されます。 

    ![ショッピング カートのカート合計](shopping-cart/_static/image7.png)
4. その他の製品 (たとえば、プレーン) をカートに追加します。
5. *後*ページに追加したすべての製品の更新後の合計が表示されます。 

    ![ショッピング カートの複数の製品](shopping-cart/_static/image8.png)
6. ブラウザー ウィンドウを閉じるには、実行中のアプリを停止します。

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>ショッピング カートへの更新プログラムとチェック アウト ボタンの追加

ユーザーがショッピング カートの内容を変更できるように、追加します、**更新**ボタンと**チェック アウト**ショッピング カート ページにボタンをクリックします。 **チェック アウト**ボタンは、このチュートリアル シリーズの後半になるまでは使用されません。

1. **ソリューション エクスプ ローラー**、オープン、*後*ページ、web アプリケーション プロジェクトのルートにします。
2. 追加する、 **Update**ボタンと**チェック アウト**ボタンを*後*ページのように、既存のマークアップを黄色で強調表示するマークアップを追加、次のコード:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

ユーザーがクリックすると、 **Update**  ボタン、`UpdateBtn_Click`イベント ハンドラーが呼び出されます。 このイベント ハンドラーでは、次の手順で追加するコードを呼び出します。

次に含まれるコードを更新することができます、 *ShoppingCart.aspx.cs*カートのアイテムと呼び出しをループするファイル、`RemoveItem`と`UpdateItem`メソッド。

1. **ソリューション エクスプ ローラー**、オープン、 *ShoppingCart.aspx.cs* web アプリケーション プロジェクトのルートにあるファイル。
2. 次のコード セクションに黄色で強調表示を追加、 *ShoppingCart.aspx.cs*ファイル。   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

ユーザーがクリックすると、**更新**のボタンでは、*後* ページで、UpdateCartItems メソッドが呼び出されます。 UpdateCartItems メソッドは、ショッピング カート内の各項目を更新する値を取得します。 UpdateCartItems メソッドを呼び出して、`UpdateShoppingCartDatabase`メソッド (追加され、次の手順で説明されている) に追加するか、ショッピング カートから項目を削除します。 ショッピング カートへの更新を反映するように、データベースが更新されたら、 **GridView**コントロールが呼び出すことによって、ショッピング カート ページの更新、`DataBind`のメソッド、 **GridView**します。 また、ショッピング カート ページの合計注文金額が更新された項目の一覧を反映するように更新されます。

### <a name="updating-and-removing-shopping-cart-items"></a>更新して、ショッピング カートのアイテムを削除します。

*後*ページに、品目の数量を更新して、アイテムを削除するためのコントロールが追加されて表示できます。 これらのコントロール動作を反映するコードを追加します。

1. **ソリューション エクスプ ローラー**、オープン、 *ShoppingCartActions.cs*ファイル、*ロジック*フォルダー。
2. 次のコードを黄色で強調表示を追加、 *ShoppingCartActions.cs*クラス ファイル。   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase`から呼び出されるメソッド、`UpdateCartItems`メソッドを*ShoppingCart.aspx.cs*  ページで、更新するか、ショッピング カートから項目を削除するためのロジックが含まれています。 `UpdateShoppingCartDatabase`メソッドは、ショッピング カート リスト内のすべての行を反復処理します。 ショッピング カートのアイテムが削除するには、マークされてまたは数量が 1 より小さい場合、`RemoveItem`メソッドが呼び出されます。 ショッピング カートのアイテムをチェックする場合は、更新するときに、`UpdateItem`メソッドが呼び出されます。 ショッピング カートのアイテムを削除または更新すると、データベースの変更は保存されます。

`ShoppingCartUpdates`構造がショッピング カートのアイテムを保持するために使用します。 `UpdateShoppingCartDatabase`メソッドは、`ShoppingCartUpdates`の構造をかどうか、項目のいずれかの更新または削除する必要があります。

次のチュートリアルでは使用して、`EmptyCart`製品を購入した後、ショッピングを消去するメソッドがカートします。 ここでは、使用するが、`GetCount`に追加したメソッド、 *ShoppingCartActions.cs*ショッピング カートにアイテムの数を確認するファイル。

### <a name="adding-a-shopping-cart-counter"></a>ショッピング カート カウンターの追加

ショッピング カートにアイテムの合計数を表示するユーザーを許可するのにカウンターを追加する、 *Site.Master*ページ。 このカウンターは、ショッピング カートへのリンクとしても機能します。

1. **ソリューション エクスプ ローラー**、オープン、 *Site.Master*ページ。
2. 次のように表示されるように、[ナビゲーション] セクションに黄色で示すように、ショッピング カート カウンターのリンクを追加することで、マークアップを変更します。  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. 分離コードを次に、更新、 *Site.Master.cs*ファイルは次のように黄色で強調表示のコードを追加します。  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

ページが HTML としてレンダリングされる前に、`Page_PreRender`イベントが発生します。 `Page_PreRender`ハンドラー、ショッピング カートの合計数は、呼び出すことによって決まりますが、`GetCount`メソッド。 返される値に追加、`cartCount`スパンのマークアップに含まれる、 *Site.Master*ページ。 `<span>`タグにより、内部の要素を適切に表示します。 サイトの任意のページが表示されたら、ショッピング カート合計が表示されます。 ユーザーは、ショッピング カート合計は、ショッピング カートの表示をクリックすることもします。

## <a name="testing-the-completed-shopping-cart"></a>完了したショッピング カートのテスト

ショッピング カートでは、アプリケーションを追加する方法を確認するようになりました、delete、および更新プログラムを実行できます。 ショッピング カート合計は、ショッピング カート内のすべての項目の合計コストに反映されます。

1. **F5** キーを押してアプリケーションを実行します。  
 ブラウザーが開きを示しています、 *Default.aspx*ページ。
2. 選択**自動車**カテゴリのナビゲーション メニュー。
3. をクリックして、 **Add To Cart**最初の製品の横にあるリンクです。   
 *後*ページには、注文の合計が表示されます。
4. 選択**平面**カテゴリのナビゲーション メニュー。
5. をクリックして、 **Add To Cart**最初の製品の横にあるリンクです。
6. 3 をショッピング カート内の最初の項目の数量を設定し、選択、**項目の削除**2 番目の項目のチェック ボックス。<a id="a"></a>
7. をクリックして、**更新**ショッピング カート ページを更新および新しい注文の合計を表示するボタンをクリックします。 

    ![ショッピング カートのカートの更新](shopping-cart/_static/image9.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、Wingtip Toys の Web フォームのサンプル アプリケーションのショッピング カートを作成しました。 このチュートリアルでは、Entity Framework Code First、データ注釈厳密に型指定されたデータ コントロール、およびモデルのバインディングを使用しています。

ショッピング カートには、追加、削除、およびご購入いただいた場合、ユーザーが選択した項目の更新がサポートしています。 ショッピング カートの機能を実装する以外には、ショッピング カートのアイテムを表示する方法を説明しました、 **GridView**を制御し、注文の合計を計算します。

## <a name="addition-information"></a>詳細については

[ASP.NET セッション状態の概要](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [前へ](display_data_items_and_details.md)
> [次へ](checkout-and-payment-with-paypal.md)
