---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: データベースを作成する |Microsoft Docs
author: microsoft
description: 手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーションのデータを RSVP の手順を示します。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 2a2ed8c91aa3ec0da63cd8e8686ff601f6e66374
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818967"
---
<a name="create-a-database"></a>データベースを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 2 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーションのデータを RSVP の手順を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner 手順 2: データベースの作成

使用するデータベースすべて Dinner および RSVP のデータのアプリケーションで NerdDinner を格納します。

以下の手順は、無料の SQL Server Express edition を使用してデータベースの作成を示します (の V2 を使用して簡単にインストールできますが、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。 SQL Server Express と完全な SQL Server の両方で動作するすべてのコードを記述します。

### <a name="creating-a-new-sql-server-express-database"></a>新しい SQL Server Express データベースを作成します。

右クリックし、web プロジェクトを開始し、いますが、**追加 -&gt;新しい項目の**メニュー コマンド。

![](create-a-database/_static/image1.png)

Visual Studio の「新しい項目の追加」ダイアログが表示されます。 「データ」カテゴリでフィルター処理、「SQL Server データベース」の項目テンプレートを選択します。

![](create-a-database/_static/image2.png)

という名前を SQL Server Express のデータベースを"NerdDinner.mdf"を作成し、[ok] をクリックします。 Visual Studio は、ご質問、\App にこのファイルを追加するかどうかは\_データ ディレクトリ (ディレクトリは既に読み書きの両方でのセットアップし、セキュリティ Acl を記述)。

![](create-a-database/_static/image3.png)

[はい] をクリックし、新しいデータベースが作成され、ソリューション エクスプ ローラーに追加します。

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>データベース内のテーブルを作成します。

新しい空のデータベースがあるようになりました。 いくつかのテーブルを追加してみましょう。

これを行う、データベースとサーバーの管理により、Visual Studio 内の [サーバー エクスプ ローラー] タブ ウィンドウへ移動します。 SQL Server Express のデータベース、\App に格納されている\_サーバー エクスプ ローラーで、アプリケーションのデータ フォルダーが自動的に表示。 必要に応じてもリストに追加の SQL Server データベース (ローカルおよびリモート) を追加するのに"サーバー エクスプ ローラー ウィンドウの上部にある「データベースに接続」アイコンを使用できます。

![](create-a-database/_static/image5.png)

NerdDinner データベース –、Dinners、およびその他、RSVP の承諾を追跡を格納する 1 つを 2 つのテーブルに追加します。 データベース内の「テーブル」フォルダーを右クリックして、"新しいテーブルの追加 メニューのコマンドを選択して新しいテーブルを作成できます。

![](create-a-database/_static/image6.png)

テーブルのスキーマを構成することができるようにする、テーブル デザイナーが開きます。 "Dinners"テーブルは 10 個の列のデータの追加します。

![](create-a-database/_static/image7.png)

テーブルの一意の主キー列に"DinnerID"します。 この"DinnerID"列を右クリックし、「主キーの設定」メニュー項目を選択して構成できます。

![](create-a-database/_static/image8.png)

DinnerID 主キーを作成に加えたいことも、テーブルに新しいデータ行が追加されると、値が自動的にインクリメントされる"id"列として構成する場合 (つまり、最初に挿入された Dinner 行が 1 の DinnerID が、2 番目の挿入された行DinnerID が 2 など)。

"DinnerID"列を選択してこれを"Yes"には、列に「(Id である)」プロパティを設定する [列のプロパティ] エディターを使用できます。 標準の id の既定値を使用して、(1 から始まりますおよび新しい Dinner 行ごとに 1 インクリメント)。

![](create-a-database/_static/image9.png)

CTRL-S を入力するかを使用していますし、テーブルを保存します、**ファイル -&gt;保存**メニュー コマンド。 テーブルの名前を入力を求められます。 という名前に"Dinners"。

![](create-a-database/_static/image10.png)

新しい Dinners テーブルも、サーバー エクスプ ローラーで、データベース内で表示されます。

上記の手順を繰り返して、され"RSVP"テーブルを作成します。 このテーブルでは、3 つの列があります。 RsvpID 列、主キーとしてセットアップされ、id 列にすることもされます。

![](create-a-database/_static/image11.png)

保存がされ"RSVP"という名前を付けます。

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>テーブル間に外部キー リレーションシップの設定

これで 2 つのテーブルがある、データベース内で。 Dinner 行ごとに適用する 0 個以上の RSVP の行に関連付けられたできますように – 2 つのテーブル間の「1 対多」リレーションシップを設定する、最後のスキーマのデザイン手順がなります。 "Dinners"テーブルに"DinnerID"列に外部キー リレーションシップが RSVP テーブルの"DinnerID"列を構成することによって行います。

これを行うサーバー エクスプ ローラーでダブルクリックして、テーブル デザイナー内で予約テーブルを開くします。 "DinnerID"の列選択し、右クリックし、「Relationshps...」のコンテキスト メニュー コマンドを選択。

![](create-a-database/_static/image12.png)

テーブル間のリレーションシップをセットアップするために使用できるダイアログが表示されます。

![](create-a-database/_static/image13.png)

ダイアログ ボックスに新しいリレーションシップを追加する、[追加] ボタンをクリックします。 リレーションシップを追加するは、ダイアログ ボックスの右側にプロパティ グリッド内で「テーブルと列の仕様」ツリー ビュー ノードを展開し、右側に「...」ボタンをクリックしましたします。

![](create-a-database/_static/image14.png)

「...」ボタンをクリックすると、どのテーブルと列は、リレーションシップに関係するだけでなく、リレーションシップの名前を指定できるようにする別のダイアログが表示されます。

主キー テーブルの"Dinners"を変更いたします。 その主キーとして Dinners テーブル内の"DinnerID"列を選択します。 RSVP テーブルは、外部キー テーブルと、予約になります。外部キーとして関連付けられている DinnerID 列になります。

![](create-a-database/_static/image15.png)

ようになりました RSVP テーブルの各行の Dinner テーブルの行に関連付けられます。 SQL Server は私たちの参照整合性を維持し、これが有効な Dinner 行を指していない場合に新しい RSVP 行を追加することはできません。 これはもすることはできませんからまだ行が参照することを RSVP がある場合は、Dinner 行を削除します。

### <a name="adding-data-to-our-tables"></a>データ テーブルを追加します。

[完了]、Dinners テーブルにサンプル データを追加してみましょう。 サーバー エクスプ ローラー内で右クリックし、「テーブル データの表示」コマンドを選択して、テーブルにデータを追加します。

![](create-a-database/_static/image16.png)

アプリケーションの実装を開始すると後で使用できるデータを Dinner の少数の行を追加します。

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>次の手順

データベースの作成は終了しました。 クエリを実行し、更新するために使用できるモデル クラスを今すぐ作成しましょう。

> [!div class="step-by-step"]
> [前へ](create-a-new-aspnet-mvc-project.md)
> [次へ](build-a-model-with-business-rule-validations.md)
