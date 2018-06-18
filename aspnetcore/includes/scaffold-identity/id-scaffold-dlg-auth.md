<span data-ttu-id="30ba8-101">Identity scaffolder を実行します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30ba8-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30ba8-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="30ba8-103">**ソリューション エクスプ ローラー**、プロジェクトを右クリックして >**追加** > **スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="30ba8-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="30ba8-104">左側のウィンドウから、**追加 Scaffold**ダイアログで、 **Identity** > **追加**です。</span><span class="sxs-lookup"><span data-stu-id="30ba8-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="30ba8-105">**追加 Identity**ダイアログ ボックスで、目的のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="30ba8-106">既存のレイアウト ページを選択するか、正しくないマークアップでレイアウトは、ファイルが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="30ba8-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="30ba8-107">既存の _Layout.cshtml ファイルを選択すると、**いない**上書きされます。</span><span class="sxs-lookup"><span data-stu-id="30ba8-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="30ba8-108">たとえば`~/Pages/Shared/_Layout.cshtml`Razor ページの`~/Views/Shared/_Layout.cshtml`MVC プロジェクト</span><span class="sxs-lookup"><span data-stu-id="30ba8-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="30ba8-109">使用するには、既存のデータ コンテキストを上書きするには、少なくとも 1 つのファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="30ba8-110">データ コンテキストを追加するには、少なくとも 1 つのファイルを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="30ba8-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="30ba8-111">データ コンテキスト クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-111">Select your data context class.</span></span>
  * <span data-ttu-id="30ba8-112">選択**追加**です。</span><span class="sxs-lookup"><span data-stu-id="30ba8-112">Select **ADD**.</span></span>
* <span data-ttu-id="30ba8-113">新しいユーザー コンテキストを作成して、可能性のある Id のカスタム ユーザー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="30ba8-114">選択、 **+** を新規作成するにはボタン**データ コンテキスト クラス**です。</span><span class="sxs-lookup"><span data-stu-id="30ba8-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="30ba8-115">選択**追加**です。</span><span class="sxs-lookup"><span data-stu-id="30ba8-115">Select **ADD**.</span></span>

<span data-ttu-id="30ba8-116">注: 新しいユーザー コンテキストを作成する場合をオーバーライドするファイルを選択する必要ありません。</span><span class="sxs-lookup"><span data-stu-id="30ba8-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="30ba8-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="30ba8-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="30ba8-118">ASP.NET scaffolder を以前インストールしていない場合は、今すぐインストールします。</span><span class="sxs-lookup"><span data-stu-id="30ba8-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="30ba8-119">パッケージの参照を追加[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)プロジェクト (\*.csproj) ファイル。</span><span class="sxs-lookup"><span data-stu-id="30ba8-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="30ba8-120">プロジェクト ディレクトリに、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="30ba8-121">Identity scaffolder オプションの一覧を次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="30ba8-122">プロジェクト フォルダー内には、必要なオプションで Identity scaffolder を実行します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="30ba8-123">たとえば、既定の UI での id とファイルの最小数を設定するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="30ba8-124">DB コンテキストの正しい完全修飾名を使用します。</span><span class="sxs-lookup"><span data-stu-id="30ba8-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
