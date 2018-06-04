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
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729520"
---
# <a name="short-survey-of-other-authentication-providers"></a>他の認証プロバイダーの簡単な調査

<a name="security-authentication-other-logins"></a>

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Pranav Rastogi](https://github.com/rustd)、および[Valeriy Novytskyy](https://github.com/01binary)

その他の一般的な OAuth プロバイダーに指示をここで設定されます。 NuGet パッケージによって管理されるものなどのサード パーティ製[aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) ASP.NET Core チームが実装されている認証プロバイダーを補完するために使用できます。

* セットアップ**LinkedIn**サインイン: [ https://www.linkedin.com/developer/apps](https://www.linkedin.com/developer/apps)です。 参照してください[公式手順](https://developer.linkedin.com/docs/oauth2)です。

* セットアップ**Instagram**サインイン: [ https://www.instagram.com/developer/register/](https://www.instagram.com/developer/register/)です。 参照してください[公式手順](https://www.instagram.com/developer/authentication/)です。

* セットアップ**Reddit**サインイン: [ https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps)です。 参照してください[公式手順](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example)です。

* セットアップ**Github**サインイン: [ https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew)です。 参照してください[公式手順](https://developer.github.com/v3/oauth/)です。

* セットアップ**Yahoo**サインイン: [ https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F)です。 参照してください[公式手順](https://developer.yahoo.com/bbauth/user.html)です。

* セットアップ**Tumblr**サインイン: [ https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps)です。 参照してください[公式手順](https://www.tumblr.com/docs/api/v2#auth)です。

* セットアップ**Pinterest**サインイン: [ https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F)です。 参照してください[公式手順](https://developers.pinterest.com/docs/api/overview/?)です。

* セットアップ**Pocket**サインイン: [ https://getpocket.com/developer/apps/new](https://getpocket.com/developer/apps/new)です。 参照してください[公式手順](https://getpocket.com/developer/docs/authentication)です。

* セットアップ**Flickr**サインイン: [ https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create)です。 参照してください[公式手順](https://www.flickr.com/services/api/auth.oauth.html)です。

* セットアップ**Dribble**サインイン: [ https://dribbble.com/signup](https://dribbble.com/signup)です。 参照してください[公式手順](http://developer.dribbble.com/v1/oauth/)です。

* セットアップ**Vimeo**サインイン: [ https://vimeo.com/join](https://vimeo.com/join)です。 参照してください[公式手順](https://developer.vimeo.com/api/authentication)です。

* セットアップ**SoundCloud**サインイン: [ https://soundcloud.com/you/apps/new](https://soundcloud.com/you/apps/new)です。 参照してください[公式手順](https://developers.soundcloud.com/blog/we-love-oauth-2)です。

* セットアップ**VK**サインイン: [ https://vk.com/apps?act=manage](https://vk.com/apps?act=manage)です。 参照してください[公式手順](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites)です。

## <a name="multiple-authentication-providers"></a>複数の認証プロバイダー

[!INCLUDE[](~/includes/chain-auth-providers.md)]
