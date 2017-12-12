<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="3eedd-101">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="3eedd-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="3eedd-102">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="3eedd-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3eedd-103">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3eedd-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="3eedd-104">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="3eedd-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="3eedd-105">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="3eedd-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="3eedd-106">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="3eedd-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="3eedd-107">Visual Studio を終了し、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="3eedd-107">Exit Visual Studio and run the command again.</span></span>