---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: "未処理の例外 (VB) の処理 |Microsoft ドキュメント"
author: rick-anderson
description: "実稼働環境で web アプリケーションで、実行時エラーが発生したときは、重要なは、開発者に通知して、エラーを記録できるように、la a から診断することがあります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: c1653cb3a8c684fd3d4fc2a039a947cfb5d42400
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="processing-unhandled-exceptions-vb"></a>未処理の例外 (VB) の処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_vb.pdf)

> 実稼働環境で web アプリケーションで、実行時エラーが発生したときは、重要なは、開発者に通知して、エラーを記録できるように、後の時点で診断可能性があります。 このチュートリアルでは、ASP.NET のランタイム エラーを処理およびカスタム コードを実行するたびに、ASP.NET ランタイムまで、未処理の例外のバブルの 1 つの方法では検索の概要を示します。


## <a name="introduction"></a>はじめに

ASP.NET のランタイムを発生させるまでバブルが ASP.NET アプリケーションでハンドルされない例外が発生したときに、`Error`イベントと、適切なエラー ページが表示されます。 エラー ページの次の 3 つの種類があります。 ランタイム エラー黄色画面の死亡 (YSOD) です。例外の詳細 YSOD です。カスタム エラー ページ。 [前のチュートリアル](displaying-a-custom-error-page-vb.md)リモート ユーザーおよびユーザーへのアクセスをローカルでの例外の詳細 YSOD にカスタム エラー ページを使用するアプリケーションを構成しました。

サイトのルック アンド フィールに合ったわかりやすいカスタム エラー ページを使用してランタイム エラー YSOD、既定値に優先はカスタム エラー ページを表示する包括的なエラー処理ソリューションの一部にすぎません。 実稼働環境でアプリケーションでエラーが発生するときに、例外の原因を正確に把握してそれに対応できるように、開発者がエラーの通知ことが重要です。 さらに、エラーを調査したり、後で時間に診断できるようには、エラーの詳細を記録する必要があります。

このチュートリアルでは、ログインできるように、未処理の例外の詳細情報にアクセスする方法と、開発者に通知します。 次のいずれかのこの 2 つのチュートリアルでは、エラーのログ記録ライブラリを構成では、少し後に自動的に実行時エラーの開発者に通知し、その詳細をログを調査します。

> [!NOTE]
> このチュートリアルで確認情報は、いくつかの固有またはカスタマイズされた方法で未処理の例外を処理する必要がある場合に便利です。 のみ必要がある場合、例外を記録し、開発者に通知をエラー ログ記録ライブラリを使用する、移動する方法です。 次の 2 つのチュートリアルでは、このような 2 つのライブラリの概要を示します。


## <a name="executing-code-when-theerrorevent-is-raised"></a>ときにコードを実行する、`Error`イベントが発生しました。

イベントは、オブジェクトに何か興味深いものが発生したこと、シグナルを発生して、応答にコードを実行する別のオブジェクトのメカニズムを提供します。 ASP.NET の開発者は、イベントの観点から考えることに慣れています。 そのボタンのイベント ハンドラーを作成する場合は、ユーザーが特定のボタンをクリックしたときに、いくつかのコードを実行するには、`Click`イベント コードを記述してあるとします。 ASP.NET ランタイムを発生させることを考えるその[`Error`イベント](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx)エラーの詳細をログ記録のコードは、イベント ハンドラーに移動できる未処理の例外が発生すると、次にします。 イベント ハンドラーを作成するが、`Error`イベントしますか?

`Error`で多数のイベントのいずれかのイベントは、 [ `HttpApplication`クラス](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx)要求の有効期間中に HTTP パイプラインの特定の段階で発生します。 など、`HttpApplication`クラスの[`BeginRequest`イベント](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.beginrequest.aspx); すべての要求の開始時に発生したその[`AuthenticateRequest`イベント](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx)セキュリティ モジュールが、要求元を識別されたときに発生します。 これら`HttpApplication`イベントに、ページの開発の要求の有効期間中にさまざまな時点でのカスタム ロジックを実行するための手段が得られます。

イベント ハンドラー、`HttpApplication`イベントは、という名前の特殊なファイルに配置できる`Global.asax`です。 Web サイトでこのファイルを作成するには、名前のグローバル アプリケーション クラス テンプレートを使用して、web サイトのルートに新しい項目を追加`Global.asax`です。

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**図 1**: 追加`Global.asax`Web アプリケーション  
 ([フルサイズのイメージを表示するをクリックして](processing-unhandled-exceptions-vb/_static/image3.png))

コンテンツと構造の`Global.asax`Visual Studio で作成されたファイルが Web アプリケーション プロジェクト (WAP) または Web サイト プロジェクト (WSP) のどちらを使用しているかどうかに基づいて若干が異なります。 WAP で、`Global.asax`は 2 つの個別ファイル - として実装`Global.asax`と`Global.asax.vb`です。 `Global.asax`ファイルでは、何も含まれていますが、`@Application`を参照するディレクティブ、`.vb`ファイル以外で目的のハンドラーが定義されているイベントの場合は、`Global.asax.vb`ファイル。 WSPs、1 つのファイルのみが作成、`Global.asax`でイベント ハンドラーが定義されていると、`<script runat="server">`ブロックします。

`Global.asax` WAP で Visual Studio のグローバル アプリケーション クラス テンプレートで作成したファイルには、という名前のイベント ハンドラーが含まれています`Application_BeginRequest`、 `Application_AuthenticateRequest`、および`Application_Error`、用のイベント ハンドラーは、`HttpApplication`イベント`BeginRequest`、 。`AuthenticateRequest`、および`Error`、それぞれします。 という名前のイベント ハンドラーもあります`Application_Start`、 `Session_Start`、 `Application_End`、および`Session_End`、どの web アプリケーションの起動時に、新しいセッションを開始、ときに、アプリケーションの終了時に発生するイベント ハンドラーは、セッションが終了します。それぞれします。 `Global.asax` WSP で Visual Studio によって作成されたファイルのみを含み、 `Application_Error`、 `Application_Start`、 `Session_Start`、 `Application_End`、および`Session_End`イベント ハンドラー。

> [!NOTE]
> コピーする必要があります、ASP.NET アプリケーションを展開するときに、`Global.asax`運用環境へのファイルです。 `Global.asax.vb` WAP で作成したファイルはこのコードは、プロジェクトのアセンブリにコンパイルされるので、実稼働環境にコピーする必要はありません。


Visual Studio のグローバル アプリケーション クラス テンプレートで作成したイベント ハンドラーが完全ではありません。 イベント ハンドラーを追加するには任意の`HttpApplication`イベント、イベント ハンドラーの名前を付けて`Application_EventName`です。 次のコードを追加するなど、`Global.asax`のイベント ハンドラーを作成するファイル、 [ `AuthorizeRequest`イベント](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

同様に、不要なグローバル アプリケーション クラス テンプレートで作成されたイベント ハンドラーを削除することができます。 このチュートリアルでのみ必要がありますのイベント ハンドラー、`Error`イベント; から他のイベント ハンドラーを削除しても構いません、`Global.asax`ファイル。

> [!NOTE]
> *HTTP モジュール*イベントのハンドラーを定義することもできます`HttpApplication`イベント。 HTTP モジュールは、別のクラス ライブラリに直接 web アプリケーション プロジェクト内に配置したりできる分離クラス ファイルとして作成されます。 HTTP モジュールが作成するための柔軟性と再利用可能なモデルを提供してクラス ライブラリ分割すること、ため`HttpApplication`イベント ハンドラー。 一方、`Global.asax`ファイルは、特定、web アプリケーションが配置されている場所をするには、HTTP モジュールをアセンブリ、この時点で、web サイトへの HTTP モジュールの追加のアセンブリを削除するように単純にコンパイルすることができます、`Bin`フォルダーとの登録、内のモジュール`Web.config`です。 このチュートリアルを作成して、HTTP モジュールを使用するのには検索しませんが、次の 2 つのチュートリアルで使用される 2 つのエラーのログ記録ライブラリは、HTTP モジュールとして実装されます。 HTTP モジュールの利点の詳細についてを参照してください[を使用して HTTP モジュールは、プラグ可能な ASP.NET コンポーネントを作成するハンドラー](https://msdn.microsoft.com/en-us/library/aa479332.aspx)です。


## <a name="retrieving-information-about-the-unhandled-exception"></a>未処理の例外に関する情報を取得します。

この時点で、Global.asax ファイルである、`Application_Error`イベント ハンドラー。 このイベント ハンドラーを実行するときは、エラーの開発者に通知し、その詳細を記録する必要があります。 これらのタスクを実行するには、最初の発生した例外の詳細を確認する必要があります。 サーバー オブジェクトの[`GetLastError`メソッド](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.getlasterror.aspx)、未処理の例外の原因となったの詳細を取得、`Error`イベントが発生します。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError`型のオブジェクトが返されます`Exception`、これは、.NET Framework のすべての例外の基本型です。 ただし、上記のコードでは am オブジェクトをキャスト例外によって返される`GetLastError`に、`HttpException`オブジェクト。 場合、`Error`内でスローされた例外をラップし、ASP.NET のリソースの処理中に例外がスローされたために、イベントが発生している、`HttpException`です。 実際にエラー イベントの使用の原因となった例外を取得する、`InnerException`プロパティです。 場合、`Error`イベントが存在しないページの要求などの HTTP ベースの例外により発生した、`HttpException`がスローされますが、内部例外にありません。

次のコードでは、`GetLastErrormessage`を引き起こした例外に関する情報を取得、`Error`イベントを格納する、`HttpException`という名前の変数で`lastErrorWrapper`です。 保存の種類、メッセージ、および元の例外のスタック トレースを確認する場合は、次の 3 つの文字列変数で、`lastErrorWrapper`が実際に発生した例外、 `Error` (の場合は HTTP ベースの例外) イベント、または単なるかどうかは、要求の処理中にスローされた例外のラッパーです。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

この時点ではデータベース テーブルに、例外の詳細を記録するコードを記述する必要。 すべての情報があります。 各エラーの詳細については、要求されたページの URL と、現在ログオンしているユーザーの名前などの他の便利部分と共に - 種類、メッセージ、スタック トレース、およびよびな - の関心のある列を含むデータベース テーブルを作成できます。 `Application_Error`イベント ハンドラーは、データベースに接続し、テーブルにレコードを挿入します。 同様に、電子メールでエラーの開発者がアラートを生成するコードを追加できます。

次の 2 つのチュートリアルで確認エラー ログ記録ライブラリは、このエラーのログ記録と通知を自分でビルドする必要はありませんのでに既定では、このような機能を提供します。 ただし、ことを示すために、`Error`イベントが発生することと、`Application_Error`ログに記録するエラーの詳細と、開発者に通知エラーが発生したときに、開発者に通知するコードを追加してみましょう。 イベント ハンドラーを使用することができます。

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>ハンドルされない例外が発生したときに、開発者に通知します。

実稼働環境でハンドルされない例外が発生したときにすることが重要、エラーを評価し、実行する必要があるアクションを決定できるように、開発チームのアラートを生成します。 たとえばがある場合 double 型にする必要がありますし、データベースに接続するときにエラー、接続文字列を確認してまた、おそらく、サポート チケットを開く web ホスティング会社です。 プログラミング エラーのため、例外が発生した場合、追加のコードまたは検証ロジックは、今後、このようなエラーを防ぐために追加する必要があります。

.NET Framework クラス、 [ `System.Net.Mail`名前空間](https://msdn.microsoft.com/en-us/library/system.net.mail.aspx)電子メールを送信するが簡単です。 [ `MailMessage`クラス](https://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx)電子メール メッセージを表しなどのプロパティを持つ`To`、 `From`、 `Subject`、 `Body`、および`Attachments`です。 `SmtpClass`送信に使用されて、`MailMessage`オブジェクト指定された SMTP サーバーを使用して; SMTP サーバーの設定をプログラムでも宣言的に指定することができます、 [ `<system.net>`要素](https://msdn.microsoft.com/en-us/library/6484zdc1.aspx)で、`Web.config file`です。 電子メールの送信の詳細については、ASP.NET アプリケーション内のメッセージをチェック アウト私の記事[ASP.NET で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)、および[System.Net.Mail FAQ](http://systemnetmail.com/)です。

> [!NOTE]
> `<system.net>`要素にはで使用する SMTP サーバー設定が含まれています、`SmtpClient`クラスの電子メールを送信するときにします。 Web ホスティング可能性の高い会社には、アプリケーションから電子メールを送信に使用できる SMTP サーバーがあります。 Web アプリケーションで使用する必要があります、SMTP サーバー設定については、web ホストのサポート セクションを参照してください。


次のコードを追加、`Application_Error`エラーが発生したときに、開発者に電子メールを送信するイベントのハンドラー。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

上記のコードは非常に長いですが、これの大部分を開発者に送信する電子メールに表示される HTML を作成します。 コードが参照することで始まる、`HttpException`によって返される、`GetLastError`メソッド (`lastErrorWrapper`)。 実際に要求によって発生した例外はを介して取得された`lastErrorWrapper.InnerException`変数に割り当てられている`lastError`です。 型、メッセージ、およびスタックからトレース情報を取得`lastError`3 つの文字列変数に格納されているとします。

次に、`MailMessage`という名前のオブジェクト`mm`を作成します。 電子メールの本文は HTML 形式であり、要求されたページの URL、現在ログオンしているユーザー、および例外 (種類、メッセージ、およびスタック トレース) に関する情報の名前を表示します。 優れた点の 1 つ、`HttpException`クラスは、呼び出すことによって、例外の詳細黄色画面の死亡 (YSOD) を作成するために使用する HTML を生成することができます、 [GetHtmlErrorMessage メソッド](https://msdn.microsoft.com/en-us/library/system.web.httpexception.gethtmlerrormessage.aspx)です。 このメソッドは、例外の詳細 YSOD マークアップを取得し、電子メールの添付ファイルとしてに追加するここで使用されます。 注意: かどうか、例外をトリガーする、`Error`イベントが存在しないページの要求) などの HTTP ベースの例外、`GetHtmlErrorMessage`メソッドが返す`null`です。

最後に、送信、`MailMessage`です。 これで新たに作成する`SmtpClient`メソッドを呼び出してその`Send`メソッドです。

> [!NOTE]
> Web アプリケーションでこのコードを使用する前にしておくの値を変更、`ToAddress`と`FromAddress`定数のsupport@example.comや電子メール アドレスをエラー通知電子メールを送信する必要があり、由来します。 SMTP サーバー設定を指定する必要も、 `<system.net>` 」の「`Web.config`です。 使用する SMTP サーバー設定を確認する、web ホスト プロバイダーを参照してください。


配置でこのコード エラーが発生、開発者は送信という電子メール メッセージをエラーの概要し、YSOD が含まれています。 前のチュートリアルで Genre.aspx を訪問し、無効な渡すことによって、実行時エラーを示すお`ID`と同様に、クエリ文字列を通じて値`Genre.aspx?ID=foo`です。 使用してページへのアクセス、 `Global.asax` - 前のチュートリアルで開発環境で引き続き実稼働環境で次の手順中に、例外の詳細黄色画面の死亡事故を参照するくださいと、ファイルの場所には、同じユーザー エクスペリエンスを作成します。カスタム エラー ページを参照してください。 この既存の動作だけでなく、開発者に電子メールが送信されます。

**図 2**オフィスを訪れたときに受信した電子メールを示しています。`Genre.aspx?ID=foo`です。 電子メールの本文には、例外情報をまとめたものです中に、`YSOD.htm`添付ファイルには、例外の詳細 YSOD に表示されているコンテンツが表示されます (を参照してください**図 3**)。

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**図 2**: ハンドルされない例外が発生するたびに、開発者の電子メール通知が送信します。  
 ([フルサイズのイメージを表示するをクリックして](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**図 3**: 電子メール通知には、添付ファイルとして YSOD 例外の詳細が含まれています。  
 ([フルサイズのイメージを表示するをクリックして](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>使用する場合は、カスタム エラー ページですか。

このチュートリアルで使用する方法を示しました`Global.asax`と`Application_Error`ハンドルされない例外が発生したときにコードを実行するイベント ハンドラー。 具体的には、使用して、このイベント ハンドラーにするとエラーの開発者に通知また、データベース内のエラーの詳細をログに記録する拡張できます。 存在、`Application_Error`イベント ハンドラーでは、エンドユーザーのエクスペリエンスには影響しません。 構成済みのエラー ページで、これは、エラーの詳細 YSOD、ランタイム エラー YSOD またはカスタム エラー ページが表示されます。

疑問を抱くと自然なであるかどうか、`Global.asax`ファイルと`Application_Error`イベントは、カスタム エラー ページを使用する場合に必要です。 エラーが発生したカスタム エラー ページを表示ため理由できないコードを作成しました、開発者に通知し、カスタム エラー ページの分離コード クラスに、エラーの詳細をログインしますか? カスタム エラー ページの分離コード クラスにコードを追加できますが、確実に発生する例外の詳細へのアクセスにはできません、`Error`前のチュートリアルでは探索方法を使用するときにイベント。 呼び出す、`GetLastError`カスタム エラー ページからメソッドを返します`Nothing`です。

この動作の理由のリダイレクトを使用して、カスタム エラー ページに達したためにです。 未処理の例外に達した場合、ASP.NET ランタイム エンジンは、ASP.NET を発生させるその`Error`イベント (が実行される、`Application_Error`イベント ハンドラー) し、*リダイレクト*を発行して、カスタムエラーページにユーザー`Response.Redirect(customErrorPageUrl)`. `Response.Redirect`メソッドは、新しい URL、つまりカスタム エラー ページを要求するブラウザーに指示を HTTP 302 ステータス コードでは、クライアントへの応答を送信します。 ブラウザーに自動的に要求この新しいページします。 わかりますカスタム エラー ページがページから個別に要求されたブラウザーのアドレス バーは、カスタム エラー ページの URL に変更されるため、エラーが発生した (を参照してください**図 4**)。

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**図 4**: ブラウザーは、カスタム エラー ページの URL にリダイレクト取得エラーが発生  
 ([フルサイズのイメージを表示するをクリックして](processing-unhandled-exceptions-vb/_static/image12.png))

実質的な影響は、サーバーが HTTP 302 リダイレクトに応答するときに未処理の例外が発生した要求を終了します。 カスタム エラー ページに後続の要求は、新しい要求です。この段階で、ASP.NET エンジン エラー情報が破棄し、さらにのカスタム エラー ページの新しい要求に、前回の要求でハンドルされない例外を関連付けることはできません。 その理由は`GetLastError`返します`null`カスタム エラー ページから呼び出されるとします。

ただし、エラーが発生した同じ要求時に実行されるカスタム エラー ページを設定することができます。 [ `Server.Transfer(url)` ](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.transfer.aspx)メソッドが実行を指定した URL に転送し、同じ要求内で処理します。 内のコードに移動する可能性があります、`Application_Error`イベント ハンドラーに置き換えることで、カスタム エラー ページの分離コード クラス`Global.asax`を次のコード。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

これでハンドルされない例外が発生すると、`Application_Error`イベント ハンドラーは、HTTP ステータス コードに基づいて、適切なカスタム エラー ページに制御を移します。 カスタム エラー ページが経由で未処理の例外情報へのアクセスを持つコントロールは、転送されたため`Server.GetLastError`とがエラーの開発者に通知し、その詳細を記録します。 `Server.Transfer`呼び出しには、カスタム エラー ページにユーザーをリダイレクトすることから ASP.NET エンジンが停止しました。 代わりに、カスタム エラー ページのコンテンツは、エラーが発生したページへの応答として返されます。

## <a name="summary"></a>概要

ASP.NET web アプリケーションでハンドルされない例外が発生したときに、ASP.NET ランタイムを発生させます、`Error`イベントと、構成済みのエラー ページが表示されます。 お開発者にエラーの通知の詳細をログに記録したりエラー イベントのイベント ハンドラーを作成することで、さまざまな方法で処理できます。 イベント ハンドラーを作成する 2 つの方法がある`HttpApplication`イベントと同様に`Error`: で、`Global.asax`ファイルまたは HTTP モジュールからです。 このチュートリアルで作成する方法を示しました、`Error`内のイベント ハンドラー、`Global.asax`ファイルを電子メール メッセージを使用して、エラーの開発者に通知します。

作成する、`Error`イベント ハンドラーがいくつかの固有またはカスタマイズされた方法で未処理の例外を処理する必要がある場合に便利です。 ただし、独自の作成`Error`または開発者に通知する例外をログにイベント ハンドラーは、時間の最も効率的な使用数分で設定できる無料で使いやすいもののエラー ログのライブラリが存在します。 次の 2 つのチュートリアルでは、このような 2 つのライブラリを確認します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET HTTP モジュールは、HTTP ハンドラーの概要](https://support.microsoft.com/kb/307985)
- [未処理の例外の処理 - 未処理の例外に応答して適切に](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`クラスと ASP.NET アプリケーション オブジェクト](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP ハンドラーと ASP.NET の HTTP モジュール](http://www.15seconds.com/Issue/020417.htm)
- [ASP.NET で電子メールを送信します。](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [について、`Global.asax`ファイル](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [HTTP モジュールとハンドラーを使用して、プラグ可能な ASP.NET コンポーネントを作成するには](https://msdn.microsoft.com/en-us/library/aa479332.aspx)
- [ASP.NET を扱う`Global.asax`ファイル](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [扱う`HttpApplication`インスタンス](https://msdn.microsoft.com/en-us/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[前へ](displaying-a-custom-error-page-vb.md)
[次へ](logging-error-details-with-asp-net-health-monitoring-vb.md)
