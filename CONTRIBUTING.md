# ASP.NET ドキュメントへのコントリビュート

このドキュメントでは、[ASP.NET ドキュメントサイト](https://docs.microsoft.com/aspnet/) に掲載されている記事やコードサンプルにコントリビュートするプロセスについて説明します。コントリビュートは、誤字のような単純なものでも新しい記事でも構いません。

## 簡易な修正や提案をする方法

記事は、Markdownファイルでリポジトリに格納されています。Markdownファイルの内容の軽微な変更するには、ブラウザーの右上にある**Edit**のリンクを選択します。 （ブラウザーの幅が狭い場合は、**オプション**バーを開いて**Edit**のリンクを表示します）。画面に従ってプルリクエスト（PR）を作成します。私たちはPRをレビューし、受け入れるか変更の提案をします。


## より複雑な投稿を作成する方法

[Git と GitHub.com](https://guides.github.com/activities/hello-world/) の基本的な理解が必要となります。

* 既存の記事の変更や新たに記事を作成するには、やりたいことを記載してIssueを作成しましょう。作業に時間を費やす前に、チームからの承認を得ましょう。
* [aspnet/Docs](https://github.com/aspnet/Docs/)のリポジトリをフォークし、変更をするためのブランチを作ります。
* 変更した内容を mastar にプルリクエスト（PR）を出します。
* もしPRに 'cla-required' というラベルがついたら、[the Contribution License Agreement (CLA、コントリビューションライセンス契約)を締結してください](https://cla2.dotnetfoundation.org/)。
* PRのフィードバックを待ちましょう。

このプロセスで新しい記事の発行に至った例については、.NET リポジトリの[issue 67](https://github.com/dotnet/docs/issues/67) と [pull request 798](https://github.com/dotnet/docs/pull/798) を参照してください。

## Markdown シンタックス

記事は [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/) のスーパーセットである [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html) で書かれています。ASP.NETのドキュメントでよく使われるUI機能についてのDFM構文の例は、.NET リポジトリのスタイルガイド [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) を参照してください。

## フォルダー構成の規約

Markdownファイルごとに、画像用のフォルダーとサンプルコード用のフォルダーがあります。例として、[fundamentals/configuration.md](https://github.com/aspnet/Docs/blob/master/aspnetcore/fundamentals/configuration.md) の記事では、画像は [fundamentals/configuration/\_static](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/_static)、サンプルコードは [fundamentals/configuration/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/sample) です。*fundamentals/configuration.md* ファイル内の画像は、次のMarkdownによってレンダリングされます。

```
![画像のalt属性の説明](configuration/_static/imagename.png)
```

**すべての**画像は [代替テキスト](https://en.wikipedia.org/wiki/Alt_attribute)が必要です。

Markdownのファイル名と画像のファイル名は、すべて小文字でなければなりません。

## コードスニペット

記事には、要点を説明するためのコードスニペットが含まれていることがよくあります。DFMでは、コードをMarkdownファイルにコピーしたり、別のコードファイルを参照することができます。コード内のエラーを最小限にするため、可能な限りコードのファイルを分けることを推奨しています。コードのファイルは、上記のサンプルプロジェクトで記載したフォルダ構造でリポジトリに保存する必要があります。

[DFMコードスニペット構文](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet) は、*configuration.md* ファイルで使用されているコードスニペットの例です。

コードのファイル全体をスニペットとしてレンダリングするには：

```
[!code-csharp[Main](configuration/sample/Program.cs)]
```

行番号を使用してファイルの一部をスニペットとしてレンダリングするには：

```
[!code-csharp[Main](configuration/sample/Program.cs?range=1-10,20,30,40-50]
[!code-html[Main](configuration/sample/Views/Home/Index.cshtml?range=1-10,20,30,40-50]
```

C＃のスニペットでは、[C＃のregion](https://msdn.microsoft.com/en-us/library/9a1ybwek.aspx) を参照できます。可能な限り、行番号ではなくregionを使用します。これは、コードファイルの行番号が変更され、Markdownの行番号の参照と同期しなくなる傾向があるためです。C＃のregionは入れ子にすることができますが、外側のregionを参照すると、内側の `＃region` と ` #endregion` ディレクティブはスニペットでレンダリングされなくなります。

"snippet_Example" と名付けたC#のregionをレンダリングするには：

```
[!code-csharp[Main](configuration/sample/Program.cs?name=snippet_Example)]
```

レンダリングされたスニペットで選択された行をハイライトするには（通常、黄色の背景色としてレンダリングされます）：

```
[!code-csharp[Main](configuration/sample/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
[!code-csharp[Main](configuration/sample/Program.cs?range=10-20&highlight=1-3]
[!code-html[Main](configuration/sample/Views/Home/Index.cshtml?range=10-20&highlight=1-3]
[!code-javascript[Main](configuration/sample/Project.json?range=10-20&highlight=1-3]
```

## DocFXで変更部分をテスト

サイトのローカルでホストされたバージョンを作成する [DocFX command-line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool) を使用して、変更部分をテストしましょう。DocFXは、docs.microsoft.comで作られたスタイルやサイト拡張をレンダリングしません。

DocFXは、Windowsでは.NET Framwork、LinuxまたはmacOSだとMonoが必要です。


### Windowsでのインストール

* [DocFX releases](https://github.com/dotnet/docfx/releases) からダウンロードして、*docfx.zip*を展開します。
* パスをDocFXに追加します。
* ウインドウのコマンドラインで、*docfx.json*ファイルを含む適切なフォルダ（ASP.NETコンテンツの場合は*aspnet*、ASP.NET Coreコンテンツの場合は*aspnetcore*）に移動し、次のコマンドを実行します。

   ```
   docfx -t default --serve
   ```

* ブラウザーで、`http://localhost:8080` を表示します。


### Monoでのインストール

* Homebrewの `brew install mono` 経由でMonoをインストールします。
* [latest version of DocFX](https://github.com/dotnet/docfx/releases) をダウンロードします。
* `\bin\docfx` に展開します。
* **docfx**のエイリアスを作ります。

  ```
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }
    
  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

`Docs\aspnet` または `Docs\aspnetcore` のディレクトリーで **docfx** を実行してサイトをビルドし、**docfx-serve** を実行して `http://localhost:8080` でサイトを見ます。

## Voice and tone

私たちの目標は、できるだけ多くの人が理解しやすいドキュメントを書くことです。そのために、コントリビューターが準拠すべき書き方のガイドラインを作成しました。詳細は、.NET リポジトリの [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) を参照してください。

## リダイレクト

記事を削除したりファイル名を変更したり、別のフォルダに移動したりすることで記事をブックマークした人が404の表示がされないようリダイレクトを作成しましょう。[master redirect file](https://github.com/aspnet/Docs/blob/master/.openpublishing.redirection.json) へのリダイレクトを追加しましょう。
\ No newline at end of file