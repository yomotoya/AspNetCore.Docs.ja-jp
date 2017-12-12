<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="47003-101">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="47003-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="47003-102">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="47003-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="47003-103">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="47003-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="47003-104">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="47003-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="47003-105">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="47003-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>