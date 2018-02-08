---
title: "SP.NET Core で Razor ページにファイルをアップロードする"
author: guardrex
description: "Razor ページにファイルをアップロードする方法を説明します。"
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 24eaa0dd9293cc932c51d280300308e835a0840e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="61ad5-103">SP.NET Core で Razor ページにファイルをアップロードする</span><span class="sxs-lookup"><span data-stu-id="61ad5-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="61ad5-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="61ad5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="61ad5-105">このセクションで、Razor ページにファイルをアップロードする例を示します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="61ad5-106">このチュートリアルの [Razor ページ動画サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)では、小さなファイルのアップロードに最適な、単純なモデル バインディングを利用してファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="61ad5-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="61ad5-107">大きなファイルをストリーミングする方法については、「[Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)」 (ストリーミングでの大きいファイルのアップロード) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="61ad5-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="61ad5-108">以下の手順では、映画スケジュール ファイルのアップロード機能をサンプル アプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-108">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="61ad5-109">映画スケジュールを表すのが `Schedule` クラスです。</span><span class="sxs-lookup"><span data-stu-id="61ad5-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="61ad5-110">このクラスには、2 つのバージョンのスケジュールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="61ad5-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="61ad5-111">1 つのバージョンは顧客に提供される `PublicSchedule` です。</span><span class="sxs-lookup"><span data-stu-id="61ad5-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="61ad5-112">もう 1 つのバージョンは社員が利用する `PrivateSchedule` です。</span><span class="sxs-lookup"><span data-stu-id="61ad5-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="61ad5-113">いずれも別個のファイルとしてアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="61ad5-114">このチュートリアルでは、POST が 1 つのページからサーバーに 2 つのファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="61ad5-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="61ad5-115">FileUpload クラスを追加する</span><span class="sxs-lookup"><span data-stu-id="61ad5-115">Add a FileUpload class</span></span>

<span data-ttu-id="61ad5-116">以下では、ファイル アップロードのペアを処理する Razor ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-116">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="61ad5-117">`FileUpload` クラスを追加して、スケジュール データを取得するページにバインドします。</span><span class="sxs-lookup"><span data-stu-id="61ad5-117">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="61ad5-118">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="61ad5-118">Right click the *Models* folder.</span></span> <span data-ttu-id="61ad5-119">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-119">Select **Add** > **Class**.</span></span> <span data-ttu-id="61ad5-120">クラスに **FileUpload** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-120">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="61ad5-121">このクラスには、スケジュールのタイトルのプロパティと 2 つあるスケジュールのバージョンのそれぞれのプロパティが含まれます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-121">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="61ad5-122">3 つのプロパティはすべて必須であり、タイトルの長さは 3 ～ 60 文字にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="61ad5-122">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="61ad5-123">ファイルをアップロードするヘルパー メソッドを追加する</span><span class="sxs-lookup"><span data-stu-id="61ad5-123">Add a helper method to upload files</span></span>

<span data-ttu-id="61ad5-124">アップロード済みのスケジュール ファイルを処理するコード重複を回避するために、静的なヘルパー メソッドを先に追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-124">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="61ad5-125">アプリで *Utilities* フォルダーを作成し、次のコンテンツを含む *FileHelpers.cs* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-125">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="61ad5-126">ヘルパー メソッドの `ProcessFormFile` は [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) と [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) を受け取り、ファイルのサイズとコンテンツが含まれる文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-126">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="61ad5-127">コンテンツの種類と長さが確認されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-127">The content type and length are checked.</span></span> <span data-ttu-id="61ad5-128">ファイルが検証チェックに合格しなければ、エラーが `ModelState` に追加されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-128">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="61ad5-129">Schedule クラスを追加する</span><span class="sxs-lookup"><span data-stu-id="61ad5-129">Add the Schedule class</span></span>

<span data-ttu-id="61ad5-130">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="61ad5-130">Right click the *Models* folder.</span></span> <span data-ttu-id="61ad5-131">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-131">Select **Add** > **Class**.</span></span> <span data-ttu-id="61ad5-132">クラスに **Schedule** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-132">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="61ad5-133">このクラスは `Display` 属性と `DisplayFormat` 属性を使用します。この 2 つの属性は、スケジュール データがレンダリングされるとき、わかりやすい名前を生成し、書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-133">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="61ad5-134">MovieContext を更新する</span><span class="sxs-lookup"><span data-stu-id="61ad5-134">Update the MovieContext</span></span>

<span data-ttu-id="61ad5-135">このスケジュールは、`MovieContext` (*Models/MovieContext.cs*) で `DbSet` を指定します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-135">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="61ad5-136">Schedule テーブルをデータベースに追加する</span><span class="sxs-lookup"><span data-stu-id="61ad5-136">Add the Schedule table to the database</span></span>

<span data-ttu-id="61ad5-137">**[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択し、パッケージ マネージャー コンソール (PMC) を起動します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-137">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="61ad5-139">PMC で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-139">In the PMC, execute the following commands.</span></span> <span data-ttu-id="61ad5-140">コマンドによって `Schedule` テーブルがデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-140">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="61ad5-141">ファイル アップロード Razor ページを追加する</span><span class="sxs-lookup"><span data-stu-id="61ad5-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="61ad5-142">*Pages* フォルダーで、*Schedules* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="61ad5-143">*Schedules* フォルダーで、*Index.cshtml* という名前のページを作成します。次のコンテンツでスケジュールをアップロードするためのページです。</span><span class="sxs-lookup"><span data-stu-id="61ad5-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="61ad5-144">各フォーム グループには、**\<label>** が含まれます。これは各クラス プロパティの名前を表示します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="61ad5-145">`FileUpload` モデルの `Display` 属性は、ラベルの表示値を与えます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="61ad5-146">たとえば、`UploadPublicSchedule` プロパティの表示名は `[Display(Name="Public Schedule")]` で設定されます。そのため、フォームがレンダリングされると、ラベルに "Public Schedule" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="61ad5-147">各フォーム グループには、検証 **\<span>** が含まれます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="61ad5-148">ユーザーの入力が `FileUpload` クラスに設定されているプロパティ属性の要求を持たさない場合、あるいは `ProcessFormFile` メソッド ファイルの検証チェックで不合格になった場合、モデルは検証に失敗します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="61ad5-149">モデルが検証に失敗すると、ヒントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="61ad5-150">たとえば、`Title` プロパティであれば、`[Required]` や `[StringLength(60, MinimumLength = 3)]` という注釈が表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="61ad5-151">ユーザーがタイトルを入力しないと、値が必須であるというメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="61ad5-152">ユーザーが 3 文字より少ないか、60 文字より多い値を入力した場合、値の長さが正しくないというメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="61ad5-153">コンテンツのないファイルが指定された場合、ファイルが空であるというメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="61ad5-154">ページ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="61ad5-154">Add the page model</span></span>

<span data-ttu-id="61ad5-155">*Schedules* フォルダーにページ モデル (*Index.cshtml.cs*) を追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-155">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="61ad5-156">ページ モデル (*Index.cshtml.cs* の `IndexModel`) は `FileUpload` クラスをバインドします。</span><span class="sxs-lookup"><span data-stu-id="61ad5-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="61ad5-157">このモデルはまた、スケジュールの一覧 (`IList<Schedule>`) を利用し、ページのデータベースに保存されているスケジュールを表示します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="61ad5-158">ページが `OnGetAsync` で読み込まれると、データベースから `Schedules` が入力され、読み込まれたスケジュールの HTML テーブルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="61ad5-159">フォームがサーバーに投稿されると、`ModelState` がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="61ad5-160">無効な場合、`Schedule` が再ビルドされます。ページがレンダリングされ、ページ検証に失敗した理由を伝える検証メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="61ad5-161">有効な場合、`FileUpload` プロパティが *OnPostAsync* で使用され、バージョンが 2 つあるスケジュール ファイルがアップロードされ、データを保存するための新しい `Schedule` オブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="61ad5-162">スケジュールがデータベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="61ad5-163">ファイル アップロード Razor ページをリンクする</span><span class="sxs-lookup"><span data-stu-id="61ad5-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="61ad5-164">*_Layout.cshtml* を開き、ファイル アップロード ページにアクセスするためのリンクをナビゲーション バーに追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="61ad5-165">スケジュール削除を確定するためのページを追加する</span><span class="sxs-lookup"><span data-stu-id="61ad5-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="61ad5-166">ユーザーがスケジュールを削除するボタンをクリックしたとき、削除を取り消す機会をユーザーに与えます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="61ad5-167">削除確定ページ (*Delete.cshtml*) を *Schedules* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="61ad5-168">このページ モデル (*Delete.cshtml.cs*) は、要求のルート データの `id` によって識別される 1 つのスケジュールを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-168">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="61ad5-169">*Delete.cshtml.cs* ファイルを *Schedules* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="61ad5-170">`OnPostAsync` メソッドは、その `id` によるスケジュール削除を処理します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="61ad5-171">スケジュールが削除されると、`RedirectToPage` はユーザーをスケジュール *Index.cshtml* ページに戻します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="61ad5-172">Schedules Razor ページの実際の動作</span><span class="sxs-lookup"><span data-stu-id="61ad5-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="61ad5-173">ページが読み込まれると、スケジュール タイトルのラベルと情報、パブリック スケジュール、プライベート スケジュール、送信ボタンがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![初回読み込み時の Schedules Razor ページ。検証エラーや空のフィールドに関する表示はまだありません。](uploading-files/_static/browser1.png)

<span data-ttu-id="61ad5-175">フィールドに何も入力せずに **[アップロード]** ボタンを選択すると、モデルの `[Required]` 属性に違反することになります。</span><span class="sxs-lookup"><span data-stu-id="61ad5-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="61ad5-176">`ModelState` が無効です。</span><span class="sxs-lookup"><span data-stu-id="61ad5-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="61ad5-177">検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-177">The validation error messages are displayed to the user:</span></span>

![各入力コントロールの隣に検証エラー メッセージが表示されます。](uploading-files/_static/browser2.png)

<span data-ttu-id="61ad5-179">**[タイトル]** フィールドに 2 つの文字を入力します。</span><span class="sxs-lookup"><span data-stu-id="61ad5-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="61ad5-180">タイトルは 3 ～ 60 文字にする必要があるという検証メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![タイトルの検証メッセージが変わりました](uploading-files/_static/browser3.png)

<span data-ttu-id="61ad5-182">1 つまたは複数のスケジュールをアップロードすると、**[Loaded Schedules]\(読み込まれたスケジュール\)** セクションに、読み込まれたスケジュールがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![読み込まれたスケジュールのテーブル。各スケジュールのタイトル、アップロード日付 (UTC)、パブリック バージョン ファイルのサイズ、プライベート バージョン ファイルのサイズ](uploading-files/_static/browser4.png)

<span data-ttu-id="61ad5-184">この **[削除]** リンクをクリックすると、削除確定ビューが表示されます。削除を確定するか、取り消す機会が与えられます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="61ad5-185">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="61ad5-185">Troubleshooting</span></span>

<span data-ttu-id="61ad5-186">`IFormFile` アップロードの問題を解決する方法については、「[ASP.NET でのファイルのアップロード](xref:mvc/models/file-uploads#troubleshooting)」の「トラブルシューティング」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="61ad5-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="61ad5-187">このたびは、この Razor ページの紹介を最後までお読みいただきありがとうございました。</span><span class="sxs-lookup"><span data-stu-id="61ad5-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="61ad5-188">コメントを残していただければ幸いです。</span><span class="sxs-lookup"><span data-stu-id="61ad5-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="61ad5-189">このチュートリアルの後は、「[Getting started with MVC and EF Core](xref:data/ef-mvc/intro)」 (MVC と EF Core の概要) にお進みいただくことが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="61ad5-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61ad5-190">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="61ad5-190">Additional resources</span></span>

* [<span data-ttu-id="61ad5-191">ASP.NET Core でのファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="61ad5-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="61ad5-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="61ad5-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="61ad5-193">前: 検証</span><span class="sxs-lookup"><span data-stu-id="61ad5-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
