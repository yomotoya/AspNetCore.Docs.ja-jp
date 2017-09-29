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
ms.openlocfilehash: f173d03f55a1ce52222a75c023f9e8a20d5c60dc
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="security"></a>セキュリティ

*   [認証](authentication/index.md)
    *   [Identity の概要](authentication/identity.md)
    *   [Facebook、Google、および他の外部プロバイダーを使用する認証の有効化](authentication/social/index.md)
    * [Windows 認証の構成](authentication/windowsauth.md)
    *   [アカウントの確認とパスワードの回復](authentication/accconfirm.md)
    *   [SMS での 2 要素認証](authentication/2fa.md) 
    *   [ASP.NET Core Identity なしでの Cookie 認証の使用](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [ASP.NET Core Web アプリへの Azure AD の統合](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD を使用した WPF アプリケーションからの ASP.NET Core Web API の呼び出し](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD を使用した ASP.NET Core Web アプリケーションでの Web API の呼び出し](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [ASP.NET Core Web アプリでの Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [IdentityServer4 での ASP.NET Core アプリのセキュリティ保護](https://identityserver4.readthedocs.io)
*   [承認](authorization/index.md)
    *   [はじめに](authorization/introduction.md)
    *   [承認によって保護されたユーザー データでのアプリの作成](xref:security/authorization/secure-data)
    *   [単純な承認](authorization/simple.md)
    *   [ロール ベースの承認](authorization/roles.md)
    *   [クレーム ベースの承認](authorization/claims.md)
    *   [カスタム ポリシー ベースの承認](authorization/policies.md)
    *   [要件ハンドラーでの依存性の注入](authorization/dependencyinjection.md)
    *   [リソース ベースの承認](authorization/resourcebased.md)
    *   [ビュー ベースの承認](authorization/views.md)
    *   [スキームによる ID の制限](authorization/limitingidentitybyscheme.md)
*   [データの保護](data-protection/index.md)
    *   [データ保護の概要](data-protection/introduction.md)
    *   [データ保護 API の概要](data-protection/using-data-protection.md)
    *   [コンシューマー API](data-protection/consumer-apis/index.md)
        *   [コンシューマー API の概要](data-protection/consumer-apis/overview.md)
        *   [目的文字列](data-protection/consumer-apis/purpose-strings.md)
        *   [目的の階層とマルチテナント](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [パスワードのハッシュ](data-protection/consumer-apis/password-hashing.md)
        *   [保護されたペイロードの有効期間の制限](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [キーが取り消されたペイロードの保護の解除](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [構成](data-protection/configuration/index.md)
        *   [データ保護の構成](data-protection/configuration/overview.md)
        *   [既定の設定](data-protection/configuration/default-settings.md)
        *   [コンピューター全体のポリシー](data-protection/configuration/machine-wide-policy.md)
        *   [DI に対応しないシナリオ](data-protection/configuration/non-di-scenarios.md)
    *   [拡張性 API](data-protection/extensibility/index.md)
        *   [Core の暗号の拡張性](data-protection/extensibility/core-crypto.md)
        *   [キー管理の拡張性](data-protection/extensibility/key-management.md)
        *   [その他の API](data-protection/extensibility/misc-apis.md)
    *   [実装](data-protection/implementation/index.md)
        *   [認証された暗号化の詳細](data-protection/implementation/authenticated-encryption-details.md)
        *   [サブキーの派生と認証された暗号化](data-protection/implementation/subkeyderivation.md)
        *   [コンテキスト ヘッダー](data-protection/implementation/context-headers.md)
        *   [キーの管理](data-protection/implementation/key-management.md)
        *   [キー ストレージ プロバイダー](data-protection/implementation/key-storage-providers.md)
        *   [保存時のキーの暗号化](data-protection/implementation/key-encryption-at-rest.md)
        *   [キーの不変性と設定の変更](data-protection/implementation/key-immutability.md)
        *   [キー ストレージの形式](data-protection/implementation/key-storage-format.md)
        *   [短期データ保護プロバイダー](data-protection/implementation/key-storage-ephemeral.md)
    *   [互換性](data-protection/compatibility/index.md)
        *   [アプリケーション間での Cookie の共有](data-protection/compatibility/cookie-sharing.md)
        *   [ASP.NET での <machineKey> の置換](data-protection/compatibility/replacing-machinekey.md)
*   [承認によって保護されたユーザー データでのアプリの作成](xref:security/authorization/secure-data)
*   [開発中のアプリ シークレットの安全な保存](app-secrets.md)
*   [Azure Key Vault 構成プロバイダー](key-vault-configuration.md)
*   [SSL の適用](enforcing-ssl.md)
*   [リクエスト フォージェリの対策](anti-request-forgery.md)
*   [オープン リダイレクト攻撃の防止](preventing-open-redirects.md)
*   [クロスサイト スクリプティングの防止](cross-site-scripting.md)
*   [クロスオリジン要求 (CORS) の有効化](cors.md)
