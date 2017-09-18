## <a name="implement-the-other-crud-operations"></a>その他の CRUD 操作の実装

コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。 これらはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。 プロジェクトは、コードの追加または変更後にビルドします。

### <a name="create"></a>作成

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

これは、HTTP POST メソッドです。[`[HttpPost]`](https://docs.microsoft.com/aspnet/core/api) 属性を使用します。 MVC は、[`[FromBody]`](https://docs.microsoft.com/aspnet/core/api) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。

`CreatedAtRoute` メソッドは、サーバーに新しいリソースを作成する HTTP POST メソッドの標準の応答である 201 の応答を返します。 `CreatedAtRoute` では、応答に場所ヘッダーも追加されます。 場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。 「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。

### <a name="use-postman-to-send-a-create-request"></a>Postman を使用した作成要求の送信

![Postman コンソール](../../tutorials/first-web-api/_static/pmc.png)

* HTTP メソッド名を `POST` に設定します。
* **[Body]** ラジオ ボタンを選択します。
* **[raw]** ラジオ ボタンを選択します。
* 型を JSON に設定します。
* キー/値エディターで次のような To do 項目を追加します。 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* **[Send]** を選択します。

* 次のように、下のウィンドウの [Headers] タブを選択して、**[Location]** ヘッダーをコピーします。

![Postman コンソールの [Headers] タブ](../../tutorials/first-web-api/_static/pmget.png)

[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。 `GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>更新

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` は `Create` と似ていますが、HTTP PUT を使用します。 応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。 HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。 部分的な更新をサポートするには、HTTP PATCH を使用します。

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>削除

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmd.png)
