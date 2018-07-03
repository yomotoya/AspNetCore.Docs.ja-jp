<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="33c3a-101">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="33c3a-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="33c3a-102">コマンド ライン (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むプロジェクト ディレクトリ) で次を実行します。</span><span class="sxs-lookup"><span data-stu-id="33c3a-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="33c3a-103">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="33c3a-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="33c3a-104">前のエラーは、間違ったディレクトリにいる場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="33c3a-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="33c3a-105">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) のコマンド シェルを開いて、前のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="33c3a-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="33c3a-106">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="33c3a-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="33c3a-107">Visual Studio を終了し、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="33c3a-107">Exit Visual Studio and run the command again.</span></span>
