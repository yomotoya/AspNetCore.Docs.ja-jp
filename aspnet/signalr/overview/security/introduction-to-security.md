---
uid: signalr/overview/security/introduction-to-security
title: SignalR セキュリティ入門 |Microsoft Docs
author: pfletcher
description: SignalR アプリケーションを開発する際に考慮する必要があるセキュリティの問題について説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 765abd36c5182f291499042e787bcb4fcc727997
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910857"
---
<a name="introduction-to-signalr-security"></a>SignalR セキュリティ入門
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、SignalR アプリケーションを開発する際に考慮する必要があるセキュリティの問題について説明します。
>
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [SignalR セキュリティの概念](#concepts)

    - [認証と承認](#authentication)
    - [接続トークン](#connectiontoken)
    - [再接続時にグループの再参加](#rejoingroup)
- [SignalR がクロスサイト リクエスト フォージェリを防止する方法](#csrf)
- [SignalR セキュリティに関する推奨事項](#recommendations)

    - [セキュリティで保護されたソケット レイヤー (SSL) プロトコル](#ssl)
    - [セキュリティ メカニズムとしてグループを使用しません](#groupsecurity)
    - [安全にクライアントからの入力の処理](#input)
    - [アクティブな接続のユーザーの状態の変更の調整](#reconcile)
    - [自動的に生成された JavaScript プロキシ ファイル](#autogen)
    - [例外](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR セキュリティの概念

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>認証と承認

SignalR では、ユーザーを認証するための機能は提供されません。 代わりに、アプリケーションの既存の認証の構造に SignalR の機能を統合します。 通常、SignalR での認証の結果での作業、アプリケーションでのコードは、ユーザーを認証します。 たとえば、ASP.NET フォーム認証でユーザー認証を行うし、ハブ内にユーザーを適用する場合があります。 またはメソッドを呼び出す権限がロール。 ハブでユーザー名またはユーザーがクライアントに、ロールに属しているかどうかなどの認証情報を渡すこともできます。

SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーを指定する属性。 Authorize 属性を適用するには、ハブまたはハブのメソッドを特定します。 Authorize 属性がない場合は、ハブのすべてのパブリック メソッドは、ハブに接続されているクライアント使用できます。 ハブの詳細については、次を参照してください。 [SignalR ハブの認証と承認](hub-authorization.md)します。

適用する、`Authorize`属性をハブがいない永続的な接続します。 使用する場合は、承認規則を適用する、`PersistentConnection`オーバーライドする必要があります、`AuthorizeRequest`メソッド。 永続的な接続の詳細については、次を参照してください。 [SignalR 永続的な接続の認証と承認](persistent-connection-authorization.md)します。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>接続トークン

SignalR では、送信者の id を検証することで悪意のあるコマンドを実行するリスクを軽減します。 要求ごとに、クライアントとサーバー接続の id と認証されたユーザーのユーザー名が含まれた接続トークンを渡します。 接続 id は、各接続のクライアントを一意に識別します。 サーバーは、新しい接続が作成され、接続の間にその id が解決しない場合、接続 id をランダムに生成します。 Web アプリケーションの認証メカニズムは、ユーザー名を提供します。 SignalR は、接続トークンを保護するのに暗号化とデジタル署名を使用します。

![](introduction-to-security/_static/image2.png)

要求ごとに、サーバーは、要求が、指定されたユーザーから送信されたことを確認するトークンの内容を検証します。 ユーザー名は、接続 id に対応する必要があります。接続の id とユーザー名の両方を検証するには、ことでは、SignalR は、簡単に別のユーザーを偽装する悪意のあるユーザーを回避します。 サーバーは、接続トークンを検証できません、要求は失敗します。

![](introduction-to-security/_static/image4.png)

接続 id が検証プロセスの一部であるため、他のユーザーに 1 つのユーザーの接続 id を表示または cookie などのクライアントでは、値を格納しない必要があります。

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>再接続時にグループの再参加

既定では、SignalR アプリケーションは自動的に再ユーザー グループに割り当てる適切な接続が削除され、接続がタイムアウトする前に再確立するなどの一時中断から再接続時に。再接続時に、クライアントは接続の id と割り当てられたグループを含むグループ トークンを渡します。 グループのトークンはデジタル署名および暗号化します。 クライアントが保持する; 再接続後も同じ接続 idそのため、再接続のクライアントから渡された接続 id は、クライアントで使用される前の接続 id と一致する必要があります。 この検証では、悪意のあるユーザーが再接続時に承認されていないグループの参加要求を渡すことを防ぎます。

ただしに注意してください、グループのトークンが失効しないことができます。 ユーザーが以前は、グループに属しているかが、そのグループから禁止されていますが、そのユーザーは禁止されているグループを含むグループ トークンを模倣することがあります。 どのグループに属するユーザーを安全に管理する必要がある場合は、データベースのようにサーバーで、そのデータを格納する必要があります。 次に、サーバーで、ユーザーがグループに属しているかどうかを検証するアプリケーション ロジックを追加します。 グループ メンバーシップの確認の例は、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)します。

グループを自動的に再参加と、一時中断の後の接続が再接続されたときにのみ適用します。 によって、ユーザーが切断した場合、アプリケーションまたはアプリケーションが、アプリケーションが再起動を離れると、正しいグループにそのユーザーを追加する方法が処理する必要があります。 詳細については、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)します。

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR がクロスサイト リクエスト フォージェリを防止する方法

クロスサイト リクエスト フォージェリ (CSRF) は、悪意のあるサイトがで、ユーザーが現在ログインしている脆弱性のあるサイトに要求を送信する攻撃です。 SignalR では、非常に SignalR アプリケーションの有効な要求を作成する悪意のあるサイトの可能性は低いので、CSRF ができないようにします。

### <a name="description-of-csrf-attack"></a>CSRF 攻撃の説明

CSRF 攻撃の例を次に示します。

1. www.example.com にユーザーが、フォーム認証の使用します。
2. サーバーは、ユーザーを認証します。 サーバーからの応答には、認証 cookie が含まれています。
3. ログアウトすると、しなくてもユーザー悪意のある web サイトにアクセスします。 この悪意のあるサイトには、次の HTML フォームが含まれています。

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   フォームのアクションが悪意のあるサイトではない、脆弱性のあるサイトに投稿することに注意してください。 これは、CSRF の"cross-site"の一部です。
4. ユーザーは、[送信] ボタンをクリックします。 ブラウザーには、要求の認証クッキーが含まれています。
5. 要求では、ユーザーの認証コンテキストを持つ example.com サーバーで実行され、認証されたユーザーの実行が許可されたものを何でも実行できます。

この例には、ユーザーがフォームのボタンをクリックする必要がありますが、悪意のあるページは、SignalR アプリケーションに AJAX 要求を送信するスクリプトを簡単に実行する場合と同様でした。 さらに、SSL を使用しても、CSRF 攻撃、悪意のあるサイトは、"https://"要求を送信できるので。

通常、CSRF 攻撃は、認証 cookie を使用する web サイトに対して可能なブラウザーすべての関連クッキーを模擬 web サイトに送信するためです。 ただし、CSRF 攻撃では、cookie の利用に制限はありません。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 基本認証またはダイジェスト認証を使用してユーザーのログオン、ブラウザーは、セッションが終了するまで自動的に資格情報を送信します。

### <a name="csrf-mitigations-taken-by-signalr"></a>SignalR 要した CSRF 対策

SignalR は、悪意のあるサイトが、アプリケーションに有効な要求を作成するを防ぐために、次の手順を受け取ります。 SignalR は、次の手順を既定では、コードで何もする必要はありません。

- **クロス ドメイン要求を無効にする**SignalR は、ユーザーが、外部ドメインから SignalR エンドポイントを呼び出すことを防止するクロス ドメイン要求を無効にします。 SignalR では、無効である、外部ドメインのすべての要求を考慮し、要求をブロックします。 この既定の動作を維持することをお勧めします。それ以外の場合、悪意のあるサイトでは、サイトにコマンドを送信するのにユーザーをだましてでした。 クロス ドメイン要求を使用する必要がある場合は、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。
- **クエリ文字列、cookie ではない接続トークンを渡す**SignalR は、の代わりにクエリ文字列の値として接続トークンを cookie として渡します。 悪意のあるコードが発生した場合に、ブラウザーは接続トークンを転送誤ってできるため、接続トークンを cookie に格納するセーフはありません。 また、クエリ文字列で接続トークンを渡すの現在の接続を超える永続化接続トークンが防止します。 そのため、悪意のあるユーザーは、別のユーザーの認証の資格情報で要求を作成することはできません。
- **接続トークンを確認します。** 」の説明に従って、[接続トークン](#connectiontoken) セクションで、サーバーでは、どの接続 id が各認証済みユーザーに関連付けられたがわかっています。 サーバーは、ユーザー名と一致しない接続 id からの要求を処理しません。 悪意のあるユーザーは、ユーザー名と、現在の接続をランダムに生成された id を知っている必要があるために、悪意のあるユーザーは有効な要求を推測でした可能性が高いことはできません。接続が終了するとすぐに、その接続 id が無効になります。 匿名ユーザーには、機密情報へのアクセスはありません。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR セキュリティに関する推奨事項

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>セキュリティで保護されたソケット レイヤー (SSL) プロトコル

SSL プロトコルは、クライアントとサーバー間のデータの転送をセキュリティで保護するのに暗号化を使用します。 SignalR アプリケーションがクライアントとサーバー間で機密情報を送信する場合は、トランスポートの SSL を使用します。 SSL の設定の詳細については、次を参照してください。 [IIS 7 で SSL を設定する方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)します。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>セキュリティ メカニズムとしてグループを使用しません

グループは、関連するユーザーを収集する場合の便利な方法が、機密情報へのアクセスを制限するためのセキュリティで保護されたメカニズムではありません。 これは、ユーザーことができます、再接続中にグループを自動的に再参加するときに特に当てはまります。 代わりに、特権のあるユーザーをロールに追加して、そのロールのメンバーのみにハブ メソッドへのアクセスを制限することを検討してください。 役割に基づくアクセス制限の例は、次を参照してください。 [SignalR ハブの認証と承認](hub-authorization.md)します。 再接続時にユーザー グループへのアクセスのチェックの例は、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)します。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全にクライアントからの入力の処理

悪意のあるユーザーが他のユーザーにスクリプトを送信しないことを確認するには、他のクライアントへのブロードキャストが想定されているクライアントからのすべての入力をエンコードする必要があります。 SignalR アプリケーションのさまざまな種類のクライアントの可能性があるため、サーバーではなく、受信側のクライアントにメッセージをエンコードする必要があります。 そのため、HTML エンコードと、web クライアントの場合、その他の種類のクライアントではなくは機能します。 たとえば、チャット メッセージを表示する web クライアントのメソッドは安全に処理、ユーザー名とメッセージ呼び出すことによって、`html()`関数。

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>アクティブな接続のユーザーの状態の変更の調整

アクティブな接続が存在する場合、ユーザーの認証の状態が変更された場合、ユーザーを示すエラーが表示されます、「ユーザー id をアクティブな SignalR 接続中に変更できません」 その場合は、アプリケーションでは、接続 id とユーザー名が調整されたことを確認するサーバーに接続する必要がありますし直します。 たとえば、アプリケーションでは、アクティブな接続が存在する場合、ログアウトするユーザーを許可している場合、最後に次の要求で渡される名前が、接続のユーザー名と一致しなくなります。 ユーザーがログアウトすると、前に、接続を停止してから再起動されます。

ただしは、ほとんどのアプリケーションが、手動で停止し、接続を開始する必要がないことに注意してください。 アクティブな接続が自動的に切断していないアプリケーション、Web フォーム アプリケーションまたは MVC アプリケーションの既定の動作など、ログインした後、別のページにユーザーをリダイレクトまたはログアウトした後は、現在のページを更新、追加アクションが必要です。

次の例では、停止し、ユーザーの状態が変更されたときに、接続を開始する方法を示します。

[!code-html[Main](introduction-to-security/samples/sample3.html)]

または、サイトでは、スライド式有効期限を使用して、フォーム認証で有効な認証クッキーを保持するアクティビティがない場合、ユーザーの認証の状態を変更することがあります。 その場合は、ユーザーがログアウトして、ユーザー名は、接続トークン内のユーザー名と一致しなくなります。 この問題を修正するには、定期的に有効な認証クッキーを保持する web サーバー上のリソースを要求するいくつかのスクリプトを追加します。 次の例では、30 分ごとにリソースを要求する方法を示します。

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>自動的に生成された JavaScript プロキシ ファイル

ユーザーごとの JavaScript プロキシ ファイルにすべてのハブおよびメソッドを含めるしたくない場合は、ファイルの自動生成を無効にできます。 複数のハブおよびメソッドがあるすべてのメソッドの対応するすべてのユーザーを作成したくない場合は、このオプションを選択する可能性があります。 自動生成を無効に設定して**EnableJavaScriptProxies**に**false**します。

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

JavaScript プロキシ ファイルの詳細については、次を参照してください。 [、生成されたプロキシとは何が](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)します。 <a id="exceptions"></a>

### <a name="exceptions"></a>例外

オブジェクトは機密情報をクライアントに公開される可能性があるために、クライアントに例外オブジェクトを渡すことは避ける必要があります。 代わりに、関連するエラー メッセージを表示するクライアントでメソッドを呼び出します。

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
