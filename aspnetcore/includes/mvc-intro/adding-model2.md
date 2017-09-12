## <a name="add-initial-migration-and-update-the-database"></a>最初の移行を追加し、データベースの更新

* コマンド プロンプトを開き、プロジェクト ディレクトリに移動します。 (ディレクトリを含む、 *Startup.cs*ファイル)。

* コマンド プロンプトで次のコマンドを実行します。

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](https://docs.microsoft.com/dotnet/core/tools/index) .NET のクロスプラット フォーム実装です。 これらのコマンドの次のとおりです。

  * `dotnet restore`: に指定された NuGet パッケージのダウンロード、 *.csproj*ファイル。
  * `dotnet ef migrations add Initial`Entity Framework .NET Core CLI 移行コマンドを実行し、最初の移行を作成します。 「追加」の後にパラメーターは、移行に割り当てられる名前です。 ここで名前を付ける移行「初期」初期データベースの移行になっているためです。 この操作を作成、*データ/移行/\<日付と時刻 > _Initial.cs* 、移行に追加するコマンドを含むファイル、*ムービー*データベースにテーブルです。
  * `dotnet ef database update`先ほど作成した移行では、データベースを更新します。

データベースと接続文字列について、次のチュートリアル」で説明します。 データ モデルの変更について学習します。、[フィールドを追加する](xref:tutorials/first-mvc-app/new-field)チュートリアルです。
