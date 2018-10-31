---
title: ASP.NET Core Security の概要
author: tdykstra
description: ASP.NET Core での認証、承認、およびセキュリティの基本を学習します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 4277266e20ab1921a2ba24d4500358ba90330370
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252946"
---
# <a name="overview-of-aspnet-core-security"></a><span data-ttu-id="e8f46-103">ASP.NET Core Security の概要</span><span class="sxs-lookup"><span data-stu-id="e8f46-103">Overview of ASP.NET Core Security</span></span>

<span data-ttu-id="e8f46-104">ASP.NET Core を使用することで、開発者はアプリのセキュリティを簡単に構成して管理することができます。</span><span class="sxs-lookup"><span data-stu-id="e8f46-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="e8f46-105">ASP.NET Core には、認証、承認、データ保護、SSL の適用、アプリ シークレット、リクエスト フォージェリの対策保護、および CORS を管理するための機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e8f46-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="e8f46-106">これらのセキュリティ機能を使用すれば、堅牢かつセキュアな ASP.NET Core アプリを構築できます。</span><span class="sxs-lookup"><span data-stu-id="e8f46-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span>

## <a name="aspnet-core-security-features"></a><span data-ttu-id="e8f46-107">ASP.NET Core セキュリティ機能</span><span class="sxs-lookup"><span data-stu-id="e8f46-107">ASP.NET Core security features</span></span>

<span data-ttu-id="e8f46-108">ASP.NET Core では、組み込みの ID プロバイダーを含むアプリをセキュリティで保護するための多くのツールとライブラリが提供されますが、Facebook、Twitter、LinkedIn などのサードパーティの ID サービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e8f46-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="e8f46-109">ASP.NET Core では、コードで公開せずに機密情報を格納して使用する方法である、アプリ シークレットを簡単に管理できます。</span><span class="sxs-lookup"><span data-stu-id="e8f46-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span>

## <a name="authentication-vs-authorization"></a><span data-ttu-id="e8f46-110">認証と承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="e8f46-111">認証とは、ユーザーが提供した資格情報をオペレーティング システム、データベース、アプリまたはリソースに格納されているものと比較するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="e8f46-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="e8f46-112">資格情報が一致した場合、ユーザーは正常に認証され、承認プロセス中に、承認されたアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="e8f46-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="e8f46-113">承認とは、ユーザーに許可する実行内容を決定するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="e8f46-113">The authorization refers to the process that determines what a user is allowed to do.</span></span>

<span data-ttu-id="e8f46-114">また、認証はサーバー、データベース、アプリまたはリソースなどの領域に入る方法と見なすことができます。一方、承認はユーザーがその領域 (サーバー、データベース、またはアプリ) 内のいずかのオブジェクトに対して実行できるアクションです。</span><span class="sxs-lookup"><span data-stu-id="e8f46-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="e8f46-115">ソフトウェアの一般的な脆弱性</span><span class="sxs-lookup"><span data-stu-id="e8f46-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="e8f46-116">ASP.NET Core および EF には、アプリをセキュリティで保護し、セキュリティ違反を防止するのに役立つ機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e8f46-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="e8f46-117">以下にリストされているリンクから、Web アプリの最も一般的なセキュリティの脆弱性を回避するための手法の詳細を示すドキュメントにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="e8f46-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="e8f46-118">クロスサイト スクリプト攻撃</span><span class="sxs-lookup"><span data-stu-id="e8f46-118">Cross-site scripting attacks</span></span>](xref:security/cross-site-scripting)
* [<span data-ttu-id="e8f46-119">SQL インジェクション攻撃</span><span class="sxs-lookup"><span data-stu-id="e8f46-119">SQL injection attacks</span></span>](/ef/core/querying/raw-sql)
* [<span data-ttu-id="e8f46-120">クロスサイト リクエスト フォージェリ (CSRF)</span><span class="sxs-lookup"><span data-stu-id="e8f46-120">Cross-Site Request Forgery (CSRF)</span></span>](xref:security/anti-request-forgery)
* [<span data-ttu-id="e8f46-121">オープン リダイレクト攻撃</span><span class="sxs-lookup"><span data-stu-id="e8f46-121">Open redirect attacks</span></span>](xref:security/preventing-open-redirects)

<span data-ttu-id="e8f46-122">この他にも知っておく必要がある脆弱性はあります。</span><span class="sxs-lookup"><span data-stu-id="e8f46-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="e8f46-123">詳細については、このドキュメントの「*ASP.NET Core セキュリティに関するドキュメント*」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e8f46-123">For more information, see the section in this document on *ASP.NET Core Security Documentation*.</span></span>

## <a name="aspnet-core-security-documentation"></a><span data-ttu-id="e8f46-124">ASP.NET Core セキュリティに関するドキュメント</span><span class="sxs-lookup"><span data-stu-id="e8f46-124">ASP.NET Core Security Documentation</span></span>

* <span data-ttu-id="e8f46-125">認証</span><span class="sxs-lookup"><span data-stu-id="e8f46-125">Authentication</span></span>
  * [<span data-ttu-id="e8f46-126">Identity の概要</span><span class="sxs-lookup"><span data-stu-id="e8f46-126">Introduction to Identity</span></span>](xref:security/authentication/identity)
  * [<span data-ttu-id="e8f46-127">Facebook、Google、および他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="e8f46-127">Enable authentication using Facebook, Google, and other external providers</span></span>](xref:security/authentication/social/index)
  * [<span data-ttu-id="e8f46-128">WS フェデレーションを使用した認証の有効化</span><span class="sxs-lookup"><span data-stu-id="e8f46-128">Enable authentication with WS-Federation</span></span>](xref:security/authentication/ws-federation)
  * [<span data-ttu-id="e8f46-129">Windows 認証の構成</span><span class="sxs-lookup"><span data-stu-id="e8f46-129">Configure Windows Authentication</span></span>](xref:security/authentication/windowsauth)
  * [<span data-ttu-id="e8f46-130">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="e8f46-130">Account confirmation and password recovery</span></span>](xref:security/authentication/accconfirm)
  * [<span data-ttu-id="e8f46-131">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="e8f46-131">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
  * [<span data-ttu-id="e8f46-132">Identity なしでの Cookie 認証の使用</span><span class="sxs-lookup"><span data-stu-id="e8f46-132">Use cookie authentication without Identity</span></span>](xref:security/authentication/cookie)
  * [<span data-ttu-id="e8f46-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e8f46-133">Azure Active Directory</span></span>](xref:security/authentication/azure-active-directory/index)
    * [<span data-ttu-id="e8f46-134">ASP.NET Core Web アプリへの Azure AD の統合</span><span class="sxs-lookup"><span data-stu-id="e8f46-134">Integrate Azure AD into an ASP.NET Core web app</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
    * [<span data-ttu-id="e8f46-135">Azure AD を使用した WPF アプリからの ASP.NET Core Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="e8f46-135">Call an ASP.NET Core Web API from a WPF app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
    * [<span data-ttu-id="e8f46-136">Azure AD を使用した ASP.NET Core Web アプリでの Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="e8f46-136">Call a Web API in an ASP.NET Core web app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
    * [<span data-ttu-id="e8f46-137">ASP.NET Core Web アプリでの Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="e8f46-137">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
  * [<span data-ttu-id="e8f46-138">IdentityServer4 での ASP.NET Core アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="e8f46-138">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
* <span data-ttu-id="e8f46-139">承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-139">Authorization</span></span>
  * [<span data-ttu-id="e8f46-140">はじめに</span><span class="sxs-lookup"><span data-stu-id="e8f46-140">Introduction</span></span>](xref:security/authorization/introduction)
  * [<span data-ttu-id="e8f46-141">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="e8f46-141">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
  * [<span data-ttu-id="e8f46-142">単純な承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-142">Simple authorization</span></span>](xref:security/authorization/simple)
  * [<span data-ttu-id="e8f46-143">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-143">Role-based authorization</span></span>](xref:security/authorization/roles)
  * [<span data-ttu-id="e8f46-144">クレーム ベースの承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-144">Claims-based authorization</span></span>](xref:security/authorization/claims)
  * [<span data-ttu-id="e8f46-145">ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-145">Policy-based authorization</span></span>](xref:security/authorization/policies)
  * [<span data-ttu-id="e8f46-146">要件ハンドラーでの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="e8f46-146">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
  * [<span data-ttu-id="e8f46-147">リソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-147">Resource-based authorization</span></span>](xref:security/authorization/resourcebased)
  * [<span data-ttu-id="e8f46-148">ビュー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="e8f46-148">View-based authorization</span></span>](xref:security/authorization/views)
  * [<span data-ttu-id="e8f46-149">スキームによる ID の制限</span><span class="sxs-lookup"><span data-stu-id="e8f46-149">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* <span data-ttu-id="e8f46-150">データの保護</span><span class="sxs-lookup"><span data-stu-id="e8f46-150">Data protection</span></span>
  * [<span data-ttu-id="e8f46-151">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="e8f46-151">Introduction to data protection</span></span>](xref:security/data-protection/introduction)
  * [<span data-ttu-id="e8f46-152">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="e8f46-152">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)
  * <span data-ttu-id="e8f46-153">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="e8f46-153">Consumer APIs</span></span>
    * [<span data-ttu-id="e8f46-154">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="e8f46-154">Consumer APIs Overview</span></span>](xref:security/data-protection/consumer-apis/overview)
    * [<span data-ttu-id="e8f46-155">目的文字列</span><span class="sxs-lookup"><span data-stu-id="e8f46-155">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)
    * [<span data-ttu-id="e8f46-156">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="e8f46-156">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
    * [<span data-ttu-id="e8f46-157">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="e8f46-157">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)
    * [<span data-ttu-id="e8f46-158">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="e8f46-158">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
    * [<span data-ttu-id="e8f46-159">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="e8f46-159">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)
  * [<span data-ttu-id="e8f46-160">構成</span><span class="sxs-lookup"><span data-stu-id="e8f46-160">Configuration</span></span>](xref:security/data-protection/configuration/index)
    * [<span data-ttu-id="e8f46-161">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="e8f46-161">Configure data protection</span></span>](xref:security/data-protection/configuration/overview)
    * [<span data-ttu-id="e8f46-162">既定の設定</span><span class="sxs-lookup"><span data-stu-id="e8f46-162">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)
    * [<span data-ttu-id="e8f46-163">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="e8f46-163">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
    * [<span data-ttu-id="e8f46-164">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="e8f46-164">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
  * [<span data-ttu-id="e8f46-165">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="e8f46-165">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)
    * [<span data-ttu-id="e8f46-166">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="e8f46-166">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)
    * [<span data-ttu-id="e8f46-167">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="e8f46-167">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)
    * [<span data-ttu-id="e8f46-168">その他の API</span><span class="sxs-lookup"><span data-stu-id="e8f46-168">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)
  * [<span data-ttu-id="e8f46-169">実装</span><span class="sxs-lookup"><span data-stu-id="e8f46-169">Implementation</span></span>](xref:security/data-protection/implementation/index)
    * [<span data-ttu-id="e8f46-170">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="e8f46-170">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)
    * [<span data-ttu-id="e8f46-171">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="e8f46-171">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)
    * [<span data-ttu-id="e8f46-172">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="e8f46-172">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)
    * [<span data-ttu-id="e8f46-173">キーの管理</span><span class="sxs-lookup"><span data-stu-id="e8f46-173">Key management</span></span>](xref:security/data-protection/implementation/key-management)
    * [<span data-ttu-id="e8f46-174">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e8f46-174">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)
    * [<span data-ttu-id="e8f46-175">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="e8f46-175">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)
    * [<span data-ttu-id="e8f46-176">キーの不変性と設定</span><span class="sxs-lookup"><span data-stu-id="e8f46-176">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)
    * [<span data-ttu-id="e8f46-177">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="e8f46-177">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)
    * [<span data-ttu-id="e8f46-178">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e8f46-178">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)
  * [<span data-ttu-id="e8f46-179">互換性</span><span class="sxs-lookup"><span data-stu-id="e8f46-179">Compatibility</span></span>](xref:security/data-protection/compatibility/index)
    * [<span data-ttu-id="e8f46-180">ASP.NET の \<machineKey> を置換する</span><span class="sxs-lookup"><span data-stu-id="e8f46-180">Replace \<machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
* [<span data-ttu-id="e8f46-181">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="e8f46-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
* [<span data-ttu-id="e8f46-182">開発中のアプリ シークレットの安全な格納</span><span class="sxs-lookup"><span data-stu-id="e8f46-182">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="e8f46-183">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e8f46-183">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
* [<span data-ttu-id="e8f46-184">SSL の適用</span><span class="sxs-lookup"><span data-stu-id="e8f46-184">Enforce SSL</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="e8f46-185">リクエスト フォージェリの対策</span><span class="sxs-lookup"><span data-stu-id="e8f46-185">Anti-Request Forgery</span></span>](xref:security/anti-request-forgery)
* [<span data-ttu-id="e8f46-186">オープン リダイレクト攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="e8f46-186">Prevent open redirect attacks</span></span>](xref:security/preventing-open-redirects)
* [<span data-ttu-id="e8f46-187">クロスサイト スクリプティングの防止</span><span class="sxs-lookup"><span data-stu-id="e8f46-187">Prevent Cross-Site Scripting</span></span>](xref:security/cross-site-scripting)
* [<span data-ttu-id="e8f46-188">クロスオリジン要求 (CORS) の有効化</span><span class="sxs-lookup"><span data-stu-id="e8f46-188">Enable Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="e8f46-189">アプリ間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="e8f46-189">Share cookies among apps</span></span>](xref:security/cookie-sharing)
* [<span data-ttu-id="e8f46-190">IP セーフリスト</span><span class="sxs-lookup"><span data-stu-id="e8f46-190">IP safelist</span></span>](xref:security/ip-safelist)
