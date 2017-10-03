`Movie` クラスに次のプロパティを追加します。

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` フィールドは、データベースで主キー用に必要です。

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>データベース コンテキスト クラスの追加

*MovieContext.cs* という `DbContext`派生クラスを *Models* フォルダーに追加します。

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

上記のコードによって、エンティティ セットの `DbSet` プロパティが作成されます。 Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応し、エンティティはテーブルの行に対応します。 `DbSet` プロパティ名が `Movies` です。 データベースでは単数形の名前が使用されるので、このサンプルは `OnModelCreating` をオーバーライドし、テーブル名に単数形 (`Movie`) を使用します。
