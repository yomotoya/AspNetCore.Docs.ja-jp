---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'パート 4: 製品のリスト |Microsoft ドキュメント'
author: JoeStagner
description: このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 4 カバーが GridView contr. で製品を一覧表示する.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a>パート 4: 一覧の製品
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、GridView コントロールの一覧の製品について説明します。


## <a id="_Toc260221670"></a>  GridView コントロールでの製品を一覧表示します。

"Add"および「新しい項目の」をクリックし、ソリューションでは、「を右クリック」、ProductsList.aspx ページの実装を開始してみましょう。

![](tailspin-spyworks-part-4/_static/image1.jpg)

「Web フォームを使用してマスター ページ」を選択し、ProductsList.aspx のページ名を入力してください"です。

[追加] をクリックします。

![](tailspin-spyworks-part-4/_static/image2.jpg)

次 Site.Master ページを配置お「スタイル」フォルダーを選択し、「フォルダーの内容」ウィンドウから選択します。

![](tailspin-spyworks-part-4/_static/image3.jpg)

"Ok"ページを作成する をクリックします。

データベースには、次のように製品のデータが格納されます。

![](tailspin-spyworks-part-4/_static/image4.jpg)

ページを作成した後エンティティ データ ソースを使用して、その製品データにアクセスをもう一度おがこのインスタンスで製品のエンティティを選択する必要があり、選択したカテゴリのものだけに返される項目を制限する必要があります。

これを実現するには、WHERE 句を自動生成する EntityDataSource を説明し、WhereParameter を指定します。

この「製品カテゴリ メニュー」で、メニュー項目を作成したときに動的に構築リンク リンクごとに、クエリ文字列を CatagoryID を追加することで説明します。 クエリ文字列パラメーターから WHERE パラメーターを派生するエンティティのデータ ソースお知らせします。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

次に、製品の一覧を表示するリスト ビュー コントロールを構成します。 ショッピング最適なエクスペリエンスを作成するには、ListVew に表示される個々 の製品にいくつかの簡潔な機能を縮小します。

- 製品名、製品の詳細ビューへのリンクになります。
- 製品の価格が表示されます。
- 製品の画像が表示され、アプリケーションでは、カタログ イメージ ディレクトリからイメージを選択おを動的にします。
- すぐに特定の製品をショッピング カートに追加へのリンクが含まれます。

ListView コントロール インスタンスのマークアップを次に示します。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

表示されている各製品のいくつかのリンクを動的に構築おされます。

また、独自の新しいページをテストする前に、ディレクトリ構造、製品のカタログのイメージを次のように作成する必要があります。

![](tailspin-spyworks-part-4/_static/image1.png)

当社製品の画像がアクセス可能な製品リスト ページをテストします。

![](tailspin-spyworks-part-4/_static/image5.jpg)

サイトのホーム ページでは、カテゴリの一覧のリンクのいずれかをクリックします。

![](tailspin-spyworks-part-4/_static/image6.jpg)

ProductDetials.apsx ページと AddToCart 機能を実装する必要があります。

ファイルを使用して&gt;を以前と同様に、サイトのマスター ページを使用して ProductDetails.aspx ページ名を作成します。

EntityDataSource コントロールを使用して、データベース内の特定の製品レコードにアクセスする、もう一度おと、次のように、製品データを表示する ASP.NET FormView コントロールを使用します。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

書式設定するビットおもしろい見える場合、心配しないでください。 上記のマークアップの余地が画面のレイアウトで、いくつかの機能を後で実装します。

ショッピング カートは、アプリケーションではより複雑なロジックを表します。 、作業を開始するには、次を使用してファイル-&gt;MyShoppingCart.aspx をという名前のページを作成します。

後の名前を選択していないいるおことに注意してください。

データベースには、"ShoppingCart"という名前のテーブルが含まれています。 Entity Data Model を生成したとき、クラスは、データベース内の各テーブルに対して作成されました。 そのため、エンティティ データ モデルでは、"ShoppingCart"という名前のエンティティ クラスが生成されます。 競合を回避する名前を選択するだけに代わりに登録されますが、ショッピング カート実装するためには、その名前を使用または、ニーズの拡張おでしたように、そのモデルを編集お可能性があります。

簡単なショッピング カートを作成しているに留意とがショッピング カート表示に、ショッピング カート ロジックを埋め込み価値もです。 完全に独立したビジネス層に、ショッピング カートを実装する場合がありますも選択します。

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-3.md)
> [次へ](tailspin-spyworks-part-5.md)
