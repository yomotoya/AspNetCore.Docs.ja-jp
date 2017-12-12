---
uid: web-api/overview/security/authentication-filters
title: "ASP.NET Web API 2 の認証フィルター |Microsoft ドキュメント"
author: MikeWasson
description: "認証フィルターは、HTTP 要求を認証するコンポーネントです。 認証フィルターをサポートして両方 web API 2 と MVC 5 が若干異なるしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: eee4e7accd338262698d127ed08d4182608839ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 の認証フィルター
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> 認証フィルターは、HTTP 要求を認証するコンポーネントです。 認証フィルターをサポート両方 web API 2 と MVC 5 がフィルター インターフェイスの名前付け規則でほとんどの場合、少しが異なります。 このトピックでは、Web API 認証フィルターについて説明します。


認証フィルターを使用して、個々 のコント ローラーまたはアクションの認証方式を設定できます。 このように、アプリは、さまざまな HTTP リソースに対して異なる認証メカニズムをサポートできます。

この記事で紹介からコードを[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)のサンプル[http://aspnet.codeplex.com](http://aspnet.codeplex.com)です。このサンプルでは、HTTP 基本アクセス認証方式 (RFC 2617) を実装する認証フィルターを示します。 フィルターがという名前のクラスで実装されている`IdentityBasicAuthenticationAttribute`です。 私は表示されませんのすべてのサンプル コード認証フィルターを作成する方法を説明する部分だけです。

## <a name="setting-an-authentication-filter"></a>認証フィルターを設定

他のフィルターと同様には、認証フィルターは適用されている各コント ローラー、アクション、またはをグローバルにすべての Web API コント ローラーを指定できます。

コント ローラーに認証フィルターを適用するには、フィルター属性でコント ローラー クラスを装飾します。 次のコード セット、`[IdentityBasicAuthentication]`コント ローラーのアクションのすべての基本認証を有効にするコント ローラー クラスでフィルターします。

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

1 つのアクションにフィルターを適用するには、フィルターでアクションを装飾します。 次のコード セット、`[IdentityBasicAuthentication]`コント ローラーのフィルター`Post`メソッドです。

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

フィルターを適用する、すべての Web API コント ローラーに、追加する**GlobalConfiguration.Filters**です。

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Web API 認証フィルターを実装します。

Web API では認証フィルターを実装して、 [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/en-us/library/system.web.http.filters.iauthenticationfilter.aspx)インターフェイスです。 継承する必要がありますも**System.Attribute**、属性として適用するためにします。

**IAuthenticationFilter**インターフェイスが 2 つのメソッドには。

- **AuthenticateAsync**存在する場合、要求内の資格情報を検証することによって、要求を認証します。
- **ChallengeAsync**必要な場合は、HTTP 応答に認証チャレンジを追加します。

これらのメソッドがで定義されている認証フローに対応して[RFC 2612](http://tools.ietf.org/html/rfc2616)と[RFC 2617](http://tools.ietf.org/html/rfc2617):

1. クライアントは、承認ヘッダーに資格情報を送信します。 これは通常、クライアントがサーバーから 401 (未承認) 応答を受信した後に発生します。 ただし、クライアントは、401 を取得した後だけでなく、要求に資格情報を送信できます。
2. サーバーが資格情報を受け付けない場合は、401 (未承認) 応答を返します。 応答には、1 つまたは複数の課題を表す Www-authenticate ヘッダーが含まれます。 各チャレンジは、サーバーによって認識される認証スキームを指定します。

サーバーでは、匿名の要求から 401 を返すこともできます。 実際には、通常、これは、認証プロセスを開始する方法です。

1. クライアントは、匿名の要求を送信します。
2. サーバーでは、401 を返します。
3. クライアントは、資格情報を持つ要求を再送信します。

このフローには、両方が含まれます*認証*と*承認*手順を実行します。

- 認証は、クライアントの身元を証明します。
- 承認は、クライアントが特定のリソースにアクセスできるかどうかを判断します。

Web API では、認証フィルターは、認証が承認されませんを処理します。 承認フィルターまたはコント ローラー アクションの内部は、承認を行う必要があります。

Web API 2 のパイプラインの流れを次に示します。

1. アクションを呼び出す前に、Web API は、その操作で使用する認証フィルターの一覧を作成します。 これには、アクション スコープ、コント ローラーのスコープ、およびグローバル スコープでフィルターが含まれます。
2. Web API 呼び出し**AuthenticateAsync**リスト内のすべてのフィルターにします。 各フィルターは、要求に資格情報を検証できます。 フィルターを作成している場合は任意のフィルターは、資格情報を正常に検証、 **IPrincipal**を要求にアタッチします。 フィルターは、エラーをこの時点でトリガーすることもできます。 その場合、パイプラインの残りの部分は実行されません。
3. 要求は、エラーがないと仮定した場合、残りのパイプラインを経由して流れます。
4. Web API が最後に、すべて認証フィルターを呼び出して**ChallengeAsync**メソッドです。 フィルターは、必要な場合は、応答にチャレンジを追加するこのメソッドを使用します。 通常 (必ずではありませんが) ですが、発生する可能性、401 エラーに応答します。

次の図は、次の 2 つの可能なケースを示します。 最初の例で認証フィルターは、要求を正常に認証、承認フィルターは、要求を承認およびコント ローラーのアクションには、200 (OK) が返されます。

![](authentication-filters/_static/image1.png)

2 番目の例では、要求を認証する認証フィルターが、承認フィルターが 401 (Unauthorized) を返します。 この場合、コント ローラーのアクションは呼び出されません。 認証フィルターは、Www 認証ヘッダーを応答に追加します。

![](authentication-filters/_static/image2.png)

その他の組み合わせも有効である&mdash;など、コント ローラーのアクションは、匿名の要求を許可している場合が起き認証フィルターが、権限がありません。

## <a name="implementing-the-authenticateasync-method"></a>AuthenticateAsync メソッドの実装

**AuthenticateAsync**メソッドは、要求を認証しようとしています。 メソッドのシグネチャを次に示します。

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync**メソッドは、次のいずれかを実行する必要があります。

1. 何も行われません (何も行いません)。
2. 作成、 **IPrincipal**し、要求に設定します。
3. エラーの結果を設定します。

オプション (1) では、要求には、フィルターを理解しているすべての資格情報がありませんでした。 オプション (2) では、フィルターでは、要求が正常に認証されます。 オプション (3) では、要求は、エラー応答をトリガーする (正しくないパスワード) などの無効な資格情報をいました。

実装するための一般的な概要を次に示します**AuthenticateAsync**です。

1. 要求で資格情報を参照してください。
2. 資格情報がない場合は、何もしないし、(何も行いません) を返します。
3. 資格情報があるフィルターは、認証スキームを認識しない場合は、何もしないし、(何も行いません) を返します。 パイプライン内の別のフィルターは、スキームを理解することがあります。
4. フィルターを認識する資格情報がある場合は、認証を再試行してください。
5. 資格情報が正しくない場合は、設定して 401 を返す`context.ErrorResult`です。
6. 資格情報が有効な場合は、作成、 **IPrincipal**設定と`context.Principal`です。

次のコードに示す、 **AuthenticateAsync**メソッドから、[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)サンプルです。 コメントは、各手順を示します。 コードは、いくつかの種類のエラー: ない資格情報、形式が正しくない資格情報、および不適切なユーザー名とパスワードと共に、Authorization ヘッダー。

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>エラーの結果を設定

フィルターを設定する必要があります、資格情報が有効でない場合`context.ErrorResult`を**IHttpActionResult**エラー応答を作成します。 詳細については**IHttpActionResult**を参照してください[Web API 2 のアクション結果](../getting-started-with-aspnet-web-api/action-results.md)です。

基本認証のサンプルが含まれています、`AuthenticationFailureResult`この目的に適したクラスです。

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>ChallengeAsync を実装します。

目的、 **ChallengeAsync**メソッドは、必要な場合は、応答に認証チャレンジを追加するは。 メソッドのシグネチャを次に示します。

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

メソッドは、要求パイプライン内のすべての認証フィルターで呼び出されます。

理解することが重要**ChallengeAsync**が呼び出された*する前に*HTTP 応答が作成され、コント ローラー アクションの実行前にも可能性があります。 ときに**ChallengeAsync**が呼び出されると、`context.Result`が含まれています、 **IHttpActionResult**、後で、HTTP 応答の作成に使用されます。 ため**ChallengeAsync**が呼び出されると、HTTP 応答についての情報がまだ決まっていません。 **ChallengeAsync**メソッドの元の値を置き換える必要があります`context.Result`を新しい**IHttpActionResult**です。 これは、 **IHttpActionResult**元をラップする必要があります`context.Result`です。

![](authentication-filters/_static/image3.png)

元の名前を付けます**IHttpActionResult** 、*内部の結果*、および新しい**IHttpActionResult** 、*外部結果*です。 外側の結果では、次の操作を行います必要があります。

1. HTTP 応答を作成する内部の結果を呼び出します。
2. 応答を確認します。
3. 必要な場合は、応答に認証チャレンジを追加します。

次の例は、基本認証のサンプルから取得されます。 定義する、 **IHttpActionResult**の外側の結果。

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult`プロパティは、内部、保持**IHttpActionResult**です。 `Challenge`プロパティは、Www 認証ヘッダーを表します。 注意して**ExecuteAsync** 、まず呼び出し`InnerResult.ExecuteAsync`を HTTP 応答を作成し、必要な場合は、チャレンジを追加します。

チャレンジを追加する前に、応答コードを確認します。 ほとんどの認証スキームのみチャレンジを追加する場合は次のように、応答は 401、します。 ただし、一部の認証スキームでは、成功の応答にチャレンジを追加しないでください。 たとえばを参照してください[Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559)。

指定された、`AddChallengeOnUnauthorizedResult`クラス、実際のコードで**ChallengeAsync**は単純です。 だけ、結果を作成してアタッチすることを`context.Result`です。

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

注: 基本認証のサンプルを抽象化このロジックをビット拡張メソッドに配置します。

## <a name="combining-authentication-filters-with-host-level-authentication"></a>ホスト レベルの認証で認証フィルターを組み合わせる

「ホスト レベルの認証」では、要求に達すると、Web API フレームワークの前に (IIS など)、ホストによって認証が実行です。

多くの場合、残り、アプリケーションのホスト レベルの認証を有効にするには、Web API コント ローラーに対して無効にすることがあります。 たとえば、一般的なシナリオはホスト レベルでは、フォーム認証を有効にするが、Web API のトークン ベースの認証を使用します。

Web API パイプライン内のホスト レベルの認証を無効にするを呼び出す`config.SuppressHostPrincipal()`構成にします。 これが原因で削除する Web API、 **IPrincipal** Web API パイプラインが入力したすべての要求からです。 実際には、その&quot;解除-認証&quot;要求します。

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web API のセキュリティ フィルターの](https://msdn.microsoft.com/en-us/magazine/dn781361.aspx)(MSDN マガジン)
