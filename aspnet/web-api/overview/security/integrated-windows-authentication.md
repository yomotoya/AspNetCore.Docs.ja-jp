---
uid: web-api/overview/security/integrated-windows-authentication
title: 統合 Windows 認証 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で統合 Windows 認証の使用について説明します。
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: 13dead421abf7ded73cbb2e5f87e54b1a869b5d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835367"
---
<a name="integrated-windows-authentication"></a>統合 Windows 認証
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

統合 Windows 認証では、Kerberos または NTLM を使用して、Windows 資格情報でログインすることができます。 クライアントは、Authorization ヘッダーで、資格情報を送信します。 Windows 認証は、イントラネット環境に最適です。 詳細については、次を参照してください。 [Windows 認証](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)します。

| 長所 | 短所 |
| --- | --- |
| -IIS に組み込まれています。 -要求では、ユーザーの資格情報を送信しません。 場合、クライアント コンピューターは、ドメイン (たとえば、イントラネット アプリケーション) に属している、ユーザーが資格情報を入力する必要はありません。 | -インターネット アプリケーションはお勧めしません。 -クライアントで Kerberos または NTLM のサポートが必要です。 クライアントは、Active Directory ドメインになければなりません。 |

> [!NOTE]
> アプリケーションが Azure でホストし、オンプレミスの Active Directory ドメインを使用している場合、オンプレミスの AD と Azure Active Directory のフェデレーションを検討してください。 これにより、ユーザーが自分のオンプレミスの資格情報でログインできますが、Azure AD によって認証されます。 詳細については、次を参照してください。 [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md)します。


統合 Windows 認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「イントラネット アプリケーション」テンプレートを選択します。 このプロジェクト テンプレートは、Web.config ファイルに次の設定を格納します。

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

統合 Windows 認証がサポートするブラウザーで動作、クライアント側で、[ネゴシエート](http://www.ietf.org/rfc/rfc4559.txt)認証方式は、ほとんどの主要なブラウザーが含まれています。 .NET クライアント アプリケーションで、 **HttpClient**クラスは、Windows 認証をサポートしています。

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows 認証では、クロスサイト リクエスト フォージェリ (CSRF) 攻撃に対して脆弱です。 参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)します。
