---
uid: signalr/overview/older-versions/introduction-to-security
title: SignalR セキュリティ入門 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: SignalR アプリケーションを開発する際に考慮する必要があるセキュリティの問題について説明します。
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: cb705ccb6052297d0214deeaaeb8181e283245f3
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228560"
---
<a name="introduction-to-signalr-security-signalr-1x"></a>SignalR セキュリティ入門 (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、SignalR アプリケーションを開発する際に考慮する必要があるセキュリティの問題について説明します。


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

アプリケーションの既存の認証の構造に統合するのには、SignalR は設計されています。 ユーザーを認証するための機能は行いません。 代わりに、アプリケーションでは、通常どおり、SignalR のコードで、認証の結果を使用すると、ユーザーを認証します。 たとえば、ASP.NET フォーム認証でユーザー認証を行うし、ハブ内にユーザーを適用する場合があります。 またはメソッドを呼び出す権限がロール。 ハブでユーザー名またはユーザーがクライアントに、ロールに属しているかどうかなどの認証情報を渡すこともできます。

SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーを指定する属性。 Authorize 属性を適用するには、ハブまたはハブのメソッドを特定します。 Authorize 属性がない場合は、ハブのすべてのパブリック メソッドは、ハブに接続されているクライアント使用できます。 ハブの詳細については、次を参照してください。 [SignalR ハブの認証と承認](../security/hub-authorization.md)します。

`Authorize`属性は、ハブでのみ使用されます。 使用する場合は、承認規則を適用する、`PersistentConnection`オーバーライドする必要があります、`AuthorizeRequest`メソッド。 永続的な接続の詳細については、次を参照してください。 [SignalR 永続的な接続の認証と承認](../security/persistent-connection-authorization.md)します。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>接続トークン

SignalR では、送信者の id を検証することで悪意のあるコマンドを実行するリスクを軽減します。 接続の id と認証されたユーザーのユーザー名を含む、接続トークンは、要求ごとに、クライアントとサーバー間で渡されます。 接続 id は、新しい接続が作成され、接続の期間が永続化されるときに、サーバーによってランダムに生成される一意の識別子です。 ユーザー名は、web アプリケーションの認証メカニズムによって提供されます。 接続トークンは、暗号化とデジタル署名で保護されます。

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

1. ユーザーがログイン`www.example.com`、フォーム認証を使用します。
2. サーバーは、ユーザーを認証します。 サーバーからの応答には、認証 cookie が含まれています。
3. ログアウトすると、しなくてもユーザー悪意のある web サイトにアクセスします。 この悪意のあるサイトには、次の HTML フォームが含まれています。 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   フォームのアクションが悪意のあるサイトではない、脆弱性のあるサイトに投稿することに注意してください。 これは、CSRF の"cross-site"の一部です。
4. ユーザーは、[送信] ボタンをクリックします。 ブラウザーには、要求の認証クッキーが含まれています。
5. 要求では、ユーザーの認証コンテキストを持つ example.com サーバーで実行され、認証されたユーザーの実行が許可されたものを何でも実行できます。

この例には、ユーザーがフォームのボタンをクリックする必要がありますが、悪意のあるページは、SignalR アプリケーションに AJAX 要求を送信するスクリプトを簡単に実行する場合と同様でした。 さらに、SSL を使用しても、CSRF 攻撃、悪意のあるサイトは、"https://"要求を送信できるので。

通常、CSRF 攻撃は、認証 cookie を使用する web サイトに対して可能なブラウザーすべての関連クッキーを模擬 web サイトに送信するためです。 ただし、CSRF 攻撃では、cookie の利用に制限はありません。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 基本認証またはダイジェスト認証を使用してユーザーのログオン、ブラウザーは、セッションが終了するまで自動的に資格情報を送信します。

### <a name="csrf-mitigations-taken-by-signalr"></a>SignalR 要した CSRF 対策

SignalR は、悪意のあるサイトが SignalR アプリケーションに有効な要求を作成するを防ぐために、次の手順を受け取ります。 これらの手順では、既定によって実行され、コード内のアクションを必要としません。

- **クロス ドメイン要求を無効にします。**  
 既定では、クロス ドメイン要求が無効になっているで SignalR アプリケーションをユーザーが、外部ドメインから SignalR エンドポイントを呼び出すことを防ぐためにします。 外部ドメインに由来するすべての要求が自動的に無効とみなされはブロックされます。 この既定の動作を維持することをお勧めそれ以外の場合、悪意のあるサイトでは、サイトにコマンドを送信するのにユーザーをだましてでした。 クロス ドメイン要求を使用する必要がある場合は、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。
- **クエリ文字列、cookie ではない接続トークンを渡す**  
 SignalR は、代わりにクエリ文字列の値として接続トークンを cookie として渡します。 接続トークンを cookie として保存しないで、接続トークンが誤っては転送されません、ブラウザーによって悪意のあるコードが検出されたとき。 また、接続トークンは、現在の接続以外は保持されません。 そのため、悪意のあるユーザーは、別のユーザーの認証の資格情報で要求を作成することはできません。
- **接続トークンを確認します。**  
 」の説明に従って、[接続トークン](#connectiontoken) セクションで、サーバーでは、どの接続 id が各認証済みユーザーに関連付けられたがわかっています。 サーバーは、ユーザー名と一致しない接続 id からの要求を処理しません。 悪意のあるユーザーは、ユーザー名と、現在の接続をランダムに生成された id を知っている必要があるために、悪意のあるユーザーは有効な要求を推測でした可能性が高いことはできません。接続が終了するとすぐに、その接続 id が無効になります。 匿名ユーザーには、機密情報へのアクセスはありません。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR セキュリティに関する推奨事項

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>セキュリティで保護されたソケット レイヤー (SSL) プロトコル

SSL プロトコルは、クライアントとサーバー間のデータの転送をセキュリティで保護するのに暗号化を使用します。 SignalR アプリケーションがクライアントとサーバー間で機密情報を送信する場合は、トランスポートの SSL を使用します。 SSL の設定の詳細については、次を参照してください。 [IIS 7 で SSL を設定する方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)します。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>セキュリティ メカニズムとしてグループを使用しません

グループは、関連するユーザーを収集する場合の便利な方法が、機密情報へのアクセスを制限するためのセキュリティで保護されたメカニズムではありません。 これは、ユーザーことができます、再接続中にグループを自動的に再参加するときに特に当てはまります。 代わりに、特権のあるユーザーをロールに追加して、そのロールのメンバーのみにハブ メソッドへのアクセスを制限することを検討してください。 役割に基づくアクセス制限の例は、次を参照してください。 [SignalR ハブの認証と承認](../security/hub-authorization.md)します。 再接続時にユーザー グループへのアクセスのチェックの例は、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)します。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全にクライアントからの入力の処理

悪意のあるユーザーが他のユーザーにスクリプトを送信しないことを確認する他のクライアントへのブロードキャストが想定されているクライアントからのすべての入力をエンコードする必要があります。 お勧め、サーバーではなく、受信側のクライアントにメッセージをエンコードする SignalR アプリケーションのさまざまな種類のクライアントの可能性があるためです。 そのため、HTML エンコードと、web クライアントの場合、その他の種類のクライアントではなくは機能します。 たとえば、チャット メッセージを表示する web クライアントのメソッドは安全に処理、ユーザー名とメッセージ呼び出すことによって、`html()`関数。

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
