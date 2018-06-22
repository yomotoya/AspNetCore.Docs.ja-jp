---
title: ASP.NET Core のコミュニティ OSS 認証オプション
author: rick-anderson
description: ASP.NET Core のオープン ソースの認証オプションを検出します。
ms.author: riande
ms.date: 03/12/2018
uid: security/authentication/community
ms.openlocfilehash: 8b3800631cb71f6bd5120157c89765f6d72628ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276569"
---
# <a name="community-oss-authentication-options-for-aspnet-core"></a><span data-ttu-id="e66ae-103">ASP.NET Core のコミュニティ OSS 認証オプション</span><span class="sxs-lookup"><span data-stu-id="e66ae-103">Community OSS authentication options for ASP.NET Core</span></span>

<span data-ttu-id="e66ae-104">このページには、ASP.NET Core のコミュニティに用意されているオープン ソースの認証オプションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e66ae-104">This page contains community-provided, open source authentication options for ASP.NET Core.</span></span> <span data-ttu-id="e66ae-105">このページは、新しいプロバイダーが使用可能になるとして定期的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="e66ae-105">This page is periodically updated as new providers become available.</span></span>

# <a name="oss-authentication-providers"></a><span data-ttu-id="e66ae-106">OSS 認証プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e66ae-106">OSS authentication providers</span></span>

<span data-ttu-id="e66ae-107">以下の一覧はアルファベット順に並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="e66ae-107">The list below is sorted alphabetically.</span></span>

| <span data-ttu-id="e66ae-108">name</span><span class="sxs-lookup"><span data-stu-id="e66ae-108">Name</span></span> | <span data-ttu-id="e66ae-109">説明</span><span class="sxs-lookup"><span data-stu-id="e66ae-109">Description</span></span> |
| ---- | ----------- |
| [<span data-ttu-id="e66ae-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span><span class="sxs-lookup"><span data-stu-id="e66ae-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span></span>](https://github.com/aspnet-contrib/AspNet.Security.OpenIdConnect.Server) | <span data-ttu-id="e66ae-111">ASOS は、低レベルで、プロトコル最初 OpenID Connect サーバー フレームワーク用 ASP.NET Core と OWIN/Katana です。</span><span class="sxs-lookup"><span data-stu-id="e66ae-111">ASOS is a low-level, protocol-first OpenID Connect server framework for ASP.NET Core and OWIN/Katana.</span></span> |
| [<span data-ttu-id="e66ae-112">Cierge</span><span class="sxs-lookup"><span data-stu-id="e66ae-112">Cierge</span></span>](https://github.com/pwdless/Cierge) | <span data-ttu-id="e66ae-113">Cierge は、ユーザーのサインアップ、ログイン、プロファイル、管理、およびソーシャル ログインを処理する OpenID Connect のサーバーです。</span><span class="sxs-lookup"><span data-stu-id="e66ae-113">Cierge is an OpenID Connect server that handles user signup, login, profiles, management, and social logins.</span></span> |
| [<span data-ttu-id="e66ae-114">Gluu サーバー</span><span class="sxs-lookup"><span data-stu-id="e66ae-114">Gluu Server</span></span>](https://gluu.org/) | <span data-ttu-id="e66ae-115">エンタープライズ向け、オープン ソース ソフトウェアは、id の管理 (IAM)、およびシングル サインオン (SSO) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e66ae-115">Enterprise ready, open source software for identity, access management (IAM), and single sign-on (SSO).</span></span> <span data-ttu-id="e66ae-116">詳細については、次を参照してください。、 [Gluu 製品ドキュメント](https://gluu.org/docs/)です。</span><span class="sxs-lookup"><span data-stu-id="e66ae-116">For more information, see the [Gluu Product Documentation](https://gluu.org/docs/).</span></span> |
| [<span data-ttu-id="e66ae-117">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="e66ae-117">IdentityServer</span></span>](https://identityserver.io/) | <span data-ttu-id="e66ae-118">IdentityServer は、公式に認定 OpenID Foundation と .NET Foundation の管理下にある ASP.NET Core の OpenID Connect および OAuth 2.0 フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="e66ae-118">IdentityServer is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core, officially certified by the OpenID Foundation and under governance of the .NET Foundation.</span></span> <span data-ttu-id="e66ae-119">詳細については、次を参照してください。 [IdentityServer4 へようこそ (ドキュメント)](https://identityserver4.readthedocs.io/en/release/)です。</span><span class="sxs-lookup"><span data-stu-id="e66ae-119">For more information, see [Welcome to IdentityServer4 (Documentation)](https://identityserver4.readthedocs.io/en/release/).</span></span> |
| [<span data-ttu-id="e66ae-120">OpenIddict</span><span class="sxs-lookup"><span data-stu-id="e66ae-120">OpenIddict</span></span>](https://github.com/openiddict/openiddict-core) | <span data-ttu-id="e66ae-121">OpenIddict は、ASP.NET Core の使いやすい OpenID Connect サーバーです。</span><span class="sxs-lookup"><span data-stu-id="e66ae-121">OpenIddict is an easy-to-use OpenID Connect server for ASP.NET Core.</span></span> |

<span data-ttu-id="e66ae-122">プロバイダーを追加する[このページの編集](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md)です。</span><span class="sxs-lookup"><span data-stu-id="e66ae-122">To add a provider, [edit this page](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span></span>
