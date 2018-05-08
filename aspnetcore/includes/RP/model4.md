<span data-ttu-id="2d98d-101">次の表で、ASP.NET Core コード ジェネレーターのパラメーターについて詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="2d98d-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="2d98d-102">パラメーター</span><span class="sxs-lookup"><span data-stu-id="2d98d-102">Parameter</span></span>               | <span data-ttu-id="2d98d-103">説明</span><span class="sxs-lookup"><span data-stu-id="2d98d-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="2d98d-104">-m</span><span class="sxs-lookup"><span data-stu-id="2d98d-104">-m</span></span>  | <span data-ttu-id="2d98d-105">モデルの名前。</span><span class="sxs-lookup"><span data-stu-id="2d98d-105">The name of the model.</span></span> |
| <span data-ttu-id="2d98d-106">-dc</span><span class="sxs-lookup"><span data-stu-id="2d98d-106">-dc</span></span>  | <span data-ttu-id="2d98d-107">データ コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="2d98d-107">The data context.</span></span> |
| <span data-ttu-id="2d98d-108">-udl</span><span class="sxs-lookup"><span data-stu-id="2d98d-108">-udl</span></span> | <span data-ttu-id="2d98d-109">既定のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="2d98d-109">Use the default layout.</span></span> |
| <span data-ttu-id="2d98d-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="2d98d-110">-outDir</span></span> | <span data-ttu-id="2d98d-111">ビューを作成するための相対出力フォルダー パス。</span><span class="sxs-lookup"><span data-stu-id="2d98d-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="2d98d-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="2d98d-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="2d98d-113">[編集] および [作成] ページに `_ValidationScriptsPartial` を追加します。</span><span class="sxs-lookup"><span data-stu-id="2d98d-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="2d98d-114">次のように、`h` スイッチを使用して、`aspnet-codegenerator razorpage` コマンドに関するヘルプを取得します。</span><span class="sxs-lookup"><span data-stu-id="2d98d-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="2d98d-115">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="2d98d-115">Test the app</span></span>

* <span data-ttu-id="2d98d-116">アプリを実行し、ブラウザーで URL に `/Movies` を追加します (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="2d98d-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="2d98d-117">**[作成]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="2d98d-117">Test the **Create** link.</span></span>

  ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="2d98d-119">**[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="2d98d-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="2d98d-120">次のようなエラーが発生した場合は、移行を実行しデータベースを更新したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2d98d-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
