---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 承認サーバー |Microsoft Docs
author: hongyes
description: このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。 これは、高度なチュートリアルをそののみ outlin です.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: b8451d2d9e346bd5e2f51ba45e48030a5221b549
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667649"
---
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="612fd-104">OWIN OAuth 2.0 承認サーバー</span><span class="sxs-lookup"><span data-stu-id="612fd-104">OWIN OAuth 2.0 Authorization Server</span></span>

> <span data-ttu-id="612fd-105">このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="612fd-105">This tutorial will guide you on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span> <span data-ttu-id="612fd-106">これは、のみ OWIN OAuth 2.0 承認サーバーを作成する手順が説明されている、高度なチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="612fd-106">This is an advanced tutorial that only outlines the steps to create an OWIN OAuth 2.0 Authorization Server.</span></span> <span data-ttu-id="612fd-107">これは、ステップ バイ ステップ チュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="612fd-107">This is not a step by step tutorial.</span></span> <span data-ttu-id="612fd-108">[サンプル コードをダウンロード](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)します。</span><span class="sxs-lookup"><span data-stu-id="612fd-108">[Download the sample code](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).</span></span>
>
> > [!NOTE]
> > <span data-ttu-id="612fd-109">このアウトラインは、セキュリティで保護された実稼働アプリを作成するために使用されるされないものする必要があります。</span><span class="sxs-lookup"><span data-stu-id="612fd-109">This outline should not be intended to be used for creating a secure production app.</span></span> <span data-ttu-id="612fd-110">このチュートリアルの目的は、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法のアウトラインのみを提供します。</span><span class="sxs-lookup"><span data-stu-id="612fd-110">This tutorial is intended to provide only an outline on how to implement an OAuth 2.0 Authorization Server using OWIN OAuth middleware.</span></span>
>
>
> ## <a name="software-versions"></a><span data-ttu-id="612fd-111">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="612fd-111">Software versions</span></span>
>
> | <span data-ttu-id="612fd-112">**このチュートリアルで示すように**</span><span class="sxs-lookup"><span data-stu-id="612fd-112">**Shown in the tutorial**</span></span> | <span data-ttu-id="612fd-113">**でも使用できます。**</span><span class="sxs-lookup"><span data-stu-id="612fd-113">**Also works with**</span></span> |
> | --- | --- |
> | <span data-ttu-id="612fd-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="612fd-114">Windows 8.1</span></span> | <span data-ttu-id="612fd-115">Windows 10、Windows 8、Windows 7</span><span class="sxs-lookup"><span data-stu-id="612fd-115">Windows 10, Windows 8, Windows 7</span></span> |
> | [<span data-ttu-id="612fd-116">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="612fd-116">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> | <span data-ttu-id="612fd-117">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="612fd-117">.NET 4.7.2</span></span> |  |
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="612fd-118">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="612fd-118">Questions and comments</span></span>
>
> <span data-ttu-id="612fd-119">チュートリアルに直接関連付けられていない質問がある場合でこれらを投稿[Katana プロジェクト github](https://github.com/aspnet/AspNetKatana/)します。</span><span class="sxs-lookup"><span data-stu-id="612fd-119">If you have questions that are not directly related to the tutorial, you can post them at [Katana Project on GitHub](https://github.com/aspnet/AspNetKatana/).</span></span> <span data-ttu-id="612fd-120">チュートリアルに関する意見やご質問は、ページの下部にあるコメント セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="612fd-120">For questions and comments regarding the tutorial itself, see the comments section at the bottom of the page.</span></span>


<span data-ttu-id="612fd-121">[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) HTTP サービスへのアクセス制限を取得するサード パーティ製アプリができるようにします。</span><span class="sxs-lookup"><span data-stu-id="612fd-121">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="612fd-122">保護されたリソースにアクセスするリソース所有者の資格情報を使用する代わりに、クライアントがアクセス トークンを取得します (これは、文字列、特定のスコープ、有効期間、およびその他のアクセス属性を示す)。</span><span class="sxs-lookup"><span data-stu-id="612fd-122">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="612fd-123">アクセス トークンは、リソース所有者の承認をサード パーティ製のクライアントに、承認サーバーによって発行されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-123">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="612fd-124">このチュートリアルを取り上げます。</span><span class="sxs-lookup"><span data-stu-id="612fd-124">This tutorial will cover:</span></span>

- <span data-ttu-id="612fd-125">次の 4 つの承認をサポートする承認サーバーを作成する方法と更新トークンを付与タイプ。</span><span class="sxs-lookup"><span data-stu-id="612fd-125">How to create an authorization server to support four authorization grant types and refresh tokens:</span></span>
    - <span data-ttu-id="612fd-126">認証コード付与</span><span class="sxs-lookup"><span data-stu-id="612fd-126">Authorization code grant</span></span>
    - <span data-ttu-id="612fd-127">暗黙的な許可</span><span class="sxs-lookup"><span data-stu-id="612fd-127">Implicit Grant</span></span>
    - <span data-ttu-id="612fd-128">リソース所有者のパスワード資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="612fd-128">Resource Owner Password Credentials Grant</span></span>
    - <span data-ttu-id="612fd-129">クライアントの資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="612fd-129">Client Credentials Grant</span></span>
- <span data-ttu-id="612fd-130">アクセス トークンで保護されたリソース サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-130">Creating a resource server which is protected by an access token.</span></span>
- <span data-ttu-id="612fd-131">OAuth 2.0 クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-131">Creating OAuth 2.0 clients.</span></span>

<a id="prerequisites"></a>
## <a name="prerequisites"></a><span data-ttu-id="612fd-132">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="612fd-132">Prerequisites</span></span>

- <span data-ttu-id="612fd-133">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)に記載されている**ソフトウェア バージョン**ページの上部にあります。</span><span class="sxs-lookup"><span data-stu-id="612fd-133">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) as indicated in **Software Versions** at the top of the page.</span></span>
- <span data-ttu-id="612fd-134">OWIN の知識。</span><span class="sxs-lookup"><span data-stu-id="612fd-134">Familiarity with OWIN.</span></span> <span data-ttu-id="612fd-135">参照してください[Katana プロジェクトの概要](https://msdn.microsoft.com/magazine/dn451439.aspx)と[OWIN と Katana の新](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="612fd-135">See [Getting Started with the Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx) and [What's new in OWIN and Katana](index.md).</span></span>
- <span data-ttu-id="612fd-136">知識[OAuth](http://tools.ietf.org/html/rfc6749)用語では、含む[ロール](http://tools.ietf.org/html/rfc6749#section-1.1)、[プロトコル フロー](http://tools.ietf.org/html/rfc6749#section-1.2)と[承認付与](http://tools.ietf.org/html/rfc6749#section-1.3)します。</span><span class="sxs-lookup"><span data-stu-id="612fd-136">Familiarity with [OAuth](http://tools.ietf.org/html/rfc6749) terminology, including [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [Protocol Flow](http://tools.ietf.org/html/rfc6749#section-1.2), and [Authorization Grant](http://tools.ietf.org/html/rfc6749#section-1.3).</span></span> <span data-ttu-id="612fd-137">[OAuth 2.0 の概要](http://tools.ietf.org/html/rfc6749#section-1)の紹介を提供します。</span><span class="sxs-lookup"><span data-stu-id="612fd-137">[OAuth 2.0 introduction](http://tools.ietf.org/html/rfc6749#section-1) provides a good introduction.</span></span>

## <a name="create-an-authorization-server"></a><span data-ttu-id="612fd-138">承認サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-138">Create an Authorization Server</span></span>

<span data-ttu-id="612fd-139">このチュートリアルでは使用する方法についてはスケッチほぼ[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)と ASP.NET MVC、承認サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-139">In this tutorial, we will roughly sketch out how to use [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) and ASP.NET MVC to create an authorization server.</span></span> <span data-ttu-id="612fd-140">このチュートリアルには、各ステップが含まれていないとすぐに完全なサンプルのダウンロードを提供することと思います。</span><span class="sxs-lookup"><span data-stu-id="612fd-140">We hope to soon provide a download for the completed sample, as this tutorial does not include each step.</span></span> <span data-ttu-id="612fd-141">という名前の空の web アプリを最初に、作成*AuthorizationServer*し、次のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="612fd-141">First, create an empty web app named *AuthorizationServer* and install the following packages:</span></span>

- <span data-ttu-id="612fd-142">Microsoft.AspNet.Mvc</span><span class="sxs-lookup"><span data-stu-id="612fd-142">Microsoft.AspNet.Mvc</span></span>
- <span data-ttu-id="612fd-143">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="612fd-143">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="612fd-144">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="612fd-144">Microsoft.Owin.Security.OAuth</span></span>
- <span data-ttu-id="612fd-145">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="612fd-145">Microsoft.Owin.Security.Cookies</span></span>
- <span data-ttu-id="612fd-146">Microsoft.Owin.Security.Google (または Microsoft.Owin.Security.Facebook などその他の任意のソーシャル ログインをパッケージ化)</span><span class="sxs-lookup"><span data-stu-id="612fd-146">Microsoft.Owin.Security.Google (Or any other social login package such as Microsoft.Owin.Security.Facebook)</span></span>

<span data-ttu-id="612fd-147">追加、 [OWIN Startup クラス](owin-startup-class-detection.md)という名前のプロジェクト ルート フォルダーの下*スタートアップ*します。</span><span class="sxs-lookup"><span data-stu-id="612fd-147">Add an [OWIN Startup class](owin-startup-class-detection.md) under the project root folder named *Startup*.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

<span data-ttu-id="612fd-148">作成、*アプリ\_開始*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="612fd-148">Create an *App\_Start* folder.</span></span> <span data-ttu-id="612fd-149">選択、*アプリ\_開始*フォルダーとのダウンロードしたバージョンを追加するには Shift + Alt + A を使用して、 *AuthorizationServer\App\_Start\Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="612fd-149">Select the *App\_Start* folder and use Shift+Alt+A to add the downloaded version of the *AuthorizationServer\App\_Start\Startup.Auth.cs* file.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

<span data-ttu-id="612fd-150">上記のコードでは、アプリケーション/外部サインイン cookie および Google の認証、承認サーバー自体でアカウントを管理するために使用でを使用できます。</span><span class="sxs-lookup"><span data-stu-id="612fd-150">The code above enables application/external sign in cookies and Google authentication, which are used by authorization server itself to manage accounts.</span></span>

<span data-ttu-id="612fd-151">`UseOAuthAuthorizationServer`拡張メソッドは、承認サーバーをセットアップします。</span><span class="sxs-lookup"><span data-stu-id="612fd-151">The `UseOAuthAuthorizationServer` extension method is to setup the authorization server.</span></span> <span data-ttu-id="612fd-152">セットアップのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="612fd-152">The setup options are:</span></span>

- <span data-ttu-id="612fd-153">`AuthorizeEndpointPath`:クライアント アプリケーションがユーザーを取得するために、ユーザー エージェントをリダイレクト場所の要求パスは、コードまたはトークン発行に同意します。</span><span class="sxs-lookup"><span data-stu-id="612fd-153">`AuthorizeEndpointPath`: The request path where client applications will redirect the user-agent in order to obtain the users consent to issue a token or code.</span></span> <span data-ttu-id="612fd-154">たとえば、先頭のスラッシュで始める必要があります"`/Authorize`"。</span><span class="sxs-lookup"><span data-stu-id="612fd-154">It must begin with a leading slash, for example, "`/Authorize`".</span></span>
- <span data-ttu-id="612fd-155">`TokenEndpointPath`:アクセス トークンを取得する要求パスのクライアント アプリケーションが直接通信します。</span><span class="sxs-lookup"><span data-stu-id="612fd-155">`TokenEndpointPath`: The request path client applications directly communicate to obtain the access token.</span></span> <span data-ttu-id="612fd-156">"/Token"のように、先頭にスラッシュが始まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="612fd-156">It must begin with a leading slash, like "/Token".</span></span> <span data-ttu-id="612fd-157">クライアントが発行されている場合、[クライアント\_シークレット](http://tools.ietf.org/html/rfc6749#appendix-A.2)、このエンドポイントに指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="612fd-157">If the client is issued a [client\_secret](http://tools.ietf.org/html/rfc6749#appendix-A.2), it must be provided to this endpoint.</span></span>
- <span data-ttu-id="612fd-158">`ApplicationCanDisplayErrors`:設定`true`web アプリケーションがクライアントの検証エラーのカスタム エラー ページを生成する必要がある場合`/Authorize`エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="612fd-158">`ApplicationCanDisplayErrors`: Set to `true` if the web application wants to generate a custom error page for the client validation errors on `/Authorize` endpoint.</span></span> <span data-ttu-id="612fd-159">たとえばバックアップ、クライアント アプリケーションに、ブラウザーがリダイレクトしない場合にのみ必要です、ときに、`client_id`または`redirect_uri`が正しくありません。</span><span class="sxs-lookup"><span data-stu-id="612fd-159">This is only needed for cases where the browser is not redirected back to the client application, for example, when the `client_id` or `redirect_uri` are incorrect.</span></span> <span data-ttu-id="612fd-160">`/Authorize`エンドポイントが"oauth を表示することが予想されます。エラー"、"oauth します。ErrorDescription"、および"oauth します。ErrorUri"プロパティは、OWIN 環境に追加されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-160">The `/Authorize` endpoint should expect to see the "oauth.Error", "oauth.ErrorDescription", and "oauth.ErrorUri" properties are added to the OWIN environment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="612fd-161">指定しない場合、true の場合、承認サーバーは既定のエラー ページとエラーの詳細が返すされます。</span><span class="sxs-lookup"><span data-stu-id="612fd-161">If not true, the authorization server will return a default error page with the error details.</span></span>
- <span data-ttu-id="612fd-162">`AllowInsecureHttp`:認証して、トークン要求、HTTP URI アドレスに到着して、着信を許可する場合は true`redirect_uri`要求パラメーターが HTTP URI アドレスを承認します。</span><span class="sxs-lookup"><span data-stu-id="612fd-162">`AllowInsecureHttp`: True to allow authorize and token requests to arrive on HTTP URI addresses, and to allow incoming `redirect_uri` authorize request parameters to have HTTP URI addresses.</span></span>

    > [!WARNING]
    > <span data-ttu-id="612fd-163">セキュリティ - これは開発専用の。</span><span class="sxs-lookup"><span data-stu-id="612fd-163">Security - This is for development only.</span></span>
- <span data-ttu-id="612fd-164">`Provider`:承認サーバーのミドルウェアが発生させたイベントを処理するアプリケーションが提供するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="612fd-164">`Provider`: The object provided by the application to process events raised by the Authorization Server middleware.</span></span> <span data-ttu-id="612fd-165">アプリケーションが完全には、インターフェイスを実装することがありますまたはのインスタンスを作成することがあります`OAuthAuthorizationServerProvider`OAuth フローがこのサーバーをサポートするために必要なデリゲートを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="612fd-165">The application may implement the interface fully, or it may create an instance of `OAuthAuthorizationServerProvider` and assign delegates necessary for the OAuth flows this server supports.</span></span>
- <span data-ttu-id="612fd-166">`AuthorizationCodeProvider`:クライアント アプリケーションに返される単一使用承認コードを生成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-166">`AuthorizationCodeProvider`: Produces a single-use authorization code to return to the client application.</span></span> <span data-ttu-id="612fd-167">OAuth サーバーは、アプリケーションをセキュリティで保護**する必要があります**のインスタンスを指定`AuthorizationCodeProvider`によってトークンが生成される、`OnCreate/OnCreateAsync`イベントが 1 つだけの呼び出しの有効と見なさ`OnReceive/OnReceiveAsync`します。</span><span class="sxs-lookup"><span data-stu-id="612fd-167">For the OAuth server to be secure the application **MUST** provide an instance for `AuthorizationCodeProvider` where the token produced by the `OnCreate/OnCreateAsync` event is considered valid for only one call to `OnReceive/OnReceiveAsync`.</span></span>
- <span data-ttu-id="612fd-168">`RefreshTokenProvider`:必要なときに、新しいアクセス トークンを生成するために使用できる更新トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-168">`RefreshTokenProvider`: Produces a refresh token which may be used to produce a new access token when needed.</span></span> <span data-ttu-id="612fd-169">かどうかは指定されていない、承認サーバーは返されませんから更新トークン、`/Token`エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="612fd-169">If not provided the authorization server will not return refresh tokens from the `/Token` endpoint.</span></span>

## <a name="account-management"></a><span data-ttu-id="612fd-170">アカウントの管理</span><span class="sxs-lookup"><span data-stu-id="612fd-170">Account Management</span></span>

<span data-ttu-id="612fd-171">OAuth は、ユーザー アカウント情報の管理や場所、方法に関係ありません。</span><span class="sxs-lookup"><span data-stu-id="612fd-171">OAuth doesn't care where or how you manage your user account information.</span></span> <span data-ttu-id="612fd-172">[ASP.NET Identity](../../../identity/index.md)はその責任を負います。</span><span class="sxs-lookup"><span data-stu-id="612fd-172">It's [ASP.NET Identity](../../../identity/index.md) which is responsible for it.</span></span> <span data-ttu-id="612fd-173">このチュートリアルでは、アカウント管理コードを簡略化し、だけそのユーザーがログインする OWIN クッキー ミドルウェアを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="612fd-173">In this tutorial, we will simplify the account management code and just make sure that user can login using OWIN cookie middleware.</span></span> <span data-ttu-id="612fd-174">次の簡略化されたサンプル コードに示します、 `AccountController`:</span><span class="sxs-lookup"><span data-stu-id="612fd-174">Here is the simplified sample code for the `AccountController`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

<span data-ttu-id="612fd-175">`ValidateClientRedirectUri` その登録されたリダイレクト URL を使用してクライアントの検証に使用されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-175">`ValidateClientRedirectUri` is used to validate the client with its registered redirect URL.</span></span> <span data-ttu-id="612fd-176">`ValidateClientAuthentication` 基本的なスキームのヘッダーとクライアントの資格情報を取得するフォームの本文を確認します。</span><span class="sxs-lookup"><span data-stu-id="612fd-176">`ValidateClientAuthentication` checks the basic scheme header and form body to get the client's credentials.</span></span>

<span data-ttu-id="612fd-177">ログイン ページは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="612fd-177">The login page is shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image1.png)

<span data-ttu-id="612fd-178">IETF の OAuth 2 の確認[認証コード付与](http://tools.ietf.org/html/rfc6749#section-4.1)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="612fd-178">Review the IETF's OAuth 2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) section now.</span></span>

<span data-ttu-id="612fd-179">**プロバイダー** (次の表では) 機能は[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)します。型のプロバイダー、 `OAuthAuthorizationServerProvider`、OAuth サーバーのすべてのイベントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="612fd-179">**Provider** (in the table below) is [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx).Provider, which is of type `OAuthAuthorizationServerProvider`, which contains all OAuth server events.</span></span>

| <span data-ttu-id="612fd-180">認証コード付与セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="612fd-180">Flow steps from Authorization Code Grant section</span></span> | <span data-ttu-id="612fd-181">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-181">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="612fd-182">(A)、クライアントは、リソース所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。</span><span class="sxs-lookup"><span data-stu-id="612fd-182">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="612fd-183">クライアントには、クライアント識別子、要求されたスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェントへのアクセスが許可 (または拒否された) 後のリダイレクト URI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="612fd-183">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="612fd-184">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="612fd-184">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="612fd-185">(B)、承認サーバーは、(ユーザー エージェント) を使用して、リソースの所有者を認証し、リソース所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。</span><span class="sxs-lookup"><span data-stu-id="612fd-185">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="612fd-186">**&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="612fd-186">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="612fd-187">(承認サーバーは、リダイレクト URI を使用してクライアントにユーザー エージェントをリダイレクトのアクセスを許可 C)、リソースの所有者と仮定した場合 (要求またはクライアント登録時に) 以前。</span><span class="sxs-lookup"><span data-stu-id="612fd-187">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="612fd-188">...</span><span class="sxs-lookup"><span data-stu-id="612fd-188">...</span></span> |  |
|  |  |
| <span data-ttu-id="612fd-189">(D)、クライアントは、前の手順で受け取った承認コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="612fd-189">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="612fd-190">要求を行うときに、クライアントは承認サーバーで認証されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-190">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="612fd-191">クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="612fd-191">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> | <span data-ttu-id="612fd-192">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="612fd-192">Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |

<span data-ttu-id="612fd-193">実装サンプル`AuthorizationCodeProvider.CreateAsync`と`ReceiveAsync`を作成し、認証コードの検証の制御を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="612fd-193">A sample implementation for `AuthorizationCodeProvider.CreateAsync` and `ReceiveAsync` to control the creation and validation of authorization code is shown below.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

<span data-ttu-id="612fd-194">上記のコードでは、コードと id のチケットを格納して、コードを受信した後は、id を復元するメモリ内の同時実行ディクショナリを使用します。</span><span class="sxs-lookup"><span data-stu-id="612fd-194">The code above uses an in-memory concurrent dictionary to store the code and identity ticket and restore the identity after receiving the code.</span></span> <span data-ttu-id="612fd-195">実際のアプリケーションで永続的なデータ ストアによって置き換えが。</span><span class="sxs-lookup"><span data-stu-id="612fd-195">In a real application, it would be replaced by a persistent data store.</span></span> <span data-ttu-id="612fd-196">承認エンドポイントは、クライアントへのアクセス許可リソースの所有者です。</span><span class="sxs-lookup"><span data-stu-id="612fd-196">The authorization endpoint is for the resource owner to grant access to the client.</span></span> <span data-ttu-id="612fd-197">通常、ユーザー インターフェイスにボタンをクリックし、許可を確認します。 ユーザーを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="612fd-197">Usually, it needs a user interface to allow the user to click a button and confirm the grant.</span></span> <span data-ttu-id="612fd-198">OWIN OAuth のミドルウェアは、承認エンドポイントを処理するためにアプリケーション コードを利用します。</span><span class="sxs-lookup"><span data-stu-id="612fd-198">OWIN OAuth middleware allows application code to handle the authorization endpoint.</span></span> <span data-ttu-id="612fd-199">このサンプル アプリでと呼ばれる MVC コント ローラーを使用して`OAuthController`を処理します。</span><span class="sxs-lookup"><span data-stu-id="612fd-199">In our sample app, we use an MVC controller called `OAuthController` to handle it.</span></span> <span data-ttu-id="612fd-200">サンプル実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="612fd-200">Here is the sample implementation:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

<span data-ttu-id="612fd-201">`Authorize`アクションでは、ユーザーがログインに承認サーバーへのかどうかはまず調べます。</span><span class="sxs-lookup"><span data-stu-id="612fd-201">The `Authorize` action will first check if the user has logged in to the authorization server.</span></span> <span data-ttu-id="612fd-202">存在しない場合、認証ミドルウェアの課題を呼び出し元「アプリケーション」の cookie を使用して認証をログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="612fd-202">If not, the authentication middleware challenges the caller to authenticate using the "Application" cookie and redirects to the login page.</span></span> <span data-ttu-id="612fd-203">(上記の強調表示されたコードを参照してください)。ユーザーがログインに場合、次に示すよう、承認のビューがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="612fd-203">(See highlighted code above.) If user has logged in, it will render the Authorize view, as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image2.png)

<span data-ttu-id="612fd-204">場合、 **Grant**ボタンが選択されている、`Authorize`アクションは、新しい"Bearer"の id と、サインインを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-204">If the **Grant** button is selected, the `Authorize` action will create a new "Bearer" identity and sign in with it.</span></span> <span data-ttu-id="612fd-205">ベアラー トークンを生成し、JSON ペイロードをクライアントに送信する承認サーバーのトリガーとなります。</span><span class="sxs-lookup"><span data-stu-id="612fd-205">It will trigger the authorization server to generate a bearer token and send it back to the client with JSON payload.</span></span>

### <a name="implicit-grant"></a><span data-ttu-id="612fd-206">暗黙的な許可</span><span class="sxs-lookup"><span data-stu-id="612fd-206">Implicit Grant</span></span>

<span data-ttu-id="612fd-207">IETF の OAuth 2 を参照してください[Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="612fd-207">Refer to the IETF's OAuth 2 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) section now.</span></span>

 <span data-ttu-id="612fd-208">[Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)フロー図 4 は、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="612fd-208">The [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) flow shown in Figure 4 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="612fd-209">暗黙的な許可 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="612fd-209">Flow steps from Implicit Grant section</span></span> | <span data-ttu-id="612fd-210">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-210">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="612fd-211">(A)、クライアントは、リソース所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。</span><span class="sxs-lookup"><span data-stu-id="612fd-211">(A) The client initiates the flow by directing the resource owner's user-agent to the authorization endpoint.</span></span> <span data-ttu-id="612fd-212">クライアントには、クライアント識別子、要求されたスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェントへのアクセスが許可 (または拒否された) 後のリダイレクト URI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="612fd-212">The client includes its client identifier, requested scope, local state, and a redirection URI to which the authorization server will send the user-agent back once access is granted (or denied).</span></span> | <span data-ttu-id="612fd-213">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span><span class="sxs-lookup"><span data-stu-id="612fd-213">Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint</span></span> |
|  |  |
| <span data-ttu-id="612fd-214">(B)、承認サーバーは、(ユーザー エージェント) を使用して、リソースの所有者を認証し、リソース所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。</span><span class="sxs-lookup"><span data-stu-id="612fd-214">(B) The authorization server authenticates the resource owner (via the user-agent) and establishes whether the resource owner grants or denies the client's access request.</span></span> | <span data-ttu-id="612fd-215">**&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="612fd-215">**&lt;If user grants access&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="612fd-216">(承認サーバーは、リダイレクト URI を使用してクライアントにユーザー エージェントをリダイレクトのアクセスを許可 C)、リソースの所有者と仮定した場合 (要求またはクライアント登録時に) 以前。</span><span class="sxs-lookup"><span data-stu-id="612fd-216">(C) Assuming the resource owner grants access, the authorization server redirects the user-agent back to the client using the redirection URI provided earlier (in the request or during client registration).</span></span> <span data-ttu-id="612fd-217">...</span><span class="sxs-lookup"><span data-stu-id="612fd-217">...</span></span> |  |
|  |  |
| <span data-ttu-id="612fd-218">(D)、クライアントは、前の手順で受け取った承認コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="612fd-218">(D) The client requests an access token from the authorization server's token endpoint by including the authorization code received in the previous step.</span></span> <span data-ttu-id="612fd-219">要求を行うときに、クライアントは承認サーバーで認証されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-219">When making the request, the client authenticates with the authorization server.</span></span> <span data-ttu-id="612fd-220">クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="612fd-220">The client includes the redirection URI used to obtain the authorization code for verification.</span></span> |  |

<span data-ttu-id="612fd-221">承認エンドポイントを既に実装されていますので (`OAuthController.Authorize`アクション) の認証コード付与では、それを自動的に有効暗黙的フローもします。</span><span class="sxs-lookup"><span data-stu-id="612fd-221">Since we already implemented the authorization endpoint (`OAuthController.Authorize` action) for authorization code grant, it automatically enables implicit flow as well.</span></span> <span data-ttu-id="612fd-222">注:`Provider.ValidateClientRedirectUri`送信アクセス トークンを悪意のあるクライアントからの暗黙的な許可フローは、保護、そのリダイレクト URL にクライアント ID を検証するために使用 ([で中間者攻撃](https://www.owasp.org/index.php/Man-in-the-middle_attack))。</span><span class="sxs-lookup"><span data-stu-id="612fd-222">Note: `Provider.ValidateClientRedirectUri` is used to validate the client ID with its redirection URL, which protects the implicit grant flow from sending the access token to malicious clients ([Man-in-the-middle attack](https://www.owasp.org/index.php/Man-in-the-middle_attack)).</span></span>

### <a name="resource-owner-password-credentials-grant"></a><span data-ttu-id="612fd-223">リソース所有者のパスワード資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="612fd-223">Resource Owner Password Credentials Grant</span></span>

<span data-ttu-id="612fd-224">IETF の OAuth 2 を参照してください[リソース所有者パスワード資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.3)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="612fd-224">Refer to the IETF's OAuth 2 [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) section now.</span></span>

 <span data-ttu-id="612fd-225">[リソース所有者パスワード資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.3)フロー図 5 に示すは、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="612fd-225">The [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) flow shown in Figure 5 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="612fd-226">リソース所有者パスワード資格情報の付与 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="612fd-226">Flow steps from Resource Owner Password Credentials Grant section</span></span> | <span data-ttu-id="612fd-227">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-227">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="612fd-228">(A) リソースの所有者では、そのユーザー名とパスワード、クライアントを提供します。</span><span class="sxs-lookup"><span data-stu-id="612fd-228">(A) The resource owner provides the client with its username and password.</span></span> |  |
|  |  |
| <span data-ttu-id="612fd-229">(B)、クライアントは、リソース所有者から受け取った資格情報を含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="612fd-229">(B) The client requests an access token from the authorization server's token endpoint by including the credentials received from the resource owner.</span></span> <span data-ttu-id="612fd-230">要求を行うときに、クライアントは承認サーバーで認証されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-230">When making the request, the client authenticates with the authorization server.</span></span> | <span data-ttu-id="612fd-231">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="612fd-231">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="612fd-232">(C)、承認サーバーは、クライアントを認証し、リソース所有者の資格情報を検証し、有効な場合は、アクセス トークンを発行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-232">(C) The authorization server authenticates the client and validates the resource owner credentials, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="612fd-233">サンプルの実装を次に示します`Provider.GrantResourceOwnerCredentials`:</span><span class="sxs-lookup"><span data-stu-id="612fd-233">Here is the sample implementation for `Provider.GrantResourceOwnerCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> <span data-ttu-id="612fd-234">上記のコード、チュートリアルのこのセクションで説明するものでは、安全では使用する必要がありますまたは実稼働アプリ。</span><span class="sxs-lookup"><span data-stu-id="612fd-234">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="612fd-235">リソース所有者の資格情報は確認しません。</span><span class="sxs-lookup"><span data-stu-id="612fd-235">It does not check the resource owners credentials.</span></span> <span data-ttu-id="612fd-236">すべての資格情報が有効でありの新しい id を作成します。 前提としています。</span><span class="sxs-lookup"><span data-stu-id="612fd-236">It assumes every credential is valid and creates a new identity for it.</span></span> <span data-ttu-id="612fd-237">新しい id は、アクセス トークンを生成し、更新トークンに使用されます。</span><span class="sxs-lookup"><span data-stu-id="612fd-237">The new identity will be used to generate the access token and refresh token.</span></span> <span data-ttu-id="612fd-238">セキュリティで保護されたアカウント管理コードには、コードに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="612fd-238">Please replace the code with your own secure account management code.</span></span>


### <a name="client-credentials-grant"></a><span data-ttu-id="612fd-239">クライアントの資格情報の付与</span><span class="sxs-lookup"><span data-stu-id="612fd-239">Client Credentials Grant</span></span>

<span data-ttu-id="612fd-240">IETF の OAuth 2 を参照してください[クライアント資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.4)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="612fd-240">Refer to the IETF's OAuth 2 [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) section now.</span></span>

 <span data-ttu-id="612fd-241">[クライアント資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.4)フロー図 6 に示すようには、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="612fd-241">The [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4) flow shown in Figure 6 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="612fd-242">クライアント資格情報の付与 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="612fd-242">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="612fd-243">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-243">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="612fd-244">(A)、クライアントは、承認サーバーで認証し、トークン エンドポイントからアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="612fd-244">(A) The client authenticates with the authorization server and requests an access token from the token endpoint.</span></span> | <span data-ttu-id="612fd-245">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="612fd-245">Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="612fd-246">(B)、承認サーバーは、クライアントを認証し、有効な場合は、アクセス トークンを発行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-246">(B) The authorization server authenticates the client, and if valid, issues an access token.</span></span> |  |

<span data-ttu-id="612fd-247">サンプルの実装を次に示します`Provider.GrantClientCredentials`:</span><span class="sxs-lookup"><span data-stu-id="612fd-247">Here is the sample implementation for `Provider.GrantClientCredentials`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="612fd-248">上記のコード、チュートリアルのこのセクションで説明するものでは、安全では使用する必要がありますまたは実稼働アプリ。</span><span class="sxs-lookup"><span data-stu-id="612fd-248">The code above is intended to explain this section of the tutorial and should not be used in secure or production apps.</span></span> <span data-ttu-id="612fd-249">セキュリティで保護されたクライアント管理コードには、コードに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="612fd-249">Please replace the code with your own secure client management code.</span></span>


### <a name="refresh-token"></a><span data-ttu-id="612fd-250">更新トークン</span><span class="sxs-lookup"><span data-stu-id="612fd-250">Refresh Token</span></span>

<span data-ttu-id="612fd-251">IETF の OAuth 2 を参照してください[更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)セクションのようになりました。</span><span class="sxs-lookup"><span data-stu-id="612fd-251">Refer to the IETF's OAuth 2 [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) section now.</span></span>

 <span data-ttu-id="612fd-252">[更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)フロー図 2 は、フローとマッピングする OWIN OAuth ミドルウェアに依存します。</span><span class="sxs-lookup"><span data-stu-id="612fd-252">The [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) flow shown in Figure 2 is the flow and mapping which the OWIN OAuth middleware follows.</span></span>

| <span data-ttu-id="612fd-253">クライアント資格情報の付与 セクションからフローのステップ</span><span class="sxs-lookup"><span data-stu-id="612fd-253">Flow steps from Client Credentials Grant section</span></span> | <span data-ttu-id="612fd-254">サンプルのダウンロードでは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-254">Sample download performs these steps with:</span></span> |
| --- | --- |
|  |  |
| <span data-ttu-id="612fd-255">(G)、クライアントは、承認サーバーによる認証方法と更新トークンを提示して新しいアクセス トークンを要求します。</span><span class="sxs-lookup"><span data-stu-id="612fd-255">(G) The client requests a new access token by authenticating with the authorization server and presenting the refresh token.</span></span> <span data-ttu-id="612fd-256">クライアントの認証要件は、クライアントの種類とサーバーの承認ポリシーに基づいています。</span><span class="sxs-lookup"><span data-stu-id="612fd-256">The client authentication requirements are based on the client type and on the authorization server policies.</span></span> | <span data-ttu-id="612fd-257">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync</span><span class="sxs-lookup"><span data-stu-id="612fd-257">Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync</span></span> |
|  |  |
| <span data-ttu-id="612fd-258">(H)、承認サーバーは、クライアントを認証し、更新トークンを検証しますと、有効な場合は、新しいアクセス トークン (および、必要に応じて、新しい更新トークン) を発行します。</span><span class="sxs-lookup"><span data-stu-id="612fd-258">(H) The authorization server authenticates the client and validates the refresh token, and if valid, issues a new access token (and, optionally, a new refresh token).</span></span> |  |

<span data-ttu-id="612fd-259">サンプルの実装を次に示します`Provider.GrantRefreshToken`:</span><span class="sxs-lookup"><span data-stu-id="612fd-259">Here is the sample implementation for `Provider.GrantRefreshToken`:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a><span data-ttu-id="612fd-260">アクセス トークンで保護されたリソース サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-260">Create a Resource Server which is protected by Access Token</span></span>

<span data-ttu-id="612fd-261">空の web アプリ プロジェクトを作成し、次のプロジェクト内のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="612fd-261">Create an empty web app project and install following packages in the project:</span></span>

- <span data-ttu-id="612fd-262">Microsoft.AspNet.WebApi.Owin</span><span class="sxs-lookup"><span data-stu-id="612fd-262">Microsoft.AspNet.WebApi.Owin</span></span>
- <span data-ttu-id="612fd-263">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="612fd-263">Microsoft.Owin.Host.SystemWeb</span></span>
- <span data-ttu-id="612fd-264">Microsoft.Owin.Security.OAuth</span><span class="sxs-lookup"><span data-stu-id="612fd-264">Microsoft.Owin.Security.OAuth</span></span>

<span data-ttu-id="612fd-265">Startup クラスを作成し、認証と Web API を構成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-265">Create a startup class and configure authentication and Web API.</span></span> <span data-ttu-id="612fd-266">参照してください*AuthorizationServer\ResourceServer\Startup.cs*サンプル ダウンロードにします。</span><span class="sxs-lookup"><span data-stu-id="612fd-266">See *AuthorizationServer\ResourceServer\Startup.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

<span data-ttu-id="612fd-267">参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*サンプル ダウンロードにします。</span><span class="sxs-lookup"><span data-stu-id="612fd-267">See *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

<span data-ttu-id="612fd-268">参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*サンプル ダウンロードにします。</span><span class="sxs-lookup"><span data-stu-id="612fd-268">See *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* in the sample download.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- <span data-ttu-id="612fd-269">`UseCors` メソッドは、すべてのドメインに対して CORS を使用します。</span><span class="sxs-lookup"><span data-stu-id="612fd-269">`UseCors` method allows CORS for all domains.</span></span>
- <span data-ttu-id="612fd-270">`UseOAuthBearerAuthentication` メソッドは、OAuth ベアラー トークンの認証ミドルウェアが受信し、要求の承認ヘッダーからのベアラー トークンを検証できます。</span><span class="sxs-lookup"><span data-stu-id="612fd-270">`UseOAuthBearerAuthentication` method enables OAuth bearer token authentication middleware which will receive and validate bearer token from authorization header in the request.</span></span>
- <span data-ttu-id="612fd-271">`Config.SuppressDefaultHostAuthenticaiton` 既定の抑制、アプリからの認証済みプリンシパルのホスト、この呼び出しの後に匿名してすべての要求があるためです。</span><span class="sxs-lookup"><span data-stu-id="612fd-271">`Config.SuppressDefaultHostAuthenticaiton` suppresses default host authenticated principal from the app, therefore all requests will be anonymous after this call.</span></span>
- <span data-ttu-id="612fd-272">`HostAuthenticationFilter` だけ、指定した認証の種類の認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="612fd-272">`HostAuthenticationFilter` enables authentication just for the specified authentication type.</span></span> <span data-ttu-id="612fd-273">この場合は、ベアラー認証の種類を勧めします。</span><span class="sxs-lookup"><span data-stu-id="612fd-273">In this case, it's bearer authentication type.</span></span>

<span data-ttu-id="612fd-274">認証済み id を示すためには、現在のユーザーの要求を出力する、ApiController を作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-274">In order to demonstrate the authenticated identity, we create an ApiController to output current user's claims.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

<span data-ttu-id="612fd-275">承認サーバーと、リソース サーバーが同じコンピューター上にない場合は、OAuth ミドルウェアは暗号化ベアラー アクセス トークンを復号化し、別のマシン キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="612fd-275">If the authorization server and the resource server are not on the same computer, the OAuth middleware will use the different machine keys to encrypt and decrypt bearer access token.</span></span> <span data-ttu-id="612fd-276">同じ両方のプロジェクト間で同じ秘密キーを共有するには追加`machinekey`両方で設定*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="612fd-276">In order to share the same private key between both projects, we add the same `machinekey` setting in both *web.config* files.</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a><span data-ttu-id="612fd-277">OAuth 2.0 クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="612fd-277">Create OAuth 2.0 Clients</span></span>

 <span data-ttu-id="612fd-278">使用して、 [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet パッケージをクライアント コードを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="612fd-278">We use the [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet package to simplify the client code.</span></span>

### <a name="authorization-code-grant-client"></a><span data-ttu-id="612fd-279">承認コード付与クライアント</span><span class="sxs-lookup"><span data-stu-id="612fd-279">Authorization Code Grant Client</span></span>

 <span data-ttu-id="612fd-280">このクライアントは、MVC アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="612fd-280">This client is an MVC application.</span></span> <span data-ttu-id="612fd-281">バックエンドからアクセス トークンを取得するには、認証コード付与フローのトリガーとなります。</span><span class="sxs-lookup"><span data-stu-id="612fd-281">It will trigger an authorization code grant flow to get the access token from backend.</span></span> <span data-ttu-id="612fd-282">次に示す 1 つのページがあります。</span><span class="sxs-lookup"><span data-stu-id="612fd-282">It has a single page as shown below:</span></span>

![](owin-oauth-20-authorization-server/_static/image3.png)

- <span data-ttu-id="612fd-283">**Authorize**ボタンは、このクライアントにアクセスを許可するリソースの所有者に通知する承認サーバーへのブラウザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="612fd-283">The **Authorize** button will redirect browser to the authorization server to notify the resource owner to grant access to this client.</span></span>
- <span data-ttu-id="612fd-284">**更新**ボタンはトークンを取得、新しいアクセス トークンと更新トークンが現在の更新を使用します。</span><span class="sxs-lookup"><span data-stu-id="612fd-284">The **Refresh** button will get a new access token and refresh token using the current refresh token.</span></span>
- <span data-ttu-id="612fd-285">**アクセスの保護されたリソースの API**ボタンは現在のユーザーの信頼性情報データを取得し、それらをページに表示するリソース サーバーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="612fd-285">The **Access Protected Resource API** button will call the resource server to get current user's claims data and show them on the page.</span></span>

<span data-ttu-id="612fd-286">次のサンプル コードに示します、`HomeController`クライアント。</span><span class="sxs-lookup"><span data-stu-id="612fd-286">Here is the sample code of the `HomeController` of the client.</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

<span data-ttu-id="612fd-287">`DotNetOpenAuth` 既定で SSL が必要です。</span><span class="sxs-lookup"><span data-stu-id="612fd-287">`DotNetOpenAuth` requires SSL by default.</span></span> <span data-ttu-id="612fd-288">追加する必要があるため、このデモは、HTTP を使用して、次の構成ファイルの設定。</span><span class="sxs-lookup"><span data-stu-id="612fd-288">Since our demo is using HTTP, you need to add following setting in the config file:</span></span>

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> <span data-ttu-id="612fd-289">セキュリティの運用アプリで無効 SSL ことはありません。</span><span class="sxs-lookup"><span data-stu-id="612fd-289">Security - Never disable SSL in a production app.</span></span> <span data-ttu-id="612fd-290">ログイン資格情報は、ネットワーク経由でクリア テキストで送信されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="612fd-290">Your login credentials are now being sent in clear-text across the wire.</span></span> <span data-ttu-id="612fd-291">上記のコードは、ローカル サンプルのデバッグと探索のためだけです。</span><span class="sxs-lookup"><span data-stu-id="612fd-291">The code above is just for local sample debugging and exploration.</span></span>


### <a name="implicit-grant-client"></a><span data-ttu-id="612fd-292">暗黙的な許可クライアント</span><span class="sxs-lookup"><span data-stu-id="612fd-292">Implicit Grant Client</span></span>

<span data-ttu-id="612fd-293">このクライアントは、JavaScript を使用してください。</span><span class="sxs-lookup"><span data-stu-id="612fd-293">This client is using JavaScript to:</span></span>

1. <span data-ttu-id="612fd-294">新しいウィンドウを開き、承認サーバーの承認エンドポイントにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="612fd-294">Open a new window and redirect to the authorize endpoint of the Authorization Server.</span></span>
2. <span data-ttu-id="612fd-295">アクセス トークンを取得 URL フラグメントからリダイレクト戻るときにします。</span><span class="sxs-lookup"><span data-stu-id="612fd-295">Get the access token from URL fragments when it redirects back.</span></span>

<span data-ttu-id="612fd-296">次の図は、このプロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="612fd-296">The following image shows this process:</span></span>

![](owin-oauth-20-authorization-server/_static/image4.png)

<span data-ttu-id="612fd-297">クライアントが 2 つのページにある必要があります。 ホーム ページで、コールバックのもう 1 つ。JavaScript のコード サンプルを次に示します、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="612fd-297">The client should have two pages: one for home page and the other for callback.Here is the sample JavaScript code found in the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

<span data-ttu-id="612fd-298">処理コードにコールバックを次に示します*SignIn.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="612fd-298">Here is the callback handling code in *SignIn.cshtml* file:</span></span>

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> <span data-ttu-id="612fd-299">JavaScript を外部ファイルに移動し、Razor マークアップを埋め込まないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="612fd-299">A best practice is to move the JavaScript to an external file and not embed it with the Razor markup.</span></span> <span data-ttu-id="612fd-300">このサンプルをシンプルにするが統合されました。</span><span class="sxs-lookup"><span data-stu-id="612fd-300">To keep this sample simple, they have been combined.</span></span>


### <a name="resource-owner-password-credentials-grant-client"></a><span data-ttu-id="612fd-301">リソース所有者のパスワードの資格情報付与クライアント</span><span class="sxs-lookup"><span data-stu-id="612fd-301">Resource Owner Password Credentials Grant Client</span></span>

<span data-ttu-id="612fd-302">このクライアントをデモするため、コンソール アプリを使用しています。</span><span class="sxs-lookup"><span data-stu-id="612fd-302">We uses a console app to demo this client.</span></span> <span data-ttu-id="612fd-303">次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="612fd-303">Here is the code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a><span data-ttu-id="612fd-304">クライアント資格情報付与</span><span class="sxs-lookup"><span data-stu-id="612fd-304">Client Credentials Grant Client</span></span>

<span data-ttu-id="612fd-305">リソース所有者パスワード資格情報の付与と同様の場合、ここで、コンソール アプリのコードを示します。</span><span class="sxs-lookup"><span data-stu-id="612fd-305">Similar to the Resource Owner Password Credentials Grant, here is console app code:</span></span>

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
