---
title: ASP.NET Core モジュール構成の参照
author: guardrex
description: ASP.NET Core アプリケーションをホストするための ASP.NET Core モジュールを構成する方法を説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core モジュール構成の参照

によって[Luke Latham](https://github.com/guardrex)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、および[Sourabh Shirhatti](https://twitter.com/sshirhatti)

このドキュメントでは、ASP.NET Core アプリケーションをホストするための ASP.NET Core モジュールを構成する方法について説明します。 ASP.NET Core モジュールとインストール手順の概要については、次を参照してください。、 [ASP.NET コア モジュールの概要](xref:fundamentals/servers/aspnet-core-module)です。

## <a name="configuration-with-webconfig"></a>Web.config の構成

ASP.NET Core モジュールの構成には、`aspNetCore`のセクションで、`system.webServer`ノードに、サイトの*web.config*ファイル。

次*web.config*ファイルは公開され、[フレームワークに依存する展開](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)サイト要求を処理する ASP.NET Core モジュールを構成および。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

次*web.config*は公開され、[自己完結型の配置](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

アプリを展開するときに[Azure App Service](https://azure.microsoft.com/services/app-service/)、`stdoutLogFile`にパスが設定されている`\\?\%home%\LogFiles\stdout`です。 パスに標準出力ログの保存、 *LogFiles*サービスによって自動的に、場所となるフォルダーを作成します。

参照してください[サブ アプリケーション構成](xref:host-and-deploy/iis/index#sub-application-configuration)の構成に関連する重要なメモを*web.config*サブ アプリ内のファイルです。

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore 要素の属性

::: moniker range="<= aspnetcore-2.0"
| 属性 | 説明 | 既定値 |
| --------- | ----------- | :-----: |
| `arguments` | <p>省略可能な文字列属性。</p><p>指定された実行可能ファイルへの引数**processPath**です。</p>| |
| `disableStartUpErrorPage` | true または false。</p><p>True の場合、 **502.5 - プロセス エラー**ページは表示されず、502 ステータス コード ページで構成されている、 *web.config*が優先されます。</p> | `false` |
| `forwardWindowsAuthToken` | true または false。</p><p>True の場合、トークンが 1 回の要求ヘッダー ' MS ASPNETCORE WINAUTHTOKEN' として ASPNETCORE_PORT % でリッスンしている子プロセスに転送されます。 このトークン要求ごとに CloseHandle を呼び出すには、そのプロセスの役割です。</p> | `true` |
| `processPath` | <p>必須の文字列属性です。</p><p>HTTP 要求をリッスンしているプロセスを起動する実行可能ファイルへのパス。 相対パスがサポートされます。 パスが始まる場合`.`パスはサイトのルートの相対パスであると見なされます。</p> | |
| `rapidFailsPerMinute` | <p>省略可能な整数属性です。</p><p>プロセスが指定された回数を指定**processPath**は 1 分あたりのクラッシュを許可します。 この制限を超えた場合、モジュールは、1 分間の残りのプロセスの起動を停止します。</p> | `10` |
| `requestTimeout` | <p>省略可能な timespan 属性です。</p><p>%Aspnetcore_port でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</p><p>ASP.NET Core 2.0 またはそれ以前は、リリースに付属していた ASP.NET コア モジュールのバージョンでは、`requestTimeout`必要がありますで指定する整数の分だけ、それ以外の場合、既定値は 2 分です。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>省略可能な整数属性です。</p><p>モジュールが、実行可能ファイルを正常にシャット ダウンを待機する秒単位で期間ときに、 *app_offline.htm*ファイルが検出されました。</p> | `10` |
| `startupTimeLimit` | <p>省略可能な整数属性です。</p><p>モジュールがポートでリッスンしているプロセスを開始する実行可能ファイルを待機する秒単位で期間です。 この制限時間を超えた場合、モジュールには、プロセスが強制終了します。 モジュールが、新しい要求を受信し、アプリの起動に失敗しない限り、後続の受信要求の処理を再開を試行し続け、プロセスを再起動しようとしています**rapidFailsPerMinute** 、最後の回数。分をロールバックしています。</p> | `120` |
| `stdoutLogEnabled` | <p>省略可能な Boolean 属性です。</p><p>True の場合、 **stdout**と**stderr**で指定されたプロセスの**processPath**で指定されたファイルにリダイレクトされます**stdoutLogFile**です。</p> | `false` |
| `stdoutLogFile` | <p>省略可能な文字列属性。</p><p>対象の相対パスまたは絶対ファイル パスを指定**stdout**と**stderr**で指定されたプロセスから**processPath**ログに記録されます。 相対パスは、サイトのルートです。 以降で任意のパス`.`は、サイトと相対的なルートとその他のすべてのパスが絶対パスとして扱われます。 モジュール、ログ ファイルを作成するためにパスで提供されるすべてのフォルダーが存在する必要があります。 区切り記号のアンダー スコア、タイムスタンプ、プロセス ID、およびファイル拡張子を使用して (*.log*)、最後のセグメントに追加されます、 **stdoutLogFile**パス。 場合`.\logs\stdout`が指定される値として例を標準出力ログとして保存*stdout_20180205194132_1934.log*で、*ログ*フォルダー 19時 41分: 32 1934年のプロセス id で 2/5/2018 に保存される場合。</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| 属性 | 説明 | 既定値 |
| --------- | ----------- | :-----: |
| `arguments` | <p>省略可能な文字列属性。</p><p>指定された実行可能ファイルへの引数**processPath**です。</p>| |
| `disableStartUpErrorPage` | true または false。</p><p>True の場合、 **502.5 - プロセス エラー**ページは表示されず、502 ステータス コード ページで構成されている、 *web.config*が優先されます。</p> | `false` |
| `forwardWindowsAuthToken` | true または false。</p><p>True の場合、トークンが 1 回の要求ヘッダー ' MS ASPNETCORE WINAUTHTOKEN' として ASPNETCORE_PORT % でリッスンしている子プロセスに転送されます。 このトークン要求ごとに CloseHandle を呼び出すには、そのプロセスの役割です。</p> | `true` |
| `processPath` | <p>必須の文字列属性です。</p><p>HTTP 要求をリッスンしているプロセスを起動する実行可能ファイルへのパス。 相対パスがサポートされます。 パスが始まる場合`.`パスはサイトのルートの相対パスであると見なされます。</p> | |
| `rapidFailsPerMinute` | <p>省略可能な整数属性です。</p><p>プロセスが指定された回数を指定**processPath**は 1 分あたりのクラッシュを許可します。 この制限を超えた場合、モジュールは、1 分間の残りのプロセスの起動を停止します。</p> | `10` |
| `requestTimeout` | <p>省略可能な timespan 属性です。</p><p>%Aspnetcore_port でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</p><p>ASP.NET Core 2.1 以降のリリースに付属していた ASP.NET コア モジュールのバージョンでは、`requestTimeout`は時間、分、および秒単位で指定します。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>省略可能な整数属性です。</p><p>モジュールが、実行可能ファイルを正常にシャット ダウンを待機する秒単位で期間ときに、 *app_offline.htm*ファイルが検出されました。</p> | `10` |
| `startupTimeLimit` | <p>省略可能な整数属性です。</p><p>モジュールがポートでリッスンしているプロセスを開始する実行可能ファイルを待機する秒単位で期間です。 この制限時間を超えた場合、モジュールには、プロセスが強制終了します。 モジュールが、新しい要求を受信し、アプリの起動に失敗しない限り、後続の受信要求の処理を再開を試行し続け、プロセスを再起動しようとしています**rapidFailsPerMinute** 、最後の回数。分をロールバックしています。</p> | `120` |
| `stdoutLogEnabled` | <p>省略可能な Boolean 属性です。</p><p>True の場合、 **stdout**と**stderr**で指定されたプロセスの**processPath**で指定されたファイルにリダイレクトされます**stdoutLogFile**です。</p> | `false` |
| `stdoutLogFile` | <p>省略可能な文字列属性。</p><p>対象の相対パスまたは絶対ファイル パスを指定**stdout**と**stderr**で指定されたプロセスから**processPath**ログに記録されます。 相対パスは、サイトのルートです。 以降で任意のパス`.`は、サイトと相対的なルートとその他のすべてのパスが絶対パスとして扱われます。 モジュール、ログ ファイルを作成するためにパスで提供されるすべてのフォルダーが存在する必要があります。 区切り記号のアンダー スコア、タイムスタンプ、プロセス ID、およびファイル拡張子を使用して (*.log*)、最後のセグメントに追加されます、 **stdoutLogFile**パス。 場合`.\logs\stdout`が指定される値として例を標準出力ログとして保存*stdout_20180205194132_1934.log*で、*ログ*フォルダー 19時 41分: 32 1934年のプロセス id で 2/5/2018 に保存される場合。</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>環境変数の設定

プロセスの環境変数を指定することができます、`processPath`属性。 指定された環境変数を`environmentVariable`の子要素、`environmentVariables`コレクション要素。 このセクションで設定されている環境変数よりも優先システム環境変数。

次の例では、2 つの環境変数を設定します。 `ASPNETCORE_ENVIRONMENT` アプリの環境を構成`Development`です。 開発者は、この値にへの設定は一時的に可能性があります、 *web.config*を強制するためにファイル、[開発者例外ページ](xref:fundamentals/error-handling)アプリ例外をデバッグするときに読み込みます。 `CONFIG_DIR` スタートアップ時にアプリの構成ファイルを読み込むためのパスを形成する値を読み取るためのコードを記述は、ユーザー定義の環境変数の例を示します。

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> のみを設定、 `ASPNETCORE_ENVIRONMENT` envirnonment 変数を`Development`にステージングと、インターネットなどの信頼されていないネットワークにアクセスできないサーバーをテストします。

## <a name="appofflinehtm"></a>app_offline.htm

名前のファイルが場合、 *app_offline.htm*が検出された、アプリのルート ディレクトリで ASP.NET Core モジュールしようとすると正常にシャット ダウン、アプリとの受信要求の処理を停止します。 定義された秒数の後に、アプリが実行中の場合`shutdownTimeLimit`、ASP.NET Core モジュール実行中のプロセスを強制終了します。

中に、 *app_offline.htm*ファイルが存在 ASP.NET Core モジュールが要求に応答の内容を送信することによって、 *app_offline.htm*ファイル。 ときに、 *app_offline.htm*ファイルが削除され、次の要求は、アプリを起動します。

## <a name="start-up-error-page"></a>スタートアップ エラー ページ

バックエンド プロセスまたはバックエンドの処理が開始されるが、構成されたポートでリッスンに失敗するを起動する ASP.NET Core モジュールが失敗した場合、 *502.5 プロセス エラー*ステータス コード ページが表示されます。 このページを抑制して、既定の IIS 502 ステータス コード ページを元に戻すを使用して、`disableStartUpErrorPage`属性。 カスタム エラー メッセージを構成する方法については、次を参照してください。 [HTTP エラー `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/)です。

![502.5 プロセス エラー ステータス コード ページ](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>ログの作成とリダイレクト

ASP.NET Core モジュール リダイレクトの場合はディスクへの stdout と stderr のログ、`stdoutLogEnabled`と`stdoutLogFile`の属性、`aspNetCore`要素を設定します。 すべてのフォルダ、`stdoutLogFile`パスは、モジュール、ログ ファイルを作成するために存在する必要があります。 アプリケーション プールでは、ログが書き込まれる場所に書き込みアクセス権が必要 (を使用して`IIS AppPool\<app_pool_name>`書き込みアクセス許可を提供する)。

プロセスのリサイクル/再起動が発生しない限り、ログは循環されません。 ホストは、ログの使用ディスク領域を制限するの役割です。

標準出力ログを使用するは、アプリの起動に関する問題のトラブルシューティングにのみ推奨されます。 一般的なアプリのログ記録の目的で、標準出力ログを使用しないでください。 ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。 詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。

タイムスタンプとファイルの拡張機能は、ログ ファイルの作成時に自動的に追加されます。 ログ ファイルの名前は、タイムスタンプ、プロセス ID、およびファイル拡張子を追加することによって作成されて (*.log*) の最後のセグメントを`stdoutLogFile`パス (通常*stdout*) アンダー スコアで区切られます。 場合、`stdoutLogFile`パスの終わりに*stdout*、1934 2/5/2018 の 19時 42分: 32 時に作成の PID を使用してアプリのログ ファイル名である*stdout_20180205194132_1934.log*です。

次のサンプル`aspNetCore`要素は Azure App Service でホストされているアプリの標準出力ログを構成します。 ローカル パスまたはネットワーク共有のパスがローカル ログもかまわないです。 AppPool ユーザー id が指定されたパスに書き込む権限を持っていることを確認します。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

参照してください[構成 web.config で](#configuration-with-webconfig)の例については、`aspNetCore`内の要素、 *web.config*ファイル。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>プロキシの構成で HTTP プロトコルとペアリング トークンを使用する

ASP.NET Core モジュールと Kestrel の間に作成されたプロキシは、HTTP プロトコルを使用します。 HTTP を使用するは、パフォーマンスの最適化、モジュールと Kestrel 間のトラフィックが行われる、ループバック アドレスでネットワーク インターフェイスからです。 モジュールと、サーバーからの場所から Kestrel 間のトラフィックを傍受のリスクはありません。

ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。 ペアリング トークンが作成され、環境変数に設定 (`ASPNETCORE_TOKEN`) モジュールによってです。 ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MSAspNetCoreToken`) にも設定されます。 IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。 トークンの値が一致しない場合、要求はログに記録され、拒否されます。 ペアリングのトークンの環境変数と、モジュールと Kestrel 間のトラフィックは、サーバーからの場所からアクセスできません。 ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>IIS と ASP.NET Core モジュール構成を共有します。

ASP.NET Core モジュール インストーラーの実行の権限を持つ、**システム**アカウント。 インストーラーが、アクセス拒否エラーのモジュールの設定を構成しようとするときにヒットため、ローカル システム アカウントには、IIS 共有構成で使用される共有のパスに対するアクセス許可を変更しませんが、 *applicationHost.config*共有します。 IIS 共有構成を使用する場合は、次の手順に従います。

1. IIS 共有構成を無効にします。
1. インストーラーを実行します。
1. 更新されたエクスポート*applicationHost.config*共有するファイル。
1. IIS 共有構成を再度有効にします。

## <a name="module-version-and-hosting-bundle-installer-logs"></a>モジュールのバージョンとバンドルをホストしているインストーラーのログ

インストールされている ASP.NET のコア モジュールのバージョンを確認します。

1. ホストのシステム上に移動 *%windir%\System32\inetsrv*です。
1. 検索、 *aspnetcore.dll*ファイル。
1. ファイルを右クリックし **プロパティ**コンテキスト メニューからです。
1. 選択、**詳細**タブです。**ファイル バージョン**と**製品バージョン**モジュールのインストールされているバージョンを表します。

モジュールのバンドルをホストしているインストーラーのログがある*c:\\ユーザー\\%username%\\AppData\\ローカル\\Temp*です。ファイルの名前は*dd_DotNetCoreWinSvrHosting__\<タイムスタンプ > _000_AspNetCoreModule_x64.log*です。

## <a name="module-schema-and-configuration-file-locations"></a>モジュール、スキーマ、および構成ファイルの場所

### <a name="module"></a>Module

**IIS (x86 または amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86 または amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>構成

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

検索して、ファイルが見つかりません*aspnetcore.dll*で、 *applicationHost.config*ファイル。 IIS Express を*applicationHost.config*ファイルは既定では存在しません。 ファイルが作成された *\<application_root >\\.vs\\config* Visual Studio ソリューションで任意の web アプリ プロジェクトを開始するとき。
