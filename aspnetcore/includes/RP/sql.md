# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a>ASP.NET Core Razor ページ アプリでの SQLite の操作

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

`MovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。 データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) の Web サイトは次のようなものです。

> SQLite は埋め込みの全機能を備えた、自己完結型かつ高信頼性でパブリック ドメインの SQL データベース エンジンです。 SQLite は、世界で最も使用されているデータベース エンジンです。

SQLite データベースを管理および表示するためにダウンロードできるサードパーティ製ツールは多数あります。 以下の画像は [DB Browser for SQLite](http://sqlitebrowser.org/) のものです。 お気に入りの SQLite ツールがございましたら、そのツールの気に入っている点についてコメントしてください。

![movie db を示している DB Browser for SQLite](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>データベースのシード

*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。 生成されたコードを次のコードに置き換えます。

[!code-csharp[](code/Models/SeedData.cs)]

DB にムービーがある場合、シード初期化子が返されます。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>シード初期化子の追加

次のように、*Program.cs* ファイルで `Main` メソッドにシード初期化子を追加します。

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a>アプリのテスト

DB 内のすべてのレコードを削除します (そのため Seed メソッドが実行されます)。 アプリを停止および起動して、データベースをシードします。

アプリにシードされたデータが表示されます。
