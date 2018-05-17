<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

上記の `Index` メソッド:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

`id` パラメーターで更新された `Index` メソッド:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。

![ghost という単語が URL に追加された索引ビュー。Ghostbusters と Ghostbusters 2 という 2 本のムービーからなるムービーリストが返されています。](../../tutorials/first-mvc-app/search/_static/g2.png)

ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。 そのため、ここでは UI 要素を追加して、ムービーをフィルターできるようにします。 ルート バインドされた `ID` パラメーターを渡す方法をテストするために `Index` メソッドの署名を変更した場合は、`searchString` という名前のパラメーターを受け取るように署名を元に戻します。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

*Views/Movies/Index.cshtml* ファイルを開き、以下の強調表示されている `<form>` マークアップを追加します。

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` タグでは[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms)が使用されるため、フォームを送信するときに、フィルター文字列がムービー コントローラーの `Index` アクションに投稿されます。 変更内容を保存してから、フィルターをテストします。

![タイトル フィルター テキストボックスに ghost という単語が入力されたインデックス ビュー](../../tutorials/first-mvc-app/search/_static/filter.png)

予想どおり、`Index` メソッドの `[HttpPost]` オーバーロードはありません。 メソッドではデータをフィルターするだけで、アプリの状態を変更しないため、オーバーロードは必要ありません。

以下の `[HttpPost] Index` メソッドを追加できます。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` パラメーターは、`Index` メソッドのオーバーロードを作成するために使用されます。 これについては、チュートリアルの後半で説明します。

このメソッドを追加すると、アクション呼び出し元が `[HttpPost] Index` メソッドと一致し、`[HttpPost] Index` メソッドが以下のイメージのように実行されます。

![From HttpPost Index: filter on ghost というアプリケーション応答を示すブラウザー ウィンドウ](../../tutorials/first-mvc-app/search/_static/fo.png)

ただし、この `[HttpPost]` バージョンの `Index` メソッドを追加しても、実装方法は制限されます。 たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。 HTTP POST 要求の URL は、GET 要求の URL (localhost:xxxxx/Movies/Index) と同じであり、URL には検索情報がないことに注意してください。 検索文字列情報は、[フォーム フィールド値](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)としてサーバーに送信されます。 ブラウザーの開発者ツールまたは優れた [Fiddler ツール](http://www.telerik.com/fiddler)を使用して、これを確認できます。 次のイメージは、Chrome ブラウザーの開発者ツールを示しています。

![searchString 値が ghost の要求本文を示す、Microsoft Edge の開発者ツールの [ネットワーク] タブ](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

要求本文に検索パラメーターと [XSRF](xref:security/anti-request-forgery) トークンが表示されています。 前のチュートリアルで説明したように、[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms)では [XSRF](xref:security/anti-request-forgery) 偽造防止トークンが生成されることに注意してください。 ここではデータを変更しないため、コントローラー メソッドでトークンを検証する必要はありません。

検索パラメーターが URL ではなく、要求本文にあるため、その検索情報をキャプチャして、ブックマークしたり、他のユーザーと共有したりすることはできません。 これを解決するために、`HTTP GET` 要求を指定します。
