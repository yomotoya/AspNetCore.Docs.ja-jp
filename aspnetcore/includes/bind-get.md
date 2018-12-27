> [!WARNING]
> セキュリティ上の理由から、ページ モデルのプロパティに対して `GET` 要求データのバインドをオプトインする必要があります。 プロパティにマップする前に、ユーザー入力を確認してください。 `GET` バインドをオプトインするのは、クエリ文字列やルート値に依存するシナリオに対処する場合に便利です。
>
> `GET` 要求のプロパティをバインドするには、[[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) 属性の `SupportsGet` プロパティを `true`: `[BindProperty(SupportsGet = true)]` に設定します
