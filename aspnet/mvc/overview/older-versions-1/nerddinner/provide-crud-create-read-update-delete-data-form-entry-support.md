---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: "提供 CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート |Microsoft ドキュメント"
author: microsoft
description: "手順 5 では、編集、作成、および関連付けディナーを削除すると、同様のサポートを有効にし、さらに、DinnersController クラスを得る方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 5a314a1761527d8a2273166a743e3deac012a557
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>提供 CRUD (作成、読み取り、更新、削除) データ フォームのエントリのサポート
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 5 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 5 では、編集、作成、および関連付けディナーを削除すると、同様のサポートを有効にし、さらに、DinnersController クラスを得る方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner 手順 5: 作成、更新、フォームのシナリオの削除

導入されたコント ローラーとビュー、およびそれらを使用してサイトにディナーのカタログ/登録エクスペリエンスを実装する方法を説明しました。 次の手順は、DinnersController クラスはさらに、編集、作成、および関連付けディナーを削除すると、同様のサポートを有効になります。

### <a name="urls-handled-by-dinnerscontroller"></a>DinnersController によって処理される Url

2 つの Url のサポートを実装する DinnersController を以前にアクション メソッドを追加しました: */Dinners*と*/Dinners 詳細/[id]*です。

| **URL** | **動詞** | **目的** |
| --- | --- | --- |
| */Dinners/* | GET | 今後ディナーの HTML の一覧を表示します。 |
| */Dinners 詳細/[id]* | GET | 特定の dinner に関する詳細を表示します。 |

今すぐに次の 3 つの追加の Url を実装するアクション メソッドを追加します: */Dinners/編集/[id]、ディナー/作成、*と*/Dinners/削除/[id]*です。 これらの Url は、新しいディナーを作成およびディナーを削除する編集の既存ディナーのサポートを有効になります。

これらの新しい Url に HTTP GET および HTTP POST 動詞相互作用がサポートされます。 これらの Url に HTTP GET 要求は、(フォームの場合は"edit"Dinner データが設定される、「作成」の場合は空白のフォームおよび「削除」の場合、削除の確認画面) のデータの初期の HTML ビューに表示されます。 これらの Url に HTTP POST 要求は、Dinner データ、DinnerRepository (および、データベースにそこから) を保存/更新/削除になります。

| **URL** | **動詞** | **目的** |
| --- | --- | --- |
| */Dinners/編集/[id]* | GET | Dinner データが設定される編集可能な HTML フォームを表示します。 |
| 投稿 | データベースに特定 Dinner のフォームの変更を保存します。 |
| */ディナー/作成* | GET | により、ユーザーが新しいディナーを定義する空の HTML フォームを表示します。 |
| 投稿 | 新しい Dinner を作成し、データベースに保存します。 |
| */Dinners/削除/[id]* | GET | 削除の確認画面を表示します。 |
| 投稿 | 指定された dinner データベースから削除します。 |

### <a name="edit-support"></a>サポートを編集します。

"Edit"シナリオを実装することで見てみましょう。

#### <a name="the-http-get-edit-action-method"></a>HTTP GET 編集アクション メソッド

まず、HTTP"GET"メソッドの動作の編集操作を実装します。 このメソッドがあるときに呼び出さ、 */Dinners/編集/[id]* URL を要求します。 この実装は、ようになります。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

上記のコードは、Dinner オブジェクトを取得するのに、DinnerRepository を使用します。 ビューを Dinner オブジェクトを使用してテンプレートをレンダリングします。 テンプレート名を明示的に渡されたしていないため、 *View()*ヘルパー メソッドに使用する規約に基づく既定のパス テンプレートの表示を解決するには:/Views/Dinners/Edit.aspx です。

このテンプレートの表示を今すぐ作成してみましょう。 このが作業を行う編集メソッド内で右クリックし、「ビューの追加」のコンテキスト メニュー コマンドを選択します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

「ビューの追加」ダイアログ ボックスでおすることを示すためお Dinner オブジェクトとしてのモデル、ビュー テンプレートに渡す自動 scaffold を「編集」テンプレートを選択します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

[追加] ボタンをクリックするおと Visual Studio は新しい"Edit.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国追加します。 開くまで、新しい"Edit.aspx"ビュー内のテンプレート、コード エディター: 初期"Edit"scaffold 実装すると、以下に設定されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

生成されると、既定値"edit"scaffold にいくつかの変更を加えるし、(を公開したくありませんプロパティの一部を削除) コンテンツが編集ビュー テンプレートを更新してみましょう。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

アプリケーションと要求を実行するとき、 *「/ディナー/編集/1」* URL は、次のページが表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

このビューによって生成される HTML マークアップは、以下のようになります。 です。 標準 HTML –、&lt;フォーム&gt;要素への HTTP POST を実行する、 */Dinners/Edit/1* URL と [保存]&lt;入力の種類 ="送信"/&gt;ボタンが押されました。 Html ドキュメント&lt;入力の種類 ="text"/&gt;要素が編集可能なプロパティごとに出力されました。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() と Html.TextBox() Html ヘルパー メソッド

この"Edit.aspx"ビューのテンプレートがいくつかの方法「Html ヘルパー」: Html.ValidationSummary()、Html.BeginForm()、Html.TextBox()、および Html.ValidationMessage() です。 組み込みのエラー処理と検証の HTML マークアップを生成する、に加えてこれらのヘルパー メソッドを提供をサポートします。

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() ヘルパー メソッド

Html.BeginForm() ヘルパー メソッドは HTML 出力&lt;フォーム&gt;マークアップ内の要素。 当社 Edit.aspx ビュー テンプレートに適用する c# の"using"ステートメントこのメソッドを使用する場合は明らかです。 中かっこの先頭を示す、&lt;フォーム&gt;コンテンツ、および終了の中かっこがの末尾を示す内容、 &lt;/form&gt;要素。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

また、"using"ステートメントを検索する場合は、次のようなシナリオの不自然なアプローチ、Html.BeginForm() と Html.EndForm() の組み合わせが同じもの) を使用することができます。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

パラメーターを使用せず Html.BeginForm() を呼び出すことを現在の要求の URL に HTTP POST を行うフォーム要素を出力することになります。 つまり、編集ビューを生成理由、 *&lt;フォーム アクション =「/ディナー/編集/1」のメソッド"post"=&gt;* 要素。 私たちがまたはに渡すことが明示的なパラメーター Html.BeginForm() 別の URL に投稿する場合。

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() ヘルパー メソッド

当社 Edit.aspx ビュー Html.TextBox() ヘルパー メソッドを使用して出力する&lt;入力の種類 ="text"/&gt;要素。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

上記の Html.TextBox() メソッドには 1 つのパラメーター – の id または名前属性を指定するために使用される、&lt;入力の種類 ="text"/&gt;要素からテキスト ボックス値の設定に、モデル プロパティと同様に、出力をします。 たとえば、編集ビューに渡されたお Dinner オブジェクト「.NET フューチャ」の"Title"プロパティの値があり、Html.TextBox("Title") メソッドが出力を呼び出すため: *&lt;入力 id ="Title"名前「タイトル」の種類 ="text"の値を = =「.NET フューチャ」/&gt;*.

または、Html.TextBox() の最初のパラメーターを使用して、要素の id または名前を指定し、2 番目のパラメーターとして使用する値で明示的に渡すことができますお。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

多くの場合、出力される値のカスタム書式設定を実行することをおします。 指定する静的メソッドに組み込ま .NET は、これらのシナリオに役立ちます。 当社 Edit.aspx ビュー テンプレートを使用してこの、EventDate 値 (DateTime 型の) を書式設定、時間の秒数を表示しないように。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() を 3 番目のパラメーターは、追加の HTML 属性を出力するオプションで使用できます。 次のコードがその他のサイズを表示する方法を示します =「30」属性とクラス"mycssclass"属性を = on、&lt;入力の種類 ="text"/&gt;要素。 クラス属性を使用して、名前をエスケープお方法に注意してください、"@" character because "クラス"を予約済みキーワード (C#)。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>HTTP POST 編集アクション メソッドの実装

おに、編集アクション メソッドが実装されて、HTTP GET バージョンがインストールされているようになりました。 ユーザーが要求したとき、 */Dinners/Edit/1*次のような HTML ページが表示されるこれらの URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

フォームの post を上書き保存 ボタンを押すと、 */Dinners/Edit/1* URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。 – Dinner の保存を処理する、編集のアクション メソッドの HTTP POST の動作を実装してみましょう。

まず、DinnersController 上に"AcceptVerbs"属性があることを示す HTTP POST のシナリオを処理するオーバー ロードされた"Edit"アクション メソッドを追加します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

[AcceptVerbs] 属性を適用するオーバー ロードされたアクション メソッドに ASP.NET MVC は自動的に入力方向の HTTP 動詞に応じて適切なアクション メソッドにディスパッチ要求を処理します。 HTTP POST 要求を*/Dinners/編集/[id]*を他のすべての HTTP 動詞の要求中に、上記の編集方法に移動する Url */Dinners/編集/[id]*Url に移動する (これが最初の編集メソッドを実装しましたいません [AcceptVerbs] 属性を持つ)。

| **側トピックの内容は、理由、HTTP 動詞を使用して区別しますか。** |
| --- |
| 思うかもしれません – はなぜ 1 つの URL を使用して、HTTP 動詞を使用してその動作を区別しますか。 読み込みと編集の変更の保存を処理する 2 つの独立した Url があるだけしないのはなぜですか。 例:/Dinners/編集/[id] と表示するの初期フォーム/Dinners 保存/[id] を保存するためのフォーム post を処理しますか? 発行の 2 つの独立した Url での短所は、/Dinners/Save/2 を投稿し、入力エラーのため、HTML フォームを再表示する必要がありますがの場合、エンドユーザーが終了することを (でしたので、ブラウザーのアドレス バーにディナー/保存/2 の URL を持つURL にポストされたフォーム) です。 場合、エンドユーザーのブラウザーのお気に入りリストに redisplayed このページをブックマークまたは URL のコピー/貼り付けます。 電子メールを友人に、これらは (その URL は、post 値によって異なります) してから後で動作しない URL の保存を終了します。 1 つの URL を公開することで (のような:/Dinners/Edit/[id]) および HTTP 動詞でその処理を区別するの編集 ページをブックマークや他のユーザーに、URL を送信するエンドユーザーのも安全です。 |

#### <a name="retrieving-form-post-values"></a>フォーム ポスト値を取得します。

これは、さまざまな HTTP POST、"Edit"メソッド内でのフォーム パラメーターを投稿にアクセスする方法です。 簡単な方法の 1 つは、だけフォーム コレクションへのアクセスおよび、ポストされた値を直接取得するコント ローラーの基本クラスの要求プロパティを使用します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

上記の方法が少し verbose、ただし、特にエラー処理ロジックを追加します。

このシナリオでは、組み込みの利用のより優れた方法*UpdateModel()*コント ローラーの基本クラスのヘルパー メソッドです。 受信フォーム パラメーターを使用して渡すオブジェクトのプロパティの更新をサポートします。 リフレクションを使用して、オブジェクトのプロパティの名前を決定し自動的に変換し、クライアントから送信された入力値に基づいて値を割り当てます。

UpdateModel() メソッドを使用して、HTTP POST の編集を使用してアクションこのコードを簡略化する可能性があります。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

私たちがアクセスできるようになりました、 */Dinners/Edit/1* URL、および、夕食のタイトルを変更します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

[保存] ボタンをクリックしてと、編集の操作にポストされたフォームを実行、更新された値は、データベースに保存されます。 おは、Dinner (を新たに保存した値が表示されます) の詳細情報の URL にリダイレクトされます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>編集エラーの処理

場合を除き、現在の HTTP POST 実装の動作微調整 – エラーが発生します。

ユーザーがフォームを編集誤りと、その修正方法が説明するエラーに関する情報メッセージを使用、フォームが再表示するかどうかを確認する必要があります。 場合には、エンドユーザーが正しくない入力を投稿が含まれます (例: 形式が正しくない日付文字列) 入力形式が無効であるケースもが、ビジネス ルールの違反があるように、します。 エラーが発生するフォームが最初にその変更を手動で再読み込みする必要があるない、ようにに入力したユーザーの入力データを保持する必要があります。 フォームが正常に完了するまで、このプロセスが必要な回数だけ繰り返されます。

ASP.NET MVC には、エラー処理とフォームの再表示を簡単にできるようにする便利な組み込みの機能が含まれています。 みましょうアクションでこれらの機能を表示する次のコードで、編集のアクション メソッドを更新します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

作業の周囲 try/catch エラー処理のブロックをラップするようになりましたおする点を除いて、上記のコードは前の実装では – に似ています。 UpdateModel() を呼び出すとき、またはしようとして (これは、例外が、モデル内のルール違反のため、Dinner オブジェクトを保存しようとしましたが無効な場合) DinnerRepository、保存したときに、catch のエラー処理のブロックが、例外が発生した場合実行します。 内にはお Dinner オブジェクト内に存在する任意の規則違反をループし、ModelState オブジェクト (を間もなくについて説明します) に追加します。 ここには、ビューからもう一度です。

操作してみましょうアプリケーションを再実行このを表示するには、夕食を編集し、EventDate"BOGUS"の空のタイトルを持ち、米国の国の値でイギリスの電話番号を使用するように変更します。 [保存] ボタン キーを押すと、HTTP POST の編集メソッドことはできません (エラーがある) ため、Dinner を保存して、フォームを再表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

アプリケーションには、適切なエラー経験があります。 テキスト要素を無効な入力が赤で強調表示し、それらについてエンドユーザーに検証エラー メッセージが表示されます。 フォームも保持している最初のユーザーに入力した – 入力データを何もを補充するがあるないようにします。

方法についてと思うかもしれませんでしたこれが発生しますか。 でしたタイトル、EventDate、および ContactPhone テキスト ボックス自体を赤で強調表示してに最初に入力したユーザーの値を出力する方法 でしたエラー メッセージの取得の表示方法、上部の一覧にしますか。 良いニュースは、ことマジックによるこれが発生しなかったがいくつかの組み込みの ASP.NET MVC 機能を使用する入力の検証とエラー処理シナリオを使用しているではなくです。

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Understanding ModelState と検証の HTML ヘルパー メソッド

コント ローラー クラスには、エラーがビューに渡されるモデル オブジェクトに存在することを指定する手段を提供する"ModelState"プロパティ コレクションがあります。 ModelState コレクション内のエラーのエントリでは、モデル プロパティの名前を特定の問題に (例:「タイトル」、"EventDate"または"ContactPhone")、わかりやすいエラー メッセージを指定することができ (例:「タイトルが必要」) です。

*UpdateModel()*ヘルパー メソッドは、モデル オブジェクトのプロパティをフォームの値を割り当てようとするとエラーが発生したときに自動的に ModelState コレクションを設定します。 たとえば、Dinner オブジェクトの EventDate プロパティは DateTime 型です。 UpdateModel() メソッドは、上記のシナリオで"BOGUS"の文字列値を割り当てることができませんが、そのプロパティを使用して割り当てエラーを示す ModelState コレクションにエントリが発生しました、UpdateModel() メソッドが追加されます。

開発者が行っている下ブロック内で、"catch"エラー処理のアクティブな規則違反に基づいてエントリ ModelState コレクションの読み込みはこれと同じように明示的に ModelState コレクション内のエラー エントリを追加するコードを記述できますも、Dinner オブジェクト:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>ModelState と html ヘルパーの統合

Html.TextBox() - などの HTML ヘルパー メソッドは、出力を表示するときに、ModelState コレクションを確認します。 項目のエラーが存在する場合、ユーザーが入力した値と、CSS エラー クラス レンダリングします。

たとえばで、[編集] ビューは使用 Html.TextBox() ヘルパー メソッド Dinner オブジェクトの EventDate を表示するために。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

エラーのシナリオでは、ビュー、表示されて、Html.TextBox() メソッド ModelState コレクションを参照し、夕食オブジェクトの"EventDate"プロパティに関連付けられているすべてのエラーが発生したがチェックされます。 値として送信されたユーザー入力 ("BOGUS") を表示し、css エラー クラスを追加エラーがあったことが確認された場合、&lt;入力の種類 ="textbox"/&gt;マークアップを生成します。

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

ただし場合を検索する css エラー クラスの外観をカスタマイズできます。 既定の CSS エラー クラス –「の入力の検証エラー」– がで定義されている、 *\content\site.css*下などのスタイル シートとは。

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

このような CSS 規則は、無効な入力要素を次のように強調表示の原因を示します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() ヘルパー メソッド

特定のモデル プロパティに関連付けられた ModelState エラー メッセージを出力する Html.ValidationMessage() ヘルパー メソッドを使用できます。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

上記のコードを出力: *&lt;クラスにまたがる =「フィールドの検証エラー」&gt; 'BOGUS' の値が無効&lt;/span&gt;*

Html.ValidationMessage() ヘルパー メソッドには、表示されるエラー テキスト メッセージを上書きする開発者は、2 番目のパラメーターもサポートしています。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

上記のコードを出力: *&lt;クラスにまたがる =「フィールドの検証エラー」&gt;\*&lt;/span&gt;*エラーの場合、既定のエラー テキストではなく、EventDate プロパティです。

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() ヘルパー メソッド

Html.ValidationSummary() ヘルパー メソッドは、簡単なエラー メッセージを伴うを表示するために使用できる、 &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt;メッセージはすべて詳細なエラーの一覧で、ModelState コレクション:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Html.ValidationSummary() のヘルパー メソッドは、詳細なエラーの一覧の上に表示する概要エラー メッセージを定義する、省略可能な文字列パラメーターを受け取ります。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

エラー一覧の外観をオーバーライドするのに CSS を使用することもできます。

#### <a name="using-a-addruleviolations-helper-method"></a>AddRuleViolations ヘルパー メソッドを使用してください。

初期の HTTP POST を編集実装では、Dinner オブジェクトの規則の違反をループし、コント ローラーの ModelState コレクションに追加の catch ブロック内で、foreach ステートメントを使用しました。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

このコード"ControllerHelpers"を追加することによってほとんどクリーナー NerdDinner プロジェクトにクラスし、メソッドを実装して、"AddRuleViolations"拡張機能内に、ASP.NET MVC ModelStateDictionary クラスにヘルパー メソッドを追加するようにできます。 この拡張メソッドは、ModelStateDictionary RuleViolation エラーの一覧の設定に必要なロジックをカプセル化できます。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

HTTP POST 操作メソッドの編集メソッドを使用してこの拡張機能、夕食規則違反を ModelState コレクションを設定し、更新することができます。

#### <a name="complete-edit-action-method-implementations"></a>アクション メソッドの実装は編集を完了します。

次のコードでは、すべての編集に紹介したシナリオに必要なコント ローラー ロジックを実装します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

編集実装優れている点は、コント ローラー クラスでも、ビュー テンプレート固有の検証や、夕食モデルに設定されているビジネス ルールについて知っています。 おと追加できます追加の規則に、モデル、将来、コント ローラーまたはビューでサポートされるためにそれらの順番にコード変更を加える必要はありません。 これにより、アプリケーション要件は、後で、最小限に抑えてコードの変更内容を簡単に進化する柔軟性が付加されます。

### <a name="create-support"></a>サポートを作成します。

DinnersController クラスの"Edit"動作を実装することが完了しました。 今すぐに進みましょう、– 新しいディナーを追加するユーザーが、これを「作成」のサポートを実装します。

#### <a name="the-http-get-create-action-method"></a>HTTP GET がアクション メソッドを作成します。

最初の HTTP"GET"動作を実装することによって、アクション メソッドを作成します。 アクセスと、このメソッドが呼び出されます、*ディナー/作成*URL。 この実装は、ようになります。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

上記のコードは、新しい Dinner オブジェクトを作成し、今後 1 週間である場合は、その EventDate プロパティを割り当てます。 新しい Dinner オブジェクトに基づいているビューをレンダリングします。 名前を明示的に渡されたしていないため、 *View()*ヘルパー メソッドに使用する規約に基づく既定のパス テンプレートの表示を解決するには:/Views/Dinners/Create.aspx です。

このテンプレートの表示を今すぐ作成してみましょう。 作成アクション メソッド内で右クリックし、「ビューの追加」のコンテキスト メニュー コマンドを選択してこの作業を行うことができます。 「ビューの追加」ダイアログ ボックスでをビュー テンプレートに Dinner オブジェクトを渡すことはおこと、および自動 scaffold の「作成」テンプレートを選択を示します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

「追加」ボタンをクリックして、Visual Studio は新しい scaffold ベース"Create.aspx"ビュー"\Views\Dinners"ディレクトリに保存し、ide 開きます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

みましょういくつかの変更を「作成」scaffold される既定のファイルが生成された、行い、以下のようにアップ変更します。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

今すぐ実行するとき、マイクロソフトのアプリケーションとアクセスと、 *「ディナー/作成」*今回作成アクションの実装から次のような UI を表示する、ブラウザー内の URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>アクション メソッドを作成する HTTP POST を実装します。

おには、実装、作成するアクション メソッドの HTTP GET バージョンがインストールされています。 フォーム ポスト実行ユーザーが [保存] ボタンをクリックしたときに、*ディナー/作成*URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。

実装してみましょうの HTTP POST の動作、アクション メソッドを作成します。 まず、DinnersController 上に"AcceptVerbs"属性があることを示す HTTP POST のシナリオを処理するオーバー ロードされた「作成」アクション メソッドを追加します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

これは、さまざまな方法は、HTTP POST が有効になっている「作成」のメソッド内で、ポストされたフォーム パラメーターにアクセスできます。

新しい Dinner オブジェクトを作成し、使用する方法が、 *UpdateModel()*ヘルパー メソッド (と編集の操作)、ポストされたフォーム値を設定します。 お、DinnerRepository に追加する、データベースに保持し、次のコードを使用して新しく作成された Dinner を表示するには、この詳細アクションにユーザーをリダイレクトします。、

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

または、使用できますアプローチ Dinner オブジェクトのメソッド パラメーターとして取得、Create() アクション メソッドがあります。 ASP.NET MVC は自動的にうえで、新しい Dinner オブジェクトをインスタンス化、フォームの入力を使用してそのプロパティを設定し、アクション メソッドに渡すこと。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

上記のアクション メソッドは、こと Dinner オブジェクトが正常に作成されたフォーム ポスト値を持つ ModelState.IsValid プロパティをチェックして、確認します。 変換の問題の入力がある場合は false が返されます (例: EventDate プロパティの"BOGUS"の文字列)、および、アクション メソッドが、フォームを再問題がある場合。

入力値が有効な場合、アクション メソッドは、追加し、DinnerRepository に新しい Dinner を保存を試みます。 Try ブロックと catch ブロック内でこの作業をラップし、(その結果、dinnerRepository.Save() メソッドで例外が発生する)、ビジネス ルール違反がある場合は、フォームを再表示します。

このエラーの処理の動作の動作を表示するを要求できる、*ディナー/作成*URL と新しい Dinner に関する詳細を記入します。 不適切な入力または値を次のように強調表示されているエラーが再表示するフォームの作成になります。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

フォームを作成する方法は、編集フォームと正確な同じ検証とビジネス規則を記念するために注意してください。 これは、この検証規則とビジネス規則が、モデルで定義されたが、UI またはアプリケーションのコント ローラーには埋め込まれていないためです。 これは後で変更/に応じて変化できる、検証、または 1 つのビジネス ルールを配置して、アプリケーション全体に適用することを意味します。 内で任意のコードを変更するには、編集、または、新しいルールまたは既存のテーブルへの変更が自動的に付与するアクション メソッドを作成することはありません。

入力値を修正し、[保存] ボタンをクリックして、もう一度、DinnerRepository への追記を弊社は成功しますと新しい Dinner をデータベースに追加されます。 ここにリダイレクトされます、 */Dinners 詳細/[id]* URL: 新しく作成された Dinner に関する詳細情報をその場所お表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>サポートを削除します。

当社 DinnersController に"Delete"サポートを今すぐ追加してみましょう。

#### <a name="the-http-get-delete-action-method"></a>Delete アクション メソッドを HTTP GET

まず、HTTP GET、delete アクション メソッドの動作を実装します。 このメソッドへのアクセス時に呼び出される、 */Dinners/削除/[id]* URL。 実装では、次に示します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

アクション メソッドは、削除する Dinner の取得を試みます。 Dinner が存在する場合、レンダリング、表示は、Dinner オブジェクトに基づいています。 オブジェクトが存在しない (または既に削除されている) 場合、"NotFound"をレンダリングするビューを返しますが、"Details"アクション メソッドの前に作成したテンプレートを表示します。

Delete アクション メソッド内で右クリックし、「ビューの追加」のコンテキスト メニュー コマンドを選択して、"Delete"ビューのテンプレートを作成できます。 「ビューの追加」ダイアログ ボックスでをそのモデルと、ビュー テンプレートに Dinner オブジェクトを渡すことはお、および空のテンプレートを作成することを示します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

[追加] ボタンをクリックするおと Visual Studio は新しい"Delete.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国追加します。 いくつかの HTML とコードを追加、削除の確認画面を実装するテンプレートを以下のような。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

上記のコードは、削除する Dinner と出力のタイトルを表示、&lt;フォーム&gt;をエンドユーザーが内で、[削除] ボタンをクリックした場合、/Dinners/削除/[id] URL に POST を行う要素。

このアプリケーションとアクセスを実行するとき、 *"/ディナー/削除/[id]"*オブジェクトの有効な夕食の URL を以下のような UI が表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **側トピックの内容: 理由が行って投稿しますか。** |
| --- |
| 思うかもしれません – 理由でしたを進めていくを作成する作業量、&lt;フォーム&gt;削除の確認画面内ですか? 標準のハイパーリンクを使用だけが実際の削除操作を実行するアクション メソッドへのリンクしないのはなぜですか。 理由は、検索エンジン、Url を検出して、誤ってリンクに従っている場合に削除されるデータの原因と web クローラーから保護するように注意する必要があるためにです。 HTTP GET ベース Url はアクセス/クロール、それらの「安全な」と見なされ、想定どおりに HTTP POST ものに従っていません。 HTTP POST 要求の背後にあるデータの変更操作または破壊を常に配置することを確認することをお勧めします。 |

#### <a name="implementing-the-http-post-delete-action-method"></a>HTTP POST Delete アクション メソッドの実装

お今すぐバージョンである、HTTP GET、Delete アクション メソッドを実装が削除の確認画面が表示されます。 エンドユーザーが [削除] ボタンをクリックしたときにポストされたフォームが実行されます、 */Dinners Dinner/[id]* URL。

次のコードを使用して delete アクション メソッドの HTTP"POST"動作を実装してみましょう。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Delete アクション メソッドの HTTP POST バージョンは、削除する dinner オブジェクトの取得を試みます。 (これが既に削除されているため) ことを見つけられない場合、"NotFound"テンプレートをレンダリングします。 Dinner が見つかった場合、DinnerRepository から削除します。 「削除済み」テンプレートをレンダリングします。

「削除済み」テンプレートを実装するには、アクション メソッドを右クリックし、「ビューの追加」のコンテキスト メニューを選択しておします。 おをビューの「削除済み」という名前をしてそれを空のテンプレート (と厳密に型指定されたモデル オブジェクトになりません)。 いくつかの HTML コンテンツを追加。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

今すぐ実行するとき、マイクロソフトのアプリケーションとアクセスと、 *"/ディナー/削除/[id]"*夕食をレンダリングするオブジェクトの削除の確認の有効な夕食の URL が以下のような画面します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

[削除] ボタンをクリックしたときに HTTP POST が実行されます、 */Dinners/削除/[id]*の URL をデータベースから Dinner を削除し、「削除済み」ビューのテンプレートを表示します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>モデル バインディングのセキュリティ

ASP.NET MVC の組み込みモデル バインド機能を使用する 2 つの方法について説明しました。 最初 UpdateModel() メソッドを使用して、既存のモデル オブジェクトのプロパティを更新して、2 つ目を使用して、アクション メソッドのパラメーターとしてのモデル オブジェクトを渡すための ASP.NET MVC のサポート。 これらのテクニックは、非常に強力な非常に便利です。

この電源がそれに点も担当します。 すべてのユーザー入力を受け入れる場合するセキュリティについてパラノイド常にすることが重要これとはも true フォーム入力にオブジェクトをバインドするとき。 ように注意する必要があります HTML が常に、HTML および JavaScript インジェクション攻撃を回避し、SQL インジェクション攻撃の注意が必要、ユーザーが入力した値をエンコード (注: アプリケーションでは、これらを防ぐためにパラメーターを自動的にエンコードされるの LINQ to SQL を使用します。種類の攻撃)。 ありません、単独で、クライアント側の検証に依存して、常に間違った値を送信しようとしています。 ハッカーから保護するためにサーバー側の検証を使用してください。

ASP.NET MVC のバインド機能を使用するときに考慮するかどうかを確認する追加のセキュリティ項目を 1 つは、バインドするオブジェクトのスコープです。 具体的には、バインドして、実際には更新するエンドユーザーが更新可能なこれらのプロパティのみを許可するかどうかを確認するを許可するプロパティのセキュリティへの影響を理解することを確認します。

既定では、UpdateModel() メソッドが受信フォーム パラメーターの値に一致するモデル オブジェクトのすべてのプロパティを更新しようとするされます。 同様に、または既定では、アクション メソッドのパラメーターとして渡されたオブジェクトのすべてのプロパティ フォーム パラメーターを使用して設定のことができます。

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>ロックダウン状況ベースごとにバインドします。

あたり使用量ベースのバインディング ポリシーを提供して、明示的な"include リスト"更新可能なプロパティのしてロックすることができます。 これは、次のような UpdateModel() メソッドに追加の文字列の配列パラメーターを渡すことによって実行できます。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

また、アクション メソッドのパラメーターとして渡されたオブジェクトは、「インクルード リスト」の下のように指定するプロパティを使用できるようにする [バインド] 属性をサポートします。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>ロックダウン種類ごとにバインドします。

型ごとにバインディング規則をロックダウンすることもできます。 これにより、1 回、バインディング規則を指定し、それらすべてのコント ローラーとアクション メソッド間で (UpdateModel とアクションの両方のメソッド パラメーターのシナリオを含む) すべてのシナリオに適用することができます。

種類のバインディング規則は、型の場合に、[バインド] 属性を追加するか (の型を所有していないシナリオに便利です) のアプリケーションの Global.asax ファイル内で登録することによってカスタマイズできます。 バインド属性の挿入を行うこともできますし、および制御するプロパティを除外するプロパティは、特定のクラスまたはインターフェイスのバインド可能です。

Dinner クラス NerdDinner アプリケーションでは、この手法を使用し、[バインド] 属性を追加するには、次のバインド可能なプロパティの一覧を制限することおします。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

、バインディングによって操作される RSVPs コレクションが許可されていないことに注意してください。 また DinnerID または HostedBy プロパティのバインディングによって設定を許可することはできません。 セキュリティ上の理由からおを代わりにのみ操作、アクション メソッド内で明示的なコードを使用してこれらの特定のプロパティ。

### <a name="crud-wrap-up"></a>CRUD まとめ

ASP.NET MVC には、さまざまなシナリオを投稿するフォームの実装に役立つ組み込みの機能が含まれています。 これらの機能のさまざまなを使用して、DinnerRepository 上に CRUD UI サポートを提供しました。

このアプリケーションを実装するのにモデル中心のアプローチを使用します。 つまり、すべての検証とビジネス ルール ロジック、および、コント ローラーまたはビュー内ではなく、モデル レイヤー内で定義されます。 コント ローラー クラスでも、テンプレートの表示に関する知識がまったく Dinner モデル クラスに設定されている特定のビジネス ルール。

これは、マイクロソフト アプリケーション アーキテクチャ クリーンし、テストを容易にできるようにします。 追加できるその他のビジネス ルール、モデル レイヤー今後と*を任意のコード変更を加える必要*をサポートするのには、コント ローラーまたはそれらの順序で表示します。 これは、進化し、アプリケーションでは、将来の変更に機敏性を大幅に向上にご提供する中です。

当社 DinnersController ようになりましたにより、Dinner 番組表/詳細については、だけでなくを作成、編集および削除のサポート。 以下は、クラスの完全なコードを確認できます。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>次の手順

DinnersController クラス内での実装の基本的な CRUD (作成、読み取り、更新、削除) サポートがあるようになりました。

これで、フォームでより豊富な UI を有効にする ViewData および ViewModel クラスを使用できる方法を見てみましょう。

>[!div class="step-by-step"]
[前へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[次へ](use-viewdata-and-implement-viewmodel-classes.md)
