---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: 新しいフィールドの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 87bb2c5f64e714268f5e2631b44fbb8a93a6a4b6
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578092"
---
<a name="adding-a-new-field"></a>新しいフィールドの追加
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションでは、変更がデータベースに適用されるため、モデル クラスにいくつかの変更を移行するのに Entity Framework Code First Migrations を使用します。

既定を使用する Entity Framework Code First のデータベースを自動的に作成するように、このチュートリアルで前に行ったときに Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデル クラスと同期されているかどうかを追跡できます。 同期していない、Entity Framework は、エラーをスローします。 これにより、表示されている可能性がありますそれ以外の場合のみ (原因不明のエラー) を実行時に、開発時に問題を追跡しやすくします。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>モデルの変更の Code First Migrations の設定

ソリューション エクスプ ローラーに移動します。 右クリックして、 *Movies.mdf*ファイルおよび選択**削除**ムービー データベースを削除します。 表示されない場合、 *Movies.mdf*ファイルで、をクリックして、 **すべてのファイル**アイコンを赤色のアウトラインに示します。

![](adding-a-new-field/_static/image1.png)

エラーがないかどうかを確認するアプリケーションをビルドします。

**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。

![パックのマニュアルを追加します。](adding-a-new-field/_static/image2.png)

**パッケージ マネージャー コンソール**ウィンドウで、`PM>`プロンプトを入力します

Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-migrations**コマンド (上記) を作成、 *Configuration.cs*で新しいファイル*移行*フォルダー。

![](adding-a-new-field/_static/image4.png)

Visual Studio を開き、 *Configuration.cs*ファイル。 置換、`Seed`メソッドで、 *Configuration.cs*を次のコード ファイル。

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

下の赤の波線をポイント`Movie`クリック`Show Potential Fixes`順にクリックします**を使用して** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

これにより、次を追加ステートメントを使用します。

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First Migrations を呼び出し、`Seed`すべて移行後のメソッド (呼び出しは、**データベースを更新**パッケージ マネージャー コンソールで)、このメソッドは、既に挿入されている、または場合に、それらを挿入する行を更新し、まだ存在しません。
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドは、次のコードでは、"upsert"操作を実行します。
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> [シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドは、すべての移行を実行、データベースを作成する最初の移行後に追加しようとしている行があるなる既にためだけデータを挿入できません。 "[Upsert](http://en.wikipedia.org/wiki/Upsert)"操作が既に存在する行を挿入しようとする場合に発生するとエラーを防ぐことが、アプリケーションのテスト中に行ったデータに対する変更を上書きします。 いくつかのテーブルでのテスト データたくないことが起こる: 場合によってはテスト中にデータを変更すると、変更するデータベースの更新後に残します。 条件付きの insert 操作を実行する場合: 存在しない場合にのみ行を挿入します。   
> 
> 渡される最初のパラメーター、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドを使用して、行が既に存在するかを確認するプロパティを指定します。 テスト ビデオ データを提供するには`Title`プロパティは、リスト内の各タイトルは一意であるために、この目的に使用できます。
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> このコードでは、タイトルが一意であることを前提としています。 タイトルの重複を手動で追加する場合、次回の移行を実行する次の例外が表示されます。   
> 
>  *シーケンスには、1 つ以上の要素が含まれています。*  
> 
> 詳細については、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドを参照してください[EF 4.3 AddOrUpdate メソッドを使用して対処](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).


**CTRL + SHIFT+B プロジェクトをビルドするキーを押します。**(次の手順は、この時点で構築しない場合失敗します)。

次の手順が作成するには、`DbMigration`初回移行のためのクラス。 この移行が作成することは、新しいデータベースを削除する、 *movie.mdf*前の手順でファイル。

**パッケージ マネージャー コンソール**ウィンドウ、コマンドを入力して`add-migration Initial`初期移行を作成します。 「初期」という名前は任意なので名前を移行ファイルを作成するために使用します。

![](adding-a-new-field/_static/image6.png)

Code First Migrations は、別のクラス ファイルを作成、*移行*フォルダー (名前を持つ *{日付スタンプ}\_Initial.cs* )、このクラスには、データベース スキーマを作成するコードが含まれています。 移行ファイル名は事前のタイムスタンプを持つ順序付けに関するヘルプを修正しました。 確認、 *{日付スタンプ}\_Initial.cs*ファイルを作成する手順がある、 `Movies` Movie DB のテーブル。 以下、この手順では、データベースを更新すると *{日付スタンプ}\_Initial.cs*ファイルは実行され、DB のスキーマを作成します。 次に、**シード**DB にテスト データを設定するメソッドが実行されます。

**パッケージ マネージャー コンソール**、コマンドを入力して`update-database`、データベースを作成し、実行、`Seed`メソッド。

![](adding-a-new-field/_static/image7.png)

テーブルが既に存在し、作成することはできませんを示すエラーが発生する場合は可能性があります、データベースを削除した後、および実行する前にアプリケーションを実行するため、`update-database`します。 その場合は、削除、 *Movies.mdf*ファイルをもう一度やり直して、`update-database`コマンド。 依然としてエラーが発生した場合、migrations フォルダーと内容を削除し、このページの上部にある手順を開始 (削除は、 *Movies.mdf*ファイルを Enable-migrations に進みます)。 それでもエラーが発生する場合は、SQL Server オブジェクト エクスプ ローラーを開き、一覧からデータベースを削除します。

アプリケーションを実行しに移動し、 */Movies* URL。 シード データが表示されます。

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

まず、新しい追加`Rating`プロパティを既存の`Movie`クラス。 開く、 *Models\Movie.cs*追加ファイルを開き、`Rating`次のようなプロパティ。

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

完全な`Movie`今のような次のコードをクラスします。

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

(Ctrl + Shift + B) アプリケーションをビルドします。

新しいフィールドを追加したので、`Movie`クラスもする必要があるバインドを更新*ホワイト リスト*この新しいプロパティが含まれるようにします。 更新プログラム、`bind`属性`Create`と`Edit`アクション メソッドに含める、`Rating`プロパティ。

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

ブラウザー ビューで新しい `Rating` プロパティを表示、作成、編集する目的でビュー テンプレートを更新する必要もあります。

開く、 *\Views\Movies\Index.cshtml*追加ファイルを開き、`<th>Rating</th>`直後の列ヘッダー、**価格**列。 追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。 以下はどのような更新*Index.cshtml*ビュー テンプレートのようになります。

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

次に、開く、 *\Views\Movies\Create.cshtml*追加ファイルを開き、`Rating`フィールドには、次のハイライト マークアップ。 これにより、新しいムービーの作成時に、評価を指定できるように、テキスト ボックスを表示します。

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティ。

アプリケーションを実行しに移動し、 */Movies* URL。 これを行うときに、表示されます、次のエラーのいずれか。

![](adding-a-new-field/_static/image9.png)  
  
データベースが作成されたために、'MovieDBContext' コンテキストのバックアップ モデルが変更されました。 データベースを更新する Code First Migrations を使用する (https://go.microsoft.com/fwlink/?LinkId=238269)します。

![](adding-a-new-field/_static/image10.png)

ため、このエラーが表示されている更新された`Movie`アプリケーションでモデル クラスは、スキーマの異なる、`Movie`既存のデータベースのテーブル。 (データベース テーブルに `Rating` 列はありません)。


このエラーを解決するための手法がいくつかあります。

1. Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。 この手法は、開発周期の早い段階で、テスト データベースで開発しているときに非常に便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。 短所は、データベース内の既存のデータが失われる-ようにする*しない*実稼働データベースでこのアプローチを使用する! 初期化子を利用し、データベースにテスト データを自動的に初期投入します。多くの場合、アプリケーション開発の手法として有益な方法です。 Entity Framework データベースの初期化子の詳細については、次を参照してください。 [ASP.NET MVC/Entity Framework チュートリアル](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。
2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。
3. Code First Migrations を使用して、データベース スキーマを更新します。


このチュートリアルでは、Code First Migrations を利用します。

新しい列の値を提供するように、Seed メソッドを更新します。 Migrations \configuration.cs ファイルを開き、ムービーの各オブジェクトに Rating フィールドを追加します。

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

ソリューションをビルドし、開き、**パッケージ マネージャー コンソール**ウィンドウし、次のコマンドを入力します。

`add-migration Rating`

`add-migration`コマンドは現在のムービー DB スキーマを使用して現在のムービー モデルを調べるし、DB を新しいモデルに移行するために必要なコードを作成するために移行フレームワークに指示します。 名前*評価*任意であり、移行ファイルの名前に使用されます。 移行手順のわかりやすい名前を使用することをお勧めします。

このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMigration`クラスを派生し、`Up`メソッド、新しい列を作成するコードを表示できます。

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

ソリューションをビルドし、入力、`update-database`コマンド、**パッケージ マネージャー コンソール**ウィンドウ。

次の図は、出力で、**パッケージ マネージャー コンソール**ウィンドウ (日付スタンプ プリペンド*評価*異なるものになります)。

![](adding-a-new-field/_static/image11.png)

アプリケーションを再実行し、/Movies URL に移動します。 新しい Rating フィールドを確認できます。

![](adding-a-new-field/_static/image12.png)

をクリックして、**新規作成**のリンクを新しいムービーを追加します。 評価を追加できることに注意してください。

![7_CreateRioII](adding-a-new-field/_static/image13.png)

**[作成]** をクリックします。 評価を含む、新しいムービーに表示されるビデオを一覧表示します。

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

これで、プロジェクトは、移行を使用して、新しいフィールドを追加またはそれ以外の場合、スキーマを更新するときに、データベースを削除する必要はありません。 次のセクションではスキーマの変更がお migrations を使用してデータベースを更新します。

追加することも必要があります、`Rating`フィールドを編集、詳細、および Delete ビュー テンプレートにします。

「データベースの更新」のコマンドを入力する可能性があります、**パッケージ マネージャー コンソール**ウィンドウをもう一度と移行コードが実行、スキーマには、モデルが一致するためです。 ただし、「データベースの更新」を実行しているが実行されます、`Seed`メソッドを再度とシード データを変更した場合、変更は失われますので、`Seed`メソッド アップサート データ。 詳細をご覧ください、`Seed`メソッド Tom Dykstra の人気のある[ASP.NET MVC/Entity Framework チュートリアル](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。

このセクションでは、モデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。 シナリオを試すことができますので、サンプル データを使って新しく作成したデータベースを設定する方法も学習しました。 これは、Code First に概要を参照してください[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)サブジェクトに関するより詳細なチュートリアルについてはします。 次に、高度な検証ロジックはモデル クラスを追加し、いくつかのビジネス ルールを適用するを有効にする方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](adding-search.md)
> [次へ](adding-validation.md)
