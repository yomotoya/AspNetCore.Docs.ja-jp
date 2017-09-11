<span data-ttu-id="d9cb8-101">`Movie` クラスに次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d9cb8-101">Add the following properties to the `Movie` class:</span></span>

<span data-ttu-id="d9cb8-102">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]</span><span class="sxs-lookup"><span data-stu-id="d9cb8-102">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]</span></span>

<span data-ttu-id="d9cb8-103">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="d9cb8-103">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="d9cb8-104">データベース コンテキスト クラスの追加</span><span class="sxs-lookup"><span data-stu-id="d9cb8-104">Add a database context class</span></span>

<span data-ttu-id="d9cb8-105">*MovieContext.cs* という `DbContext`派生クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="d9cb8-105">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

<span data-ttu-id="d9cb8-106">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="d9cb8-106">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="d9cb8-107">上記のコードによって、エンティティ セットの `DbSet` プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d9cb8-107">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="d9cb8-108">Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応し、エンティティはテーブルの行に対応します。</span><span class="sxs-lookup"><span data-stu-id="d9cb8-108">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>