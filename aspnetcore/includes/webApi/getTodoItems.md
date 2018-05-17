::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上記のコードでは、メソッドを使用せず、API コントローラー クラスを定義します。 次のセクションでは、API を実装するメソッドを追加します。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上記のコードでは、メソッドを使用せず、API コントローラー クラスを定義します。 次のセクションでは、API を実装するメソッドを追加します。 いくつかの便利な機能を有効にするには、`[ApiController]` 属性でクラスに注釈を付けます。 属性によって有効にする機能の詳細については、「[ApiControllerAttribute でクラスに注釈を付ける](xref:web-api/index#annotate-class-with-apicontrollerattribute)」を参照してください。
::: moniker-end

コントローラーのコンストラクターでは、[依存性の挿入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`TodoContext`) がコントローラーに挿入されています。 データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。 アイテムが存在しない場合は、コンストラクターがメモリ内データベースにアイテムを追加します。

## <a name="get-to-do-items"></a>To Do アイテムの取得

To Do アイテムを取得するには、`TodoController` クラスに次のメソッドを追加します。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

これらのメソッドでは、以下の 2 つの GET メソッドを実装します。

* `GET /api/todo`
* `GET /api/todo/{id}`

`GetAll` メソッドの HTTP 応答例を以下に示します。

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

[Postman](https://www.getpostman.com/) または [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) を使用して HTTP 応答を表示する方法については、後で説明します。

### <a name="routing-and-url-paths"></a>ルーティングと URL パス

`[HttpGet]` 属性は、HTTP GET 要求に応答するメソッドを表します。 各メソッドの URL パスは次のように構成されます。

* コントローラーの `Route` 属性でテンプレート文字列を使用します。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* `[controller]` をコントローラーの名前 ("Controller" サフィックスを除くコントローラー クラス名) に置き換えます。 このサンプルでは、コントローラー クラス名は **Todo**Controller で、ルート名は "todo" です。 ASP.NET Core の[ルーティング](xref:mvc/controllers/routing)では、大文字と小文字が区別されません。
* `[HttpGet]` 属性にルート テンプレート (`[HttpGet("/products")]` など) がある場合は、それをパスに追加します。 このサンプルではテンプレートを使用しません。 詳細については、「[Http[Verb] 属性を使用する属性ルーティング](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)」を参照してください。

次の `GetById` メソッドで、`"{id}"` は To Do アイテムの一意識別子に使用するプレースホルダーの変数です。 `GetById` が呼び出されると、メソッドの `id` パラメーターに URL の `"{id}"` の値が割り当てられます。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` により名前付きのルートを作成します。 名前付きのルートの役割は次のとおりです。

* アプリで、ルート名を使用して HTTP リンクを作成できるようにします。
* 説明は後ほど行います。

### <a name="return-values"></a>戻り値

`GetAll` メソッドは、`TodoItem` オブジェクトのコレクションを返します。 MVC は自動的にオブジェクトを [JSON](https://www.json.org/) にシリアル化して、応答メッセージの本文に JSON を書き込みます。 このメソッドの応答コードは 200 で、ハンドルされない例外がないものと想定します  ハンドルされない例外は 5xx エラーに変換されます。

::: moniker range="<= aspnetcore-2.0"
これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、より一般的な [IActionResult type](xref:web-api/action-return-types#iactionresult-type) 型を返します。 `GetById` には、次の 2 つの異なる戻り値の型があります。

* 要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。 戻り値が [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) の場合、HTTP 404 応答が返されます。
* それ以外の場合、メソッドは JSON 応答本文で 200 を返します。 戻り値が [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) の場合、HTTP 200 応答が返されます。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、[ActionResult\<T> 型](xref:web-api/action-return-types#actionresultt-type)を返します。 `GetById` には、次の 2 つの異なる戻り値の型があります。

* 要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。 戻り値が [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) の場合、HTTP 404 応答が返されます。
* それ以外の場合、メソッドは JSON 応答本文で 200 を返します。 戻り値が `item` の場合、HTTP 200 応答が返されます。
::: moniker-end