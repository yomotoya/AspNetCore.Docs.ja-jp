---
title: "ASP.NET Core で IIS のモジュールの使用"
author: guardrex
description: "ASP.NET Core アプリケーションおよび IIS モジュールを管理する方法のアクティブおよび非アクティブの IIS モジュールを検出します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: a6610e33abdc3eafb5908728b3299e95e6e7183f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>ASP.NET Core で IIS のモジュールの使用

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリケーションは、リバース プロキシの構成では IIS によってホストされます。 IIS のネイティブ モジュールの一部とすべての IIS 管理モジュールは、ASP.NET Core アプリケーションの要求の処理に使用できません。 多くの場合は、ASP.NET Core は、代わりに IIS のネイティブおよびマネージ モジュールの機能を提供します。

## <a name="native-modules"></a>ネイティブ モジュール

| Module | .NET Core Active | ASP.NET Core Option |
| ------ | :--------------: | ------------------- |
| **匿名認証**<br>`AnonymousAuthenticationModule` | [はい] | |
| **基本認証**<br>`BasicAuthenticationModule` | [はい] | |
| **クライアント証明書マッピング認証**<br>`CertificateMappingAuthenticationModule` | [はい] | |
| **CGI**<br>`CgiModule` | × | |
| **構成の検証**<br>`ConfigurationValidationModule` | [はい] | |
| **HTTP エラー**<br>`CustomErrorModule` | × | [ステータス コード ページ ミドルウェア](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **カスタム ログ**<br>`CustomLoggingModule` | [はい] | |
| **既定のドキュメント**<br>`DefaultDocumentModule` | × | [既定のファイルのミドルウェア](xref:fundamentals/static-files#serve-a-default-document) |
| **ダイジェスト認証**<br>`DigestAuthenticationModule` | [はい] | |
| **ディレクトリの参照**<br>`DirectoryListingModule` | × | [ディレクトリ参照ミドルウェア](xref:fundamentals/static-files#enable-directory-browsing) |
| **動的な圧縮**<br>`DynamicCompressionModule` | [はい] | [応答圧縮ミドルウェア](xref:performance/response-compression) |
| **トレース**<br>`FailedRequestsTracingModule` | [はい] | [ASP.NET Core のログ記録](xref:fundamentals/logging/index#the-tracesource-provider) |
| **ファイルのキャッシュ**<br>`FileCacheModule` | × | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware) |
| **HTTP キャッシュ**<br>`HttpCacheModule` | × | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware) |
| **HTTP ログ**<br>`HttpLoggingModule` | [はい] | [ASP.NET Core のログ記録](xref:fundamentals/logging/index)<br>実装: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)、 [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)、 [NLog](https://github.com/NLog/NLog.Extensions.Logging)、 [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP リダイレクト**<br>`HttpRedirectionModule` | [はい] | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) |
| **IIS クライアント証明書マッピング認証**<br>`IISCertificateMappingAuthenticationModule` | [はい] | |
| **IP およびドメインの制限**<br>`IpRestrictionModule` | [はい] | |
| **ISAPI フィルター**<br>`IsapiFilterModule` | [はい] | [ミドルウェア](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | [はい] | [ミドルウェア](xref:fundamentals/middleware/index) |
| **プロトコルのサポート**<br>`ProtocolSupportModule` | [はい] | |
| **要求のフィルタリング**<br>`RequestFilteringModule` | [はい] | [URL 書き換えミドルウェア `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **要求監視**<br>`RequestMonitorModule` | [はい] | |
| **URL 書き換え**<br>`RewriteModule` | [はい] &#8224;です。 | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) |
| **サーバー側インクルードします。**<br>`ServerSideIncludeModule` | × | |
| **静的な圧縮**<br>`StaticCompressionModule` | × | [応答圧縮ミドルウェア](xref:performance/response-compression) |
| **静的コンテンツ**<br>`StaticFileModule` | × | [静的ファイル ミドルウェア](xref:fundamentals/static-files) |
| **トークンのキャッシュ**<br>`TokenCacheModule` | [はい] | |
| **URI のキャッシュ**<br>`UriCacheModule` | [はい] | |
| **URL 認証**<br>`UrlAuthorizationModule` | [はい] | [ASP.NET Core Identity](xref:security/authentication/identity) |
| **Windows 認証**<br>`WindowsAuthenticationModule` | [はい] | |

&#8224;です。URL Rewrite モジュールの`isFile`と`isDirectory`一致の種類の変更のための ASP.NET Core アプリケーションとでは動作しない[ディレクトリ構造](xref:host-and-deploy/directory-structure)です。

## <a name="managed-modules"></a>マネージ モジュール

| Module                  | .NET Core Active | ASP.NET Core Option |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | ×               | |
| DefaultAuthentication   | ×               | |
| FileAuthorization       | ×               | |
| FormsAuthentication     | ×               | [Cookie 認証ミドルウェア](xref:security/authentication/cookie) |
| OutputCache             | ×               | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware) |
| Profile                 | ×               | |
| RoleManager             | ×               | |
| ScriptModule-4.0        | ×               | |
| セッション                 | ×               | [セッションのミドルウェア](xref:fundamentals/app-state) |
| UrlAuthorization        | ×               | |
| UrlMappingsModule       | ×               | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | ×               | [ASP.NET Core Identity](xref:security/authentication/identity) |
| WindowsAuthentication   | ×               | |

## <a name="iis-manager-application-changes"></a>IIS マネージャー アプリケーションの変更

IIS マネージャーを使用して、設定を構成する場合、 *web.config*アプリのファイルを変更します。 アプリを配置する場合と*web.config*、IIS マネージャーで加えられた変更は上書きによって展開された*web.config*ファイル。 サーバーの変更が加えられた場合*web.config*ファイル、コピー、更新された*web.config*ローカル プロジェクトをすぐに、サーバー上のファイルです。

## <a name="disabling-iis-modules"></a>IIS モジュールを無効にします。

IIS モジュールが、アプリでアプリへの追記を無効にする必要がありますサーバー レベルで構成されているかどうか*web.config*ファイルには、モジュールが無効にすることができます。 モジュールをそのまま残す (使用可能な場合)、構成の設定を使用して、非アクティブか、アプリからモジュールを削除します。

### <a name="module-deactivation"></a>モジュールの非アクティブ化

多くのモジュールには、アプリからモジュールを削除せずに無効にすることができる構成設定が用意されています。 これは、モジュールを非アクティブ化する最も簡単なと最も簡単な方法です。 たとえば、IIS URL Rewrite Module 無効にする、  **\<httpRedirect >**内の要素*web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

モジュールの構成設定を無効にする方法の詳細については、以下のリンク、*子要素*のセクション[IIS \<system.webServer >](/iis/configuration/system.webServer/)です。

### <a name="module-removal"></a>モジュールの取り外し

設定と、モジュールを削除しなかった場合は*web.config*モジュールのロックを解除、およびロック解除、 **\<モジュール >**のセクション*web.config*最初。

1. サーバー レベルのモジュールのロックを解除します。 IIS マネージャーで、IIS サーバーを選択**接続**サイドバーです。 開く、**モジュール**で、 **IIS**領域。 一覧で、モジュールを選択します。 **アクション**、右側のサイド バーを選択**Unlock**です。 多くのモジュールから削除することを計画する際のロックを解除*web.config*以降。

1. アプリを展開することがなく、 **\<モジュール >** 」の「 *web.config*です。アプリが展開された場合、 *web.config*を含む、 **\<モジュール >**がロックを解除セクション最初、IIS マネージャーで、Configuration Manager なしでは、例外をスローします。セクションのロック解除しようとしています。 そのため、アプリを展開することがなく、 **\<モジュール >**セクションです。

1. ロックを解除、 **\<モジュール >**のセクション*web.config*です。**接続**サイドバーで web サイトを選択**サイト**です。 **管理**領域で、開く、**構成エディター**です。 ナビゲーション コントロールを使用して、`system.webServer/modules`セクションです。 **アクション**、右側のサイド バーを選択する**Unlock**セクションです。

1. この時点で、 **\<モジュール >**セクションに追加できる、 *web.config*ファイルと、 **\<削除 >**からモジュールを削除する要素アプリ。 複数**\<削除 >**を複数のモジュールを削除する要素を追加することができます。 場合*web.config*に対する変更は、サーバーで、すぐにプロジェクトの同じ変更を行う*web.config*ファイルをローカルにします。 こうすると、モジュールを削除すると、サーバー上の他のアプリを使用してモジュールの使用は影響しません。

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

IIS のインストールでインストールされている既定のモジュールには、次を使用して**\<モジュール >**を既定のモジュールを削除する」セクション。

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

IIS モジュールを削除することができますも*Appcmd.exe*です。 提供、`MODULE_NAME`と`APPLICATION_NAME`コマンド。

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

たとえば、削除、`DynamicCompressionModule`既定の Web サイトから。

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>最小限のモジュールの構成

ASP.NET Core アプリケーションを実行するために必要なだけモジュールとは、匿名の認証モジュールおよび ASP.NET Core モジュールです。

![最小限のモジュールの構成とモジュールに IIS マネージャーを開く](modules/_static/modules.png)

## <a name="additional-resources"></a>その他の技術情報

* [IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)
* [IIS のアーキテクチャの概要: IIS のモジュール](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS のモジュールの概要](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [IIS 7.0 ロール、およびモジュールをカスタマイズします。](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
