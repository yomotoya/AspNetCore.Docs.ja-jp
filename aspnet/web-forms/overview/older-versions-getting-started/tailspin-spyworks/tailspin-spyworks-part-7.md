---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: "手順 7: 機能の追加 |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、アカウント revie などの追加機能を追加しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 5280de44b3e75f9d1ae85e0248bc3ef6d5444f6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-7-adding-features"></a>手順 7: 追加の機能
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、アカウントの確認、製品レビュー、および「人気のある項目」および「も購入済み」のユーザー コントロールなどの追加機能を追加します。


## <a id="_Toc260221673"></a>機能の追加

ユーザーは、弊社のカタログを参照できる、ショッピング カートにアイテムを配置とチェック アウト プロセスを完了、数をサポートする機能のサイトを向上させることが含まれているがあります。

1. アカウントの確認 (一覧からの注文が配置され、詳細を表示します。)
2. 最初のページがいくつかのコンテキストの特定のコンテンツを追加します。
3. カタログにユーザーがレビューできるように、機能、製品を追加します。
4. 表示する人気のある項目の場所を制御する最初のページのユーザー コントロールを作成します。
5. 「購入も」ユーザー コントロールを作成し、製品の詳細ページに追加します。
6. 連絡先を追加するページ。
7. 追加、ページに関するします。
8. グローバル エラー

## <a id="_Toc260221674"></a>アカウントの確認

「アカウント」フォルダーに 2 つの .aspx ページ 1 つの名前付き OrderList.aspx およびその他の名前付き OrderDetails.aspx を作成します

ここがある以前と同様、OrderList.aspx は、GridView と EntityDataSoure コントロールを活用します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure ユーザー名でフィルター処理、Orders テーブルからレコードを選択する (、WhereParameter を参照してください) のユーザーのログオン時にセッション変数に設定します。

GridView の内でこれらのパラメーターにも注意してください。

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

これらの OrderDetails.aspx ページへのクエリ文字列パラメーターとして、OrderID フィールドを指定する各製品の注文の詳細ビューへのリンクを指定します。

## <a id="_Toc260221675"></a>OrderDetails.aspx

EntityDataSource コントロールへのアクセス、注文と、FormView 注文データと、GridView と別の EntityDataSource を表示するのにすべての注文の品目を表示するのに使用します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

分離コード ファイル (OrderDetails.aspx.cs) では、ハウスキーピングのわずか 2 つのビットがあります。

まず OrderDetails が常に、OrderId を取得するかどうかを確認する必要があります。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

また、計算され、注文の品目の合計を表示する必要があります。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>ホーム ページ

Default.aspx ページには、いくつかの静的なコンテンツを追加してみましょう。

まず「コンテンツ」フォルダーを作成し、その中 Images フォルダー (およびホーム ページで使用する画像を追加します)。

Default.aspx ページの下部のプレース ホルダーには、次のマークアップを追加します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>製品のレビュー

まず、リンク ボタン追加、製品のレビューの入力に使用できる形式にします。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

クエリ文字列に、ProductID を渡していることに注意してください。

[次へ] ReviewAdd.aspx をという名前のページを追加してみましょう。

このページでは、ASP.NET AJAX コントロール ツールキットを使用します。 かどうかをまだ行っていないからダウンロードすることができますので[DevExpress](http://devexpress.com/act) 、toolkit for Visual Studio での使用は、ここを設定する方法のガイダンスがおよび[https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

デザイン モードでは、コントロールと検証コントロールをツールボックスからドラッグし、以下のようなフォームを作成します。

![](tailspin-spyworks-part-7/_static/image2.jpg)

マークアップは、次のようになります。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

レビューを入力するには、これでは、製品 ページでこれらのレビューを表示することができます。

このマークアップを ProductDetails.aspx ページに追加します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

これで、アプリケーションを実行していると、製品への移動は、顧客レビューなどの製品情報を示します。

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>人気のある項目のコントロール (ユーザー コントロールの作成)

Web サイトの売上を増やすためには、「挑発的な販売」人気のある、または関連する製品に、いくつかの機能を追加します。

これらの機能の最初の製品カタログでよく使用される製品の一覧表示されます。

好調、アプリケーションのホーム ページ上の項目を表示する「ユーザー コントロール」を作成します。 これはなるため、コントロールは、単にドラッグすると同様に任意のページ上に Visual Studio のデザイナーでコントロールを削除する任意のページに使用したことができます。

Visual Studio のソリューション エクスプ ローラーで、ソリューション名を右クリックし、「コントロール」という名前の新しいディレクトリを作成します。 そのためには必要ありませんが、お手伝い「コントロール」ディレクトリすべてのユーザー コントロールを作成して整理されたプロジェクトを保持します。

コントロールのフォルダーを右クリックし、「新しい項目の」を選択します。

![](tailspin-spyworks-part-7/_static/image4.jpg)

"PopularItems"のユーザーのコントロールの名前を指定します。 ユーザー コントロールのファイル拡張子が .ascx .aspx できませんに注意してください。

ユーザーの一般的なアイテムのユーザー コントロールは、次のように定義されます。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

ここでこのアプリケーションでまだお使用いないメソッドを使用しているおです。 リピータ コントロールを使用して、データ ソース コントロールを使用する代わりに、LINQ to Entities クエリの結果を Repeater コントロールをバインドしているおです。

ユーザーのコントロールの分離コードでそうには、次のようにします。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

ユーザーのコントロールのマークアップの上部にある重要な行を注意してください。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

分を分単位で最も人気のある項目を変更しないため、アプリケーションのパフォーマンスを向上させるために出るディレクティブを追加できます。 このディレクティブは、コントロールのキャッシュされた出力の有効期限が切れるときにのみ実行するコントロールのコードになります。 それ以外の場合は、コントロールの出力のキャッシュされたバージョンが使用されます。

やることを行うには、Default.aspc ページで、新しいコントロールが含まれます。

ドラッグ アンド ドロップで、既定のフォームの開いている列に、コントロールのインスタンスを配置します。

![](tailspin-spyworks-part-7/_static/image5.jpg)

今すぐ実行するとき、アプリケーションのホーム ページには、最も人気のある項目が表示されます。

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>(パラメーターを使用してユーザー コントロール) を管理「も購入」

コンテキストの特異性を追加することで、次のレベルを販売して推奨を作成した 2 つ目のユーザー コントロールになります。

上位の「購入も」アイテムを計算するためのロジックは、非単純です。

「購入も」コントロールは、現在選択されている ProductID の (以前の購入) OrderDetails レコードを選択しが見つかった一意の注文ごと order Id を取得します。

おは al から製品を選択これらすべての注文と合計数量を購入します。 製品を数量合計に並べ替えるし、上位 5 つのアイテムを表示おします。

このロジックの複雑さを考えると、ストアド プロシージャとして、このアルゴリズムを実装します。

ストアド プロシージャの T-SQL では次のとおりです。

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

このストアド プロシージャ (SelectPurchasedWithProducts) 存在していたこと、データベースには含まれている場合、アプリケーションを指定したテーブルと、必要な Entity Data Model ビューだけでなく、Entity Data Model を生成したときに注意してください。このストアド プロシージャを含める必要があります。

エンティティ データ モデルからストアド プロシージャにアクセスするには、関数をインポートする必要があります。

Entity Data Model デザイナーで開くし、モデル ブラウザーを開き、ソリューション エクスプ ローラーでをダブルクリックし、デザイナー内を右クリックして「関数インポートの追加」を選択します。

![](tailspin-spyworks-part-7/_static/image1.png)

そうと、このダイアログ ボックスが開きます。

![](tailspin-spyworks-part-7/_static/image2.png)

"SelectPurchasedWithProducts"を選択すると、表示されるフィールドに入力し、インポートされた関数の名前のプロシージャ名を使用します。

"Ok"をクリックします。

おが単に、プログラム、ストアド プロシージャかもしれません、モデル内の他のアイテムとしてこの手順を完了します。

そのため、「コントロール」フォルダーでは、AlsoPurchased.ascx をという名前の新しいユーザー コントロールを作成します。

このコントロールのマークアップは PopularItems コントロールに非常に使いやすくなります。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

重要な違いは、出力キャッシュしないことと、項目の表示するのには製品でによって異なるためです。

ProductId をコントロールに"property"となります。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

コントロールの PreRender イベント ハンドラーでは 3 つの作業を行う eed です。

1. ProductID が設定されていることを確認してください。
2. すべての製品購入された現在のものがあるかどうかに参照してください。
3. いくつかのアイテム 2 で決まるを出力します。

簡単に方法をモデルからストアド プロシージャを呼び出すことに注意してください。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

あります「も購入される」を決定した後だけでリピータ クエリによって返される結果にバインドできます。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

「購入も」項目がない場合だけ表示人気のあるその他の項目、カタログから。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

「購入も」アイテムを表示するには、ProductDetails.aspx ページを開き、マークアップ内のこの位置に表示されるように、ソリューション エクスプ ローラーから AlsoPurchased コントロールをドラッグします。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

これにより、ProductDetails ページの上部にあるコントロールへの参照が作成されます。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

AlsoPurchased ユーザー コントロールには、ProductId 番号が必要です。 ため ProductID プロパティ、コントロールのページの現在のデータ モデルのアイテムに対して Eval のステートメントを使用して設定されます。

![](tailspin-spyworks-part-7/_static/image3.png)

今すぐ実行し、製品を参照してビルドときに、「購入も」アイテムを確認します。

![](tailspin-spyworks-part-7/_static/image7.jpg)

>[!div class="step-by-step"]
[前へ](tailspin-spyworks-part-6.md)
[次へ](tailspin-spyworks-part-8.md)
