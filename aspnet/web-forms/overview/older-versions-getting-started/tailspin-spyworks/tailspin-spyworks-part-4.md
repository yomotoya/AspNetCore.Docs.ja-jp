---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'パート 4: 製品を一覧表示 |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、GridView contr. で製品のリスティングについて説明します.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838662"
---
<a name="part-4-listing-products"></a>パート 4: 製品のリスティング
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、GridView コントロールと製品のリスティングについて説明します。


## <a id="_Toc260221670"></a>  GridView コントロールを備えた製品を一覧表示します。

"Add"および"新しい項目 を選択して、ソリューションでは、「右クリック」ProductsList.aspx、ページの実装を開始しましょう。

![](tailspin-spyworks-part-4/_static/image1.jpg)

「Web フォームを使用してマスター ページ」を選択し、ProductsList.aspx のページ名を入力してください"です。

[追加] をクリックします。

![](tailspin-spyworks-part-4/_static/image2.jpg)

次に Site.Master ページを配置しました [スタイル] フォルダーを選択し、「フォルダーの内容」ウィンドウから選択します。

![](tailspin-spyworks-part-4/_static/image3.jpg)

ページを作成するには [Ok] をクリックします。

次に示すように、データベースには製品データが表示されます。

![](tailspin-spyworks-part-4/_static/image4.jpg)

ページを作成した後、製品データにアクセスするエンティティのデータ ソースをもう一度使用しますが、このインスタンスで製品エンティティを選択してだけ選択したカテゴリに返される項目を制限する必要があります。

これを実現するには、WHERE 句の自動生成 EntityDataSource を説明し、WhereParameter を指定します。

[製品カテゴリ メニュー] で、メニュー項目を作成したときに動的に構築、リンク、CatagoryID を各リンクのクエリ文字列に追加することで。 思い出してください。 場所のパラメーターをクエリ文字列パラメーターから派生するエンティティのデータ ソースをお知らせします。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

次に、製品の一覧を表示する ListView コントロールを構成します。 最適なショッピング エクスペリエンスを作成するには、ListVew に表示される各製品にいくつかの簡潔な機能を縮小します。

- 製品名は、製品の詳細ビューへのリンクになります。
- 製品の価格が表示されます。
- 製品の画像が表示され、アプリケーションでは、カタログ イメージのディレクトリからイメージを選択しましたが動的にします。
- すぐに特定の製品をショッピング カートに追加するリンクが表示されます。

ListView コントロール インスタンスのマークアップを次に示します。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

動的には表示されている各製品のいくつかのリンクを作成します。

また、独自の新しいページをテストする前に、製品のディレクトリ構造にカタログのイメージを次のように作成する必要があります。

![](tailspin-spyworks-part-4/_static/image1.png)

マイクロソフト製品の画像がアクセス可能な製品リスト ページをテストできます。

![](tailspin-spyworks-part-4/_static/image5.jpg)

サイトのホーム ページで、カテゴリの一覧のリンクのいずれかをクリックします。

![](tailspin-spyworks-part-4/_static/image6.jpg)

ProductDetials.apsx ページと AddToCart 機能を実装する必要があります。

ファイルを使用して、&gt;ProductDetails.aspx 以前に行ったように、サイトのマスター ページを使用してページ名を作成します。

EntityDataSource コントロールを使用して、データベース内の特定の製品レコードへのアクセスにはもう一度おと ASP.NET FormView コントロールを使用して次のように、製品データを表示します。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

少しおかしいは書式設定をする場合、心配しないでください。 上記のマークアップの余地が画面のレイアウトで、2 つの機能を後で実装します。

ショッピング カートは、アプリケーションではより複雑なロジックを表します。 最初に、使用してファイルの&gt;MyShoppingCart.aspx と呼ばれるページを作成します。

名の後選択しないことに注意してください。

データベースには、"ShoppingCart"という名前のテーブルが含まれています。 Entity Data Model を生成したときに、クラスは、データベース内の各テーブルに対して作成されました。 そのため、エンティティ データ モデルでは、"ShoppingCart"という名前のエンティティ クラスが生成されます。 行わないことに代わりに単純に現在名前の競合を回避するには、ショッピング カートの実装には、その名前を使用したり、ニーズに応じて、拡張されるように、モデルを編集しましたでした。

簡単なショッピング カートを作成したことに注意してください。 および、ショッピング カートの表示でショッピング カート ロジックの埋め込み価値もです。 また、ショッピング カートを完全に独立したビジネス層で実装に選択する場合があります。

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-3.md)
> [次へ](tailspin-spyworks-part-5.md)
