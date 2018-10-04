---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 承認サーバー |Microsoft Docs
author: hongyes
description: このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。 これは、高度なチュートリアルをそののみ outlin です.
ms.author: riande
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 2dd4af4543713ab08ad9427d183f667e2dc04f1f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578043"
---
<a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="2d806-104">OWIN OAuth 2.0 承認サーバー</span><span class="sxs-lookup"><span data-stu-id="2d806-104">OWIN OAuth 2.0 Authorization Server</span></span>
====================
<span data-ttu-id="2d806-105">によって[Hongye Sun](https://github.com/hongyes)、 [Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="2d806-105">by [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="2d806-106">このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2d806-106">This tutorial will guide you on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span> <span data-ttu-id="2d806-107">これは、のみ OWIN OAuth 2.0 承認サーバーを作成する手順が説明されている、高度なチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="2d806-107">This is an advanced tutorial that only outlines the steps to create an OWIN OAuth 2.0 Authorization Server.</span></span> <span data-ttu-id="2d806-108">これは、ステップ バイ ステップ チュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="2d806-108">This is not a step by step tutorial.</span></span> <span data-ttu-id="2d806-109">[サンプル コードをダウンロード](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)します。</span><span class="sxs-lookup"><span data-stu-id="2d806-109">[Download the sample code](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="2d806-110">このアウトラインは、セキュリティで保護された実稼働アプリを作成するために使用されるされないものする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d806-110">This outline should not be intended to be used for creating a secure production app.</span></span> <span data-ttu-id="2d806-111">このチュートリアルの目的は、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法のアウトラインのみを提供します。</span><span class="sxs-lookup"><span data-stu-id="2d806-111">This tutorial is intended to provide only an outline on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span>
> 
> 
> ## <a name="software-versions"></a><span data-ttu-id="2d806-112">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2d806-112">Software versions</span></span>
> 
> | <span data-ttu-id="2d806-113">**このチュートリアルで示すように**</span><span class="sxs-lookup"><span data-stu-id="2d806-113">**Shown in the tutorial**</span></span> | <span data-ttu-id="2d806-114">**でも使用できます。**</span><span class="sxs-lookup"><span data-stu-id="2d806-114">**Also works with**</span></span> |
> | --- | --- |
> | <span data-ttu-id="2d806-115">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="2d806-115">Windows 8.1</span></span> | <span data-ttu-id="2d806-116">Windows 8、Windows 7</span><span class="sxs-lookup"><span data-stu-id="2d806-116">Windows 8, Windows 7</span></span> |
> | [<span data-ttu-id="2d806-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2d806-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads) | <span data-ttu-id="2d806-118">[Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)します。</span><span class="sxs-lookup"><span data-stu-id="2d806-118">[Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express).</span></span> <span data-ttu-id="2d806-119">Visual Studio 2012 と最新の更新プログラムが動作する必要がありますが、このチュートリアルは、テストされていないといくつかのメニューとダイアログ ボックスが異なる。</span><span class="sxs-lookup"><span data-stu-id="2d806-119">Visual Studio 2012 with the latest update should work, but the tutorial has not been tested with it, and some menu selections and dialog boxes are different.</span></span> |
> | <span data-ttu-id="2d806-120">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2d806-120">.NET 4.5</span></span> |  |
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2d806-121">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="2d806-121">Questions and Comments</span></span>
> 
> <span data-ttu-id="2d806-122">チュートリアルに直接関連付けられていない質問がある場合でこれらを投稿[Katana プロジェクト github](https://github.com/aspnet/AspNetKatana/)します。</span><span class="sxs-lookup"><span data-stu-id="2d806-122">If you have questions that are not directly related to the tutorial, you can post them at [Katana Project on GitHub](https://github.com/aspnet/AspNetKatana/).</span></span> <span data-ttu-id="2d806-123">チュートリアルに関する意見やご質問は、ページの下部にあるコメント セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2d806-123">For questions and comments regarding the tutorial itself, see the comments section at the bottom of the page.</span></span>


<span data-ttu-id="2d806-124">[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) HTTP サービスへのアクセス制限を取得するサード パーティ製アプリができるようにします。</span><span class="sxs-lookup"><span data-stu-id="2d806-124">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="2d806-125">保護されたリソースにアクセスするリソース所有者の資格情報を使用する代わりに、クライアントがアクセス トークンを取得します (これは、文字列、特定のスコープ、有効期間、およびその他のアクセス属性を示す)。</span><span class="sxs-lookup"><span data-stu-id="2d806-125">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="2d806-126">アクセス トークンは、リソース所有者の承認をサード パーティ製のクライアントに、承認サーバーによって発行されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-126">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="2d806-127">このチュートリアルを取り上げます。</span><span class="sxs-lookup"><span data-stu-id="2d806-127">This tutorial will cover:</span></span>

- <span data-ttu-id="2d806-128">次の 4 つの承認をサポートする承認サーバーを作成する方法と更新トークンを付与タイプ。</span><span class="sxs-lookup"><span data-stu-id="2d806-128">How to create an authorization server to support four authorization grant types and refresh tokens:</span></span>
    - <span data-ttu-id="2d806-129">認証コード付与</span><span class="sxs-lookup"><span data-stu-id="2d806-129">Authorization code grant</span></span>
    - <span data-ttu-id="2d806-130">暗黙的な許可</span><span class="sxs-lookup"><span data-stu-id="2d806-130">Implicit Grant</span></span>
    - <span data-ttu-id="2d806-131">リソース所有者のパスワード資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="2d806-131">Resource Owner Password Credentials Grant</span></span>
    - <span data-ttu-id="2d806-132">クライアントの資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="2d806-132">Client Credentials Grant</span></span>
- <span data-ttu-id="2d806-133">アクセス トークンで保護されたリソース サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-133">Creating a resource server which is protected by an access token.</span></span>
- <span data-ttu-id="2d806-134">OAuth 2.0 クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-134">Creating OAuth 2.0 clients.</span></span>

<a id="prerequisites"></a>
## <a name="prerequisites"></a><span data-ttu-id="2d806-135">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2d806-135">Prerequisites</span></span>

- <span data-ttu-id="2d806-136">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions)または無料[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)に記載されている、**ソフトウェア バージョン**ページの上部にあります。</span><span class="sxs-lookup"><span data-stu-id="2d806-136">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) or the free [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), as indicated in **Software Versions** at the top of the page.</span></span>
- <span data-ttu-id="2d806-137">OWIN の知識。</span><span class="sxs-lookup"><span data-stu-id="2d806-137">Familiarity with OWIN.</span></span> <span data-ttu-id="2d806-138">参照してください[Katana プロジェクトの概要](https://msdn.microsoft.com/magazine/dn451439.aspx)と[OWIN と Katana の新](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="2d806-138">See [Getting Started with the Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx) and [What's new in OWIN and Katana](index.md).</span></span>
- <span data-ttu-id="2d806-139">知識[OAuth](http://tools.ietf.org/html/rfc6749)用語では、含む[ロール](http://tools.ietf.org/html/rfc6749#section-1.1)、[プロトコル フロー](http://tools.ietf.org/html/rfc6749#section-1.2)と[承認付与](http://tools.ietf.org/html/rfc6749#section-1.3)します。</span><span class="sxs-lookup"><span data-stu-id="2d806-139">Familiarity with [OAuth](http://tools.ietf.org/html/rfc6749) terminology, including [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [Protocol Flow](http://tools.ietf.org/html/rfc6749#section-1.2), and [Authorization Grant](http://tools.ietf.org/html/rfc6749#section-1.3).</span></span> <span data-ttu-id="2d806-140">[OAuth 2.0 の概要](http://tools.ietf.org/html/rfc6749#section-1)の紹介を提供します。</span><span class="sxs-lookup"><span data-stu-id="2d806-140">[OAuth 2.0 introduction](http://tools.ietf.org/html/rfc6749#section-1) provides a good introduction.</span></span>

## <a name="create-an-authorization-server"></a><span data-ttu-id="2d806-141">承認サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-141">Create an Authorization Server</span></span>

<span data-ttu-id="2d806-142">このチュートリアルでは使用する方法についてはスケッチほぼ[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)と ASP.NET MVC、承認サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-142">In this tutorial, we will roughly sketch out how to use [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) and ASP.NET MVC to create an authorization server.</span></span> <span data-ttu-id="2d806-143">このチュートリアルには、各ステップが含まれていないとすぐに完全なサンプルのダウンロードを提供することと思います。</span><span class="sxs-lookup"><span data-stu-id="2d806-143">We hope to soon provide a download for the completed sample, as this tutorial does not include each step.</span></span> <span data-ttu-id="2d806-144">という名前の空の web アプリを最初に、作成*AuthorizationServer*し、次のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="2d806-144">First, create an empty web app named *AuthorizationServer* and install the following packages:</span></span>

- <span data-ttu-id="2d806-145">Microsoft.AspNet.Mvc</span><span class="sxs-lookup"><span data-stu-id="2d806-145">Microsoft.AspNet.Mvc</span></span>
- <span data-ttu-id="2d806-146">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="2d806-146">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="2d806-147">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="2d806-147">Microsoft.Owin.Security.OAuth</span></span>
- <span data-ttu-id="2d806-148">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="2d806-148">Microsoft.Owin.Security.Cookies</span></span>
- <span data-ttu-id="2d806-149">Microsoft.Owin.Security.Google (または Microsoft.Owin.Security.Facebook などその他の任意のソーシャル ログインをパッケージ化)</span><span class="sxs-lookup"><span data-stu-id="2d806-149">Microsoft.Owin.Security.Google (Or any other social login package such as Microsoft.Owin.Security.Facebook)</span></span>

<span data-ttu-id="2d806-150">追加、 [OWIN Startup クラス](owin-startup-class-detection.md)という名前のプロジェクト ルート フォルダーの下*スタートアップ*します。</span><span class="sxs-lookup"><span data-stu-id="2d806-150">Add an [OWIN Startup class](owin-startup-class-detection.md) under the project root folder named *Startup*.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

<span data-ttu-id="2d806-151">作成、*アプリ\_開始*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2d806-151">Create an *App\_Start* folder.</span></span> <span data-ttu-id="2d806-152">選択、*アプリ\_開始*フォルダーとのダウンロードしたバージョンを追加するには Shift + Alt + A を使用して、 *AuthorizationServer\App\_Start\Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2d806-152">Select the *App\_Start* folder and use Shift+Alt+A to add the downloaded version of the *AuthorizationServer\App\_Start\Startup.Auth.cs* file.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

<span data-ttu-id="2d806-153">上記のコードでは、アプリケーション/外部サインイン cookie および Google の認証、承認サーバー自体でアカウントを管理するために使用でを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2d806-153">The code above enables application/external sign in cookies and Google authentication, which are used by authorization server itself to manage accounts.</span></span>

<span data-ttu-id="2d806-154">`UseOAuthAuthorizationServer`拡張メソッドは、承認サーバーをセットアップします。</span><span class="sxs-lookup"><span data-stu-id="2d806-154">The `UseOAuthAuthorizationServer` extension method is to setup the authorization server.</span></span> <span data-ttu-id="2d806-155">セットアップのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="2d806-155">The setup options are:</span></span>

- <span data-ttu-id="2d806-156">`AuthorizeEndpointPath`: クライアント アプリケーションがユーザーを取得するために、ユーザー エージェントをリダイレクト場所の要求パスは、コードまたはトークン発行に同意します。</span><span class="sxs-lookup"><span data-stu-id="2d806-156">`AuthorizeEndpointPath`: The request path where client applications will redirect the user-agent in order to obtain the users consent to issue a token or code.</span></span> <span data-ttu-id="2d806-157">たとえば、先頭のスラッシュで始める必要があります"`/Authorize`"。</span><span class="sxs-lookup"><span data-stu-id="2d806-157">It must begin with a leading slash, for example, "`/Authorize`".</span></span>
- <span data-ttu-id="2d806-158">`TokenEndpointPath`要求パスのクライアント アプリケーションは、アクセス トークンを取得する直接通信します。</span><span class="sxs-lookup"><span data-stu-id="2d806-158">`TokenEndpointPath`: The request path client applications directly communicate to obtain the access token.</span></span> <span data-ttu-id="2d806-159">"/Token"のように、先頭にスラッシュが始まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d806-159">It must begin with a leading slash, like "/Token".</span></span> <span data-ttu-id="2d806-160">クライアントが発行されている場合、[クライアント\_シークレット](http://tools.ietf.org/html/rfc6749#appendix-A.2)、このエンドポイントに指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d806-160">If the client is issued a [client\_secret](http://tools.ietf.org/html/rfc6749#appendix-A.2), it must be provided to this endpoint.</span></span>
- <span data-ttu-id="2d806-161">`ApplicationCanDisplayErrors`: に設定します。 `true` web アプリケーションがクライアントの検証エラーのカスタム エラー ページを生成する必要がある場合`/Authorize`エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="2d806-161">`ApplicationCanDisplayErrors`: Set to `true` if the web application wants to generate a custom error page for the client validation errors on `/Authorize` endpoint.</span></span> <span data-ttu-id="2d806-162">たとえばバックアップ、クライアント アプリケーションに、ブラウザーがリダイレクトしない場合にのみ必要です、ときに、`client_id`または`redirect_uri`が正しくありません。</span><span class="sxs-lookup"><span data-stu-id="2d806-162">This is only needed for cases where the browser is not redirected back to the client application, for example, when the `client_id` or `redirect_uri` are incorrect.</span></span> <span data-ttu-id="2d806-163">`/Authorize`エンドポイントが"oauth を表示することが予想されます。エラー"、"oauth します。ErrorDescription"、および"oauth します。ErrorUri"プロパティは、OWIN 環境に追加されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-163">The `/Authorize` endpoint should expect to see the "oauth.Error", "oauth.ErrorDescription", and "oauth.ErrorUri" properties are added to the OWIN environment.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="2d806-164">指定しない場合、true の場合、承認サーバーは既定のエラー ページとエラーの詳細が返すされます。</span><span class="sxs-lookup"><span data-stu-id="2d806-164">If not true, the authorization server will return a default error page with the error details.</span></span>
- <span data-ttu-id="2d806-165">`AllowInsecureHttp`: 承認し、HTTP URI アドレスに到着して、受信を許可するトークンが要求を許可する True を`redirect_uri`要求パラメーターが HTTP URI アドレスを承認します。</span><span class="sxs-lookup"><span data-stu-id="2d806-165">`AllowInsecureHttp`: True to allow authorize and token requests to arrive on HTTP URI addresses, and to allow incoming `redirect_uri` authorize request parameters to have HTTP URI addresses.</span></span> 

    > [!WARNING]
    > <span data-ttu-id="2d806-166">セキュリティ - これは開発専用の。</span><span class="sxs-lookup"><span data-stu-id="2d806-166">Security - This is for development only.</span></span>
- <span data-ttu-id="2d806-167">`Provider`: 認証サーバー ミドルウェアによって発生したイベントを処理するアプリケーションによって提供されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="2d806-167">`Provider`: The object provided by the application to process events raised by the Authorization Server middleware.</span></span> <span data-ttu-id="2d806-168">アプリケーションが完全には、インターフェイスを実装することがありますまたはのインスタンスを作成することがあります`OAuthAuthorizationServerProvider`OAuth フローがこのサーバーをサポートするために必要なデリゲートを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="2d806-168">The application may implement the interface fully, or it may create an instance of `OAuthAuthorizationServerProvider` and assign delegates necessary for the OAuth flows this server supports.</span></span>
- <span data-ttu-id="2d806-169">`AuthorizationCodeProvider`: クライアント アプリケーションに返される単一使用承認コードを生成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-169">`AuthorizationCodeProvider`: Produces a single-use authorization code to return to the client application.</span></span> <span data-ttu-id="2d806-170">OAuth サーバーは、アプリケーションをセキュリティで保護**する必要があります**のインスタンスを指定`AuthorizationCodeProvider`によってトークンが生成される、`OnCreate/OnCreateAsync`イベントが 1 つだけの呼び出しの有効と見なさ`OnReceive/OnReceiveAsync`します。</span><span class="sxs-lookup"><span data-stu-id="2d806-170">For the OAuth server to be secure the application **MUST** provide an instance for `AuthorizationCodeProvider` where the token produced by the `OnCreate/OnCreateAsync` event is considered valid for only one call to `OnReceive/OnReceiveAsync`.</span></span>
- <span data-ttu-id="2d806-171">`RefreshTokenProvider`: 必要なときに、新しいアクセス トークンを生成するために使用できる更新トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-171">`RefreshTokenProvider`: Produces a refresh token which may be used to produce a new access token when needed.</span></span> <span data-ttu-id="2d806-172">かどうかは指定されていない、承認サーバーは返されませんから更新トークン、`/Token`エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="2d806-172">If not provided the authorization server will not return refresh tokens from the `/Token` endpoint.</span></span>

## <a name="account-management"></a><span data-ttu-id="2d806-173">アカウントの管理</span><span class="sxs-lookup"><span data-stu-id="2d806-173">Account Management</span></span>

<span data-ttu-id="2d806-174">OAuth は、ユーザー アカウント情報の管理や場所、方法に関係ありません。</span><span class="sxs-lookup"><span data-stu-id="2d806-174">OAuth doesn't care where or how you manage your user account information.</span></span> <span data-ttu-id="2d806-175">[ASP.NET Identity](../../../identity/index.md)はその責任を負います。</span><span class="sxs-lookup"><span data-stu-id="2d806-175">It's [ASP.NET Identity](../../../identity/index.md) which is responsible for it.</span></span> <span data-ttu-id="2d806-176">このチュートリアルでは、アカウント管理コードを簡略化し、だけそのユーザーがログインする OWIN クッキー ミドルウェアを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2d806-176">In this tutorial, we will simplify the account management code and just make sure that user can login using OWIN cookie middleware.</span></span> <span data-ttu-id="2d806-177">次の簡略化されたサンプル コードに示します、 `AccountController`:</span><span class="sxs-lookup"><span data-stu-id="2d806-177">Here is the simplified sample code for the `AccountController`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

<span data-ttu-id="2d806-178">`ValidateClientRedirectUri` その登録されたリダイレクト URL を使用してクライアントの検証に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-178">`ValidateClientRedirectUri` is used to validate the client with its registered redirect URL.</span></span> <span data-ttu-id="2d806-179">`ValidateClientAuthentication` 基本的なスキームのヘッダーとクライアントの資格情報を取得するフォームの本文を確認します。</span><span class="sxs-lookup"><span data-stu-id="2d806-179">`ValidateClientAuthentication` checks the basic scheme header and form body to get the client's credentials.</span></span>

<span data-ttu-id="2d806-180">ログイン ページは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="2d806-180">The login page is shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image1.png)

<span data-ttu-id="2d806-181">IETF の OAuth 2 の確認[認証コード付与](http://tools.ietf.org/html/rfc6749#section-4.1)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="2d806-181">Review the IETF's OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) section now.</span></span> 

<span data-ttu-id="2d806-182">**プロバイダー** (次の表では) 機能は[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)します。型のプロバイダー、 `OAuthAuthorizationServerProvider`、OAuth サーバーのすべてのイベントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d806-182">**Provider** (in the table below) is [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx).Provider, which is of type `OAuthAuthorizationServerProvider`, which contains all OAuth server events.</span></span> 

| <span data-ttu-id="2d806-183">認証コード付与セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="2d806-183">Flow steps from Authorization Code Grant section</span></span> | <span data-ttu-id="2d806-184">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-184">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2d806-185">(A)、クライアントは、リソース所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。</span><span class="sxs-lookup"><span data-stu-id="2d806-185">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="2d806-186">クライアントには、クライアント識別子、要求されたスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェントへのアクセスが許可 (または拒否された) 後のリダイレクト URI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d806-186">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="2d806-187">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="2d806-187">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="2d806-188">(B)、承認サーバーは、(ユーザー エージェント) を使用して、リソースの所有者を認証し、リソース所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。</span><span class="sxs-lookup"><span data-stu-id="2d806-188">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="2d806-189">**&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2d806-189">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2d806-190">(承認サーバーは、リダイレクト URI を使用してクライアントにユーザー エージェントをリダイレクトのアクセスを許可 C)、リソースの所有者と仮定した場合 (要求またはクライアント登録時に) 以前。</span><span class="sxs-lookup"><span data-stu-id="2d806-190">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="2d806-191">...</span><span class="sxs-lookup"><span data-stu-id="2d806-191">...</span></span> |  |
|  |  |
| <span data-ttu-id="2d806-192">(D)、クライアントは、前の手順で受け取った承認コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="2d806-192">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="2d806-193">要求を行うときに、クライアントは承認サーバーで認証されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-193">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="2d806-194">クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d806-194">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> | <span data-ttu-id="2d806-195">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2d806-195">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |

<span data-ttu-id="2d806-196">実装サンプル`AuthorizationCodeProvider.CreateAsync`と`ReceiveAsync`を作成し、認証コードの検証の制御を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="2d806-196">A sample implementation for `AuthorizationCodeProvider.CreateAsync` and `ReceiveAsync` to control the creation and validation of authorization code is shown below.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

<span data-ttu-id="2d806-197">上記のコードでは、コードと id のチケットを格納して、コードを受信した後は、id を復元するメモリ内の同時実行ディクショナリを使用します。</span><span class="sxs-lookup"><span data-stu-id="2d806-197">The code above uses an in-memory concurrent dictionary to store the code and identity ticket and restore the identity after receiving the code.</span></span> <span data-ttu-id="2d806-198">実際のアプリケーションで永続的なデータ ストアによって置き換えが。</span><span class="sxs-lookup"><span data-stu-id="2d806-198">In a real application, it would be replaced by a persistent data store.</span></span> <span data-ttu-id="2d806-199">承認エンドポイントは、クライアントへのアクセス許可リソースの所有者です。</span><span class="sxs-lookup"><span data-stu-id="2d806-199">The authorization endpoint is for the resource owner to grant access to the client.</span></span> <span data-ttu-id="2d806-200">通常、ユーザー インターフェイスにボタンをクリックし、許可を確認します。 ユーザーを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d806-200">Usually, it needs a user interface to allow the user to click a button and confirm the grant.</span></span> <span data-ttu-id="2d806-201">OWIN OAuth のミドルウェアは、承認エンドポイントを処理するためにアプリケーション コードを利用します。</span><span class="sxs-lookup"><span data-stu-id="2d806-201">OWIN OAuth middleware allows application code to handle the authorization endpoint.</span></span> <span data-ttu-id="2d806-202">このサンプル アプリでと呼ばれる MVC コント ローラーを使用して`OAuthController`を処理します。</span><span class="sxs-lookup"><span data-stu-id="2d806-202">In our sample app, we use an MVC controller called `OAuthController` to handle it.</span></span> <span data-ttu-id="2d806-203">サンプル実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2d806-203">Here is the sample implementation:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

<span data-ttu-id="2d806-204">`Authorize`アクションでは、ユーザーがログインに承認サーバーへのかどうかはまず調べます。</span><span class="sxs-lookup"><span data-stu-id="2d806-204">The `Authorize` action will first check if the user has logged in to the authorization server.</span></span> <span data-ttu-id="2d806-205">存在しない場合、認証ミドルウェアの課題を呼び出し元「アプリケーション」の cookie を使用して認証をログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="2d806-205">If not, the authentication middleware challenges the caller to authenticate using the "Application" cookie and redirects to the login page.</span></span> <span data-ttu-id="2d806-206">(上記の強調表示されたコードを参照してください)。ユーザーがログインに場合、次に示すよう、承認のビューがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="2d806-206">(See highlighted code above.) If user has logged in, it will render the Authorize view, as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image2.png)

<span data-ttu-id="2d806-207">場合、 **Grant**ボタンが選択されている、`Authorize`アクションは、新しい"Bearer"の id と、サインインを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-207">If the **Grant** button is selected, the `Authorize` action will create a new "Bearer" identity and sign in with it.</span></span> <span data-ttu-id="2d806-208">ベアラー トークンを生成し、JSON ペイロードをクライアントに送信する承認サーバーのトリガーとなります。</span><span class="sxs-lookup"><span data-stu-id="2d806-208">It will trigger the authorization server to generate a bearer token and send it back to the client with JSON payload.</span></span> 

### <a name="implicit-grant"></a><span data-ttu-id="2d806-209">暗黙的な許可</span><span class="sxs-lookup"><span data-stu-id="2d806-209">Implicit Grant</span></span>

<span data-ttu-id="2d806-210">IETF の OAuth 2 を参照してください[Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="2d806-210">Refer to the IETF's OAuth 2 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) section now.</span></span>

 <span data-ttu-id="2d806-211">[Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)フロー図 4 は、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="2d806-211">The [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) flow shown in Figure 4 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2d806-212">暗黙的な許可 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="2d806-212">Flow steps from Implicit Grant section</span></span> | <span data-ttu-id="2d806-213">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-213">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2d806-214">(A)、クライアントは、リソース所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。</span><span class="sxs-lookup"><span data-stu-id="2d806-214">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="2d806-215">クライアントには、クライアント識別子、要求されたスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェントへのアクセスが許可 (または拒否された) 後のリダイレクト URI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d806-215">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="2d806-216">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="2d806-216">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="2d806-217">(B)、承認サーバーは、(ユーザー エージェント) を使用して、リソースの所有者を認証し、リソース所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。</span><span class="sxs-lookup"><span data-stu-id="2d806-217">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="2d806-218">**&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2d806-218">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2d806-219">(承認サーバーは、リダイレクト URI を使用してクライアントにユーザー エージェントをリダイレクトのアクセスを許可 C)、リソースの所有者と仮定した場合 (要求またはクライアント登録時に) 以前。</span><span class="sxs-lookup"><span data-stu-id="2d806-219">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="2d806-220">...</span><span class="sxs-lookup"><span data-stu-id="2d806-220">...</span></span> |  |
|  |  |
| <span data-ttu-id="2d806-221">(D)、クライアントは、前の手順で受け取った承認コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="2d806-221">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="2d806-222">要求を行うときに、クライアントは承認サーバーで認証されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-222">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="2d806-223">クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2d806-223">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> |  |

<span data-ttu-id="2d806-224">承認エンドポイントを既に実装されていますので (`OAuthController.Authorize`アクション) の認証コード付与では、それを自動的に有効暗黙的フローもします。</span><span class="sxs-lookup"><span data-stu-id="2d806-224">Since we already implemented the authorization endpoint (`OAuthController.Authorize` action) for authorization code grant, it automatically enables implicit flow as well.</span></span> <span data-ttu-id="2d806-225">注:`Provider.ValidateClientRedirectUri`送信アクセス トークンを悪意のあるクライアントからの暗黙的な許可フローは、保護、そのリダイレクト URL にクライアント ID を検証するために使用 ([で中間者攻撃](https://www.owasp.org/index.php/Man-in-the-middle_attack))。</span><span class="sxs-lookup"><span data-stu-id="2d806-225">Note: `Provider.ValidateClientRedirectUri` is used to validate the client ID with its redirection URL, which protects the implicit grant flow from sending the access token to malicious clients ([Man-in-the-middle attack](https://www.owasp.org/index.php/Man-in-the-middle_attack)).</span></span>

### <a name="resource-owner-password-credentials-grant"></a><span data-ttu-id="2d806-226">リソース所有者のパスワード資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="2d806-226">Resource Owner Password Credentials Grant</span></span>

<span data-ttu-id="2d806-227">IETF の OAuth 2 を参照してください[リソース所有者パスワード資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.3)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="2d806-227">Refer to the IETF's OAuth 2 [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) section now.</span></span>

 <span data-ttu-id="2d806-228">[リソース所有者パスワード資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.3)フロー図 5 に示すは、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="2d806-228">The [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) flow shown in Figure 5 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2d806-229">リソース所有者パスワード資格情報の付与 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="2d806-229">Flow steps from Resource Owner Password Credentials Grant section</span></span> | <span data-ttu-id="2d806-230">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-230">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2d806-231">(A) リソースの所有者では、そのユーザー名とパスワード、クライアントを提供します。</span><span class="sxs-lookup"><span data-stu-id="2d806-231">(A) The resource owner provides the client with its username and password.</span></span> |  |
|  |  |
| <span data-ttu-id="2d806-232">(B)、クライアントは、リソース所有者から受け取った資格情報を含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="2d806-232">(B) The client requests an access token from the authorization server's token endpoint by including the credentials received from the resource owner.</span></span> <span data-ttu-id="2d806-233">要求を行うときに、クライアントは承認サーバーで認証されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-233">When making the request, the client authenticates with the authorization server.</span></span> | <span data-ttu-id="2d806-234">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2d806-234">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2d806-235">(C)、承認サーバーは、クライアントを認証し、リソース所有者の資格情報を検証し、有効な場合は、アクセス トークンを発行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-235">(C) The authorization server authenticates the client and validates the resource owner credentials, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="2d806-236">サンプルの実装を次に示します`Provider.GrantResourceOwnerCredentials`:</span><span class="sxs-lookup"><span data-stu-id="2d806-236">Here is the sample implementation for `Provider.GrantResourceOwnerCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> <span data-ttu-id="2d806-237">上記のコード、チュートリアルのこのセクションで説明するものでは、安全では使用する必要がありますまたは実稼働アプリ。</span><span class="sxs-lookup"><span data-stu-id="2d806-237">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="2d806-238">リソース所有者の資格情報は確認しません。</span><span class="sxs-lookup"><span data-stu-id="2d806-238">It does not check the resource owners credentials.</span></span> <span data-ttu-id="2d806-239">すべての資格情報が有効でありの新しい id を作成します。 前提としています。</span><span class="sxs-lookup"><span data-stu-id="2d806-239">It assumes every credential is valid and creates a new identity for it.</span></span> <span data-ttu-id="2d806-240">新しい id は、アクセス トークンを生成し、更新トークンに使用されます。</span><span class="sxs-lookup"><span data-stu-id="2d806-240">The new identity will be used to generate the access token and refresh token.</span></span> <span data-ttu-id="2d806-241">セキュリティで保護されたアカウント管理コードには、コードに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="2d806-241">Please replace the code with your own secure account management code.</span></span>


### <a name="client-credentials-grant"></a><span data-ttu-id="2d806-242">クライアントの資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="2d806-242">Client Credentials Grant</span></span>

<span data-ttu-id="2d806-243">IETF の OAuth 2 を参照してください[クライアント資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.4)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="2d806-243">Refer to the IETF's OAuth 2 [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) section now.</span></span>

 <span data-ttu-id="2d806-244">[クライアント資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.4)フロー図 6 に示すようには、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="2d806-244">The [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) flow shown in Figure 6 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2d806-245">クライアント資格情報の付与 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="2d806-245">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="2d806-246">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-246">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2d806-247">(A)、クライアントは、承認サーバーで認証し、トークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="2d806-247">(A) The client authenticates with the authorization server and requests an access token from the token endpoint.</span></span> | <span data-ttu-id="2d806-248">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2d806-248">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2d806-249">(B)、承認サーバーは、クライアントを認証し、有効な場合は、アクセス トークンを発行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-249">(B) The authorization server authenticates the client, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="2d806-250">サンプルの実装を次に示します`Provider.GrantClientCredentials`:</span><span class="sxs-lookup"><span data-stu-id="2d806-250">Here is the sample implementation for `Provider.GrantClientCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="2d806-251">上記のコード、チュートリアルのこのセクションで説明するものでは、安全では使用する必要がありますまたは実稼働アプリ。</span><span class="sxs-lookup"><span data-stu-id="2d806-251">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="2d806-252">セキュリティで保護されたクライアント管理コードには、コードに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="2d806-252">Please replace the code with your own secure client management code.</span></span>


### <a name="refresh-token"></a><span data-ttu-id="2d806-253">更新トークン</span><span class="sxs-lookup"><span data-stu-id="2d806-253">Refresh Token</span></span>

<span data-ttu-id="2d806-254">IETF の OAuth 2 を参照してください[更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="2d806-254">Refer to the IETF's OAuth 2 [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) section now.</span></span>

 <span data-ttu-id="2d806-255">[更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)フロー図 2 は、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="2d806-255">The [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) flow shown in Figure 2 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>  

| <span data-ttu-id="2d806-256">クライアント資格情報の付与 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="2d806-256">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="2d806-257">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-257">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="2d806-258">(G)、クライアントは、承認サーバーによる認証方法と更新トークンを提示して新しいアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="2d806-258">(G) The client requests a new access token by authenticating with the authorization server and presenting the refresh token.</span></span> <span data-ttu-id="2d806-259">クライアントの認証要件は、クライアントの種類とサーバーの承認ポリシーに基づいています。</span><span class="sxs-lookup"><span data-stu-id="2d806-259">The client authentication requirements are based on the client type and on the authorization server policies.</span></span> | <span data-ttu-id="2d806-260">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="2d806-260">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="2d806-261">(H)、承認サーバーは、クライアントを認証し、更新トークンを検証しますと、有効な場合は、新しいアクセス トークン (および、必要に応じて、新しい更新トークン) を発行します。</span><span class="sxs-lookup"><span data-stu-id="2d806-261">(H) The authorization server authenticates the client and validates the refresh token, and if valid, issues a new access token (and, optionally, a new refresh token).</span></span> |  |

<span data-ttu-id="2d806-262">サンプルの実装を次に示します`Provider.GrantRefreshToken`:</span><span class="sxs-lookup"><span data-stu-id="2d806-262">Here is the sample implementation for `Provider.GrantRefreshToken`:</span></span> 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a><span data-ttu-id="2d806-263">アクセス トークンで保護されたリソース サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-263">Create a Resource Server which is protected by Access Token</span></span>

<span data-ttu-id="2d806-264">空の web アプリ プロジェクトを作成し、次のプロジェクト内のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="2d806-264">Create an empty web app project and install following packages in the project:</span></span>

- <span data-ttu-id="2d806-265">Microsoft.AspNet.WebApi.Owin</span><span class="sxs-lookup"><span data-stu-id="2d806-265">Microsoft.AspNet.WebApi.Owin</span></span>
- <span data-ttu-id="2d806-266">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="2d806-266">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="2d806-267">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="2d806-267">Microsoft.Owin.Security.OAuth</span></span>

<span data-ttu-id="2d806-268">Startup クラスを作成し、認証と Web API を構成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-268">Create a startup class and configure authentication and Web API.</span></span> <span data-ttu-id="2d806-269">参照してください*AuthorizationServer\ResourceServer\Startup.cs*サンプル ダウンロードにします。</span><span class="sxs-lookup"><span data-stu-id="2d806-269">See *AuthorizationServer\ResourceServer\Startup.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

<span data-ttu-id="2d806-270">参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*サンプル ダウンロードにします。</span><span class="sxs-lookup"><span data-stu-id="2d806-270">See *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

<span data-ttu-id="2d806-271">参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*サンプル ダウンロードにします。</span><span class="sxs-lookup"><span data-stu-id="2d806-271">See *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- <span data-ttu-id="2d806-272">`UseCors` メソッドは、すべてのドメインに対して CORS を使用します。</span><span class="sxs-lookup"><span data-stu-id="2d806-272">`UseCors` method allows CORS for all domains.</span></span>
- <span data-ttu-id="2d806-273">`UseOAuthBearerAuthentication` メソッドは、OAuth ベアラー トークンの認証ミドルウェアが受信し、要求の承認ヘッダーからのベアラー トークンを検証できます。</span><span class="sxs-lookup"><span data-stu-id="2d806-273">`UseOAuthBearerAuthentication` method enables OAuth bearer token authentication middleware which will receive and validate bearer token from authorization header in the request.</span></span>
- <span data-ttu-id="2d806-274">`Config.SuppressDefaultHostAuthenticaiton` 既定の抑制、アプリからの認証済みプリンシパルのホスト、この呼び出しの後に匿名してすべての要求があるためです。</span><span class="sxs-lookup"><span data-stu-id="2d806-274">`Config.SuppressDefaultHostAuthenticaiton` suppresses default host authenticated principal from the app, therefore all requests will be anonymous after this call.</span></span>
- <span data-ttu-id="2d806-275">`HostAuthenticationFilter` だけ、指定した認証の種類の認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2d806-275">`HostAuthenticationFilter` enables authentication just for the specified authentication type.</span></span> <span data-ttu-id="2d806-276">この場合は、ベアラー認証の種類を勧めします。</span><span class="sxs-lookup"><span data-stu-id="2d806-276">In this case, it's bearer authentication type.</span></span>

<span data-ttu-id="2d806-277">認証済み id を示すためには、現在のユーザーの要求を出力する、ApiController を作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-277">In order to demonstrate the authenticated identity, we create an ApiController to output current user's claims.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

<span data-ttu-id="2d806-278">承認サーバーと、リソース サーバーが同じコンピューター上にない場合は、OAuth ミドルウェアは暗号化ベアラー アクセス トークンを復号化し、別のマシン キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2d806-278">If the authorization server and the resource server are not on the same computer, the OAuth middleware will use the different machine keys to encrypt and decrypt bearer access token.</span></span> <span data-ttu-id="2d806-279">同じ両方のプロジェクト間で同じ秘密キーを共有するには追加`machinekey`両方で設定*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2d806-279">In order to share the same private key between both projects, we add the same `machinekey` setting in both *web.config* files.</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a><span data-ttu-id="2d806-280">OAuth 2.0 クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2d806-280">Create OAuth 2.0 Clients</span></span>

 <span data-ttu-id="2d806-281">使用して、 [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet パッケージをクライアント コードを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="2d806-281">We use the [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet package to simplify the client code.</span></span>

### <a name="authorization-code-grant-client"></a><span data-ttu-id="2d806-282">承認コード付与クライアント</span><span class="sxs-lookup"><span data-stu-id="2d806-282">Authorization Code Grant Client</span></span>

 <span data-ttu-id="2d806-283">このクライアントは、MVC アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="2d806-283">This client is an MVC application.</span></span> <span data-ttu-id="2d806-284">バックエンドからアクセス トークンを取得するには、認証コード付与フローのトリガーとなります。</span><span class="sxs-lookup"><span data-stu-id="2d806-284">It will trigger an authorization code grant flow to get the access token from backend.</span></span> <span data-ttu-id="2d806-285">次に示す 1 つのページがあります。</span><span class="sxs-lookup"><span data-stu-id="2d806-285">It has a single page as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image3.png)

- <span data-ttu-id="2d806-286">**Authorize**ボタンは、このクライアントにアクセスを許可するリソースの所有者に通知する承認サーバーへのブラウザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="2d806-286">The **Authorize** button will redirect browser to the authorization server to notify the resource owner to grant access to this client.</span></span>
- <span data-ttu-id="2d806-287">**更新**ボタンはトークンを取得、新しいアクセス トークンと更新トークンが現在の更新を使用します。</span><span class="sxs-lookup"><span data-stu-id="2d806-287">The **Refresh** button will get a new access token and refresh token using the current refresh token.</span></span>
- <span data-ttu-id="2d806-288">**アクセスの保護されたリソースの API**ボタンは現在のユーザーの信頼性情報データを取得し、それらをページに表示するリソース サーバーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2d806-288">The **Access Protected Resource API** button will call the resource server to get current user's claims data and show them on the page.</span></span>

<span data-ttu-id="2d806-289">次のサンプル コードに示します、`HomeController`クライアント。</span><span class="sxs-lookup"><span data-stu-id="2d806-289">Here is the sample code of the `HomeController` of the client.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

<span data-ttu-id="2d806-290">`DotNetOpenAuth` 既定で SSL が必要です。</span><span class="sxs-lookup"><span data-stu-id="2d806-290">`DotNetOpenAuth` requires SSL by default.</span></span> <span data-ttu-id="2d806-291">追加する必要があるため、このデモは、HTTP を使用して、次の構成ファイルの設定。</span><span class="sxs-lookup"><span data-stu-id="2d806-291">Since our demo is using HTTP, you need to add following setting in the config file:</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> <span data-ttu-id="2d806-292">セキュリティの運用アプリで無効 SSL ことはありません。</span><span class="sxs-lookup"><span data-stu-id="2d806-292">Security - Never disable SSL in a production app.</span></span> <span data-ttu-id="2d806-293">ログイン資格情報は、ネットワーク経由でクリア テキストで送信されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="2d806-293">Your login credentials are now being sent in clear-text across the wire.</span></span> <span data-ttu-id="2d806-294">上記のコードは、ローカル サンプルのデバッグと探索のためだけです。</span><span class="sxs-lookup"><span data-stu-id="2d806-294">The code above is just for local sample debugging and exploration.</span></span>


### <a name="implicit-grant-client"></a><span data-ttu-id="2d806-295">暗黙的な許可クライアント</span><span class="sxs-lookup"><span data-stu-id="2d806-295">Implicit Grant Client</span></span>

<span data-ttu-id="2d806-296">このクライアントは、JavaScript を使用してください。</span><span class="sxs-lookup"><span data-stu-id="2d806-296">This client is using JavaScript to:</span></span>

1. <span data-ttu-id="2d806-297">新しいウィンドウを開き、承認サーバーの承認エンドポイントにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="2d806-297">Open a new window and redirect to the authorize endpoint of the Authorization Server.</span></span>
2. <span data-ttu-id="2d806-298">アクセス トークンを取得 URL フラグメントからリダイレクト戻るときにします。</span><span class="sxs-lookup"><span data-stu-id="2d806-298">Get the access token from URL fragments when it redirects back.</span></span>

<span data-ttu-id="2d806-299">次の図は、このプロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="2d806-299">The following image shows this process:</span></span>

![](owin-oauth-20-authorization-server/_static/image4.png)

<span data-ttu-id="2d806-300">クライアントが 2 つのページにある必要があります。 ホーム ページで、コールバックのもう 1 つ。JavaScript のコード サンプルを次に示します、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2d806-300">The client should have two pages: one for home page and the other for callback.Here is the sample JavaScript code found in the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

<span data-ttu-id="2d806-301">処理コードにコールバックを次に示します*SignIn.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2d806-301">Here is the callback handling code in *SignIn.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> <span data-ttu-id="2d806-302">JavaScript を外部ファイルに移動し、Razor マークアップを埋め込まないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2d806-302">A best practice is to move the JavaScript to an external file and not embed it with the Razor markup.</span></span> <span data-ttu-id="2d806-303">このサンプルをシンプルにするが統合されました。</span><span class="sxs-lookup"><span data-stu-id="2d806-303">To keep this sample simple, they have been combined.</span></span>


### <a name="resource-owner-password-credentials-grant-client"></a><span data-ttu-id="2d806-304">リソース所有者のパスワードの資格情報付与クライアント</span><span class="sxs-lookup"><span data-stu-id="2d806-304">Resource Owner Password Credentials Grant Client</span></span>

<span data-ttu-id="2d806-305">このクライアントをデモするため、コンソール アプリを使用しています。</span><span class="sxs-lookup"><span data-stu-id="2d806-305">We uses a console app to demo this client.</span></span> <span data-ttu-id="2d806-306">次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="2d806-306">Here is the code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a><span data-ttu-id="2d806-307">クライアント資格情報付与</span><span class="sxs-lookup"><span data-stu-id="2d806-307">Client Credentials Grant Client</span></span>

<span data-ttu-id="2d806-308">リソース所有者パスワード資格情報の付与と同様の場合、ここで、コンソール アプリのコードを示します。</span><span class="sxs-lookup"><span data-stu-id="2d806-308">Similar to the Resource Owner Password Credentials Grant, here is console app code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
