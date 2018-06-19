---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: 新しいフィールドを追加する |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874185"
---
<a name="adding-a-new-field"></a>新しいフィールドの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションでは、データベースに、変更が適用されるため、モデル クラスにいくつかの変更を移行するのに Entity Framework Code First Migrations を使用します。

既定を使用して Entity Framework Code First を自動的に、データベースの作成と同様、このチュートリアルで前に Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデルのクラスと同期されているかどうかを追跡できます。 同期そうでない場合、Entity Framework は、エラーをスローします。 これにより、それ以外の場合のみがあります (あいまいなエラー) を実行時に開発時に問題を追跡しやすくします。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>モデルの変更の Code First Migrations を設定します。

ソリューション エクスプ ローラーに移動します。 右クリックして、 *Movies.mdf*ファイルおよび選択した**削除**映画データベースを削除します。 表示されない場合、 *Movies.mdf*ファイルをクリックして、 **すべてのファイル**赤い枠で下に表示されるアイコン。

![](adding-a-new-field/_static/image1.png)

アプリケーションをビルドしてエラーがないことを確認します。

**ツール** メニューのをクリックして**NuGet Package Manager**し**Package Manager Console**です。

![パックのマニュアルを追加します。](adding-a-new-field/_static/image2.png)

**Package Manager Console**ウィンドウで、`PM>`プロンプトを入力してください

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-migrations**コマンド (上記) を作成、*される Configuration.cs*を新しいファイル*移行*フォルダーです。

![](adding-a-new-field/_static/image4.png)

Visual Studio を開き、*される Configuration.cs*ファイル。 置換、`Seed`メソッドで、*される Configuration.cs*を次のコード ファイル。

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

下の赤い波線ポインターを合わせる`Movie` をクリック`Show Potential Fixes` をクリックし、**を使用して** **MvcMovie.Models です。**

![](adding-a-new-field/_static/image5.png)

これにより、次を追加ステートメントを使用します。

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First Migrations 呼び出し、`Seed`メソッドすべての移行した後に (つまり、呼び出し**データベースを更新**パッケージ マネージャー コンソールで)、このメソッドは既に挿入されている、または場合に、それらを挿入する行を更新し、まだ存在していません。
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)次のコードにメソッド"upsert"操作を実行します。
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> [シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドは、すべての移行が実行され、追加しようとしている行が既にありますデータベースを作成する最初の移行後にあるため、データを挿入することはできませんだけです。 "[Upsert](http://en.wikipedia.org/wiki/Upsert)"操作が既に存在する行を挿入しようとする場合に発生するとエラーを防ぐことが、アプリケーションのテスト中に対して行ったデータに対する変更を上書きします。 いくつかのテーブルでのテスト データを使用しない場合発生すること。 場合によってはテスト中にデータを変更すると、変更するデータベースの更新後に残します。 条件付きの挿入操作を実行する場合: 存在しない場合にのみ行を挿入します。   
> 
> 渡される最初のパラメーター、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドが使用して、行が既に存在するかどうかを確認するプロパティを指定します。 次の情報を提供して、テストのムービー データの`Title`一意では、リスト内の各タイトルために、この目的のプロパティを使用できます。
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> このコードでは、タイトルが一意であることを前提としています。 タイトルの重複を手動で追加する場合は、次回の移行を実行する次の例外が表示されます。   
> 
>  *シーケンスには、複数の要素が含まれています。*  
> 
> 詳細については、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドを参照してください[EF 4.3 AddOrUpdate メソッドを使用して注意](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).


**プロジェクトをビルドするには CTRL-shift キーを押し-B を押します。**(次の手順は失敗しますこの時点で構築しない場合。

次の手順が作成するには、`DbMigration`初期の移行のためのクラスです。 この移行を作成することは、新しいデータベースを削除する、 *movie.mdf*前の手順でのファイルです。

**Package Manager Console**ウィンドウ、コマンドを入力して`add-migration Initial`初期の移行を作成します。 名前「初期」は任意され、作成した移行ファイルの名前に使用されます。

![](adding-a-new-field/_static/image6.png)

Code First Migrations で別のクラス ファイルの作成、*移行*フォルダー (名前を持つ *{日付スタンプ}\_Initial.cs* )、このクラスには、データベース スキーマを作成するコードが含まれています。 移行ファイル名は、順序付けに役立てるために事前、タイムスタンプを持つ固定されています。 確認、 *{日付スタンプ}\_Initial.cs*ファイルを作成する手順がある、`Movies`ムービー DB のテーブルです。 以下、この手順では、データベースを更新すると *{日付スタンプ}\_Initial.cs*ファイルが実行され、データベースのスキーマを作成します。 続いて、**シード**DB にテスト データを設定するメソッドが実行されます。

**Package Manager Console**、コマンドを入力して`update-database`データベースを作成および実行する、`Seed`メソッドです。

![](adding-a-new-field/_static/image7.png)

テーブルは既に存在し、作成することはできませんを示すエラーが発生すると考えられますが、データベースを削除した後、および実行する前に、アプリケーションが実行された`update-database`です。 その場合は、削除、 *Movies.mdf*ファイルをもう一度やり直して、`update-database`コマンド。 依然としてエラーが発生した場合に、migrations フォルダーと内容を削除し、このページの上部にある手順を開始 (削除である、 *Movies.mdf*ファイル、Enable-migrations に進みます)。 引き続き、eror を取得する場合は、SQL Server オブジェクト エクスプ ローラーを開き、一覧からデータベースを削除します。

アプリケーションを実行しに移動し、 */Movies* URL。 シード データが表示されます。

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

新しく追加することによって開始`Rating`を既存のプロパティ`Movie`クラスです。 開く、 *Models\Movie.cs*ファイルを追加、`Rating`次のようなプロパティ。

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

完全な`Movie`クラスの現在のような次のコード。

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

アプリケーションをビルドします (Ctrl + Shift + B)。

新しいフィールドを追加したので、`Movie`クラスも更新する必要がバインディング*ホワイト リスト*のため、この新しいプロパティが含まれます。 更新プログラム、`bind`属性`Create`と`Edit`アクション メソッドに含める、`Rating`プロパティ。

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

ブラウザー ビューで新しい `Rating` プロパティを表示、作成、編集する目的でビュー テンプレートを更新する必要もあります。

開く、 *\Views\Movies\Index.cshtml*ファイルを追加、`<th>Rating</th>`列見出し直後、**価格**列です。 追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。 以下はどのような更新*Index.cshtml*ビュー テンプレートはようになります。

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

次に、開く、 *\Views\Movies\Create.cshtml*ファイルを追加、`Rating`フィールドに、次のハイライト マークアップ。 これにより、評価を指定するには、新しいムービーの作成時にできるように、テキスト ボックスが表示します。

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティです。

アプリケーションを実行しに移動し、 */Movies* URL。 これを行うときにが表示されます、次のエラーのいずれか。

![](adding-a-new-field/_static/image9.png)  
  
データベースが作成されたために、'MovieDBContext' コンテキストのバックアップ モデルが変更されました。 Code First Migrations を使用して、データベースを更新するを検討してください (https://go.microsoft.com/fwlink/?LinkId=238269)です。

![](adding-a-new-field/_static/image10.png)

このエラーが表示されている、更新された`Movie`アプリケーションのモデル クラスがのスキーマと異なるようになりました、`Movie`既存のデータベースのテーブルです。 (データベース テーブルに `Rating` 列はありません)。


このエラーを解決するための手法がいくつかあります。

1. Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。 この手法は、開発周期の早い段階で、テスト データベースで開発しているときに非常に便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。 、欠点は、データベース内の既存のデータが失われること、— ようにする*しない*を実稼働データベースでこの方法を使用します。 初期化子を利用し、データベースにテスト データを自動的に初期投入します。多くの場合、アプリケーション開発の手法として有益な方法です。 Entity Framework データベースの初期化子の詳細については、次を参照してください。 [ASP.NET MVC/Entity Framework チュートリアル](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。
2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。
3. Code First Migrations を使用して、データベース スキーマを更新します。


このチュートリアルでは、Code First Migrations を利用します。

新しい列の値を提供できるようにシード メソッドを更新します。 Migrations\Configuration.cs ファイルを開き、ムービーの各オブジェクトに格納された Rating フィールドを追加します。

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

ソリューションをビルドし、開きます、**パッケージ マネージャー コンソール**ウィンドウと、次のコマンドを入力します。

`add-migration Rating`

`add-migration`コマンドを現在のムービー DB スキーマと、現在のムービー モデルを調査して、新しいモデルにデータベースを移行するために必要なコードを作成する移行フレームワークに指示します。 名前*評価*任意あり、移行ファイルの名前を使用します。 移行手順のわかりやすい名前を使用することをお勧めします。

このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMigration`クラスを派生し、、`Up`方法、新しい列を作成するコードを確認することができます。

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

ソリューションをビルドし、入力、`update-database`コマンドを**Package Manager Console**ウィンドウです。

出力を次の図に示します、**パッケージ マネージャー コンソール**ウィンドウ (日付スタンプ付加*評価*異なるになります)。

![](adding-a-new-field/_static/image11.png)

アプリケーションを再実行し、/Movies の URL に移動します。 新しい評価 フィールドを表示できます。

![](adding-a-new-field/_static/image12.png)

をクリックして、**新規作成**リンクを新しいムービーを追加します。 評価を追加できることに注意してください。

![7_CreateRioII](adding-a-new-field/_static/image13.png)

**[作成]** をクリックします。 この評価を含む、新しいムービーに表示されます、映画を一覧表示します。

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

これで、プロジェクトは、migrations を使用して、新しいフィールドを追加またはそれ以外の場合、スキーマを更新するときに、データベースを削除する必要はありません。 次のセクションでおを複数のスキーマ変更を使用して移行をデータベースを更新します。

追加することも必要があります、`Rating`フィールドを編集、詳細、および Delete テンプレートの表示にします。

「データベースの更新」コマンドを入力する可能性があります、 **Package Manager Console**ウィンドウをもう一度移行コードがないと実行およびスキーマが、モデルと一致するためです。 ただし、「データベースの更新」を実行しているが実行されます、`Seed`メソッドを再度、およびシードのデータを変更した場合、変更は失われますので、`Seed`メソッド アップサート データ。 に関する詳細を読み取ることができます、`Seed`メソッドに Tom Dykstra 人気のある[ASP.NET MVC/Entity Framework チュートリアル](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。

このセクションではモデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。 また、シナリオを実行するためのサンプル データ、新しく作成されたデータベースに設定する方法も学習しました。 これが Code First に概要を参照してください[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)サブジェクトに関するより詳細なチュートリアルについてはします。 次に、モデル クラスをより詳細な検証ロジックを追加し、適用するビジネス ルールの一部を有効にする方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](adding-search.md)
> [次へ](adding-validation.md)
