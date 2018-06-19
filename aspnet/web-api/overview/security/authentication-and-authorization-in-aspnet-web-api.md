---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: ASP.NET Web API で認証と承認 |Microsoft ドキュメント
author: MikeWasson
description: ASP.NET Web API で認証と承認の概要を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d7cbb9505afb6461ba4c2087d57e9ea0da38ede
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
ms.locfileid: "29726759"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>ASP.NET Web API で認証と承認
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

Web API を作成したが、それへのアクセスを制御するようになりました。 この一連の記事では、承認されていないユーザーから web API をセキュリティで保護するためのいくつかのオプションを紹介します。 この系列は、認証と承認の両方に説明します。

- *認証*ユーザーの id を知ることができます。 たとえば、アリスが自分のユーザー名とパスワードを使用してログイン、サーバーを使用して、パスワード認証 Alice です。
- *承認*操作を実行するユーザーが許可されているかどうかを決定します。 たとえば、Alice は、リソースを取得する、リソースを作成できませんが、権限を持っています。

系列の最初の記事では、ASP.NET Web API で認証と承認の全般の概要を提供します。 その他のトピックでは、Web API の一般的な認証シナリオについて説明します。

> [!NOTE]
> この系列をレビューし、有益なフィードバックを提供するユーザー感謝: Rick Anderson、Levi Broderick、Barry Dorrans、Tom Dykstra、Hongmei Ge、David Matson、Daniel Roth、Tim Teebken です。


## <a name="authentication"></a>認証

Web API では、その認証は、ホストでが行われる前提としています。 Web ホスティングは、ホストは、認証用の HTTP モジュールを使用して、IIS です。 IIS または ASP.NET に組み込まれている認証モジュールを使用するプロジェクトを構成したり、カスタム認証を実行する独自の HTTP モジュールを記述できます。

作成、ホストが、ユーザーを認証するとき、*プリンシパル*、これは、 [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)コードが実行されているセキュリティ コンテキストを表すオブジェクト。 ホストが現在のスレッドに設定してプリンシパルをアタッチ**Thread.CurrentPrincipal**です。 プリンシパルが含まれていますが、関連付けられている**Identity**ユーザーに関する情報を含むオブジェクト。 ユーザーが認証されている場合、 **Identity.IsAuthenticated**プロパティから返される**true**です。 匿名要求について**IsAuthenticated**返します**false**です。 プリンシパルの詳細については、次を参照してください。[ロール ベース セキュリティ](https://msdn.microsoft.com/library/shz8h065.aspx)です。

### <a name="http-message-handlers-for-authentication"></a>認証用の HTTP メッセージ ハンドラー

を使う代わりに、ホスト認証のために認証ロジックを配置することができます、 [HTTP メッセージ ハンドラー](../advanced/http-message-handlers.md)です。 その場合は、メッセージ ハンドラーは、HTTP 要求を検査し、プリンシパルを設定します。

認証のメッセージ ハンドラーを使用するには場合必要がありますか。 一部のトレードオフを次に示します。

- HTTP モジュールには、ASP.NET パイプラインを通過するすべての要求が表示されます。 メッセージ ハンドラーには、Web API にルーティングされる要求のみが表示されます。
- ルート別のメッセージのハンドラーを特定のルートへの認証方式を適用することができますを設定できます。
- HTTP モジュールは、IIS に固有です。 メッセージ ハンドラーは、web ホストとの両方で自己ホストを使用できるように、ホストに依存しないがします。
- HTTP モジュールは、IIS ログ、監査、およびように参加します。
- HTTP モジュールは、パイプラインの前で実行します。 メッセージ ハンドラーでの認証を処理する場合、プリンシパルはまで設定されません、ハンドラーが実行されます。 さらに、応答が、メッセージ ハンドラーを離れると、プリンシパルは、以前のプリンシパルに戻ります。

一般に、自己ホスト型をサポートするために必要としない場合、HTTP モジュールより優れたオプションです。 自己ホスト型をサポートする必要がある場合は、メッセージのハンドラーを検討してください。

### <a name="setting-the-principal"></a>プリンシパルの設定

アプリケーションでは、任意のカスタム認証ロジックを実行する場合は、2 つの場所にプリンシパルを設定する必要があります。

- **Thread.CurrentPrincipal**です。 このプロパティは、.net では、スレッドのプリンシパルを設定する標準的な方法です。
- **HttpContext.Current.User**です。 このプロパティは、ASP.NET に固有です。

次のコードでは、プリンシパルを設定する方法を示します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Web ホスティングの両方の場所で、プリンシパルを設定する必要があります。それ以外の場合、セキュリティ コンテキストは、一貫性のないなる可能性があります。 自己ホスト、ただし、 **HttpContext.Current**が null です。 コードがホストにとらわれないのためには、そのため、null のチェックを割り当てる前に**HttpContext.Current**ように、します。

## <a name="authorization"></a>承認

承認は、パイプラインで後で発生コント ローラーに近づきます。 リソースへのアクセスを許可した場合より詳細な選択肢を行うことができます。

- *承認フィルター*コント ローラー アクションの前に実行します。 要求が許可されていない場合は、フィルターには、エラー応答が返されます、アクションは呼び出されません。
- コント ローラーのアクション、内から現在のプリンシパルを取得することができます、 **ApiController.User**プロパティです。 たとえば、可能性がありますをフィルター処理するユーザー名に基づいて、リソースの一覧にそのユーザーに属しているリソースのみを返します。

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>使用して、[承認] 属性

組み込み承認フィルターを提供する web API [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)です。 このフィルターは、ユーザーが認証されたかどうかを確認します。 それ以外の場合は、アクションを呼び出すことがなく HTTP ステータス コード 401 (Unauthorized) を返します。

コント ローラー レベル、または個々 のアクション レベルでグローバルに、フィルターを適用することができます。

**グローバル**: すべての Web API コント ローラーのアクセスを制限するには追加、 **AuthorizeAttribute**グローバル フィルターの一覧にフィルター。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**コント ローラー**: 特定のコント ローラーのアクセスを制限するフィルター属性としてコント ローラーに追加します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**アクション**: 特定のアクションのアクセスを制限するには、アクション メソッドに属性を追加します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

また、コント ローラーを制限してを使用して、特定の操作への匿名アクセスを許可、`[AllowAnonymous]`属性。 次の例で、`Post`メソッドは、制限されたが、`Get`メソッドは、匿名アクセスを許可します。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

前の例で、フィルターでは認証されたユーザーに、メソッドにアクセス制限です。匿名ユーザーのみが保持されます。特定のロールにユーザーまたは特定のユーザーにアクセスを制限することもできます。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute**に Web API コント ローラーのフィルターがある、 **System.Web.Http**名前空間。 MVC コント ローラーのようなフィルターがある、 **System.Web.Mvc**名前空間は、Web API コント ローラーと互換性がありません。


### <a name="custom-authorization-filters"></a>カスタム承認フィルター

カスタム承認フィルターを作成するには、これらの型のいずれかから派生します。

- **AuthorizeAttribute**です。 現在のユーザーとユーザーのロールに基づく承認ロジックを実行するには、このクラスを拡張します。
- **AuthorizationFilterAttribute**です。 現在のユーザーまたはロールに必ずしも基づかない同期承認ロジックを実行するには、このクラスを拡張します。
- **IAuthorizationFilter**です。 非同期の承認ロジックを実行するには、このインターフェイスを実装します。たとえば、承認ロジックが非同期 I/O またはネットワークの呼び出しを行う場合。 (から派生する方が簡単では、承認ロジックが CPU 主体の場合は、 **AuthorizationFilterAttribute**ので、非同期メソッドを記述する必要はありません)。

次の図のクラス階層を示しています、 **AuthorizeAttribute**クラスです。

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>コント ローラーのアクション内の承認

場合によっては、続行するがプリンシパルに基づく動作を変更する要求を許可する場合があります。 たとえばを返す情報は、ユーザーの役割に応じて変更可能性があります。 コント ローラー メソッド内から現在のプリンシパルを取得することができます、 **ApiController.User**プロパティです。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
