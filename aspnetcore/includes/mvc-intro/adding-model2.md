## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="7d809-101">最初の移行を追加し、データベースの更新</span><span class="sxs-lookup"><span data-stu-id="7d809-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="7d809-102">コマンド プロンプトを開き、プロジェクト ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="7d809-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="7d809-103">(ディレクトリを含む、 *Startup.cs*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="7d809-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="7d809-104">コマンド プロンプトで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7d809-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="7d809-105">[.NET core](https://docs.microsoft.com/dotnet/core/tools/index) .NET のクロスプラット フォーム実装です。</span><span class="sxs-lookup"><span data-stu-id="7d809-105">[.NET Core](https://docs.microsoft.com/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="7d809-106">これらのコマンドの次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="7d809-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="7d809-107">`dotnet restore`: に指定された NuGet パッケージのダウンロード、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7d809-107">`dotnet restore`: Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="7d809-108">`dotnet ef migrations add Initial`Entity Framework .NET Core CLI 移行コマンドを実行し、最初の移行を作成します。</span><span class="sxs-lookup"><span data-stu-id="7d809-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="7d809-109">「追加」の後にパラメーターは、移行に割り当てられる名前です。</span><span class="sxs-lookup"><span data-stu-id="7d809-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="7d809-110">ここで名前を付ける移行「初期」初期データベースの移行になっているためです。</span><span class="sxs-lookup"><span data-stu-id="7d809-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="7d809-111">この操作を作成、*データ/移行/\<日付と時刻 > _Initial.cs* 、移行に追加するコマンドを含むファイル、*ムービー*データベースにテーブルです。</span><span class="sxs-lookup"><span data-stu-id="7d809-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="7d809-112">`dotnet ef database update`先ほど作成した移行では、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="7d809-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="7d809-113">データベースと接続文字列について、次のチュートリアル」で説明します。</span><span class="sxs-lookup"><span data-stu-id="7d809-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="7d809-114">データ モデルの変更について学習します。、[フィールドを追加する](xref:tutorials/first-mvc-app/new-field)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="7d809-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
