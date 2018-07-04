---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: ASP.NET Web API でクロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止 |Microsoft Docs
author: MikeWasson
description: クロスサイト リクエスト フォージェリ (CSRF) 攻撃と ASP.NET Web API での CSRF 対策メジャーを実装する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 7dfddf09a1577cfa7a52f58b37533724a8475435
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379875"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>ASP.NET Web API でクロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

クロスサイト リクエスト フォージェリ (CSRF) は、攻撃で、ユーザーが現在ログインしている脆弱性のあるサイトに、悪意のあるサイトが要求を送信する場所

CSRF 攻撃の例を次に示します。

1. ユーザーがログイン`www.example.com`フォーム認証を使用します。
2. サーバーは、ユーザーを認証します。 サーバーからの応答には、認証 cookie が含まれています。
3. ログアウトすると、しなくてもユーザー悪意のある web サイトにアクセスします。 この悪意のあるサイトには、次の HTML フォームが含まれています。 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    フォームのアクションが悪意のあるサイトではない、脆弱性のあるサイトに投稿することに注意してください。 これは、CSRF の"cross-site"の一部です。
4. ユーザーは、[送信] ボタンをクリックします。 ブラウザーには、要求の認証クッキーが含まれています。
5. 要求はユーザーの認証コンテキストを持つサーバーで実行されには、認証されたユーザーが許可されているものを何でも実行できます。

この例には、ユーザーがフォームのボタンをクリックする必要がありますが、悪意のあるページがフォームを自動的に送信するスクリプトを簡単に実行する場合と同様でした。 さらに、SSL を使用しても、CSRF 攻撃、悪意のあるサイトは、"https://"要求を送信できるので。

通常、CSRF 攻撃は、認証 cookie を使用する web サイトに対して可能なブラウザーすべての関連クッキーを模擬 web サイトに送信するためです。 ただし、CSRF 攻撃では、cookie の利用に制限はありません。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 基本認証またはダイジェスト認証を使用してログオンした後 セッションが終了するまで、ブラウザーは資格情報を自動的に送信します。

## <a name="anti-forgery-tokens"></a>偽造防止トークン

CSRF 攻撃を防ぐため、ASP.NET MVC とも呼ばれる、偽造防止トークンを使用して*検証トークンを要求*します。

1. クライアントは、フォームを含む HTML ページを要求します。
2. サーバーには、応答内の 2 つのトークンが含まれています。 1 つのトークンは cookie として送信されます。 隠しフォーム フィールド、もう一方が配置されます。 トークンは、敵対者は、値を推測できないように、ランダムに生成されます。
3. クライアントは、フォームを送信する、サーバーに両方のトークンが送信する必要があります。 クライアントが、cookie として cookie トークンを送信し、フォーム データ内で、フォーム トークンを送信します。 (ブラウザー クライアントが自動的にこのユーザーがフォームを送信します。)
4. 要求に両方のトークンが含まれていない場合、サーバーには、要求が許可されていません。

隠しフォーム トークンを使用して HTML フォームの例を次に示します。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

偽造防止トークンは、悪意のあるページが同一オリジン ポリシーにより、ユーザーのトークンを読み取ることができませんので機能します。 ([同一オリジン ポリシー](http://www.w3.org/Security/wiki/Same_Origin_Policy)から他のコンテンツにアクセスする 2 つの異なるサイトでホストされているドキュメントを防止します。 ように、前の例で、悪意のあるページは、example.com に要求を送信できますが、応答を読み取ることができません)。

CSRF 攻撃を防ぐため、ブラウザーがサイレント モードで送信任意の認証プロトコルで偽造防止トークンを使用して、ユーザーがログインした後、資格情報します。 これは、フォーム認証など、cookie ベースの認証プロトコルを含むだけでなく基本認証とダイジェスト認証などのプロトコルします。

Nonsafe メソッド (POST、PUT、DELETE) の偽造防止トークンを要求する必要があります。 また、安全なメソッド (GET、HEAD) には、副作用がないことを確認します。 さらに、CORS または JSONP などのクロス ドメインのサポートを有効にした場合も安全な GET メソッドは、CSRF 攻撃の可能性がある機密データを閲覧できるようにする可能性のある脆弱であります。

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>ASP.NET MVC で偽造防止トークン

Razor ページに、偽造防止トークンを追加するには、使用、 **HtmlHelper.AntiForgeryToken**ヘルパー メソッド。

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

このメソッドは、隠しフォーム フィールドを追加し、また cookie トークンを設定します。

## <a name="anti-csrf-and-ajax"></a>CSRF 対策と AJAX

フォームのトークンは、JSON データを HTML フォーム データではなく、AJAX 要求を送信するため、AJAX 要求に対して問題にすることができます。 1 つのソリューションでは、カスタム HTTP ヘッダーでトークンを送信します。 次のコードでは、Razor 構文を使用して、トークンを生成し、AJAX 要求にトークンが追加されます。 トークンが呼び出すことによって、サーバーで生成された**AntiForgery.GetTokens**します。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

要求を処理するときに、要求ヘッダーからトークンを抽出します。 呼び出して、 **AntiForgery.Validate**トークンを検証するメソッド。 **検証**トークンが有効でない場合、メソッドが例外をスローします。

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
