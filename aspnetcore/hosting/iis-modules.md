---
title: "ASP.NET Core で IIS のモジュールの使用"
author: guardrex
description: "ASP.NET Core アプリケーションのアクティブおよび非アクティブの IIS モジュールを記述する参照ドキュメント。"
keywords: "ASP.NET Core、iis、モジュール、リバース プロキシ"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: afad266874d3ac059d9f3a6d26a5330a0006320b
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>ASP.NET Core で IIS のモジュールの使用

によって[Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリケーションは、リバース プロキシの構成では IIS によってホストされます。 IIS のネイティブ モジュールの一部と IIS 管理モジュールのすべてでは、ASP.NET Core アプリケーションの要求の処理に使用できません。 多くの場合は、ASP.NET Core は、代わりに IIS のネイティブおよびマネージ モジュールの機能を提供します。

## <a name="native-modules"></a>ネイティブ モジュール
モジュール | .NET core アクティブ | ASP.NET Core オプション
--- | :---: | ---
**匿名認証**<br>`AnonymousAuthenticationModule` | はい | 
**基本認証**<br>`BasicAuthenticationModule` | はい | 
**クライアント証明書マッピング認証**<br>`CertificateMappingAuthenticationModule` | はい | 
**CGI**<br>`CgiModule` | いいえ | 
**構成の検証**<br>`ConfigurationValidationModule` | はい | 
**HTTP エラー**<br>`CustomErrorModule` | いいえ | [ステータス コード ページ ミドルウェア](xref:fundamentals/error-handling#configuring-status-code-pages)
**カスタム ログ**<br>`CustomLoggingModule` | はい | 
**既定のドキュメント**<br>`DefaultDocumentModule` | いいえ | [既定のファイルのミドルウェア](xref:fundamentals/static-files#serving-a-default-document)
**ダイジェスト認証**<br>`DigestAuthenticationModule` | はい | 
**ディレクトリの参照**<br>`DirectoryListingModule` | いいえ | [ディレクトリ参照ミドルウェア](xref:fundamentals/static-files#enabling-directory-browsing)
**動的な圧縮**<br>`DynamicCompressionModule` | はい | [応答圧縮ミドルウェア](xref:performance/response-compression)
**トレース**<br>`FailedRequestsTracingModule` | はい | [ASP.NET Core のログ記録](xref:fundamentals/logging#the-tracesource-provider)
**ファイルのキャッシュ**<br>`FileCacheModule` | いいえ | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
**HTTP キャッシュ**<br>`HttpCacheModule` | いいえ | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
**HTTP ログ**<br>`HttpLoggingModule` | はい | [ASP.NET Core のログ記録](xref:fundamentals/logging)<br>実装: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)、 [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)、 [NLog](https://github.com/NLog/NLog.Extensions.Logging)、 [Serilog](https://github.com/serilog/serilog-extensions-logging)
**HTTP リダイレクト**<br>`HttpRedirectionModule` | はい | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting)
**IIS クライアント証明書マッピング認証**<br>`IISCertificateMappingAuthenticationModule` | はい | 
**IP およびドメインの制限**<br>`IpRestrictionModule` | はい | 
**ISAPI フィルター**<br>`IsapiFilterModule` | はい | [ミドルウェア](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | はい | [ミドルウェア](xref:fundamentals/middleware)
**プロトコルのサポート**<br>`ProtocolSupportModule` | はい | 
**要求のフィルタリング**<br>`RequestFilteringModule` | はい | [URL 書き換えミドルウェア`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**要求監視**<br>`RequestMonitorModule` | はい | 
**URL 書き換え**<br>`RewriteModule` | Yes† | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting)
**サーバー側インクルード**<br>`ServerSideIncludeModule` | いいえ | 
**静的な圧縮**<br>`StaticCompressionModule` | いいえ | [応答圧縮ミドルウェア](xref:performance/response-compression)
**静的コンテンツ**<br>`StaticFileModule` | いいえ | [静的ファイル ミドルウェア](xref:fundamentals/static-files)
**トークンのキャッシュ**<br>`TokenCacheModule` | はい | 
**URI のキャッシュ**<br>`UriCacheModule` | はい | 
**URL 認証**<br>`UrlAuthorizationModule` | はい | [ASP.NET Core の Id](xref:security/authentication/identity)
**Windows 認証**<br>`WindowsAuthenticationModule` | はい | 

URL Rewrite Module を †The`isFile`と`isDirectory`の変更のための ASP.NET Core アプリケーションと連携していない[ディレクトリ構造](xref:hosting/directory-structure)です。

## <a name="managed-modules"></a>マネージ モジュール
モジュール | .NET core アクティブ | ASP.NET Core オプション
--- | :---: | ---
AnonymousIdentification | いいえ | 
DefaultAuthentication | いいえ | 
FileAuthorization | いいえ | 
FormsAuthentication | いいえ | [Cookie 認証ミドルウェア](xref:security/authentication/cookie)
OutputCache | いいえ | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
Profile | いいえ | 
RoleManager | いいえ | 
ScriptModule 4.0 | いいえ | 
セッション | いいえ | [セッションのミドルウェア](xref:fundamentals/app-state)
UrlAuthorization | いいえ | 
UrlMappingsModule | いいえ | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | いいえ | [ASP.NET Core の Id](xref:security/authentication/identity)
WindowsAuthentication | いいえ | 

## <a name="iis-manager-application-changes"></a>IIS マネージャー アプリケーションの変更
IIS マネージャーを使用して設定を構成すると、直接変更する、 *web.config*アプリのファイルです。 アプリを展開して含める*web.config*、IIS マネージャーで行った変更は上書きされます、デプロイされているによって*web.config ファイル*です。 そのために、サーバーの変更を加える場合*web.config*ファイル、コピー、更新された*web.config*ファイルをすぐにローカル プロジェクト。

## <a name="disabling-iis-modules"></a>IIS モジュールを無効にします。
アプリケーションを無効にするにはサーバー レベルで構成されている IIS モジュールがあれば、監視できるように追加されると、 *web.config*ファイル。 モジュールをそのまま残す (使用可能な場合)、構成の設定を使用して、非アクティブか、アプリからモジュールを削除します。

### <a name="module-deactivation"></a>モジュールの非アクティブ化
多くのモジュールには、削除せずに、アプリケーションからそれらを無効にできる構成設定が用意されています。 これは、モジュールを非アクティブ化する最も簡単なと最も簡単な方法です。 たとえば、IIS URL Rewrite Module を無効にするを使用する場合、`<httpRedirect>`次に示すようにします。 モジュールの構成設定を無効にする方法の詳細については、以下のリンク、*子要素*のセクション[IIS `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)です。

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>モジュールの取り外し
設定を削除してから、モジュールを選択したかどうかは*web.config*、モジュールのロックを解除して、ロック解除する必要があります、`<modules>`のセクション*web.config*最初。 手順を次に説明します。

1. サーバー レベルのモジュールのロックを解除します。 IIS サーバー、IIS マネージャーの **接続**サイドバーです。 開く、**モジュール**で、 **IIS**領域。 リスト内のモジュールをクリックします。 **アクション**、右側のサイド バーをクリックして**Unlock**です。 多くのモジュールを削除してから計画する際のロックを解除*web.config*以降。

2. せず、アプリケーションを配置、 `<modules>` 」の「 *web.config*です。使用したアプリを展開する場合、 *web.config*を含む、`<modules>`セクションがロックを解除セクション最初、IIS マネージャーで、Configuration Manager せずセクションのロック解除しようとしたときに例外がスローされます。 そのため、せず、アプリケーションを展開、`<modules>`セクションです。

3. ロックを解除、`<modules>`のセクション*web.config*です。**接続**をサイド バーに web サイトをクリックして**サイト**です。 **管理**領域で、開く、**構成エディター**です。 ナビゲーション コントロールを使用して、`system.webServer/modules`セクションです。 **アクション**をクリックして、右側のサイドバー **Unlock**セクションです。

4. この時点では、ことができますを追加する、`<modules>`セクション、 *web.config*ファイルと、`<remove>`アプリケーションからモジュールを削除する要素。 複数を追加する`<remove>`複数のモジュールを削除する要素。 忘れずを加えた場合*web.config*それらを直ちにでするプロジェクトをローカル サーバーに変更します。 こうすると、モジュールを削除すると、サーバー上の他のアプリを使用してモジュールの使用は影響しません。

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

IIS のインストールでインストールされている既定のモジュールには、次を使用できます`<module>`セクションを既定のモジュールを削除します。

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

IIS モジュールを削除することもできます。 *Appcmd.exe*です。 提供、`MODULE_NAME`と`APPLICATION_NAME`次に示すコマンドで。

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

ここでは削除する方法、`DynamicCompressionModule`既定の Web サイトから。

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>最小限のモジュールの構成
ASP.NET Core アプリケーションを実行するために必要なだけモジュールとは、匿名の認証モジュールおよび ASP.NET Core モジュールです。

![最小限のモジュールの構成とモジュールに IIS マネージャーを開く](iis-modules/_static/modules.png)

## <a name="resources"></a>リソース
* [IIS に発行します。](xref:publishing/iis)
* [IIS のモジュールの概要](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [IIS 7.0 ロール、およびモジュールをカスタマイズします。](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
