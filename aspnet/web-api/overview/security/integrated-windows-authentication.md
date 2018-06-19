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
<a name="integrated-windows-authentication"></a><span data-ttu-id="5b917-103">統合 Windows 認証</span><span class="sxs-lookup"><span data-stu-id="5b917-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="5b917-104">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5b917-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5b917-105">統合 Windows 認証では、Kerberos または NTLM を使用して、Windows 資格情報でログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="5b917-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="5b917-106">クライアントは、承認ヘッダーに資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="5b917-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="5b917-107">Windows 認証は、イントラネット環境に最も適しています。</span><span class="sxs-lookup"><span data-stu-id="5b917-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="5b917-108">詳細については、次を参照してください。 [Windows 認証](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)です。</span><span class="sxs-lookup"><span data-stu-id="5b917-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="5b917-109">長所</span><span class="sxs-lookup"><span data-stu-id="5b917-109">Advantages</span></span> | <span data-ttu-id="5b917-110">短所</span><span class="sxs-lookup"><span data-stu-id="5b917-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="5b917-111">-IIS に組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="5b917-111">- Built into IIS.</span></span> <span data-ttu-id="5b917-112">-要求では、ユーザーの資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="5b917-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="5b917-113">-クライアント コンピューターに属する場合、ドメイン (たとえば、イントラネット アプリケーション)、ユーザーが資格情報を入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5b917-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="5b917-114">-インターネット アプリケーションは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="5b917-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="5b917-115">-クライアントでの Kerberos または NTLM のサポートが必要です。</span><span class="sxs-lookup"><span data-stu-id="5b917-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="5b917-116">クライアントは、Active Directory ドメインにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b917-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="5b917-117">アプリケーションが Azure でホストされているし、内部設置型 Active Directory ドメインを使用している場合、オンプレミス AD と Azure Active Directory のフェデレーションを検討してください。</span><span class="sxs-lookup"><span data-stu-id="5b917-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="5b917-118">ようにして、ユーザーは自分のオンプレミスの資格情報でログインすることができますが、Azure AD によって認証されます。</span><span class="sxs-lookup"><span data-stu-id="5b917-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="5b917-119">詳細については、次を参照してください。 [Azure 認証](../../../visual-studio/overview/2012/windows-azure-authentication.md)です。</span><span class="sxs-lookup"><span data-stu-id="5b917-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="5b917-120">統合 Windows 認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「イントラネット アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="5b917-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="5b917-121">このプロジェクト テンプレートは、Web.config ファイルで、次の設定しています。</span><span class="sxs-lookup"><span data-stu-id="5b917-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="5b917-122">クライアント側で統合 Windows 認証の動作をサポートするすべてのブラウザーで、 [Negotiate](http://www.ietf.org/rfc/rfc4559.txt)認証方式は、ほとんどの主要なブラウザーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5b917-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="5b917-123">.NET クライアント アプリケーションの場合、 **HttpClient**クラスは、Windows 認証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="5b917-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="5b917-124">Windows 認証は、クロスサイト リクエスト フォージェリ (CSRF) 攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="5b917-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="5b917-125">参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)です。</span><span class="sxs-lookup"><span data-stu-id="5b917-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
