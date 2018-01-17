<a name="cli"></a>
## <a name="perform-initial-migration"></a>最初の移行の実行

コマンド ラインから、次の .NET Core CLI コマンドを実行します。

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。 このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MvcMovieContext.cs* ファイル内)。 `Initial` 引数は移行の命名に使用されます。 任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。 詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。

`dotnet ef database update` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。
