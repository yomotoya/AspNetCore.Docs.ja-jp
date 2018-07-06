生成された Id のデータベース コードが必要です[Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/)します。 移行を作成し、データベースを更新します。 たとえば、次のコマンドを実行します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio で**パッケージ マネージャー コンソール**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

"CreateIdentitySchema"名前のパラメーター、`Add-Migration`コマンドは任意です。 `"CreateIdentitySchema"` 移行をについて説明します。