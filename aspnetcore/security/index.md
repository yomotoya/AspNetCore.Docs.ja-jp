---
title: "ASP.NET Core Security の概要"
author: rachelappel
description: "ASP.NET Core での認証、承認、およびセキュリティの基本を学習します。"
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: e03256d7b8b442569b0b0126983732c10817e20f
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="aspnet-core-security-overview"></a><span data-ttu-id="77a58-103">ASP.NET Core セキュリティの概要</span><span class="sxs-lookup"><span data-stu-id="77a58-103">ASP.NET Core Security Overview</span></span>

<span data-ttu-id="77a58-104">ASP.NET Core を使用することで、開発者はアプリのセキュリティを簡単に構成して管理することができます。</span><span class="sxs-lookup"><span data-stu-id="77a58-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="77a58-105">ASP.NET Core には、認証、承認、データ保護、SSL の適用、アプリ シークレット、リクエスト フォージェリの対策保護、および CORS を管理するための機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="77a58-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="77a58-106">これらのセキュリティ機能を使用すれば、堅牢かつセキュアな ASP.NET Core アプリを構築できます。</span><span class="sxs-lookup"><span data-stu-id="77a58-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span>

## <a name="aspnet-core-security-features"></a><span data-ttu-id="77a58-107">ASP.NET Core セキュリティ機能</span><span class="sxs-lookup"><span data-stu-id="77a58-107">ASP.NET Core security features</span></span>

<span data-ttu-id="77a58-108">ASP.NET Core では、組み込みの ID プロバイダーを含むアプリをセキュリティで保護するための多くのツールとライブラリが提供されますが、Facebook、Twitter、LinkedIn などのサードパーティの ID サービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="77a58-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="77a58-109">ASP.NET Core では、コードで公開せずに機密情報を格納して使用する方法である、アプリ シークレットを簡単に管理できます。</span><span class="sxs-lookup"><span data-stu-id="77a58-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span>

## <a name="authentication-vs-authorization"></a><span data-ttu-id="77a58-110">認証と承認</span><span class="sxs-lookup"><span data-stu-id="77a58-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="77a58-111">認証とは、ユーザーが提供した資格情報をオペレーティング システム、データベース、アプリまたはリソースに格納されているものと比較するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="77a58-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="77a58-112">資格情報が一致した場合、ユーザーは正常に認証され、承認プロセス中に、承認されたアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="77a58-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="77a58-113">承認とは、ユーザーに許可する実行内容を決定するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="77a58-113">The authorization refers to the process that determines what a user is allowed to do.</span></span>

<span data-ttu-id="77a58-114">また、認証はサーバー、データベース、アプリまたはリソースなどの領域に入る方法と見なすことができます。一方、承認はユーザーがその領域 (サーバー、データベース、またはアプリ) 内のいずかのオブジェクトに対して実行できるアクションです。</span><span class="sxs-lookup"><span data-stu-id="77a58-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="77a58-115">ソフトウェアの一般的な脆弱性</span><span class="sxs-lookup"><span data-stu-id="77a58-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="77a58-116">ASP.NET Core および EF には、アプリをセキュリティで保護し、セキュリティ違反を防止するのに役立つ機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="77a58-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="77a58-117">以下にリストされているリンクから、Web アプリの最も一般的なセキュリティの脆弱性を回避するための手法の詳細を示すドキュメントにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="77a58-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="77a58-118">クロスサイト スクリプト攻撃</span><span class="sxs-lookup"><span data-stu-id="77a58-118">Cross-site scripting attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [<span data-ttu-id="77a58-119">SQL インジェクション攻撃</span><span class="sxs-lookup"><span data-stu-id="77a58-119">SQL injection attacks</span></span>](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [<span data-ttu-id="77a58-120">クロスサイト リクエスト フォージェリ (CSRF)</span><span class="sxs-lookup"><span data-stu-id="77a58-120">Cross-Site Request Forgery (CSRF)</span></span>](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [<span data-ttu-id="77a58-121">オープン リダイレクト攻撃</span><span class="sxs-lookup"><span data-stu-id="77a58-121">Open redirect attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

<span data-ttu-id="77a58-122">この他にも知っておく必要がある脆弱性はあります。</span><span class="sxs-lookup"><span data-stu-id="77a58-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="77a58-123">詳細については、このドキュメントの「*ASP.NET セキュリティに関するドキュメント*」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="77a58-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span>

## <a name="aspnet-security-documentation"></a><span data-ttu-id="77a58-124">ASP.NET セキュリティに関するドキュメント</span><span class="sxs-lookup"><span data-stu-id="77a58-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="77a58-125">認証</span><span class="sxs-lookup"><span data-stu-id="77a58-125">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="77a58-126">Identity の概要</span><span class="sxs-lookup"><span data-stu-id="77a58-126">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="77a58-127">Facebook、Google、および他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="77a58-127">Enable authentication using Facebook, Google, and other external providers</span></span>](authentication/social/index.md)
    *   [<span data-ttu-id="77a58-128">WS フェデレーションを使用した認証の有効化</span><span class="sxs-lookup"><span data-stu-id="77a58-128">Enable authentication with WS-Federation</span></span>](authentication/ws-federation.md)
    * [<span data-ttu-id="77a58-129">Windows 認証の構成</span><span class="sxs-lookup"><span data-stu-id="77a58-129">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="77a58-130">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="77a58-130">Account confirmation and password recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="77a58-131">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="77a58-131">Two-factor authentication with SMS</span></span>](authentication/2fa.md)
    *   [<span data-ttu-id="77a58-132">Identity なしでの Cookie 認証の使用</span><span class="sxs-lookup"><span data-stu-id="77a58-132">Use cookie authentication without Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="77a58-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="77a58-133">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="77a58-134">ASP.NET Core Web アプリへの Azure AD の統合</span><span class="sxs-lookup"><span data-stu-id="77a58-134">Integrate Azure AD into an ASP.NET Core web app</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="77a58-135">Azure AD を使用した WPF アプリからの ASP.NET Core Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="77a58-135">Call an ASP.NET Core Web API from a WPF app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="77a58-136">Azure AD を使用した ASP.NET Core Web アプリでの Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="77a58-136">Call a Web API in an ASP.NET Core web app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="77a58-137">ASP.NET Core Web アプリでの Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="77a58-137">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="77a58-138">IdentityServer4 での ASP.NET Core アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="77a58-138">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="77a58-139">承認</span><span class="sxs-lookup"><span data-stu-id="77a58-139">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="77a58-140">はじめに</span><span class="sxs-lookup"><span data-stu-id="77a58-140">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="77a58-141">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="77a58-141">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="77a58-142">単純な承認</span><span class="sxs-lookup"><span data-stu-id="77a58-142">Simple authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="77a58-143">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="77a58-143">Role-based authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="77a58-144">クレーム ベースの承認</span><span class="sxs-lookup"><span data-stu-id="77a58-144">Claims-based authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="77a58-145">ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="77a58-145">Policy-based authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="77a58-146">要件ハンドラーでの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="77a58-146">Dependency injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="77a58-147">リソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="77a58-147">Resource-based authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="77a58-148">ビュー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="77a58-148">View-based authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="77a58-149">スキームによる ID の制限</span><span class="sxs-lookup"><span data-stu-id="77a58-149">Limit identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="77a58-150">データ保護</span><span class="sxs-lookup"><span data-stu-id="77a58-150">Data protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="77a58-151">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="77a58-151">Introduction to data protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="77a58-152">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="77a58-152">Get started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="77a58-153">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="77a58-153">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="77a58-154">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="77a58-154">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="77a58-155">目的文字列</span><span class="sxs-lookup"><span data-stu-id="77a58-155">Purpose strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="77a58-156">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="77a58-156">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="77a58-157">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="77a58-157">Password hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="77a58-158">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="77a58-158">Limit the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="77a58-159">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="77a58-159">Unprotect payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="77a58-160">構成</span><span class="sxs-lookup"><span data-stu-id="77a58-160">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="77a58-161">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="77a58-161">Configure data protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="77a58-162">既定の設定</span><span class="sxs-lookup"><span data-stu-id="77a58-162">Default settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="77a58-163">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="77a58-163">Machine-wide policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="77a58-164">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="77a58-164">Non DI-aware scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="77a58-165">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="77a58-165">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="77a58-166">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="77a58-166">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="77a58-167">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="77a58-167">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="77a58-168">その他の API</span><span class="sxs-lookup"><span data-stu-id="77a58-168">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="77a58-169">実装</span><span class="sxs-lookup"><span data-stu-id="77a58-169">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="77a58-170">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="77a58-170">Authenticated encryption details</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="77a58-171">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="77a58-171">Subkey derivation and authenticated encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="77a58-172">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="77a58-172">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="77a58-173">キーの管理</span><span class="sxs-lookup"><span data-stu-id="77a58-173">Key management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="77a58-174">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="77a58-174">Key storage providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="77a58-175">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="77a58-175">Key encryption at rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="77a58-176">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="77a58-176">Key immutability and changing settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="77a58-177">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="77a58-177">Key storage format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="77a58-178">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="77a58-178">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="77a58-179">互換性</span><span class="sxs-lookup"><span data-stu-id="77a58-179">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="77a58-180">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="77a58-180">Replace <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="77a58-181">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="77a58-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="77a58-182">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="77a58-182">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="77a58-183">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="77a58-183">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="77a58-184">SSL の適用</span><span class="sxs-lookup"><span data-stu-id="77a58-184">Enforce SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="77a58-185">リクエスト フォージェリの対策</span><span class="sxs-lookup"><span data-stu-id="77a58-185">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="77a58-186">オープン リダイレクト攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="77a58-186">Prevent open redirect attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="77a58-187">クロスサイト スクリプティングの防止</span><span class="sxs-lookup"><span data-stu-id="77a58-187">Prevent Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="77a58-188">クロスオリジン要求 (CORS) の有効化</span><span class="sxs-lookup"><span data-stu-id="77a58-188">Enable Cross-Origin Requests (CORS)</span></span>](cors.md)
*   [<span data-ttu-id="77a58-189">アプリ間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="77a58-189">Share cookies among apps</span></span>](cookie-sharing.md)
