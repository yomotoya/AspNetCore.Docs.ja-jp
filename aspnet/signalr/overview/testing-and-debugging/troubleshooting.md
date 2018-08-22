---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: SignalR トラブルシューティング |Microsoft Docs
author: pfletcher
description: この記事では、SignalR アプリケーションの開発に関する一般的な問題について説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 77eedeb962bed06f1375284bcf05c4e4ffcdde3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826730"
---
<a name="signalr-troubleshooting"></a>SignalR トラブルシューティング
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このドキュメントでは、SignalR を使って一般的な問題のトラブルシューティングについて説明します。
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 2 のバージョン
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


このドキュメントには、次のセクションが含まれています。

- [サイレント モードでが失敗したクライアントとサーバー間のメソッドの呼び出し](#connection)
- [停止したクライアントを検出するためにピンポン IIS websocket を構成します。](#pong)
- [その他の接続の問題](#other)
- [コンパイルとサーバー側のエラー](#server)
- [Visual Studio の問題](#vs)
- [インターネット インフォメーション サービスの問題](#iis)
- [Microsoft Azure を発行します。](#azure)

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

[!code-css[Main](troubleshooting/samples/sample4.css)]

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

### <a name="ondisconnect-not-firing-at-consistent-times"></a>一貫性のタイミングで発生しない OnDisconnect

この動作は意図されたものです。 ユーザーがアクティブな SignalR 接続に関するページから移動しようとすると、SignalR クライアントは、クライアント接続が停止されることをサーバーに通知するベストエフォートの試行を加えます。 SignalR クライアントのベスト エフォート場合は、サーバーに到達する試行が失敗した後、構成可能な接続の破棄は、サーバー、`DisconnectTimeout`時点で、後で、`OnDisconnected`イベントが発生します。 試行が成功すると、SignalR クライアントのベスト エフォートである場合、`OnDisconnected`イベントは、すぐに発生します。

設定の詳細について、`DisconnectTimeout`設定、表示[接続の有効期間イベントの処理: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)します。

### <a name="connection-limit-reached"></a>接続の上限に達しました

Windows 7 などのクライアント オペレーティング システムでの完全版の IIS を使用する場合は、10 接続制限が適用されます。 クライアント OS を使用して、IIS Express 代わりに使用してこの制限を回避します。

### <a name="cross-domain-connection-not-set-up-properly"></a>ドメイン間の接続が正しく設定されていません

ドメイン間の接続の場合 (対象の SignalR URL が、ホスティング ページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。 ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>.NET クライアントで機能しない NTLM (Active Directory) を使用して接続

ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続が失敗する場合は、接続が正しく構成されていません。 SignalR を使用して、ドメイン環境で、次のように、必要な接続プロパティを設定します。

**接続の資格情報を実装する c# クライアント コード**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>停止したクライアントを検出するためにピンポン IIS websocket を構成します。

SignalR のサーバーには、クライアントが切れているか、そのに依存している基になるための websocket 接続の失敗をからの通知は、OnClose コールバックがわかりません。 この問題を 1 つのソリューションでは、IIS websocket ping/pong な作業を構成します。 これにより、予期せず中断の場合、接続が閉じられます。 詳細については、次を参照してください。[この stackoverflow の投稿](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)します。

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
- **ハブ ルートを追加する前にアプリケーションへのルートの追加:** アプリケーションでは、他のルートを使用する場合、最初のルートが追加の呼び出しの確認`MapSignalR`します。
- **IIS 7 または 7.5、更新プログラムがない拡張子のない Url を使用して:** を使用して IIS 7 または 7.5、更新プログラムが必要拡張子のない Url、サーバーにハブの定義へのアクセスを提供できるように`/signalr/hubs`します。 更新プログラムが見つかります[ここ](https://support.microsoft.com/kb/980368)します。
- **IIS が最新でないキャッシュまたは破損している:** キャッシュの内容が古くないことを確認するには、キャッシュをクリアする PowerShell ウィンドウで次のコマンドを入力します。

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>「500 内部サーバー エラー」

これは、さまざまな原因の可能性がある非常に一般的なエラーです。 エラーの詳細については、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグを確認できます。 サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。 詳細については、次を参照してください。[ハブ クラス内のエラーの処理方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)します。

ファイアウォールまたはプロキシが正しく構成されていない、書き換え要求ヘッダーの原因の場合、このエラーは発生も一般的です。 ソリューションでは、ファイアウォールまたはプロキシのポート 80 が有効であるかどうかを確認します。

### <a name="unexpected-response-code-500"></a>"予期しない応答コード: 500"

このエラーは、アプリケーションで使用される .NET framework のバージョンが Web.Config で指定されたバージョンと一致しない場合に発生する可能性があります。このソリューションでは、.NET 4.5 がアプリケーションの設定と、Web.Config ファイルの両方で使用されていることを確認します。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt;が定義されていません"エラー

このエラーが発生する呼び出し`MapSignalR`が正しく行われていません。 参照してください[SignalR ミドルウェアを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。

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

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>「RuntimeBinderException はユーザー コードで未処理でした」エラー

このエラーが発生する場合の正しくないオーバー ロード`Hub.On`使用されます。 メソッドの戻り値の場合、戻り値の型がジェネリック型パラメーターとして指定する必要があります。

**(生成されたプロキシは) を使用せず、クライアントで定義されたメソッド**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

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

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>プロトコルのフレーム「アクセス許可が拒否されました」を使用して永久に

これは既知の問題で説明されている[ここ](https://github.com/SignalR/SignalR/issues/1963)します。 この現象は、最新 JQuery ライブラリを使用して表示する可能性があります。回避策では、JQuery 1.8.2 にアプリケーションをダウン グレードします。

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: 有効な web ソケット要求ではありません。

このエラーは、WebSocket プロトコルを使用すると、ネットワーク プロキシが要求ヘッダーを変更する場合に発生する可能性があります。 ソリューションでは、ポート 80 で WebSocket を許可するプロキシを構成します。

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"例外:&lt;メソッド名&gt;メソッドを解決できませんでした"クライアントがサーバーでメソッドを呼び出すと

このエラーは、配列などの JSON ペイロードを検出できないデータ型の使用から発生します。 回避策では、IList などの JSON で検出可能なデータ型を使用します。 詳細については、次を参照してください。 [.NET クライアントの配列パラメーターを持つハブ メソッドを呼び出すことができません](https://github.com/SignalR/SignalR/issues/2672)します。

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

場合、このエラーが発生する`MapSignalR`は、アプリケーションによって 2 回呼び出されます。 いくつかの例のアプリケーション呼び出し`MapSignalR`直接スタートアップ クラスで他のユーザー呼び出しを行うラッパー クラスにします。 アプリケーションはないこと両方を確認します。

### <a name="websocket-is-not-used"></a>WebSocket が使用されません。

サーバーとクライアントが WebSocket の要件を満たしていることを確認した場合 (記載、[サポートされているプラットフォーム](../getting-started/supported-platforms.md)ドキュメント)、サーバーで WebSocket を有効にする必要があります。 これを行うための手順を参照して[ここ](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)します。

### <a name="connection-is-undefined"></a>$.connection が定義されていません

このエラーは、ページ上のスクリプトが正常に読み込まれていないまたはハブ プロキシに到達できないか、正しくアクセスしていることを示します。 ページのスクリプト参照がプロジェクトに読み込まれたスクリプトに対応していると、サーバーが実行されているときに、/signalr/hubs をブラウザーでアクセスできることを確認します。

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>動的な式のコンパイルに必要な 1 つまたは複数の種類が見つかりません

このエラーには、ことを示します、`Microsoft.CSharp`ライブラリがありません。 追加することで、**アセンブリ -&gt;Framework**  タブ。

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>Visual Basic または厳密に型指定されたハブ; Clients.Caller から呼び出し元の状態にアクセスできません。「型 'Task (Of Object)' を 'String' を型に変換が無効です」エラー

Visual basic、または厳密に型指定されたハブに呼び出し元の状態にアクセスするには、使用、 `Clients.CallerState` (SignalR 2.1 で導入) の代わりにプロパティ`Clients.Caller`します。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio の問題

このセクションでは、Visual Studio で発生する問題について説明します。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>ソリューション エクスプ ローラーでスクリプト ドキュメントのノードが表示されません。

チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで [スクリプト ドキュメント] ノードに送る。 このノードは、JavaScript デバッガーによって生成され、Internet explorer のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合は、ノードは表示されません。 JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR では、Visual Studio 2008 またはそれ以前は機能しません

この動作は意図されたものです。 SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降に SignalR アプリケーションを開発することが必要です。 SignalR のサーバー コンポーネントでは、.NET Framework 4.5 が必要です。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS の問題

このセクションには、インターネット インフォメーション サービスの問題が含まれています。

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>Visual Studio 開発サーバーで、IIS ではなく、SignalR works

IIS 7.0 および 7.5、SignalR がサポートされていますが、サポート拡張子のない Url を追加する必要があります。 拡張子のない Url のサポートを追加するを参照してください。 [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR では、ASP.NET (ASP.NET がインストールされていない IIS で既定で) サーバーにインストールする必要があります。 ASP.NET をインストールするを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)します。

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Microsoft Azure を発行します。

このセクションには、Microsoft Azure での問題が含まれています。

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException Azure ワーカー ロールで SignalR をホストする場合

例外で、Azure Worker ロールで SignalR をホストしている可能性があります"ファイルまたはアセンブリを読み込むことができません ' Microsoft.Owin、バージョン 2.0.0.0 を ="。 これは、NuGet の既知の問題バインド リダイレクトは、Azure ワーカー ロール プロジェクトでは自動的に追加されません。 これを解決するには、バインド リダイレクトを手動で追加することができます。 次の行を追加、`app.config`のワーカー ロール プロジェクトのファイル。

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>メッセージがトピック名を変更した後は Azure のバック プレーンを介して受信されていません。

Azure のバック プレーンで使用されるトピックが内部的に保持されます。ユーザー構成可能にするものではありません。
