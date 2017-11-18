<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="f9eb0-101">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="f9eb0-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="f9eb0-102">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9eb0-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f9eb0-103">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f9eb0-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```