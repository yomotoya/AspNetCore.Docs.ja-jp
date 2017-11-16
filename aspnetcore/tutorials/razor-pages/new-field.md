---
title: "Razor ページへの新しいフィールドの追加"
author: rick-anderson
description: "Entity Framework Core を使用した Razor ページへの新しいフィールドの追加方法"
keywords: "ASP.NET Core,Entity Framework Core,移行"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 128b69513976a56104524bb803f2b8cb1daf1967
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a>Razor ページへの新しいフィールドの追加

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このセクションでは、[Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations を利用し、新しいフィールドをモデルに追加し、その変更をデータベースに移行します。

EF Code First を利用してデータベースを自動作成すると、Code First はテーブルをデータベースに追加し、データベースのスキーマが生成元のモデル クラスと同期しているか追跡します。 同期していない場合、EF は例外をスローします。 一貫性のないデータベース/コードの問題を簡単に見つけられます。

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

アプリをビルドします (Ctrl+Shift+B)。

*Pages/Movies/Index.cshtml* を編集し、`Rating` フィールドを追加します。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

[削除] と [詳細] ページに、`Rating` フィールドを追加します。

*Create.cshtml* を `Rating` フィールドを使用して更新します。 前の `<div>` 要素をコピー/貼り付けし、intelliSense にフィールドを更新させることができます。 IntelliSense は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)と連動します。

![開発者は、ビューの 2 番目のラベル要素で、asp-for の属性値に文字 R を入力しました。 Intellisense のコンテキスト メニューが表示され、[評価] を含む、利用可能なフィールドが表示されました。[評価] は一覧の中で自動的に強調表示されています。 開発者はこのフィールドをクリックするか、キーボードの Enter を押すと、値が [評価] に設定されます。](new-field/_static/cr.png)

次に、`Rating` フィールドがある *Create.cshtml* のコードを示します。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

[編集] ページに、`Rating` フィールドを追加します。

DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。 ここで実行すると、アプリによって `SqlException` がスローされます。

```
SqlException: Invalid column name 'Rating'.
```

このエラーが表示されるのは、更新された Movie モデル クラスがデータベースの Movie テーブルのスキーマと異なるためです。 (データベース テーブルに `Rating` 列はありません)。

このエラーを解決するための手法がいくつかあります。

1. Entity Framework に、新しいモデル クラス スキーマを使用してデータベースを自動的にドロップさせ、再作成させます。 この手法は、開発周期の早い段階で便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。 これの欠点は、データベースで既存のデータが失われることです。 実稼働データベースでこの方法は使用したくないでしょう。 スキーマの変更時に DB を削除し、初期化子を使用して、データベースにテスト データを自動的にシードすることは、多くの場合、アプリケーションを開発する上で有益な方法となります。

2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。

3. Code First Migrations を使用して、データベース スキーマを更新します。

このチュートリアルでは、Code First Migrations を利用します。

新しい列に値を提供するように、`SeedData` クラスを更新します。 下に変更のサンプルがありますが、`new Movie` ブロックごとにこの変更を行ってください。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

[完成した SeedData.cs ファイル](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs)を参照してください。

ソリューションをビルドします。

<a name="pmc"></a> **[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。
PMC で、次のコマンドを入力します。

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` コマンドにより、フレームワークに次の指示があります。

* `Movie` モデルを、`Movie` DB のスキーマと比較します。
* DB スキーマを新しいモデルに移行するコードを作成します。

"Rating (評価)" という名前は任意です。移行ファイルに名前を付けるために利用されます。 移行ファイルには意味のある名前を使用すると便利です。

<a name="ssox"></a> DB 内のすべてのレコードを削除すると、初期化子は DB にデータを初期投入し、`Rating` フィールドを追加します。 これはブラウザーの削除リンクで行うか、[Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) から行うことができます。 SSOX からデータベースを削除するには:

* SSOX でデータベースを選択します。
* データベースを右クリックし、*[削除]* を選択します。
* **[既存の接続を閉じる]** をチェックします。
* **[OK]** を選択します。
* [PMC](xref:tutorials/razor-pages/new-field#pmc) でデータベースを更新します。

  ```powershell
  Update-Database
  ```

アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。 データベースがシードされていない場合は IIS Express を停止し、アプリを実行します。

>[!div class="step-by-step"]
[前: 検索の追加](xref:tutorials/razor-pages/search)
[次: 検証の追加](xref:tutorials/razor-pages/validation)
