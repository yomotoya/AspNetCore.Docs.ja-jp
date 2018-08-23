---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: 提供の CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート |Microsoft Docs
author: microsoft
description: 手順 5 では、編集、作成、およびられて Dinners を削除すると、同様のサポートを有効にして、DinnersController クラスをさらにする方法を示します。
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 45d74249a34fc7e37e9776a398615d2f613a7582
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833556"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>提供の CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 5 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 5 では、編集、作成、およびられて Dinners を削除すると、同様のサポートを有効にして、DinnersController クラスをさらにする方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner 手順 5: 作成、更新、削除のフォームのシナリオ

コント ローラーとビューを導入し、それらを使用してサイトの Dinners のリスティング/詳細エクスペリエンスを実装する方法について説明しました。 次の手順をさらに、DinnersController クラスを取得して編集、作成、およびられて Dinners を削除すると、同様のサポートを有効になります。

### <a name="urls-handled-by-dinnerscontroller"></a>DinnersController によって処理される Url

以前に 2 つの Url のサポートを実装した DinnersController アクション メソッドを追加しました: */Dinners*と */Dinners 詳細/[id]* します。

| **URL** | **動詞** | **目的** |
| --- | --- | --- |
| */Dinners/* | GET | 今後の dinners の HTML リストを表示します。 |
| */Dinners 詳細/[id]* | GET | 特定の dinner に関する詳細を表示します。 |

これで 3 つの追加の Url を実装するアクション メソッドを追加します: <em>/Dinners/編集/[id]、/Dinners/作成、</em>と<em>/Dinners/削除/[id]</em>します。 これらの Url は、新しい Dinners を作成および Dinners を削除する編集の既存 Dinners のサポートを有効になります。

私たちは、これらの新しい Url に HTTP GET と HTTP POST の両方の動詞相互作用をサポートします。 これらの Url に HTTP GET 要求 ("edit"の場合、Dinner データ設定フォーム、「作成」の場合、空のフォームおよび「削除」の場合、削除の確認画面) のデータの初期の HTML ビューが表示されます。 これらの Url に HTTP POST 要求は、Dinner のデータのときは必ず DinnerRepository (および、データベースにそこから) を保存/更新/削除になります。

| **URL** | **動詞** | **目的** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | Dinner データが設定の編集可能な HTML フォームを表示します。 |
| POST | 特定の夕食をデータベースには、フォームの変更を保存します。 |
| */Dinners/Create* | GET | ユーザーが新しい Dinners を定義できる空の HTML フォームを表示します。 |
| POST | 新しい夕食を作成し、データベースに保存します。 |
| */Dinners/Delete/[id]* | GET | 削除の確認画面を表示します。 |
| POST | データベースから指定の夕食を削除します。 |

### <a name="edit-support"></a>編集のサポート

"Edit"シナリオの実装を見てみましょう。

#### <a name="the-http-get-edit-action-method"></a>HTTP GET の Edit アクション メソッド

Edit アクション メソッドの HTTP"GET"動作を実装することから始めます。 このメソッドになりますされると呼び出されます、 */Dinners/編集/[id]* URL を要求します。 今回の実装は、ようになります。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

上記のコードでは、Dinner オブジェクトを取得するのにときは、必ず DinnerRepository を使用します。 Dinner オブジェクトを使用してビュー テンプレートをレンダリングします。 テンプレート名を明示的に渡されたしていないため、 *View()* ヘルパー メソッドをテンプレートの表示を解決するのには規約ベースの既定パスを使用には:/Views/Dinners/Edit.aspx します。

このテンプレートの表示を今すぐ作成しましょう。 Edit メソッド内で右クリックし、「ビューの追加」コンテキスト メニュー コマンドを選択して行います。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

"ビューの追加 ダイアログ ボックスで指定する Dinner オブジェクトとしてのモデル、ビュー テンプレートに渡すし、自動スキャフォールディングを「編集」テンプレートを選択します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

[追加] ボタンをクリックすると、ときに Visual Studio は新しい"Edit.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内を追加します。 初期「編集」スキャフォールディング実装すると、以下を含むコードのエディター – 内に、新しい"Edit.aspx"ビュー テンプレートも開きます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

生成されると、既定値"edit"スキャフォールディングにいくつかの変更を加えるし、(いくつかのプロパティを公開する必要はありませんが削除されます) をコンテンツの編集ビュー テンプレートを更新しましょう。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

アプリケーションと要求を実行したときに、 *「/1/Dinners/編集」* URL は、次のページが表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

このビューによって生成される HTML マークアップは、以下のようになります。 – 標準の HTML では、&lt;フォーム&gt;要素への HTTP POST を実行する、 */Dinners/Edit/1* URL と [保存]&lt;入力の種類 =「送信」/&gt;ボタンが押されました。 HTML&lt;入力の種類 ="text"/&gt;要素には、編集可能な各プロパティの出力がされています。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() と Html.TextBox() Html ヘルパー メソッド

"Edit.aspx"ビュー テンプレートがいくつかの「Html ヘルパー」メソッドを使用して: Html.ValidationSummary()、Html.BeginForm()、Html.TextBox()、および Html.ValidationMessage() します。 組み込みのエラー処理と検証の HTML マークアップを生成する、に加えてこれらのヘルパー メソッドを提供をサポートします。

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() ヘルパー メソッド

Html.BeginForm() のヘルパー メソッドは、どのような出力 HTML&lt;フォーム&gt;マークアップ要素。 Edit.aspx ビュー テンプレートに適用する"using"ステートメントと、このメソッドを使用して C# でわかります。 中かっこの先頭を示します、&lt;フォーム&gt;コンテンツ、および右中かっこがの末尾を示す内容、 &lt;/form&gt;要素。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

また、"using"ステートメントを検索する場合は、このようなシナリオの不自然なアプローチ、Html.BeginForm() と Html.EndForm() を組み合わせて (これは、同じ動作が) を使用することができます。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

パラメーターを指定せず Html.BeginForm() を呼び出すことは現在の要求の URL への HTTP POST をフォーム要素を出力することになります。 つまり、編集ビューを生成理由、 *&lt;フォーム アクション =「/1/Dinners/編集」メソッド ="post"&gt;* 要素。 でしたがまたは明示的なパラメーターに渡した Html.BeginForm() 別の URL に投稿する場合。

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() ヘルパー メソッド

この Edit.aspx ビュー Html.TextBox() のヘルパー メソッドを使用して出力する&lt;入力の種類 ="text"/&gt;要素。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

上記 Html.TextBox() メソッドには、1 つのパラメーター – の id/名前属性を指定するために使用される、&lt;入力の種類 ="text"/&gt;からテキスト ボックスに値を設定するモデルのプロパティと同様に、出力する要素。 たとえば、編集ビューに渡された Dinner オブジェクトが".NET Future"の"Title"プロパティの値と、Html.TextBox("Title") メソッドが出力を呼び出すため: *&lt;入力 id ="Title"名前 =「タイトル」の種類 ="text"の値 =".NET Future"/&gt;*.

または、要素の id/名前を指定し、2 番目のパラメーターとして使用する値で明示的に渡す Html.TextBox() の最初のパラメーターを使用できます。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

多くの場合、出力される値のカスタム書式設定を実行するします。 .NET に組み込まれて指定する静的メソッドは、これらのシナリオに適しています。 Edit.aspx ビュー テンプレートを使用してこの書式、EventDate 値 (DateTime 型の) 時間の秒を表示しないように。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() の 3 番目のパラメーターは、追加の HTML 属性を出力するオプションで使用できます。 次のコードのスニペットは、その他のサイズを表示する方法を示します「30」属性とクラスを = = on"mycssclass"属性、&lt;入力の種類 ="text"/&gt;要素。 使用して、クラス属性の名前をエスケープする方法に注意してください、"@" character because "クラス"予約済みキーワード (C#)。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>HTTP POST の Edit アクション メソッドを実装します。

HTTP GET バージョン、Edit アクション メソッドの実装のようになりましたがあります。 ユーザーが要求したとき、 */Dinners/Edit/1*次のような HTML ページを受信した URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

フォーム post を「保存」ボタンを押すと、 */Dinners/Edit/1* URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。 今すぐ – Dinner の保存を処理する、edit アクション メソッドの HTTP POST の動作を実装してみましょう。

まず、DinnersController を"AcceptVerbs"属性を持つ HTTP POST のシナリオを扱うことを示すを「編集」アクション オーバー ロードされたメソッドを追加します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

[AcceptVerbs] 属性は、オーバー ロードされたアクション メソッドに適用するときに ASP.NET MVC は自動的に入力方向の HTTP 動詞に応じて適切なアクション メソッドにディスパッチ要求を処理します。 HTTP POST 要求を<em>/Dinners/編集/[id]</em> Url への他のすべての HTTP 動詞要求中に、上記の Edit メソッドに移動します<em>/Dinners/編集/[id]</em>Url が変わります (これが最初の編集メソッドを実装しましたいない属性を持つ [AcceptVerbs])。

| **側のトピック: は、HTTP 動詞を使用して区別理由でしょうか。** |
| --- |
| 思うかもしれません – はなぜ 1 つの URL を使用して、HTTP 動詞を使用してその動作を区別しますか。 なぜの読み込みと編集の変更の保存を処理するために 2 つの個別の Url があるだけですか。 例:/Dinners/編集/[id] の初期フォームと/Dinners/保存/[id] を保存してフォーム ポストを処理するために表示するか。 発行の 2 つの独立した Url に欠点最終的には、/Dinners/Save/2 に投稿し、入力エラーのための HTML フォームを再表示する必要があります場所の場合、エンドユーザーが (だったために、ブラウザーのアドレス バーに Dinners/保存/2 の URL を持つURL にポストされたフォーム)。 場合は、エンド ユーザー ブラウザーのお気に入りの一覧にこの redisplayed ページをブックマークまたは URL のコピー/貼り付け、友人にメールで送信 (ため、その URL は、投稿の値によって異なります) は、後で動作しない URL を保存になります。 1 つの URL を公開することで (のような:/Dinners/Edit/[id]) および HTTP 動詞でその処理を区別するの編集ページをブックマークや他のユーザーに、URL を送信するエンドユーザーのも安全です。 |

#### <a name="retrieving-form-post-values"></a>フォーム ポスト値を取得します。

さまざまなフォーム パラメーター、HTTP POST の「編集」メソッド内での投稿にアクセスする方法があります。 単純なアプローチの 1 つだけコント ローラーの基本クラスで要求のプロパティを使用して、フォームのコレクションへのアクセスを直接ポストされた値を取得することです。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

上記の方法は特に、エラー処理ロジックを追加した、少し冗長です。

このシナリオでは、組み込みの活用のより優れた方法*UpdateModel()* コント ローラーの基本クラスのヘルパー メソッド。 渡します受信フォーム パラメーターを使用してオブジェクトのプロパティの更新をサポートします。 リフレクションを使用して、オブジェクトのプロパティ名を決定して自動的に変換し、クライアントから送信された入力値に基づいて値を割り当てます。

UpdateModel() メソッドを使用して、HTTP POST アクションの編集このコードを使用して簡素化でした。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

アクセスできるようになりました、 */Dinners/Edit/1* URL、および、Dinner のタイトルを変更します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

[保存] ボタンをクリックしてとポストされたフォームの編集の操作を実行します、更新された値は、データベースに保存されます。 私たちは、夕食 (これは、新しく保存された値が表示されます) の詳細 URL にリダイレクトされます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>編集エラーの処理

場合を除き、現在の HTTP POST 実装の動作を細かく – エラーが発生します。

ユーザーが間違いを犯したフォームを編集すると、フォームが問題の修正をガイドするわかりやすいエラー メッセージと共に再表示するかどうかを確認する必要があります。 エンドユーザーが正しくない入力を投稿するケースが含まれます (例: 形式が正しくない日付の文字列)、あるケースも入力形式は有効ですが、ビジネス ルール違反があります。 エラーが発生した場合、フォームにその変更を手動で再読み込みするがあるないように最初に入力したユーザーの入力データを保持する必要があります。 フォームが正常に完了するまで、このプロセスが必要な回数だけ繰り返されます。

ASP.NET MVC には、エラー処理とフォームの再表示を簡単に構成するいくつかの便利な組み込み機能が含まれています。 みましょうアクションでこれらの機能を表示する次のコードで、Edit アクション メソッドを更新します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

上記のコードは、前の実装に似ていますが、作業を try/catch エラー処理のブロックをラップするようになりました。 UpdateModel() を呼び出すときに、またはしようとして、ときは必ず DinnerRepository (これが、私たちのモデル内のルール違反のため、Dinner オブジェクトを保存しようとしましたが無効な場合、例外が発生)、保存時に、ブロックの catch エラー処理、例外が発生した場合は実行します。 内にループに Dinner オブジェクト内に存在する任意の規則違反の上を (すぐ説明します) する ModelState オブジェクトに追加します。 ここには、ビュー、再表示します。

早速アプリケーションを再実行動作しているこのかの夕食を編集し、EventDate"BOGUS"の空のタイトルがあり、米国の国の値で英国の電話番号を使用するように変更します。 キーを押すと [保存] ボタンと HTTP POST の編集方法では、ことはできません (エラーがある) ため、Dinner を保存して、フォームを再表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

このアプリケーションでは、適切なエラー エクスペリエンスがあります。 入力に無効なテキスト要素が赤で強調表示し、それらについてエンドユーザーに検証エラー メッセージが表示されます。 フォームも保持しているユーザーの元の入力: 入力データを何も再設定があるないようにします。

方法については、疑問に思うかもしれませんがこれが発生するでしょうか。 方法でした、タイトル、EventDate、および ContactPhone のテキスト ボックス自体を赤で強調表示し、最初に入力したユーザーの値を出力するでしょうか。 方法がエラー メッセージを取得一覧に表示、上部にあるでしょうか。 良い知らせは、あるマジックでこれが発生しなかったがいくつかの組み込みの ASP.NET MVC 機能を簡単にする入力の検証とエラー処理シナリオを使用しているためではなくです。

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>ModelState の理解と検証の HTML ヘルパー メソッド

コント ローラー クラスでは、エラーがビューに渡されるモデル オブジェクトに存在することを示す方法を提供する"ModelState"プロパティ コレクションがあります。 ModelState コレクション内のエラーのエントリでは、モデル プロパティの名前を識別して問題 (例:"Title"、"EventDate"または"ContactPhone")、人間にとってわかりやすいエラー メッセージを指定することができ (例:「タイトルが必要です」)。

*UpdateModel()* モデル オブジェクトのプロパティにフォームの値を代入しようとしています。 中にエラーが発生したときにヘルパー メソッドが ModelState コレクションに自動的に設定します。 たとえば、Dinner オブジェクトの EventDate プロパティは DateTime 型です。 UpdateModel() メソッドは、上記のシナリオで"BOGUS"の文字列値を割り当てることができませんが、そのプロパティに割り当てエラーを示す ModelState コレクションにエントリが発生しました UpdateModel() メソッドが追加されます。

開発者は、以下、"catch"エラーの処理ブロック内でエントリで、アクティブなルール違反に基づく ModelState コレクションが読み込みを実行しているように明示的に ModelState コレクションへのエラー エントリを追加するコードを記述できますも、Dinner オブジェクト:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>ModelState と html ヘルパーの統合

Html.TextBox() - などの HTML ヘルパー メソッドは、出力を表示するときに、ModelState のコレクションを確認します。 項目のエラーが存在する場合、ユーザーが入力した値と CSS エラー クラス レンダリングします。

たとえばで、[編集] ビューは使用 Html.TextBox() ヘルパー メソッドは、Dinner オブジェクトの EventDate を表示するために。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

エラー シナリオでは、ビューがレンダリングされた、Html.TextBox() メソッドで ModelState コレクションは、Dinner オブジェクトの"EventDate"プロパティに関連付けられたすべてのエラーがないかどうがチェックされます。 値として送信されたユーザー入力 ("BOGUS") を表示し、css エラー クラスを追加エラーがあったことが確認された場合、&lt;入力の種類 ="textbox"/&gt;が生成されたマークアップ。

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

ただしすることを確認する css エラー クラスの外観をカスタマイズすることができます。 既定の CSS エラー クラス –「入力の検証エラー」-がで定義されている、 *\content\site.css*スタイル シートとは次のような。

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

この CSS ルールでは、以下のように強調表示する、無効な入力要素の原因を示します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() ヘルパー メソッド

特定のモデル プロパティに関連付けられた ModelState エラー メッセージを出力する Html.ValidationMessage() ヘルパー メソッドを使用できます。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

上記のコードを出力: *&lt;クラスにまたがる =「フィールドの検証エラー」&gt; 'BOGUS' 値が有効でない&lt;/span&gt;*

Html.ValidationMessage() のヘルパー メソッドには、表示されるエラー テキスト メッセージを上書きできるようにする 2 番目のパラメーターもサポートしています。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

上記のコードを出力: <em>&lt;クラスにまたがる =「フィールドの検証エラー」&gt;\*&lt;/span&gt;</em>エラーの場合、既定のエラー テキストではなく、EventDate のプロパティです。

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() ヘルパー メソッド

Html.ValidationSummary() のヘルパー メソッドを伴う概要エラー メッセージを表示するために使用できます、 &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt;でメッセージをすべて詳細なエラーの一覧、ModelState コレクション:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Html.ValidationSummary() のヘルパー メソッドでは、詳細なエラーの一覧の上に表示される概要エラー メッセージを定義する、省略可能な文字列パラメーターを受け取ります。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

必要に応じて CSS を使用して、エラー一覧のようにオーバーライドすることができます。

#### <a name="using-a-addruleviolations-helper-method"></a>AddRuleViolations ヘルパー メソッドを使用してください。

最初の HTTP POST 編集実装では、Dinner オブジェクトの規則違反をループし、コント ローラーの ModelState のコレクションに追加するその catch ブロック内での foreach ステートメントを使用しました。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

"ControllerHelpers"を追加することによってほとんど明確になり NerdDinner プロジェクトにクラスし、メソッドを実装して"AddRuleViolations"拡張機能内に ASP.NET MVC ModelStateDictionary クラスにヘルパー メソッドを追加する、このコードを実行できます。 この拡張メソッドは、ModelStateDictionary RuleViolation エラーの一覧を設定するために必要なロジックをカプセル化できます。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

HTTP POST アクション メソッドの編集、Dinner 規則違反で ModelState のコレクションを設定するこの拡張メソッドを使用することを更新できます。

#### <a name="complete-edit-action-method-implementations"></a>Edit アクション メソッドの実装を完了します。

次のコードは、編集シナリオに必要なコント ローラー ロジックのすべてを実装します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

編集実装の便利な点は、コント ローラー クラスでも、テンプレートの表示が、特定の検証や、Dinner モデルに設定されているビジネス ルールについての知識が。 追加ルールを追加できます、モデル、今後おに、コント ローラーまたはビューをサポートするためにコード変更を加える必要はありません。 これにより、アプリケーションの要件、将来を最小限のコードの変更を簡単に進化する柔軟性を活用できます。

### <a name="create-support"></a>サポートを作成します。

DinnersController クラスの"Edit"の動作を実装するは終了しました。 今すぐに進みましょうが – 新しい Dinners を追加するユーザーを有効にする「作成」のサポートを実装します。

#### <a name="the-http-get-create-action-method"></a>HTTP GET アクション メソッドを作成します。

まずの HTTP"GET"動作を実装することによって、アクション メソッドを作成します。 アクセスと、このメソッドが呼び出される、 */Dinners/作成*URL。 今回の実装は、ようになります。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

上記のコードでは、新しい Dinner オブジェクトを作成し、今後 1 週間に、EventDate のプロパティを割り当てます。 新しい Dinner オブジェクトに基づいているビューをレンダリングします。 名を明示的に渡されたしていないため、 *View()* ヘルパー メソッドをテンプレートの表示を解決するのには規約ベースの既定パスを使用には:/Views/Dinners/Create.aspx します。

このテンプレートの表示を今すぐ作成しましょう。 これは、Create アクション メソッド内で右クリックし、「ビューの追加」コンテキスト メニュー コマンドを選択して実行できます。 "ビューの追加 ダイアログ ボックスで指定する Dinner オブジェクト、ビュー テンプレートに渡すし、自動スキャフォールディングの「作成」テンプレートを選択します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

[追加] ボタンをクリックすると、ときに、Visual Studio は新しいスキャフォールディング ベース"Create.aspx"ビューを"\Views\Dinners"ディレクトリに保存し、ide の起動。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

既定値「作成」スキャフォールディングの生成されたファイルに、いくつかの変更を加えるし、以下のように変更をしましょう。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

今すぐ実行するとき、アプリケーションとアクセスと、 *「Dinners/作成」* 作成アクションの実装から次のような UI を表示するブラウザー内の URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>アクション メソッドを作成する HTTP POST を実装します。

私たちには、実装、Create アクション メソッドの HTTP GET バージョンがインストールされています。 フォーム post を実行、ユーザーが [保存] ボタンをクリックすると、 */Dinners/作成*URL、HTML を送信して&lt;入力&gt;HTTP POST 動詞を使用して値を形成します。

ここでの HTTP POST の動作を実装してみましょう、アクション メソッドを作成します。 まず、DinnersController を"AcceptVerbs"属性を持つ HTTP POST のシナリオを扱うことを示すをオーバー ロードされたの"Create"のアクション メソッドを追加します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

これは、さまざまな方法は、HTTP POST が有効になっている"Create"メソッド内で、ポストされたフォーム パラメーターにアクセスできます。

1 つのアプローチは、新しい Dinner オブジェクトを作成し、使用する、 *UpdateModel()* (編集の操作を使用したとき) などのヘルパー メソッドにポストされたフォーム値を設定します。 私たちのときは必ず DinnerRepository に追加、データベースに保存し、次のコードを使用して、新しく作成された Dinner を表示するには、当社の詳細アクションにユーザーをリダイレクトします。、

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

または、アプローチを使用しました、メソッド パラメーターとして Dinner オブジェクト Create() アクション メソッドは、あります。 ASP.NET MVC は自動的に私たちにとって新しい Dinner オブジェクトをインスタンス化、フォームの入力を使用してそのプロパティを設定し、アクション メソッドに渡します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

ある Dinner オブジェクトが正常に値が設定されて、フォーム post ModelState.IsValid プロパティをチェックして、上記のアクション メソッドを確認します。 変換の問題の入力がある場合は false が返されます (例: EventDate のプロパティの"BOGUS"の文字列)、し、問題がある場合、アクション メソッドはフォームを再表示します。

入力値が有効な場合、アクション メソッドは、追加し、新しい夕食をときは必ず DinnerRepository に保存を試みます。 Try/catch ブロック内でこの作業をラップし、(例外を発生させる dinnerRepository.Save() メソッドになる) ため、ビジネス ルール違反がある場合は、フォームを再表示します。

このエラーの処理の動作の動作を確認することを要求できます、 */Dinners/作成*URL と新しい夕食について詳細に記入します。 不適切な入力または値には、次のように強調表示されたエラーと共に再表示するフォームを作成するが発生します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

フォームを作成するによって、編集フォームとして正確な同じ検証規則とビジネス規則が受け入れることに注意してください。 これは、検証とビジネス ルールがモデルでは、定義された、UI またはアプリケーションのコント ローラー内で埋め込まれていないためです。 これは後で変更/進化させる、検証、または 1 つのビジネス ルールに置き、アプリケーション全体で適用することがあることを意味します。 私たちは、内いずれかのコードは、編集を変更したり、新しいルールまたは既存の変更を自動的に優先するアクション メソッドを作成する必要はありません。

入力の値を修正し、[保存] ボタンをクリックします。 もう一度、追加するときは、必ず DinnerRepository は成功しますと新しい夕食をデータベースに追加されます。 ここにリダイレクトされますが、 */Dinners 詳細/[id]* URL – の詳細については、新しく作成された Dinner が、その場所表示されます。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Delete のサポート

DinnersController を「削除」のサポートを今すぐ追加してみましょう。

#### <a name="the-http-get-delete-action-method"></a>Delete アクション メソッドを HTTP GET

まず、HTTP GET、delete アクション メソッドの動作を実装します。 このメソッドへのアクセス時に呼び出される、 */Dinners/削除/[id]* URL。 実装を次に示します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

アクション メソッドは、削除する Dinner の取得を試みます。 Dinner が存在する場合、レンダリング Dinner オブジェクトにビューを基づいています。 オブジェクトが存在しない (または既に削除されている) 場合、"NotFound"をレンダリングするビューを返しますが、"Details"アクション メソッドの前に作成したテンプレートを表示します。

Delete アクション メソッド内で右クリックし、「ビューの追加」コンテキスト メニュー コマンドを選択して、「削除」ビュー テンプレートを作成できます。 "ビューの追加 ダイアログ ボックスで Dinner オブジェクトとしてのモデル、ビュー テンプレートに渡すと、空のテンプレートを作成することを指定します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

[追加] ボタンをクリックするとと、Visual Studio は新しい"Delete.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内の追加します。 削除の確認 画面を実装するためにテンプレートをいくつかの HTML とコードを追加します次のような。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

上記のコードは、削除するには、Dinner と出力のタイトルを表示、&lt;フォーム&gt;要素を/Dinners/削除/[id] URL に投稿を行う場合は、エンドユーザーがその中の [削除] ボタンをクリックします。

当社のアプリケーションとアクセスを実行したときに、 *"/Dinners/削除/[id]"* オブジェクトの有効な夕食 URL 次のような UI をレンダリングします。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **側のトピックは、理由をしている投稿ですか。** |
| --- |
| 作成する作業を通じて移動でした理由: 依頼可能性があります、&lt;フォーム&gt;削除の確認画面内か? なぜ実際の削除操作を実行するアクション メソッドにリンクするだけの標準のハイパーリンクを使用しますか。 Web クローラーからの保護し、検索エンジン、Url を探索し、リンクに従っている場合に削除されるデータを誤って原因に注意する必要があるためにです。 HTTP GET ベースの Url アクセス/クロールにするには「安全」と見なされに、HTTP POST ものに従っていません。 HTTP POST 要求の背後にあるデータ変更操作または破壊を常に配置することを確認することをお勧めします。 |

#### <a name="implementing-the-http-post-delete-action-method"></a>Delete アクション メソッドを HTTP POST を実装します。

HTTP GET、Delete アクション メソッドの実装のバージョンが削除の確認 画面を表示しますがあるようになりました。 フォーム post が実行されます、エンドユーザーが [削除] ボタンをクリックすると、 */Dinners Dinner/[id]* URL。

みましょう delete アクション メソッドを次のコードを使用して HTTP"POST"動作を実装するようになりました。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Delete アクション メソッドを当社の HTTP POST のバージョンは、削除する dinner オブジェクトを取得しようとします。 (既に削除されている) ために検出できない場合、"NotFound"テンプレートを表示します。 Dinner を検出した場合、ときは必ず DinnerRepository から削除します。 「削除済み」テンプレートをレンダリングします。

「削除済み」テンプレートを実装するには、アクション メソッドで右クリックし、「ビューの追加」コンテキスト メニューを選択してします。 このビューの「削除済み」という名前がされ空のテンプレート (および厳密に型指定されたモデル オブジェクトにならない) ことがあります。 いくつかの HTML コンテンツを追加します。

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

今すぐ実行するとき、アプリケーションとアクセスと、 *"/Dinners/削除/[id]"* 夕食を表示するオブジェクトの削除の確認、有効な Dinner の URL は以下のような画面します。

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

[削除] ボタンをクリックしたときに HTTP POST が実行されます、 */Dinners/削除/[id]* Dinner をデータベースから削除され、表示、「削除済み」のビュー テンプレートの URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>モデル バインディングのセキュリティ

ASP.NET MVC の組み込みのモデル バインド機能を使用する 2 つの方法を説明しました。 最初 UpdateModel() メソッドを使用して、既存のモデル オブジェクトのプロパティを更新して、2 つ目を使用してアクション メソッドのパラメーターとしてのモデル オブジェクトを渡すための ASP.NET MVC のサポート。 これらのテクニックは、非常に強力で非常に便利です。

この power も伴います。 この責任です。 すべてのユーザー入力を受け入れるときにセキュリティに関する妄想常にすることが重要とフォームの入力にオブジェクトをバインドする場合は true。 これはもします。 ように注意する必要があります常に、HTML エンコードを HTML および JavaScript インジェクション攻撃を回避し、SQL インジェクション攻撃注意が必要なユーザーが入力した値 (注: このアプリケーションでは、これらを防ぐためにパラメーターを自動的にエンコード LINQ to SQL を使用しています種類の攻撃)。 偽の値を送信しようとするハッカーから保護するためのサーバー側の検証を常に採用できるにクライアント側の検証だけで、依存する必要があることはありません。

ASP.NET MVC のバインド機能を使用する場合について検討するかどうかを確認する 1 つの追加のセキュリティ項目は、バインドしているオブジェクトのスコープです。 具体的には、バインド、および更新するには、エンドユーザーが更新可能な実際にする必要があるこれらのプロパティのみを許可するかどうかを確認することが許可されているプロパティのセキュリティの影響を理解することを確認します。

既定では、UpdateModel() メソッドは受信フォーム パラメーターの値に一致するモデル オブジェクトのすべてのプロパティの更新を試みます。 同様に、または既定では、アクション メソッドのパラメーターとして渡されたオブジェクトすべてのプロパティはフォーム パラメーターを使用して設定のことができます。

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>あたり使用量ベースのバインディングのロックダウン

提供する、明示的な「インクルード リスト」を更新できるプロパティのあたり使用量ベースのバインディング ポリシーをロックダウンすることができます。 これは、次のような UpdateModel() メソッドに余分な文字列の配列パラメーターを渡すことによって実行できます。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

「インクルード リスト」のプロパティを次のように指定できるを許可できるようにする [バインド] 属性をサポートしても、アクション メソッドのパラメーターとして渡されるオブジェクト。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>バインドの種類ごとにロックダウン

型ごとにバインディング規則をロックダウンすることもできます。 これにより、1 回、バインディング規則を指定し、それらすべてのコント ローラーとアクション メソッド間で (UpdateModel とアクションの両方のメソッド パラメーターのシナリオを含む) すべてのシナリオで適用できます。

型の場合に、[バインド] 属性を追加することで、または (種類を所有していない場合に便利です)、アプリケーションの Global.asax ファイル内で登録することによって、型のバインディング規則をカスタマイズできます。 プロパティを制御するプロパティを除外するは、特定のクラスまたはインターフェイスのバインド可能なとをバインド属性のインクルードを使用できます。

NerdDinner アプリケーションでは、Dinner クラスのこの手法を使用して、され、次のバインド可能なプロパティの一覧を制限することを [Bind] 属性を追加します。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

バインディング、によって操作される予約したコレクションが許可されていないことも、DinnerID または HostedBy プロパティのバインドを使用して設定が許可されていることに注意してください。 セキュリティ上の理由からを代わりにのみ操作、アクション メソッド内で明示的なコードを使用してこれらの特定のプロパティ。

### <a name="crud-wrap-up"></a>CRUD のまとめ

ASP.NET MVC には、さまざまなフォーム ポスト シナリオの実装に役立つ組み込みの機能が含まれています。 このときは必ず DinnerRepository 上に CRUD UI サポートを提供するさまざまなこれらの機能を使いました。

モデルに重点を置いたアプローチは、アプリケーションを実装するために使用します。 つまり、すべての検証とビジネス ルールのロジック、および、コント ローラーまたはビュー内ではなく、モデル レイヤー内で定義されます。 コント ローラー クラスでも、テンプレートの表示、Dinner モデル クラスに設定されている特定のビジネス ルールに関する知識します。

アプリケーション アーキテクチャをクリーンに維持を容易にテストします。 他のビジネス ルールを将来、モデル レイヤーに追加しましたできますと*コード変更を加える必要がなく*をサポートするのには、コント ローラーまたはビューでそれらの順番にします。 これで、大きな進化し、アプリケーションに将来の変更に機敏性を提供します。

DinnersController ようになりましたにより、Dinner 番組/詳細、だけでなくを作成、編集および削除のサポート。 クラスの完全なコードは、以下をあります。

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>次の手順

基本的な CRUD (作成、読み取り、更新、削除) サポートが、DinnersController クラス内で実装があるようになりました。

これで、ViewData とビューモデル クラスを使用できます、フォームでより豊富な UI を有効にする方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [次へ](use-viewdata-and-implement-viewmodel-classes.md)
