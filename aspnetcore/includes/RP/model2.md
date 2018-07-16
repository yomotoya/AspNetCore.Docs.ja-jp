<span data-ttu-id="86050-101">`Movie` クラスに次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="86050-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="86050-102">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="86050-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="86050-103">データベース コンテキスト クラスの追加</span><span class="sxs-lookup"><span data-stu-id="86050-103">Add a database context class</span></span>

<span data-ttu-id="86050-104">次の *MovieContext.cs* クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="86050-104">Add the following *MovieContext.cs* class to the *Models* folder:</span></span>  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="86050-105">上記のコードによって、エンティティ セットの `DbSet` プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="86050-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="86050-106">Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応し、エンティティはテーブルの行に対応します。</span><span class="sxs-lookup"><span data-stu-id="86050-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
