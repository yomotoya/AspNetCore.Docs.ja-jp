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
ms.openlocfilehash: e1aaae09fe69e6b65a917785b436f927fac5345d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-security-overview"></a><span data-ttu-id="95156-103">ASP.NET Core セキュリティの概要</span><span class="sxs-lookup"><span data-stu-id="95156-103">ASP.NET Core Security Overview</span></span>

<span data-ttu-id="95156-104">ASP.NET Core を使用することで、開発者はアプリのセキュリティを簡単に構成して管理することができます。</span><span class="sxs-lookup"><span data-stu-id="95156-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="95156-105">ASP.NET Core には、認証、承認、データ保護、SSL の適用、アプリ シークレット、リクエスト フォージェリの対策保護、および CORS を管理するための機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="95156-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="95156-106">これらのセキュリティ機能を使用すれば、堅牢かつセキュアな ASP.NET Core アプリを構築できます。</span><span class="sxs-lookup"><span data-stu-id="95156-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span> 

## <a name="aspnet-core-security-features"></a><span data-ttu-id="95156-107">ASP.NET Core セキュリティ機能</span><span class="sxs-lookup"><span data-stu-id="95156-107">ASP.NET Core security features</span></span>

<span data-ttu-id="95156-108">ASP.NET Core では、組み込みの ID プロバイダーを含むアプリをセキュリティで保護するための多くのツールとライブラリが提供されますが、Facebook、Twitter、LinkedIn などのサードパーティの ID サービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="95156-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="95156-109">ASP.NET Core では、コードで公開せずに機密情報を格納して使用する方法である、アプリ シークレットを簡単に管理できます。</span><span class="sxs-lookup"><span data-stu-id="95156-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span> 

## <a name="authentication-vs-authorization"></a><span data-ttu-id="95156-110">認証と承認</span><span class="sxs-lookup"><span data-stu-id="95156-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="95156-111">認証とは、ユーザーが提供した資格情報をオペレーティング システム、データベース、アプリまたはリソースに格納されているものと比較するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="95156-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="95156-112">資格情報が一致した場合、ユーザーは正常に認証され、承認プロセス中に、承認されたアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="95156-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="95156-113">承認とは、ユーザーに許可する実行内容を決定するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="95156-113">The authorization refers to the process that determines what a user is allowed to do.</span></span> 

<span data-ttu-id="95156-114">また、認証はサーバー、データベース、アプリまたはリソースなどの領域に入る方法と見なすことができます。一方、承認はユーザーがその領域 (サーバー、データベース、またはアプリ) 内のいずかのオブジェクトに対して実行できるアクションです。</span><span class="sxs-lookup"><span data-stu-id="95156-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="95156-115">ソフトウェアの一般的な脆弱性</span><span class="sxs-lookup"><span data-stu-id="95156-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="95156-116">ASP.NET Core および EF には、アプリをセキュリティで保護し、セキュリティ違反を防止するのに役立つ機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="95156-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="95156-117">以下にリストされているリンクから、Web アプリの最も一般的なセキュリティの脆弱性を回避するための手法の詳細を示すドキュメントにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="95156-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="95156-118">クロスサイト スクリプト攻撃</span><span class="sxs-lookup"><span data-stu-id="95156-118">Cross-site scripting attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [<span data-ttu-id="95156-119">SQL インジェクション攻撃</span><span class="sxs-lookup"><span data-stu-id="95156-119">SQL injection attacks</span></span>](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [<span data-ttu-id="95156-120">クロスサイト リクエスト フォージェリ (CSRF)</span><span class="sxs-lookup"><span data-stu-id="95156-120">Cross-Site Request Forgery (CSRF)</span></span>](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [<span data-ttu-id="95156-121">オープン リダイレクト攻撃</span><span class="sxs-lookup"><span data-stu-id="95156-121">Open redirect attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

<span data-ttu-id="95156-122">この他にも知っておく必要がある脆弱性はあります。</span><span class="sxs-lookup"><span data-stu-id="95156-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="95156-123">詳細については、このドキュメントの「*ASP.NET セキュリティに関するドキュメント*」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="95156-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span> 

## <a name="aspnet-security-documentation"></a><span data-ttu-id="95156-124">ASP.NET セキュリティに関するドキュメント</span><span class="sxs-lookup"><span data-stu-id="95156-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="95156-125">認証</span><span class="sxs-lookup"><span data-stu-id="95156-125">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="95156-126">Identity の概要</span><span class="sxs-lookup"><span data-stu-id="95156-126">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="95156-127">Facebook、Google、および他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="95156-127">Enable authentication using Facebook, Google, and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="95156-128">Windows 認証の構成</span><span class="sxs-lookup"><span data-stu-id="95156-128">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="95156-129">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="95156-129">Account confirmation and password recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="95156-130">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="95156-130">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="95156-131">Identity なしでの Cookie 認証の使用</span><span class="sxs-lookup"><span data-stu-id="95156-131">Use cookie authentication without Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="95156-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95156-132">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="95156-133">ASP.NET Core Web アプリへの Azure AD の統合</span><span class="sxs-lookup"><span data-stu-id="95156-133">Integrate Azure AD into an ASP.NET Core web app</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="95156-134">Azure AD を使用した WPF アプリからの ASP.NET Core Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="95156-134">Call an ASP.NET Core Web API from a WPF app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="95156-135">Azure AD を使用した ASP.NET Core Web アプリでの Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="95156-135">Call a Web API in an ASP.NET Core web app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="95156-136">ASP.NET Core Web アプリでの Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="95156-136">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="95156-137">IdentityServer4 での ASP.NET Core アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="95156-137">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="95156-138">承認</span><span class="sxs-lookup"><span data-stu-id="95156-138">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="95156-139">はじめに</span><span class="sxs-lookup"><span data-stu-id="95156-139">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="95156-140">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="95156-140">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="95156-141">単純な承認</span><span class="sxs-lookup"><span data-stu-id="95156-141">Simple authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="95156-142">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="95156-142">Role-based authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="95156-143">クレーム ベースの承認</span><span class="sxs-lookup"><span data-stu-id="95156-143">Claims-based authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="95156-144">ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="95156-144">Policy-based authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="95156-145">要件ハンドラーでの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="95156-145">Dependency injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="95156-146">リソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="95156-146">Resource-based authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="95156-147">ビュー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="95156-147">View-based authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="95156-148">スキームによる ID の制限</span><span class="sxs-lookup"><span data-stu-id="95156-148">Limit identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="95156-149">データ保護</span><span class="sxs-lookup"><span data-stu-id="95156-149">Data protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="95156-150">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="95156-150">Introduction to data protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="95156-151">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="95156-151">Get started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="95156-152">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="95156-152">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="95156-153">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="95156-153">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="95156-154">目的文字列</span><span class="sxs-lookup"><span data-stu-id="95156-154">Purpose strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="95156-155">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="95156-155">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="95156-156">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="95156-156">Password hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="95156-157">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="95156-157">Limit the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="95156-158">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="95156-158">Unprotect payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="95156-159">構成</span><span class="sxs-lookup"><span data-stu-id="95156-159">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="95156-160">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="95156-160">Configure data protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="95156-161">既定の設定</span><span class="sxs-lookup"><span data-stu-id="95156-161">Default settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="95156-162">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="95156-162">Machine-wide policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="95156-163">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="95156-163">Non DI-aware scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="95156-164">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="95156-164">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="95156-165">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="95156-165">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="95156-166">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="95156-166">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="95156-167">その他の API</span><span class="sxs-lookup"><span data-stu-id="95156-167">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="95156-168">実装</span><span class="sxs-lookup"><span data-stu-id="95156-168">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="95156-169">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="95156-169">Authenticated encryption details</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="95156-170">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="95156-170">Subkey derivation and authenticated encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="95156-171">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="95156-171">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="95156-172">キーの管理</span><span class="sxs-lookup"><span data-stu-id="95156-172">Key management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="95156-173">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="95156-173">Key storage providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="95156-174">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="95156-174">Key encryption at rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="95156-175">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="95156-175">Key immutability and changing settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="95156-176">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="95156-176">Key storage format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="95156-177">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="95156-177">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="95156-178">互換性</span><span class="sxs-lookup"><span data-stu-id="95156-178">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="95156-179">アプリ間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="95156-179">Share cookies among apps</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="95156-180">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="95156-180">Replace <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="95156-181">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="95156-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="95156-182">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="95156-182">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="95156-183">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="95156-183">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="95156-184">SSL の適用</span><span class="sxs-lookup"><span data-stu-id="95156-184">Enforce SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="95156-185">リクエスト フォージェリの対策</span><span class="sxs-lookup"><span data-stu-id="95156-185">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="95156-186">オープン リダイレクト攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="95156-186">Prevent open redirect attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="95156-187">クロスサイト スクリプティングの防止</span><span class="sxs-lookup"><span data-stu-id="95156-187">Prevent Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="95156-188">クロスオリジン要求 (CORS) の有効化</span><span class="sxs-lookup"><span data-stu-id="95156-188">Enable Cross-Origin Requests (CORS)</span></span>](cors.md)
