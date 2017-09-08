---
title: "他の認証プロバイダーの簡単な調査します。"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 11/3/2016
ms.topic: article
ms.assetid: BC36CA84-3DE8-496E-9AA2-2F1B74AE8309
ms.prod: asp.net-core
uid: security/authentication/otherlogins
ms.openlocfilehash: 421db8c89e01cebba0c1f98cc9286288a75777f2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="short-survey-of-other-authentication-providers"></a><span data-ttu-id="fa75e-102">他の認証プロバイダーの簡単な調査</span><span class="sxs-lookup"><span data-stu-id="fa75e-102">Short survey of other authentication providers</span></span>

<a name=security-authentication-other-logins></a>

<span data-ttu-id="fa75e-103">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Pranav Rastogi](https://github.com/rustd)、および[Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="fa75e-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="fa75e-104">その他の一般的な OAuth プロバイダーに指示をここで設定されます。</span><span class="sxs-lookup"><span data-stu-id="fa75e-104">Here are set up instructions for some other common OAuth providers.</span></span> <span data-ttu-id="fa75e-105">NuGet パッケージによって管理されるものなどのサード パーティ製[aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) ASP.NET Core チームが実装されている認証プロバイダーを補完するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa75e-105">Third-party NuGet packages such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) can be used to complement authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="fa75e-106">セットアップ**LinkedIn**サインイン: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-106">Set up **LinkedIn** sign in: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps).</span></span> <span data-ttu-id="fa75e-107">参照してください[公式手順](https://developer.linkedin.com/docs/oauth2)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-107">See [official steps](https://developer.linkedin.com/docs/oauth2).</span></span>

* <span data-ttu-id="fa75e-108">セットアップ**Instagram**サインイン: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-108">Set up **Instagram** sign in: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage).</span></span> <span data-ttu-id="fa75e-109">参照してください[公式手順](https://www.instagram.com/developer/authentication/)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-109">See [official steps](https://www.instagram.com/developer/authentication/).</span></span>

* <span data-ttu-id="fa75e-110">セットアップ**Reddit**サインイン: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-110">Set up **Reddit** sign in: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps).</span></span> <span data-ttu-id="fa75e-111">参照してください[公式手順](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-111">See [official steps](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span></span>

* <span data-ttu-id="fa75e-112">セットアップ**Github**サインイン: [https://github.com/settings/applications/new](https://github.com/settings/applications/new)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-112">Set up **Github** sign in: [https://github.com/settings/applications/new](https://github.com/settings/applications/new).</span></span> <span data-ttu-id="fa75e-113">参照してください[公式手順](https://developer.github.com/v3/oauth/)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-113">See [official steps](https://developer.github.com/v3/oauth/).</span></span>

* <span data-ttu-id="fa75e-114">セットアップ**Yahoo**サインイン: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-114">Set up **Yahoo** sign in: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/).</span></span> <span data-ttu-id="fa75e-115">参照してください[公式手順](https://developer.yahoo.com/bbauth/user.html)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-115">See [official steps](https://developer.yahoo.com/bbauth/user.html).</span></span>

* <span data-ttu-id="fa75e-116">セットアップ**Tumblr**サインイン: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-116">Set up **Tumblr** sign in: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span></span> <span data-ttu-id="fa75e-117">参照してください[公式手順](https://www.tumblr.com/docs/en/api/v2#auth)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-117">See [official steps](https://www.tumblr.com/docs/en/api/v2#auth).</span></span>

* <span data-ttu-id="fa75e-118">セットアップ**Pinterest**サインイン: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-118">Set up **Pinterest** sign in: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps).</span></span> <span data-ttu-id="fa75e-119">参照してください[公式手順](https://developers.pinterest.com/docs/api/overview/?)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-119">See [official steps](https://developers.pinterest.com/docs/api/overview/?).</span></span>

* <span data-ttu-id="fa75e-120">セットアップ**Pocket**サインイン: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-120">Set up **Pocket** sign in: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new).</span></span> <span data-ttu-id="fa75e-121">参照してください[公式手順](https://getpocket.com/developer/docs/authentication)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-121">See [official steps](https://getpocket.com/developer/docs/authentication).</span></span>

* <span data-ttu-id="fa75e-122">セットアップ**Flickr**サインイン: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-122">Set up **Flickr** sign in: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span></span> <span data-ttu-id="fa75e-123">参照してください[公式手順](https://www.flickr.com/services/api/auth.oauth.html)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-123">See [official steps](https://www.flickr.com/services/api/auth.oauth.html).</span></span>

* <span data-ttu-id="fa75e-124">セットアップ**Dribble**サインイン: [https://dribbble.com/signup](https://dribbble.com/signup)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-124">Set up **Dribble** sign in: [https://dribbble.com/signup](https://dribbble.com/signup).</span></span> <span data-ttu-id="fa75e-125">参照してください[公式手順](http://developer.dribbble.com/v1/oauth/)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-125">See [official steps](http://developer.dribbble.com/v1/oauth/).</span></span>

* <span data-ttu-id="fa75e-126">セットアップ**Vimeo**サインイン: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-126">Set up **Vimeo** sign in: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps).</span></span> <span data-ttu-id="fa75e-127">参照してください[公式手順](https://developer.vimeo.com/api/authentication)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-127">See [official steps](https://developer.vimeo.com/api/authentication).</span></span>

* <span data-ttu-id="fa75e-128">セットアップ**SoundCloud**サインイン: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-128">Set up **SoundCloud** sign in: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new).</span></span> <span data-ttu-id="fa75e-129">参照してください[公式手順](https://developers.soundcloud.com/blog/we-love-oauth-2)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-129">See [official steps](https://developers.soundcloud.com/blog/we-love-oauth-2).</span></span>

* <span data-ttu-id="fa75e-130">セットアップ**VK**サインイン: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-130">Set up **VK** sign in: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span></span> <span data-ttu-id="fa75e-131">参照してください[公式手順](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites)です。</span><span class="sxs-lookup"><span data-stu-id="fa75e-131">See [official steps](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span></span>
