---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: AJAX を使用して、動的更新を配信する |Microsoft Docs
author: microsoft
description: 手順 10 の実装は、RSVP にログインしているユーザーの dinner 詳細内で統合された Ajax ベースのアプローチを使用して、夕食に参加している関心をサポート.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825202"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>AJAX を使用して、動的更新を配信するには
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 10 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 10 の実装では、dinner の詳細 ページ内で統合された Ajax ベースのアプローチを使用して、夕食に参加している関心 RSVP にログインしているユーザーのサポートします。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner 手順 10: AJAX 予約の有効化を受け入れる

今すぐ RSVP を夕食に参加している関心にログインしているユーザー向けのサポートを実装してみましょう。 有効にしますこの dinner の詳細 ページ内で統合された AJAX ベースのアプローチを使用します。

### <a name="indicating-whether-the-user-is-rsvpd"></a>ユーザーに対する RSVP があるかどうかを示す

ユーザーがアクセスできる、 */Dinners/詳細/[id*] 特定の dinner に関する詳細を表示する URL:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

アクション メソッドが実装されている Details() ようになります。

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Dinner オブジェクト (前に作成した Dinner.cs 部分クラス) 内"IsUserRegistered(username)"ヘルパー メソッドを追加する RSVP のサポートを実装する、最初のステップになります。 このヘルパー メソッドは、true または false かどうか、ユーザーが現在に対する RSVP、夕食によって返されます。

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

次のコードまたはイベントではなく、ユーザーが登録されているかどうかを示す適切なメッセージを表示する、Details.aspx ビュー テンプレートに追加できます。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

これでユーザーに登録されている夕食にアクセスしたとき、メッセージが表示されますこの。

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

見られるは登録されません夕食を訪問するときに、次のメッセージ。

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>登録アクション メソッドの実装

今すぐ RSVP を夕食の詳細ページからユーザーを有効にするために必要な機能を追加してみましょう。

を実装する、\Controllers ディレクトリを右クリックし、[追加]-選択して、新しい"RSVPController"クラスを作成します、&gt;コント ローラーのメニュー コマンド。

引数としての Dinner の id を受け取り、適切な Dinner オブジェクトの場合とでは、ログインのユーザーが現在、登録したユーザーの一覧である場合にチェックを取得する新しい RSVPController クラス内での"Register"アクション メソッドを実装しますそれらの RSVP オブジェクトを追加できません。

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

上記のアクション メソッドの出力として、単純な文字列を返す方法に注意してください。 ビュー テンプレート – 内でこのメッセージを埋め込むことでしたが、コント ローラーの基本クラスを戻り値の上などの文字列メッセージ Content() ヘルパー メソッドを使用しますだけは非常に小さいため。

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>AJAX を使用して RSVPForEvent アクション メソッドを呼び出す

AJAX、詳細ビューから登録アクション メソッドの呼び出しに使用します。 この実装は非常に簡単です。 最初に、2 つのスクリプト ライブラリの参照を追加します。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

最初のライブラリは、core の ASP.NET AJAX クライアント側のスクリプト ライブラリを参照します。 このファイルは、約 24 k (圧縮) サイズでは、中核となるクライアント側の AJAX 機能が含まれています。 2 番目のライブラリには、ASP.NET MVC の組み込み AJAX ヘルパー メソッド (これはまもなく使用します) と統合するユーティリティ関数が含まれています。

更新プログラム ビュー テンプレート コードが表示されるように「が登録されていないこのイベントの」メッセージを出力時ではなく代わりにリンクするプッシュされるときに追加しましたが、RSVP コント ローラーで、RSVPForEvent アクション メソッドを呼び出す AJAX 呼び出しを実行してからことができます。ユーザーを RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

上記 Ajax.ActionLink() ヘルパー メソッドは ASP.NET MVC に組み込まれてと同様 Html.ActionLink() ヘルパー メソッドになる標準的な移動を実行する代わりに、アクション メソッドへの AJAX 呼び出しをリンクをクリックするとします。 上記の"RSVP"コント ローラーの"Register"のアクション メソッドを呼び出すことはしています"id"パラメーターとして、DinnerID を渡します。 最後の AjaxOptions パラメーターを渡しているでは、アクション メソッドから返されるコンテンツを受け取り、HTML を更新することを示します&lt;div&gt; id が"rsvpmsg"、ページの要素。

およびようになりました、夕食をユーザーが閲覧する際に登録されていませんまだ、その RSVP へのリンクが表示されます。

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

RSVP コント ローラーで、登録アクション メソッドへの AJAX 呼び出しを行うします"RSVP このイベントの"リンクをクリックした場合と更新されたメッセージが表示されますが、完了時に次のような。

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

ネットワーク帯域幅およびトラフィックに関係するは、この AJAX 呼び出しを行うときに、非常に軽量です。 HTTP POST 小さなネットワーク要求が行われる、ユーザーは、「このイベントを予約」リンクをクリックすると、 */Dinners/Register/1* URL をネットワーク上で次のようになります。

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

この登録アクション メソッドからの応答は単に.

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

この軽量の呼び出しは、高速は低速ネットワーク上でも動作します。

### <a name="adding-a-jquery-animation"></a>JQuery アニメーションを追加します。

AJAX 機能を実装しましたは、そして高速に動作します。 場合によって発生してもため高速で、ユーザーに RSVP のリンクを新しいテキストに置き換えられること気付かないこと。 結果をもう少し明確なさせる更新メッセージに注目させるために単純なアニメーションを追加できます。

既定 ASP.NET MVC プロジェクト テンプレートにはには、jQuery – Microsoft ではサポートされても優れた (と非常に人気のある) のオープン ソース JavaScript ライブラリが含まれています。 jQuery では、さまざまな優れた HTML DOM の選択と効果ライブラリを含む機能を提供します。

JQuery を使用するには、スクリプト参照を最初に追加します。 サイト内のさまざまな場所で jQuery を使用するので、すべてのページが使用できるように、Site.master マスター ページ ファイル内のスクリプト参照を追加します。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*ヒント: VS 2008 sp1 (jQuery など)、JavaScript ファイルの高度な intellisense サポートを有効にする JavaScript intellisense の修正プログラムがインストールされていることを確認します。ダウンロードすることができます。 http://tinyurl.com/vs2008javascripthotfix*

多くの場合、JQuery を使用して記述されたコードは、グローバル「$ ()」を使用して JavaScript メソッドを CSS セレクターを使用して 1 つまたは複数の HTML 要素を取得します。 たとえば、 <em>$("#rsvpmsg")</em> rsvpmsg の id を持つ任意の HTML 要素を選択中に<em>$(".something")</em> 「何か」CSS ですべての要素を選択クラス名。 「すべてのチェックのラジオ ボタンを返す」などのより高度なクエリを記述することもできます。 セレクターのようなクエリを使用して: <em>$("入力 [@type= ラジオ] [@checked]")</em>します。

要素を選択すると、非表示にするように、アクションを実行するためにメソッドを呼び出すことができます: *$("#rsvpmsg").hide();*

ここで予約は、"rsvpmsg"を選択する"AnimateRSVPMessage"という名前の単純な JavaScript 関数を定義します&lt;div&gt;とそのテキスト コンテンツのサイズをアニメーション化します。 次のコード開始小さなテキストとし、原因 400 ミリ秒単位の期間の経過とともに増大します。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

いますし、ワイヤ アップできる、Ajax.ActionLink() ヘルパー メソッドにその名前を渡すことによって、AJAX 呼び出しが正常に完了した後に呼び出される場合は、この JavaScript 関数 (AjaxOptions"OnSuccess"を使用してイベントのプロパティ)。

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

ようになりました"RSVP このイベントの"リンクをクリックし、AJAX 呼び出しが正常に完了、内容のメッセージが送信されるときにアニメーション化する前後のサイズが大きく。

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

"OnSuccess"イベントを提供するだけでなくは、AjaxOptions オブジェクトは、(その他のプロパティと便利なオプションのさまざまな) と共に処理できる OnBegin、OnFailure、および OnComplete イベントを公開します。

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>クリーンアップ - RSVP の部分的なビューをリファクタリング

詳細ビュー テンプレートは、どの超過は少し大変になることを理解する少し長を取得する開始します。 コードの読みやすさを向上させるには、終了 RSVP ビュー コードの詳細ページのすべてをカプセル化する部分ビュー – RSVPStatus.ascx – を作成します。

この \Views\Dinners フォルダーを右クリックし、[追加]-で実行できます&gt;メニュー コマンドを表示します。 その厳密に型指定された ViewModel として Dinner オブジェクトの取得があります。 私たちことができますし、コピー/貼り付け RSVP コンテンツを Details.aspx ビューから。

処理が完了したら、別の部分ビュー – EditAndDeleteLinks.ascx -、編集、削除のリンクの表示コードをカプセル化するを作成しましょうも。 その厳密に型指定された ViewModel として Dinner オブジェクトとそこに、Details.aspx ビューから編集および削除ロジックのコピー/貼り付けを必要もあります。

当社の詳細は、テンプレートが表示し、下部にある 2 つの Html.RenderPartial() メソッドの呼び出しを含めるだけです。

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

これにより、コードの読み取りおよびメンテナンス クリーナー。

### <a name="next-step"></a>次の手順

これで、さらに詳しく AJAX を使用し、アプリケーションに対話型のマッピング サポートを追加方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](secure-applications-using-authentication-and-authorization.md)
> [次へ](use-ajax-to-implement-mapping-scenarios.md)
