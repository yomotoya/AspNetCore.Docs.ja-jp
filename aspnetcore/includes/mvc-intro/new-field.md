# <a name="adding-a-new-field"></a>新しいフィールドの追加

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、`Movies` テーブルに新しいフィールドを追加します。 ここでは、スキーマを変更する (新しいフィールドを追加する) 際にデータベースをドロップし、新しいデータベースを作成します。 このワークフローは、保存する実稼働データがない開発の早い段階に適しています。

アプリが配置され、保存する必要があるデータが存在する状態で、スキーマを変更する必要がある場合は DB をドロップすることはできません。 Entity Framework [Code First Migrations](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) では、スキーマを更新し、データを失うことなくデータベースを移行できます。 Migrations は SQL Server でよく使用される機能ですが、SQLlite では多くの移行スキーマ操作がサポートされないため、実行できるのはごく簡単な移行のみとなります。 詳細については、[SQLite の制限事項](https://docs.microsoft.com/ef/core/providers/sqlite/limitations)に関するページを参照してください。

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

新しいフィールドを `Movie` クラスに追加したため、この新しいプロパティが含まれるように、拘束力のあるホワイトリストを更新する必要もあります。 *MoviesController.cs* で、アクション メソッドの `Create` と `Edit` の両方の `[Bind]` 属性を更新し、`Rating` プロパティが含まれるようにします。

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

ブラウザー ビューで新しい `Rating` プロパティを表示、作成、編集するためにビュー テンプレートを更新する必要もあります。

*/Views/Movies/Index.cshtml* ファイルを編集し、`Rating` フィールドを追加します。

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

*/Views/Movies/Create.cshtml* を `Rating` フィールドで更新します。

DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。 ここですぐに実行すると、次の `SqliteException` が表示されます。

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

このエラーが表示されるのは、更新された Movie モデル クラスが既存のデータベースの Movie テーブルのスキーマと異なるためです  (データベース テーブルに `Rating` 列はありません)。

このエラーを解決するための手法がいくつかあります。

1. データベースをドロップし、Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的に再作成させます。 この手法では、データベース内の既存のデータが失われるため、実稼働データベースでは使用できません。 初期化子を使用して、データベースにテスト データを自動的にシードすることは、多くの場合、アプリケーションを開発する上で有益な方法となります。

2. モデル クラスと一致するように、既存のデータベースのスキーマを手動で変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。

3. Code First Migrations を使用して、データベース スキーマを更新します。

このチュートリアルでは、スキーマの変更時にデータベースをドロップして再作成します。 端末から次のコマンドを実行して、db をドロップします。

`dotnet ef database drop`

新しい列に値を提供するように、`SeedData` クラスを更新します。 下に変更のサンプルがありますが、`new Movie` ごとにこの変更を行ってください。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

`Rating` フィールドを `Edit`、`Details`、および `Delete` ビューに追加します。

アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。 テンプレート。