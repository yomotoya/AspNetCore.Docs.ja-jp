
## <a name="test-the-app"></a><span data-ttu-id="512e8-101">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="512e8-101">Test the app</span></span>

* <span data-ttu-id="512e8-102">アプリを実行し、**[Mvc ムービー]** リンクをタップします。</span><span class="sxs-lookup"><span data-stu-id="512e8-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="512e8-103">**[新規作成]** リンクをタップし、ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="512e8-103">Tap the **Create New** link and create a movie.</span></span>

  ![ジャンル、価格、リリース日、タイトルのフィールドを持つビューを作成します。](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="512e8-105">`Price` フィールドに小数点またはコンマを入力することはできない場合があります。</span><span class="sxs-lookup"><span data-stu-id="512e8-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="512e8-106">小数点にコンマ (",") を使用し、英語 (米国) 以外の日付形式を使用する英語以外のロケールの [jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="512e8-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="512e8-107">詳細については、https://github.com/aspnet/Docs/issues/4076 および [その他のリソース](#additional-resources)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="512e8-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="512e8-108">ここでは、単に 10 のような整数を入力します。</span><span class="sxs-lookup"><span data-stu-id="512e8-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="512e8-109">いくつかのロケールでは、日付形式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="512e8-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="512e8-110">次の強調表示されたコードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="512e8-110">See the highlighted code below.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="512e8-111">`DataAnnotations` についてはチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="512e8-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="512e8-112">**[作成]** をタップすると、ムービー情報がデータベースに保存されているサーバーにフォームが送信されます。</span><span class="sxs-lookup"><span data-stu-id="512e8-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="512e8-113">アプリは、*/Movies* URL にリダイレクトし、新しく作成したムービー情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="512e8-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![新しく作成したムービーの一覧を表示したムービー ビュー](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="512e8-115">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="512e8-115">Create a couple more movie entries.</span></span> <span data-ttu-id="512e8-116">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="512e8-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
