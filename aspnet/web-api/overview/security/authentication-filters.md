---
uid: web-api/overview/security/authentication-filters
title: ASP.NET Web API 2 での認証フィルター |Microsoft Docs
author: MikeWasson
description: 認証フィルターは、HTTP 要求を認証するコンポーネントです。 認証フィルターを両方サポート web API 2 と MVC 5 がこれらとは若干.
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: a14facad4cbd0f9be1ff7bde2667f61ec8cc2a14
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829307"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 での認証フィルター
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> 認証フィルターは、HTTP 要求を認証するコンポーネントです。 認証フィルターを両方サポート web API 2 と MVC 5 がフィルター インターフェイスの名前付け規則のほとんどの場合、少しが異なります。 このトピックでは、Web API 認証フィルターについて説明します。


認証フィルターを使用して、個々 のコント ローラーまたはアクションの認証方式を設定できます。 これにより、アプリは、さまざまな HTTP リソースを異なる認証メカニズムをサポートできます。

この記事でのコードを紹介します、[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)でサンプル[ http://aspnet.codeplex.com](http://aspnet.codeplex.com)します。 このサンプルでは、(RFC 2617) HTTP 基本アクセス認証スキームを実装する認証フィルターを示します。 フィルターがという名前のクラスで実装された`IdentityBasicAuthenticationAttribute`します。 見せしませんのサンプル コードはすべて認証フィルターを記述する方法を説明する部分だけです。

## <a name="setting-an-authentication-filter"></a>認証フィルターの設定

他のフィルターのような認証フィルターは、適用されたコント ローラーごと、アクション、またはにグローバルにすべての Web API コント ローラー。

コント ローラーには、認証フィルターを適用するには、フィルター属性でコント ローラー クラスを装飾します。 次のコード セット、`[IdentityBasicAuthentication]`コント ローラー クラスは、すべてのコント ローラーのアクションの基本認証を有効にするフィルター。

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

1 つのアクションには、フィルターを適用するには、フィルターでアクションを装飾します。 次のコード セット、`[IdentityBasicAuthentication]`コント ローラーのフィルター`Post`メソッド。

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

追加するすべての Web API コント ローラーに、フィルターを適用する**GlobalConfiguration.Filters**します。

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Web API の認証フィルターを実装します。

Web api では、認証フィルターを実装、 [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)インターフェイス。 継承する必要がありますも**System.Attribute**、属性として適用するためにします。

**IAuthenticationFilter**インターフェイスが 2 つのメソッドには。

- **AuthenticateAsync**存在する場合、要求で資格情報を検証することで要求を認証します。
- **ChallengeAsync**必要な場合に、HTTP 応答に認証チャレンジを追加します。

これらのメソッドで定義されている認証フローに対応して[RFC 2612](http://tools.ietf.org/html/rfc2616)と[RFC 2617](http://tools.ietf.org/html/rfc2617):

1. クライアントは、Authorization ヘッダーで、資格情報を送信します。 これは通常、クライアントがサーバーから 401 (未承認) 応答を受信した後に発生します。 ただし、クライアントは、401 を取得した後だけでなく、すべての要求で資格情報を送信できます。
2. サーバーが資格情報を受け付けない場合は、401 (未承認) 応答を返します。 応答には、1 つまたは複数の課題を含む Www-authenticate ヘッダーが含まれています。 各チャレンジでは、サーバーによって認識される認証方式を指定します。

サーバーでは、匿名の要求から 401 を返すこともできます。 実際には、通常、これは、認証プロセスを開始する方法です。

1. クライアントは、匿名の要求を送信します。
2. サーバーは、401 を返します。
3. クライアントは、資格情報を持つ要求を再送信します。

このフローでは、両方に含まれる*認証*と*承認*手順を実行します。

- 認証では、クライアントの身元を証明します。
- 承認は、クライアントが特定のリソースにアクセスできるかどうかを判断します。

Web api では、認証フィルター処理が、認証、承認ではありません。 承認フィルターまたはコント ローラー アクション内では、承認を行う必要があります。

Web API 2 のパイプラインで、フローを示します。

1. アクションを呼び出す前に、Web API は、そのアクションの認証フィルターの一覧を作成します。 これには、アクションの範囲、コント ローラーのスコープ、およびグローバル スコープを使用したフィルターが含まれます。
2. Web API 呼び出し**AuthenticateAsync**一覧内のすべてのフィルターにします。 各フィルターは、要求に資格情報を検証できます。 任意のフィルターに資格情報が正常に検証する場合、フィルターを作成、 **IPrincipal**要求にアタッチします。 フィルターはこの時点でエラーをトリガーもできます。 そうである場合、パイプラインの残りの部分は実行されません。
3. エラーがないと仮定すると、要求は、パイプラインの残りの部分をフローします。
4. 最後に、Web API 呼び出しはすべて認証フィルターの**ChallengeAsync**メソッド。 フィルターは、必要な場合、応答にチャレンジを追加するこのメソッドを使用します。 通常 (が常にではありません) ですが、発生 401 エラーに応答します。

次の図は、2 つの可能なケースを示します。 最初の例では、認証フィルターは、要求を正常に認証、承認フィルターは、要求を承認およびコント ローラー アクションは、200 (OK) を返します。

![](authentication-filters/_static/image1.png)

2 番目の例では、認証フィルターは、要求を認証、承認フィルターは 401 (Unauthorized) を返します。 この場合、コント ローラー アクションは呼び出されません。 認証フィルターは、Www 認証ヘッダーを応答に追加します。

![](authentication-filters/_static/image2.png)

その他の組み合わせが可能です。&mdash;など、コント ローラー アクションは、匿名の要求を許可している場合する必要がありますが、認証フィルター、承認なし。

## <a name="implementing-the-authenticateasync-method"></a>AuthenticateAsync メソッドを実装します。

**AuthenticateAsync**メソッドは、要求を認証しようとしています。 メソッド シグネチャを次に示します。

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync**メソッドは、次のいずれかを実行する必要があります。

1. Nothing (操作なし)。
2. 作成、 **IPrincipal**し、要求に設定します。
3. エラーの結果を設定します。

オプション (1) では、要求には、フィルターを認識する資格情報がありませんでした。 オプション (2) では、フィルターは、要求を正常に認証します。 オプション (3) では、要求は、エラー応答をトリガーする (正しくないパスワード) のような無効な資格情報をいました。

実装するための一般的な概要を次に示します**AuthenticateAsync**します。

1. 要求で資格情報を参照してください。
2. 資格情報がない場合は、何もしないし、(操作なし) を返します。
3. 資格情報が、フィルターは、認証スキームを認識しない場合、何もしないし、(操作なし) を返します。 パイプライン内の別のフィルターは、スキームを理解する可能性があります。
4. フィルターを認識する資格情報がある場合は、認証をお試しください。
5. 資格情報が正しくない場合は、401 を返すを設定して`context.ErrorResult`します。
6. 資格情報が有効な場合は、作成、 **IPrincipal**設定と`context.Principal`します。

次のコードに示す、 **AuthenticateAsync**からメソッド、[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)サンプル。 コメントは、各手順を示します。 コードでは、いくつかの種類のエラーを示しています。: ない資格情報、形式が正しくない資格情報、および不適切なユーザー名/パスワードで An Authorization ヘッダー。

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>エラーの結果を設定

資格情報が有効でない場合、フィルターを設定する必要があります`context.ErrorResult`を**IHttpActionResult**エラー応答を作成します。 詳細については**IHttpActionResult**を参照してください[Web API 2 でアクションの結果](../getting-started-with-aspnet-web-api/action-results.md)します。

基本認証のサンプルが含まれています、`AuthenticationFailureResult`はこの目的に適したクラスです。

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>ChallengeAsync を実装します。

目的、 **ChallengeAsync**メソッドは、必要な場合、応答に認証チャレンジを追加するは。 メソッド シグネチャを次に示します。

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

メソッドは、要求パイプライン内のすべての認証フィルターで呼び出されます。

理解することが重要**ChallengeAsync**呼びます*する前に*HTTP 応答が作成、され、コント ローラー アクションを実行する前にも可能性があります。 ときに**ChallengeAsync**が呼び出され、`context.Result`が含まれています、 **IHttpActionResult**、HTTP 応答を作成する後で使用します。 したがって**ChallengeAsync**が呼び出されると、何もわからない HTTP 応答についてまだします。 **ChallengeAsync**メソッドの元の値を置き換える必要があります`context.Result`を新しい**IHttpActionResult**します。 これは、 **IHttpActionResult**元をラップする必要があります`context.Result`します。

![](authentication-filters/_static/image3.png)

元と呼ぶことに**IHttpActionResult** 、*内部結果*、および新しい**IHttpActionResult** 、*外部結果*。 外部の結果では、次の操作を行う必要があります。

1. HTTP 応答を作成する内部の結果を呼び出します。
2. 応答を確認します。
3. 必要な場合は、応答に認証チャレンジを追加します。

次の例は、基本認証のサンプルから取得されます。 定義、 **IHttpActionResult**の外側の結果。

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult`プロパティを保持する内部**IHttpActionResult**します。 `Challenge`プロパティは、Www 認証ヘッダーを表します。 注意して**ExecuteAsync**まず呼び出し`InnerResult.ExecuteAsync`HTTP 応答を作成するために必要な場合に、チャレンジを追加します。

チャレンジを追加する前に、応答コードを確認します。 ほとんどの認証方式は、チャレンジを追加するだけ応答は、次に示すように、401 が場合。 ただし、一部の認証スキームでは、成功応答にチャレンジを追加しないでください。 たとえばを参照してください[ネゴシエート](http://tools.ietf.org/html/rfc4559#section-5)(RFC 4559)。

指定された、`AddChallengeOnUnauthorizedResult`クラス、実際のコードで**ChallengeAsync**は単純です。 だけ、結果を作成およびアタッチ先`context.Result`します。

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

注: 基本認証のサンプルを抽象化このロジック、拡張メソッドに配置します。

## <a name="combining-authentication-filters-with-host-level-authentication"></a>ホスト レベルの認証を使用した認証フィルターを組み合わせる

「ホスト レベルの認証」では前の要求に達すると、Web API フレームワークに、(IIS) などのホストによって認証が実行です。

多くの場合に、アプリケーションの残りのホスト レベルの認証を有効にするが、Web API コント ローラーに対して無効にする可能性があります。 たとえば、一般的なシナリオはホスト レベルでは、フォーム認証を有効にするが、Web API のトークン ベースの認証を使用します。

Web API パイプライン内のホスト レベルの認証を無効にするには、呼び出す`config.SuppressHostPrincipal()`構成します。 これが原因で削除する Web API、 **IPrincipal** Web API パイプラインが入力したすべての要求から。 実際には、その&quot;解除-認証&quot;要求。

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web API のセキュリティ フィルター](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN マガジン)
