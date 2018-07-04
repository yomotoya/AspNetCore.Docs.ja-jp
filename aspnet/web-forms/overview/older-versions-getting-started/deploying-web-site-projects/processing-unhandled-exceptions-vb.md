---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: 未処理の例外 (VB) の処理 |Microsoft Docs
author: rick-anderson
description: 実稼働環境で web アプリケーションでランタイム エラーが発生したとき、重要なは、開発者に通知して、la で診断ができるように、エラーをログには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c0f6883f2a19d060aff1573238cc1df1838e824
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365985"
---
<a name="processing-unhandled-exceptions-vb"></a>ハンドルされない例外 (VB) を処理します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples)します ([ダウンロード方法](/aspnet/core/tutorials/index#how-to-download-a-sample))。

> 運用環境で web アプリケーションでランタイム エラーが発生したときは、重要なは、開発者に通知して、エラーを記録して、時間で、後で診断することがあります。 このチュートリアルでは、ASP.NET のランタイム エラーを処理およびカスタム コードが実行されるたびに、ASP.NET の実行時までの未処理の例外バブルの 1 つの方法で検索の概要を示します。


## <a name="introduction"></a>はじめに

発生させます ASP.NET ランタイム バブルアップ、ASP.NET アプリケーションでハンドルされない例外が発生したときに、`Error`イベントと、該当するエラー ページが表示されます。 エラー ページの 3 つの種類があります。 ランタイム エラー黄色い画面の死 (YSOD)。例外の詳細 YSOD;カスタム エラー ページ。 [前のチュートリアル](displaying-a-custom-error-page-vb.md)リモート ユーザーにローカルにアクセスするユーザーの例外の詳細 YSOD カスタム エラー ページを使用するアプリケーションを構成しました。

サイトのルック アンド フィールに一致する人が読みやすいカスタム エラー ページを使用してランタイム エラー YSOD、既定値に優先がカスタム エラー ページを表示する包括的なエラー処理ソリューションの一部にすぎません。 運用環境でアプリケーションでエラーが発生することが、開発者は、例外の原因を見つけることとそれに対応できるように、エラーの通知が重要です。 さらに、エラーを確認し、時間で、後で診断できるように、エラーの詳細を記録する必要があります。

このチュートリアルでは、未処理の例外の詳細へのアクセスがログに記録できるようにする方法と、開発者に通知します。 次のいずれかのこの 2 つのチュートリアルでは、エラー ログ記録ライブラリを少しの構成の後に自動的に実行時エラーの開発者に通知し、その詳細をログ記録について説明します。

> [!NOTE]
> このチュートリアルで調査情報は、いくつか一意またはカスタマイズされた方法で未処理の例外を処理する必要がある場合に便利です。 だけする必要がある例外をログに、開発者に通知する場合、移動する方法は、エラーのログ記録ライブラリを使用します。 次の 2 つのチュートリアルでは、このような 2 つのライブラリの概要を示します。


## <a name="executing-code-when-theerrorevent-is-raised"></a>ときにコードを実行する、`Error`イベントが発生します

イベントは、オブジェクトに興味深いが発生したこと、シグナルを発生して、応答コードを実行する別のオブジェクトのメカニズムを提供します。 ASP.NET 開発者向けイベントの観点から考えることに慣れています。 そのボタンのイベント ハンドラーを作成する場合は、ユーザーが特定のボタンをクリックしたときにいくつかのコードを実行するには、`Click`イベントと、コードをそこに配置します。 ASP.NET ランタイムを発生させますが、 [ `Error`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)イベント ハンドラーでエラーの詳細をログ記録のコードが仮にに述べたようにハンドルされない例外が発生するたびにします。 イベント ハンドラーを作成する方法が、`Error`イベントでしょうか。

`Error`イベントがイベントの多くの 1 つ、 [ `HttpApplication`クラス](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)は、要求の有効期間中に特定の HTTP パイプライン ステージで発生します。 たとえば、`HttpApplication`クラスの[`BeginRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)がすべての要求の開始時に発生しますその[`AuthenticateRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)セキュリティ モジュールには、要求元が識別するときに発生します。 これら`HttpApplication`イベント提供ページの開発者の要求の有効期間中にさまざまな時点でのカスタム ロジックを実行することを意味します。

イベント ハンドラー、`HttpApplication`という名前の特殊なファイルのイベントを配置できる`Global.asax`します。 このファイルを web サイトを作成するには、名前のグローバル アプリケーション クラス テンプレートを使用して、web サイトのルートに新しい項目を追加`Global.asax`します。

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**図 1**: 追加`Global.asax`Web アプリケーション  
 ([フルサイズの画像を表示する をクリックします](processing-unhandled-exceptions-vb/_static/image3.png))。

コンテンツとの構造、 `Global.asax` Visual Studio によって作成されたファイルが Web アプリケーション プロジェクト (WAP) または Web サイト プロジェクト (WSP) のどちらを使用しているかどうかに基づいて若干異なります。 WAP と、`Global.asax`は 2 つの個別ファイルとして実装`Global.asax`と`Global.asax.vb`します。 `Global.asax`ファイルでは、何も含まれていますが、`@Application`を参照するディレクティブ、`.vb`ファイル; で目的のハンドラーが定義されているイベント、`Global.asax.vb`ファイル。 Wsp、1 つのファイルのみを作成、`Global.asax`でイベント ハンドラーが定義されていると、`<script runat="server">`ブロックします。

`Global.asax` WAP で Visual Studio のグローバル アプリケーション クラス テンプレートで作成したファイルには、という名前のイベント ハンドラーが含まれています`Application_BeginRequest`、 `Application_AuthenticateRequest`、および`Application_Error`、用のイベント ハンドラーは、`HttpApplication`イベント`BeginRequest`、 。`AuthenticateRequest`、および`Error`、それぞれします。 という名前のイベント ハンドラーもあります`Application_Start`、 `Session_Start`、 `Application_End`、および`Session_End`、どの web アプリケーションの起動時に、新しいセッションを開始、ときに、アプリケーションの終了時に発生するイベント ハンドラーは、セッションが終了します。それぞれします。 `Global.asax` WSP で Visual Studio によって作成されたファイルのみを含み、 `Application_Error`、 `Application_Start`、 `Session_Start`、 `Application_End`、および`Session_End`イベント ハンドラー。

> [!NOTE]
> ASP.NET アプリケーションをデプロイするときにコピーする必要があります、`Global.asax`運用環境へのファイル。 `Global.asax.vb` WAP で作成されると、ファイルはこのコードは、プロジェクトのアセンブリにコンパイルされるので、運用環境にコピーする必要はありません。


Visual Studio のグローバル アプリケーション クラス テンプレートで作成されたイベント ハンドラーが完全ではありません。 いずれかのイベント ハンドラーを追加する`HttpApplication`イベント、イベント ハンドラーの名前を付けて`Application_EventName`します。 次のコードを追加するなど、`Global.asax`のイベント ハンドラーを作成するファイル、 [ `AuthorizeRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

同様に、不要なグローバル アプリケーション クラス テンプレートで作成されたイベント ハンドラーを削除することができます。 このチュートリアルでのみが必要なイベント ハンドラーを`Error`イベント; から他のイベント ハンドラーを削除する自由、`Global.asax`ファイル。

> [!NOTE]
> *HTTP モジュール*のイベント ハンドラーを定義する別の方法を提供して`HttpApplication`イベント。 HTTP モジュールは、別のクラス ライブラリに直接 web アプリケーション プロジェクト内に配置または分離が可能なクラス ファイルとして作成されます。 クラス ライブラリ分割すること、ために、HTTP モジュールは作成するための柔軟で再利用可能なモデルを提供`HttpApplication`イベント ハンドラー。 一方、`Global.asax`ファイルは特定が存在する場所、web アプリケーションに HTTP モジュールをこの時点ではのアセンブリを削除するだけでは web サイトへの HTTP モジュールの追加のアセンブリにコンパイルできます、`Bin`フォルダーと登録、モジュールで`Web.config`します。 このチュートリアルは、作成すると、HTTP モジュールを使用して検索しませんが、次の 2 つのチュートリアルで使用される 2 つのエラーのログ記録ライブラリは、HTTP モジュールとして実装されます。 HTTP モジュールの利点の詳細についてを参照してください[を使用して HTTP モジュールは、プラグ可能な ASP.NET コンポーネントを作成するためのハンドラー](https://msdn.microsoft.com/library/aa479332.aspx)します。


## <a name="retrieving-information-about-the-unhandled-exception"></a>ハンドルされない例外に関する情報を取得します。

この時点での Global.asax ファイルがある、`Application_Error`イベント ハンドラー。 このイベント ハンドラーを実行するときは、開発者は、エラーを通知し、その詳細をログ記録する必要があります。 これらのタスクを実行するには、最初の発生した例外の詳細を確認する必要があります。 サーバー オブジェクトの[`GetLastError`メソッド](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx)原因となったハンドルされない例外の詳細を取得、`Error`イベントが発生します。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError`メソッド型のオブジェクトを返します`Exception`、.NET Framework のすべての例外の基本型です。 ただし、上記のコードではいるオブジェクトをキャスト例外によって返される`GetLastError`に、`HttpException`オブジェクト。 場合、`Error`イベントが発生している内でスローされた例外をラップし、ASP.NET リソースの処理中に例外がスローされたため、`HttpException`します。 エラー イベントの使用の原因となった実際の例外を取得する、`InnerException`プロパティ。 場合、`Error`イベントが存在しないページでは、要求などの HTTP ベースの例外のために発生した、`HttpException`がスローされますが、内部例外はありません。

次のコードでは、`GetLastErrormessage`を発生させた例外に関する情報を取得、`Error`イベントを格納する、`HttpException`という名前の変数で`lastErrorWrapper`します。 種類、メッセージ、および元の例外のスタック トレースをかどうかをチェック、3 つの文字列変数に格納、`lastErrorWrapper`は実際の例外を発生させた、 `Error` (の場合は HTTP ベースの例外) イベント、または単なるかどうかは、要求の処理中にスローされた例外のラッパーです。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

この時点で、データベース テーブルに、例外の詳細を記録するコードを記述する必要があるすべての情報があります。 各エラーの詳細については、要求されたページの URL と、現在ログオンしているユーザーの名前などの他の便利な情報と共に -、種類、メッセージ、スタック トレース、および具合 - の関心のある列を含むデータベース テーブルを作成できます。 `Application_Error`イベント ハンドラーはデータベースに接続し、テーブルにレコードを挿入します。 同様に、開発者は電子メールでエラーのアラートを生成するコードを追加する可能性があります。

自分で、このエラーのログ記録と通知を作成する必要はありませんので、次の 2 つのチュートリアルでは、検証エラーのログ記録ライブラリは既定では、このような機能を提供します。 ただし、説明するため、`Error`イベントが発生していると、`Application_Error`エラーの詳細をログ記録、開発者に通知し、エラーが発生したときに、開発者に通知するコードを追加してみましょうイベント ハンドラーを使用できます。

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>ハンドルされない例外が発生したときに、開発者に通知

運用環境でハンドルされない例外が発生した場合は、開発チームに通知が、エラーを評価し、実行する必要があるアクションを決定できるように必要があります。 などがある場合エラー double 型にする必要がありますし、データベースへの接続で接続文字列を確認し、おそらく、web ホスティング会社でサポート チケットを開きます。 プログラミング エラーのため、例外が発生した場合は、コードを追加または検証ロジックが、今後、このようなエラーを防ぐために追加する必要があります。

.NET Framework クラス、 [ `System.Net.Mail`名前空間](https://msdn.microsoft.com/library/system.net.mail.aspx)電子メールを送信するが簡単です。 [ `MailMessage`クラス](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)などのプロパティであり、電子メール メッセージを表します`To`、 `From`、 `Subject`、 `Body`、および`Attachments`します。 `SmtpClass`送信に使用される、`MailMessage`オブジェクトを指定した SMTP サーバーを使用して、プログラムから、または宣言的で SMTP サーバーの設定を指定することができます、 [ `<system.net>`要素](https://msdn.microsoft.com/library/6484zdc1.aspx)で、`Web.config file`します。 電子メールの送信の詳細については、ASP.NET アプリケーション内のメッセージをチェック アウト筆者の記事[ASP.NET で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)、および[System.Net.Mail FAQ](http://systemnetmail.com/)します。

> [!NOTE]
> `<system.net>`要素にはで使用される SMTP サーバーの設定が含まれています、`SmtpClient`クラス、電子メールを送信するときにします。 Web ホスティング会社可能性がありますが、アプリケーションからの電子メールの送信に使用できる SMTP サーバー。 Web アプリケーションで使用する必要があります、SMTP サーバー設定については、web ホストのサポート セクションを参照してください。


次のコードを追加、`Application_Error`エラーが発生したときに、開発者に電子メールを送信するイベント ハンドラー。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

上記のコードは非常に長いですが、その一括は開発者に送信されるメールで表示される HTML を作成します。 コードが参照することで始まる、`HttpException`によって返される、`GetLastError`メソッド (`lastErrorWrapper`)。 使用して、要求によって発生した実際の例外が取得された`lastErrorWrapper.InnerException`変数に割り当てられていると`lastError`します。 型、メッセージ、およびスタックからトレース情報を取得`lastError`3 つの文字列変数に格納されているとします。

次に、`MailMessage`という名前のオブジェクト`mm`が作成されます。 電子メールの本文は HTML 形式であり、要求されたページの URL、現在ログオンしているユーザー、および (種類、メッセージ、およびスタック トレース) 例外に関する情報の名前を表示します。 優れた点の 1 つ、`HttpException`クラスは、呼び出すことによって、例外の詳細黄色い画面の死 (YSOD) を作成するために使用する HTML を生成することができます、 [GetHtmlErrorMessage メソッド](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx)します。 このメソッドは、例外の詳細 YSOD マークアップを取得し、電子メールの添付ファイルとしてに追加するには、ここに使用されます。 注意: かどうか、例外をトリガーする、`Error`イベントが存在しないページに対する要求) などの HTTP ベースの例外、`GetHtmlErrorMessage`メソッドが返す`null`します。

最後に、送信、`MailMessage`します。 これは、新しいを作成することで`SmtpClient`メソッドを呼び出してその`Send`メソッド。

> [!NOTE]
> 値を変更しますが、web アプリケーションでこのコードを使用する前に、`ToAddress`と`FromAddress`定数support@example.comにどのような電子メール エラー通知の電子メール アドレスに送信する必要があり、由来します。 SMTP サーバー設定を指定する必要も、`<system.net>`セクション`Web.config`します。 使用する SMTP サーバーの設定を決定する、web ホスト プロバイダーを参照してください。


コード エラーがあるときにいつでも、開発者は送信電子メール メッセージ、エラーの概要し、YSOD が含まれていますが、されます。 前のチュートリアルで Genre.aspx へのアクセスで渡して、無効な実行時エラーを示す`ID`など、クエリ文字列を通じて値`Genre.aspx?ID=foo`します。 ページにアクセスして、`Global.asax`インプレースでファイルが生成されます、同じユーザー エクスペリエンスの前のチュートリアルで開発環境で引き続きものを運用環境で学習中に、例外の詳細黄色い画面の死亡事故を参照してくださいカスタム エラー ページを参照してください。 この既存の動作に加え、開発者にメールが送信されます。

**図 2**にアクセスしたときに受信した電子メールを示しています。`Genre.aspx?ID=foo`します。 電子メールの本文は、例外情報をまとめたものです。 中に、`YSOD.htm`添付ファイルには、例外の詳細 YSOD に表示されているコンテンツが表示されます (を参照してください**図 3**)。

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**図 2**: ハンドルされない例外があるたびに、開発者の電子メール通知が送信します。  
 ([フルサイズの画像を表示する をクリックします](processing-unhandled-exceptions-vb/_static/image6.png))。

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**図 3**: 電子メール通知には、添付ファイルとして YSOD 例外の詳細が含まれています。  
 ([フルサイズの画像を表示する をクリックします](processing-unhandled-exceptions-vb/_static/image9.png))。

## <a name="what-about-using-the-custom-error-page"></a>カスタム エラー ページを使用してでしょうか。

このチュートリアルで使用する方法を示しました`Global.asax`、`Application_Error`ハンドルされない例外が発生したときにコードを実行するイベント ハンドラー。 具体的には、エラーの開発者に通知するこのイベント ハンドラーを使いましたまた、データベースでエラーの詳細をログに記録する拡張できます。 有無、`Application_Error`イベント ハンドラーでは、エンドユーザーのエクスペリエンスには影響しません。 これらには、構成済みのエラー ページで、これは、エラーの詳細 YSOD、ランタイム エラー YSOD、またはカスタム エラー ページも参照してください。

不思議に思うは当然だかどうか、`Global.asax`ファイルと`Application_Error`カスタム エラー ページを使用する場合は、イベントが必要です。 エラーが発生したカスタム エラー ページが表示されますので理由できないコードを作成しましたを開発者に通知し、カスタム エラー ページの分離コード クラスに、エラーの詳細をログしますか? 発生させた例外の詳細へのアクセスは必要はカスタム エラー ページの分離コード クラスにコードを追加できますが、確かに、`Error`イベント前のチュートリアルで調査されていること、この手法を使用する場合。 呼び出す、`GetLastError`カスタム エラー ページからメソッドを返します`Nothing`します。

この動作の理由のリダイレクトを使用して、カスタム エラー ページに達するためにです。 ハンドルされない例外に達した場合、ASP.NET ランタイム、ASP.NET エンジンを発生させますその`Error`イベント (を実行する、`Application_Error`イベント ハンドラー) をクリックし*リダイレクト*を発行することによってカスタムエラーページにユーザー`Response.Redirect(customErrorPageUrl)`. `Response.Redirect`メソッドは、新しい URL、つまりカスタム エラー ページを要求するブラウザーに指示する HTTP 302 ステータス コードでは、使用してクライアントに応答を送信します。 この新しいページを要求に自動的に、ブラウザーしています。 わかりますカスタム エラー ページがページから個別に要求されたカスタム エラー ページの URL にブラウザーのアドレス バーが変更されるため、エラーの発生元 (を参照してください**図 4**)。

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**図 4**: エラーが発生、ブラウザーは、カスタム エラー ページの URL にリダイレクトされます  
 ([フルサイズの画像を表示する をクリックします](processing-unhandled-exceptions-vb/_static/image12.png))。

実際の効果は、サーバーは HTTP 302 リダイレクトに応答するときにハンドルされない例外が発生した要求を終了します。 カスタム エラー ページに後続の要求は、新しい要求;この段階で、ASP.NET エンジン エラー情報が破棄し、さらに、カスタム エラー ページの新しい要求を前の要求で未処理の例外を関連付ける方法がありません。 これは、ため`GetLastError`返します`null`カスタム エラー ページから呼び出されるとします。

ただし、エラーが発生したのと同じ要求中に実行されるカスタム エラー ページを含めることは可能です。 [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx)メソッドが実行を指定した URL に転送して、同じ要求内で処理します。 コードを移動する可能性があります、`Application_Error`置換することで、カスタム エラー ページの分離コード クラスにイベント ハンドラーを`Global.asax`を次のコード。

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

ハンドルされない例外が発生時に今すぐ、`Application_Error`イベント ハンドラーは、HTTP ステータス コードに基づいて、適切なカスタム エラー ページに制御を転送します。 カスタム エラー ページが経由で未処理の例外情報へのアクセスを持つコントロールは、転送されたため、`Server.GetLastError`とできます、エラーの開発者に通知し、その詳細をログ記録します。 `Server.Transfer`呼び出しからユーザーをカスタム エラー ページにリダイレクトする、ASP.NET エンジンを停止します。 代わりに、カスタム エラー ページのコンテンツは、エラーが発生したページへの応答として返されます。

## <a name="summary"></a>まとめ

ASP.NET web アプリケーションでハンドルされない例外が発生すると、ASP.NET ランタイムを発生させます、`Error`イベントと、構成済みのエラー ページが表示されます。 開発者に、エラーの通知、その詳細をログ記録、またはエラー イベントのイベント ハンドラーを作成してその他のなんらかの方法で処理することをことができます。 イベント ハンドラーを作成する 2 つの方法はあります`HttpApplication`などのイベント`Error`: で、`Global.asax`ファイルまたは HTTP モジュールから。 このチュートリアルで作成する方法を示しました、`Error`内のイベント ハンドラー、`Global.asax`ファイルを電子メール メッセージを使用して、エラーの開発者に通知します。

作成、`Error`イベント ハンドラーがいくつか一意またはカスタマイズされた方法で未処理の例外を処理する必要がある場合に便利です。 ただし、独自に作成`Error`または開発者に通知する、例外をログにイベント ハンドラーはありません時間の最も効率的な使用がほんの数分でセットアップできる無料で使いやすいエラー ログ記録ライブラリに既に存在します。 次の 2 つのチュートリアルでは、このような 2 つのライブラリを確認します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET HTTP モジュールは、HTTP ハンドラーの概要](https://support.microsoft.com/kb/307985)
- [未処理の例外の処理 - 未処理の例外を適切に応答](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` クラスおよび ASP.NET アプリケーション オブジェクト](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP ハンドラーと ASP.NET HTTP モジュール](http://www.15seconds.com/Issue/020417.htm)
- [ASP.NET で電子メールを送信します。](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [について、`Global.asax`ファイル](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [HTTP モジュールとハンドラーを使用して、プラグ可能な ASP.NET コンポーネントを作成するには](https://msdn.microsoft.com/library/aa479332.aspx)
- [ASP.NET の操作`Global.asax`ファイル](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [操作`HttpApplication`インスタンス](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [前へ](displaying-a-custom-error-page-vb.md)
> [次へ](logging-error-details-with-asp-net-health-monitoring-vb.md)
