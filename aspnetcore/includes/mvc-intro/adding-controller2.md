*Controllers/HelloWorldController.cs* の内容を次のように置き換えます。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

コントローラーのすべての `public` メソッドが、HTTP エンドポイントとして呼び出されます。 上のサンプルでは、両方のメソッドが文字列を返します。  各メソッドの前のコメントに注意してください。

HTTP エンドポイントは、Web アプリケーション内のターゲット設定可能な URL (`http://localhost:1234/HelloWorld` など) であり、使われているプロトコル (`HTTP`)、Web サーバーの (TCP ポートを含む) ネットワーク上の場所 (`localhost:1234`)、ターゲットの URI (`HelloWorld`) を組み合わせたものです。

1 番目のコメントは、これが [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) メソッドであり、ベース URL に "/HelloWorld/" を追加することによって呼び出されることを示しています。 2 番目のコメントは、URL に "/HelloWorld/Welcome/" を追加することによって呼び出される [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) メソッドを示しています。 このチュートリアルではこの後、スキャフォールディング エンジンを使って `HTTP POST` メソッドを生成します。

非デバッグ モードでアプリを実行し、アドレス バーのパスに "HelloWorld" を追加します。 `Index` メソッドが文字列を返します。

!["This is my default action" というアプリケーションの応答が表示されているブラウザー ウィンドウ](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC は、着信 URL に応じてコントローラー クラス (およびそれらに含まれるアクション メソッド) を呼び出します。 MVC によって使われる既定の [URL ルーティング ロジック](../../mvc/controllers/routing.md)では、次のような形式を使って呼び出すコードが決定されます。

`/[Controller]/[ActionName]/[Parameters]`

ルーティングの形式は、*Startup.cs* ファイルで設定します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

URL セグメントを指定しないでアプリを実行すると、既定により、"Home" コントローラーと "Index" メソッドが、上の強調表示されている template 行で指定されます。

1 番目の URL セグメントでは、実行するコントローラー クラスが決定されます。 したがって、`localhost:xxxx/HelloWorld` は `HelloWorldController` クラスにマップします。 URL セグメントの 2 番目の部分では、クラスのアクション メソッドが決定されます。 したがって、`localhost:xxxx/HelloWorld/Index` では `HelloWorldController` クラスの `Index` メソッドが実行されます。 参照する必要があるのは `localhost:xxxx/HelloWorld` だけであり、`Index` メソッドは既定で呼び出されることに注意してください。 これは、`Index` はメソッド名が明示的に指定されていない場合にコントローラーで呼び出される既定のメソッドであるためです。 URL セグメントの 3 番目の部分 (`id`) はルート データ用です。 ルート データについては後で説明します。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome` メソッドが実行され、"This is the Welcome action method..." という文字列を返します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

!["This is the Welcome action method" というアプリケーションの応答が表示されているブラウザー ウィンドウ](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

URL からコントローラーにいくつかのパラメーター情報を渡すように、コードを変更します。 たとえば、`/HelloWorld/Welcome?name=Rick&numtimes=4` のようにします。 次のコードで示すように、2 つのパラメーターを含むように `Welcome` メソッドを変更します。 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

上のコードでは以下の操作が行われます。

* C# のオプション パラメーター機能を使って、`numTimes` パラメーターに値が渡されない場合の既定値が 1 であることを示します。
* `HtmlEncoder.Default.Encode` を使って、悪意のある入力 (つまり JavaScript) からアプリを保護します。 
* [補間文字列](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) を使います。

アプリを実行して次の URL を参照します。

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(xxxx は実際のポート番号に置き換えます)URL の `name` と `numtimes` に違う値を指定してみてください。 MVC の[モデル バインド](../../mvc/models/model-binding.md) システムは、名前付きパラメーターを、アドレス バーのクエリ文字列からメソッドのパラメーターに自動的にマップします。 詳しくは、「[モデル バインド](../../mvc/models/model-binding.md)」をご覧ください。

!["Hello Rick, NumTimes is: 4" というアプリケーションの応答が表示されているブラウザー ウィンドウ](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

上の図では、URL セグメント (`Parameters`) は使われておらず、`name` および `numTimes` パラメーターは[クエリ文字列](https://wikipedia.org/wiki/Query_string)として渡されています。 上の URL の `?` (疑問符) は区切り記号であり、後にクエリ文字列が続きます。 `&` 文字は、クエリ文字列を区切ります。

`Welcome` メソッドを次のコードで置き換えます。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

アプリを実行し、次の URL を入力します。`http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

!["Hello Rick, ID: 3" というアプリケーションの応答が表示されているブラウザー ウィンドウ](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

今度は、3 番目の URL セグメントがルート パラメーター `id` と一致しました。 `Welcome` メソッドには、`MapRoute` メソッドの URL テンプレートと一致したパラメーター `id` が含まれます。 末尾の `?` (`id?`) は、`id` パラメーターが省略可能であることを示します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

これらの例では、コントローラーは MVC の "VC" 部分を実行しています。つまり、ビューとコントローラーが動作します。 コントローラーは HTML を直接返しています。 一般に、コントローラーが HTML を直接返すのは、コーディングと保守が非常に面倒になるので、望ましくありません。 代わりに、通常は、別の Razor ビュー テンプレート ファイルを使って、HTML 応答の生成を支援します。 これは次のチュートリアルで行います。
