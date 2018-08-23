---
uid: signalr/overview/older-versions/troubleshooting
title: SignalR トラブルシューティング (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: この記事では、SignalR アプリケーションの開発に関する一般的な問題について説明します。
ms.author: riande
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: df949347cecd9ac617a52ad798f37bebdb8524fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833539"
---
<a name="signalr-troubleshooting-signalr-1x"></a>SignalR トラブルシューティング (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このドキュメントでは、SignalR を使って一般的な問題のトラブルシューティングについて説明します。


このドキュメントには、次のセクションが含まれています。

- [サイレント モードでが失敗したクライアントとサーバー間のメソッドの呼び出し](#connection)
- [その他の接続の問題](#other)
- [コンパイルとサーバー側のエラー](#server)
- [Visual Studio の問題](#vs)
- [インターネット インフォメーション サービスの問題](#iis)
- [Azure の問題](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>サイレント モードでが失敗したクライアントとサーバー間のメソッドの呼び出し

このセクションでは、意味のあるエラー メッセージを表示せずに失敗するには、クライアントとサーバー間のメソッド呼び出しの考えられる原因について説明します。 SignalR アプリケーションで、サーバーに関する情報がない。 クライアントを実装する方法サーバーは、クライアント メソッドを呼び出しとメソッドの名前とパラメーターのデータがクライアントに送信される、メソッドが実行されるは、サーバーが指定した形式である場合にのみ。 一致するメソッドが検出されないクライアントでは、何も起こりません、サーバー上のエラー メッセージが発生しなかった場合は。

クライアント メソッドが呼び出されない作業をさらに調査するには、どのような呼び出しを表示するハブの start メソッドは、サーバーから送信される呼び出しの前にログ記録にできます。 JavaScript アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (JavaScript クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)します。 .NET クライアント アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (.NET クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-net-client.md#logging)します。

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>スペルの正しくないメソッド、不適切なメソッドのシグネチャ、または不適切なハブの名前

名前または呼び出されたメソッドのシグネチャが一致しない場合、クライアント上の適切なメソッド、呼び出しは失敗します。 メソッド名がサーバーによって呼び出されますが、クライアント上のメソッドの名前と一致することを確認します。 また、SignalR は camel 形式のメソッドを使用するハブ プロキシを作成します。 JavaScript では、そのため、メソッドと呼ばれます`SendMessage`サーバーでが呼び出されます`sendMessage`クライアント プロキシで。 使用する場合、`HubName`サーバー側コードで属性を使用する名前が、クライアントで、ハブの作成に使用する名前と一致していることを確認します。 使用しない場合、`HubName`属性、JavaScript クライアント内でハブの名前が ChatHub ではなく chatHub など、キャメル形式で表記であることを確認します。

### <a name="duplicate-method-name-on-client"></a>クライアント上のメソッド名が重複しています

大文字小文字によってのみとは異なるクライアントで重複するメソッドがないことを確認します。 場合は、クライアント アプリケーションがある呼び出されるメソッド`sendMessage`、いないというメソッドもを確認して`SendMessage`もします。

### <a name="missing-json-parser-on-the-client"></a>クライアントで不足している JSON のパーサー

SignalR では、JSON パーサーは、サーバーとクライアント間の呼び出しをシリアル化するために必要です。 クライアントが (Internet Explorer 7 の場合) などの組み込み JSON パーサーを持っていない場合は、アプリケーションのいずれかに含める必要があります。 JSON パーサーをダウンロードする[ここ](http://nuget.org/packages/json2)します。

### <a name="mixing-hub-and-persistentconnection-syntax"></a>ハブおよび PersistentConnection 構文を混在させる

SignalR は、2 つの通信モデルを使用します。 ハブおよび PersistentConnections します。 これらの 2 つの通信モデルを呼び出すための構文は、クライアント コードで異なります。 サーバー コードにハブを追加した場合は、すべてのクライアント コードは適切なハブの構文を使用することを確認します。

**JavaScript クライアント内で、PersistentConnection を作成する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Javascript クライアント内でハブ プロキシを作成する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# サーバー コードを PersistentConnection にルートをマップします。**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**複数のアプリケーションがある場合、ハブ、または複数のハブ ルートをマップ c# サーバー コード**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>サブスクリプションを追加する前に開始した接続

プロキシ サーバーから呼び出すことができるメソッドが追加される前に、ハブの接続が開始されると、メッセージが受信されません。 次の JavaScript コードは、ハブを正しく開始できません。

**ハブのメッセージを受信することはできません。 JavaScript クライアント コードが正しくないです。**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

代わりに、開始を呼び出す前に、メソッドのサブスクリプションを追加します。

**ハブに誤ってサブスクリプションを追加する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>ハブ プロキシのメソッド名がありません。

サーバーで定義されたメソッドがクライアントで購読していることを確認します。 場合でも、サーバーは、メソッドを定義、クライアント プロキシにも追加する必要があります。 メソッドは、次の方法でクライアント プロキシに追加できます (注、メソッドに追加される、`client`ハブのハブではなく直接のメンバー)。

**ハブ プロキシのメソッドを追加する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>ハブまたはハブ メソッドをパブリックとして宣言されていません

クライアントに表示される、ハブの実装とメソッドとして宣言する必要があります`public`します。

### <a name="accessing-hub-from-a-different-application"></a>別のアプリケーションからハブへのアクセス

SignalR ハブは、SignalR クライアントを実装するアプリケーションからのみアクセスできます。 SignalR ことはできません (SOAP サービスまたは WCF web サービスです。) などの他の通信ライブラリとの相互運用します。ターゲット プラットフォームの使用可能な SignalR クライアントがない場合、サーバーのエンドポイントに直接アクセスすることはできません。

### <a name="manually-serializing-data"></a>手動でデータをシリアル化

SignalR は自動的に JSON シリアル化に使用、メソッド パラメーターであるのなら自分自身でするのに必要ありません。

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>リモートのハブ メソッドをクライアント OnDisconnected 関数では実行されません。

この動作は意図されたものです。 ときに`OnDisconnected`が呼び出されると、ハブが既に入力、`Disconnected`によりさらにハブ メソッドを呼び出せる状態。

**OnDisconnected イベント内のコードを正しく実行 c# サーバー コード**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>接続の上限に達しました

Windows 7 などのクライアント オペレーティング システムでの完全版の IIS を使用する場合は、10 接続制限が適用されます。 クライアント OS を使用して、IIS Express 代わりに使用してこの制限を回避します。

### <a name="cross-domain-connection-not-set-up-properly"></a>ドメイン間の接続が正しく設定されていません

ドメイン間の接続の場合 (対象の SignalR URL が、ホスティング ページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。 ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>.NET クライアントで機能しない NTLM (Active Directory) を使用して接続

ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続が失敗する場合は、接続が正しく構成されていません。 SignalR を使用して、ドメイン環境で、次のように、必要な接続プロパティを設定します。

**接続の資格情報を実装する c# クライアント コード**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>その他の接続の問題

このセクションでは、原因と解決策の特定の現象または接続中に発生するエラー メッセージについて説明します。

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>「スタートにはデータを送信する前に呼び出す必要があります」エラー

このエラーは、接続を開始する前に、コードが SignalR オブジェクトを参照している場合によく見られます。 ハンドラーなどのワイヤアップがメソッドの呼び出し、サーバーで定義されている必要があります追加されること、接続が完了した後。 なおへの呼び出し`Start`は前に、呼び出しを実行することが後のコードが完了するために非同期でします。 接続が完全に開始した後、ハンドラーを追加する最善の方法は、start メソッドにパラメーターとして渡されるコールバック関数には。

**正しく SignalR オブジェクトを参照するイベント ハンドラーを追加する JavaScript クライアント コード**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

このエラーは、SignalR オブジェクトはまだ参照されているときに、接続が停止した場合にも表示されます。

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>「301 は完全に移動されました」または「302 が一時的に移動されました」エラー

このエラーは、プロジェクトには、SignalR で、自動的に作成されたプロキシが干渉をという名前のフォルダーが含まれている場合に発生する可能性があります。 このエラーを回避するために使用しないでくださいという名前のフォルダー`SignalR`アプリケーション、または有効にする自動プロキシの生成をオフにします。 参照してください[、生成されたプロキシとは何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)の詳細。

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>.NET または Silverlight クライアントに「403 アクセス不可」エラー

このエラーは、ドメイン間の通信が正しく有効化しないクロス ドメイン環境で発生する可能性があります。 ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。 Silverlight クライアントでのドメイン間の接続を確立するを参照してください。 [Silverlight クライアントからのドメインを越えた接続](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)します。

### <a name="404-not-found-error"></a>「404 見つかりません」エラー

この問題のいくつかの原因があります。 次のすべてを確認します。

- **ハブ プロキシ アドレスの参照が正しくフォーマットされていません:** このエラーは生成されたハブ プロキシのアドレスへの参照が正しくフォーマットされていない場合によく見られます。 ハブ アドレスへの参照が正しく行われたことを確認します。 参照してください[動的に生成されたプロキシを参照する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)詳細についてはします。
- **ハブ ルートを追加する前にアプリケーションへのルートの追加:** アプリケーションでは、他のルートを使用する場合、最初のルートが追加の呼び出しの確認`MapHubs`します。

### <a name="500-internal-server-error"></a>「500 内部サーバー エラー」

これは、さまざまな原因の可能性がある非常に一般的なエラーです。 エラーの詳細については、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグを確認できます。 サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。 詳細については、次を参照してください。[ハブ クラス内のエラーの処理方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)します。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt;が定義されていません"エラー

このエラーが発生する呼び出し`MapHubs`が正しく行われていません。 参照してください[SignalR のルートを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException がユーザー コードでハンドルされませんでした。

パラメーター、メソッドに送信するには、シリアル化できない型 (ファイル ハンドル、データベース接続など) が含まれていないことを確認します。 使用 (またはセキュリティのためのシリアル化の理由から)、クライアントに送信したくないのサーバー側オブジェクトにメンバーを使用する必要がある場合、`JSONIgnore`属性。

### <a name="protocol-error-unknown-transport-error"></a>"プロトコル エラー: 不明なトランスポート"エラー

このエラーは、クライアントは SignalR を使用するトランスポートをサポートしていない場合に発生する可能性があります。 参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)についてを SignalR でブラウザーを使用できます。

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>「JavaScript ハブ プロキシの生成が無効になっています。」

このエラーが発生`DisableJavaScriptProxies`で動的に生成されたプロキシへの参照を含むも中に設定されている`signalr/hubs`します。 プロキシを手動で作成する方法の詳細については、次を参照してください。 [、生成されたプロキシとは何が](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)します。

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>「接続 ID は形式が正しくありません」または「ユーザー id は、SignalR のアクティブな接続中に変更できません」エラー

このエラーは、認証を使用して、接続が停止する前に、クライアントがログアウトした場合に発生する可能性があります。 ソリューションでは、クライアントをログアウトする前に SignalR 接続を停止します。

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"エラーをキャッチできない: SignalR: jQuery が見つかりませんでした。 SignalR.js ファイルの前に jQuery が参照されていることを確認してください"のエラー

SignalR JavaScript クライアントでは、jQuery を実行する必要があります。 JQuery への参照が使用されるパスが有効であるおよび SignalR への参照を前に jQuery への参照が正しいことを確認します。

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"TypeError をキャッチできない: プロパティを読み取ることができません '&lt;プロパティ&gt;' 未定義の"エラー

このエラーは、jQuery またはハブ プロキシを適切に参照されていないことから発生します。 JQuery、およびハブ プロキシへの参照が使用されるパスが有効であると、ハブ プロキシへの参照を前に jQuery への参照が正しいことを確認します。 ハブ プロキシを既定の参照は、次のようになります。

**ハブ プロキシを正しく参照する HTML クライアント側のコード**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>「RuntimeBinderException はユーザー コードで未処理でした」エラー

このエラーが発生する場合の正しくないオーバー ロード`Hub.On`使用されます。 メソッドの戻り値の場合、戻り値の型がジェネリック型パラメーターとして指定する必要があります。

**(生成されたプロキシは) を使用せず、クライアントで定義されたメソッド**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>接続 ID が一貫性のあるか、ページ読み込みの間で接続が切断

この動作は意図されたものです。 ハブ オブジェクトは、page オブジェクトでホストされている、ため、ハブは、ページが更新されると破棄されます。 複数ページのアプリケーションは、ページ読み込みの間の一貫性になるように、ユーザーと接続 Id 間の関連付けを維持する必要があります。 接続 Id は、いずれかで、サーバーに格納できる、`ConcurrentDictionary`オブジェクトまたはデータベース。

### <a name="value-cannot-be-null-error"></a>「値を null にすることはできません」エラー

省略可能なパラメーターを持つサーバー側のメソッドは現在サポートされていません。省略可能なパラメーターを省略した場合、メソッドは失敗します。 詳細については、次を参照してください。[省略可能なパラメーター](https://github.com/SignalR/SignalR/issues/324)します。

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox でサーバーへの接続を確立できません&lt;アドレス&gt;"Firebug でのエラー

このエラー メッセージは、WebSocket トランスポートのネゴシエーションは失敗し、他のトランスポートが代わりに使用される場合、Firebug で確認できます。 この動作は意図されたものです。

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>.NET クライアント アプリケーションで「リモート証明書が検証の手順に従って無効です」エラー

場合は、サーバーでは、要求が行われる前に、接続に x509certificate を追加することができますし、カスタムのクライアント証明書が必要です。 接続を使用して、証明書を追加`Connection.AddClientCertificate`します。

### <a name="connection-drops-after-authentication-times-out"></a>認証のタイムアウト後の接続を削除します

この動作は意図されたものです。 接続がアクティブになったときに、認証資格情報を変更することはできません。資格情報を更新するには、接続を停止および再起動してする必要があります。

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected が jQuery Mobile を使用する場合に 2 回呼び出されます

Mobile を jQuery`initializePage`関数は、再実行するには、各ページのスクリプトは、2 番目の接続を作成します。 この問題のソリューションは次のとおりです。

- JQuery Mobile、JavaScript ファイルへの参照が含まれます。
- 無効にする、`initializePage`関数を設定して`$.mobile.autoInitializePage = false`します。
- 接続を開始する前に初期化を完了してページを待機します。

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>サーバー送信イベントを使用して Silverlight アプリケーションでメッセージが遅延します。

Silverlight でイベントを送信するサーバーを使用する場合、メッセージが遅延します。 長い代わりに使用するポーリングを強制するには、接続を開始するときに、次を使用します。

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>プロトコルのフレーム「アクセス許可が拒否されました」を使用して永久に

これは既知の問題で説明されている[ここ](https://github.com/SignalR/SignalR/issues/1963)します。 この現象は、最新 JQuery ライブラリを使用して表示する可能性があります。回避策では、JQuery 1.8.2 にアプリケーションをダウン グレードします。

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>コンパイルとサーバー側のエラー

 次のセクションには、コンパイラとサーバー側のランタイム エラーの考えられる解決策が含まれています。 

### <a name="reference-to-hub-instance-is-null"></a>ハブ インスタンスへの参照が null

接続ごとにハブ インスタンスが作成された後できませんインスタンスを作成するハブのコードで自分でします。 ハブ自体では、外部からハブのメソッドを呼び出すを参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)のハブ コンテキストへの参照を取得する方法。

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session が null

この動作は意図されたものです。 双方向メッセージング中断は、セッション状態を有効にするため、SignalR は ASP.NET セッション状態をサポートしません。

### <a name="no-suitable-method-to-override"></a>オーバーライドする適切なメソッドはありません。

以前のドキュメントまたはブログからのコードを使用している場合は、このエラーを参照してください可能性があります。 変更または非推奨とされているメソッドの名前を参照していないことを確認します (など`OnConnectedAsync`)。

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl が null

この動作は意図されたものです。 このメンバーは非推奨とされますは使用できません。

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>「'Signalr.hubs' という名前のルートは既にルート コレクションがいます」エラー

場合、このエラーが発生する`MapHubs`は、アプリケーションによって 2 回呼び出されます。 いくつかの例のアプリケーション呼び出し`MapHubs`ラッパー クラスでの呼び出しを行う他のユーザーは、グローバル アプリケーション ファイルで直接します。 アプリケーションはないこと両方を確認します。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio の問題

このセクションでは、Visual Studio で発生する問題について説明します。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>ソリューション エクスプ ローラーでスクリプト ドキュメントのノードが表示されません。

チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで [スクリプト ドキュメント] ノードに送る。 このノードは、JavaScript デバッガーによって生成され、Internet explorer のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合は、ノードは表示されません。 JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR では、Visual Studio 2008 またはそれ以前は機能しません

この動作は意図されたものです。 SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降に SignalR アプリケーションを開発することが必要です。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS の問題

このセクションには、インターネット インフォメーション サービスの問題が含まれています。

### <a name="web-site-crashes-after-maphubs-call"></a>MapHubs 呼び出し後、web サイトがクラッシュします。

この問題は SignalR の最新バージョンで修正されました。 NuGet を使用して、インストールを更新して最新のリリース バージョンの SignalR を使用していることを確認します。

<a id="azure"></a>

## <a name="azure-issues"></a>Azure の問題

このセクションには、Microsoft Azure での問題が含まれています。

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>メッセージがトピック名を変更した後は Azure のバック プレーンを介して受信されていません。

Azure のバック プレーンで使用されるトピックは、ユーザー構成可能にするものではありません。
