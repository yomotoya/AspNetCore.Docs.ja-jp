---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: ムービーのモデルとテーブルに新しいフィールドを追加する |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>ムービーのモデルとテーブルに新しいフィールドを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


このセクションでは、データベースに、変更が適用されるため、モデル クラスにいくつかの変更を移行するのに Entity Framework Code First Migrations を使用します。

既定を使用して Entity Framework Code First を自動的に、データベースの作成と同様、このチュートリアルで前に Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデルのクラスと同期されているかどうかを追跡できます。 同期そうでない場合、Entity Framework は、エラーをスローします。 これにより、それ以外の場合のみがあります (あいまいなエラー) を実行時に開発時に問題を追跡しやすくします。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>モデルの変更の Code First Migrations を設定します。

Visual Studio 2012 を使用している場合をダブルクリックして、 *Movies.mdf*ソリューション エクスプ ローラーを開くには、データベース ツールからファイルです。 Visual Studio の Express for Web が表示されますデータベース エクスプ ローラーで、Visual Studio 2012 サーバー エクスプ ローラーが表示されます。 Visual Studio 2010 を使用している場合は、SQL Server オブジェクト エクスプ ローラーを使用します。

ツールでは、データベース (データベース エクスプ ローラー、サーバー エクスプ ローラーまたは SQL Server オブジェクト エクスプ ローラー) を右クリックして`MovieDBContext`選択**削除**映画データベースを削除します。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

ソリューション エクスプ ローラーに移動します。 右クリックして、 *Movies.mdf*ファイルおよび選択した**削除**映画データベースを削除します。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

アプリケーションをビルドしてエラーがないことを確認します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**Package Manager Console**です。

![パックのマニュアルを追加します。](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

**Package Manager Console**ウィンドウで、`PM>`プロンプト"Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext"を入力してください。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-migrations**コマンド (上記) を作成、*される Configuration.cs*を新しいファイル*移行*フォルダーです。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio を開き、*される Configuration.cs*ファイル。 置換、`Seed`メソッドで、*される Configuration.cs*を次のコード ファイル。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

下の赤い波線を右クリックして`Movie`選択**を解決する**し**を使用して** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

これにより、次を追加ステートメントを使用します。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations 呼び出し、`Seed`メソッドすべての移行した後に (つまり、呼び出し**データベースを更新**パッケージ マネージャー コンソールで)、このメソッドは既に挿入されている、または場合に、それらを挿入する行を更新し、まだ存在していません。


**プロジェクトをビルドするには CTRL-shift キーを押し-B を押します。**(場合に、次の手順は失敗、この時点で構築しない)。

次の手順が作成するには、`DbMigration`初期の移行のためのクラスです。 移行を作成することは、新しいデータベースを削除する、 *movie.mdf*前の手順でのファイルです。

**Package Manager Console**ウィンドウで、「追加移行初期」コマンドを入力して初期の移行を作成します。 名前「初期」は任意され、作成した移行ファイルの名前に使用されます。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First Migrations で別のクラス ファイルの作成、*移行*フォルダー (名前を持つ*{日付スタンプ}\_Initial.cs* )、このクラスには、データベース スキーマを作成するコードが含まれています。 移行ファイル名は、順序付けに役立てるために事前、タイムスタンプを持つ固定されています。 確認、 *{日付スタンプ}\_Initial.cs*ファイル、ムービー DB の映画のテーブルを作成する手順があります。 以下、この手順では、データベースを更新すると*{日付スタンプ}\_Initial.cs*ファイルが実行され、データベースのスキーマを作成します。 続いて、**シード**DB にテスト データを設定するメソッドが実行されます。

**Package Manager Console**、コマンド"更新プログラム-データベースを入力します"、データベースを作成および実行する、**シード**メソッドです。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

テーブルは既に存在し、作成することはできませんを示すエラーが発生すると考えられますが、データベースを削除した後、および実行する前に、アプリケーションが実行された`update-database`です。 その場合は、削除、 *Movies.mdf*ファイルをもう一度やり直して、`update-database`コマンド。 依然としてエラーが発生した場合に、migrations フォルダーと内容を削除し、このページの上部にある手順を開始 (削除である、 *Movies.mdf*ファイル、Enable-migrations に進みます)。

アプリケーションを実行しに移動し、 */Movies* URL。 シード データが表示されます。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

新しく追加することによって開始`Rating`を既存のプロパティ`Movie`クラスです。 開く、 *Models\Movie.cs*ファイルを追加、`Rating`次のようなプロパティ。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

完全な`Movie`クラスの現在のような次のコード。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

使用して、アプリケーションをビルド、**ビルド** &gt;**作成の映画**メニュー コマンドまたは ctrl キーを押し-b shift キーを押すとして

これで、更新した、`Model`クラスも更新する必要が、 *\Views\Movies\Index.cshtml*と*\Views\Movies\Create.cshtml*新しいを表示するためにテンプレートの表示`Rating`ブラウザー ビューのプロパティです。

開く、<em>\Views\Movies\Index.cshtml</em>ファイルを追加、`<th>Rating</th>`列見出し直後、<strong>価格</strong>列です。 追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。 以下はどのような更新<em>Index.cshtml</em>ビュー テンプレートはようになります。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

次に、開く、 *\Views\Movies\Create.cshtml*ファイルおよびフォームの末尾付近の次のマークアップを追加します。 これにより、評価を指定するには、新しいムービーの作成時にできるように、テキスト ボックスが表示します。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティです。

これで、アプリケーションを実行しに移動し、 */Movies* URL。 これを行うときにが表示されます、次のエラーのいずれか。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

このエラーが表示されている、更新された`Movie`アプリケーションのモデル クラスがのスキーマと異なるようになりました、`Movie`既存のデータベースのテーブルです。 (データベース テーブルに `Rating` 列はありません)。


このエラーを解決するための手法がいくつかあります。

1. Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。 テスト データベース; 上のアクティブな開発を行うときに、この方法は非常に便利簡単にモデルとデータベースのスキーマを一緒に発展させることができます。 、欠点は、データベース内の既存のデータが失われること、— ようにする*しない*を実稼働データベースでこの方法を使用します。 初期化子を使用してデータベースにテスト データを自動的にシードするは、多くの場合、アプリケーションを開発する生産性の高い方法です。 Entity Framework データベースの初期化子の詳細については、Tom Dykstra を参照してください。 [ASP.NET MVC/Entity Framework チュートリアル](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。
2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。
3. Code First Migrations を使用して、データベース スキーマを更新します。


このチュートリアルでは、Code First Migrations を利用します。

新しい列の値を提供できるようにシード メソッドを更新します。 Migrations\Configuration.cs ファイルを開き、ムービーの各オブジェクトに格納された Rating フィールドを追加します。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

ソリューションをビルドし、開きます、**パッケージ マネージャー コンソール**ウィンドウと、次のコマンドを入力します。

`add-migration AddRatingMig`

`add-migration`コマンドを現在のムービー DB スキーマと、現在のムービー モデルを調査して、新しいモデルにデータベースを移行するために必要なコードを作成する移行フレームワークに指示します。 AddRatingMig は任意し、移行ファイルの名前に使用されます。 移行手順のわかりやすい名前を使用することをお勧めします。

このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`クラスを派生し、、`Up`方法、新しい列を作成するコードを確認することができます。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

ソリューションをビルドし、コマンドを入力して、「データベースの更新」で、 **Package Manager Console**ウィンドウです。

次の図での出力、 **Package Manager Console**ウィンドウ (AddRatingMig を付加すること、日付スタンプされるが異なります)。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

アプリケーションを再実行し、/Movies の URL に移動します。 新しい評価 フィールドを表示できます。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

をクリックして、**新規作成**リンクを新しいムービーを追加します。 評価を追加できることに注意してください。

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

**[作成]**をクリックします。 この評価を含む、新しいムービーに表示されます、映画を一覧表示します。

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

追加することも必要があります、`Rating`テンプレートの表示、編集、詳細 SearchIndex するフィールドです。

「データベースの更新」コマンドを入力する可能性があります、 **Package Manager Console**ウィンドウをもう一度と変更はありません行われる、スキーマが、モデルと一致するためです。

このセクションではモデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。 また、シナリオを実行するためのサンプル データ、新しく作成されたデータベースに設定する方法も学習しました。 次に、モデル クラスをより詳細な検証ロジックを追加し、適用するビジネス ルールの一部を有効にする方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-validation-to-the-model.md)
