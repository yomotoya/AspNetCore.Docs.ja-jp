---
title: "ASP.NET Core セキュリティの概要 | Microsoft Docs"
author: rachelappel
description: "ASP.NET Core での認証、承認、およびセキュリティの基本を学習します"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 71bde77e0bc5698b670b560455301cae642165a6
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-security-overview"></a>ASP.NET Core セキュリティの概要

ASP.NET Core を使用することで、開発者はアプリのセキュリティを簡単に構成して管理することができます。 ASP.NET Core には、認証、承認、データ保護、SSL の適用、アプリ シークレット、リクエスト フォージェリの対策保護、および CORS を管理するための機能が含まれています。 これらのセキュリティ機能を使用すれば、堅牢かつセキュアな ASP.NET Core アプリを構築できます。 

## <a name="aspnet-core-security-features"></a>ASP.NET Core セキュリティ機能

ASP.NET Core では、組み込みの ID プロバイダーを含むアプリをセキュリティで保護するための多くのツールとライブラリが提供されますが、Facebook、Twitter、LinkedIn などのサードパーティの ID サービスを使用することもできます。 ASP.NET Core では、コードで公開せずに機密情報を格納して使用する方法である、アプリ シークレットを簡単に管理できます。 

## <a name="authentication-vs-authorization"></a>認証と承認

認証とは、ユーザーが提供した資格情報をオペレーティング システム、データベース、アプリまたはリソースに格納されているものと比較するプロセスのことです。 資格情報が一致した場合、ユーザーは正常に認証され、承認プロセス中に、承認されたアクションを実行できます。 承認とは、ユーザーに許可する実行内容を決定するプロセスのことです。 

また、認証はサーバー、データベース、アプリまたはリソースなどの領域に入る方法と見なすことができます。一方、承認はユーザーがその領域 (サーバー、データベース、またはアプリ) 内のいずかのオブジェクトに対して実行できるアクションです。

## <a name="common-vulnerabilities-in-software"></a>ソフトウェアの一般的な脆弱性

ASP.NET Core および EF には、アプリをセキュリティで保護し、セキュリティ違反を防止するのに役立つ機能が含まれています。 以下にリストされているリンクから、Web アプリの最も一般的なセキュリティの脆弱性を回避するための手法の詳細を示すドキュメントにアクセスできます。

* [クロスサイト スクリプト攻撃](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [SQL インジェクション攻撃](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [クロスサイト リクエスト フォージェリ (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [オープン リダイレクト攻撃](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

この他にも知っておく必要がある脆弱性はあります。 詳細については、このドキュメントの「*ASP.NET セキュリティに関するドキュメント*」セクションを参照してください。 

## <a name="aspnet-security-documentation"></a>ASP.NET セキュリティに関するドキュメント

*   [認証](authentication/index.md)
    *   [Identity の概要](authentication/identity.md)
    *   [Facebook、Google、および他の外部プロバイダーを使用する認証の有効化](authentication/social/index.md)
    * [Windows 認証の構成](authentication/windowsauth.md)
    *   [アカウントの確認とパスワードの回復](authentication/accconfirm.md)
    *   [SMS での 2 要素認証](authentication/2fa.md) 
    *   [Identity なしでの Cookie 認証の使用](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [ASP.NET Core Web アプリへの Azure AD の統合](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD を使用した WPF アプリからの ASP.NET Core Web API の呼び出し](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD を使用した ASP.NET Core Web アプリでの Web API の呼び出し](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [ASP.NET Core Web アプリでの Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [IdentityServer4 での ASP.NET Core アプリのセキュリティ保護](https://identityserver4.readthedocs.io)
*   [承認](authorization/index.md)
    *   [はじめに](authorization/introduction.md)
    *   [承認によって保護されたユーザー データでのアプリの作成](xref:security/authorization/secure-data)
    *   [単純な承認](authorization/simple.md)
    *   [ロール ベースの承認](authorization/roles.md)
    *   [クレーム ベースの承認](authorization/claims.md)
    *   [ポリシー ベースの承認](authorization/policies.md)
    *   [要件ハンドラーでの依存性の注入](authorization/dependencyinjection.md)
    *   [リソース ベースの承認](authorization/resourcebased.md)
    *   [ビュー ベースの承認](authorization/views.md)
    *   [スキームによる ID の制限](authorization/limitingidentitybyscheme.md)
*   [データ保護](data-protection/index.md)
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
