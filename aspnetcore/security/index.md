---
title: "セキュリティ"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a><span data-ttu-id="6c80e-103">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="6c80e-103">Security</span></span>

*   [<span data-ttu-id="6c80e-104">認証</span><span class="sxs-lookup"><span data-stu-id="6c80e-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="6c80e-105">Identity の概要</span><span class="sxs-lookup"><span data-stu-id="6c80e-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="6c80e-106">Facebook、Google、および他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="6c80e-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="6c80e-107">Windows 認証の構成</span><span class="sxs-lookup"><span data-stu-id="6c80e-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="6c80e-108">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="6c80e-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="6c80e-109">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="6c80e-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="6c80e-110">ASP.NET Core Identity なしでの Cookie 認証の使用</span><span class="sxs-lookup"><span data-stu-id="6c80e-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="6c80e-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c80e-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="6c80e-112">ASP.NET Core Web アプリへの Azure AD の統合</span><span class="sxs-lookup"><span data-stu-id="6c80e-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="6c80e-113">Azure AD を使用した WPF アプリケーションからの ASP.NET Core Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="6c80e-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="6c80e-114">Azure AD を使用した ASP.NET Core Web アプリケーションでの Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="6c80e-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="6c80e-115">ASP.NET Core Web アプリでの Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="6c80e-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="6c80e-116">IdentityServer4 での ASP.NET Core アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="6c80e-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="6c80e-117">承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="6c80e-118">はじめに</span><span class="sxs-lookup"><span data-stu-id="6c80e-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="6c80e-119">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="6c80e-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="6c80e-120">単純な承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="6c80e-121">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="6c80e-122">クレーム ベースの承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="6c80e-123">カスタム ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="6c80e-124">要件ハンドラーでの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="6c80e-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="6c80e-125">リソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="6c80e-126">ビュー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="6c80e-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="6c80e-127">スキームによる ID の制限</span><span class="sxs-lookup"><span data-stu-id="6c80e-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="6c80e-128">データの保護</span><span class="sxs-lookup"><span data-stu-id="6c80e-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="6c80e-129">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="6c80e-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="6c80e-130">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="6c80e-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="6c80e-131">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="6c80e-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="6c80e-132">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="6c80e-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="6c80e-133">目的文字列</span><span class="sxs-lookup"><span data-stu-id="6c80e-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="6c80e-134">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="6c80e-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="6c80e-135">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="6c80e-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="6c80e-136">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="6c80e-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="6c80e-137">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="6c80e-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="6c80e-138">構成</span><span class="sxs-lookup"><span data-stu-id="6c80e-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="6c80e-139">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="6c80e-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="6c80e-140">既定の設定</span><span class="sxs-lookup"><span data-stu-id="6c80e-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="6c80e-141">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="6c80e-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="6c80e-142">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="6c80e-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="6c80e-143">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="6c80e-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="6c80e-144">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="6c80e-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="6c80e-145">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="6c80e-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="6c80e-146">その他の API</span><span class="sxs-lookup"><span data-stu-id="6c80e-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="6c80e-147">実装</span><span class="sxs-lookup"><span data-stu-id="6c80e-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="6c80e-148">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="6c80e-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="6c80e-149">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="6c80e-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="6c80e-150">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="6c80e-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="6c80e-151">キーの管理</span><span class="sxs-lookup"><span data-stu-id="6c80e-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="6c80e-152">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="6c80e-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="6c80e-153">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="6c80e-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="6c80e-154">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="6c80e-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="6c80e-155">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="6c80e-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="6c80e-156">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="6c80e-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="6c80e-157">互換性</span><span class="sxs-lookup"><span data-stu-id="6c80e-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="6c80e-158">アプリケーション間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="6c80e-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="6c80e-159">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="6c80e-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="6c80e-160">承認によって保護されたユーザー データでのアプリの作成</span><span class="sxs-lookup"><span data-stu-id="6c80e-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="6c80e-161">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="6c80e-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="6c80e-162">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="6c80e-162">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="6c80e-163">SSL の適用</span><span class="sxs-lookup"><span data-stu-id="6c80e-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="6c80e-164">開発用の HTTPS の設定</span><span class="sxs-lookup"><span data-stu-id="6c80e-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="6c80e-165">リクエスト フォージェリの対策</span><span class="sxs-lookup"><span data-stu-id="6c80e-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="6c80e-166">オープン リダイレクト攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="6c80e-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="6c80e-167">クロスサイト スクリプティングの防止</span><span class="sxs-lookup"><span data-stu-id="6c80e-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="6c80e-168">クロスオリジン要求 (CORS) の有効化</span><span class="sxs-lookup"><span data-stu-id="6c80e-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
