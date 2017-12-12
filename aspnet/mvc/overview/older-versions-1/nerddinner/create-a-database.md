---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "データベースを作成 |Microsoft ドキュメント"
author: microsoft
description: "手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーション用のデータを RSVP 手順を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a>データベースを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料のステップ 2 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーション用のデータを RSVP 手順を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner 手順 2: データベースの作成

使用するデータベースすべて Dinner および RSVP のデータの NerdDinner アプリケーションを格納します。

無料の SQL Server Express edition を使用してデータベースを作成する次の手順を表示する (の V2 を使用して簡単にインストールできますが、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。 SQL Server Express と完全 SQL サーバーの両方で動作するすべてのコードを記述します。

### <a name="creating-a-new-sql-server-express-database"></a>新しい SQL Server Express データベースを作成します。

うまく、web プロジェクトを右クリックして開始しを選択、**追加 -&gt;新しい項目の**メニュー コマンド。

![](create-a-database/_static/image1.png)

Visual Studio の新しい項目の追加 ダイアログが表示されます。 「データ」カテゴリでフィルタ リングして、「SQL Server データベース」の項目テンプレートを選択してお行います。

![](create-a-database/_static/image2.png)

"NerdDinner.mdf"を作成し、[ok] をクリックすると SQL Server Express データベースという名前です。 Visual Studio は、ご質問、\App にこのファイルを追加するかどうかは\_データ ディレクトリ (ディレクトリは既に読み書きの両方でのセットアップし、セキュリティ Acl の書き込み)。

![](create-a-database/_static/image3.png)

ここをクリックして、"Yes"と、新しいデータベースが作成され、ソリューション エクスプ ローラーに追加。

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>データベース内のテーブルを作成します。

新しい空のデータベースがあるようになりました。 一部のテーブルを追加してみましょう。

これを行うには、"サーバー"タブ エクスプ ローラー、データベースとサーバーを管理することにより、Visual Studio 内に移動おをします。 \App に格納されている SQL Server Express データベース\_サーバー エクスプ ローラーで、アプリケーションのデータ フォルダーが自動的に表示。 必要に応じても一覧に追加の SQL Server データベース (ローカルおよびリモート) を追加するのに"[サーバー エクスプ ローラー] ウィンドウの上部にある「データベースへの接続」アイコンを使用できます。

![](create-a-database/_static/image5.png)

NerdDinner データベース –、ディナー、もう一方に RSVP 承諾を追跡するためを格納する 1 つに 2 つのテーブルを追加します。 データベース内の"Tables"フォルダーを右クリックして、「新しいテーブルの追加」メニュー コマンド を選択して新しいテーブルを作成できます。

![](create-a-database/_static/image6.png)

テーブルのスキーマを構成することができるテーブル デザイナーが開きます。 お「ディナー」テーブルでは、10 列のデータを追加します。

![](create-a-database/_static/image7.png)

テーブルの一意の主キー列に"DinnerID"します。 "DinnerID"列を右クリックし、「主キーの設定」メニュー項目を選択してこれを構成できます。

![](create-a-database/_static/image8.png)

DinnerID 主キーを行うだけでなくたいことも、テーブルに新しい行のデータが追加されると、値が自動的に増分されます"id"列として構成する (つまり、最初に挿入された Dinner 行が 1 の DinnerID が、もう 1 つ挿入された行DinnerID 2 など)。

これには、"DinnerID"列を選択すると、プロパティを設定する、「(された Id)」を"Yes"の列に「列のプロパティ」エディターを使用してしたりできます。 標準の id の既定値を使用して (1 から始まり、新しい Dinner 各行に 1 をインクリメント)。

![](create-a-database/_static/image9.png)

CTRL-S を入力するかを使用しておし、テーブルを保存します、**ファイル -&gt;保存**メニュー コマンド。 テーブルの名前を求められます。 という名前に「ディナー」。

![](create-a-database/_static/image10.png)

この新しいディナー テーブルは、サーバー エクスプ ローラーで、データベース内で表示されます。

この手順を繰り返し、"RSVP"テーブルを作成します。 このテーブルでは、3 つの列があります。 RsvpID 列、主キーとして設定して、id 列にすることもおされます。

![](create-a-database/_static/image11.png)

保存し、"RSVP"の名前を付けることがあります。

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>テーブル間に外部キー リレーションシップを設定します。

データベース内で 2 つのテーブルであるおようになりました。 最後のスキーマ デザイン手順がする – これら 2 つのテーブル間の「一対多」リレーションシップを設定するように Dinner 行ごとに適用する 0 個以上の RSVP 行に関連付けられたことができます。 RSVP テーブルの"DinnerID"列「ディナー」テーブルに"DinnerID"列に外部キー関係を構成することによってこの作業が行うされます。

サーバー エクスプ ローラーでダブルクリックして、テーブル デザイナー内で RSVP テーブルを開くことがこれを行うには。 "DinnerID"の列がしを選択し、右クリック「Relationshps…」を選択 コンテキスト メニューのコマンド:

![](create-a-database/_static/image12.png)

テーブル間のリレーションシップのセットアップに使用できるダイアログが表示されます。

![](create-a-database/_static/image13.png)

ダイアログ ボックスに新しいリレーションシップを追加する「追加」ボタンをクリックします。 リレーションシップを追加すると、ダイアログ ボックスの右側に、プロパティ グリッド内で「テーブルと列の仕様」ツリー ビュー ノードを展開し、その右側にある [...] ボタンをクリックおします。

![](create-a-database/_static/image14.png)

「...」ボタンをクリックすると、指定のテーブルと列は、リレーションシップに関係するだけでなく、リレーションシップの名前を付けることにより、別のダイアログ ボックスが表示されます。

「ディナー」である主キー テーブルを変更し、主キーとしてディナー テーブル内の"DinnerID"列を選択おはします。 RSVP テーブルは、外部キー テーブル、および、RSVP になります。DinnerID 列は、外部キーとして関連付けられているになります。

![](create-a-database/_static/image15.png)

RSVP テーブルの各行に関連付けられる Dinner テーブルの行にします。 SQL Server はご利用の米国 – 参照整合性を維持し、有効な夕食行を指定していない場合は、新しい RSVP 行を追加することを防ぐ。 できなくなります us からまだ行が参照することを RSVP がある場合は、Dinner 行を削除します。

### <a name="adding-data-to-our-tables"></a>データは、テーブルを追加します。

このディナー テーブルにサンプル データを追加することで完了してみましょう。 サーバー エクスプ ローラー内でそれを右クリックし、「テーブルのデータを表示する」コマンドを選択するデータをテーブルに追加できます。

![](create-a-database/_static/image16.png)

数行のように、アプリケーションの実装を開始して後で使用できる Dinner データ追加されます。

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>次の手順

データベースの作成が完了したらです。 クエリを実行し、更新に使用できるモデル クラスを今すぐ作成してみましょう。

>[!div class="step-by-step"]
[前へ](create-a-new-aspnet-mvc-project.md)
[次へ](build-a-model-with-business-rule-validations.md)
