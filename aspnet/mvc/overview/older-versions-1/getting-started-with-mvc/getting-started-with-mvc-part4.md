---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: "データベースを作成する |Microsoft ドキュメント"
author: shanselman
description: "これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 6a9617b8056f883d6be2547588b13063a58abd0e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-database"></a>データベースを作成します。
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


このセクションで行う、ムービー データ格納および取得に使用する新しい SQL Express データベースを作成します。 Visual Web Developer IDE 内でビューを選択 |サーバー エクスプ ローラー。 データ接続を右クリックし、[接続の追加] をクリックしてください.

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

データ ソースの選択 ダイアログ ボックスでは、Microsoft SQL Server を選択し、続行 を選択します。

![](getting-started-with-mvc-part4/_static/image2.png)

接続の追加 ダイアログ ボックスで、次を入力してください。"。 \SQLEXPRESS"、サーバー名に対応し、新しいデータベースの名前として"映画"を入力します。

[![追加の接続 ダイアログ ボックス](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

[Ok] をクリックし、たずねられますをそのデータベースを作成するかどうか。 [はい] を選択します。

[![ムービーを作成しますか。](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

作成できたところで、空のデータベース サーバー エクスプ ローラーにします。

![新しいテーブルを追加します。](getting-started-with-mvc-part4/_static/image7.png)

テーブルを右クリックし、[テーブルの追加] をクリックします。 テーブル デザイナーが表示されます。 Id、タイトル、ReleaseDate、ジャンル、および価格の列を追加します。 ID 列を右クリックし、主キーを設定する をクリックします。 ようになります、どのようなデザイン領域を次に示します。

[![データベース テーブル エディター](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

また、Id 列を選択し、以下の列のプロパティには、"Yes"に「Identity の指定」を変更

[![IsIdentity - 列のプロパティ](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

これを取得したら、ツールバーの [保存] アイコンをクリックするかファイルを選択して |メニューから、保存し、テーブルの名前"**ムービー**"(単数形)。 データベースとテーブルを特定しました!

[![名前を選択します。](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

サーバー エクスプ ローラーに戻る、ムービー テーブルを右クリックし、選択「テーブル データの表示」です。 データベースにいくつかのデータがあるため、いくつかの映画を入力します。

[![データベース テーブルの編集](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>モデルを作成します。

ここで、IDE の右辺でソリューション エクスプ ローラーに戻りますし、モデル フォルダーを右クリックし、追加 |新しい項目。

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

新しいデータベースから Entity Model を作成しようとしています。 これでは、一連のクラスを容易にクエリを実行し、データベース内のデータを操作するうえで、プロジェクトに追加します。 このダイアログの左側にあるデータ ノードを選択し、ADO.NET Entity Data Model の項目テンプレートを選択します。 Movies.edmx 名前を付けます。

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

[追加] ボタンをクリックします。 これは、「Entity Data Model ウィザード」を開始されます。

ポップアップ表示される新しい ダイアログでは、データベースから生成を選択します。 データベース、だけしましたのでおのみ必要があります、新しいデータベースとそのテーブルは、Entity Framework を伝えます。 Web アプリケーションの構成では、データベース接続の横をクリックします。 テーブルとムービーを確認して、[完了] チェック ボックスをクリックします。

[![Entity Data Model ウィザード](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

これで、Entity Framework Designer で、新しいムービー表を参照して、コードからアクセスできます。

[![映画 - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

デザイン画面では、「ムービー」クラスを表示できます。 このクラスがデータベースに「ムービー」テーブルにマップされ、その中の各プロパティがテーブルと列にマップします。 「ムービー」クラスの各インスタンスは、「ムービー」テーブル内の行に対応します。

既定の名前付けと、Entity Framework で使用される規則のマッピングに満足できない場合は、変更またはカスタマイズするに Entity Framework デザイナーを使用することができます。 このアプリケーションがうまく、既定値を使用し、ファイルとして保存-はします。

ここで、いくつかの実際のデータを操作できます。

>[!div class="step-by-step"]
[前へ](getting-started-with-mvc-part3.md)
[次へ](getting-started-with-mvc-part5.md)
