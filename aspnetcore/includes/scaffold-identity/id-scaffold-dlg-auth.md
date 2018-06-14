Identity scaffolder を実行します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ソリューション エクスプ ローラー**、プロジェクトを右クリックして >**追加** > **スキャフォールディングされた新しい項目**です。
* 左側のウィンドウから、**追加 Scaffold**ダイアログで、 **Identity** > **追加**です。
* **追加 Identity**ダイアログ ボックスで、目的のオプションを選択します。
  * 既存のレイアウト ページを選択するか、正しくないマークアップでレイアウトは、ファイルが上書きされます。 既存の _Layout.cshtml ファイルを選択すると、**いない**上書きされます。

 たとえば`~/Pages/Shared/_Layout.cshtml`Razor ページの`~/Views/Shared/_Layout.cshtml`MVC プロジェクト
* 使用するには、既存のデータ コンテキストを上書きするには、少なくとも 1 つのファイルを選択します。 データ コンテキストを追加するには、少なくとも 1 つのファイルを選択する必要があります。
  * データ コンテキスト クラスを選択します。
  * 選択**追加**です。
* 新しいユーザー コンテキストを作成して、可能性のある Id のカスタム ユーザー クラスを作成します。
  * 選択、 **+** を新規作成するにはボタン**データ コンテキスト クラス**です。
  * 選択**追加**です。

注: 新しいユーザー コンテキストを作成する場合をオーバーライドするファイルを選択する必要ありません。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET scaffolder を以前インストールしていない場合は、今すぐインストールします。

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

パッケージの参照を追加[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)プロジェクト (\*.csproj) ファイル。 プロジェクト ディレクトリに、次のコマンドを実行します。

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Identity scaffolder オプションの一覧を次のコマンドを実行します。

```cli
dotnet aspnet-codegenerator identity -h
```

プロジェクト フォルダー内には、必要なオプションで Identity scaffolder を実行します。 たとえば、既定の UI での id とファイルの最小数を設定するには、次のコマンドを実行します。 DB コンテキストの正しい完全修飾名を使用します。

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
