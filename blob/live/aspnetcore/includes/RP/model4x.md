<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="d385c-101">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="d385c-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="d385c-102">コマンド ライン (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むプロジェクト ディレクトリ) で次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d385c-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="d385c-103">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="d385c-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="d385c-104">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) のコマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="d385c-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
