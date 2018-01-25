---
uid: signalr/overview/older-versions/introduction-to-security
title: "SignalR のセキュリティの概要 (SignalR 1.x) |Microsoft ドキュメント"
author: pfletcher
description: "SignalR アプリケーションを開発する場合に考慮する必要があります、セキュリティの問題について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: ebc83098b73902fa3f7a90a38dafc43b413e75fe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-signalr-security-signalr-1x"></a>SignalR のセキュリティの概要 (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、SignalR アプリケーションを開発する場合に考慮する必要があります、セキュリティの問題について説明します。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [SignalR のセキュリティの概念](#concepts)

    - [認証と承認](#authentication)
    - [接続トークン](#connectiontoken)
    - [再接続するときにグループの再参加](#rejoingroup)
- [SignalR でクロスサイト リクエスト フォージェリを防止する方法](#csrf)
- [SignalR のセキュリティに関する推奨事項](#recommendations)

    - [セキュア ソケット レイヤー (SSL) プロトコル](#ssl)
    - [グループは、セキュリティ メカニズムとして使用しないでください。](#groupsecurity)
    - [安全にクライアントからの入力の処理](#input)
    - [アクティブな接続のユーザーの状態の変更を調整](#reconcile)
    - [自動的に生成された JavaScript プロキシ ファイル](#autogen)
    - [例外](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR のセキュリティの概念

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>認証と承認

SignalR は、アプリケーションの既存の認証構造に統合できるように設計されています。 ユーザーの認証にも任意の機能は提供されません。 代わりに、ユーザーを認証する、アプリケーションで通常どおりし、SignalR コードで、認証の結果を処理できます。 たとえば、ASP.NET フォーム認証でユーザー認証を行うし、ハブでユーザーを適用する可能性があります。 またはロールがメソッドを呼び出す権限がします。 ハブでは、ユーザー名またはユーザーがクライアントに、ロールに属しているかどうかなどの認証情報を渡すこともします。

SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーを指定する属性。 承認属性を適用するには、ハブまたは特定のハブ メソッドのいずれかにします。 Authorize 属性がない、ハブのすべてのパブリック メソッドがハブに接続されているクライアントで使用します。 ハブの詳細については、次を参照してください。 [SignalR ハブの認証と承認](../security/hub-authorization.md)です。

`Authorize`属性は、ハブでのみ使用します。 使用する場合は、承認規則を適用する、`PersistentConnection`オーバーライドする必要があります、`AuthorizeRequest`メソッドです。 永続的な接続の詳細については、次を参照してください。 [SignalR 固定接続の認証と承認](../security/persistent-connection-authorization.md)です。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>接続トークン

SignalR では、送信者の id を検証することによって悪意のあるコマンドを実行するリスクを軽減します。 接続 id と認証されたユーザーのユーザー名を含む、接続トークンは、要求ごとに、クライアントとサーバー間で渡されます。 接続 id は、新しい接続が作成され、接続の期間が永続化されるときに、サーバーによってランダムに生成される一意の識別子です。 ユーザー名は、web アプリケーションの認証メカニズムによって提供されます。 接続トークンは、暗号化とデジタル署名で保護されています。

![](introduction-to-security/_static/image2.png)

各要求に対しては、サーバーは、要求が、指定されたユーザーから送信されたことを確認するトークンの内容を検証します。 ユーザー名は、接続 id に対応する必要があります。接続 id とユーザー名の両方を検証するでは、SignalR は、悪意のあるユーザーを簡単に別のユーザーになりすますが回避されます。 サーバーは、接続トークンを検証できません、要求は失敗します。

![](introduction-to-security/_static/image4.png)

接続 id は、検証プロセスの一部であり、他のユーザーに 1 つのユーザーの接続 id を表示またはなど、クライアント上の値を cookie に保存されません必要があります。

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>再接続するときにグループの再参加

既定では、SignalR アプリケーションが自動的に再割り当て、ユーザー、適切なグループをなど、接続が削除され、再び確立したが、接続がタイムアウトする前に、一時的に中断から再接続するときにします。再接続時に、クライアントは接続の id と、割り当てられているグループを含むグループ トークンを渡します。 グループのトークンは、デジタル署名および暗号化します。 クライアントは、接続と同じ id を; 再接続した後に保持されます。そのため、再接続したクライアントから渡される接続 id クライアントによって使用される以前接続 id と一致する必要があります。 この検証では、悪意のあるユーザーが再接続するときに権限のないグループに参加する要求を渡すことを防ぎます。

ただしに注意してください、あるグループ トークン有効期限はありませんが。 ユーザーが以前は、グループに属していたが、そのグループから禁止されている場合は、可能性がありますことができるユーザーである、禁止されているグループを含むグループ トークンを模倣するためにします。 どのグループに属するユーザーを安全に管理する必要がある場合など、データベースにサーバーで、そのデータを格納する必要があります。 次に、サーバーでユーザーがグループに属しているかどうかを検証するアプリケーションにロジックを追加します。 グループ メンバーシップの確認の例は、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)です。

グループを自動的に再参加のみ適用されますが一時的に中断した後、接続が再接続されたとき。 によって、ユーザーが切断した場合、アプリケーションまたはアプリケーションが再起動をアプリケーションから移動と適切なグループにそのユーザーを追加する方法が処理する必要があります。 詳細については、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)です。

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR でクロスサイト リクエスト フォージェリを防止する方法

クロスサイト リクエスト フォージェリ (CSRF) は、攻撃が悪意のあるサイトは、ユーザーが現在記録される場所で脆弱なサイトに要求を送信します。 SignalR では、SignalR アプリケーションの有効な要求を作成する悪意のあるサイトの非常に珍しいことで CSRF を回避します。

### <a name="description-of-csrf-attack"></a>CSRF 攻撃の説明

CSRF 攻撃の例を次に示します。

1. Www.example.com にユーザーが、フォーム認証の使用します。
2. サーバーは、ユーザーを認証します。 サーバーからの応答には、認証 cookie が含まれています。
3. ログアウト、しなくてもユーザー悪意のある web サイトにアクセスします。 この悪意のあるサイトには、次の HTML フォームが含まれています。 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 フォームのアクションが悪意のあるサイトではない、脆弱なサイトにポストすることに注意してください。 これは、CSRF の「クロスサイト」の一部です。
4. ユーザーは、[送信] ボタンをクリックします。 ブラウザーには、要求と共に認証 cookie が含まれています。
5. 要求は、ユーザーの認証コンテキストで example.com サーバーで実行されを行うには、認証されたユーザーが許可されている操作を実行します。

この例には、ユーザーがフォームのボタンをクリックする必要がありますが、悪意のあるページは、SignalR アプリケーションに AJAX 要求を送信するスクリプトを簡単に実行でした。 さらに、SSL を使用して有効になって、CSRF 攻撃では、悪意のあるサイトは、"https://"要求を送信できるためです。

通常、CSRF 攻撃は、ブラウザーが web サイトに関連するすべての cookie を送信するため、認証の cookie を使用する web サイトに対して可能です。 ただし、CSRF 攻撃では、cookie の悪用に制限はありません。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 ユーザーは、基本認証またはダイジェスト認証を使用してログイン、ブラウザーは、セッションが終了するまで自動的に資格情報を送信します。

### <a name="csrf-mitigations-taken-by-signalr"></a>SignalR によって使用された CSRF 緩和策

SignalR では、悪意のあるサイトが、SignalR アプリケーションに対する有効な要求を作成できないようにするのには、次の手順を受け取ります。 これらの手順では、既定で実行され、コードのどのアクションは不要です。

- **クロス ドメイン要求を無効にします。**  
 既定では、クロス ドメイン要求は無効になっているユーザーが、外部ドメインの SignalR エンドポイントを呼び出すことを防止する SignalR アプリケーションでします。 外部のドメインから取得したすべての要求は自動的に無効とみなされがブロックされています。 この既定の動作を保持することをお勧めそれ以外の場合、悪意のあるサイトがユーザーを騙して、サイトにコマンドを送信します。 クロス ドメイン要求を使用する必要がある場合は、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。
- **クエリ文字列、cookie ではない接続トークンを渡します**  
 SignalR では、クッキーとしての代わりにクエリ文字列の値として接続トークンを渡します。 Cookie との接続トークンを保存しないで、接続トークンが誤って転送されませんブラウザーによって悪意のあるコードが発生した場合。 また、接続トークンは、現在の接続を超えるは保持されません。 そのため、悪意のあるユーザーは、別のユーザーの認証の資格情報で要求を作成することはできません。
- **接続トークンを確認してください。**  
 」の説明に従って、[接続トークン](#connectiontoken) セクションで、サーバー接続 id が認証されたユーザーごとに関連付けられたことを認識します。 サーバーは、ユーザー名と一致しない接続の id からのすべての要求を処理しません。 悪意のあるユーザーは、ユーザー名と、現在の接続をランダムに生成された id を知っている必要があるために、悪意のあるユーザーは有効な要求を推測できなかった可能性が高いことはできません。接続が終了するとすぐに、その接続 id が無効になります。 匿名ユーザーには、機密情報へのアクセスをことはできません。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR のセキュリティに関する推奨事項

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>セキュア ソケット レイヤー (SSL) プロトコル

SSL プロトコルは、クライアントとサーバー間のデータの転送をセキュリティで保護するのに暗号化を使用します。 SignalR アプリケーションは、クライアントとサーバー間で機密情報を送信する場合は、トランスポートの SSL を使用します。 SSL の設定の詳細については、次を参照してください。 [IIS 7 で SSL を設定する方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)です。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>グループは、セキュリティ メカニズムとして使用しないでください。

グループは、関連するユーザーは、収集する場合の便利な方法が、機密情報へのアクセスを制限するための安全なメカニズムはありません。 これは、ユーザーことができます、再接続時にグループを自動的に再度参加させますときに特に当てはまります。 代わりに、ロールに権限を持つユーザーを追加して、そのロールのメンバーのみにハブ メソッドへのアクセスを制限することを検討します。 役割に基づくアクセス制限の例は、次を参照してください。 [SignalR ハブの認証と承認](../security/hub-authorization.md)です。 再接続するときに、ユーザー グループへのアクセスをチェックの例は、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)です。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全にクライアントからの入力の処理

他のクライアントにブロードキャストのためのものでは、クライアントからのすべての入力は、悪意のあるユーザーは他のユーザーにスクリプトを送信しないことを確認してくださいにエンコードする必要があります。 お勧め、サーバーではなく、受信側のクライアントにメッセージをエンコードする SignalR アプリケーションのさまざまな種類のクライアントの可能性があるためです。 したがって、HTML エンコードと、web クライアントの場合はその他の種類のクライアントは動作します。 たとえば、チャット メッセージを表示する web クライアント メソッドが安全に処理、ユーザー名とメッセージを呼び出して、`html()`関数。

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>アクティブな接続のユーザーの状態の変更を調整

アクティブな接続が存在する間、ユーザーの認証の状態が変更された場合、ユーザーを示すエラーが表示されます、「ユーザー id をアクティブな SignalR 接続中に変更できません」 その場合は、アプリケーションでは、接続 id とユーザー名が調整されたことを確認するサーバーに接続する必要がありますし直します。 など、アプリケーションでは、アクティブな接続が存在する間、ログアウトするユーザーを許可している場合、最後に、次の要求で渡される名前が、接続のユーザー名と一致しなくなります。 ユーザーがログアウトする前に、接続を停止し、再開されます。

ただし、ほとんどのアプリケーションが、手動で停止し、接続を開始する必要がないことに注意する必要です。 アクティブな接続が自動的に切断していないアプリ、Web フォーム アプリケーションまたは MVC アプリケーションの既定の動作など、ログインした後、ユーザーを別のページにリダイレクトまたはログアウト後、現在のページを更新、追加アクションが必要です。

次の例では、停止し、ユーザーの状態が変更されたときに、接続を開始する方法を示します。

[!code-html[Main](introduction-to-security/samples/sample3.html)]

または、サイトでは、スライド式有効期限を使用して、フォーム認証を使用し、有効な認証クッキーを保持するアクティビティがない場合、ユーザーの認証の状態を変更することがあります。 その場合は、ユーザーがログアウトして、ユーザー名は、接続トークン内のユーザー名と一致しなくなります。 この問題を修正するには、定期的に有効な認証クッキーを保持する web サーバー上のリソースを要求するいくつかのスクリプトを追加します。 次の例では、30 分ごとにリソースを要求する方法を示します。

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>自動的に生成された JavaScript プロキシ ファイル

ユーザーごとの JavaScript プロキシ ファイルに含めるすべてのハブおよびメソッドのたくない場合は、ファイルの自動生成を無効にすることができます。 複数のハブおよびメソッドをしたが、すべてのユーザーのすべてのメソッドに注意する必要がない場合は、このオプションを選択する場合があります。 自動生成を無効に設定して**EnableJavaScriptProxies**に**false**です。

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

JavaScript プロキシ ファイルの詳細については、次を参照してください。 [、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)です。 <a id="exceptions"></a>

### <a name="exceptions"></a>例外

オブジェクトが機密情報をクライアントに公開されるため、例外オブジェクトをクライアントに渡すを避ける必要があります。 代わりに、関連するエラー メッセージを表示するクライアントでメソッドを呼び出します。

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
