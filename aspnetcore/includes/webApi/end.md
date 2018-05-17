## <a name="implement-the-other-crud-operations"></a>その他の CRUD 操作の実装

次のセクションでは、`Create`、`Update`、`Delete` の各メソッドをコントローラーに追加します。

### <a name="create"></a>作成

次の `Create` メソッドを追加します。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上記のコードは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されるように、HTTP POST メソッドです。 MVC は、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上記のコードは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されるように、HTTP POST メソッドです。 MVC は、HTTP 要求の本文から to-do 項目の値を取得します。
::: moniker-end

`CreatedAtRoute` メソッド:

* 201 応答を返します。 HTTP 201 は、サーバーに新しいリソースを作成する HTTP POST メソッドに対する標準の応答です。
* 応答に場所ヘッダーを追加します。 場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。 「[10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。
* "GetTodo" という名前のルートを使用して、URL を作成します。 "GetTodo" という名前のルートは `GetById` で定義します。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Postman を使用した作成要求の送信

* アプリを起動します。
* Postman を開きます。

![Postman コンソール](../../tutorials/first-web-api/_static/pmc.png)

* localhost URL のポート番号を更新します。
* HTTP メソッドを *POST* に設定します。
* **[Body]** タブをクリックします。
* **[raw]** ラジオ ボタンを選択します。
* 型を *[JSON (application/json)]* に設定します。
* 次の JSON のような、to-do 項目を含む要求本文を入力します。

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* **[Send]** ボタンをクリックします。

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> **[Send]** をクリックしても応答が表示されない場合、**[SSL certification verification]** オプションを無効にします。 これは、**[File]**、**[Settings]** の下にあります。 設定を無効にした後にもう一度 **[Send]** ボタンをクリックします。
::: moniker-end

次のように、**[Response]** ウィンドウの **[Headers]** タブをクリックして、**[Location]** ヘッダー値をコピーします。

![Postman コンソールの [Headers] タブ](../../tutorials/first-web-api/_static/pmc2.png)

場所ヘッダー URI を使用して新しいアイテムにアクセスできます。

### <a name="update"></a>更新

次の `Update` メソッドを追加します。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` は `Create` と似ていますが、HTTP PUT を使用します。 応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。 HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。 部分的な更新をサポートするには、HTTP PATCH を使用します。

Postman を使用し、to-do 項目の名前を "walk cat" に更新します。

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>削除

次の `Delete` メソッドを追加します。

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` の応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。

Postman を使用し、to-do 項目を削除します。

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmd.png)
