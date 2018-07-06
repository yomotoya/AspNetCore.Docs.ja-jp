---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 項目し、詳細のデータの表示 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 083f7182416012c85f05db255fcab4d8e535b52a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820348"
---
<a name="display-data-items-and-details"></a>項目し、詳細のデータを表示
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、データ項目と ASP.NET Web フォームと Entity Framework Code First を使用してデータ項目の詳細を表示する方法について説明します。 このチュートリアルでは、前のチュートリアルでは、「UI とナビゲーション」で作成し、、Wingtip 玩具店のチュートリアル シリーズの一部です。 このチュートリアルを完了すると、ときにの製品を表示するができます、 *ProductsList.aspx*ページとに個別の製品に関する情報を*ProductDetails.aspx*ページ。

## <a name="what-youll-learn"></a>学習内容。

- データベースから製品を表示するデータ コントロールを追加する方法。
- データ コントロールを選択したデータに接続する方法。
- データベースから製品の詳細を表示するデータ コントロールを追加する方法。
- クエリ文字列から値を取得し、その値を使用して、データベースから取得したデータを制限する方法。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>このチュートリアルで導入された機能を次に示します。

- モデル バインド
- 値プロバイダー

## <a name="adding-a-data-control-to-display-products"></a>製品を表示するデータ コントロールを追加します。

サーバー コントロールにデータをバインドするときに使用できるいくつかの異なるオプションがあります。 データ ソース コントロールの追加、コードを手動で追加またはモデルのバインディングを使用して、最も一般的なオプションが含まれます。

### <a name="using-a-data-source-control-to-bind-data"></a>データ ソース コントロールを使用してデータをバインドするには

データ ソース コントロールの追加データを表示するコントロールにデータ ソース コントロールをリンクすることができます。 このアプローチでは、宣言によってサーバー側コントロールをプログラム的なアプローチを使用するのではなく、データ ソースに直接接続できます。

### <a name="coding-by-hand-to-bind-data"></a>データ バインドを手動でコーディング

追加を手作業でコードが含まれて値を読み取るの null 値の確認、適切な型に変換しようとして、変換が成功したかどうかをチェックおよびクエリの最後に、値を使用しています。 データ アクセス ロジックを完全に制御を保持する必要がある場合は、このアプローチを使用します。

### <a name="using-model-binding-to-bind-data"></a>モデルのデータをバインドするバインドを使用します。

モデル バインドを使用して、はるかに少ないコードを使用して結果をバインドすることができます、使用すると、アプリケーション全体で機能を再利用できます。 モデル バインドの豊富なデータ バインディング フレームワークの利点を維持しながらコードを重視したデータ アクセス ロジックの使用を簡略化する目的です。

## <a name="displaying-products"></a>製品を表示します。

このチュートリアルでは、データをバインドするのにモデル バインドを使用します。 モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する`SelectMethod`プロパティ ページのコード内のメソッドの名前にします。 データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。 明示的に呼び出す必要はありません、`DataBind`メソッド。

次の手順を使用して、内のマークアップを変更します、 *ProductList.aspx*ページ、ページは、製品を表示できるようにします。

1. **ソリューション エクスプ ローラー**、オープン、 *ProductList.aspx*ページ。
2. 既存のマークアップを次のマークアップに置き換えます。   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

このコードを使用して、 **ListView**製品を表示する「[productlist]」という名前のコントロール。

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView**コントロール テンプレートおよびスタイルを使用して定義した形式でデータを表示します。 任意の繰り返し構造内のデータと便利です。 これは、 **ListView**例には、データベース、ユーザーを編集、挿入、およびデータを削除し、並べ替えを有効にしたただしおよびコードなしですべてのページのデータからデータを単には示しています。

設定して、`ItemType`プロパティ、 **ListView**制御、データ バインディング式`Item`が使用可能なコントロールが厳密に型指定されました。 前のチュートリアルで説明したように、詳細を指定するなど、IntelliSense を使用してアイテム オブジェクトの選択もできます、 `ProductName`:

![データを表示する項目と IntelliSense の詳細](display_data_items_and_details/_static/image1.png)

さらに、モデル バインドを使用して指定するが、`SelectMethod`値。 この値 (`GetProducts`) は、次の手順に製品を表示する分離コードを追加するメソッドに対応します。

### <a name="adding-code-to-display-products"></a>製品を表示するコードを追加します。

この手順では、データを読み込むコードを追加します、 **ListView**コントロールに、データベースから製品データ。 コードでは、すべての製品を示すほか、個々 のカテゴリで表示されている製品をサポートします。

1. **ソリューション エクスプ ローラー**、右クリック*ProductList.aspx*  をクリックし、**コードの表示**します。
2. 既存のコードを置き換える、 *ProductList.aspx.cs*を次のコード ファイル。   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

このコードを示しています、`GetProducts`によって参照されるメソッド、`ItemType`のプロパティ、 **ListView**を制御、 *ProductList.aspx*ページ。 コードの設定、データベース内の特定のカテゴリに結果を制限するため、`categoryId`に渡されるクエリ文字列値から、 *ProductList.aspx*ときにページ、 *ProductList.aspx*ページは、移動します。 `QueryStringAttribute`クラス、`System.Web.ModelBinding`名前空間を使用して、クエリ文字列変数 id の値を取得します。これに指示するクエリ文字列から値をバインドしようとするモデル バインド、`categoryId`実行時にパラメーター。

クエリの結果に一致するデータベース内のこれらの製品に制限されています ページにクエリ文字列として有効なカテゴリが渡される、`categoryId`値。 たとえば場合の URL、 *ProductsList.aspx*ページは、次。

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

ページには、製品のみが表示されます。 場所、 `category` equals`1`します。

クエリ文字列が含まれていないに移動すると場合、 *ProductList.aspx*  ページで、すべての製品が表示されます。

これらのメソッドの値のソースとして参照されます*値プロバイダー* (など*QueryString*)、使用する値プロバイダーを示すパラメーターの属性値として参照されますプロバイダーの属性 (など"`id`")。 ASP.NET には、クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションでの値プロバイダーとすべてのユーザー入力の一般的なソースの対応する属性が含まれています。 カスタム値プロバイダーを記述することもできます。

### <a name="running-the-application"></a>アプリケーションの実行

すべての製品または製品カテゴリによって制限のセットだけを表示する方法を表示するアプリケーションを実行します。

1. **ソリューション エクスプ ローラー**を右クリックし、 *Default.aspx*  ページ**ブラウザーで表示**します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**自動車**製品カテゴリのナビゲーション メニュー。  
 *ProductList.aspx* "Cars"カテゴリに含まれる製品のみを示すページが表示されます。 このチュートリアルで後で製品の詳細が表示されます。  

    ![データを表示する項目と詳細 - 自動車](display_data_items_and_details/_static/image2.png)
3. 選択**製品**上部にあるナビゲーション メニュー。  
 もう一度、 *ProductList.aspx*ページが表示されますが、今度は製品の全リストが表示されます。   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image3.png)
4. ブラウザーを閉じて、Visual Studio に戻ります。

### <a name="adding-a-data-control-to-display-product-details"></a>製品の詳細を表示するデータ コントロールを追加します。

内のマークアップを次に、変更を*ProductDetails.aspx*ページは、個々 の製品についての情報を表示できるように、前のチュートリアルで追加したページです。

1. **ソリューション エクスプ ローラー**、オープン、 *ProductDetails.aspx*ページ。
2. 既存のマークアップを次のマークアップに置き換えます。   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

このコードを使用して、 **FormView**個別の製品についての詳細を表示するコントロール。 このマークアップは、データを表示するために使用するようなメソッドを使用して、 *ProductList.aspx*ページ。 **FormView**コントロールを使用すると、データ ソースから一度に 1 つのレコードを表示します。 使用すると、 **FormView**コントロール、データ バインドされた値を表示および編集テンプレートを作成します。 テンプレートには、コントロールに、バインド式と書式設定をフォームの機能と外観を定義が含まれています。

上記のマークアップをデータベースに接続するにコードを追加する必要があります、 *ProductDetails.aspx*コード。

1. **ソリューション エクスプ ローラー**、右クリック*ProductDetails.aspx*  をクリックし、**コードの表示**します。  
   *ProductDetails.aspx.cs*ファイルが表示されます。
2. 既存のコードを次のコードで置換します。   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

このコードをチェックする"`productID`"クエリ文字列値。 有効なクエリ文字列値が見つかった場合は、一致する製品が表示されます。 製品が表示されない場合、クエリ文字列が見つからないか、クエリ文字列値が無効です、 *ProductDetails.aspx*ページ。

### <a name="running-the-application"></a>アプリケーションの実行

これでアプリケーションが表示される個別の製品を実行する、製品の id に基づいています。

1. キーを押して**F5** Visual Studio でアプリケーションを実行するときにします。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. カテゴリのナビゲーション メニューから「ボート」を選択します。  
 *ProductList.aspx*ページが表示されます。
3. 製品の一覧から「用紙ボート」製品を選択します。  
 *ProductDetails.aspx*ページが表示されます。   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image4.png)
4. ブラウザーを閉じます。

## <a name="summary"></a>まとめ

シリーズのこのチュートリアルでが、マークアップと製品一覧を表示して、製品の詳細を表示するコードを追加します。 このプロセス中には、厳密に型指定されたデータ コントロール、モデル バインド、および値プロバイダーについて説明しました。 次のチュートリアルでは、Wingtip Toys のサンプル アプリケーションをショッピング カートを追加します。

## <a name="additional-resources"></a>その他のリソース

[取得して、モデル バインディングと web フォームでデータの表示](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [前へ](ui_and_navigation.md)
> [次へ](shopping-cart.md)
