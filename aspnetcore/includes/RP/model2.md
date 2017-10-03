<span data-ttu-id="e80ec-101">`Movie` クラスに次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e80ec-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="e80ec-102">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="e80ec-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="e80ec-103">データベース コンテキスト クラスの追加</span><span class="sxs-lookup"><span data-stu-id="e80ec-103">Add a database context class</span></span>

<span data-ttu-id="e80ec-104">*MovieContext.cs* という `DbContext`派生クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="e80ec-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

<span data-ttu-id="e80ec-105">上記のコードによって、エンティティ セットの `DbSet` プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e80ec-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="e80ec-106">Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応し、エンティティはテーブルの行に対応します。</span><span class="sxs-lookup"><span data-stu-id="e80ec-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="e80ec-107">`DbSet` プロパティ名が `Movies` です。</span><span class="sxs-lookup"><span data-stu-id="e80ec-107">The `DbSet` property name is `Movies`.</span></span> <span data-ttu-id="e80ec-108">データベースでは単数形の名前が使用されるので、このサンプルは `OnModelCreating` をオーバーライドし、テーブル名に単数形 (`Movie`) を使用します。</span><span class="sxs-lookup"><span data-stu-id="e80ec-108">Since the database uses singular names, the sample overrides `OnModelCreating` to use the singular form (`Movie`) for the table name.</span></span>
