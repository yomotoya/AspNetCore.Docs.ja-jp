---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: ASP.NET Web API で認証と承認 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で認証と承認の概要を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 981eebeaa1daaf85cb90a52f073c88cb71099edb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365621"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>ASP.NET Web API で認証と承認
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

Web API を作成したが、それへのアクセスを制御するようになりました。 この一連の記事では、承認されていないユーザーから web API をセキュリティで保護するためのいくつかのオプションを紹介します。 このシリーズは、認証と承認の両方について説明します。

- *認証*ユーザーの id が把握することです。 たとえば、Alice は自分のユーザー名とパスワード、ログイン、サーバーでは、パスワードを使用して、Alice の認証。
- *承認*アクションを実行するユーザーが許可されているかどうかを決めることです。 たとえば、Alice、リソースを取得するが、リソースを作成できません権があるのです。

シリーズの最初の記事では、ASP.NET Web API で認証と承認の概要を示します。 その他のトピックでは、Web API の一般的な認証シナリオについて説明します。

> [!NOTE]
> このシリーズを確認し、貴重なフィードバックを提供するユーザーに協力してくれた: Rick Anderson、Levi Broderick、Barry Dorrans、Tom Dykstra、Hongmei Ge、David Matson、Daniel Roth、Tim Teebken します。


## <a name="authentication"></a>認証

Web API では、ホストでその認証を行いますを前提としています。 Web ホスト、ホスト認証のための HTTP モジュールを使用して、IIS です。 IIS または ASP.NET に組み込まれている認証モジュールのいずれかを使用するプロジェクトを構成したり、カスタム認証を実行する独自の HTTP モジュールを記述できます。

作成、ホストが、ユーザーを認証するとき、*プリンシパル*は、 [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)コードが実行されているセキュリティ コンテキストを表すオブジェクト。 ホストを設定して現在のスレッドにプリンシパルをアタッチします**Thread.CurrentPrincipal**します。 プリンシパルが含まれていますが、関連付けられている**Identity**ユーザーに関する情報を含むオブジェクト。 ユーザーが認証されている場合、 **Identity.IsAuthenticated**プロパティが返す**true**します。 匿名の要求に対して**IsAuthenticated**返します**false**します。 プリンシパルの詳細については、次を参照してください。[ロール ベース セキュリティ](https://msdn.microsoft.com/library/shz8h065.aspx)します。

### <a name="http-message-handlers-for-authentication"></a>認証のための HTTP メッセージ ハンドラー

ホストの認証を使用する代わりに認証ロジックを配置することができます、 [HTTP メッセージ ハンドラー](../advanced/http-message-handlers.md)します。 その場合は、メッセージ ハンドラーでは、HTTP 要求を検査し、プリンシパルを設定します。

認証メッセージ ハンドラーを使用するにとする必要がありますか。 一部のトレードオフについて次に示します。

- HTTP モジュールでは、ASP.NET パイプラインを通過するすべての要求が表示されます。 メッセージ ハンドラーには、Web API にルーティングされる要求のみが表示されます。
- -ルート メッセージ ハンドラーでは、特定のルートへの認証方式を適用できる設定できます。
- HTTP モジュールは、IIS に固有です。 メッセージ ハンドラーは、web ホスティングと自己ホスト両方で使えるように、ホストに依存しないをします。
- HTTP モジュールは、IIS ログ、監査、および具合に参加します。
- HTTP モジュールは、パイプラインの前で実行します。 メッセージ ハンドラーでの認証を処理する場合、プリンシパルはまで設定されません、ハンドラーが実行されます。 さらに、応答が、メッセージ ハンドラーを離れると、プリンシパルは、以前のプリンシパルに戻ります。

一般に、自己ホストをサポートするために不要な場合は、HTTP モジュールはより適切なオプションには。 自己ホストをサポートする必要がある場合は、メッセージ ハンドラーを検討してください。

### <a name="setting-the-principal"></a>プリンシパルの設定

アプリケーションでは、任意のカスタム認証ロジックを実行する場合は、2 つの場所にプリンシパルを設定する必要があります。

- **Thread.CurrentPrincipal**します。 このプロパティは、.net では、スレッドのプリンシパルを設定する標準的な方法です。
- **ロール**します。 このプロパティは、ASP.NET に固有です。

次のコードでは、プリンシパルを設定する方法を示します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Web ホストの両方の場所で、プリンシパルを設定する必要があります。それ以外の場合、セキュリティ コンテキストは、一貫性のないなる可能性があります。 自己ホスト、ただし、 **HttpContext.Current**が null です。 コードがホストに依存しないのためには、そのため、null チェックを割り当てる前に**HttpContext.Current**に示すようにします。

## <a name="authorization"></a>承認

承認の発生、パイプラインの後半で、コント ローラーに近いです。 リソースへのアクセスを付与する場合より詳細な選択を行うことができます。

- *承認フィルター*コント ローラー アクションの前に実行します。 要求が許可されていない場合、フィルターが、エラー応答を返すし、アクションは呼び出されません。
- コント ローラーのアクション内から現在のプリンシパルを取得できます、 **ApiController.User**プロパティ。 など、ユーザー名に基づくリソースの一覧をフィルターする可能性がありますにそのユーザーに属しているリソースのみを返します。

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>使用して、[承認] 属性

Web API は、組み込みの承認フィルターを提供します。 [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)します。 このフィルターは、ユーザーが認証されたかどうかを確認します。 それ以外の場合は、アクションを呼び出すことがなく HTTP 状態コード 401 (Unauthorized) を返します。

コント ローラー レベル、または個々 のアクションのレベルでグローバルにフィルターを適用することができます。

**グローバルに**: すべての Web API コント ローラーのアクセスを制限するには、追加、 **AuthorizeAttribute**グローバル フィルターの一覧にフィルター。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**コント ローラー**: 特定のコント ローラーへのアクセスを制限するには、コント ローラーに属性として、フィルターを追加します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**アクション**: 特定の操作に対してアクセスを制限するには、アクション メソッドに属性を追加します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

また、コント ローラーを制限しを使用して、特定のアクションへの匿名アクセスを許可、`[AllowAnonymous]`属性。 次の例では、`Post`メソッドが制限されていますが、`Get`メソッドは匿名アクセスを許可します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

前の例では、フィルターが認証されたユーザーに制限されているメソッド; へのアクセスを許可します。匿名ユーザーのみが保持されます。特定のユーザーまたは特定のロールのユーザーへのアクセスを制限することもできます。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute**に Web API コント ローラーのフィルターがある、 **System.Web.Http**名前空間。 MVC コント ローラーのようなフィルターがある、 **System.Web.Mvc**名前空間は、Web API コント ローラーと互換性がありません。


### <a name="custom-authorization-filters"></a>カスタム承認フィルター

カスタム承認フィルターを作成するには、これらの型のいずれかから派生します。

- **AuthorizeAttribute**します。 現在のユーザーとユーザーのロールに基づいて承認ロジックを実行するには、このクラスを拡張します。
- **AuthorizationFilterAttribute**します。 現在のユーザーまたはロールに対しては必ずしも基づいていない同期承認ロジックを実行するには、このクラスを拡張します。
- **IAuthorizationFilter**します。 非同期の承認ロジックを実行するには、このインターフェイスを実装します。たとえば、承認ロジックが非同期 I/O またはネットワーク呼び出しを行った場合。 (から派生する方が簡単では、承認ロジックが CPU バインドの場合は、 **AuthorizationFilterAttribute**のため、非同期のメソッドを記述する必要はありません)。

次の図は、クラスの階層構造、 **AuthorizeAttribute**クラス。

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>コント ローラー アクション内での承認

場合によっては、続行して、プリンシパルに基づく動作を変更するには、要求を許可する場合があります。 などを返す情報は、ユーザーのロールに応じて変更可能性があります。 コント ローラー メソッド内から現在のプリンシパルを取得できます、 **ApiController.User**プロパティ。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
