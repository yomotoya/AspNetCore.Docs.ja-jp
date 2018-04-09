---
title: ASP.NET Core での IIS モジュール
author: guardrex
description: ASP.NET Core アプリケーションおよび IIS モジュールを管理する方法のアクティブおよび非アクティブの IIS モジュールを検出します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: d9b3de915df333153255f91649f9169f76ba2fe0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="iis-modules-with-aspnet-core"></a><span data-ttu-id="9f65d-103">ASP.NET Core での IIS モジュール</span><span class="sxs-lookup"><span data-stu-id="9f65d-103">IIS modules with ASP.NET Core</span></span>

<span data-ttu-id="9f65d-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9f65d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9f65d-105">ASP.NET Core アプリケーションは、リバース プロキシの構成では IIS によってホストされます。</span><span class="sxs-lookup"><span data-stu-id="9f65d-105">ASP.NET Core apps are hosted by IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="9f65d-106">IIS のネイティブ モジュールの一部とすべての IIS 管理モジュールは、ASP.NET Core アプリケーションの要求の処理に使用できません。</span><span class="sxs-lookup"><span data-stu-id="9f65d-106">Some of the native IIS modules and all of the IIS managed modules aren't available to process requests for ASP.NET Core apps.</span></span> <span data-ttu-id="9f65d-107">多くの場合は、ASP.NET Core は、代わりに IIS のネイティブおよびマネージ モジュールの機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="9f65d-107">In many cases, ASP.NET Core offers an alternative to the features of IIS native and managed modules.</span></span>

## <a name="native-modules"></a><span data-ttu-id="9f65d-108">ネイティブ モジュール</span><span class="sxs-lookup"><span data-stu-id="9f65d-108">Native modules</span></span>

| <span data-ttu-id="9f65d-109">Module</span><span class="sxs-lookup"><span data-stu-id="9f65d-109">Module</span></span> | <span data-ttu-id="9f65d-110">.NET Core Active</span><span class="sxs-lookup"><span data-stu-id="9f65d-110">.NET Core Active</span></span> | <span data-ttu-id="9f65d-111">ASP.NET Core Option</span><span class="sxs-lookup"><span data-stu-id="9f65d-111">ASP.NET Core Option</span></span> |
| ------ | :--------------: | ------------------- |
| <span data-ttu-id="9f65d-112">**匿名認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-112">**Anonymous Authentication**</span></span><br>`AnonymousAuthenticationModule` | <span data-ttu-id="9f65d-113">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-113">Yes</span></span> | |
| <span data-ttu-id="9f65d-114">**基本認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-114">**Basic Authentication**</span></span><br>`BasicAuthenticationModule` | <span data-ttu-id="9f65d-115">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-115">Yes</span></span> | |
| <span data-ttu-id="9f65d-116">**クライアント証明書マッピング認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-116">**Client Certification Mapping Authentication**</span></span><br>`CertificateMappingAuthenticationModule` | <span data-ttu-id="9f65d-117">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-117">Yes</span></span> | |
| <span data-ttu-id="9f65d-118">**CGI**</span><span class="sxs-lookup"><span data-stu-id="9f65d-118">**CGI**</span></span><br>`CgiModule` | <span data-ttu-id="9f65d-119">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-119">No</span></span> | |
| <span data-ttu-id="9f65d-120">**構成の検証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-120">**Configuration Validation**</span></span><br>`ConfigurationValidationModule` | <span data-ttu-id="9f65d-121">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-121">Yes</span></span> | |
| <span data-ttu-id="9f65d-122">**HTTP エラー**</span><span class="sxs-lookup"><span data-stu-id="9f65d-122">**HTTP Errors**</span></span><br>`CustomErrorModule` | <span data-ttu-id="9f65d-123">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-123">No</span></span> | [<span data-ttu-id="9f65d-124">ステータス コード ページ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-124">Status Code Pages Middleware</span></span>](xref:fundamentals/error-handling#configuring-status-code-pages) |
| <span data-ttu-id="9f65d-125">**カスタム ログ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-125">**Custom Logging**</span></span><br>`CustomLoggingModule` | <span data-ttu-id="9f65d-126">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-126">Yes</span></span> | |
| <span data-ttu-id="9f65d-127">**既定のドキュメント**</span><span class="sxs-lookup"><span data-stu-id="9f65d-127">**Default Document**</span></span><br>`DefaultDocumentModule` | <span data-ttu-id="9f65d-128">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-128">No</span></span> | [<span data-ttu-id="9f65d-129">既定のファイルのミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-129">Default Files Middleware</span></span>](xref:fundamentals/static-files#serve-a-default-document) |
| <span data-ttu-id="9f65d-130">**ダイジェスト認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-130">**Digest Authentication**</span></span><br>`DigestAuthenticationModule` | <span data-ttu-id="9f65d-131">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-131">Yes</span></span> | |
| <span data-ttu-id="9f65d-132">**ディレクトリの参照**</span><span class="sxs-lookup"><span data-stu-id="9f65d-132">**Directory Browsing**</span></span><br>`DirectoryListingModule` | <span data-ttu-id="9f65d-133">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-133">No</span></span> | [<span data-ttu-id="9f65d-134">ディレクトリ参照ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-134">Directory Browsing Middleware</span></span>](xref:fundamentals/static-files#enable-directory-browsing) |
| <span data-ttu-id="9f65d-135">**動的な圧縮**</span><span class="sxs-lookup"><span data-stu-id="9f65d-135">**Dynamic Compression**</span></span><br>`DynamicCompressionModule` | <span data-ttu-id="9f65d-136">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-136">Yes</span></span> | [<span data-ttu-id="9f65d-137">応答圧縮ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-137">Response Compression Middleware</span></span>](xref:performance/response-compression) |
| <span data-ttu-id="9f65d-138">**トレース**</span><span class="sxs-lookup"><span data-stu-id="9f65d-138">**Tracing**</span></span><br>`FailedRequestsTracingModule` | <span data-ttu-id="9f65d-139">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-139">Yes</span></span> | [<span data-ttu-id="9f65d-140">ASP.NET Core のログ記録</span><span class="sxs-lookup"><span data-stu-id="9f65d-140">ASP.NET Core Logging</span></span>](xref:fundamentals/logging/index#the-tracesource-provider) |
| <span data-ttu-id="9f65d-141">**ファイルのキャッシュ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-141">**File Caching**</span></span><br>`FileCacheModule` | <span data-ttu-id="9f65d-142">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-142">No</span></span> | [<span data-ttu-id="9f65d-143">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-143">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="9f65d-144">**HTTP キャッシュ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-144">**HTTP Caching**</span></span><br>`HttpCacheModule` | <span data-ttu-id="9f65d-145">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-145">No</span></span> | [<span data-ttu-id="9f65d-146">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-146">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="9f65d-147">**HTTP ログ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-147">**HTTP Logging**</span></span><br>`HttpLoggingModule` | <span data-ttu-id="9f65d-148">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-148">Yes</span></span> | [<span data-ttu-id="9f65d-149">ASP.NET Core のログ記録</span><span class="sxs-lookup"><span data-stu-id="9f65d-149">ASP.NET Core Logging</span></span>](xref:fundamentals/logging/index)<br><span data-ttu-id="9f65d-150">実装: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)、 [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)、 [NLog](https://github.com/NLog/NLog.Extensions.Logging)、 [Serilog](https://github.com/serilog/serilog-extensions-logging)</span><span class="sxs-lookup"><span data-stu-id="9f65d-150">Implementations: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)</span></span>
| <span data-ttu-id="9f65d-151">**HTTP リダイレクト**</span><span class="sxs-lookup"><span data-stu-id="9f65d-151">**HTTP Redirection**</span></span><br>`HttpRedirectionModule` | <span data-ttu-id="9f65d-152">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-152">Yes</span></span> | [<span data-ttu-id="9f65d-153">URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="9f65d-154">**IIS クライアント証明書マッピング認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-154">**IIS Client Certificate Mapping Authentication**</span></span><br>`IISCertificateMappingAuthenticationModule` | <span data-ttu-id="9f65d-155">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-155">Yes</span></span> | |
| <span data-ttu-id="9f65d-156">**IP およびドメインの制限**</span><span class="sxs-lookup"><span data-stu-id="9f65d-156">**IP and Domain Restrictions**</span></span><br>`IpRestrictionModule` | <span data-ttu-id="9f65d-157">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-157">Yes</span></span> | |
| <span data-ttu-id="9f65d-158">**ISAPI フィルター**</span><span class="sxs-lookup"><span data-stu-id="9f65d-158">**ISAPI Filters**</span></span><br>`IsapiFilterModule` | <span data-ttu-id="9f65d-159">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-159">Yes</span></span> | [<span data-ttu-id="9f65d-160">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-160">Middleware</span></span>](xref:fundamentals/middleware/index) |
| <span data-ttu-id="9f65d-161">**ISAPI**</span><span class="sxs-lookup"><span data-stu-id="9f65d-161">**ISAPI**</span></span><br>`IsapiModule` | <span data-ttu-id="9f65d-162">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-162">Yes</span></span> | [<span data-ttu-id="9f65d-163">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-163">Middleware</span></span>](xref:fundamentals/middleware/index) |
| <span data-ttu-id="9f65d-164">**プロトコルのサポート**</span><span class="sxs-lookup"><span data-stu-id="9f65d-164">**Protocol Support**</span></span><br>`ProtocolSupportModule` | <span data-ttu-id="9f65d-165">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-165">Yes</span></span> | |
| <span data-ttu-id="9f65d-166">**要求のフィルタリング**</span><span class="sxs-lookup"><span data-stu-id="9f65d-166">**Request Filtering**</span></span><br>`RequestFilteringModule` | <span data-ttu-id="9f65d-167">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-167">Yes</span></span> | [<span data-ttu-id="9f65d-168">URL 書き換えミドルウェア `IRule`</span><span class="sxs-lookup"><span data-stu-id="9f65d-168">URL Rewriting Middleware `IRule`</span></span>](xref:fundamentals/url-rewriting#irule-based-rule) |
| <span data-ttu-id="9f65d-169">**要求監視**</span><span class="sxs-lookup"><span data-stu-id="9f65d-169">**Request Monitor**</span></span><br>`RequestMonitorModule` | <span data-ttu-id="9f65d-170">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-170">Yes</span></span> | |
| <span data-ttu-id="9f65d-171">**URL 書き換え**</span><span class="sxs-lookup"><span data-stu-id="9f65d-171">**URL Rewriting**</span></span><br>`RewriteModule` | <span data-ttu-id="9f65d-172">うん&#8224;</span><span class="sxs-lookup"><span data-stu-id="9f65d-172">Yes&#8224;</span></span> | [<span data-ttu-id="9f65d-173">URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-173">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="9f65d-174">**サーバー側インクルードします。**</span><span class="sxs-lookup"><span data-stu-id="9f65d-174">**Server-Side Includes**</span></span><br>`ServerSideIncludeModule` | <span data-ttu-id="9f65d-175">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-175">No</span></span> | |
| <span data-ttu-id="9f65d-176">**静的な圧縮**</span><span class="sxs-lookup"><span data-stu-id="9f65d-176">**Static Compression**</span></span><br>`StaticCompressionModule` | <span data-ttu-id="9f65d-177">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-177">No</span></span> | [<span data-ttu-id="9f65d-178">応答圧縮ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-178">Response Compression Middleware</span></span>](xref:performance/response-compression) |
| <span data-ttu-id="9f65d-179">**静的コンテンツ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-179">**Static Content**</span></span><br>`StaticFileModule` | <span data-ttu-id="9f65d-180">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-180">No</span></span> | [<span data-ttu-id="9f65d-181">静的ファイル ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-181">Static File Middleware</span></span>](xref:fundamentals/static-files) |
| <span data-ttu-id="9f65d-182">**トークンのキャッシュ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-182">**Token Caching**</span></span><br>`TokenCacheModule` | <span data-ttu-id="9f65d-183">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-183">Yes</span></span> | |
| <span data-ttu-id="9f65d-184">**URI のキャッシュ**</span><span class="sxs-lookup"><span data-stu-id="9f65d-184">**URI Caching**</span></span><br>`UriCacheModule` | <span data-ttu-id="9f65d-185">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-185">Yes</span></span> | |
| <span data-ttu-id="9f65d-186">**URL 認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-186">**URL Authorization**</span></span><br>`UrlAuthorizationModule` | <span data-ttu-id="9f65d-187">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-187">Yes</span></span> | [<span data-ttu-id="9f65d-188">ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="9f65d-188">ASP.NET Core Identity</span></span>](xref:security/authentication/identity) |
| <span data-ttu-id="9f65d-189">**Windows 認証**</span><span class="sxs-lookup"><span data-stu-id="9f65d-189">**Windows Authentication**</span></span><br>`WindowsAuthenticationModule` | <span data-ttu-id="9f65d-190">[はい]</span><span class="sxs-lookup"><span data-stu-id="9f65d-190">Yes</span></span> | |

<span data-ttu-id="9f65d-191">&#8224;URL Rewrite モジュールの`isFile`と`isDirectory`一致の種類の変更のための ASP.NET Core アプリケーションとでは動作しない[ディレクトリ構造](xref:host-and-deploy/directory-structure)です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-191">&#8224;The URL Rewrite Module's `isFile` and `isDirectory` match types don't work with ASP.NET Core apps due to the changes in [directory structure](xref:host-and-deploy/directory-structure).</span></span>

## <a name="managed-modules"></a><span data-ttu-id="9f65d-192">マネージ モジュール</span><span class="sxs-lookup"><span data-stu-id="9f65d-192">Managed modules</span></span>

| <span data-ttu-id="9f65d-193">Module</span><span class="sxs-lookup"><span data-stu-id="9f65d-193">Module</span></span>                  | <span data-ttu-id="9f65d-194">.NET Core Active</span><span class="sxs-lookup"><span data-stu-id="9f65d-194">.NET Core Active</span></span> | <span data-ttu-id="9f65d-195">ASP.NET Core Option</span><span class="sxs-lookup"><span data-stu-id="9f65d-195">ASP.NET Core Option</span></span> |
| ----------------------- | :--------------: | ------------------- |
| <span data-ttu-id="9f65d-196">AnonymousIdentification</span><span class="sxs-lookup"><span data-stu-id="9f65d-196">AnonymousIdentification</span></span> | <span data-ttu-id="9f65d-197">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-197">No</span></span>               | |
| <span data-ttu-id="9f65d-198">DefaultAuthentication</span><span class="sxs-lookup"><span data-stu-id="9f65d-198">DefaultAuthentication</span></span>   | <span data-ttu-id="9f65d-199">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-199">No</span></span>               | |
| <span data-ttu-id="9f65d-200">FileAuthorization</span><span class="sxs-lookup"><span data-stu-id="9f65d-200">FileAuthorization</span></span>       | <span data-ttu-id="9f65d-201">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-201">No</span></span>               | |
| <span data-ttu-id="9f65d-202">FormsAuthentication</span><span class="sxs-lookup"><span data-stu-id="9f65d-202">FormsAuthentication</span></span>     | <span data-ttu-id="9f65d-203">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-203">No</span></span>               | [<span data-ttu-id="9f65d-204">Cookie 認証ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-204">Cookie Authentication Middleware</span></span>](xref:security/authentication/cookie) |
| <span data-ttu-id="9f65d-205">OutputCache</span><span class="sxs-lookup"><span data-stu-id="9f65d-205">OutputCache</span></span>             | <span data-ttu-id="9f65d-206">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-206">No</span></span>               | [<span data-ttu-id="9f65d-207">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-207">Response Caching Middleware</span></span>](xref:performance/caching/middleware) |
| <span data-ttu-id="9f65d-208">Profile</span><span class="sxs-lookup"><span data-stu-id="9f65d-208">Profile</span></span>                 | <span data-ttu-id="9f65d-209">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-209">No</span></span>               | |
| <span data-ttu-id="9f65d-210">RoleManager</span><span class="sxs-lookup"><span data-stu-id="9f65d-210">RoleManager</span></span>             | <span data-ttu-id="9f65d-211">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-211">No</span></span>               | |
| <span data-ttu-id="9f65d-212">ScriptModule-4.0</span><span class="sxs-lookup"><span data-stu-id="9f65d-212">ScriptModule-4.0</span></span>        | <span data-ttu-id="9f65d-213">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-213">No</span></span>               | |
| <span data-ttu-id="9f65d-214">セッション</span><span class="sxs-lookup"><span data-stu-id="9f65d-214">Session</span></span>                 | <span data-ttu-id="9f65d-215">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-215">No</span></span>               | [<span data-ttu-id="9f65d-216">セッションのミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-216">Session Middleware</span></span>](xref:fundamentals/app-state) |
| <span data-ttu-id="9f65d-217">UrlAuthorization</span><span class="sxs-lookup"><span data-stu-id="9f65d-217">UrlAuthorization</span></span>        | <span data-ttu-id="9f65d-218">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-218">No</span></span>               | |
| <span data-ttu-id="9f65d-219">UrlMappingsModule</span><span class="sxs-lookup"><span data-stu-id="9f65d-219">UrlMappingsModule</span></span>       | <span data-ttu-id="9f65d-220">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-220">No</span></span>               | [<span data-ttu-id="9f65d-221">URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9f65d-221">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) |
| <span data-ttu-id="9f65d-222">UrlRoutingModule-4.0</span><span class="sxs-lookup"><span data-stu-id="9f65d-222">UrlRoutingModule-4.0</span></span>    | <span data-ttu-id="9f65d-223">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-223">No</span></span>               | [<span data-ttu-id="9f65d-224">ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="9f65d-224">ASP.NET Core Identity</span></span>](xref:security/authentication/identity) |
| <span data-ttu-id="9f65d-225">WindowsAuthentication</span><span class="sxs-lookup"><span data-stu-id="9f65d-225">WindowsAuthentication</span></span>   | <span data-ttu-id="9f65d-226">×</span><span class="sxs-lookup"><span data-stu-id="9f65d-226">No</span></span>               | |

## <a name="iis-manager-application-changes"></a><span data-ttu-id="9f65d-227">IIS マネージャー アプリケーションの変更</span><span class="sxs-lookup"><span data-stu-id="9f65d-227">IIS Manager application changes</span></span>

<span data-ttu-id="9f65d-228">IIS マネージャーを使用して、設定を構成する場合、 *web.config*アプリのファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="9f65d-228">When using IIS Manager to configure settings, the *web.config* file of the app is changed.</span></span> <span data-ttu-id="9f65d-229">アプリを配置する場合と*web.config*、IIS マネージャーで加えられた変更は上書きによって展開された*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9f65d-229">If deploying an app and including *web.config*, any changes made with IIS Manager are overwritten by the deployed *web.config* file.</span></span> <span data-ttu-id="9f65d-230">サーバーの変更が加えられた場合*web.config*ファイル、コピー、更新された*web.config*ローカル プロジェクトをすぐに、サーバー上のファイルです。</span><span class="sxs-lookup"><span data-stu-id="9f65d-230">If changes are made to the server's *web.config* file, copy the updated *web.config* file on the server to the local project immediately.</span></span>

## <a name="disabling-iis-modules"></a><span data-ttu-id="9f65d-231">IIS モジュールを無効にします。</span><span class="sxs-lookup"><span data-stu-id="9f65d-231">Disabling IIS modules</span></span>

<span data-ttu-id="9f65d-232">IIS モジュールが、アプリでアプリへの追記を無効にする必要がありますサーバー レベルで構成されているかどうか*web.config*ファイルには、モジュールが無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="9f65d-232">If an IIS module is configured at the server level that must be disabled for an app, an addition to the app's *web.config* file can disable the module.</span></span> <span data-ttu-id="9f65d-233">モジュールをそのまま残す (使用可能な場合)、構成の設定を使用して、非アクティブか、アプリからモジュールを削除します。</span><span class="sxs-lookup"><span data-stu-id="9f65d-233">Either leave the module in place and deactivate it using a configuration setting (if available) or remove the module from the app.</span></span>

### <a name="module-deactivation"></a><span data-ttu-id="9f65d-234">モジュールの非アクティブ化</span><span class="sxs-lookup"><span data-stu-id="9f65d-234">Module deactivation</span></span>

<span data-ttu-id="9f65d-235">多くのモジュールには、アプリからモジュールを削除せずに無効にすることができる構成設定が用意されています。</span><span class="sxs-lookup"><span data-stu-id="9f65d-235">Many modules offer a configuration setting that allows them to be disabled without removing the module from the app.</span></span> <span data-ttu-id="9f65d-236">これは、モジュールを非アクティブ化する最も簡単なと最も簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-236">This is the simplest and quickest way to deactivate a module.</span></span> <span data-ttu-id="9f65d-237">たとえば、IIS URL Rewrite Module 無効にする、  **\<httpRedirect >**内の要素*web.config*:</span><span class="sxs-lookup"><span data-stu-id="9f65d-237">For example, the IIS URL Rewrite Module can be disabled with the **\<httpRedirect>** element in *web.config*:</span></span>

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="9f65d-238">モジュールの構成設定を無効にする方法の詳細については、以下のリンク、*子要素*のセクション[IIS \<system.webServer >](/iis/configuration/system.webServer/)です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-238">For more information on disabling modules with configuration settings, follow the links in the *Child Elements* section of [IIS \<system.webServer>](/iis/configuration/system.webServer/).</span></span>

### <a name="module-removal"></a><span data-ttu-id="9f65d-239">モジュールの取り外し</span><span class="sxs-lookup"><span data-stu-id="9f65d-239">Module removal</span></span>

<span data-ttu-id="9f65d-240">設定と、モジュールを削除しなかった場合は*web.config*モジュールのロックを解除、およびロック解除、 **\<モジュール >**のセクション*web.config*最初。</span><span class="sxs-lookup"><span data-stu-id="9f65d-240">If opting to remove a module with a setting in *web.config*, unlock the module and unlock the **\<modules>** section of *web.config* first:</span></span>

1. <span data-ttu-id="9f65d-241">サーバー レベルのモジュールのロックを解除します。</span><span class="sxs-lookup"><span data-stu-id="9f65d-241">Unlock the module at the server level.</span></span> <span data-ttu-id="9f65d-242">IIS マネージャーで、IIS サーバーを選択**接続**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="9f65d-242">Select the IIS server in the IIS Manager **Connections** sidebar.</span></span> <span data-ttu-id="9f65d-243">開く、**モジュール**で、 **IIS**領域。</span><span class="sxs-lookup"><span data-stu-id="9f65d-243">Open the **Modules** in the **IIS** area.</span></span> <span data-ttu-id="9f65d-244">一覧で、モジュールを選択します。</span><span class="sxs-lookup"><span data-stu-id="9f65d-244">Select the module in the list.</span></span> <span data-ttu-id="9f65d-245">**アクション**、右側のサイド バーを選択**Unlock**です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-245">In the **Actions** sidebar on the right, select **Unlock**.</span></span> <span data-ttu-id="9f65d-246">多くのモジュールから削除することを計画する際のロックを解除*web.config*以降。</span><span class="sxs-lookup"><span data-stu-id="9f65d-246">Unlock as many modules as you plan to remove from *web.config* later.</span></span>

2. <span data-ttu-id="9f65d-247">アプリを展開することがなく、 **\<モジュール >** 」の「 *web.config*です。アプリが展開された場合、 *web.config*を含む、 **\<モジュール >**がロックを解除セクション最初、IIS マネージャーで、Configuration Manager なしでは、例外をスローします。セクションのロック解除しようとしています。</span><span class="sxs-lookup"><span data-stu-id="9f65d-247">Deploy the app without a **\<modules>** section in *web.config*. If an app is deployed with a *web.config* containing the **\<modules>** section without having unlocked the section first in the IIS Manager, the Configuration Manager throws an exception when attempting to unlock the section.</span></span> <span data-ttu-id="9f65d-248">そのため、アプリを展開することがなく、 **\<モジュール >**セクションです。</span><span class="sxs-lookup"><span data-stu-id="9f65d-248">Therefore, deploy the app without a **\<modules>** section.</span></span>

3. <span data-ttu-id="9f65d-249">ロックを解除、 **\<モジュール >**のセクション*web.config*です。**接続**サイドバーで web サイトを選択**サイト**です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-249">Unlock the **\<modules>** section of *web.config*. In the **Connections** sidebar, select the website in **Sites**.</span></span> <span data-ttu-id="9f65d-250">**管理**領域で、開く、**構成エディター**です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-250">In the **Management** area, open the **Configuration Editor**.</span></span> <span data-ttu-id="9f65d-251">ナビゲーション コントロールを使用して、`system.webServer/modules`セクションです。</span><span class="sxs-lookup"><span data-stu-id="9f65d-251">Use the navigation controls to select the `system.webServer/modules` section.</span></span> <span data-ttu-id="9f65d-252">**アクション**、右側のサイド バーを選択する**Unlock**セクションです。</span><span class="sxs-lookup"><span data-stu-id="9f65d-252">In the **Actions** sidebar on the right, select to **Unlock** the section.</span></span>

4. <span data-ttu-id="9f65d-253">この時点で、 **\<モジュール >**セクションに追加できる、 *web.config*ファイルと、 **\<削除 >**からモジュールを削除する要素アプリ。</span><span class="sxs-lookup"><span data-stu-id="9f65d-253">At this point, a **\<modules>** section can be added to the *web.config* file with a **\<remove>** element to remove the module from the app.</span></span> <span data-ttu-id="9f65d-254">複数**\<削除 >**を複数のモジュールを削除する要素を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9f65d-254">Multiple **\<remove>** elements can be added to remove multiple modules.</span></span> <span data-ttu-id="9f65d-255">場合*web.config*に対する変更は、サーバーで、すぐにプロジェクトの同じ変更を行う*web.config*ファイルをローカルにします。</span><span class="sxs-lookup"><span data-stu-id="9f65d-255">If *web.config* changes are made on the server, immediately make the same changes to the project's *web.config* file locally.</span></span> <span data-ttu-id="9f65d-256">こうすると、モジュールを削除すると、サーバー上の他のアプリを使用してモジュールの使用は影響しません。</span><span class="sxs-lookup"><span data-stu-id="9f65d-256">Removing a module this way won't affect the use of the module with other apps on the server.</span></span>

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

<span data-ttu-id="9f65d-257">IIS のインストールでインストールされている既定のモジュールには、次を使用して**\<モジュール >**を既定のモジュールを削除する」セクション。</span><span class="sxs-lookup"><span data-stu-id="9f65d-257">For an IIS installation with the default modules installed, use the following **\<module>** section to remove the default modules.</span></span>

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

<span data-ttu-id="9f65d-258">IIS モジュールを削除することができますも*Appcmd.exe*です。</span><span class="sxs-lookup"><span data-stu-id="9f65d-258">An IIS module can also be removed with *Appcmd.exe*.</span></span> <span data-ttu-id="9f65d-259">提供、`MODULE_NAME`と`APPLICATION_NAME`コマンド。</span><span class="sxs-lookup"><span data-stu-id="9f65d-259">Provide the `MODULE_NAME` and `APPLICATION_NAME` in the command:</span></span>

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

<span data-ttu-id="9f65d-260">たとえば、削除、`DynamicCompressionModule`既定の Web サイトから。</span><span class="sxs-lookup"><span data-stu-id="9f65d-260">For example, remove the `DynamicCompressionModule` from the Default Web Site:</span></span>

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a><span data-ttu-id="9f65d-261">最小限のモジュールの構成</span><span class="sxs-lookup"><span data-stu-id="9f65d-261">Minimum module configuration</span></span>

<span data-ttu-id="9f65d-262">ASP.NET Core アプリケーションを実行するために必要なだけモジュールとは、匿名の認証モジュールおよび ASP.NET Core モジュールです。</span><span class="sxs-lookup"><span data-stu-id="9f65d-262">The only modules required to run an ASP.NET Core app are the Anonymous Authentication Module and the ASP.NET Core Module.</span></span>

![最小限のモジュールの構成とモジュールに IIS マネージャーを開く](modules/_static/modules.png)

## <a name="additional-resources"></a><span data-ttu-id="9f65d-264">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="9f65d-264">Additional resources</span></span>

* [<span data-ttu-id="9f65d-265">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="9f65d-265">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="9f65d-266">IIS のアーキテクチャの概要: IIS のモジュール</span><span class="sxs-lookup"><span data-stu-id="9f65d-266">Introduction to IIS Architectures: Modules in IIS</span></span>](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [<span data-ttu-id="9f65d-267">IIS のモジュールの概要</span><span class="sxs-lookup"><span data-stu-id="9f65d-267">IIS Modules Overview</span></span>](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [<span data-ttu-id="9f65d-268">IIS 7.0 ロール、およびモジュールをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="9f65d-268">Customizing IIS 7.0 Roles and Modules</span></span>](https://technet.microsoft.com/library/cc627313.aspx)
* [<span data-ttu-id="9f65d-269">IIS `<system.webServer>`</span><span class="sxs-lookup"><span data-stu-id="9f65d-269">IIS `<system.webServer>`</span></span>](/iis/configuration/system.webServer/)
