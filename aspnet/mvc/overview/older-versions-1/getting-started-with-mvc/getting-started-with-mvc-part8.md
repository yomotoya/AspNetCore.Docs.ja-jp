---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: モデルに列を追加する |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-column-to-the-model"></a>モデルに列を追加します。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


このセクションで行う、データベースのスキーマを変更して、アプリケーション内で、変更を処理した方法について説明します。

ムービーの表に、「評価」の列を追加してみましょう。 IDE に戻るし、[データベース エクスプ ローラー] をクリックします。 ムービー テーブルを右クリックし、開くテーブルの定義を選択します。

以下に示すように、「評価」列を追加します。 これで任意の評価があるありません、ために、列は null を許容できます。 [保存] をクリックします。

[![映画のテーブルの編集](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

次に、ソリューション エクスプ ローラーに返し、Movies.edmx ファイル (は \Models フォルダーにあります) を開きます。 デザイン サーフェイス (白の領域) を右クリックし、データベースから更新プログラムのモデルを選択します。

[![映画 - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

これにより、「更新ウィザード」が起動します。 内で、[更新] タブをクリックし、[完了] をクリックします。 ムービー モデル クラスは、新しい列と共に更新されます。

![(2) の更新ウィザード](getting-started-with-mvc-part8/_static/image5.png)

[完了] をクリックすると、表示できます、ムービー エンティティ モデルに新しい [評価] 列が追加されました。

[![ムービーのエンティティ](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

データベース モデルの列が追加されましたが、ビューには、把握できていないこと。

## <a name="update-views-with-model-changes"></a>モデルの変更と更新の表示

いくつかの方法が、新しい [評価] 列を反映するように、テンプレートの表示を更新することができます。 ビューの追加 ダイアログを使用してそれらを生成することによってこれらのビューを作成した後は、それらを削除しはもう一度再おでした。 ただし、通常のユーザーが既に変更を加えました、テンプレートを表示する最初のスキャフォールディング生成から、追加したり、作成、ID フィールドに行ったように、手動でフィールドを削除する必要があります。

\Views\Movies\Index.aspx テンプレートを開き、追加、 &lt;th&gt;評価&lt;/th&gt;ムービー テーブルの先頭にします。 ジャンル後、私は追加されます。 次に、同じ列の位置が下位に、行を追加して、新しい評価を出力します。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

最終的な Index.aspx テンプレートは、次のようになります。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

みましょう \Views\Movies\Create.aspx テンプレートを開き、新しい評価プロパティのラベルとテキスト ボックスを追加します。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

最終的な Create.aspx テンプレートが次のように、され、ブラウザーのタイトルとセカンダリを変更してみましょう&lt;h2&gt;映画のように"を作成、"ここにいるときに何かにタイトルです。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

アプリを実行し、作成できたところで、[作成] ページに追加されているデータベースの新しいフィールドです。 新しいムービーの評価の場合は、この時間を追加し、作成 をクリックします。

[![Windows Internet Explorer - ムービーを作成します。](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

作成 をクリックすると、インデックス ページに送信している、新しいムービーが記載されている場合、新しい評価データベース内の列

[![ムービーの一覧]-[Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

この基本的なチュートリアルでは、コント ローラー、ビューに関連付けると、ハード コードされたデータを渡すことを行う作業を開始する取得しました。 お作成およびデータベースを設計および一部のデータを格納しでします。 データベースからデータを取得し、HTML テーブルに表示します。 データを追加、データベース自体から Web アプリケーション内でユーザーを許可するフォームの作成が追加されました。 追加の検証、その後、クライアント側で JavaScript を使用して、検証が行われました。 最後に、おに、データの新しい列を含めるデータベースを変更し、作成し、この新しいデータを表示する、2 つのページを更新します。

今すぐみて、中間レベルのチュートリアルへお進み"[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)"だけでなく多くのビデオおよびにリソースを[ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC についてさらに学習します。

それではお楽しみください。

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com)と[ @shanselman ](http://twitter.com/shanselman) Twitter でします。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part7.md)
