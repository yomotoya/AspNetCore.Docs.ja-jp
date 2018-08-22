---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: 属性ルーティングで ASP.NET Web API 2 で REST API の作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: da3ca8f89f823fcb2c4ab74af6ddf4f61d4e663a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825947"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="d61dc-102">ASP.NET Web API 2 で属性ルーティングで REST API を作成します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d61dc-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d61dc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d61dc-104">新しい型をサポートする web API 2 のルーティングと呼ばれる*属性ルーティング*します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="d61dc-105">属性ルーティングの一般的な概要については、次を参照してください。[属性は、Web API 2 でルーティング](attribute-routing-in-web-api-2.md)します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="d61dc-106">このチュートリアルではルーティングを使用して属性のブックの「コレクションの REST API を作成します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="d61dc-107">API は、次の操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-107">The API will support the following actions:</span></span>

| <span data-ttu-id="d61dc-108">アクション</span><span class="sxs-lookup"><span data-stu-id="d61dc-108">Action</span></span> | <span data-ttu-id="d61dc-109">URI の例</span><span class="sxs-lookup"><span data-stu-id="d61dc-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="d61dc-110">すべての書籍の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-110">Get a list of all books.</span></span> | <span data-ttu-id="d61dc-111">/api/books</span><span class="sxs-lookup"><span data-stu-id="d61dc-111">/api/books</span></span> |
| <span data-ttu-id="d61dc-112">ID での書籍を入手します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-112">Get a book by ID.</span></span> | <span data-ttu-id="d61dc-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="d61dc-113">/api/books/1</span></span> |
| <span data-ttu-id="d61dc-114">書籍の詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-114">Get the details of a book.</span></span> | <span data-ttu-id="d61dc-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="d61dc-115">/api/books/1/details</span></span> |
| <span data-ttu-id="d61dc-116">ジャンルによる書籍の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-116">Get a list of books by genre.</span></span> | <span data-ttu-id="d61dc-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="d61dc-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="d61dc-118">発行日によって、書籍の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="d61dc-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span><span class="sxs-lookup"><span data-stu-id="d61dc-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="d61dc-120">特定の作成者によって書籍の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="d61dc-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="d61dc-121">/api/authors/1/books</span></span> |

<span data-ttu-id="d61dc-122">すべてのメソッドは、読み取り専用 (HTTP GET 要求) です。</span><span class="sxs-lookup"><span data-stu-id="d61dc-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="d61dc-123">データ層に Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="d61dc-124">書籍のレコードには、次のフィールドがあります。</span><span class="sxs-lookup"><span data-stu-id="d61dc-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="d61dc-125">ID</span><span class="sxs-lookup"><span data-stu-id="d61dc-125">ID</span></span>
- <span data-ttu-id="d61dc-126">Title</span><span class="sxs-lookup"><span data-stu-id="d61dc-126">Title</span></span>
- <span data-ttu-id="d61dc-127">ジャンル</span><span class="sxs-lookup"><span data-stu-id="d61dc-127">Genre</span></span>
- <span data-ttu-id="d61dc-128">発行日</span><span class="sxs-lookup"><span data-stu-id="d61dc-128">Publication date</span></span>
- <span data-ttu-id="d61dc-129">価格</span><span class="sxs-lookup"><span data-stu-id="d61dc-129">Price</span></span>
- <span data-ttu-id="d61dc-130">説明</span><span class="sxs-lookup"><span data-stu-id="d61dc-130">Description</span></span>
- <span data-ttu-id="d61dc-131">著者 (Authors テーブルへの外部キー) の Id</span><span class="sxs-lookup"><span data-stu-id="d61dc-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="d61dc-132">ほとんどの要求に対してただし、API では返します (タイトル、作成者、およびジャンル) は、このデータのサブセットです。</span><span class="sxs-lookup"><span data-stu-id="d61dc-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="d61dc-133">完全なレコードをクライアント要求を取得する`/api/books/{id}/details`します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d61dc-134">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d61dc-134">Prerequisites</span></span>

<span data-ttu-id="d61dc-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional または Enterprise edition。</span><span class="sxs-lookup"><span data-stu-id="d61dc-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="d61dc-136">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="d61dc-137">Visual Studio を実行して開始します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-137">Start by running Visual Studio.</span></span> <span data-ttu-id="d61dc-138">**ファイル**メニューの **新規**選び**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="d61dc-139">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="d61dc-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="d61dc-140">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="d61dc-141">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="d61dc-142">プロジェクトに名前を&quot;BooksAPI&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="d61dc-143">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="d61dc-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="d61dc-144">[フォルダーを追加およびコアの参照] を選択、 **Web API**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="d61dc-145">クリックして**プロジェクトを作成する**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="d61dc-146">これは、Web API の機能が構成されているスケルトン プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="d61dc-147">ドメイン モデル</span><span class="sxs-lookup"><span data-stu-id="d61dc-147">Domain Models</span></span>

<span data-ttu-id="d61dc-148">次に、ドメイン モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-148">Next, add classes for domain models.</span></span> <span data-ttu-id="d61dc-149">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="d61dc-150">選択**追加**を選択し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="d61dc-151">クラスに `Author` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d61dc-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="d61dc-152">次のように Author.cs 内のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d61dc-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="d61dc-153">という名前の別のクラスを追加するようになりました`Book`します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="d61dc-154">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-154">Add a Web API Controller</span></span>

<span data-ttu-id="d61dc-155">この手順では、データ層として Entity Framework を使用する Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="d61dc-156">Ctrl キーと Shift キーを押しながら B キーを押して、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="d61dc-157">Entity Framework では、リフレクションを使用して、コンパイル済みのアセンブリ、データベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="d61dc-158">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="d61dc-159">選択**追加**を選択し、**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="d61dc-160">**スキャフォールディングの追加**ダイアログ ボックスで、"Web API 2 コント ローラーと読み取り/書き込みアクションでは、Entity Framework を使用します"。</span><span class="sxs-lookup"><span data-stu-id="d61dc-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="d61dc-161">**コント ローラーの追加**ダイアログ ボックスの**コント ローラー名**、入力&quot;BooksController&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="d61dc-162">選択、&quot;非同期コント ローラー アクションを使用して、&quot;チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="d61dc-163">**モデル クラス**、&quot;帳&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="d61dc-164">(表示されない場合、`Book`クラスが、ドロップダウン リストに表示、プロジェクトをビルドするかどうかを確認してください)。「+」ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="d61dc-165">クリックして**追加**で、**新しいデータ コンテキスト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="d61dc-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="d61dc-166">クリックして**追加**で、**コント ローラーの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="d61dc-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="d61dc-167">スキャフォールディングがという名前のクラスを追加します`BooksController`API コント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="d61dc-168">という名前のクラスも追加`BooksAPIContext`Models フォルダーでの Entity Framework データ コンテキストを定義します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="d61dc-169">データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-169">Seed the Database</span></span>

<span data-ttu-id="d61dc-170">ツール メニューで、次のように選択します。**ライブラリ パッケージ マネージャー**、し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="d61dc-171">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="d61dc-172">このコマンドは、Migrations フォルダーを作成し、Configuration.cs をという名前の新しいコード ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="d61dc-173">このファイルを開き、次のコードを追加、`Configuration.Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="d61dc-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="d61dc-174">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="d61dc-175">これらのコマンドは、ローカル データベースを作成し、データベースを設定するシード メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="d61dc-176">DTO クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-176">Add DTO Classes</span></span>

<span data-ttu-id="d61dc-177">これでアプリケーションを実行して、/api/books/1 に GET 要求を送信して、応答は次のような検索します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="d61dc-178">(読みやすくするためのインデントが追加されます)。</span><span class="sxs-lookup"><span data-stu-id="d61dc-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="d61dc-179">代わりに、この要求のフィールドのサブセットを返すにします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="d61dc-180">作成者 ID ではなく、著者の名を返すさせたいことも、</span><span class="sxs-lookup"><span data-stu-id="d61dc-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="d61dc-181">コント ローラー メソッドの戻り値に変更します。 これを実現する、*データ転送オブジェクト*(DTO) EF モデルではなく。</span><span class="sxs-lookup"><span data-stu-id="d61dc-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="d61dc-182">DTO は、データを伝達するだけに設計されているオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="d61dc-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="d61dc-183">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** | **新しいフォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="d61dc-184">フォルダーの名前&quot;Dto&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="d61dc-185">という名前のクラスを追加`BookDto`次の定義で、Dto フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d61dc-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="d61dc-186">という名前の別のクラスを追加`BookDetailDto`します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="d61dc-187">次に、更新、`BooksController`を返すクラス`BookDto`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="d61dc-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="d61dc-188">使用して、 [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)メソッドをプロジェクトに`Book`インスタンスを`BookDto`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="d61dc-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="d61dc-189">コント ローラー クラスの更新されたコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="d61dc-190">削除し、 `PutBook`、 `PostBook`、および`DeleteBook`メソッド、このチュートリアルで必要としないためです。</span><span class="sxs-lookup"><span data-stu-id="d61dc-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="d61dc-191">これでアプリケーションを実行し、/api/books/1 を要求した場合これよう、応答本文になります。</span><span class="sxs-lookup"><span data-stu-id="d61dc-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="d61dc-192">ルート属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-192">Add Route Attributes</span></span>

<span data-ttu-id="d61dc-193">次に、属性ルーティングを使用するコント ローラーを変換します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="d61dc-194">最初に、追加、 **RoutePrefix**属性をコント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="d61dc-195">この属性は、このコント ローラー上のすべてのメソッドの最初の URI セグメントを定義します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="d61dc-196">追加し、 **[ルート]** 次のように、コント ローラー アクションに属性します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="d61dc-197">各コント ローラー メソッドのルート テンプレートは、プレフィックスと、文字列に指定されて、**ルート**属性。</span><span class="sxs-lookup"><span data-stu-id="d61dc-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="d61dc-198">`GetBook`メソッド、ルート テンプレートには、パラメーター化された文字列が含まれています。 &quot;{0} id: int}&quot;、URI セグメントには、整数値が含まれている場合に一致します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="d61dc-199">メソッド</span><span class="sxs-lookup"><span data-stu-id="d61dc-199">Method</span></span> | <span data-ttu-id="d61dc-200">ルート テンプレート</span><span class="sxs-lookup"><span data-stu-id="d61dc-200">Route Template</span></span> | <span data-ttu-id="d61dc-201">URI の例</span><span class="sxs-lookup"><span data-stu-id="d61dc-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="d61dc-202">"api/books"</span><span class="sxs-lookup"><span data-stu-id="d61dc-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="d61dc-203">"api/books/{id:int}"</span><span class="sxs-lookup"><span data-stu-id="d61dc-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="d61dc-204">書籍の詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-204">Get Book Details</span></span>

<span data-ttu-id="d61dc-205">書籍の詳細を取得するクライアントへの GET 要求を送信`/api/books/{id}/details`ここで、 *{id}* 書籍の ID です。</span><span class="sxs-lookup"><span data-stu-id="d61dc-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="d61dc-206">`BooksController` クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="d61dc-207">要求した場合`/api/books/1/details`、次のような応答。</span><span class="sxs-lookup"><span data-stu-id="d61dc-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="d61dc-208">ジャンルでブックを取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-208">Get Books By Genre</span></span>

<span data-ttu-id="d61dc-209">書籍の一覧には、特定のジャンルを取得するクライアントへの GET 要求を送信`/api/books/genre`ここで、*ジャンル*ジャンルの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="d61dc-210">(たとえば、`/api/books/fantasy`)。</span><span class="sxs-lookup"><span data-stu-id="d61dc-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="d61dc-211">次のメソッドを追加`BooksController`します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="d61dc-212">ここで、URI テンプレート内の {ジャンル} パラメーターを含むルート定義しています。</span><span class="sxs-lookup"><span data-stu-id="d61dc-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="d61dc-213">Web API がこれら 2 つの Uri を識別し、別のメソッドにルーティングできることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d61dc-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="d61dc-214">だ、`GetBook`メソッドに"id"セグメントは整数値である必要があります、制約が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d61dc-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="d61dc-215">/Api/books/fantasy を要求すると、応答はようになります。</span><span class="sxs-lookup"><span data-stu-id="d61dc-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="d61dc-216">ブックの作成者を取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-216">Get Books By Author</span></span>

<span data-ttu-id="d61dc-217">に特定の作成者の、書籍の一覧を取得するクライアントは、への GET 要求を送信`/api/authors/id/books`ここで、 *id*作成者の ID です。</span><span class="sxs-lookup"><span data-stu-id="d61dc-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="d61dc-218">次のメソッドを追加`BooksController`します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="d61dc-219">興味深いは、この例ため&quot;ブックの「&quot;はの子リソースを扱う&quot;作成者&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="d61dc-220">このパターンは、RESTful Api で非常に一般的です。</span><span class="sxs-lookup"><span data-stu-id="d61dc-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="d61dc-221">ルート テンプレートにティルダ (~) のオーバーライドでは、そのルート プレフィックス、 **RoutePrefix**属性。</span><span class="sxs-lookup"><span data-stu-id="d61dc-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="d61dc-222">発行日によってブックを取得します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-222">Get Books By Publication Date</span></span>

<span data-ttu-id="d61dc-223">発行日、書籍の一覧を取得するクライアントへの GET 要求を送信`/api/books/date/yyyy-mm-dd`ここで、 *- yyyy-mm-dd*日です。</span><span class="sxs-lookup"><span data-stu-id="d61dc-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="d61dc-224">これを行う 1 つの方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="d61dc-225">`{pubdate:datetime}`パラメーターが一致するように制約を**DateTime**値。</span><span class="sxs-lookup"><span data-stu-id="d61dc-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="d61dc-226">これは、動作より実際より制限の少ない。</span><span class="sxs-lookup"><span data-stu-id="d61dc-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="d61dc-227">たとえば、これらの Uri は、ルートと一致も。</span><span class="sxs-lookup"><span data-stu-id="d61dc-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="d61dc-228">これらの Uri を許可することに問題があります。</span><span class="sxs-lookup"><span data-stu-id="d61dc-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="d61dc-229">ただし、ルート テンプレートに正規表現の制約を追加することで、ルートを特定の形式に制限できます。</span><span class="sxs-lookup"><span data-stu-id="d61dc-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="d61dc-230">形式で日付を今すぐのみ&quot;- yyyy-mm-dd&quot;は一致します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="d61dc-231">実際の日付を入手したことを検証する正規表現を使用しない点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="d61dc-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="d61dc-232">Web API に URI セグメントに変換しようとするときに処理する、 **DateTime**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="d61dc-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="d61dc-233">無効な日付では、' 2012 などの 47-99' は、変換に失敗し、クライアントには、404 エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="d61dc-234">スラッシュ区切り記号をサポートすることも (`/api/books/date/yyyy/mm/dd`) 別の操作を追加することによって **[ルート]** 属性はさまざまな正規表現を使用します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="d61dc-235">若干の重要な詳細をここでは。</span><span class="sxs-lookup"><span data-stu-id="d61dc-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="d61dc-236">2 つ目のルート テンプレートがワイルドカード文字 (\*) {pubdate} パラメーターの先頭。</span><span class="sxs-lookup"><span data-stu-id="d61dc-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="d61dc-237">これは、ように、ルーティング エンジンで指示 {pubdate} は、URI の残りの部分と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d61dc-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="d61dc-238">既定では、テンプレート パラメーターは 1 つの URI セグメントと一致します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="d61dc-239">この場合は、{pubdate} にいくつかの URI セグメントをまたがるします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="d61dc-240">コント ローラー コード</span><span class="sxs-lookup"><span data-stu-id="d61dc-240">Controller Code</span></span>

<span data-ttu-id="d61dc-241">BooksController クラスの完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d61dc-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="d61dc-242">まとめ</span><span class="sxs-lookup"><span data-stu-id="d61dc-242">Summary</span></span>

<span data-ttu-id="d61dc-243">属性ルーティングでは、管理性と柔軟性、API の Uri を設計するときにします。</span><span class="sxs-lookup"><span data-stu-id="d61dc-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
