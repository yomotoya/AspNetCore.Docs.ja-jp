---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: データの表示項目、詳細 |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892570"
---
<a name="display-data-items-and-details"></a>データの表示項目、詳細
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、データ項目と ASP.NET Web フォームと Entity Framework Code First を使用してデータ アイテムの詳細を表示する方法について説明します。 このチュートリアルでは、前のチュートリアル」UI とナビゲーション「上に構築され、Wingtip Toy ストア チュートリアル シリーズの一部です。 このチュートリアルを完了したことができますに製品を表示する、 *ProductsList.aspx*ページおよびで個々 の製品に関する詳細、 *ProductDetails.aspx*ページ。

## <a name="what-youll-learn"></a>学習する内容。

- データベースから製品を表示するデータ コントロールを追加する方法。
- データ コントロールを選択したデータに接続する方法です。
- データベースから製品詳細を表示するデータ コントロールを追加する方法。
- クエリ文字列から値を取得し、その値を使用して、データベースから取得したデータを制限する方法。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>これらは、このチュートリアルで導入された機能です。

- モデル バインド
- 値プロバイダー

## <a name="adding-a-data-control-to-display-products"></a>製品を表示するデータ コントロールを追加します。

サーバー コントロールにデータをバインドするときに使用できるいくつかの異なるオプションがあります。 データ ソース コントロールの追加、コードを手動で追加すること、またはモデル バインディングを使用して、最も一般的なオプションが含まれます。

### <a name="using-a-data-source-control-to-bind-data"></a>データ ソース コントロールを使用してデータをバインドするには

データ ソース コントロールを追加するには、データを表示するコントロールをデータ ソース コントロールをリンクすることができます。 この方法では、宣言によってサーバー側のコントロールをプログラム的なアプローチを使用するのではなく、データ ソースに直接接続することができます。

### <a name="coding-by-hand-to-bind-data"></a>データ バインドを手動でコーディング

手動でコードを追加する値を読み取るの null 値を確認、適切な型に変換しようと、変換が成功したかどうかをチェックおよび最後に、クエリで値を使用します。 データ アクセス ロジックを完全に制御を保持する必要がある場合は、このアプローチを使用します。

### <a name="using-model-binding-to-bind-data"></a>モデルのデータをバインドするバインディングを使用します。

モデル バインディングを使用して、はるかに少ないコードを使用して結果をバインドすることができ、使用すると、アプリケーション全体の機能を再利用できます。 モデル バインディングの目的は、データ バインディングの豊富なフレームワークの利点を活かしながらコード中心のデータ アクセス ロジックの操作が簡単です。

## <a name="displaying-products"></a>製品を表示します。

このチュートリアルで使用するモデル バインド データをバインドします。 バインディングを使用してモデル データを選択するデータ コントロールを構成するには、コントロールを設定`SelectMethod`プロパティ ページのコード内のメソッドの名前にします。 データ コントロールは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。 明示的に呼び出す必要はありません、`DataBind`メソッドです。

次の手順を使用して、変更してみます内のマークアップ、 *ProductList.aspx*ページのページは、製品を表示できるようにします。

1. **ソリューション エクスプ ローラー**を開き、 *ProductList.aspx*ページ。
2. 既存のマークアップを次のマークアップに置き換えます。   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

このコードを使用して、 **ListView** "productList"をという名前の製品を表示するコントロール。

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView**コントロールは、テンプレートとスタイルを使用して定義された形式でデータを表示します。 任意の繰り返し構造内のデータに便利です。 これは、 **ListView**例が、データベース、ただしユーザーを編集、挿入、およびデータを削除および並べ替えを有効にしてコードがないすべてのページのデータからデータを表示します。

設定して、`ItemType`プロパティに、 **ListView**制御、データ バインディング式`Item`が利用可能なコントロールが厳密に型指定されたとします。 必要に応じて、前のチュートリアルで説明したようにを指定するなど、IntelliSense を使用してアイテム オブジェクトの詳細を選択、 `ProductName`:

![データを表示する項目と IntelliSense の詳細。](display_data_items_and_details/_static/image1.png)

さらに、モデル バインディングを使用して指定するが、`SelectMethod`値。 この値 (`GetProducts`) は、次の手順で製品を表示する分離コードを追加するメソッドに対応します。

### <a name="adding-code-to-display-products"></a>製品を表示するコードを追加します。

この手順でデータを読み込むコードを追加、 **ListView**データベースから製品データを制御します。 コードでは、個々 のカテゴリだけでなくすべての製品を表示して示す製品をサポートします。

1. **ソリューション エクスプ ローラー**を右クリックして*ProductList.aspx*  をクリックし、**コードの表示**です。
2. 既存のコードを置き換える、 *ProductList.aspx.cs*を次のコード ファイル。   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

このコードを示しています、`GetProducts`によって参照されるメソッド、`ItemType`のプロパティ、 **ListView**内の制御、 *ProductList.aspx*ページ。 データベースの特定のカテゴリに結果を制限するため、設定するコードを`categoryId`に渡されるクエリ文字列の値から値、 *ProductList.aspx*ページ、 *ProductList.aspx*ページ移動します。 `QueryStringAttribute`クラス内で、`System.Web.ModelBinding`名前空間を使用して、クエリ文字列変数 id の値を取得します。これに指示するクエリ文字列から値をバインドしようとするモデル バインド、`categoryId`実行時にパラメーター。

クエリの結果が一致する、データベース内のこれらの製品に制限されてページにクエリ文字列として有効なカテゴリが渡されると、`categoryId`値。 インスタンスの場合、URL、 *ProductsList.aspx*ページは、次。

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

ページには、製品のみが表示されます。 ここで、 `category` equals`1`です。

クエリ文字列が含まれていないに移動するとき場合、 *ProductList.aspx*  ページで、すべての製品が表示されます。

これらのメソッドの値のソースと呼びます*値プロバイダー* (など*QueryString*) とを使用する値プロバイダーを示すパラメーターの属性値として参照されますプロバイダー属性 (たとえば"`id`") です。 ASP.NET には、クエリ文字列、cookie、フォームの値、コントロール、ビュー状態、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションでの値プロバイダーとすべてのユーザー入力の典型的なソースの対応する属性が含まれています。 カスタム値プロバイダーを記述することもできます。

### <a name="running-the-application"></a>アプリケーションの実行

すべての製品または製品カテゴリによって制限のセットだけを表示する方法を表示するアプリケーションを実行します。

1. **ソリューション エクスプ ローラー**を右クリックし、 *Default.aspx*ページし、選択**ブラウザーで表示**です。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. 選択**車**ナビゲーション メニューの製品カテゴリ。  
 *ProductList.aspx* 「自動車」カテゴリに含まれる製品のみが表示されたページが表示されます。 このチュートリアルの後半では、製品の詳細が表示されます。  

    ![データを表示する項目と車の詳細。](display_data_items_and_details/_static/image2.png)
3. 選択**製品**上部にあるナビゲーション メニュー。  
 もう一度、 *ProductList.aspx*ページが表示されますが、今度は製品の完全なリストが表示されます。   

    ![データを表示する項目と詳細 - 製品](display_data_items_and_details/_static/image3.png)
4. ブラウザーを閉じて、Visual Studio に戻ります。

### <a name="adding-a-data-control-to-display-product-details"></a>製品の詳細を表示するデータ コントロールを追加します。

次に、内のマークアップを変更、 *ProductDetails.aspx*ページは、個々 の製品についての情報を表示できるように、前のチュートリアルで追加するページ。

1. **ソリューション エクスプ ローラー**を開き、 *ProductDetails.aspx*ページ。
2. 既存のマークアップを次のマークアップに置き換えます。   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

このコードを使用して、 **FormView**個々 の製品に関する詳細を表示するコントロール。 このマークアップは、データを表示するために使用するようなメソッドを使用して、 *ProductList.aspx*ページ。 **FormView**コントロールを使用して、データ ソースから一度に 1 つのレコードを表示します。 使用すると、 **FormView**コントロール、表示およびデータ バインドされた値を編集するテンプレートを作成します。 テンプレートには、バインディング式、および書式設定を定義するコントロール外観とフォームの機能にはが含まれます。

上記のマークアップをデータベースに接続するには、する追加のコードを追加する必要があります、 *ProductDetails.aspx*コード。

1. **ソリューション エクスプ ローラー**を右クリックして*ProductDetails.aspx*  をクリックし、**コードの表示**です。  
   *ProductDetails.aspx.cs*ファイルが表示されます。
2. 既存のコードを次のコードで置換します。   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

このコードをチェックする"`productID`"クエリ文字列値です。 有効なクエリ文字列値が見つかった場合は、一致する製品が表示されます。 クエリ文字列が見つからない、またはクエリ文字列値が無効、製品が表示されない、 *ProductDetails.aspx*ページ。

### <a name="running-the-application"></a>アプリケーションの実行

これで、表示される個々 の製品を表示するアプリケーションを実行することができます、製品の id に基づいています。

1. キーを押して**f5 キーを押して**Visual Studio でアプリケーションを実行中です。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. ナビゲーション メニューのカテゴリには、「ボート」を選択します。  
 *ProductList.aspx*ページが表示されます。
3. 製品の一覧から「用紙ボート」製品を選択します。  
 *ProductDetails.aspx*ページが表示されます。   

    ![データを表示する項目と詳細 - 製品](display_data_items_and_details/_static/image4.png)
4. ブラウザーを閉じます。

## <a name="summary"></a>まとめ

系列には、このチュートリアルでは、マークアップと製品一覧を表示して、製品の詳細を表示するコードを追加がします。 このプロセス中には、厳密に型指定されたデータ コントロール、モデル バインディング、および値プロバイダーについて学習しました。 チュートリアルでは、[次へ]、ショッピング カートを Wingtip Toys のサンプル アプリケーションに追加します。

## <a name="additional-resources"></a>その他のリソース

[取得して、モデル バインディング機能と web フォームでデータの表示](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [前へ](ui_and_navigation.md)
> [次へ](shopping-cart.md)
