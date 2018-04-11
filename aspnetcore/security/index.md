---
title: ASP.NET Core Security の概要
author: rachelappel
description: ASP.NET Core での認証、承認、およびセキュリティの基本を学習します。
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: da3829b2d5ae5db1861c7423da5afc7acbee6697
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="overview-of-aspnet-core-security"></a>ASP.NET Core Security の概要

ASP.NET Core を使用することで、開発者はアプリのセキュリティを簡単に構成して管理することができます。 ASP.NET Core には、認証、承認、データ保護、SSL の適用、アプリ シークレット、リクエスト フォージェリの対策保護、および CORS を管理するための機能が含まれています。 これらのセキュリティ機能を使用すれば、堅牢かつセキュアな ASP.NET Core アプリを構築できます。

## <a name="aspnet-core-security-features"></a>ASP.NET Core セキュリティ機能

ASP.NET Core では、組み込みの ID プロバイダーを含むアプリをセキュリティで保護するための多くのツールとライブラリが提供されますが、Facebook、Twitter、LinkedIn などのサードパーティの ID サービスを使用することもできます。 ASP.NET Core では、コードで公開せずに機密情報を格納して使用する方法である、アプリ シークレットを簡単に管理できます。

## <a name="authentication-vs-authorization"></a>認証と承認

認証とは、ユーザーが提供した資格情報をオペレーティング システム、データベース、アプリまたはリソースに格納されているものと比較するプロセスのことです。 資格情報が一致した場合、ユーザーは正常に認証され、承認プロセス中に、承認されたアクションを実行できます。 承認とは、ユーザーに許可する実行内容を決定するプロセスのことです。

また、認証はサーバー、データベース、アプリまたはリソースなどの領域に入る方法と見なすことができます。一方、承認はユーザーがその領域 (サーバー、データベース、またはアプリ) 内のいずかのオブジェクトに対して実行できるアクションです。

## <a name="common-vulnerabilities-in-software"></a>ソフトウェアの一般的な脆弱性

ASP.NET Core および EF には、アプリをセキュリティで保護し、セキュリティ違反を防止するのに役立つ機能が含まれています。 以下にリストされているリンクから、Web アプリの最も一般的なセキュリティの脆弱性を回避するための手法の詳細を示すドキュメントにアクセスできます。

* [クロスサイト スクリプト攻撃](xref:security/cross-site-scripting)
* [SQL インジェクション攻撃](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [クロスサイト リクエスト フォージェリ (CSRF)](xref:security/anti-request-forgery)
* [オープン リダイレクト攻撃](xref:security/preventing-open-redirects)

この他にも知っておく必要がある脆弱性はあります。 詳細については、このドキュメントの「*ASP.NET セキュリティに関するドキュメント*」セクションを参照してください。

## <a name="aspnet-security-documentation"></a>ASP.NET セキュリティに関するドキュメント

*   [認証](xref:security/authentication/index)
    *   [Identity の概要](xref:security/authentication/identity)
    *   [Facebook、Google、および他の外部プロバイダーを使用する認証の有効化](xref:security/authentication/social/index)
    *   [WS フェデレーションを使用した認証の有効化](xref:security/authentication/ws-federation)
    * [Windows 認証の構成](xref:security/authentication/windowsauth)
    *   [アカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)
    *   [SMS での 2 要素認証](xref:security/authentication/2fa)
    *   [Identity なしでの Cookie 認証の使用](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [ASP.NET Core Web アプリへの Azure AD の統合](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD を使用した WPF アプリからの ASP.NET Core Web API の呼び出し](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD を使用した ASP.NET Core Web アプリでの Web API の呼び出し](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [ASP.NET Core Web アプリでの Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [IdentityServer4 での ASP.NET Core アプリのセキュリティ保護](https://identityserver4.readthedocs.io)
*   [承認](xref:security/authorization/index)
    *   [はじめに](xref:security/authorization/introduction)
    *   [承認によって保護されたユーザー データでのアプリの作成](xref:security/authorization/secure-data)
    *   [単純な承認](xref:security/authorization/simple)
    *   [ロール ベースの承認](xref:security/authorization/roles)
    *   [クレーム ベースの承認](xref:security/authorization/claims)
    *   [ポリシー ベースの承認](xref:security/authorization/policies)
    *   [要件ハンドラーでの依存性の注入](xref:security/authorization/dependencyinjection)
    *   [リソース ベースの承認](xref:security/authorization/resourcebased)
    *   [ビュー ベースの承認](xref:security/authorization/views)
    *   [スキームによる ID の制限](xref:security/authorization/limitingidentitybyscheme)
*   [データ保護](xref:security/data-protection/index)
    *   [データ保護の概要](xref:security/data-protection/introduction)
    *   [データ保護 API の概要](xref:security/data-protection/using-data-protection)
    *   [コンシューマー API](xref:security/data-protection/consumer-apis/index)
        *   [コンシューマー API の概要](xref:security/data-protection/consumer-apis/overview)
        *   [目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [目的の階層とマルチテナント](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [パスワードのハッシュ](xref:security/data-protection/consumer-apis/password-hashing)
        *   [保護されたペイロードの有効期間の制限](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [キーが取り消されたペイロードの保護の解除](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [構成](xref:security/data-protection/configuration/index)
        *   [データ保護の構成](xref:security/data-protection/configuration/overview)
        *   [既定の設定](xref:security/data-protection/configuration/default-settings)
        *   [コンピューター全体のポリシー](xref:security/data-protection/configuration/machine-wide-policy)
        *   [DI に対応しないシナリオ](xref:security/data-protection/configuration/non-di-scenarios)
    *   [拡張性 API](xref:security/data-protection/extensibility/index)
        *   [Core の暗号の拡張性](xref:security/data-protection/extensibility/core-crypto)
        *   [キー管理の拡張性](xref:security/data-protection/extensibility/key-management)
        *   [その他の API](xref:security/data-protection/extensibility/misc-apis)
    *   [実装](xref:security/data-protection/implementation/index)
        *   [認証された暗号化の詳細](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [サブキーの派生と認証された暗号化](xref:security/data-protection/implementation/subkeyderivation)
        *   [コンテキスト ヘッダー](xref:security/data-protection/implementation/context-headers)
        *   [キーの管理](xref:security/data-protection/implementation/key-management)
        *   [キー ストレージ プロバイダー](xref:security/data-protection/implementation/key-storage-providers)
        *   [保存時のキーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [キーの不変性と設定](xref:security/data-protection/implementation/key-immutability)
        *   [キー ストレージの形式](xref:security/data-protection/implementation/key-storage-format)
        *   [短期データ保護プロバイダー](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [互換性](xref:security/data-protection/compatibility/index)
        *   [ASP.NET での <machineKey> の置換](xref:security/data-protection/compatibility/replacing-machinekey)
*   [承認によって保護されたユーザー データでのアプリの作成](xref:security/authorization/secure-data)
*   [開発中のアプリ シークレットの安全な格納](xref:security/app-secrets)
*   [Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)
*   [SSL の適用](xref:security/enforcing-ssl)
*   [リクエスト フォージェリの対策](xref:security/anti-request-forgery)
*   [オープン リダイレクト攻撃の防止](xref:security/preventing-open-redirects)
*   [クロスサイト スクリプティングの防止](xref:security/cross-site-scripting)
*   [クロスオリジン要求 (CORS) の有効化](xref:security/cors)
*   [アプリ間での Cookie の共有](xref:security/cookie-sharing)
