---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: ASP.NET Web API でクロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ |Microsoft ドキュメント
author: MikeWasson
description: クロスサイト リクエスト フォージェリ (CSRF) 攻撃と ASP.NET Web API でアンチ CSRF メジャーを実装する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 1cd03f3b396cc2ece1d8dbe6820f6277c02d8e62
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508151"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>ASP.NET Web API でクロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止
====================
によって[Mike Wasson](https://github.com/MikeWasson)

クロスサイト リクエスト フォージェリ (CSRF) は、攻撃が悪意のあるサイトは、ユーザーが現在記録される場所で脆弱なサイトに要求を送信

CSRF 攻撃の例を次に示します。

1. Www.example.com にユーザーが、フォーム認証の使用します。
2. サーバーは、ユーザーを認証します。 サーバーからの応答には、認証 cookie が含まれています。
3. ログアウト、しなくてもユーザー悪意のある web サイトにアクセスします。 この悪意のあるサイトには、次の HTML フォームが含まれています。 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    フォームのアクションが悪意のあるサイトではない、脆弱なサイトにポストすることに注意してください。 これは、CSRF の「クロスサイト」の一部です。
4. ユーザーは、[送信] ボタンをクリックします。 ブラウザーには、要求と共に認証 cookie が含まれています。
5. 要求は、ユーザーの認証コンテキストを使用してサーバー上で実行し、は、認証されたユーザーの操作が許可されているものを何でも実行できます。

この例には、ユーザーがフォームのボタンをクリックする必要がありますが、悪意のあるページがフォームを自動的に送信するスクリプトを簡単に実行でした。 さらに、SSL を使用して有効になって、CSRF 攻撃では、悪意のあるサイトは、"https://"要求を送信できるためです。

通常、CSRF 攻撃は、ブラウザーが web サイトに関連するすべての cookie を送信するため、認証の cookie を使用する web サイトに対して可能です。 ただし、CSRF 攻撃では、cookie の悪用に制限はありません。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 基本認証またはダイジェスト認証を使用してログオンした後 ブラウザーは、セッションが終了するまで、資格情報を自動的に送信します。

## <a name="anti-forgery-tokens"></a>偽造防止トークン

ASP.NET MVC を CSRF 攻撃を防止するとも呼ばれる、偽造防止トークンを使用して*検証トークンを要求できる*です。

1. クライアントは、フォームを含む HTML ページを要求します。
2. サーバーには、応答内の 2 つのトークンが含まれています。 1 つのトークンは、cookie として送信されます。 隠しフォーム フィールドに、他の配置されます。 トークンは、敵対者が、値を推測できないようにランダムに生成されます。
3. クライアントは、フォームを送信するときに、サーバーに両方のトークンを送信、必要があります。 クライアントがクッキーとして cookie トークンを送信して、内フォーム データ、フォーム トークンを送信します。 (ブラウザー クライアントが自動的にこのユーザーがフォームを送信します。)
4. 要求に両方のトークンが含まれていない場合、サーバーには、要求が許可されていません。

非表示のフォームのトークンを使用して HTML フォームの例を次に示します。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

偽造防止トークンは動作ため、悪意のあるページが同じオリジン ポリシーにより、ユーザーのトークンを読み取ることができません。 ([同じオリジン ポリシー](http://www.w3.org/Security/wiki/Same_Origin_Policy)が互いのコンテンツにアクセスの 2 つの異なるサイトでホストされているドキュメントを防止します。 前の例では、悪意のあるページは、example.com に要求を送信できますが、応答を読み取ることができません)。

CSRF 攻撃を防ぐため、ブラウザーが密かに送信任意の認証プロトコルの偽造防止トークンを使用するには、資格情報をユーザーにログインした後です。 これは、フォーム認証など、クッキー ベースの認証プロトコルを含むだけでなく基本認証とダイジェスト認証などのプロトコルです。

偽造防止トークンは、nonsafe メソッド (POST、PUT、DELETE) 必要があります。 また、セーフ メソッド (GET、HEAD) には、副作用がないことを確認します。 さらに、CORS など、JSONP、クロス ドメインのサポートを有効にした場合 GET なども安全な方法は、攻撃者が機密性の高いデータを読み取ることができる CSRF 攻撃に対して脆弱であります。

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>ASP.NET MVC の偽造防止トークン

Razor ページに、偽造防止トークンを追加するには、使用、 **HtmlHelper.AntiForgeryToken**ヘルパー メソッド。

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

このメソッドは、隠しフォーム フィールドを追加し、また、cookie トークンを設定します。

## <a name="anti-csrf-and-ajax"></a>アンチ CSRF と AJAX

フォームのトークンには、AJAX 要求が送信する JSON データを HTML フォーム データではなくために AJAX 要求時に問題があります。 1 つのソリューションでは、カスタム HTTP ヘッダーにトークンを送信します。 次のコードでは、Razor 構文を使用して、トークンを生成し、AJAX 要求をトークンを追加します。 トークンが呼び出しによって、サーバーで生成された**AntiForgery.GetTokens**です。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

要求を処理するときは、要求ヘッダーからトークンを抽出します。 まず、 **AntiForgery.Validate**トークンを検証するメソッド。 **検証**トークンが有効でない場合、メソッドが例外をスローします。

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
