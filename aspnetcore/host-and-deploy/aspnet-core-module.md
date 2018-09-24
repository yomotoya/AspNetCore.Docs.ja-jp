---
title: ASP.NET Core モジュール構成リファレンス
author: guardrex
description: ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: bf7a60b67b1ea78bb346e6dd5eeef38b54bfdbe4
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010950"
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core モジュール構成リファレンス

著者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)

このドキュメントでは、ASP.NET Core アプリをホストするための ASP.NET Core モジュールの構成方法について説明します。 ASP.NET Core モジュールの概要とインストールの説明については、「[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。

## <a name="configuration-with-webconfig"></a>web.config での構成

ASP.NET Core モジュールは、サイトの *web.config* ファイルの `system.webServer` ノードの `aspNetCore` セクションを使って構成します。

次に示す *web.config* ファイルは、[フレームワークに依存する展開](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)用に発行されたもので、サイトの要求を処理するように ASP.NET Core モジュールを構成します。

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

次の *web.config* は、[自己完結型の展開](/dotnet/articles/core/deploying/#self-contained-deployments-scd)用に発行されたものです。

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

アプリが [Azure App Service](https://azure.microsoft.com/services/app-service/) に対して展開されると、`stdoutLogFile` パスは `\\?\%home%\LogFiles\stdout` に設定されます。 パスは stdout ログを *LogFiles* フォルダーに保存します。これは、サービスによって自動的に作成される場所です。

サブアプリでの *web.config* ファイルの構成に関する重要な注意事項については、「[サブアプリケーション構成](xref:host-and-deploy/iis/index#sub-application-configuration)」をご覧ください。

### <a name="attributes-of-the-aspnetcore-element"></a>aspNetCore 要素の属性

::: moniker range="<= aspnetcore-2.0"

| 属性 | 説明 | 既定値 |
| --------- | ----------- | :-----: |
| `arguments` | <p>省略可能な文字列属性。</p><p>**processPath** において指定されている実行可能ファイルへの引数です。</p>| |
| `disableStartUpErrorPage` | true または false。</p><p>true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</p> | `false` |
| `forwardWindowsAuthToken` | true または false。</p><p>true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。 要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</p> | `true` |
| `processPath` | <p>必須の文字列属性です。</p><p>HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。 相対パスがサポートされています。 パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</p> | |
| `rapidFailsPerMinute` | <p>省略可能な整数属性</p><p>**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。 この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</p> | `10` |
| `requestTimeout` | <p>省略可能な期間属性。</p><p>%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</p><p>ASP.NET Core 2.0 以前のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は整数でのみ指定する必要があります。そうしないと、既定値の 2 分に設定されます。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>省略可能な整数属性</p><p>*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</p> | `10` |
| `startupTimeLimit` | <p>省略可能な整数属性</p><p>ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。 この制限時間を超えた場合、モジュールはプロセスを強制終了します。 最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</p> | `120` |
| `stdoutLogEnabled` | <p>省略可能な Boolean 属性です。</p><p>true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</p> | `false` |
| `stdoutLogFile` | <p>省略可能な文字列属性。</p><p>**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。 相対パスの基準はサイトのルートです。 `.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。 モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。 アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。 たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| 属性 | 説明 | 既定値 |
| --------- | ----------- | :-----: |
| `arguments` | <p>省略可能な文字列属性。</p><p>**processPath** において指定されている実行可能ファイルへの引数です。</p>| |
| `disableStartUpErrorPage` | true または false。</p><p>true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</p> | `false` |
| `forwardWindowsAuthToken` | true または false。</p><p>true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。 要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</p> | `true` |
| `processPath` | <p>必須の文字列属性です。</p><p>HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。 相対パスがサポートされています。 パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</p> | |
| `rapidFailsPerMinute` | <p>省略可能な整数属性</p><p>**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。 この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</p> | `10` |
| `requestTimeout` | <p>省略可能な期間属性。</p><p>%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</p><p>ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>省略可能な整数属性</p><p>*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</p> | `10` |
| `startupTimeLimit` | <p>省略可能な整数属性</p><p>ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。 この制限時間を超えた場合、モジュールはプロセスを強制終了します。 最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</p> | `120` |
| `stdoutLogEnabled` | <p>省略可能な Boolean 属性です。</p><p>true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</p> | `false` |
| `stdoutLogFile` | <p>省略可能な文字列属性。</p><p>**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。 相対パスの基準はサイトのルートです。 `.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。 モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。 アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。 たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>環境変数の設定

`processPath` 属性で、プロセスに対する環境変数を指定できます。 `environmentVariables` コレクション要素の `environmentVariable` 子要素で、環境変数を指定します。 このセクションで設定された環境変数は、システム環境変数より優先されます。

次の例では、2 つの環境変数を設定しています。 `ASPNETCORE_ENVIRONMENT` は、`Development` に対するアプリの環境を構成します。 開発者は、アプリの例外をデバッグするときに[開発者例外ページ](xref:fundamentals/error-handling)を強制的に読み込むため、*web.config* ファイルでこの値を一時的に設定できます。 `CONFIG_DIR` はユーザー定義の環境変数の例です。開発者はここに、アプリの構成ファイルを読み込むためのパスを形成するために起動時に値を読み取るコードを記述してあります。

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
> インターネットなどの信頼されていないネットワークにアクセスできないステージング サーバーやテスト サーバーでは、`ASPNETCORE_ENVIRONMENT` 環境変数を `Development` に設定するだけです。

## <a name="appofflinehtm"></a>app_offline.htm

*app_offline.htm* という名前のファイルがアプリのルート ディレクトリで検出された場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、受信要求の処理を停止することを試みます。 `shutdownTimeLimit` で定義されている秒数が経過してもまだアプリが実行している場合、ASP.NET Core モジュールは実行中のプロセスを強制終了します。

*app_offline.htm* ファイルが存在している間、ASP.NET Core モジュールは、*app_offline.htm* ファイルの内容を返送することで、要求に応答します。 *app_offline.htm* ファイルが削除されると、次の要求によってアプリが起動されます。

## <a name="start-up-error-page"></a>起動エラー ページ

ASP.NET Core モジュールが、バックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。 このページを抑制して、既定の IIS 502 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。 カスタム エラー メッセージの構成方法について詳しくは、[HTTP エラー `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/) に関するページをご覧ください。

![502.5 処理エラーの状態コード ページ](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>ログの作成とリダイレクト

`aspNetCore` 要素の `stdoutLogEnabled` 属性および `stdoutLogFile` 属性が設定されている場合は、stdout および stderr コンソール出力が ASP.NET Core モジュールによってディスクにリダイレクトされます。 モジュールがログ ファイルを作成するためには、`stdoutLogFile` パスのすべてのフォルダーが存在している必要があります。 アプリ プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります (書き込みアクセス許可を提供するには、`IIS AppPool\<app_pool_name>` を使います)。

プロセスのリサイクル/再起動が発生しない場合、ログは循環されません。 ログが使用するディスク領域を制限するのは、ホストの役割です。

stdout ログの使用は、アプリ起動時の問題をトラブルシューティングする場合にのみ推奨されます。 一般的なアプリ ログの目的には、stdout ログを使わないでください。 ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。 詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。

ログ ファイルの作成時には、タイムスタンプとファイルの拡張子が自動的に追加されます。 ログ ファイル名は、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) を `stdoutLogFile` パスの最後のセグメント (通常は *stdout*) にアンダースコアで区切って追加することで構成されます。 `stdoutLogFile` パスが *stdout* で終わっている場合、PID が 1934 で 2018 年 2 月 5 日の 19:42:32 に作成されたアプリのログのファイル名は、*stdout_20180205194132_1934.log* になります。

次の `aspNetCore` 要素の例では、Azure App Service でホストされているアプリの stdout ログを構成しています。 ローカル ログには、ローカル パスまたはネットワーク共有パスを使用できます。 AppPool のユーザー ID に、指定されたパスへの書き込みアクセス許可があることを確認してください。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

*web.config* ファイルでの `aspNetCore` 要素の例については、「[web.config での構成](#configuration-with-webconfig)」をご覧ください。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>プロキシの構成で HTTP プロトコルとペアリング トークンを使用する

ASP.NET Core モジュールと Kestrel の間に作成されるプロキシは、HTTP プロトコルを使います。 HTTP を使うと、パフォーマンスが最適化されます。この場合、モジュールと Kestrel の間のトラフィックは、ネットワーク インターフェイスから切り離されたループバック アドレスで発生します。 モジュールと Kestrel の間のトラフィックが、サーバーから離れた場所で傍受される危険はありません。

ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。 ペアリング トークンは、モジュールによって作成されて、環境変数 (`ASPNETCORE_TOKEN`) に設定されます。 ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MSAspNetCoreToken`) にも設定されます。 IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。 トークンの値が一致しない場合、要求はログに記録され、拒否されます。 ペアリング トークン環境変数およびモジュールと Kestrel の間のトラフィックには、サーバーから離れた場所からアクセスすることはできません。 ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>IIS 共有構成での ASP.NET Core モジュール

ASP.NET Core モジュールのインストーラーは、**SYSTEM** アカウントのアクセス許可を使って実行します。 ローカル システム アカウントには、IIS 共有構成によって使われる共有パスに対する変更アクセス許可がないため、インストーラーが共有上の *applicationHost.config* 内のモジュール設定を構成しようとすると、アクセス拒否エラーが発生します。 IIS 共有構成を使うときは、次の手順で行います。

1. IIS 共有構成を無効にします。
1. インストーラーを実行します。
1. 更新された *applicationHost.config* ファイルを共有にエクスポートします。
1. IIS 共有構成を再び有効にします。

## <a name="module-version-and-hosting-bundle-installer-logs"></a>モジュールのバージョンとホスティング バンドル インストーラーのログ

インストールされている ASP.NET Core モジュールのバージョンを確認するには:

1. ホスティング システム上で、*%windir%\System32\inetsrv* に移動します。
1. *aspnetcore.dll* ファイルを探します。
1. ファイルを右クリックし、コンテキスト メニューの **[プロパティ]** を選びます。
1. **[詳細]** タブを選びます。**[ファイル バージョン]** と **[製品バージョン]** が、インストールされているモジュールのバージョンを表します。

モジュールのホスティング バンドル インストーラーのログは、*C:\\Users\\%UserName%\\AppData\\Local\\Temp* にあります。ファイルの名前は、*dd_DotNetCoreWinSvrHosting__\<タイムスタンプ>_000_AspNetCoreModule_x64.log* です。

## <a name="module-schema-and-configuration-file-locations"></a>モジュール、スキーマ、構成ファイルの場所

### <a name="module"></a>Module

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

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

ファイルは、*applicationHost.config* ファイルで *aspnetcore.dll* を検索することにより見つかります。 IIS Express の場合、*applicationHost.config* ファイルは既定では存在しません。 ファイルは、Visual Studio ソリューションにおいて Web アプリ プロジェクトを開始すると、*\<アプリケーション ルート>\\.vs\\config* に作成されます。
