---
uid: signalr/overview/security/introduction-to-security
title: "SignalR のセキュリティの概要 |Microsoft ドキュメント"
author: pfletcher
description: "SignalR アプリケーションを開発する場合に考慮する必要があります、セキュリティの問題について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 1cb9f15a958028822b50decf4b420c36596ce25e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-signalr-security"></a>SignalR のセキュリティの概要
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、SignalR アプリケーションを開発する場合に考慮する必要があります、セキュリティの問題について説明します。 
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

SignalR では、ユーザーを認証するための機能は提供されません。 代わりに、SignalR の機能を統合するには、アプリケーションの既存の認証構造にします。 ユーザーを認証するは、通常、アプリケーション、および、SignalR での認証の結果を含む作業でコーディングするとします。 たとえば、ASP.NET フォーム認証でユーザー認証を行うし、ハブでユーザーを適用する可能性があります。 またはロールがメソッドを呼び出す権限がします。 ハブでは、ユーザー名またはユーザーがクライアントに、ロールに属しているかどうかなどの認証情報を渡すこともします。

SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーを指定する属性。 承認属性を適用するには、ハブまたは特定のハブ メソッドのいずれかにします。 Authorize 属性がない、ハブのすべてのパブリック メソッドがハブに接続されているクライアントで使用します。 ハブの詳細については、次を参照してください。 [SignalR ハブの認証と承認](hub-authorization.md)です。

適用する、`Authorize`属性をハブがいない永続的な接続します。 使用する場合は、承認規則を適用する、`PersistentConnection`オーバーライドする必要があります、`AuthorizeRequest`メソッドです。 永続的な接続の詳細については、次を参照してください。 [SignalR 固定接続の認証と承認](persistent-connection-authorization.md)です。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>接続トークン

SignalR では、送信者の id を検証することによって悪意のあるコマンドを実行するリスクを軽減します。 要求ごとに、クライアントとサーバーの接続 id と認証済みユーザーのユーザー名を含む接続トークンを渡します。 接続 id は、接続されている各クライアントを一意に識別します。 サーバーは、新しい接続が作成され、接続の間にその id が引き続き発生するときに、接続 id をランダムに生成します。 Web アプリケーションの認証メカニズムでは、ユーザー名を提供します。 SignalR では、接続トークンを保護するのに暗号化およびデジタル署名を使用します。

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

SignalR では、悪意のあるサイトが、アプリケーションに対する有効な要求を作成できないようにするのには、次の手順を受け取ります。 SignalR は既定では、次の手順を受け取り、コードで処理する必要はありません。

- **クロス ドメイン要求を無効にします。**  
 SignalR には、ユーザーが、外部ドメインの SignalR エンドポイントを呼び出すことを防止するクロス ドメイン要求が無効にします。 SignalR では、無効になる、外部ドメインのすべての要求を考慮し、要求をブロックします。 この既定の動作を保持することをお勧めします。それ以外の場合、悪意のあるサイトがユーザーを騙して、サイトにコマンドを送信します。 クロス ドメイン要求を使用する必要がある場合は、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。
- **クエリ文字列、cookie ではない接続トークンを渡します**  
 SignalR では、クッキーとしての代わりにクエリ文字列の値として接続トークンを渡します。 悪意のあるコードが発生したときに、ブラウザーは接続トークンを転送誤ってできるため、cookie の接続トークンを格納する安全ではありません。 また、クエリ文字列内の接続トークンを渡すことにより、接続トークンの現在の接続を超える永続化します。 そのため、悪意のあるユーザーは、別のユーザーの認証の資格情報で要求を作成することはできません。
- **接続トークンを確認してください。**  
 」の説明に従って、[接続トークン](#connectiontoken) セクションで、サーバー接続 id が認証されたユーザーごとに関連付けられたことを認識します。 サーバーは、ユーザー名と一致しない接続の id からのすべての要求を処理しません。 悪意のあるユーザーは、ユーザー名と、現在の接続をランダムに生成された id を知っている必要があるために、悪意のあるユーザーは有効な要求を推測できなかった可能性が高いことはできません。接続が終了するとすぐに、その接続 id が無効になります。 匿名ユーザーには、機密情報へのアクセスをことはできません。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR のセキュリティに関する推奨事項

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>セキュア ソケット レイヤー (SSL) プロトコル

SSL プロトコルは、クライアントとサーバー間のデータの転送をセキュリティで保護するのに暗号化を使用します。 SignalR アプリケーションは、クライアントとサーバー間で機密情報を送信する場合は、トランスポートの SSL を使用します。 SSL の設定の詳細については、次を参照してください。 [IIS 7 で SSL を設定する方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)です。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>グループは、セキュリティ メカニズムとして使用しないでください。

グループは、関連するユーザーは、収集する場合の便利な方法が、機密情報へのアクセスを制限するための安全なメカニズムはありません。 これは、ユーザーことができます、再接続時にグループを自動的に再度参加させますときに特に当てはまります。 代わりに、ロールに権限を持つユーザーを追加して、そのロールのメンバーのみにハブ メソッドへのアクセスを制限することを検討します。 役割に基づくアクセス制限の例は、次を参照してください。 [SignalR ハブの認証と承認](hub-authorization.md)です。 再接続するときに、ユーザー グループへのアクセスをチェックの例は、次を参照してください。[グループの操作](../guide-to-the-api/working-with-groups.md)です。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全にクライアントからの入力の処理

悪意のあるユーザーは他のユーザーにスクリプトを送信しないことを確認してください、他のクライアントにブロードキャストのためのものでは、クライアントからのすべての入力をエンコードする必要があります。 SignalR アプリケーションのさまざまな種類のクライアントの可能性があるため、サーバーではなく、受信側のクライアントにメッセージをエンコードする必要があります。 したがって、HTML エンコードと、web クライアントの場合はその他の種類のクライアントは動作します。 たとえば、チャット メッセージを表示する web クライアント メソッドが安全に処理、ユーザー名とメッセージを呼び出して、`html()`関数。

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
