---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: AJAX を使用して、動的な更新プログラムを配信 |Microsoft ドキュメント
author: microsoft
description: RSVP のログイン ユーザーの該当する dinner 詳細内に組み込まれて Ajax ベースのアプローチを使用して、夕食への参加をサポートして手順 10. を実装しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870181"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>AJAX を使用して、動的更新を配信するには
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 10. ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> RSVP のログイン ユーザーの dinner の詳細 ページ内に組み込まれて Ajax ベースのアプローチを使用して、夕食への参加の目的をサポートして手順 10. を実装します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner 手順 10: RSVPs を有効にする AJAX を受け入れます

RSVP、夕食への参加の目的にログインしているユーザー向けのサポートを実装してみましょう。 有効にしますこの dinner の詳細 ページ内に組み込まれて AJAX ベースのアプローチを使用します。

### <a name="indicating-whether-the-user-is-rsvpd"></a>ユーザーが RSVP'd かどうかを示す

ユーザーがアクセスできる、 */Dinners/詳細/[id*] 特定 dinner に関する詳細を表示する URL。

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

アクション メソッドが実装されている Details() 次のようにします。

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

RSVP のサポートを実装する、最初の手順は、Dinner オブジェクト (前に作成した Dinner.cs 部分クラス) 内に"IsUserRegistered(username)"ヘルパー メソッドを追加するされます。 このヘルパー メソッドには、true またはユーザーが現在 Dinner の RSVP'd かどうかに応じて false が返されます。

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Details.aspx ビュー テンプレートで、ユーザーが登録されているかどうかを示す、適切なメッセージを表示するにまたはイベントではなく、次のコードを追加できます。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

今すぐに登録されている夕食にアクセスするユーザーが表示されますこのメッセージ。

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

それらが登録されていませんが表示されます Dinner を訪問するときに、メッセージの下。

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>登録アクション メソッドの実装

今すぐ RSVP な夕食の詳細 ページからユーザーを有効にする必要な機能を追加してみましょう。

これを実装する、\Controllers ディレクトリを右クリックして、追加の を選択して、新しい"RSVPController"クラスを作成しますお&gt;コント ローラーのメニュー コマンド。

引数として Dinner の id を受け取り、適切な夕食オブジェクト、ログイン ユーザーが、登録したユーザーの一覧で、現在あり場合を確認を取得する新しい RSVPController クラス内で"Register"アクション メソッドを実装しますそれらの RSVP オブジェクトを追加できません。

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

単純な文字列を返すアクション メソッドの出力としてお方法の上に注意してください。 おでしたが埋め込まれているビュー テンプレート – 内でこのメッセージが小さいためであるために使用 Content() ヘルパー メソッドのコント ローラーの基本クラスと上と同様に、文字列メッセージの戻り値で。

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>AJAX を使用して RSVPForEvent アクション メソッドを呼び出す

この詳細ビューから登録アクション メソッドを呼び出すに AJAX が使用されます。 この実装は非常に簡単です。 まず 2 つのスクリプト ライブラリの参照を追加します。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

最初のライブラリは、コア ASP.NET AJAX クライアント側スクリプト ライブラリを参照します。 このファイル (圧縮) サイズの約 24 k は、中核となるクライアント側の AJAX 機能が含まれています。 2 番目のライブラリには、ASP.NET MVC の組み込み AJAX ヘルパー メソッド (間もなくが使用されます) と統合されるユーティリティ関数が含まれています。

更新プログラム テンプレート コードの表示「が登録されていないこのイベントの」メッセージ、した出力ではなく代わりにレンダリングのリンクをプッシュされたときになるように追加しましたが、RSVP コント ローラーで、RSVPForEvent アクション メソッドを呼び出す AJAX 呼び出しを実行し、ごことができます。ユーザーを RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

上記 Ajax.ActionLink() ヘルパー メソッドは ASP.NET MVC に組み込まれ Html.ActionLink() ヘルパー メソッドと似ていますなる標準的なナビゲーションを実行する代わりに、アクション メソッドへの AJAX 呼び出しのリンクがクリックされたときにします。 上記の"RSVP"コント ローラーの「登録」アクション メソッドを呼び出すと、"id"パラメーターとして、DinnerID を渡すことをおはします。 最後の AjaxOptions パラメーターを渡しているは、アクション メソッドから返されるコンテンツを行い、HTML を更新することを示します&lt;div&gt;の id を持つ"rsvpmsg"ページの要素。

およびようになりました、夕食にユーザーが参照するときに登録されていませんまだ、その RSVP へのリンクが表示されます。

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

RSVP コント ローラーで、登録アクション メソッドへの AJAX 呼び出しを行うします"RSVP このイベントの"リンクをクリックした場合と、完了時に更新されたメッセージが表示されます以下のような。

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

ネットワーク帯域幅とこの AJAX 呼び出しを行うときに関係するトラフィックは非常に軽量です。 小規模の HTTP POST ネットワーク要求が行われる、ユーザーは、"このイベントの RSVP"リンクをクリックすると、 */Dinners/Register/1* URL をネットワーク上で次のようになります。

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

単に、登録アクション メソッドからの応答。

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

この軽量コールは高速であり、でも、低速ネットワーク経由で動作します。

### <a name="adding-a-jquery-animation"></a>JQuery アニメーションを追加します。

実装しました AJAX 機能は、正しいで高速に動作します。 場合があります、起こります速すぎてただし、ことユーザーつかない可能性があります RSVP のリンクが新しいテキストに置き換えられています。 結果を少しより明確に更新メッセージに注目させる単純なアニメーションを追加できます。

既定 ASP.NET MVC プロジェクト テンプレートにはには、jQuery – Microsoft によってサポートされても優れた (と非常によく使用される) のオープン ソースの JavaScript ライブラリが含まれています。 jQuery では、いくつかの便利な HTML DOM の選択と効果ライブラリを含む機能を提供します。

JQuery を使用するには、へのスクリプト参照を最初に追加されます。 スクリプト参照のすべてのページが使用できるように、サイト内でさまざまな場所内での jQuery を使用して行う、ため、Site.master マスター ページ ファイル内を追加します。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*ヒント: VS 2008 sp1 を JavaScript ファイル (jQuery を含む) の豊富な intellisense サポートを有効にする JavaScript intellisense の修正プログラムがインストールされていることを確認します。ダウンロードすることができます。 http://tinyurl.com/vs2008javascripthotfix*

多くの場合、JQuery を使用して記述されたコードは、グローバル「$ ()」を使用して、CSS セレクターを使用して 1 つまたは複数の HTML 要素を取得する JavaScript メソッドです。 たとえば、 <em>$("#rsvpmsg")</em> rsvpmsg の id を持つ任意の HTML 要素の選択中に<em>$(".something")</em> 「何か」CSS ですべての要素を選択クラス名。 「すべてのチェックされているオプション ボタンを返す」などのより高度なクエリを記述することもできます。 のような選択クエリを使用して: <em>$("の入力 [@typeラジオを =] [@checked]")</em>です。

要素を選択したら、それらを非表示にするように、アクションを実行するためにメソッドを呼び出すことができます: *$("#rsvpmsg").hide() です。*

RSVP シナリオでは、"rsvpmsg"を選択する"AnimateRSVPMessage"をという名前の単純な JavaScript 関数を定義してあります&lt;div&gt;とテキスト コンテンツのサイズをアニメーション化します。 コードの下、小さなテキストと開始し、原因、400 ミリ秒の期間の経過と共に増加します。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

おできますし、ワイヤ アップ、Ajax.ActionLink() ヘルパー メソッドにその名前を渡すことによって、AJAX 呼び出しが正常に完了した後に呼び出される JavaScript 関数 (AjaxOptions"OnSuccess"を使用してイベント プロパティ)。

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

これで、および"RSVP このイベントの"リンクをクリックし、AJAX 呼び出しが正常に完了、内容のメッセージが送信されるときにバックアップがアニメーション化サイズが大きくなります。

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

「成功」イベントを提供するだけでなくは、AjaxOptions オブジェクトは、(と共に、さまざまな他のプロパティと便利なオプション) を処理できる OnBegin、OnFailure、および OnComplete イベントを表示します。

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>RSVP の部分ビューをリファクタリングのクリーンアップ

この詳細ビュー テンプレートはどの超過が難しく、少し理解する少し長くなるを開始します。 コードの読みやすさを向上させるには、終了 RSVP コードの表示、詳細ページ用のすべてをカプセル化する部分ビュー – RSVPStatus.ascx – を作成します。

\Views\Dinners フォルダーを右クリックし、追加 - この作業を行うことができます&gt;メニュー コマンドを表示します。 必要がある、厳密に型指定された ViewModel として Dinner オブジェクトを取得します。 おできますし、コピー/貼り付け RSVP コンテンツに、Details.aspx ビューからです。

処理が完了したら、みましょうもビューを作成別部分 – EditAndDeleteLinks.ascx - 編集および削除リンク表示コードをカプセル化します。 その厳密に型指定された ViewModel として Dinner オブジェクトを取得し、コピー/貼り付けを Details.aspx ビューから編集および削除ロジックは、それを必要もあります。

詳細は表示テンプレートはし、下部にある 2 つの Html.RenderPartial() メソッドの呼び出しを含めるだけ。

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

これにより、コードの読み取りおよびメンテナンス クリーナーです。

### <a name="next-step"></a>次の手順

これで、AJAX を使用して、さらにし、対話型のマッピングのサポートをアプリケーションに追加して方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](secure-applications-using-authentication-and-authorization.md)
> [次へ](use-ajax-to-implement-mapping-scenarios.md)
