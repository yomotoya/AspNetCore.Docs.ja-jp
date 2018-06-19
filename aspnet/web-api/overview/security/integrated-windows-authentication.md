---
uid: web-api/overview/security/integrated-windows-authentication
title: 統合 Windows 認証 |Microsoft ドキュメント
author: MikeWasson
description: ASP.NET Web API で統合 Windows 認証の使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508161"
---
<a name="integrated-windows-authentication"></a>統合 Windows 認証
====================
によって[Mike Wasson](https://github.com/MikeWasson)

統合 Windows 認証では、Kerberos または NTLM を使用して、Windows 資格情報でログインすることができます。 クライアントは、承認ヘッダーに資格情報を送信します。 Windows 認証は、イントラネット環境に最も適しています。 詳細については、次を参照してください。 [Windows 認証](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)です。

| 長所 | 短所 |
| --- | --- |
| -IIS に組み込まれます。 -要求では、ユーザーの資格情報を送信しません。 -クライアント コンピューターに属する場合、ドメイン (たとえば、イントラネット アプリケーション)、ユーザーが資格情報を入力する必要はありません。 | -インターネット アプリケーションは推奨されません。 -クライアントでの Kerberos または NTLM のサポートが必要です。 クライアントは、Active Directory ドメインにする必要があります。 |

> [!NOTE]
> アプリケーションが Azure でホストされているし、内部設置型 Active Directory ドメインを使用している場合、オンプレミス AD と Azure Active Directory のフェデレーションを検討してください。 ようにして、ユーザーは自分のオンプレミスの資格情報でログインすることができますが、Azure AD によって認証されます。 詳細については、次を参照してください。 [Azure 認証](../../../visual-studio/overview/2012/windows-azure-authentication.md)です。


統合 Windows 認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「イントラネット アプリケーション」テンプレートを選択します。 このプロジェクト テンプレートは、Web.config ファイルで、次の設定しています。

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

クライアント側で統合 Windows 認証の動作をサポートするすべてのブラウザーで、 [Negotiate](http://www.ietf.org/rfc/rfc4559.txt)認証方式は、ほとんどの主要なブラウザーが含まれています。 .NET クライアント アプリケーションの場合、 **HttpClient**クラスは、Windows 認証をサポートします。

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows 認証は、クロスサイト リクエスト フォージェリ (CSRF) 攻撃に対して脆弱です。 参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)です。
