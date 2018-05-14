## <a name="implement-the-other-crud-operations"></a>その他の CRUD 操作の実装

次のセクションでは、`Create`、`Update`、`Delete` の各メソッドをコントローラーに追加します。

### <a name="create"></a>作成

次の `Create` メソッドを追加します。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上記のコードは HTTP POST メソッドです。[`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 属性を使用します。 MVC は、[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。

`CreatedAtRoute` メソッド:

* 201 応答を返します。 HTTP 201 は、サーバーに新しいリソースを作成する HTTP POST メソッドに対する標準の応答です。
* 応答に場所ヘッダーを追加します。 場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。 「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。
* "GetTodo" という名前のルートを使用して、URL を作成します。 "GetTodo" という名前のルートは `GetById` で定義します。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

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

場所ヘッダー URI を使用して新しいアイテムにアクセスできます。

### <a name="update"></a>更新

次の `Update` メソッドを追加します。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` は `Create` と似ていますが、HTTP PUT を使用します。 応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。 HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。 部分的な更新をサポートするには、HTTP PATCH を使用します。

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>削除

次の `Delete` メソッドを追加します。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` の応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。

`Delete` をテストします。 

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmd.png)
