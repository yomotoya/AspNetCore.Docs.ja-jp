---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Movie モデルとテーブルに新しいフィールドの追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 39b48c67b5264a9b3ad97389f6a5c2bf9d94d25f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576562"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Movie モデルとテーブルに新しいフィールドの追加
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


このセクションでは、変更がデータベースに適用されるため、モデル クラスにいくつかの変更を移行するのに Entity Framework Code First Migrations を使用します。

既定を使用する Entity Framework Code First のデータベースを自動的に作成するように、このチュートリアルで前に行ったときに Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデル クラスと同期されているかどうかを追跡できます。 同期していない、Entity Framework は、エラーをスローします。 これにより、表示されている可能性がありますそれ以外の場合のみ (原因不明のエラー) を実行時に、開発時に問題を追跡しやすくします。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>モデルの変更の Code First Migrations の設定

Visual Studio 2012 を使用している場合をダブルクリックして、 *Movies.mdf*ファイルがソリューション エクスプ ローラーで、データベース ツールを開きます。 Visual Studio の Express for Web はデータベース エクスプ ローラーを表示、サーバー エクスプ ローラーを Visual Studio 2012 が表示されます。 Visual Studio 2010 を使用している場合は、SQL Server オブジェクト エクスプ ローラーを使用します。

(データベース エクスプ ローラー、サーバー エクスプ ローラーまたは SQL Server オブジェクト エクスプ ローラー) は、データベース ツールで右クリック`MovieDBContext`選択**削除**ムービー データベースを削除します。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

ソリューション エクスプ ローラーに移動します。 右クリックして、 *Movies.mdf*ファイルおよび選択**削除**ムービー データベースを削除します。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

エラーがないかどうかを確認するアプリケーションをビルドします。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。

![パックのマニュアルを追加します。](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

**パッケージ マネージャー コンソール**ウィンドウで、`PM>`プロンプト"Enable-migrations ContextTypeName MvcMovie.Models.MovieDBContext"を入力します。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-migrations**コマンド (上記) を作成、 *Configuration.cs*で新しいファイル*移行*フォルダー。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio を開き、 *Configuration.cs*ファイル。 置換、`Seed`メソッドで、 *Configuration.cs*を次のコード ファイル。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

下の赤の波線を右クリックして`Movie`選択**解決**し**を使用して** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

これにより、次を追加ステートメントを使用します。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations を呼び出し、`Seed`すべて移行後のメソッド (呼び出しは、**データベースを更新**パッケージ マネージャー コンソールで)、このメソッドは、既に挿入されている、または場合に、それらを挿入する行を更新し、まだ存在しません。


**CTRL + SHIFT+B プロジェクトをビルドするキーを押します。**(、次の手順が失敗する場合、この時点でビルドはありません)。

次の手順が作成するには、`DbMigration`初回移行のためのクラス。 移行が作成することは、新しいデータベースを削除する、 *movie.mdf*前の手順でファイル。

**パッケージ マネージャー コンソール**ウィンドウで、"add-migration Initial"コマンドを入力して、初期移行を作成します。 「初期」という名前は任意なので名前を移行ファイルを作成するために使用します。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First Migrations は、別のクラス ファイルを作成、*移行*フォルダー (名前を持つ *{日付スタンプ}\_Initial.cs* )、このクラスには、データベース スキーマを作成するコードが含まれています。 移行ファイル名は事前のタイムスタンプを持つ順序付けに関するヘルプを修正しました。 確認、 *{日付スタンプ}\_Initial.cs*ファイル、Movie DB の映画のテーブルを作成する手順があります。 以下、この手順では、データベースを更新すると *{日付スタンプ}\_Initial.cs*ファイルは実行され、DB のスキーマを作成します。 次に、**シード**DB にテスト データを設定するメソッドが実行されます。

**パッケージ マネージャー コンソール**、コマンド「更新プログラム-データベース」、データベースを作成し、実行の入力、**シード**メソッド。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

テーブルが既に存在し、作成することはできませんを示すエラーが発生する場合は可能性があります、データベースを削除した後、および実行する前にアプリケーションを実行するため、`update-database`します。 その場合は、削除、 *Movies.mdf*ファイルをもう一度やり直して、`update-database`コマンド。 依然としてエラーが発生した場合、migrations フォルダーと内容を削除し、このページの上部にある手順を開始 (削除は、 *Movies.mdf*ファイルを Enable-migrations に進みます)。

アプリケーションを実行しに移動し、 */Movies* URL。 シード データが表示されます。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

まず、新しい追加`Rating`プロパティを既存の`Movie`クラス。 開く、 *Models\Movie.cs*追加ファイルを開き、`Rating`次のようなプロパティ。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

完全な`Movie`今のような次のコードをクラスします。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

使用して、アプリケーションをビルド、**ビルド** &gt;**ビルド ムービー**コマンドまたは CTRL-b shift キーを押してメニュー

更新した、`Model`クラスもする必要がある更新、 *\Views\Movies\Index.cshtml*と*\Views\Movies\Create.cshtml*新しいを表示するためにテンプレートを表示`Rating`ブラウザー ビューでのプロパティ。

開く、<em>\Views\Movies\Index.cshtml</em>追加ファイルを開き、`<th>Rating</th>`直後の列ヘッダー、<strong>価格</strong>列。 追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。 以下はどのような更新<em>Index.cshtml</em>ビュー テンプレートのようになります。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

次に、開く、 *\Views\Movies\Create.cshtml*ファイルを開き、フォームの末尾付近の次のマークアップを追加します。 これにより、新しいムービーの作成時に、評価を指定できるように、テキスト ボックスを表示します。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティ。

これで、アプリケーションを実行しに移動し、 */Movies* URL。 これを行うときに、表示されます、次のエラーのいずれか。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

ため、このエラーが表示されている更新された`Movie`アプリケーションでモデル クラスは、スキーマの異なる、`Movie`既存のデータベースのテーブル。 (データベース テーブルに `Rating` 列はありません)。


このエラーを解決するための手法がいくつかあります。

1. Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。 テスト データベースでアクティブな開発を行うときに、このアプローチは非常に便利簡単にまとめてモデルとデータベース スキーマを進化させることができます。 短所は、データベース内の既存のデータが失われる-ようにする*しない*実稼働データベースでこのアプローチを使用する! データベースにテスト データを自動的にシード初期化子を使用するは、多くの場合、アプリケーションを開発する生産性の高い方法です。 Entity Framework データベースの初期化子の詳細については、Tom Dykstra を参照してください。 [ASP.NET MVC/Entity Framework チュートリアル](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。
2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。
3. Code First Migrations を使用して、データベース スキーマを更新します。


このチュートリアルでは、Code First Migrations を利用します。

新しい列の値を提供するように、Seed メソッドを更新します。 Migrations \configuration.cs ファイルを開き、ムービーの各オブジェクトに Rating フィールドを追加します。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

ソリューションをビルドし、開き、**パッケージ マネージャー コンソール**ウィンドウし、次のコマンドを入力します。

`add-migration AddRatingMig`

`add-migration`コマンドは現在のムービー DB スキーマを使用して現在のムービー モデルを調べるし、DB を新しいモデルに移行するために必要なコードを作成するために移行フレームワークに指示します。 AddRatingMig は任意なので移行ファイルの名前を使用します。 移行手順のわかりやすい名前を使用することをお勧めします。

このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMIgration`クラスを派生し、`Up`メソッド、新しい列を作成するコードを表示できます。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

ソリューションをビルドし、「データベースの更新」のコマンドを入力、**パッケージ マネージャー コンソール**ウィンドウ。

次の図は、出力で、**パッケージ マネージャー コンソール**ウィンドウ (プリペンド AddRatingMig 日付スタンプが異なるあります)。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

アプリケーションを再実行し、/Movies URL に移動します。 新しい Rating フィールドを確認できます。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

をクリックして、**新規作成**のリンクを新しいムービーを追加します。 評価を追加できることに注意してください。

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

**[作成]** をクリックします。 評価を含む、新しいムービーに表示されるビデオを一覧表示します。

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

追加することも必要があります、`Rating`テンプレートの表示、編集、詳細、SearchIndex するフィールドします。

「データベースの更新」のコマンドを入力する可能性があります、**パッケージ マネージャー コンソール**ウィンドウをもう一度と変更なしになります、スキーマには、モデルが一致するためです。

このセクションでは、モデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。 シナリオを試すことができますので、サンプル データを使って新しく作成したデータベースを設定する方法も学習しました。 次に、高度な検証ロジックはモデル クラスを追加し、いくつかのビジネス ルールを適用するを有効にする方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-validation-to-the-model.md)
