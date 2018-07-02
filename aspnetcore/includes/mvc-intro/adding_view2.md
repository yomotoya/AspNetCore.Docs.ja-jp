*Views/HelloWorld/Index.cshtml* Razor ビュー ファイルの内容を次のコードに置き換えます。

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

`http://localhost:xxxx/HelloWorld` に移動します。 `HelloWorldController` の `Index` メソッドでは多くのことは行いませんでした。つまり、ステートメント `return View();` を実行し、メソッドでビュー テンプレート ファイルを使用して、ブラウザーへの応答をレンダリングするよう指定しただけです。 ビュー テンプレート ファイルの名前を明示的に指定しなかったため、MVC では既定で */Views/HelloWorld* フォルダー内の *Index.cshtml* ビュー ファイルが使用されました。 次のイメージは、ビューにハード コーディングされた "Hello from our View Template!"  という文字列を示しています。

![ブラウザー ウィンドウ](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

ブラウザー ウィンドウが小さい場合 (モバイル デバイスなどの場合) は、右上にある [Bootstrap のナビゲーション ボタン](http://getbootstrap.com/components/#navbar)を切り替えて (タップして)、**[ホーム]**、**[バージョン情報]**、および **[連絡先]** リンクを表示する必要がある場合があります。

![Bootstrap のナビゲーション ボタンが強調表示されているブラウザー ウィンドウ](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページの変更

メニューのリンク (**[MvcMovie]**、**[ホーム]**、**[バージョン情報]**) をタップします。 各ページには同じメニューのレイアウトが表示されます。 メニューのレイアウトは、*Views/Shared/_Layout.cshtml* ファイルに実装されています。 *Views/Shared/_Layout.cshtml* ファイルを開きます。

[[レイアウト]](xref:mvc/views/layout) テンプレートでは、1 か所でサイトの HTML コンテナー レイアウトを指定し、それをサイト内の複数のページに適用できます。 `@RenderBody()` という行を見つけます。 `RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに*ラップ*されます。 たとえば、**[バージョン情報]** リンクを選択した場合、`RenderBody` メソッド内で **Views/Home/About.cshtml** ビューがレンダリングされます。

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>レイアウト ファイルでのタイトルおよびメニュー リンクの変更

タイトル要素で、`MvcMovie` を `Movie App` に変更します。 レイアウト テンプレートのアンカー テキストを `MvcMovie` から `Movie App` に変更し、コントローラーを `Home` から `Movies` に変更します。下の強調表示されている箇所をご覧ください。

::: moniker range="<= aspnetcore-2.0"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=7,31)]
::: moniker-end

>[!WARNING]
> `Movies` コントローラーはまだ実装されていないため、そのリンクをクリックすると、404 (見つかりません) エラーが表示されます。

変更内容を保存して、**[バージョン情報]** リンクをタップします。 ここでブラウザー タブのタイトルが、**About - Mvc Movie** ではなく、**About - Movie App** になっていることに注目してください: 

![[バージョン情報]タブ](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

**[連絡先]** リンクをタップし、タイトルとアンカー テキストにも **[Movie App]** と表示されていることを確認してください。 レイアウト テンプレートで一度変更しただけで、サイト上のすべてのページに新しいリンク テキストと新しいタイトルが反映できました。

*Views/_ViewStart.cshtml* ファイルを確認します。


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* ファイルは *Views/Shared/_Layout.cshtml* ファイルに取り込まれ、各ビューに適用されます。 `Layout` プロパティを使用すれば、別のレイアウト ビューを設定することができます。また、`null` に設定して、レイアウト ファイルが使用されないようにすることができます。

`Index` ビューのタイトルを変更します。

*Views/HelloWorld/Index.cshtml* を開きます。 次の 2 か所が変更されています。

   * ブラウザーのタイトルに表示されるテキスト。
   * セカンダリ ヘッダー (`<h2>` 要素)。

これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

上のコードの `ViewData["Title"] = "Movie List";` では、`Title` ディクショナリの `ViewData` プロパティを "Movie List" に設定します。 `Title` プロパティは、次のように、レイアウト ページの `<title>` HTML 要素で使用されます。


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

変更内容を保存して、`http://localhost:xxxx/HelloWorld` に移動します。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルは、*Index.cshtml* ビュー テンプレートで設定した `ViewData["Title"]` で作成されます。レイアウト ファイルには "- Movie App" が追加されます。

*Index.cshtml* ビュー テンプレートのコンテンツがどのように *Views/Shared/_Layout.cshtml* ビュー テンプレートにマージされ、1 つの HTML 応答がブラウザーに送信されたかにも注目してください。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。 詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。

![ムービー リスト ビュー](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

ここでは、"データ" のごく一部 (この場合は "Hello from our View Template!" というメッセージ) を ハード コーディングしました。 MVC アプリケーションには "V" (ビュー) があり、"C" (コントローラー) もありますが、"M" (モデル) はまだありません。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

コントローラー アクションは、受信 URL 要求への応答として呼び出されます。 コントローラー クラスでは、受信ブラウザー要求を処理するコードを記述します。 コントローラーはデータ ソースからデータを取得し、ブラウザーに返す応答の種類を決定します。 ビュー テンプレートを使用すれば、コントローラーからブラウザーへの HTML 応答を生成して書式を設定できます。

コントローラーは、ビュー テンプレートで応答をレンダリングするために必要なデータを提供します。 ベスト プラクティス: ビュー テンプレートでビジネス ロジックを実行したり、データベースと直接対話したり**しない**でください。 ビュー テンプレートでは、コントローラーによって提供されるデータのみを処理するようにしてください。 この "関心の分離" を維持すれば、コードをクリーンでテスト可能な保守しやすい状態に保つことができます。

現時点では、`HelloWorldController` クラスの `Welcome` メソッドは `name` と `ID` パラメーターを受け取ってから、ブラウザーに直接値を出力します。 コントローラーにこの応答を文字列としてレンダリングさせるのではなく、代わりにビュー テンプレートを使用するようにコントローラーを変更します。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 そのためには、コントローラーで動的データ (パラメーター) を設定します。これは、ビュー テンプレートでアクセスできるようにするために `ViewData` ディレクトリに必要なデータです。

*HelloWorldController.cs* ファイルに戻り、`Welcome` メソッドを変更して `Message` および `NumTimes` 値を `ViewData` ディクショナリに追加します。 `ViewData` ディクショナリは動的オブジェクトです。つまり、必要なものを何でも設定できます。`ViewData` オブジェクトは、その内部に何かを設定するまでプロパティは定義されません。 [MVC のモデル バインド システム](xref:mvc/models/model-binding)は、名前付きパラメーター (`name` と `numTimes`) を、アドレス バーのクエリ文字列からメソッドのパラメーターに自動的にマップします。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` ディクショナリ オブジェクトには、ビューに渡されるデータが含まれています。 

*Views/HelloWorld/Welcome.cshtml* という名前の [ようこそ] ビュー テンプレートを作成します。

"Hello" `NumTimes` を表示する *Welcome.cshtml* ビュー テンプレートでループを作成します。 *Views/HelloWorld/Welcome.cshtml* の内容を次のコードに置き換えます。

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

変更内容を保存し、次の URL を参照します。

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

データは URL から取得され、[MVC モデル バインダー](xref:mvc/models/model-binding)を使用してコントローラーに渡されます。 コントローラーはデータを `ViewData` ディクショナリにパッケージ化し、そのオブジェクトをビューに渡します。 その後、ビューでブラウザーに HTML としてデータがレンダリングされます。

![[ようこそ] ラベルと、Hello Rick という語句が 4 つ示された [バージョン情報] ビュー](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

上のサンプルでは、`ViewData` ディクショナリを使用して、コントローラーからビューにデータを渡しました。 チュートリアルの後半で、ビュー モデルを使用して、コントローラーからビューにデータを渡します。 一般には、`ViewData` ディクショナリを使用する方法より、ビュー モデルを使用してデータを渡す方法が推奨されます。 詳細については、「[ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc)」 (MVC の ViewModel、ViewData、ViewBag、TempData、Session の比較) を参照してください。

"M" (モデル) については学習しましたが、データベースについてはまだです。 学習したことを確認し、ムービーのデータベースを作成してみましょう。
