---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: データベースを作成する |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813761"
---
<a name="creating-a-database"></a>データベースを作成します。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


このセクションでは、ムービー データ格納および取得に使用する新しい SQL Express データベースを作成するいきます。 Visual Web Developer IDE 内での選択ビュー |サーバー エクスプ ローラー。 データ接続を右クリックし、[接続の追加] をクリックしてください.

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

データ ソースの選択 ダイアログ ボックスで、Microsoft SQL Server を選択し、続行 を選択します。

![](getting-started-with-mvc-part4/_static/image2.png)

接続の追加 ダイアログ ボックスで次のように入力します。"。 \SQLEXPRESS"、サーバー名に対応し、新しいデータベースの名前として"Movies"を入力します。

[![追加の接続 ダイアログ ボックス](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

[OK] をクリックし、求められますかどうかは、そのデータベースを作成します。 [はい] を選択します。

[![ムービーを作成しますか。](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

サーバー エクスプ ローラーで、空のデータベースがあります。

![新しいテーブルを追加します。](getting-started-with-mvc-part4/_static/image7.png)

テーブルを右クリックし、[テーブルの追加] をクリックします。 テーブル デザイナーが表示されます。 Id、タイトル、ReleaseDate、ジャンル、および価格の列を追加します。 ID 列を右クリックし、主キーの設定 をクリックします。 次のようになります、どのようなデザイン領域に示します。

[![データベース テーブル エディター](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

また、Id 列を選択し、以下の列のプロパティを"Yes"にします「Id の指定」を変更します。

[![IsIdentity - 列のプロパティ](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

これを取得したら、ツールバーの [保存] アイコンをクリックしますまたはファイルを選択します |。メニューから、保存し、テーブルの名前"**ムービー**"(単数形)。 データベースとテーブルを作成しました!

[![名前を選択します。](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

サーバー エクスプ ローラーに戻り、Movie テーブルを右クリックし、"テーブル データの表示。 を選択します。 データベースにいくつかのデータがあるため、いくつかの映画を入力します。

[![データベース テーブルの編集](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>モデルの作成

ここで、IDE の右側に、ソリューション エクスプ ローラーに戻るし、Models フォルダーを右クリックし、追加 |新しい項目。

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

新しいデータベースからエンティティ モデルを作成しようとしています。 これにより、一連のクラスを照会して、データベース内のデータを操作するが容易にするプロジェクトに追加されます。 ダイアログの左側にあるデータ ノードを選択し、ADO.NET Entity Data Model 項目テンプレートを選択します。 Movies.edmx 名前を付けます。

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

[追加] ボタンをクリックします。 これが、「Entity Data Model ウィザード」を起動しています。

新しいダイアログが表示されたらでは、データベースから生成を選択します。 データベースだけしていますので、新しいデータベースとそのテーブルについて、Entity Framework にのみ必要あります。 Web アプリケーションの構成では、データベース接続の横をクリックします。 ここで、テーブルとムービーを確認 [完了] チェック ボックスをクリックします。

[![Entity Data Model ウィザード](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

ここで、Entity Framework デザイナーで新しいムービー テーブルを参照してください。 し、コードからアクセスできます。

[![ビデオ - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

デザイン画面では、「ビデオ」クラスを確認できます。 このクラスは、データベース内の「ムービー」テーブルにマップし、内の各プロパティは、テーブルの列にマップします。 「ビデオ」クラスの各インスタンスは"Movie"テーブル内の行に対応します。

既定の名前付けと Entity Framework によって使用される規則のマッピングを希望しない場合は、変更またはそれらをカスタマイズする Entity Framework デザイナーを使用できます。 このアプリケーションの既定値を使用して、なりますファイルとして保存-です。

ここで、実際のデータはいくつか見ていきましょう。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part3.md)
> [次へ](getting-started-with-mvc-part5.md)
