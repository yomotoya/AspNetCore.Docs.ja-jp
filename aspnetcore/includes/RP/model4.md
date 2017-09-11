次の表で、ASP.NET Core コード ジェネレーターのパラメーターについて詳しく説明します。

| パラメーター               | 説明|
| ----------------- | ------------ |
| -m  | モデルの名前。 |
| -dc  | データ コンテキスト。 |
| -udl | 既定のレイアウトを使用します。 |
| -outDir | ビューを作成するための相対出力フォルダー パス。 |
| --referenceScriptLibraries | [編集] および [作成] ページに `_ValidationScriptsPartial` を追加します。 |

次のように、`h` スイッチを使用して、`aspnet-codegenerator razorpage` コマンドに関するヘルプを取得します。

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>アプリのテスト

* アプリを実行し、ブラウザーで URL に `/Movies` を追加します (`http://localhost:port/movies`)。
* **[作成]** リンクをテストします。

 ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* **[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。

次のエラーが発生した場合は、移行を実行しており、データベースを更新したことを確認します。

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```