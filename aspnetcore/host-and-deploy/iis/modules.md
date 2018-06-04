---
title: ASP.NET Core での IIS モジュール
author: guardrex
description: ASP.NET Core アプリに対してアクティブおよび非アクティブな IIS モジュールについて、さらに IIS モジュールの管理方法についてを把握します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 4a60b6c9bab77e8095cb9f19e615219817702b32
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566647"
---
# <a name="iis-modules-with-aspnet-core"></a>ASP.NET Core での IIS モジュール

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリは、リバース プロキシ構成を使って IIS によってホストされます。 ASP.NET Core アプリに対する要求の処理には、一部のネイティブ IIS モジュールとすべての IIS マネージド モジュールは使用できません。 多くの場合、ASP.NET Core では、IIS のネイティブ モジュールおよびマネージド モジュールの代替機能が提供されています。

## <a name="native-modules"></a>ネイティブ モジュール

次の表では、ASP.NET Core アプリへのリバース プロキシ要求に対して機能するネイティブ IIS モジュールを示します。

| Module | ASP.NET Core アプリで機能するか | ASP.NET Core のオプション |
| ------ | :-------------------------------: | ------------------- |
| **匿名認証**<br>`AnonymousAuthenticationModule` | [はい] | |
| **基本認証**<br>`BasicAuthenticationModule` | [はい] | |
| **クライアント証明書マッピング認証**<br>`CertificateMappingAuthenticationModule` | [はい] | |
| **CGI**<br>`CgiModule` | × | |
| **構成検証**<br>`ConfigurationValidationModule` | [はい] | |
| **HTTP エラー**<br>`CustomErrorModule` | × | [状態コード ページ ミドルウェア](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **カスタム ログ**<br>`CustomLoggingModule` | [はい] | |
| **既定のドキュメント**<br>`DefaultDocumentModule` | × | [既定のファイル ミドルウェア](xref:fundamentals/static-files#serve-a-default-document) |
| **ダイジェスト認証**<br>`DigestAuthenticationModule` | [はい] | |
| **ディレクトリの参照**<br>`DirectoryListingModule` | × | [ディレクトリ参照ミドルウェア](xref:fundamentals/static-files#enable-directory-browsing) |
| **動的圧縮**<br>`DynamicCompressionModule` | [はい] | [応答圧縮ミドルウェア](xref:performance/response-compression) |
| **トレース**<br>`FailedRequestsTracingModule` | [はい] | [ASP.NET Core のログ](xref:fundamentals/logging/index#tracesource-provider) |
| **ファイル キャッシュ**<br>`FileCacheModule` | × | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware) |
| **HTTP キャッシュ**<br>`HttpCacheModule` | × | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware) |
| **HTTP ログ**<br>`HttpLoggingModule` | [はい] | [ASP.NET Core のログ](xref:fundamentals/logging/index)<br>実装: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)、[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)、[NLog](https://github.com/NLog/NLog.Extensions.Logging)、[Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP リダイレクト**<br>`HttpRedirectionModule` | [はい] | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) |
| **IIS クライアント証明書マッピング認証**<br>`IISCertificateMappingAuthenticationModule` | [はい] | |
| **IP およびドメインの制限**<br>`IpRestrictionModule` | [はい] | |
| **ISAPI フィルター**<br>`IsapiFilterModule` | [はい] | [ミドルウェア](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | [はい] | [ミドルウェア](xref:fundamentals/middleware/index) |
| **プロトコル サポート**<br>`ProtocolSupportModule` | [はい] | |
| **要求のフィルタリング**<br>`RequestFilteringModule` | [はい] | [URL リライト ミドルウェア`IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **要求監視**<br>`RequestMonitorModule` | [はい] | |
| **URL リライト**<br>`RewriteModule` | はい&#8224; | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) |
| **サーバー側インクルード**<br>`ServerSideIncludeModule` | × | |
| **静的圧縮**<br>`StaticCompressionModule` | × | [応答圧縮ミドルウェア](xref:performance/response-compression) |
| **静的コンテンツ**<br>`StaticFileModule` | × | [静的ファイル ミドルウェア](xref:fundamentals/static-files) |
| **トークン キャッシュ**<br>`TokenCacheModule` | [はい] | |
| **URI キャッシュ**<br>`UriCacheModule` | [はい] | |
| **URL 認証**<br>`UrlAuthorizationModule` | [はい] | [ASP.NET Core ID](xref:security/authentication/identity) |
| **Windows 認証**<br>`WindowsAuthenticationModule` | [はい] | |

&#8224; URL リライト モジュールの `isFile` および `isDirectory` 一致タイプは、[ディレクトリ構造](xref:host-and-deploy/directory-structure)の変更のため、ASP.NET Core アプリでは動作しません。

## <a name="managed-modules"></a>マネージド モジュール

アプリ プールの .NET CLR バージョンが **[マネージ コードなし]** に設定されている場合、マネージド モジュールはホストされた ASP.NET Core アプリでは機能 "*しません*"。 ASP.NET Core では、いくつかのケースにおいてミドルウェアの代替機能が提供されています。

| Module                  | ASP.NET Core のオプション |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Cookie 認証ミドルウェア](xref:security/authentication/cookie) |
| OutputCache             | [応答キャッシュ ミドルウェア](xref:performance/caching/middleware) |
| Profile                 | |
| RoleManager             | |
| ScriptModule-4.0        | |
| セッション                 | [セッション ミドルウェア](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [ASP.NET Core ID](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS マネージャー アプリケーションの変更

IIS マネージャーを使って設定を構成すると、アプリの *web.config* ファイルが変更されます。 アプリを展開し、*web.config* を含めた場合、IIS マネージャーを使って行われた変更は、展開される *web.config* ファイルによって上書きされます。 サーバーの *web.config* ファイルを変更する場合は、サーバー上の更新された *web.config* ファイルをローカル プロジェクトにすぐにコピーしてください。

## <a name="disabling-iis-modules"></a>IIS モジュールの無効化

アプリに対して無効にする必要がある IIS モジュールがサーバー レベルで構成されている場合、アプリの *web.config* ファイルへの追加によってモジュールを無効にすることができます。 モジュールをそのまま残し、構成の設定を使って非アクティブにするか (使用可能な場合)、アプリからモジュールを削除します。

### <a name="module-deactivation"></a>モジュールの非アクティブ化

多くのモジュールには、アプリからモジュールを削除せずに無効にすることができる構成設定が用意されています。 これは、モジュールを非アクティブ化する最も簡単ですばやい方法です。 たとえば、HTTP リダイレクト モジュールは、*web.config* の **\<httpRedirect >** 要素を使って無効にできます。

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

構成設定を使ってモジュールを無効にする方法について詳しくは、[IIS \<system.webServer >](/iis/configuration/system.webServer/) の "*子要素*" に関するセクションのリンクに従ってください。

### <a name="module-removal"></a>モジュールの削除

*web.config* の設定を使ってモジュールを削除する場合は、最初に、モジュールのロックを解除し、*web.config* の **\<modules>** セクションのロックを解除します。

1. サーバー レベルでモジュールのロックを解除します。 IIS マネージャーの **[接続]** サイド バーで IIS サーバーを選びます。 **[IIS]** 領域で **[モジュール]** を開きます。 一覧でモジュールを選びます。 右側の **[アクション]** サイド バーで、**[ロック解除]** を選びます。 後で *web.config* から削除する予定のモジュールをできるだけ多くロック解除します。

2. *web.config* の **\<modules>** セクションを指定しないでアプリを展開します。最初に IIS マネージャーでセクションをロック解除せずに、**\<modules>** セクションを含む *web.config* を使ってアプリを展開した場合、セクションのロックを解除しようとすると Configuration Manager で例外がスローされます。 したがって、**\<modules>** セクションを指定しないでアプリを展開します。

3. *web.config* の **\<modules>** セクションのロックを解除します。**[接続]** サイド バーの **[サイト]** で Web サイトを選びます。 **[管理]** 領域で**構成エディター**を開きます。 ナビゲーション コントロールを使って、`system.webServer/modules` セクションを選びます。 右側の **[アクション]** サイド バーで、セクションの **[ロック解除]** を選びます。

4. このようにすると、**\<modules>** セクションを *web.config* ファイルに追加し、**\<remove>** 要素を使ってアプリからモジュールを削除できます。 複数の **\<remove>** 要素を追加して、複数のモジュールを削除することができます。 サーバー上で *web.config* を変更した場合は、すぐにプロジェクトのローカルな *web.config* ファイルに対して同じ変更を行ってください。 このようにしてモジュールを削除すると、サーバー上の他のアプリでのモジュールの使用に影響がありません。

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

IIS モジュールは、*Appcmd.exe* を使って削除することもできます。 コマンドで `MODULE_NAME` と `APPLICATION_NAME` を指定します。

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

たとえば、Default Web Site から `DynamicCompressionModule` を削除するには、次のようにします。

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>最小限のモジュール構成

ASP.NET Core アプリを実行するために必要なモジュールは、匿名認証モジュールと ASP.NET Core モジュールだけです。

![最小限のモジュール構成で [モジュール] が開かれている IIS マネージャー](modules/_static/modules.png)

URI キャッシュ モジュール (`UriCacheModule`) により、IIS は URL レベルで Web サイトの構成をキャッシュできます。 このモジュールがない場合、同じ URL が繰り返し要求された場合でも、IIS はすべての要求で構成を読み取って解析する必要があります。 すべての要求で構成を解析すると、パフォーマンスが大幅に低下します。 *URI キャッシュ モジュールはホストされた ASP.NET Core アプリを実行するために厳密には必要ありませんが、すべての ASP.NET Core の展開で URI キャッシュ モジュールを有効にすることをお勧めします。*

HTTP キャッシュ モジュール (`HttpCacheModule`) は、IIS 出力キャッシュと共に、HTTP.sys キャッシュ内のアイテムをキャッシュするためのロジックも実装します。 このモジュールがないと、コンテンツはカーネル モードでキャッシュされなくなり、キャッシュ プロファイルは無視されます。 HTTP キャッシュ モジュールを削除すると、通常、パフォーマンスとリソースの使用に悪影響があります。 *HTTP キャッシュ モジュールはホストされた ASP.NET Core アプリを実行するために厳密には必要ありませんが、すべての ASP.NET Core の展開で HTTP キャッシュ モジュールを有効にすることをお勧めします。*

## <a name="additional-resources"></a>その他の技術情報

* [IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)
* [IIS アーキテクチャの概要: IIS のモジュール](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS モジュールの概要](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [IIS 7.0 のロールとモジュールのカスタマイズ](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
