<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>ムービー モデルのスキャフォールディング

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
エラーが発生した場合は、次のようにします。
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。