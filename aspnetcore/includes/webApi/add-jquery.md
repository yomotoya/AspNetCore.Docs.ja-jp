## <a name="call-the-web-api-with-jquery"></a>jQuery での Web API の呼び出し

このセクションでは、jQuery を使用して Web API を呼び出す、HTML ページが追加されます。 jQuery で要求を開始し、API の応答からの詳細を含むページを更新します。

静的ファイルを提供し、既定のファイル マッピングを有効にするようにプロジェクトを構成します。 これには、*Startup.Configure* で [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 拡張メソッドを呼び出します。 詳細については、「[ASP.NET Core の静的ファイルの使用](xref:fundamentals/static-files)」を参照してください。

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

*index.html* という名前の HTML ファイルをプロジェクトの *wwwroot* ディレクトリに追加します。 その内容を次のマークアップに置き換えます。

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

*site.js* という名前の JavaScript ファイルをプロジェクトの *wwwroot* ディレクトリに追加します。 その内容を次のコードに置き換えます。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

ローカルで HTML ページをテストするために、ASP.NET Core プロジェクトの起動設定への変更が要求される場合があります。 プロジェクトの *Properties* ディレクトリの *launchSettings.json* を開きます。 アプリが強制的に *index.html*&mdash;プロジェクトの既定ファイルで開くようにするには、`launchUrl` プロパティを削除します。

jQuery を取得するには、いくつかの方法があります。 前のスニペットでは、ライブラリは CDN から読み込まれます。 このサンプルは、jQuery で API を呼び出す完全な CRUD の例です。 豊富な体験ができるように、このサンプルには追加の機能が用意されています。 API の呼び出しを中心にした説明を次に示します。

### <a name="get-a-list-of-to-do-items"></a>To Do アイテムのリストの取得

To Do アイテムのリストを取得するには、HTTP GET 要求を */api/todo* に送信します。

jQuery [ajax](https://api.jquery.com/jquery.ajax/) 関数は、AJAX 要求を API に送信します。この API はオブジェクトまたは配列を表す JSON を返します。 この関数では、HTTP 要求を指定した `url` に送信して、HTTP の対話式操作のすべてのフォームを処理できます。 `GET` は `type` として使用されます。 要求が成功した場合、`success` コールバック関数が呼び出されます。 コールバックでは、DOM は To Do 情報で更新されます。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>To Do アイテムの追加

To Do アイテムを追加するには、HTTP POST 要求を */api/todo/* に送信します。 要求本文に To Do オブジェクトを含める必要があります。 [ajax](https://api.jquery.com/jquery.ajax/) 関数は、`POST` を使用して API を呼び出しています。 `POST` と `PUT` の要求では、要求本文は API に送信されたデータを表します。 API は JSON 要求本文を必要としています。 `accepts` と `contentType` オプションは `application/json` に設定され、それぞれ送受信されるメディアの種類を分類します。 このデータは、[`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) を使用して、JSON オブジェクトに変換されます。 API で正常な状況コードが返された場合、`getData` 関数が呼び出され、HTML テーブルを更新します。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>To Do アイテムの更新

To Do アイテムの更新も追加も要求本文に依存するため、この 2 つはよく似ています。 この場合の更新と追加の唯一の違いは、`url` がアイテムの一意識別子を追加するために変更され、`type` が `PUT` になるということです。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>To Do アイテムの削除

To Do アイテムを削除するには、`DELETE` への AJAX 呼び出しで `type` を設定して、URL でアイテムの一意識別子を指定します。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
