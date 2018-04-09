---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: ASP.NET Web Pages の概要 - フォームを使用してデータベースのデータを入力する |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルは、入力フォームを作成し、フォームからデータベース テーブルにすると、表示 (... の ASP.NET Web ページを使用するデータを入力する方法を示します
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: bbccf8134e90c19e29efaa5afe1e46e15320c189
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>ASP.NET Web Pages の概要 - フォームを使用してデータベースのデータを入力します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、入力フォームを作成し、取得するフォームからデータベース テーブルに ASP.NET Web Pages (Razor) を使用するときにデータを入力する方法を示します。 を通じてシリーズを完了すると想定[の基礎の HTML フォームです ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251581)です。
> 
> 学習する内容。
> 
> - 入力フォームを処理する方法の詳細。
> - データベースで (insert) のデータを追加する方法です。
> - ユーザーがフォーム (ユーザー入力を検証する方法) で必要な値を入力したことを確認する方法です。
> - 検証エラーを表示する方法。
> - 現在のページから別のページに移動する方法。
>   
> 
> 説明されている機能/テクノロジ:
> 
> - `Database.Execute` メソッド。
> - SQL`Insert Into`ステートメント
> - `Validation`ヘルパー。
> - `Response.Redirect` メソッド。


## <a name="what-youll-build"></a>新機能のビルドします。

チュートリアルでは、以前のデータベースを作成する方法を紹介する、データベースで作業して WebMatrix で直接編集することによってデータベースのデータを入力した、**データベース**ワークスペース。 ほとんどのアプリでれていないが、データベースにデータを格納する実用的な方法です。 したがって、このチュートリアルでは、または他のデータを入力し、データベースに保存できるようにする web ベースのインターフェイスを作成します。

新しいムービーを入力することができます ページを作成します。 ページは、ムービーのタイトル、ジャンル、および年を入力するフィールド (テキスト ボックス) を持つ入力フォームが含まれます。 ページは、このページのようになります。

![ブラウザーの [ムービーの追加] ページ](entering-data/_static/image1.png)

テキスト ボックスに HTML なります`<input>`などこのマークアップの要素を検索します。

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>基本エントリ フォームを作成します。

という名前のページを作成する*AddMovie.cshtml*です。

次のマークアップ ファイルを置き換えます。 すべてを上書き間もなく、上部にあるコード ブロックを追加します。

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

この例では、フォームを作成する一般的な HTML を示します。 使用して`<input>`テキスト ボックスおよび [送信] ボタンの要素。 テキスト ボックスに対するキャプションが標準を使用して作成される`<label>`要素。 `<fieldset>`と`<legend>`要素は、フォームで nice ボックスを配置します。

このページのことに注意して、`<form>`要素を使用して`post`の値として、`method`属性。 前のチュートリアルで使用するフォームを作成した、`get`メソッドです。 フォームには、サーバーに値が送信される、ですが、要求しなかったを変更しないため、正しいでした。 データをフェッチは、さまざまな方法ではすべてでした。 ただし、このページに*は*変更を加える: 新しいデータベース レコードを追加する予定です。 したがって、このフォームを使用する必要があります、`post`メソッドです。 (の違いの詳細について`GET`と`POST`、操作を参照してください、[GET、POST、および HTTP 動詞の安全性](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)前のチュートリアルでサイドバーです)。

各テキスト ボックスは、`name`要素 (`title`、 `genre`、 `year`)。 前のチュートリアルで学習したようにこれらの名前は、後で、ユーザーの入力を取得できるように、それらの名前を持つ必要がありますので重要です。 任意の名前を使用することができます。 お勧めするのに役立つわかりやすい名前を使用してどのようなデータを扱う場合に注意してください。

`value`の各属性`<input>`要素には、Razor コードのビットが含まれています (たとえば、 `Request.Form["title"]`)。 このトリックのバージョンは、フォームが送信された後に (存在する場合)、テキスト ボックスに入力した値を保持するために前のチュートリアルで学習します。

## <a name="getting-the-form-values"></a>フォームの値を取得します。

次に、フォームを処理するコードを追加します。 アウトラインでは、次を行います。

1. ページがポストされるかどうかをチェック (送信) します。 コードを実行するページを最初に実行時ではなく、ユーザーは、ボタンをクリックした場合にのみです。
2. ユーザーがテキスト ボックスに入力した値を取得します。 この場合、形式が使用されているので、`POST`からフォームの値を取得する動詞を`Request.Form`コレクション。
3. 新しいレコードとして、値を挿入、*映画*データベース テーブルです。

ファイルの上部には、次のコードを追加します。

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

変数を作成する最初の数行 (`title`、 `genre`、および`year`)、テキスト ボックスから値を保持します。 行`if(IsPost)`により、変数が設定されていることを確認して*のみ*ユーザーがクリックして、**ムービーの追加**ボタン-は、ときに、フォーム ポストされました。

ような式を使用して、テキスト ボックスの値を取得する前のチュートリアルで説明したとおり`Request.Form["name"]`ここで、*名前*の名前を指定、`<input>`要素。

変数の名前 (`title`、 `genre`、および`year`) は任意です。 割り当てる名前と同様に`<input>`要素、好きなでも呼び出すことができます。 (変数の名前の名前属性と一致する必要はありません`<input>`フォーム上の要素です)。同様ですが、`<input>`要素が含まれているデータが反映される変数名を使用することをお勧めはします。 コードを記述するときに一貫した名前しやすくと、どのようなデータを扱う場合に注意してください。

## <a name="adding-data-to-the-database"></a>データベースにデータを追加します。

ブロックのコードにだけ、追加する*内*右中かっこ ( `}` ) の`if`(だけでなく、コード ブロック) 内のブロックは、次のコードを追加します。

[!code-csharp[Main](entering-data/samples/sample3.cs)]

この例では、データのフェッチと表示の前のチュートリアルで使用するコードに似ています。 始まる行`db =`が開き前に、と同じデータベース、および次の行 SQL ステートメントでは、もう一度として定義する前に説明しました。 ただし、この時間を定義する SQL`Insert Into`ステートメントです。 次の例の一般的な構文を示しています、`Insert Into`ステートメント。

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

つまりを指定するには、挿入先のテーブルに、挿入する列を一覧表示し、および挿入する値を一覧します。 (前に述べたように、SQL 大文字小文字を区別はありません人ものコマンドを読みやすくキーワードを大文字に変換します。)

挿入するか、列がコマンドで既に表示されている —`(Title, Genre, Year)`です。 興味深い部分にテキスト ボックスから値を取得する方法、`VALUES`コマンドの一部です。 実際の値の代わりに`@0`、 `@1`、および`@2`、プレース ホルダーはもちろんです。 コマンドを実行すると (上、`db.Execute`行)、テキスト ボックスから取得した値を渡します。

**重要**。 SQL ステートメント内のユーザーがオンラインに入力したデータを含める必要がありますが、唯一の方法では、プレース ホルダーを使用するように、ここに表示 (`VALUES(@0, @1, @2)`)。 SQL ステートメントにユーザー入力を連結する場合を開く自分で SQL インジェクション攻撃への説明に従って[フォームの基本 ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251581)(前のチュートリアル)。

まだ内、`if`ブロックは、後に、次の行を追加、`db.Execute`行。

[!code-css[Main](entering-data/samples/sample4.css)]

新しいムービーは、データベースに挿入されましたが、後にこの行に移動することを (リダイレクト)、*映画*ページ入力したムービーを表示できるようにします。 `~` 「Web サイトのルートです」を意味する演算子。 (、`~`演算子は一般的に HTML ではなく、ASP.NET ページでのみ動作します)。

完全なコード ブロックは、この例のようになります。

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>(これまで) の挿入 コマンドをテストします。

まだ、完了していませんが、今すぐテストする絶好のタイミングは。

WebMatrix 内のファイルのツリー ビューで右クリックし、 *AddMovie.cshtml*ページし、をクリックして**ブラウザーで起動**です。

![ブラウザーの [ムービーの追加] ページ](entering-data/_static/image2.png)

(ブラウザーの別のページ終了する場合は、url を確認してください`http://localhost:nnnnn/AddMovie`) ここで、 *nnnnn*使用しているポート番号です)。

エラー ページを入手しましたか。 場合は、ことをお読みし、コードを以前に一覧された表示だけに見えることかどうかを確認します。

ムービーの形式で入力&mdash;たとえば、"市民 Kane"、「ドラマ ・」および「1941」を使用します。 (またはすべて)。をクリックして**追加ムービー**です。

リダイレクトするしている場合はすべてうまく行けば、*映画*ページ。 新しいムービーが表示されていることを確認します。

![新しくを示すビデオ ページ ムービーの追加](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>ユーザー入力の検証

戻り、 *AddMovie*ページ、または、もう一度実行します。 別のムービーが、この時間を入力し、タイトルのみを入力&mdash;たとえば、"the で Rain の ' を家する"を入力します。 をクリックして**追加ムービー**です。

リダイレクトしている、*映画*ページをもう一度です。 新しいムービーを見つけることができますが、完全ではありません。

![ムービーがいくつかの値が欠落している新しいムービーの表示 ページします。](entering-data/_static/image4.png)

作成したときに、*映画*テーブルを明示的に言ったですべてのフィールドがある場合 null です。 ここに新しいムービーは、入力フォームがあり、空白のフィールドを終了しています。 エラーには。

この場合、データベースが発生しませんでした実際には (または*スロー*) エラー。 したがってジャンルまたは年を指定しなかった内のコード、 *AddMovie*  ページでは、いわゆるとしてそれらの値が扱われます*空の文字列*です。 ときに、SQL`Insert Into`コマンドの実行、ジャンル、年フィールドには、有用なデータがありませんでしたが、null はありませんでした。

当然ながら、ユーザー データベースに空の半分でムービー情報を入力できるようにします。 解決するには、ユーザーの入力を検証します。 最初に、検証は単に確認、ユーザーにすべてのフィールドの値が入力したこと (つまり、そのどれもが含まれています、空の文字列には)。

> [!TIP]
> 
> **Null および空の文字列**
> 
> プログラミングでは、「値はありません」の概念の違い 一般に、値は*null*しないを設定またはした任意の方法で初期化される場合。 文字データ (文字列) が必要ですが、変数を設定する一方、*空の文字列*です。 その場合は、値が null でないです。これはだけ明示的に設定されて文字の長さがゼロの文字列にします。 これら 2 つのステートメントでは、差分を表示します。
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> これが少し複雑よりも、ですが、重要な点`null`不定状態の並べ替えを表します。
> 
> 正確に値が null の場合と、空の文字列だけである場合を理解しておく必要があります。 コードで、 *AddMovie*  ページを使用して、テキスト ボックスの値を取得する`Request.Form["title"]`などです。 ときに、ページを最初に実行 (前に、ボタンをクリックすると)、値の`Request.Form["title"]`が null です。 フォームを送信するときに、`Request.Form["title"]`の値を取得、`title`テキスト ボックス。 すぐに見つからないものはありませんが、空のテキスト ボックスが null 以外ではないです。空の文字列を持つだけです。 したがって、ボタンに応答を実行したときにクリックして、`Request.Form["title"]`空の文字列が含まれる。
> 
> この区別が重要な理由 作成したときに、*映画*テーブルを明示的に言ったですべてのフィールドがある場合 null です。 ここで新しいムービーの入力フォームがあり、空白のフィールドを終了しています。 あがるジャンルまたは年の値がなかった新しいムービーを保存しようとしたときに、データベースが予測されるとします。 ですが、ポイントがある&mdash;のテキスト ボックスを空白のままにすると、場合でも、値は null 以外の場合は空の文字列を務めるです。 新しいムービーをこれらの列は空のデータベースに保存することができたら結果として、&mdash;いない null です。 &mdash; 値。 そのため、ユーザーがユーザーの入力の検証を行うには空の文字列を送信しないことを確認する必要があります。


### <a name="the-validation-helper"></a>検証ヘルパー

ASP.NET Web ページには、ヘルパーが含まれています。 &mdash; 、`Validation`ヘルパー&mdash;ユーザーが要件を満たすデータを入力するかどうかを確認を行えます。 `Validation`ヘルパーが組み込まれている ASP.NET Web ページには、以前のチュートリアルでは、Gravatar ヘルパーをインストールする方法、NuGet を使用して、パッケージとしてインストールする必要はありませんので、ヘルパーのいずれか。

ユーザーの入力を検証するには、次を行います。

- コードを使用すると、ページ上のテキスト ボックス内の値を必要とすることを指定できます。
- ムービー情報を正しくすべてを検証する場合にのみ、データベースに追加するにはコードにテストを格納します。
- エラー メッセージを表示するマークアップにコードを追加します。

コード ブロックで、 *AddMovie*  ページで、右上上部の変数の宣言の前に、次のコードを追加します。

[!code-csharp[Main](entering-data/samples/sample7.cs)]

呼び出す`Validation.RequireField`フィールドごとに 1 回 (`<input>`要素) のエントリを必要とします。 ここに表示されるように、各呼び出しのカスタム エラー メッセージを追加することもできます。 (おはさまざまあります好きな配置を示すためにメッセージです)。

問題がある場合は、新しいムービー情報、データベースに挿入されるを防止します。 `if(IsPost)`ブロックを使用して`&&`をテストする別の条件を追加する (論理 AND)`Validation.IsValid()`です。 終了すると、全体`if(IsPost)`ブロックはこのコードのようになります。

[!code-csharp[Main](entering-data/samples/sample8.cs)]

使用して登録されているフィールドのいずれかと、検証エラーがある場合、`Validation`ヘルパーに渡し、 `Validation.IsValid` false が返されます。 その場合は、そのブロック内のコードなし が実行され、データベースに無効なムービーのエントリが挿入されないようにします。 リダイレクトしているいないもちろん、*映画*ページ。

検証コードを含む、完全なコード ブロックは、この例のようになります。

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>検証エラーを表示します。

最後の手順では、すべてのエラー メッセージを表示します。 概要、またはその両方を表示または、検証エラーごとに個別のメッセージを表示できます。 このチュートリアルでは、されます両方を行うしくみを認識できるようにします。

各横にある`<input>`検証している要素では、呼び出し、`Html.ValidationMessage`メソッドの名前を渡すと、`<input>`要素を検証しています。 配置する、`Html.ValidationMessage`メソッド右側、エラー メッセージを表示します。 ページを実行すると、`Html.ValidationMessage`メソッドでの表示、`<span>`妥当性確認エラーがどこで要素です。 (エラーが存在しない場合、`<span>`要素が表示されるがあるテキストは存在しません)。

含むように、ページのマークアップを変更、 `Html.ValidationMessage` 、3 つのメソッド`<input>` ページで、要素がこの例と同様にします。

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

表示する、概要の動作方法も、次のマークアップを追加し、コードの直後に、`<h1>Add a Movie</h1>`ページ上の要素。

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

既定では、`Html.ValidationSummary`メソッドを一覧のすべての検証メッセージを表示します (、`<ul>`要素内にある、`<div>`要素)。 同様、`Html.ValidationMessage`メソッド、検証の概要のマークアップは常に表示されている以外の場合はエラーがない場合は、リスト アイテムはレンダリングされません。

使用しての代わりに、検証メッセージを表示する別の方法は、概要、`Html.ValidationMessage`各フィールドに固有のエラーを表示するメソッド。 または、概要と詳細の両方を使用することができます。 使用することもできます、`Html.ValidationSummary`一般的なエラーを表示し、使用して個々 のメソッド`Html.ValidationMessage`呼び出しの詳細を表示します。

[完了] ページは、この例のようになります。

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

それで完了です。 ムービーを追加するが、1 つまたは複数のフィールドを除外して、ページをテストできます。 操作を実行すると、次のエラーの表示を参照してください。

![検証エラー メッセージを示すビデオ ページを追加します。](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>検証エラー メッセージのスタイルを設定

エラー メッセージがあるが、実際に負ってよくことを確認できます。 ただし、エラー メッセージのスタイルを設定する簡単な方法があります。

によって表示される個別のエラー メッセージのスタイルを設定する`Html.ValidationMessage`、という名前の CSS スタイル クラスを作成する`field-validation-error`です。 検証の概要の外観を定義するには、指定された CSS スタイル クラスを作成`validation-summary-errors`です。

この手法のしくみを表示するには、追加、`<style>`内の要素、`<head>`ページのセクションです。 名前付きスタイル クラスを定義し、`field-validation-error`と`validation-summary-errors`次の規則が含まれています。

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

別のスタイル情報を通常おそらく put *.css*ファイル、わかりやすくするために配置できます ページで今のところですが。 (このチュートリアルのセットの後で移動、CSS 規則別に*.css*ファイルです)。

検証エラーがある場合、`Html.ValidationMessage`メソッドでの表示、`<span>`を含む要素`class="field-validation-error"`です。 そのクラスのスタイル定義を追加すると、メッセージの外観を構成できます。 エラーがある場合、`ValidationSummary`メソッドは、属性を動的に同様にレンダリング`class="validation-summary-errors"`です。

ページを再度実行し、意図的に 2 つのフィールドのままにします。 エラーが顕著になります。 (実際には、これら overdone しているが、何ができるかを表示するだけである)。

![スタイルが設定されている検証エラーを示すビデオ ページを追加します。](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>映画ページへのリンクの追加

取得するが簡単には、最後の 1 つの手順、 *AddMovie*オリジナルのムービーの一覧からページ。

開く、*映画*ページをもう一度です。 終了後に`</div>`に続くタグ、`WebGrid`ヘルパーに渡し、次のマークアップを追加します。

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

ASP.NET を解釈する前に説明したとおり、`~`演算子として、web サイトのルートです。 使用する必要はありません、`~`演算子; マークアップを使用する可能性があります`<a href="./AddMovie">Add a movie</a>`または HTML を理解しているパスを定義するその他の何らかの方法です。 ですが、`~`演算子は有効な一般的なアプローチ Razor ページのリンクを作成するときに、サイトをより柔軟なため、リンクに移動するものサブフォルダーに、現在のページを移動する場合、 *AddMovie*ページ。 (ことに注意して、`~`演算子でのみ機能*.cshtml*ページ。 ASP.NET が理解できるようにが標準の HTML ではありません)。

完了したら、実行、*映画*ページ。 このページのようになります。

!['追加映画' ページへのリンクと映画 ページ](entering-data/_static/image7.png)

クリックして、**ビデオを追加する**に移動することを確認してくださいへのリンク、 *AddMovie*ページ。

## <a name="coming-up-next"></a>次へ直近の見通し

次のチュートリアルでは、ユーザーが既にデータベースにデータを編集できるようにする方法を学習します。

## <a name="complete-listing-for-addmovie-page"></a>AddMovie ページの完全な一覧

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>その他のリソース

- [Razor 構文を使用して ASP.NET Web プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO ステートメント](http://www.w3schools.com/sql/sql_insert.asp)W3Schools サイト
- [サイトのページで ASP.NET Web ユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)です。 操作の詳細については、`Validation`ヘルパー。

> [!div class="step-by-step"]
> [前へ](form-basics.md)
> [次へ](updating-data.md)
