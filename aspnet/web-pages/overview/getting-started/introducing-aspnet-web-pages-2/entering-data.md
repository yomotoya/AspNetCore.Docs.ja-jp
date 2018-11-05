---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: ASP.NET Web Pages の概要 - データベースのデータを入力するフォームを使用して |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、入力フォームを作成し、取得するフォームからデータベース テーブルに ASP.NET Web Pages (... を使用するときにデータを入力する方法
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: e40d2962ccac56eaaf4812819aa42168e69295bc
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021730"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>ASP.NET Web Pages の概要 - フォームを使用してデータベースのデータを入力します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、入力フォームを作成し、取得するフォームからデータベース テーブルに ASP.NET Web Pages (Razor) を使用するときにデータを入力する方法を示します。 を通じてシリーズを完了したと想定して[基本の HTML フォームの ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251581)します。
> 
> 学習内容。
> 
> - 入力フォームを処理する方法の詳細。
> - データベースのデータ (挿入) を追加する方法。
> - ユーザーがフォーム (ユーザー入力を検証する方法) で必要な値を入力したかどうかを確認する方法。
> - 検証エラーを表示する方法。
> - 現在のページから別のページに移動する方法。
>   
> 
> 説明した機能/テクノロジ:
> 
> - `Database.Execute` メソッド。
> - SQL`Insert Into`ステートメント
> - `Validation`ヘルパー。
> - `Response.Redirect` メソッド。


## <a name="what-youll-build"></a>構築します

チュートリアルでは、前のデータベースを作成する方法を示しましたが、WebMatrix での作業で直接、データベースを編集することによってデータベースのデータを入力した、**データベース**ワークスペース。 ほとんどのアプリでないが、データベースにデータを格納する実用的な方法。 したがって、このチュートリアルでは、または他のデータを入力し、データベースに保存できる web ベースのインターフェイスを作成します。

新しいムービーを入力するページを作成します。 ページは、ムービーのタイトル、ジャンル、および年を入力するフィールド (テキスト ボックス) が含まれる入力フォームが含まれます。 ページは、このページのようになります。

![ブラウザーで映画の追加 ページ](entering-data/_static/image1.png)

テキスト ボックスに HTML なります`<input>`などこのマークアップの要素を検索します。

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>エントリの基本的なフォームを作成します。

という名前のページを作成する*AddMovie.cshtml*します。

新機能を次のマークアップ ファイルで置き換えます。 他のものを上書き間もなく、上部にあるコード ブロックを追加します。

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

この例では、フォームを作成する一般的な HTML を示します。 使用して`<input>`テキスト ボックスおよび [送信] ボタンの要素。 テキスト ボックスのキャプションが標準を使用して作成された`<label>`要素。 `<fieldset>`と`<legend>`要素がフォーム上の便利なボックスを配置します。

このページで、`<form>`要素を使用して`post`の値として、`method`属性。 前のチュートリアルで使用するフォームを作成した、`get`メソッド。 フォームでは、値をサーバーに送信される、要求に変更をしないようにしなかったので、そうでした。 もしすべては、さまざまな方法でデータをフェッチをしました。 ただし、このページに*は*変更-新しいデータベース レコードを追加しようとしています。 したがって、このフォームを使用する必要があります、`post`メソッド。 (間の違いについての詳細は`GET`と`POST`、操作を参照してください、[GET、POST、および HTTP 動詞の安全性](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)前のチュートリアルの補足記事「)。

各テキスト ボックスにメモを`name`要素 (`title`、 `genre`、 `year`)。 前のチュートリアルで説明するようにこれらの名前は、後で、ユーザーの入力を取得できるように、これらの名前を必要なため重要です。 任意の名前を使用することができます。 お勧めする際に役立つ意味のある名を使用してどのようなデータを使用しているを注意してください。

`value`の各属性`<input>`要素には Razor コードが含まれています (たとえば、 `Request.Form["title"]`)。 フォームが送信された後は、このトリックのバージョンを (ある場合)、テキスト ボックスに入力された値を保持するために、前のチュートリアルで学習します。

## <a name="getting-the-form-values"></a>フォーム値を取得します。

次に、フォームを処理するコードを追加します。 アウトラインで、次を行います。

1. ページがポストされているかどうかを確認 (送信) します。 コードを実行するページが最初に実行されるときではなく、ユーザーは、ボタンをクリックした場合にのみです。
2. ユーザーがテキスト ボックスに入力した値を取得します。 この場合、形式が使用されているので、`POST`からフォーム値を取得する、動詞、`Request.Form`コレクション。
3. 新しいレコードとして値を挿入、*映画*データベース テーブル。

ファイルの上部にある次のコードを追加します。

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

変数を作成する最初の数行 (`title`、 `genre`、および`year`) テキスト ボックスから値を保持します。 行`if(IsPost)`により、変数が設定されていることを確認して*のみ*ユーザーがクリックすると、**映画の追加**ボタン-は、ときに、フォームが投稿されました。

ような式を使用して、テキスト ボックスの値を取得する前のチュートリアルで説明するように`Request.Form["name"]`ここで、*名前*の名前を指定します、`<input>`要素。

変数の名前 (`title`、 `genre`、および`year`) は任意です。 などの名前に割り当てた`<input>`要素、好きなデザインに呼び出すことができます。 (変数の名前の名前属性と一致する必要はありません`<input>`フォーム上の要素)。同様、`<input>`要素が含まれているデータが反映される変数名を使用することをお勧めは。 コードを記述するときに一貫した名前やすくを使用しているデータを思い出してください。

## <a name="adding-data-to-the-database"></a>データベースへのデータの追加

コードでブロックするだけで、追加された*内*右中かっこ ( `}` ) の`if`ブロック (だけでなく、コード ブロック) 内で、次のコードを追加します。

[!code-csharp[Main](entering-data/samples/sample3.cs)]

この例では、データをフェッチしたり表示を前のチュートリアルで使用したコードに似ています。 始まる行`db =`以前のように、データベースと次の行として定義します SQL ステートメントでは、もう一度開く前に説明しました。 ただし、この時間を定義する SQL`Insert Into`ステートメント。 次の例の一般的な構文を示しています、`Insert Into`ステートメント。

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

つまりに挿入するテーブルには、挿入する列を一覧表示し、および挿入する値を一覧表示を指定します。 (する前に述べたように、SQL は、大文字小文字を区別しない、一部の人には、コマンドを読みやすくキーワードが大文字に変換)。

挿入する列は、コマンドに既に表示されて、`(Title, Genre, Year)`します。 興味深い部分にテキスト ボックスから値を取得する方法、`VALUES`コマンドの一部です。 実際の値の代わりに`@0`、 `@1`、および`@2`、プレース ホルダーはもちろんです。 コマンドを実行すると (上、`db.Execute`行)、テキスト ボックスから取得した値を渡します。

**重要**。 SQL ステートメント内のユーザーがオンラインに入力したデータが含まれることが唯一の方法は、次に示すよう、プレース ホルダーを使用する、(`VALUES(@0, @1, @2)`)。 SQL ステートメントにユーザー入力を連結する場合を手動で開く、SQL インジェクション攻撃をで説明したよう[フォームの基本 ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251581)(前のチュートリアル)。

まだ内、`if`ブロックで、次の行を追加、`db.Execute`行。

[!code-css[Main](entering-data/samples/sample4.css)]

この行を (リダイレクト) のジャンプに新しいムービーは、データベースに挿入されていますが後、*映画*ページ入力したムービーを表示できるようにします。 `~` 「Web サイトのルートです」を意味する演算子。 (、`~`演算子は一般的に HTML ではなく、ASP.NET ページでのみ動作します)。

完全なコード ブロックは、この例のようになります。

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>(これまで) の挿入 コマンドをテストします。

まだ、完了していませんをテストする良い機会です。

WebMatrix 内のファイルのツリー ビューで右クリックし、 *AddMovie.cshtml*ページし、クリックして**ブラウザーで起動**します。

![ブラウザーで映画の追加 ページ](entering-data/_static/image2.png)

(ブラウザーの別のページと最終的する場合は、URL があることを確認してください`http://localhost:nnnnn/AddMovie`) ここで、 *nnnnn*を使用するポート番号です)。

エラー ページを取得しましたか。 そうである場合は、よくお読みし、コードを前に示したがどのような正確に見えることかどうかを確認します。

ムービーをフォームに入力&mdash;たとえば、"市民 Kane"、「ドラマ」および「1941」を使用します。 (またはすべて)。クリックして**追加ムービー**します。

すべてうまくいけば場合に、リダイレクトされます、*映画*ページ。 新しいムービーが表示されていることを確認します。

![新しく表示ムービー ページは、ムービーを追加します。](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>ユーザー入力の検証

戻り、 *AddMovie*ページ、または、もう一度実行します。 この時間がもう 1 つのムービーをタイトルのみを入力します。&mdash;たとえば、"the で Rain の ' を家する"を入力します。 クリックして**追加ムービー**します。

リダイレクトされます、*映画*ページをもう一度です。 新しいビデオを見つけることができますは完了しません。

![ムービーをページが表示された新しいムービーにはいくつかの値がありません。](entering-data/_static/image4.png)

作成したときに、*映画*テーブル、明示的にそれほどある null のフィールドがありませんでしたが。 ここで新しい映画は、入力フォームがあり、空白のフィールドを終了しています。 エラーです。

ここでは、データベースを生成していない実際に (または*スロー*) エラー。 そのため、ジャンルまたは年を指定しなかった内のコード、 *AddMovie*ページには、いわゆるとしてそれらの値が扱われます*空の文字列*。 ときに、SQL`Insert Into`コマンドが実行された、ジャンル、年フィールドは、有用なデータがありませんでしたが、null でした。

当然ながら、ユーザーがデータベースに半空ムービー情報を入力できるようにしたくないです。 このソリューションでは、ユーザーの入力を検証します。 最初に、検証だけになります、ユーザーにすべてのフィールドの値が入力したことを確認します (空の文字列を含むなしには、)。

> [!TIP]
> 
> **Null および空の文字列**
> 
> "No value"の概念を区別してプログラミングでは、 一般に、値は*null*しない設定またはした任意の方法で初期化される場合。 これに対し、文字データ (文字列) が必要とする変数に設定することができます、*空の文字列*します。 その場合は、値が null です。これはだけ明示的に設定されて文字の長さが 0 の文字列にします。 これら 2 つのステートメントでは、差分を表示します。
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> より複雑に少しが重要な点は`null`未確定の状態の並べ替えを表します。
> 
> 値が null の場合と、空の文字列だけが正確に理解しておく必要です。 コードで、 *AddMovie*  ページを使用して、テキスト ボックスの値を取得する`Request.Form["title"]`という具合です。 ときに、ページ最初の実行 (前に、ボタンをクリックすると)、値の`Request.Form["title"]`が null です。 フォームを送信するときに、`Request.Form["title"]`の値を取得、`title`テキスト ボックス。 、明確ではありませんが、空のテキスト ボックスが null 以外ではないです。空の文字列だけがあります。 したがって、ボタンに応答コードを実行する をクリックして、`Request.Form["title"]`空の文字列が含まれる。
> 
> この区別が重要な理由 作成したときに、*映画*テーブル、明示的にそれほどある null のフィールドがありませんでしたが。 ここで新しい映画は、入力フォームがあり、空白のフィールドを終了しています。 ジャンルまたは年の値がない新しいムービーを保存しようとしたときから不満があがるにデータベースが予測されるは。 ポイントですが、&mdash;場合でも、これらのテキスト ボックスを空白のままにすると、値 null は空の文字列です。 これらの列は空のデータベースに新しいムービーを保存できますしたらその結果、&mdash;いない null です。 &mdash; 値。 そのため、ユーザーがユーザーの入力の検証を行うには空の文字列を送信しないかどうかを確認する必要あります。


### <a name="the-validation-helper"></a>検証ヘルパー

ASP.NET Web ページには、ヘルパーが含まれています。 &mdash; 、`Validation`ヘルパー&mdash;ことをユーザーが要件を満たすデータを入力するかどうかを確認に使用することができます。 `Validation`ヘルパーが組み込まれている ASP.NET Web ページには、NuGet では、前のチュートリアルで Gravatar ヘルパーをインストールする方法を使用してパッケージとしてインストールする必要はありませんので、ヘルパーのいずれか。

ユーザーの入力を検証するには、次を行います。

- コードを使用すると、ページ上のテキスト ボックス内の値を必要とすることを指定できます。
- すべてが正しく検証する場合にのみ、ムービー情報をデータベースに追加するコードにテストを配置します。
- エラー メッセージを表示するマークアップにコードを追加します。

コード ブロックで、 *AddMovie*  ページで、右上変数の宣言の前に、上部にある次のコードを追加します。

[!code-csharp[Main](entering-data/samples/sample7.cs)]

呼び出す`Validation.RequireField`フィールドごとに 1 回 (`<input>`要素) のエントリを必要とします。 ここに表示するように、各呼び出しのカスタム エラー メッセージを追加することもできます。 (私たちは変化がどのような配置を示すためにメッセージです)。

問題がある場合は、新しいムービー情報、データベースに挿入されるようにします。 `if(IsPost)`ブロック`&&`をテストするもう 1 つの条件を追加する (論理 AND)`Validation.IsValid()`します。 完了したら、全体`if(IsPost)`ブロックがこのコードのようになります。

[!code-csharp[Main](entering-data/samples/sample8.cs)]

使用して登録されているフィールドのいずれかの検証エラーがある場合、`Validation`ヘルパー、`Validation.IsValid`メソッドが false を返します。 その場合は、そのブロック内のコードの実行、データベースに無効なムービー エントリが挿入されないようにします。 もちろんできませんにリダイレクトと、*映画*ページ。

検証コードを含む、完全なコード ブロックは、この例のようになります。

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>検証エラーを表示します。

最後の手順では、すべてのエラー メッセージを表示します。 概要については、またはその両方を表示または各検証エラーに対して個別のメッセージを表示できます。 このチュートリアルで行う両方動作を確認できるようにします。

それぞれの横にある`<input>`に検証する要素では、呼び出し、`Html.ValidationMessage`メソッドの名前を渡すと、`<input>`に検証する要素。 配置する、`Html.ValidationMessage`メソッドの右側に表示されるエラー メッセージをします。 ページが実行すると、`Html.ValidationMessage`メソッドのレンダリングを`<span>`要素の検証エラーが出ます。 (エラーがない場合、`<span>`要素が表示されますでそのテキストがない)。

含むように、ページのマークアップを変更、 `Html.ValidationMessage` 、3 つのメソッド`<input>` ページで、要素などの次の例。

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

概要の動作を確認するも、次のマークアップを追加し、コードの直後に、`<h1>Add a Movie</h1>`ページ上の要素。

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

既定で、`Html.ValidationSummary`メソッドは、リストのすべての検証メッセージを表示します (、`<ul>`内にある要素を`<div>`要素)。 同様、`Html.ValidationMessage`メソッド、検証の概要のマークアップが常にレンダリングされます。 エラーがない場合は、リスト項目は表示されません。

使用しての代わりに検証メッセージを表示する別の方法は、概要、`Html.ValidationMessage`各フィールドに固有のエラーを表示するメソッド。 または、概要と詳細の両方を使用することができます。 使用することができます、`Html.ValidationSummary`メソッドは一般的なエラーの表示を使用して、個々`Html.ValidationMessage`呼び出しの詳細を表示します。

[完了] ページは、この例のようになります。

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

それで完了です。 ムービーを追加する 1 つまたは複数のフィールドを除外して、ページをテストできます。 この場合、表示、次のエラーを参照してください。

![検証エラー メッセージを示すビデオ ページを追加します。](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>検証エラー メッセージのスタイル設定

エラーのメッセージがあるが本当に負って非常にうまくことを確認できます。 ただし、エラー メッセージのスタイルを設定する簡単な方法があります。

によって表示される個別のエラー メッセージのスタイルを設定する`Html.ValidationMessage`、という名前の CSS スタイル クラスを作成する`field-validation-error`します。 検証の概要の外観を定義するには、という名前の CSS スタイル クラスを作成`validation-summary-errors`です。

この手法の動作を確認するには追加、`<style>`内の要素、`<head>`ページのセクション。 という名前のスタイル クラスを定義し、`field-validation-error`と`validation-summary-errors`次の規則が含まれています。

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

別のスタイル情報を通常おそらく配置 *.css*ファイル、わかりやすくするために配置できます ページでここでは。 (この一連のチュートリアルの後半では、個別に CSS 規則を移動します *.css*ファイルです)。

検証エラーがある場合、`Html.ValidationMessage`メソッドのレンダリングを`<span>`要素が含まれる`class="field-validation-error"`します。 そのクラスのスタイル定義を追加すると、メッセージがどのように構成できます。 エラーがある場合、`ValidationSummary`メソッドは動的に同様に、属性をレンダリング`class="validation-summary-errors"`します。

ページを再度実行し、意図的に 2 つのフィールドのままにします。 エラーがより顕著に表れます。 (実際がいる overdone、しかし、何ができるかを示すためにです)。

![スタイルが設定されている検証エラーを示すビデオ ページを追加します。](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>ムービー ページへのリンクの追加

最後の 1 つの手順が簡単に取得するようにすること、 *AddMovie*オリジナルのムービーの一覧からページ。

開く、*映画*ページをもう一度です。 終了後`</div>`タグに続く、`WebGrid`ヘルパーでは、次のマークアップを追加。

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

ASP.NET を解釈する前に説明するように、 `~` web サイトのルートとして演算子。 使用する必要はありません、`~`は演算子のマークアップを使用することが`<a href="./AddMovie">Add a movie</a>`または HTML で認識されるパスを定義するその他の何らかの方法です。 `~`演算子は適切な一般的なアプローチ、Razor ページへのリンクを作成するときに、サイトをより柔軟なため、リンクに移動しても、サブフォルダーに、現在のページを移動する場合、 *AddMovie*ページ。 (ことに注意して、`~`演算子でのみ機能 *.cshtml*ページ。 ASP.NET が理解できるようにが標準の HTML ではありません)。

完了したら、実行、*映画*ページ。 このページのようになります。

!['追加映画' ページへのリンクを含むムービー ページ](entering-data/_static/image7.png)

をクリックして、**ビデオを追加する**に移動するかどうかを確認するリンク、 *AddMovie*ページ。

## <a name="coming-up-next"></a>次回について

次のチュートリアルでは、ユーザーが既にデータベース内にあるデータを編集できるようにする方法を学習します。

## <a name="complete-listing-for-addmovie-page"></a>AddMovie ページ全体を示します

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>その他のリソース

- [Razor 構文を使用して ASP.NET Web プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=202890)
- [ステートメントに SQL 挿入](http://www.w3schools.com/sql/sql_insert.asp)W3Schools サイト
- [ページのサイトを ASP.NET Web でユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)です。 作業の詳細については、`Validation`ヘルパー。

> [!div class="step-by-step"]
> [前へ](form-basics.md)
> [次へ](updating-data.md)
