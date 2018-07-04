---
uid: web-api/overview/security/integrated-windows-authentication
title: 統合 Windows 認証 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で統合 Windows 認証の使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: f11b9fe5d98118a252c6c00dd2997b2ee9a3da7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381604"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="53f3a-103">統合 Windows 認証</span><span class="sxs-lookup"><span data-stu-id="53f3a-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="53f3a-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="53f3a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="53f3a-105">統合 Windows 認証では、Kerberos または NTLM を使用して、Windows 資格情報でログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="53f3a-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="53f3a-106">クライアントは、Authorization ヘッダーで、資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="53f3a-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="53f3a-107">Windows 認証は、イントラネット環境に最適です。</span><span class="sxs-lookup"><span data-stu-id="53f3a-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="53f3a-108">詳細については、次を参照してください。 [Windows 認証](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)します。</span><span class="sxs-lookup"><span data-stu-id="53f3a-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="53f3a-109">長所</span><span class="sxs-lookup"><span data-stu-id="53f3a-109">Advantages</span></span> | <span data-ttu-id="53f3a-110">短所</span><span class="sxs-lookup"><span data-stu-id="53f3a-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="53f3a-111">-IIS に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="53f3a-111">- Built into IIS.</span></span> <span data-ttu-id="53f3a-112">-要求では、ユーザーの資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="53f3a-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="53f3a-113">場合、クライアント コンピューターは、ドメイン (たとえば、イントラネット アプリケーション) に属している、ユーザーが資格情報を入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="53f3a-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="53f3a-114">-インターネット アプリケーションはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="53f3a-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="53f3a-115">-クライアントで Kerberos または NTLM のサポートが必要です。</span><span class="sxs-lookup"><span data-stu-id="53f3a-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="53f3a-116">クライアントは、Active Directory ドメインになければなりません。</span><span class="sxs-lookup"><span data-stu-id="53f3a-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="53f3a-117">アプリケーションが Azure でホストし、オンプレミスの Active Directory ドメインを使用している場合、オンプレミスの AD と Azure Active Directory のフェデレーションを検討してください。</span><span class="sxs-lookup"><span data-stu-id="53f3a-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="53f3a-118">これにより、ユーザーが自分のオンプレミスの資格情報でログインできますが、Azure AD によって認証されます。</span><span class="sxs-lookup"><span data-stu-id="53f3a-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="53f3a-119">詳細については、次を参照してください。 [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md)します。</span><span class="sxs-lookup"><span data-stu-id="53f3a-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="53f3a-120">統合 Windows 認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「イントラネット アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="53f3a-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="53f3a-121">このプロジェクト テンプレートは、Web.config ファイルに次の設定を格納します。</span><span class="sxs-lookup"><span data-stu-id="53f3a-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="53f3a-122">統合 Windows 認証がサポートするブラウザーで動作、クライアント側で、[ネゴシエート](http://www.ietf.org/rfc/rfc4559.txt)認証方式は、ほとんどの主要なブラウザーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="53f3a-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="53f3a-123">.NET クライアント アプリケーションで、 **HttpClient**クラスは、Windows 認証をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="53f3a-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="53f3a-124">Windows 認証では、クロスサイト リクエスト フォージェリ (CSRF) 攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="53f3a-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="53f3a-125">参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)します。</span><span class="sxs-lookup"><span data-stu-id="53f3a-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
