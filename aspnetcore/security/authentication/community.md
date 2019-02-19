---
title: ASP.NET Core のコミュニティ OSS 認証オプション
author: rick-anderson
description: ASP.NET Core のオープン ソース認証オプションを説明します。
ms.author: riande
ms.date: 02/15/2019
uid: security/authentication/community
ms.openlocfilehash: e25df794bdff8f904382e7a299755ae4c23b892e
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410285"
---
# <a name="community-oss-authentication-options-for-aspnet-core"></a><span data-ttu-id="db77a-103">ASP.NET Core のコミュニティ OSS 認証オプション</span><span class="sxs-lookup"><span data-stu-id="db77a-103">Community OSS authentication options for ASP.NET Core</span></span>

<span data-ttu-id="db77a-104">このページには、ASP.NET Core のコミュニティで提供される、オープン ソース認証オプションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="db77a-104">This page contains community-provided, open source authentication options for ASP.NET Core.</span></span> <span data-ttu-id="db77a-105">このページは、新しいプロバイダーが使用可能になると定期的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="db77a-105">This page is periodically updated as new providers become available.</span></span>

## <a name="oss-authentication-providers"></a><span data-ttu-id="db77a-106">OSS 認証プロバイダー</span><span class="sxs-lookup"><span data-stu-id="db77a-106">OSS authentication providers</span></span>

<span data-ttu-id="db77a-107">以下の一覧がアルファベット順に並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="db77a-107">The list below is sorted alphabetically.</span></span>

| <span data-ttu-id="db77a-108">名前</span><span class="sxs-lookup"><span data-stu-id="db77a-108">Name</span></span> | <span data-ttu-id="db77a-109">説明</span><span class="sxs-lookup"><span data-stu-id="db77a-109">Description</span></span> |
| ---- | ----------- |
| [<span data-ttu-id="db77a-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span><span class="sxs-lookup"><span data-stu-id="db77a-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span></span>](https://github.com/aspnet-contrib/AspNet.Security.OpenIdConnect.Server) | <span data-ttu-id="db77a-111">ASOS は、低レベルでプロトコル最初 OpenID Connect サーバー フレームワークの ASP.NET Core と Owin/katana です。</span><span class="sxs-lookup"><span data-stu-id="db77a-111">ASOS is a low-level, protocol-first OpenID Connect server framework for ASP.NET Core and OWIN/Katana.</span></span> |
| [<span data-ttu-id="db77a-112">Cierge</span><span class="sxs-lookup"><span data-stu-id="db77a-112">Cierge</span></span>](https://github.com/pwdless/Cierge) | <span data-ttu-id="db77a-113">Cierge とは、OpenID Connect ユーザーのサインアップ、ログイン、プロファイル、管理、およびソーシャル ログインを処理するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="db77a-113">Cierge is an OpenID Connect server that handles user signup, login, profiles, management, and social logins.</span></span> |
| [<span data-ttu-id="db77a-114">Gluu サーバー</span><span class="sxs-lookup"><span data-stu-id="db77a-114">Gluu Server</span></span>](https://gluu.org/) | <span data-ttu-id="db77a-115">エンタープライズ対応の場合、id、ソースのソフトウェアを開いて管理 (IAM)、およびシングル サインオン (SSO) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="db77a-115">Enterprise ready, open source software for identity, access management (IAM), and single sign-on (SSO).</span></span> <span data-ttu-id="db77a-116">詳細については、次を参照してください。、 [Gluu 製品ドキュメント](https://gluu.org/docs/)します。</span><span class="sxs-lookup"><span data-stu-id="db77a-116">For more information, see the [Gluu Product Documentation](https://gluu.org/docs/).</span></span> |
| [<span data-ttu-id="db77a-117">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="db77a-117">IdentityServer</span></span>](https://identityserver.io/) | <span data-ttu-id="db77a-118">IdentityServer は、正式に認定 OpenID foundation と .NET Foundation の管理下で、ASP.NET Core 用 OpenID Connect と OAuth 2.0 フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="db77a-118">IdentityServer is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core, officially certified by the OpenID Foundation and under governance of the .NET Foundation.</span></span> <span data-ttu-id="db77a-119">詳細については、次を参照してください。 [IdentityServer4 へようこそ (ドキュメント)](https://identityserver4.readthedocs.io/en/latest/)します。</span><span class="sxs-lookup"><span data-stu-id="db77a-119">For more information, see [Welcome to IdentityServer4 (Documentation)](https://identityserver4.readthedocs.io/en/latest/).</span></span> |
| [<span data-ttu-id="db77a-120">OpenIddict</span><span class="sxs-lookup"><span data-stu-id="db77a-120">OpenIddict</span></span>](https://github.com/openiddict/openiddict-core) | <span data-ttu-id="db77a-121">OpenIddict とは、ASP.NET Core の使いやすい OpenID Connect サーバーです。</span><span class="sxs-lookup"><span data-stu-id="db77a-121">OpenIddict is an easy-to-use OpenID Connect server for ASP.NET Core.</span></span> |

<span data-ttu-id="db77a-122">プロバイダーを追加する[このページの編集](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md)します。</span><span class="sxs-lookup"><span data-stu-id="db77a-122">To add a provider, [edit this page](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span></span>
