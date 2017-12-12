---
uid: web-api/overview/security/forms-authentication
title: "ASP.NET Web API でフォーム認証 |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API でフォーム認証の使用について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>ASP.NET Web API でフォーム認証
====================
によって[Mike Wasson](https://github.com/MikeWasson)

フォーム認証では、HTML フォームを使用して、ユーザーの資格情報をサーバーに送信します。 インターネット標準ではありません。 フォーム認証のみに適した web Api、web アプリケーションから呼び出されるユーザーが HTML フォームと対話できるようにします。

| 長所 | 短所 |
| --- | --- |
| 実装する簡単: ASP.NET に組み込まれています。 -ユーザー アカウントを管理しやすく ASP.NET メンバーシップ プロバイダーを使用します。 | -標準 HTTP 認証メカニズムです。標準の承認ヘッダーの代わりに、HTTP クッキーを使用します。 -クライアント ブラウザーが必要です。 資格情報は、プレーン テキストとして送信されます。 脆弱性にクロスサイト リクエスト フォージェリ (CSRF) です。アンチ CSRF メジャーが必要です。 -Nonbrowser クライアントから使用するが困難です。 ログインには、ブラウザーが必要です。 のユーザー資格情報は、要求で送信されます。 -一部のユーザーには、クッキーが無効にします。 |

簡単に、asp.net フォーム認証は、次のように動作します。

1. クライアントは、認証が必要なリソースを要求します。
2. ユーザーが認証されていない場合、サーバーは HTTP 302 (検出) を返し、ログイン ページにリダイレクトします。
3. ユーザーは、資格情報を入力し、フォームを送信します。
4. サーバーでは、元の URI にリダイレクトする別の HTTP 302 を返します。 この応答には、認証 cookie が含まれています。
5. クライアントは、リソースを再び要求します。 要求には、サーバー要求を許可するための認証クッキーが含まれています。

![](forms-authentication/_static/image1.png)

詳細については、次を参照してください。 [、フォーム認証の概要です。](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>フォーム認証を使用して Web API

フォーム認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「インターネット アプリケーション」テンプレートを選択します。 このテンプレートでは、アカウント管理用の MVC コント ローラーを作成します。 また、ASP.NET Fall 2012 更新プログラムで使用可能な「単一ページ アプリケーション」テンプレートを使用することができます。

Web API コント ローラーを使用してアクセスを制限することができます、`[Authorize]`属性」の説明に従って[[Authorize] 属性を使用して](authentication-and-authorization-in-aspnet-web-api.md#auth3)です。

フォーム認証では、要求の認証に、セッションの cookie を使用します。 ブラウザーは、web サイトに関連するすべての cookie を自動的に送信します。 この機能により、フォーム認証のクロスサイト リクエスト フォージェリ (CSRF) 攻撃を参照する可能性のある脆弱な[防止クロスサイト リクエスト フォージェリ (CSRF) 攻撃](preventing-cross-site-request-forgery-csrf-attacks.md)です。

フォーム認証では、ユーザーの資格情報は暗号化されません。 したがって、フォーム認証は SSL と共に使用しない限り、セキュリティで保護されたではありません。 参照してください[Web API での SSL の操作](working-with-ssl-in-web-api.md)です。
