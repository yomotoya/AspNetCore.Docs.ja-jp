<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="06df8-101">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="06df8-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="06df8-102">コマンド ライン (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むプロジェクト ディレクトリ) で次を実行します。</span><span class="sxs-lookup"><span data-stu-id="06df8-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="06df8-103">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="06df8-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="06df8-104">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) のコマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="06df8-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="06df8-105">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="06df8-105">If you get the error:</span></span>
  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

<span data-ttu-id="06df8-106">Visual Studio を終了し、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="06df8-106">Exit Visual Studio and run the command again.</span></span>
