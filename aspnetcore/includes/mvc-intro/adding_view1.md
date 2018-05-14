# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC アプリへのビューの追加

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このセクションでは、Razor ビュー テンプレート ファイルを使用して、クライアントへの HTML 応答を生成するプロセスを完全にカプセル化するために `HelloWorldController` クラスを変更します。

ビュー テンプレート ファイルは Razor を使用して作成します。 Razor ベースのビュー テンプレートには *.cshtml* ファイル拡張子が含まれています。 C# を使用して HTML 出力を作成する洗練された方法が提供されます。

現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 `HelloWorldController` クラスでは、`Index` メソッドを次のコードで置き換えます。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

上のコードは `View` オブジェクトを返します。 ビュー テンプレートを使用して、ブラウザーへの HTML 応答を生成します。 上記の `Index` メソッドなどのコントローラー メソッド (アクション メソッドともいう) は、一般に、string などの型ではなく、[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (または `ActionResult` から派生したクラス) を返します。
