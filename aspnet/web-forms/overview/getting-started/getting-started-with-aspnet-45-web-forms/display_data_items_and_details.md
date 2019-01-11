---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 項目し、詳細のデータの表示 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズが Web 用の ASP.NET 4.7 および Microsoft Visual Studio Community 2017 に ASP.NET Web フォーム アプリケーションの構築の基礎を講義します。
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207435"
---
<a name="display-data-items-and-details"></a>データ アイテムの表示と詳細
====================
によって[Erik Reitan](https://github.com/Erikre)

> このチュートリアル シリーズでは、Web 用の ASP.NET 4.7 および Microsoft Visual Studio Community 2017 に ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。

このチュートリアルでは、データ項目と ASP.NET Web フォームと Entity Framework Code First でのデータ項目の詳細を表示する方法について説明します。 このチュートリアルでは、Wingtip 玩具店のチュートリアル シリーズの一部として、前の「UI とナビゲーション」チュートリアルに基づいています。 完了したチュートリアルでは、上の製品で、 *ProductsList.aspx*ページと、製品の詳細について、 *ProductDetails.aspx*ページが表示されます。

## <a name="what-you-learn"></a>学習内容

- データベース製品を表示するデータ コントロールを追加します。
- データ コントロールを選択したデータに接続します。
- 製品の詳細を表示するデータ コントロールを追加します。
- クエリ文字列の値を解析し、取得したデータベースのデータをフィルター処理に使用します。

このチュートリアルで導入された機能には、モデル バインドと値のプロバイダーが含まれます。

## <a name="add-a-data-control-to-display-products"></a>製品を表示するデータ コントロールを追加します。
 
サーバー コントロールにデータをバインドするいくつかのオプションがあります。 最も一般的なのとおりです。

 * データ ソース コントロールの追加
 * コードを手動で追加します。
 * モデル バインドを実装します。

### <a name="use-a-data-source-control-to-bind-data"></a>データ ソース コントロールを使用してデータをバインドするには

データ ソース コントロールの追加データを表示するコントロールにデータ ソース コントロールをリンクします。 この方法により、ここでは、代わりにできますプログラムでは、サーバー側コントロールをデータ ソースに接続します。

### <a name="code-by-hand-to-bind-data"></a>データをバインドするには、手動でコード

手動でコーディングが含まれます。

1. 値の読み取り
2. かどうかは null をチェックします。
3. 適切な型に変換します。
4. 変換成功の確認
5. 変換後の値を持つクエリを作成します。 

この方法で、データ アクセス ロジックを完全に制御があります。

### <a name="use-model-binding-to-bind-data"></a>モデル バインドを使用してデータをバインドするには

モデル バインディングではるかに少ないコードで結果を連結して、使用すると、アプリケーション全体で機能を再利用できます。 豊富なデータ バインディング フレームワークを提供しながらコード中心のデータ アクセス ロジックと作業が簡略化します。

## <a name="display-products"></a>製品を表示します。

このチュートリアルでは、モデル バインドを使用してデータをバインドします。 モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する`SelectMethod`プロパティ ページのコード内のメソッドにします。 データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。 明示的に呼び出す必要はありません、`DataBind`メソッド。

変更する、次の手順を通して*ProductList.aspx*製品を表示するマークアップ。

1. **ソリューション エクスプ ローラー**オープン*ProductList.aspx*します。

2. 既存のマークアップを次のマークアップに置き換えます。 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

上記のマークアップを使用して、 **ListView**という名前のコントロール`productList`商品を表示します。

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

テンプレートとスタイルを使用して定義する方法、 **ListView**コントロールにデータが表示されます。 任意の繰り返し構造内のデータと便利です。 ただしこの**ListView**の例には、データベースのデータだけが表示されます、編集、挿入、およびデータを削除して、並べ替えやデータをページにユーザーを有効にするコードがないこともできます。

設定すると、`ItemType`プロパティ、 **ListView**制御、データ バインディング式`Item`が使用可能なコントロールが厳密に型指定されました。 指定するなど、IntelliSense 項目オブジェクトの詳細を選択するには前のチュートリアルで前述の`ProductName`:

![データを表示する項目と IntelliSense の詳細](display_data_items_and_details/_static/image1.png)

モデル バインディングで指定する、`SelectMethod`値 (`GetProducts`)。 これは、コードに追加する方法を次の手順で製品を表示する遅れています。

### <a name="add-code-to-display-products"></a>製品を表示するコードを追加します。

この手順では、データを読み込むコードを追加、 **ListView**データベース製品のデータを制御します。 コードでは、すべての製品と個々 のカテゴリの製品を表示をサポートします。

1. **ソリューション エクスプ ローラー**を右クリックして*ProductList.aspx*選び**コードの表示**します。
2. 既存のコードを置き換える、 *ProductList.aspx.cs*このファイル。   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

このコードを示しています、`GetProducts`メソッドを**ListView**コントロールの`ItemType`内のプロパティ参照*ProductList.aspx*します。 コードの設定を特定のデータベース カテゴリに結果を制限するため、`categoryId`に渡されるクエリ文字列の値を*ProductList.aspx*します。 `QueryStringAttribute`クラス、`System.Web.ModelBinding`名前空間を使用して、クエリ文字列変数を取得`id`の値します。 これにより、実行時に、クエリ文字列値をバインドするモデル バインド、`categoryId`パラメーター。

ときに有効なカテゴリ (`categoryId`) が渡されると、結果はそのカテゴリのデータベース製品に制限されています。 たとえば場合、 *ProductsList.aspx*はページの URL:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

ページには、製品のみが表示されます。 場所、 `categoryId` equals`1`します。

クエリ文字列が渡されない場合は、すべての製品が表示されます。

これらのメソッドの値のソースと呼びます*値プロバイダー* (など`QueryString`)、および使用する値プロバイダーを示すパラメーターの属性と呼びます*プロバイダー属性の値*(など`id`)。 ASP.NET には、値プロバイダーとのすべての一般的な Web フォーム アプリケーションのユーザー入力ソースの属性が含まれています。 クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティが含まれます。 カスタム値プロバイダーを記述することもできます。

### <a name="run-the-application"></a>アプリケーションの実行

すべての製品またはカテゴリの製品を表示するアプリケーションを実行します。

1. Visual Studio で、キーを押して**F5**アプリケーションを実行します。
 ブラウザーが開きを示しています、 *Default.aspx*ページ。

2. 製品カテゴリ メニューの**自動車**します。

   *ProductList.aspx*から製品のみが表示されたページが表示されます、**自動車**カテゴリ。 このチュートリアルの後半では、製品の詳細を表示します。

    ![データを表示する項目と詳細 - 自動車](display_data_items_and_details/_static/image2.png)

3. 選択**製品**上部のメニューから。
 *ProductList.aspx*ページには、すべての製品が表示されます。 

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image3.png)

4. ブラウザーを閉じて、Visual Studio に戻ります。

### <a name="add-a-data-control-to-display-product-details"></a>製品の詳細を表示するデータ コントロールを追加します。

変更、 *ProductDetails.aspx*特定の製品情報を表示する前のチュートリアルで追加したマークアップ。

1. **ソリューション エクスプ ローラー**オープン*ProductDetails.aspx*します。

2. このマークアップで既存のマークアップに置き換えます。

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

このマークアップを使用して、 **FormView**特定の製品の詳細を表示するコントロール。 データを表示するのに使用されるものなどのメソッドを使用して*ProductList.aspx*します。 **FormView**コントロールを使用すると、データ ソースから一度に 1 つのレコードを表示します。 使用すると、 **FormView**コントロール、データ バインドされた値を表示および編集テンプレートを作成します。 これらのテンプレートは、バインド式のコントロールを含めるし、フォームの外観と機能を定義する書式設定します。

前のマークアップをデータベースに接続するには、コードを追加する必要があります。

1. **ソリューション エクスプ ローラー**を右クリックして*ProductDetails.aspx*選び**コードの表示**します。  
   *ProductDetails.aspx.cs*ファイルが表示されます。

2. これで、既存のコードに置き換えます。   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

このコードをチェックする"`productID`"クエリ文字列の値。 有効な値が見つかった場合は、一致する製品が表示されます。 クエリ文字列が見つからない、またはその値が無効です、製品は表示されません。

### <a name="run-the-application"></a>アプリケーションの実行

製品 ID に基づいて特定の製品の詳細を表示するアプリケーションを実行するようになりました

1. Visual Studio で、キーを押して**F5**アプリケーションを実行します。  
 ブラウザーが開きます*Default.aspx*します。

2. [カテゴリ] メニューから選択**ボート**します。  
 *ProductList.aspx*ページが表示されます。

3. 選択**ボートを紙**します。  
 *ProductDetails.aspx*ページが表示されます。   

    ![データを表示する項目と製品の詳細](display_data_items_and_details/_static/image4.png)

次のチュートリアルでは、ショッピング カートを Wingtip Toys アプリケーションに追加します。

## <a name="additional-resources"></a>その他の技術情報

[取得して、モデル バインディングと web フォームでデータの表示](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [前へ](ui_and_navigation.md)
> [次へ](shopping-cart.md)
