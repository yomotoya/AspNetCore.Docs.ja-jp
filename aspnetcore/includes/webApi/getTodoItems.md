[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上のコードでは以下の操作が行われます。

* 空のコントローラー クラスを定義します。 次のセクションでは、API を実装するメソッドを追加します。
* コンストラクターでは、[依存性の注入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`TodoContext `) がコントローラーに挿入されています。 データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。
* アイテムが存在しない場合は、コンストラクターがメモリ内データベースにアイテムを追加します。

## <a name="getting-to-do-items"></a>To Do アイテムの取得

To Do アイテムを取得するには、`TodoController` クラスに次のメソッドを追加します。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

これらのメソッドでは、以下の 2 つの GET メソッドを実装します。

* `GET /api/todo`
* `GET /api/todo/{id}`

`GetAll` メソッドの HTTP 応答例を以下に示します。

```
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

`[HttpGet]` 属性では HTTP GET メソッドを指定します。 各メソッドの URL パスは次のように構成されます。

* コントローラーの `Route` 属性でテンプレート文字列を使用します。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* `[controller]` をコントローラーの名前 ("Controller" サフィックスを除くコントローラー クラス名) に置き換えます。 このサンプルでは、コントローラー クラス名は **Todo**Controller で、ルート名は "todo" です。 ASP.NET Core の[ルーティング](xref:mvc/controllers/routing)では大文字と小文字は区別されません。
* `[HttpGet]` 属性にルート テンプレート (`[HttpGet("/products")]` など) がある場合は、それをパスに追加します。 このサンプルではテンプレートを使用しません。 詳細については、「[Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)」 (Http[Verb] 属性を使用する属性のルーティング) を参照してください。

`GetById` メソッドの場合:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"` は、`todo` アイテムの ID に対するプレースホルダー変数です。 `GetById` が呼び出されると、メソッドの `id` パラメーターに URL の "{id}" の値が割り当てられます。

`Name = "GetTodo"` により名前付きのルートを作成します。 名前付きのルートの役割は次のとおりです。

* アプリで、ルート名を使用して HTTP リンクを作成できるようにします。
* 説明は後ほど行います。

### <a name="return-values"></a>戻り値

`GetAll` メソッドは `IEnumerable` を返します。 MVC は自動的にオブジェクトを [JSON](http://www.json.org/) にシリアル化して、応答メッセージの本文に JSON を書き込みます。 このメソッドの応答コードは 200 で、ハンドルされない例外がないものと想定します  (ハンドルされない例外は 5xx エラーに変換されます)。

これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、より一般的な `IActionResult` 型を返します。 `GetById` には、次の 2 つの異なる戻り値の型があります。

* 要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。 戻り値が `NotFound` の場合、HTTP 404 応答が返されます。

* それ以外の場合、メソッドは JSON 応答本文で 200 を返します。 戻り値が `ObjectResult` の場合、HTTP 200 応答が返されます。
