---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: "SignalR のトラブルシューティング |Microsoft ドキュメント"
author: pfletcher
description: "この記事では、SignalR アプリケーションを開発の一般的な問題について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: d7a1dcc04baaa5ab27aecf95936d943f5a9b3f0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting"></a>SignalR のトラブルシューティング
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このドキュメントでは、SignalR の一般的な問題のトラブルシューティングについて説明します。
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


このドキュメントには、次のセクションが含まれています。

- [サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す](#connection)
- [Ping/pong を停止しているクライアントを検出する IIS websocket を構成します。](#pong)
- [その他の接続の問題](#other)
- [コンパイル、およびサーバー側エラー](#server)
- [Visual Studio の問題](#vs)
- [インターネット インフォメーション サービスの問題](#iis)
- [Microsoft Azure を発行します。](#azure)

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

[!code-css[Main](troubleshooting/samples/sample4.css)]

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

### <a name="ondisconnect-not-firing-at-consistent-times"></a>一貫したタイミングで発生しない OnDisconnect

この動作は意図されたものです。 ユーザーがアクティブな SignalR 接続に関するページから移動しようとしたとき、SignalR クライアントは、クライアント接続が停止されることをサーバーに通知するベストエフォートの試行を加えます。 SignalR クライアントの最善の努力はサーバーに到達する試行が失敗した、サーバーが後に、構成可能な接続の破棄は`DisconnectTimeout`れた時点で、後で、`OnDisconnected`イベントが発生します。 SignalR クライアント ベスト エフォートの試行が成功すると、`OnDisconnected`イベントがすぐに起動されます。

設定の詳細について、`DisconnectTimeout`設定、表示[接続の有効期間イベントの処理: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)です。

### <a name="connection-limit-reached"></a>接続の制限に達しました

を Windows 7 などのクライアント オペレーティング システムで、フル バージョンの IIS を使用する場合は、10 接続制限が適用されます。 クライアントの OS を使用して、IIS Express 代わりに使用してこの制限を回避します。

### <a name="cross-domain-connection-not-set-up-properly"></a>ドメイン間の接続が正しく設定されていません

接続がドメインを越えた場合 (対象の SignalR URL がホストするページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。 ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>.NET クライアントで作業していない NTLM (Active Directory) を使用して接続

ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続には、接続が正しく構成されていない場合があります。 SignalR を使用して、ドメイン環境では、次のように、必要な接続プロパティを設定します。

**C# クライアントを実装するコード接続の資格情報**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Ping/pong を停止しているクライアントを検出する IIS websocket を構成します。

SignalR のサーバーには、クライアントが切れているか、その依存している接続の障害を基になる websocket からの通知は、OnClose コールバックが不明です。 この問題の 1 つのソリューションでは、IIS websocket ping/pong を自動的に行うを構成します。 これにより、予期せずが壊れた場合、接続が閉じられます。 詳細については、次を参照してください。[この stackoverflow の投稿](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)です。

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

- **ハブ プロキシ アドレスの参照が正しくフォーマットされていません:**このエラーは生成されたハブ プロキシのアドレスへの参照が正しくフォーマットされていない場合も表示されます。 ハブのアドレスへの参照が正しく行われたことを確認します。 参照してください[を動的に生成されたプロキシを参照する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)詳細についてはします。
- **ハブ ルートを追加する前に、アプリケーションへのルートの追加:**場合、アプリケーションでは、他のルートを使用することを確認追加された最初のルートへの呼び出し`MapSignalR`です。
- **IIS 7 または更新しないで 7.5 拡張子のない Url を使用する:**を使用して IIS 7 または 7.5 更新が要求される拡張子のない Url のサーバーで、ハブの定義へのアクセスを提供できるように`/signalr/hubs`です。 更新プログラムが見つかります[ここ](https://support.microsoft.com/kb/980368/en-us)です。
- **IIS が最新でないキャッシュまたは破損している:**キャッシュの内容が最新でないでないことを確認するには、キャッシュをクリアする PowerShell ウィンドウで次のコマンドを入力します。

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>「500 内部サーバー エラー」

これは、さまざまな原因の可能性がある非常に一般的なエラーです。 エラーの詳細は、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグから確認できます。 サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。 詳細については、次を参照してください。[ハブ クラスのエラーを処理する方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)です。

ファイアウォールまたはプロキシが構成されていない場合、適切要求ヘッダーを書き直すことが原因と、このエラーは発生も一般的です。 ソリューションでは、ファイアウォールまたはプロキシでポート 80 が有効になっていることを確認してください。

### <a name="unexpected-response-code-500"></a>"予期しない応答コード: 500"

このエラーは、アプリケーションで使用される .NET framework のバージョンが Web.Config で指定されたバージョンと一致しない場合に発生する可能性があります。この解決策では、.NET 4.5 がアプリケーションの設定と、Web.Config ファイルの両方で使用されることを確認してください。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt;が定義されていません"エラー

このエラーが発生する呼び出し`MapSignalR`が正しく行われない。 参照してください[SignalR ミドルウェアを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。

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

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>「ユーザー コードによってハンドルされませんでした。 RuntimeBinderException」エラー

このエラーが発生するときの正しくないオーバー ロード`Hub.On`を使用します。 メソッドの戻り値が、戻り値の型がジェネリック型パラメーターとして指定する必要があります。

**(生成されたプロキシは) なしのクライアントに定義されたメソッド**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

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

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>プロトコルを永久にフレーム「アクセス許可が拒否されました」を使用します。

これは、説明されている既知の問題[ここ](https://github.com/SignalR/SignalR/issues/1963)です。 この現象は、最新の JQuery ライブラリ; を使用して表示することがあります。回避策では、JQuery 1.8.2 にアプリをダウン グレードします。

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: 有効な web ソケット リクエストではありません。

このエラーは、WebSocket プロトコルを使用すると、ネットワーク プロキシには、要求ヘッダーを変更する場合に発生する可能性があります。 ソリューションでは、ポート 80 で WebSocket を許可するプロキシを構成します。

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"例外:&lt;メソッド名&gt;メソッドを解決できませんでした"クライアントがサーバーでメソッドを呼び出すと

このエラーは検出できません。 配列などの JSON ペイロードのデータ型を使用することがあります。 回避策では、IList などの JSON で検出することのあるデータ型を使用します。 詳細については、次を参照してください。 [.NET クライアント配列パラメーターを持つハブ メソッドを呼び出すことができません](https://github.com/SignalR/SignalR/issues/2672)です。

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

場合は、このエラーが発生する`MapSignalR`は、アプリケーションによって 2 回呼び出されます。 いくつかの呼び出しの例のアプリケーション`MapSignalR`直接スタートアップ クラスで他のユーザーに呼び出しラッパー クラスです。 あるアプリケーションで実行しない両方を確認します。

### <a name="websocket-is-not-used"></a>WebSocket は使用されません。

サーバーとクライアントが WebSocket の要件を満たしていることを確認した場合 (に一覧表示、[サポートされているプラットフォーム](../getting-started/supported-platforms.md)ドキュメント)、サーバーで WebSocket を有効にする必要があります。 これを行うための手順を参照して[ここ](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)です。

### <a name="connection-is-undefined"></a>$.connection が定義されていません

このエラーは、ページ上のスクリプトが正常に読み込まれていないかあるハブ プロキシが到達できないか、正しくアクセスしているを示します。 ページ上のスクリプト参照がプロジェクトに読み込まれたスクリプトに対応していると、サーバーが実行されている場合、/signalr/hubs をブラウザーでアクセスできることを確認します。

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>動的な式のコンパイルに必要な 1 つまたは複数の型が見つかりません

このエラーには、ことを示します、`Microsoft.CSharp`ライブラリがありません。 追加、**アセンブリ -&gt;Framework**タブです。

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>Visual Basic または厳密に型指定されたハブ; Clients.Caller から呼び出し元の状態にアクセスできません。「型 'Task (Of Object)' を 'String' 型からの変換が正しくありません」エラー

Visual Basic または厳密に型指定されたハブの呼び出し元の状態にアクセスするには、使用、`Clients.CallerState`の代わりに (SignalR 2.1 で導入された) プロパティ`Clients.Caller`です。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio の問題

このセクションでは、Visual Studio で発生する問題について説明します。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>ソリューション エクスプ ローラーでスクリプト ドキュメント ノードが表示されません。

チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで、[スクリプト ドキュメント] ノードに直接します。 このノードは、JavaScript デバッガーによって生成され、Internet Explorer; 内のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合、ノードは表示されません。 JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR では、Visual Studio 2008 またはそれ以前は機能しません

この動作は意図されたものです。 SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降で SignalR アプリケーションを開発することが必要です。 SignalR のサーバー コンポーネントには、.NET Framework 4.5 が必要です。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS の問題

このセクションには、インターネット インフォメーション サービスで問題が含まれています。

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>IIS ではなく Visual Studio 開発サーバーで、SignalR の動作します。

IIS 7.0、7.5、SignalR はサポートされますが、拡張子のない Url を追加する必要がありますをサポートします。 拡張子のない Url のサポートを追加するを参照してください[https://support.microsoft.com/kb/980368。](https://support.microsoft.com/kb/980368)

SignalR には、ASP.NET (ASP.NET がインストールされていない IIS で既定で) サーバーにインストールする必要があります。 ASP.NET をインストールするを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)です。

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Microsoft Azure を発行します。

このセクションには、Microsoft Azure での問題が含まれています。

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException Azure ワーカー ロールで SignalR をホストする場合

SignalR Azure ワーカー ロールでホストしている例外になる可能性があります"ファイルまたはアセンブリを読み込めませんでした ' Microsoft.Owin、バージョン 2.0.0.0 を ="です。 これは、NuGet; の既知の問題バインド リダイレクトは、Azure ワーカー ロール プロジェクトでは自動的に追加されません。 これを解決するには、バインド リダイレクトを手動で追加することができます。 次の行を追加、`app.config`のワーカー ロール プロジェクトのファイルです。

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>メッセージがトピック名を変更した後、Azure のバック プレーンから受信されていません。

Azure バック プレーンで使用されるトピックは内部的に保持されます。ユーザー構成可能であるこれらの目的ではありません。
