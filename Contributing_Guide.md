# ASP.NET ドキュメントへのコントリビュート

このドキュメントでは、[ASP.NET のドキュメント サイト](https://docs.microsoft.com/aspnet/)に掲載されている記事やコード サンプルにコントリビュートするプロセスについて説明します。

## コントリビューションには、誤字の修正のような単純なものや、新しい記事のような複雑なものがあります。

単純な修正や提案を行う方法


## 記事は、マークダウン ファイルでリポジトリに格納されています。

マークダウン ファイルの内容に単純な変更を行うには、ブラウザー ウィンドウの右上にある **[編集]** リンクを選択します。

* (ブラウザーの幅が狭い場合は、**オプション** バーを拡げて  **[編集]** リンクを表示します。)指示に従ってプル要求 (PR) を作成します。
* PR はレビューされ、受け入れられるか、変更が提案されます。
* より複雑な投稿を作成する方法
* [Git と GitHub.com](https://guides.github.com/activities/hello-world/) の基本的な理解が必要となります。
* 既存の記事の変更や新たな記事の作成など、やりたいことを記載した [issue](https://github.com/aspnet/Docs/issues/new) を作成します。

作業に多くの時間を費やす前に、チームからの承認を得ましょう。

## [aspnet/Docs](https://github.com/aspnet/Docs/) のリポジトリをフォークし、変更を行うためのブランチを作成します。

変更内容を含むプル要求 (PR) を master に送ります。

## もし PR に 'cla-required' というラベルがついたら、[Contribution License Agreement (CLA、貢献者使用許諾契約書)](https://cla2.dotnetfoundation.org/) を締結してください。

PR のフィードバックに返信します。

```
![画像のalt属性の説明](configuration/_static/imagename.png)
```

このプロセスで新しい記事の発行に至った例については、.NET リポジトリの [issue 67](https://github.com/dotnet/docs/issues/67) と [プル要求 798](https://github.com/dotnet/docs/pull/798) を参照してください。

[コードの文書化](https://docs.microsoft.com/dotnet/articles/csharp/codedoc)に関する新しい記事です。

## マークダウン構文

記事は [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/) のスーパーセットである [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html) で書かれています。

ASP.NET のドキュメントでよく使われる UI 機能についての DFM 構文の例については、.NET リポジトリのスタイル ガイドにある「[Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md)」(メタデータとマークダウンのテンプレート) を参照してください。

フォルダー構造の規則

```
[!code-csharp[Main](configuration/sample/Program.cs)]
```

マークダウン ファイルごとに、画像用のフォルダーとサンプル コード用のフォルダーがある場合があります。

```
[!code-csharp[Main](configuration/sample/Program.cs?range=1-10,20,30,40-50]
[!code-html[Main](configuration/sample/Views/Home/Index.cshtml?range=1-10,20,30,40-50]
```

例として、[fundamentals/configuration.md](https://github.com/aspnet/Docs/blob/master/aspnetcore/fundamentals/configuration.md) の記事では、画像は [fundamentals/configuration/\_static](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/_static)、サンプルのアプリケーション プロジェクト ファイルは [fundamentals/configuration/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/sample) にあります。

*fundamentals/configuration.md* ファイルの画像は、次のマークダウンによってレンダリングされます。

```
[!code-csharp[Main](configuration/sample/Program.cs?name=snippet_Example)]
```

**すべての**画像に[代替テキスト](https://wikipedia.org/wiki/Alt_attribute)が必要です。

```
[!code-csharp[Main](configuration/sample/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
[!code-csharp[Main](configuration/sample/Program.cs?range=10-20&highlight=1-3]
[!code-html[Main](configuration/sample/Views/Home/Index.cshtml?range=10-20&highlight=1-3]
[!code-javascript[Main](configuration/sample/Project.json?range=10-20&highlight=1-3]
```

## マークダウン ファイル名と画像ファイル名は、すべて小文字でなければなりません。

内部リンク

内部リンクでは、対象の記事の `uid` を xref リンクで使用します。


### 詳細については、[DocFX の相互参照](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#cross-reference)に関するページ を参照してください。

* コード スニペット
* 記事には、要点を説明するためのコード スニペットが含まれていることがよくあります。
* DFM では、コードをマークダウン ファイルにコピーさせたり、別のコード ファイルを参照させたりすることができます。

   ```
   docfx -t default --serve
   ```

* コード内のエラーの可能性を最小限にするため、できる限り別のコード ファイルを使用することを推奨しています。


### コード ファイルは、上記のサンプル プロジェクトで記載したフォルダ構造でリポジトリに格納する必要があります。

* *configuration.md* ファイルで使用される [DFM コード スニペットの構文](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet)の例です。
* コード ファイル全体をスニペットとしてレンダリングするには:
* `\bin\docfx` 行番号を使用してファイルの一部をスニペットとしてレンダリングするには:
* C# のスニペットでは、[C# の region](https://docs.microsoft.com/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region)を参照できます。

  ```
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }
    
  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

`Docs\aspnet` 可能な限り、行番号ではなく region を使用します。これは、コード ファイルの行番号の変更によってマークダウンの行番号の参照と同期しなくなる傾向があるためです。

## C# の region は入れ子にすることができますが、外側の region を参照すると、内側の `#region` および `#endregion` ディレクティブはスニペットでレンダリングされなくなります。

"snippet_Example" という名前の C# の region をレンダリングするには:

## レンダリングされたスニペットで選択された行をハイライトするには (通常、黄色の背景色としてレンダリングされます):

DocFX による変更部分のテスト


<!--HONumber=Dec17_HO4-->


