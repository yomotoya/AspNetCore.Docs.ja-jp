---
uid: signalr/overview/older-versions/troubleshooting
title: SignalR のトラブルシューティング (SignalR 1.x) |Microsoft ドキュメント
author: pfletcher
description: この記事では、SignalR アプリケーションを開発の一般的な問題について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505821"
---
<a name="signalr-troubleshooting-signalr-1x"></a>SignalR のトラブルシューティング (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このドキュメントでは、SignalR の一般的な問題のトラブルシューティングについて説明します。


このドキュメントには、次のセクションが含まれています。

- [サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す](#connection)
- [その他の接続の問題](#other)
- [コンパイル、およびサーバー側エラー](#server)
- [Visual Studio の問題](#vs)
- [インターネット インフォメーション サービスの問題](#iis)
- [Azure の問題](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す

このセクションでは、メソッドの呼び出しがわかりやすいエラー メッセージを表示せずに失敗するには、クライアントとサーバー間の考えられる原因について説明します。 SignalR のアプリケーションで、サーバー情報がありません。 クライアントを実装する方法の詳細サーバーでは、クライアント メソッドを呼び出すとメソッドの名前とパラメーターのデータは、クライアントに送信されます、サーバーが指定した形式で存在する場合にのみ、メソッドが実行されます。 一致するメソッドが検出されないクライアントでは、何も起こりません、サーバー上のエラー メッセージは発生しません。

が呼び出されたクライアント メソッドをさらに調査するには、どのような呼び出しを表示するハブの start メソッドが、サーバーから送信されて呼び出しの前にログ記録をオンにすることができません。 JavaScript アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (JavaScript クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)です。 .NET クライアント アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (.NET クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-net-client.md#logging)です。

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>スペル ミスがあるメソッド、不適切なメソッドのシグネチャまたは不適切なハブ名

名前または呼び出されたメソッドのシグネチャが一致しない場合、クライアントに適切なメソッド、呼び出しは失敗します。 サーバーによって呼び出されるメソッドの名前が、クライアントのメソッドの名前と一致していることを確認します。 SignalR による camel 形式のメソッドを使用するハブ プロキシの作成も、JavaScript では、そのため、メソッドと呼ばれます`SendMessage`サーバーでが呼び出されます`sendMessage`クライアント プロキシでします。 使用する場合、`HubName`サーバー側コードで属性を使用する名前が、クライアントで、ハブの作成に使用する名前と一致していることを確認します。 使用しない場合、`HubName`属性、JavaScript クライアントのハブの名前が camel 形式の ChatHub ではなく chatHub などであることを確認します。

### <a name="duplicate-method-name-on-client"></a>クライアント上の重複したメソッド名

大文字小文字によってのみとは異なるクライアントのメソッドの重複がないことを確認します。 場合は、クライアント アプリケーションがある呼び出されるメソッド`sendMessage`、いないという名前のメソッドもを確認して`SendMessage`もします。

### <a name="missing-json-parser-on-the-client"></a>クライアントで JSON パーサーが不足しています。

SignalR には、JSON パーサーは、サーバーとクライアント間の呼び出しをシリアル化に存在する必要があります。 クライアントには、Internet Explorer 7) などの組み込み JSON パーサーが割り当てられていないは、アプリケーションで 1 つ含める必要があります。 JSON パーサーをダウンロードする[ここ](http://nuget.org/packages/json2)です。

### <a name="mixing-hub-and-persistentconnection-syntax"></a>ハブと PersistentConnection 構文を混在させる

SignalR が 2 つの通信モデルを使用します。 ハブおよび PersistentConnections です。 これら 2 つの通信のモデルを呼び出すための構文は、クライアント コードで異なります。 サーバー コードに、ハブを追加した場合は、適切なハブの構文を使用してすべてのクライアント コードを確認します。

**JavaScript クライアント コード、JavaScript クライアントで、PersistentConnection を作成します。**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**JavaScript クライアント コードで、Javascript クライアント ハブ プロキシを作成します。**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# サーバー コード、PersistentConnection にルートをマップします。**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**複数のアプリケーションがある場合、ハブ、または複数のハブにルートをマップ c# サーバー コード**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>サブスクリプションが追加される前に開始した接続

プロキシ サーバーから呼び出すことができるメソッドが追加される前に、ハブの接続が開始されている場合、メッセージは受信されません。 次の JavaScript コードは、ハブを正しく開始できません。

**ハブのメッセージを受信することはできません。 JavaScript クライアント コードが正しくないです。**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

代わりに、開始を呼び出す前に、メソッドのサブスクリプションを追加します。

**正しくハブにサブスクリプションを追加する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>ハブ プロキシのメソッド名がありません。

クライアントで、サーバーで定義されているメソッドにサブスクライブを確認します。 メソッドを定義して、サーバー、場合でも、クライアント プロキシを引き続き追加できます必要があります。 次の方法でクライアントのプロキシ メソッドを追加することができます (注メソッドに追加される、`client`ハブ、いないハブ直接のメンバー)。

**ハブ プロキシのメソッドを追加する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>ハブまたはハブのメソッドがパブリックとして宣言されていません

クライアントに表示する、ハブの実装とメソッドとして宣言されなければなりません`public`です。

### <a name="accessing-hub-from-a-different-application"></a>別のアプリケーションからハブにアクセスします。

SignalR ハブは、SignalR クライアントを実装するアプリケーションを介してのみアクセスできることができます。 SignalR は、その他の通信ライブラリ (SOAP または WCF web サービスなどです。) と相互運用することはできません。ターゲット プラットフォームの利用可能な SignalR クライアントがない場合は、サーバーのエンドポイントを直接アクセスすることはできません。

### <a name="manually-serializing-data"></a>手動でデータをシリアル化します。

SignalR は自動的に JSON シリアル化に使用、メソッド パラメーター-がのしなくても自分で行います。

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>リモートのハブ メソッドをクライアントの OnDisconnected 関数には実行されません。

この動作は意図されたものです。 ときに`OnDisconnected`が呼び出されると、ハブが既に入力、`Disconnected`が許可されていないさらにハブ メソッドを呼び出せる状態です。

**C# サーバー コードを正しく OnDisconnected イベント内のコードを実行**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>接続の制限に達しました

を Windows 7 などのクライアント オペレーティング システムで、フル バージョンの IIS を使用する場合は、10 接続制限が適用されます。 クライアントの OS を使用して、IIS Express 代わりに使用してこの制限を回避します。

### <a name="cross-domain-connection-not-set-up-properly"></a>ドメイン間の接続が正しく設定されていません

接続がドメインを越えた場合 (対象の SignalR URL がホストするページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。 ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>.NET クライアントで作業していない NTLM (Active Directory) を使用して接続

ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続には、接続が正しく構成されていない場合があります。 SignalR を使用して、ドメイン環境では、次のように、必要な接続プロパティを設定します。

**C# クライアントを実装するコード接続の資格情報**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>その他の接続の問題

このセクションでは、原因と解決策の特定の現象または接続中に発生するエラー メッセージについて説明します。

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>「開始にはデータを送信する前に呼び出す必要があります」エラー

接続を開始する前に、コードが SignalR オブジェクトを参照している場合、このエラーは発生一般的です。 ワイヤアップ ハンドラーなどのメソッドの呼び出しに、サーバーで定義されている必要があります追加することが、接続が完了した後にします。 なおへの呼び出し`Start`はその前に呼び出しを実行することが後にコードが完了するために非同期。 接続が完全に開始した後にハンドラーを追加する最善の方法は、start メソッドにパラメーターとして渡されるコールバック関数には。

**正しく SignalR オブジェクトを参照するイベント ハンドラーを追加する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

このエラーは、SignalR オブジェクトが参照されているときに、接続が停止した場合にも表示されます。

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>「301 が完全に移動しました」または「302 は一時的に移動されました」エラー

このエラーは、プロジェクトには、SignalR で、自動的に作成されたプロキシには影響をという名前のフォルダーが含まれている場合に発生する可能性があります。 このエラーを避けるためには、使用しないという名前のフォルダー`SignalR`アプリケーション、または有効にするプロキシの自動生成をオフにします。 参照してください[、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)詳細についてはします。

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>.NET または Silverlight クライアントで「403 アクセス不可」エラー

このエラーは、ドメイン間の通信が正しく有効化されませんクロス ドメイン環境で発生する可能性があります。 ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。 Silverlight クライアントで、ドメイン間の接続を確立する、次を参照してください。 [Silverlight クライアントからドメイン間の接続](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)です。

### <a name="404-not-found-error"></a>「404 見つかりません」エラー

この問題のいくつかの原因があります。 次のすべてを確認します。

- **ハブ プロキシ アドレスの参照が正しくフォーマットされていません:** このエラーは生成されたハブ プロキシのアドレスへの参照が正しくフォーマットされていない場合も表示されます。 ハブのアドレスへの参照が正しく行われたことを確認します。 参照してください[を動的に生成されたプロキシを参照する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)詳細についてはします。
- **ハブ ルートを追加する前に、アプリケーションへのルートの追加:** 場合、アプリケーションでは、他のルートを使用することを確認追加された最初のルートへの呼び出し`MapHubs`です。

### <a name="500-internal-server-error"></a>「500 内部サーバー エラー」

これは、さまざまな原因の可能性がある非常に一般的なエラーです。 エラーの詳細は、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグから確認できます。 サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。 詳細については、次を参照してください。[ハブ クラスのエラーを処理する方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)です。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt;が定義されていません"エラー

このエラーが発生する呼び出し`MapHubs`が正しく行われない。 参照してください[SignalR のルートを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException がユーザー コードによってハンドルされませんでした。

メソッドに送信するパラメーターには、シリアル化不可能な (のような種類ファイル ハンドルやデータベース接続) にはが含まれていないことを確認します。 使用 (またはセキュリティのためのシリアル化の理由から)、クライアントに送信したくない、サーバー側オブジェクトにメンバーを使用する必要がある場合、`JSONIgnore`属性。

### <a name="protocol-error-unknown-transport-error"></a>"プロトコル エラー: 不明なトランスポート"エラー

このエラーは、クライアントは、SignalR を使用するトランスポートをサポートしていない場合に発生する可能性があります。 参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)についてを SignalR でブラウザーを使用できます。

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>「JavaScript Hub プロキシ生成は無効になっています。」

このエラーが発生`DisableJavaScriptProxies`で動的に生成されたプロキシへの参照を含めているときに設定されている`signalr/hubs`です。 手動でプロキシを作成する方法の詳細については、次を参照してください。 [、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)です。

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"接続 ID の形式が正しくありません"または「アクティブな SignalR 接続中に、ユーザー id を変更できません」エラー

このエラーは、認証が使用されていて、クライアントがログアウトしている接続を停止する前に発生する可能性があります。 ソリューションは、クライアントをログアウトする前に、SignalR 接続を停止するためです。

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"エラーをキャッチしませんでした: SignalR: jQuery が見つかりませんでした。 JQuery が SignalR.js ファイルの前に参照されていることを確認してください。"エラー

SignalR JavaScript クライアントには、jQuery を実行する必要があります。 JQuery への参照が使用されるパスが有効であることと jQuery への参照が SignalR への参照の前に、正しいことを確認します。

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"TypeError をキャッチしませんでしたプロパティを読み取ることができません '&lt;プロパティ&gt;' 未定義の"エラー。

このエラーは、jQuery またはハブ プロキシが適切に参照されていないことから発生します。 JQuery、およびハブ プロキシへの参照が使用されるパスが有効であることと jQuery への参照がハブ プロキシへの参照の前に、正しいことを確認します。 ハブ プロキシへの参照を既定値は、次のようになります。

**ハブ プロキシを正しく参照する HTML クライアント側コード**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>「ユーザー コードによってハンドルされませんでした。 RuntimeBinderException」エラー

このエラーが発生するときの正しくないオーバー ロード`Hub.On`を使用します。 メソッドの戻り値が、戻り値の型がジェネリック型パラメーターとして指定する必要があります。

**(生成されたプロキシは) なしのクライアントに定義されたメソッド**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>接続 ID が一貫性のあるか、ページの読み込みの間で接続が切断

この動作は意図されたものです。 ハブ オブジェクトが、page オブジェクトにホストされているため、ハブは、ページが更新されると破棄されます。 複数ページ アプリケーションは、ページの読み込みの間の一貫性ができるように、ユーザーと接続 Id 間の関連付けを維持する必要があります。 接続 Id は、サーバーのいずれかに格納できる、`ConcurrentDictionary`オブジェクトまたはデータベース。

### <a name="value-cannot-be-null-error"></a>「値を null にすることはできません」エラー

省略可能なパラメーターを使用してサーバー側メソッドは現在サポートされていません。省略可能なパラメーターを省略すると、メソッドは失敗します。 詳細については、次を参照してください。[省略可能なパラメーター](https://github.com/SignalR/SignalR/issues/324)です。

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox でサーバーへの接続を確立できません&lt;アドレス&gt;"Firebug のエラー

WebSocket トランスポートのネゴシエーションは失敗し、別のトランスポートが代わりに使用される場合、Firebug でこのエラー メッセージを表示できます。 この動作は意図されたものです。

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>.NET クライアント アプリケーションで「リモート証明書は、検証手順に従った有効な」エラーが発生します。

場合は、サーバーでは、要求が行われる前に、接続に x509certificate を追加することができますし、カスタム クライアント証明書が必要です。 接続を使用して、証明書を追加`Connection.AddClientCertificate`です。

### <a name="connection-drops-after-authentication-times-out"></a>接続の認証のタイムアウト後のドロップします。

この動作は意図されたものです。 接続がアクティブになったときに、認証の資格情報を変更することはできません。資格情報を更新するには、接続を停止および再起動して必要があります。

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected は jQuery Mobile を使用する場合に 2 回呼び出されます

jQuery Mobile の`initializePage`関数は、再実行するには、各ページのスクリプトは、2 番目の接続を作成します。 この問題の解決策は次のとおりです。

- JQuery Mobile、JavaScript ファイルの前にへの参照が含まれます。
- 無効にする、`initializePage`関数を設定して`$.mobile.autoInitializePage = false`です。
- 接続を開始する前に初期化を完了してページを待機します。

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>サーバー送信イベントを使用する Silverlight アプリケーションでメッセージが遅延します。

Silverlight でイベントを送信するサーバーを使用するときに、メッセージが遅延します。 長いポーリングが代わりに使用するには、接続を開始するときに、次を使用します。

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>プロトコルを永久にフレーム「アクセス許可が拒否されました」を使用します。

これは、説明されている既知の問題[ここ](https://github.com/SignalR/SignalR/issues/1963)です。 この現象は、最新の JQuery ライブラリ; を使用して表示することがあります。回避策では、JQuery 1.8.2 にアプリをダウン グレードします。

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>コンパイル、およびサーバー側エラー

 次のセクションには、コンパイラとサーバー側のランタイム エラーの考えられる解決策が含まれています。 

### <a name="reference-to-hub-instance-is-null"></a>ハブ インスタンスへの参照が null

ハブ インスタンスは接続ごとに作成される、ためにできませんインスタンスを作成するハブのコードで自分でします。 ハブ自体の外部からのハブのメソッドを呼び出すを参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)のハブ コンテキストへの参照を取得する方法です。

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session が null です。

この動作は意図されたものです。 双方向のメッセージング中断は、セッションの状態を有効にするため、SignalR は ASP.NET セッション状態をサポートしません。

### <a name="no-suitable-method-to-override"></a>オーバーライドする適切なメソッドはありません。

以前のドキュメントまたはブログからのコードを使用している場合は、このエラーを表示することがあります。 変更されたか推奨されなくなりましたがメソッドの名前を参照していないことを確認してください (と同様に`OnConnectedAsync`)。

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl が null です。

この動作は意図されたものです。 このメンバーは推奨されておらずは使用できません。

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>「'Signalr.hubs' という名前のルートは既にルートのコレクションに」エラー

場合は、このエラーが発生する`MapHubs`は、アプリケーションによって 2 回呼び出されます。 いくつかの呼び出しの例のアプリケーション`MapHubs`ラッパー クラスでの呼び出しを行う他のユーザーのグローバルなアプリケーション ファイルに直接できます。 あるアプリケーションで実行しない両方を確認します。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio の問題

このセクションでは、Visual Studio で発生する問題について説明します。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>ソリューション エクスプ ローラーでスクリプト ドキュメント ノードが表示されません。

チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで、[スクリプト ドキュメント] ノードに直接します。 このノードは、JavaScript デバッガーによって生成され、Internet Explorer; 内のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合、ノードは表示されません。 JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR では、Visual Studio 2008 またはそれ以前は機能しません

この動作は意図されたものです。 SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降で SignalR アプリケーションを開発することが必要です。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS の問題

このセクションには、インターネット インフォメーション サービスで問題が含まれています。

### <a name="web-site-crashes-after-maphubs-call"></a>MapHubs 呼び出し後に、web サイトがクラッシュします。

この問題は、SignalR の最新のバージョンで修正されました。 NuGet を使用して、インストールを更新することによって、SignalR の最新のリリース バージョンを使用していることを確認します。

<a id="azure"></a>

## <a name="azure-issues"></a>Azure の問題

このセクションには、Microsoft Azure での問題が含まれています。

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>メッセージがトピック名を変更した後、Azure のバック プレーンから受信されていません。

Azure バック プレーンで使用されるトピックは、ユーザー構成可能にするものではありません。
