---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: モデルに列の追加 |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 22a6c4e5a07e81d5876cc442e68926094e3a243d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825856"
---
<a name="adding-a-column-to-the-model"></a>モデルに列を追加します。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


このセクションではここに、データベースのスキーマ変更を加えるし、アプリケーション内で、変更を処理した方法を説明します。

Movie テーブルには、「評価」の列を追加してみましょう。 IDE に戻るし、データベース エクスプ ローラー をクリックします。 Movie テーブルを右クリックし、テーブル定義を開くを選択します。

次に示すように、「評価」列を追加します。 これで任意の評価があるない、ため、列は null を許容できます。 [保存] をクリックします。

[![映画のテーブルの編集](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

次に、ソリューション エクスプ ローラーに戻り、(これ \Models フォルダーには) Movies.edmx ファイルを開きます。 デザイン サーフェイス (白の領域) を右クリックし、データベースからモデルを更新を選択します。

[![ビデオ - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

これにより、「の更新ウィザード」が起動します。 内に、[更新] タブをクリックし、[完了] をクリックします。 Movie モデル クラスは、新しい列と共に更新されます。

![(2) の更新ウィザード](getting-started-with-mvc-part8/_static/image5.png)

[完了] をクリックすると、新しい [評価] 列が、モデル内のムービー エンティティに追加されていますがわかります。

[![ムービー エンティティ](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

データベース モデルの列が追加されましたが、それに関するビューがわからない。

## <a name="update-views-with-model-changes"></a>モデルの変更と更新の表示

新しい [評価] 列を反映するようにテンプレートの表示、更新、いくつかの方法はあります。 ビューの追加 ダイアログを使用してそれらを生成することによってこれらのビューを作成した後はそれらを削除でしたし、もう一度再作成します。 ただし、通常ユーザーが既に変更を加えたビュー テンプレートに最初のスキャフォールディングの生成から、追加または作成時に、ID フィールドのときと同様、手動でフィールドを削除します。

\Views\Movies\Index.aspx テンプレートを開き、追加、 &lt;th&gt;評価&lt;/th&gt; Movie テーブルの先頭にします。 ジャンルの後に、私は追加されます。 次で同じ列の位置が下位には、当社の新しい評価の出力に行を追加します。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

最終的な Index.aspx テンプレートは、次のようになります。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

みましょうし \Views\Movies\Create.aspx テンプレートを開き、新しい評価プロパティのラベルとテキスト ボックスを追加します。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

最終的な Create.aspx テンプレートは次のようと、ブラウザーのタイトルとセカンダリを変更してみましょう&lt;h2&gt;タイトルを「を作成、映画」ここで私たちのようなものです。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

アプリを実行し、[作成] ページに追加されているデータベースの新しいフィールドをしたようになりました。 -新しいムービーを追加するには、この時点で評価 - と作成 をクリックします。

[![Windows Internet Explorer - ムービーを作成します。](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

作成 をクリックすると後、は、インデックス ページに送信している新しいムービーが記載されている場合、データベース内の新しい評価列

[![ムービーの一覧 - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

この基本的なチュートリアルでは、コント ローラー、ビューと関連付けると、ハード コーディングされたデータの受け渡しを行う作業を開始する必要があります。 作成しデータベースを設計して一部のデータの配置でします。 データベースからデータを取得し、HTML テーブルに表示されることです。 ユーザー データを追加、データベース自体から Web アプリケーション内で使用できるフォームを作成するを追加します。 私たちは、検証を追加し、JavaScript を使用して、クライアント側で検証が行われたします。 最後に、私たちに、データの新しい列を含めるデータベースを変更し、作成し、この新しいデータを表示する、2 つのページを更新します。

中間レベルのチュートリアル移動するようになりました"[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)"と、多くのビデオおよびリソース[ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC についてさらに学習します。

それではお楽しみください。

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com)と[ @shanselman ](http://twitter.com/shanselman) twitter。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part7.md)
