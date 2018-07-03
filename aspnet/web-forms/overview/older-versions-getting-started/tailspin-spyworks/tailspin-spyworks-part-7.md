---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'パート 7: 機能の追加 |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、アカウントの確認などの追加機能を追加しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389177"
---
<a name="part-7-adding-features"></a>パート 7: 機能の追加
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、アカウントの確認、製品レビュー、および「人気のある項目」および「も購入済み」のユーザー コントロールなどの追加機能を追加します。


## <a id="_Toc260221673"></a>  機能の追加

ユーザーが、カタログを参照できる、買い物かごにアイテムを配置、清算処理を完了ししましたが、サイトを向上させるために含まれるをサポートしている多数の機能があります。

1. アカウントの確認 (リストから注文が配置され、詳細を表示します。)
2. 最初のページには、いくつかのコンテキストの特定のコンテンツを追加します。
3. カタログ内のユーザーがレビューできるようにするための機能、製品を追加します。
4. 表紙を制御する人気のある項目と場所を表示するユーザー コントロールを作成します。
5. 「も購入した」ユーザー コントロールを作成し、製品の詳細ページに追加します。
6. 連絡先を追加するページ。
7. 追加、ページについて。
8. グローバル エラー

## <a id="_Toc260221674"></a>  アカウントの確認

「アカウント」フォルダーで 1 つの名前付き OrderList.aspx とその他の名前付きの OrderDetails.aspx の 2 つの .aspx ページを作成します

私たちがある以前と同様、OrderList.aspx は GridView と EntityDataSoure コントロールを活用します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure、ユーザー名でフィルター処理、Orders テーブルからレコードを選択します (WhereParameter を参照してください) をユーザー ログオンのときにセッション変数に設定します。

GridView の内でこれらのパラメーターにも注意してください。

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

これらの OrderDetails.aspx ページへのクエリ文字列パラメーターとして、OrderID フィールドを指定する各製品の注文の詳細ビューへのリンクを指定します。

## <a id="_Toc260221675"></a>  OrderDetails.aspx

EntityDataSource コントロールを注文と FormView、注文データと別の EntityDataSource による GridView を表示するのにすべての注文の品目を表示するのにへのアクセスに使用します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

分離コード ファイル (OrderDetails.aspx.cs) では、ハウスキーピングの 2 つの小さなビットがあります。

まず OrderDetails が常に、OrderId を取得するかどうかを確認する必要があります。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

また、計算され、注文の品目の合計を表示する必要があります。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  ホーム ページ

Default.aspx ページには、いくつかの静的コンテンツを追加してみましょう。

最初に「コンテンツ」フォルダーを作成しますその中で、Images フォルダー (およびホーム ページで使用されるイメージに含めます)。

Default.aspx ページの下部にあるプレース ホルダーには、次のマークアップを追加します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  製品のレビュー

最初に製品のレビューを入力するために使用できるフォームのリンク付きのボタンを追加します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

クエリ文字列で、ProductID を渡していることに注意してください。

[次へ] ReviewAdd.aspx という名前のページを追加してみましょう

このページでは、ASP.NET AJAX Control Toolkit を使用します。 かどうかを行っていないためからダウンロードできます[DevExpress](http://devexpress.com/act)ここで Visual Studio で使用するためのツールキットの設定に関するガイダンスがあると[ https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md)します。

デザイン モードでコントロールと検証コントロールをツールボックスからドラッグし、次のようにフォームを作成します。

![](tailspin-spyworks-part-7/_static/image2.jpg)

マークアップは、次のようになります。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

レビューを入力しましたが、これで、製品ページにこれらのレビューを表示することができます。

ProductDetails.aspx ページには、このマークアップを追加します。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

これで、アプリケーションを実行している製品に移動して、顧客レビューなどの製品情報を示します。

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  人気のある項目のコントロール (ユーザー コントロールを作成する)

Web サイトの売上を強化するためには、「推奨の販売」人気のあるまたは関連する製品に、いくつかの機能を追加します。

これらの機能の 1 つ目は、製品カタログで人気の製品の一覧になります。

最も売れているアプリケーションのホーム ページ上の項目を表示する「ユーザー コントロール」を作成します。 これは、コントロールであるが、ためには、ドラッグ アンドを気に入っている任意のページに、Visual Studio のデザイナーでコントロールを削除するだけで任意のページで使用できます。

Visual Studio のソリューション エクスプ ローラーでソリューション名を右クリックし、「コントロール」をという名前の新しいディレクトリを作成します。 これを行うには必要ありませんが、「コントロール」のディレクトリで、すべてのユーザー コントロールを作成して整理された、プロジェクトの継続的されます。

コントロールのフォルダーを右クリックし、"新しい項目 を選択します。

![](tailspin-spyworks-part-7/_static/image4.jpg)

"PopularItems"のユーザーのコントロールの名前を指定します。 ユーザー コントロールのファイル拡張子が .ascx .aspx できませんに注意してください。

人気のある項目のユーザー コントロールは次のように定義されます。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

ここでこのアプリケーションでまだ使用がないメソッドを使用しています。 Repeater コントロールを使用していることとデータ ソース コントロールを使用する代わりに、LINQ to Entities クエリの結果を Repeater コントロールをバインドしているいます。

コントロールの分離コードのようなこととしては、次のようにします。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

また、コントロールのマークアップの上部にある重要な行に注意してください。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

分単位で最も人気のある項目を変更しないため、アプリケーションのパフォーマンスを向上させるために出るディレクティブを追加できます。 このディレクティブは、コントロールのキャッシュされた出力の有効期限が切れる場合に、実行されるのみコントロール コードになります。 それ以外の場合は、コントロールの出力のキャッシュされたバージョンが使用されます。

次に行う必要があるは、Default.aspc ページで、新しいコントロールを含めます。

ドラッグ アンド ドロップで、既定のフォームのオープンの列に、コントロールのインスタンスを配置します。

![](tailspin-spyworks-part-7/_static/image5.jpg)

これで、アプリケーションの実行ときに、ホーム ページには、最も人気のある項目が表示されます。

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  (パラメーターを持つユーザー コントロール) を制御する「も購入」

コンテキストの特異性を追加することで、次のレベルへの販売挑発的な方で作成する 2 つ目のユーザー コントロールになります。

最初の「も購入」の項目を計算するロジックが自明でないです。

「も購入」コントロールは、現在選択されている ProductID に対して (以前に購入された) OrderDetails レコードを選択しが見つかった各一意の注文の Orderid を取得します。

私たちは製品を選択 al からこれらすべての注文と合計数量を購入しました。 製品を並べ替えるその量の合計がされ、上位 5 つの項目を表示します。

このロジックの複雑さを考えると、ストアド プロシージャとして、このアルゴリズムを実装しましたします。

ストアド プロシージャの T-SQL では次のとおりです。

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

アプリケーションでは、テーブルと、必要な Entity Data Model ビューだけでなく、指定したエンティティ データ モデルを生成したときに含めて ときにデータベースでこのストアド プロシージャ (SelectPurchasedWithProducts) が存在したことに注意してください。このストアド プロシージャを含める必要があります。

Entity Data Model からストアド プロシージャにアクセスするには、関数をインポートする必要があります。

ダブルクリックしてデザイナーで開き、モデル ブラウザーを開き、ソリューション エクスプ ローラーで、Entity Data Model デザイナー内を右クリックし、「関数インポートの追加」を選択します。

![](tailspin-spyworks-part-7/_static/image1.png)

そうと、このダイアログ ボックスが開きます。

![](tailspin-spyworks-part-7/_static/image2.png)

"SelectPurchasedWithProducts"を選択すると、上記に参照するようにしてフィールドに入力し、インポートされた関数の名前のプロシージャ名を使用します。

[Ok] をクリックします。

かもしれませんモデルの他のアイテムとしてストアド プロシージャに対してプログラミングできる単にこれを行うこと。

そのため、「コントロール」フォルダーでは、AlsoPurchased.ascx という名前の新しいユーザー コントロールを作成します。

このコントロールのマークアップは非常に使いやすく PopularItems コントロールになります。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

重要な違いは、出力キャッシュしないすると、項目の表示には製品によって異なるためです。

ProductId をコントロールに"property"となります。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

コントロールの PreRender イベント ハンドラーで次の 3 つの作業を行う eed します。

1. ProductID が設定されていることを確認します。
2. 現在の 1 つで購入された製品があるかどうかを参照してください。
3. 手順 2 で決定されるいくつかの項目を出力します。

モデルを使ってストアド プロシージャを呼び出すがいかに簡単かに注意してください。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

ですが、「も購入」を決定した後だけです、repeater、クエリによって返される結果にバインドできます。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

「も購入」項目がない場合は、弊社のカタログから他の人気のある項目を表示しますがだけです。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

「購入も」アイテムを表示するには、ProductDetails.aspx ページを開き、マークアップでは、この位置に表示されるように、ソリューション エクスプ ローラーから AlsoPurchased コントロールをドラッグします。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

そうと、ProductDetails ページの上部にあるコントロールへの参照が作成されます。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

AlsoPurchased ユーザー コントロールには、ページの現在のデータ モデルのアイテムに対しては Eval ステートメントを使用して、コントロールの ProductID プロパティを設定しますが、ProductId 番号が必要です。

![](tailspin-spyworks-part-7/_static/image3.png)

開発し今すぐ実行および成果物への参照時に、「購入も」アイテムがわかります。

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-6.md)
> [次へ](tailspin-spyworks-part-8.md)
