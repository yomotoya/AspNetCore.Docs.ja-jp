---
title: 他の認証プロバイダーの簡単な調査
author: rick-anderson
manager: wpickett
ms.author: riande
ms.date: 11/03/2016
ms.prod: asp.net-core
ms.topic: article
uid: security/authentication/otherlogins
ms.openlocfilehash: 25c88ae2fdd210c827f3f71d90b1bcca164d86af
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729520"
---
# <a name="short-survey-of-other-authentication-providers"></a><span data-ttu-id="93941-102">他の認証プロバイダーの簡単な調査</span><span class="sxs-lookup"><span data-stu-id="93941-102">Short survey of other authentication providers</span></span>

<a name="security-authentication-other-logins"></a>

<span data-ttu-id="93941-103">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Pranav Rastogi](https://github.com/rustd)、および[Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="93941-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="93941-104">その他の一般的な OAuth プロバイダーに指示をここで設定されます。</span><span class="sxs-lookup"><span data-stu-id="93941-104">Here are set up instructions for some other common OAuth providers.</span></span> <span data-ttu-id="93941-105">NuGet パッケージによって管理されるものなどのサード パーティ製[aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) ASP.NET Core チームが実装されている認証プロバイダーを補完するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="93941-105">Third-party NuGet packages such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) can be used to complement authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="93941-106">セットアップ**LinkedIn**サインイン: [ https://www.linkedin.com/developer/apps](https://www.linkedin.com/developer/apps)です。</span><span class="sxs-lookup"><span data-stu-id="93941-106">Set up **LinkedIn** sign in: [https://www.linkedin.com/developer/apps](https://www.linkedin.com/developer/apps).</span></span> <span data-ttu-id="93941-107">参照してください[公式手順](https://developer.linkedin.com/docs/oauth2)です。</span><span class="sxs-lookup"><span data-stu-id="93941-107">See [official steps](https://developer.linkedin.com/docs/oauth2).</span></span>

* <span data-ttu-id="93941-108">セットアップ**Instagram**サインイン: [ https://www.instagram.com/developer/register/](https://www.instagram.com/developer/register/)です。</span><span class="sxs-lookup"><span data-stu-id="93941-108">Set up **Instagram** sign in: [https://www.instagram.com/developer/register/](https://www.instagram.com/developer/register/).</span></span> <span data-ttu-id="93941-109">参照してください[公式手順](https://www.instagram.com/developer/authentication/)です。</span><span class="sxs-lookup"><span data-stu-id="93941-109">See [official steps](https://www.instagram.com/developer/authentication/).</span></span>

* <span data-ttu-id="93941-110">セットアップ**Reddit**サインイン: [ https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps)です。</span><span class="sxs-lookup"><span data-stu-id="93941-110">Set up **Reddit** sign in: [https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps).</span></span> <span data-ttu-id="93941-111">参照してください[公式手順](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example)です。</span><span class="sxs-lookup"><span data-stu-id="93941-111">See [official steps](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span></span>

* <span data-ttu-id="93941-112">セットアップ**Github**サインイン: [ https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew)です。</span><span class="sxs-lookup"><span data-stu-id="93941-112">Set up **Github** sign in: [https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew).</span></span> <span data-ttu-id="93941-113">参照してください[公式手順](https://developer.github.com/v3/oauth/)です。</span><span class="sxs-lookup"><span data-stu-id="93941-113">See [official steps](https://developer.github.com/v3/oauth/).</span></span>

* <span data-ttu-id="93941-114">セットアップ**Yahoo**サインイン: [ https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F)です。</span><span class="sxs-lookup"><span data-stu-id="93941-114">Set up **Yahoo** sign in: [https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F).</span></span> <span data-ttu-id="93941-115">参照してください[公式手順](https://developer.yahoo.com/bbauth/user.html)です。</span><span class="sxs-lookup"><span data-stu-id="93941-115">See [official steps](https://developer.yahoo.com/bbauth/user.html).</span></span>

* <span data-ttu-id="93941-116">セットアップ**Tumblr**サインイン: [ https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps)です。</span><span class="sxs-lookup"><span data-stu-id="93941-116">Set up **Tumblr** sign in: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span></span> <span data-ttu-id="93941-117">参照してください[公式手順](https://www.tumblr.com/docs/api/v2#auth)です。</span><span class="sxs-lookup"><span data-stu-id="93941-117">See [official steps](https://www.tumblr.com/docs/api/v2#auth).</span></span>

* <span data-ttu-id="93941-118">セットアップ**Pinterest**サインイン: [ https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F)です。</span><span class="sxs-lookup"><span data-stu-id="93941-118">Set up **Pinterest** sign in: [https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F).</span></span> <span data-ttu-id="93941-119">参照してください[公式手順](https://developers.pinterest.com/docs/api/overview/?)です。</span><span class="sxs-lookup"><span data-stu-id="93941-119">See [official steps](https://developers.pinterest.com/docs/api/overview/?).</span></span>

* <span data-ttu-id="93941-120">セットアップ**Pocket**サインイン: [ https://getpocket.com/developer/apps/new](https://getpocket.com/developer/apps/new)です。</span><span class="sxs-lookup"><span data-stu-id="93941-120">Set up **Pocket** sign in: [https://getpocket.com/developer/apps/new](https://getpocket.com/developer/apps/new).</span></span> <span data-ttu-id="93941-121">参照してください[公式手順](https://getpocket.com/developer/docs/authentication)です。</span><span class="sxs-lookup"><span data-stu-id="93941-121">See [official steps](https://getpocket.com/developer/docs/authentication).</span></span>

* <span data-ttu-id="93941-122">セットアップ**Flickr**サインイン: [ https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create)です。</span><span class="sxs-lookup"><span data-stu-id="93941-122">Set up **Flickr** sign in: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span></span> <span data-ttu-id="93941-123">参照してください[公式手順](https://www.flickr.com/services/api/auth.oauth.html)です。</span><span class="sxs-lookup"><span data-stu-id="93941-123">See [official steps](https://www.flickr.com/services/api/auth.oauth.html).</span></span>

* <span data-ttu-id="93941-124">セットアップ**Dribble**サインイン: [ https://dribbble.com/signup](https://dribbble.com/signup)です。</span><span class="sxs-lookup"><span data-stu-id="93941-124">Set up **Dribble** sign in: [https://dribbble.com/signup](https://dribbble.com/signup).</span></span> <span data-ttu-id="93941-125">参照してください[公式手順](http://developer.dribbble.com/v1/oauth/)です。</span><span class="sxs-lookup"><span data-stu-id="93941-125">See [official steps](http://developer.dribbble.com/v1/oauth/).</span></span>

* <span data-ttu-id="93941-126">セットアップ**Vimeo**サインイン: [ https://vimeo.com/join](https://vimeo.com/join)です。</span><span class="sxs-lookup"><span data-stu-id="93941-126">Set up **Vimeo** sign in: [https://vimeo.com/join](https://vimeo.com/join).</span></span> <span data-ttu-id="93941-127">参照してください[公式手順](https://developer.vimeo.com/api/authentication)です。</span><span class="sxs-lookup"><span data-stu-id="93941-127">See [official steps](https://developer.vimeo.com/api/authentication).</span></span>

* <span data-ttu-id="93941-128">セットアップ**SoundCloud**サインイン: [ https://soundcloud.com/you/apps/new](https://soundcloud.com/you/apps/new)です。</span><span class="sxs-lookup"><span data-stu-id="93941-128">Set up **SoundCloud** sign in: [https://soundcloud.com/you/apps/new](https://soundcloud.com/you/apps/new).</span></span> <span data-ttu-id="93941-129">参照してください[公式手順](https://developers.soundcloud.com/blog/we-love-oauth-2)です。</span><span class="sxs-lookup"><span data-stu-id="93941-129">See [official steps](https://developers.soundcloud.com/blog/we-love-oauth-2).</span></span>

* <span data-ttu-id="93941-130">セットアップ**VK**サインイン: [ https://vk.com/apps?act=manage](https://vk.com/apps?act=manage)です。</span><span class="sxs-lookup"><span data-stu-id="93941-130">Set up **VK** sign in: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span></span> <span data-ttu-id="93941-131">参照してください[公式手順](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites)です。</span><span class="sxs-lookup"><span data-stu-id="93941-131">See [official steps](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span></span>

## <a name="multiple-authentication-providers"></a><span data-ttu-id="93941-132">複数の認証プロバイダー</span><span class="sxs-lookup"><span data-stu-id="93941-132">Multiple authentication providers</span></span>

[!INCLUDE[](~/includes/chain-auth-providers.md)]
