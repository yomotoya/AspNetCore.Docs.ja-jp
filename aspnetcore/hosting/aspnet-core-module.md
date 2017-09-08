---
title: "ASP.NET Core モジュール構成の参照"
author: guardrex
description: "ASP.NET Core アプリケーションをホストする ASP.NET Core モジュールを構成する方法。"
keywords: "ASP.NET Core、ancm コア モジュール、iis、stdout のログ記録、環境変数、環境変数、subapplication、subapp、appoffline app_offline、502、スキーマ"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: a676b695160b7219bd13f3915e291b722eef47c8
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2017
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core モジュール構成の参照

によって[Luke Latham](https://github.com/GuardRex)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、および[Sourabh Shirhatti](https://twitter.com/sshirhatti)

このドキュメントでは、ASP.NET Core アプリケーションをホストする ASP.NET Core モジュールを構成する方法の詳細を説明します。 ASP.NET Core モジュールとインストール手順の概要については、次を参照してください。、 [ASP.NET コア モジュールの概要](xref:fundamentals/servers/aspnet-core-module)です。

## <a name="configuration-via-webconfig"></a>Web.config を使用して構成

ASP.NET Core モジュールは、サイトまたはアプリケーションを介して構成*web.config*ファイルし、それ自体が`aspNetCore`構成セクション内で`system.webServer`です。 次に例を示します*web.config*ファイルを`Microsoft.NET.Sdk.Web`SDK は、プロジェクトが発行された場合、[フレームワークに依存する展開](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)プレース ホルダーの付いた、`processPath`と`arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*Web.config*次の例は、[自己完結型の配置](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd)を[Azure App Service](https://azure.microsoft.com/services/app-service/)です。 詳細については、次を参照してください。[を IIS に発行](xref:publishing/iis)です。 参照してください[サブ アプリケーションの構成](xref:publishing/iis#configuration-of-sub-applications)の構成に関連する重要なメモを*web.config*サブ アプリケーション内のファイルです。

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore 要素の属性

| 属性 | 説明 |
| --- | --- |
| processPath | <p>必須の文字列属性です。</p><p>HTTP 要求をリッスンしているプロセスを起動する実行可能ファイルへのパス。 相対パスがサポートされます。 パスが始まる場合 '.'、パスはサイトのルートの相対パスであると見なされます。</p><p>既定値はありません。</p> |
| 引数 | <p>省略可能な文字列属性。</p><p>指定された実行可能ファイルへの引数**processPath**です。</p><p>既定値は空の文字列です。</p> |
| startupTimeLimit | <p>省略可能な整数属性です。</p><p>モジュールがポートでリッスンしているプロセスを開始する実行可能ファイルを待機する秒単位で期間です。 この制限時間を超えた場合、モジュールは、プロセスを終了します。 モジュールは、新しい要求を受信しは、アプリケーションの起動に失敗しない限り、後続の受信要求の処理を再開しようとしています引き続き、プロセスをもう一度起動しようとします。 **rapidFailsPerMinute**数。最後のローリング分の回数。</p><p>既定値は 120 です。</p> |
| shutdownTimeLimit | <p>省略可能な整数属性です。</p><p>対象のモジュールが正常にシャット ダウンする実行可能ファイルの待機秒単位で期間ときに、 *app_offline.htm*ファイルが検出されました。</p><p>既定値は 10 です。</p> |
| rapidFailsPerMinute | <p>省略可能な整数属性です。</p><p>プロセスが指定された回数を指定**processPath**は 1 分あたりのクラッシュを許可します。 この制限を超えた場合、モジュールは、プロセスが 1 分間の残りの部分の起動を停止します。</p><p>既定値は 10 です。</p> |
| requestTimeout | <p>省略可能な timespan 属性です。</p><p>ASP.NET Core モジュールが ASPNETCORE_PORT % でリッスンしているプロセスからの応答の待機期間を指定します。</p><p>既定値は、"00:02:00" です。</p> |
| stdoutLogEnabled | <p>省略可能な Boolean 属性です。</p><p>True の場合、 **stdout**と**stderr**で指定されたプロセスの**processPath**で指定されたファイルにリダイレクトされます**stdoutLogFile**.</p><p>既定値は false です。</p> |
| stdoutLogFile | <p>省略可能な文字列属性。</p><p>対象の相対パスまたは絶対ファイル パスを指定**stdout**と**stderr**で指定されたプロセスから**processPath**ログに記録されます。 相対パスは、サイトのルートです。 以降で任意のパス '.' サイト ルートに対する相対パスおよびその他のすべてのパスが絶対パスとして扱われます。 モジュール、ログ ファイルを作成するためにパスで提供されるすべてのフォルダーが存在する必要があります。 プロセス ID、タイムスタンプ (*yyyyMdhms*)、ファイル拡張子と (*.log*) アンダー スコアで区切り記号が追加、最後のセグメント、 **stdoutLogFile**提供します。</p><p>既定値は `aspnetcore-stdout` です。</p> |
| forwardWindowsAuthToken | true または false。</p><p>True の場合、トークンが 1 回の要求ヘッダー ' MS ASPNETCORE WINAUTHTOKEN' として ASPNETCORE_PORT % でリッスンしている子プロセスに転送されます。 このトークン要求ごとに CloseHandle を呼び出すには、そのプロセスの役割です。</p><p>既定値は true です。</p> |
| disableStartUpErrorPage | true または false。</p><p>True の場合、 **502.5 - プロセス エラー**ページは表示されずで構成されている 502 ステータス コード ページ、 *web.config*が優先されます。</p><p>既定値は false です。</p> |

### <a name="setting-environment-variables"></a>環境変数の設定

ASP.NET Core モジュールによりで指定されたプロセスの環境変数を指定する、`processPath`属性を 1 つ以上指定することによって`environmentVariable`の子要素、`environmentVariables`コレクション要素の下、`aspNetCore`要素。 このセクションで設定されている環境変数よりも優先システム プロセスの環境変数。

次の例では、2 つの環境変数を設定します。 `ASPNETCORE_ENVIRONMENT`アプリケーションの環境を構成`Development`です。 開発者は、この値にへの設定は一時的に可能性があります、 *web.config*を強制するためにファイル、[開発者例外ページ](xref:fundamentals/error-handling)アプリ例外をデバッグするときに読み込みます。 `CONFIG_DIR`スタートアップ時にアプリの構成ファイルを読み込むために、パスを形成する値を読み取るコードを記述は、ユーザー定義の環境変数の例を示します。

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

## <a name="appofflinehtm"></a>app_offline.htm

名前のファイルを配置する場合*app_offline.htm* web アプリケーションのディレクトリのルート ディレクトリにある ASP.NET Core モジュールは、アプリを正常にシャット ダウンしようし、着信要求の処理を停止します。 後に、アプリが実行中の場合`shutdownTimeLimit`数 (秒単位) の ASP.NET Core モジュールが実行中のプロセスを終了します。

中に、 *app_offline.htm*ファイルが存在 ASP.NET Core モジュールは要求に応答の内容を送信することによって、 *app_offline.htm*ファイル。 1 回、 *app_offline.htm*ファイルが削除され、次の要求が要求に応答し、アプリケーションを読み込みます。

## <a name="start-up-error-page"></a>スタートアップ エラー ページ

バックエンド プロセスまたはバックエンドの処理が開始されるが、構成されたポートでリッスンに失敗するを起動する ASP.NET Core モジュールが失敗した場合は、HTTP 502.5 ステータス コード ページが表示されます。 このページを抑制して、既定の IIS 502 ステータス コード ページを元に戻すを使用して、`disableStartUpErrorPage`属性。 カスタム エラー メッセージを構成する方法については、次を参照してください。 [HTTP エラー `<httpErrors>`](https://www.iis.net/configreference/system.webserver/httperrors)です。

![502 状態 ページ](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>ログの作成とリダイレクト

ASP.NET Core モジュール リダイレクト`stdout`と`stderr`を設定する場合はディスクへのログ、`stdoutLogEnabled`と`stdoutLogFile`の属性、`aspNetCore`要素。 すべてのフォルダ、`stdoutLogFile`パスは、モジュール、ログ ファイルを作成するために存在する必要があります。 タイムスタンプとファイルの拡張機能は、ログ ファイルの作成時に自動的に追加されます。 プロセスのリサイクル/再起動が発生しない限り、ログは回転しません。 ホストは、ログの使用ディスク領域を制限するの役割です。 使用して、`stdout`ログは、アプリケーションのスタートアップの問題のトラブルシューティングとない一般的なアプリケーションのログ記録のためにのみ推奨されます。

ログ ファイルの名前は、プロセス ID (PID)、タイムスタンプを追加することによって作成されて (*yyyyMdhms*)、ファイル拡張子と (*.log*) の最後のセグメントを`stdoutLogFile`パス (通常*stdout*) アンダー スコアで区切られます。 たとえば場合、`stdoutLogFile`パスの終わりに*stdout*、12時 05分: 02 で 8/10/2017 で作成された 10652 の PID を使用してアプリのログ ファイル名である*stdout_10652_20178101252.log*です。

サンプルを次に示します`aspNetCore`を構成する要素`stdout`ログ記録します。 `stdoutLogFile`の例に示すパスは Azure App Service に適しています。 ローカル パスまたはネットワーク共有のパスがローカル ログもかまわないです。 AppPool ユーザー id が指定されたパスに書き込む権限を持っていることを確認します。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>IIS と ASP.NET Core モジュール構成を共有します。

ASP.NET Core モジュール インストーラーの実行の権限を持つ、**システム**アカウント。 インストーラーが、アクセス拒否エラーのモジュールの設定を構成しようとするときに達するため、ローカル システム アカウントが変更しません IIS 共有構成で使用される共有のパスに対するアクセス許可、 *applicationHost.config*共有します。

IIS 共有構成を無効にする、インストーラーを実行、更新されたエクスポートのサポートされていないの回避策は*applicationHost.config*共有ファイルおよび IIS 共有構成を再度有効にします。

## <a name="module-schema-and-configuration-file-locations"></a>モジュール、スキーマ、および構成ファイルの場所

### <a name="module"></a>モジュール

**IIS (x86 または amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86 または amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %Programfiles (x86) %\IIS Express\aspnetcore.dll

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

検索することができます*aspnetcore.dll*で、 *applicationHost.config*ファイル。 IIS Express を*applicationHost.config*ファイルは既定では存在しません。 ファイルが作成された*{アプリケーション ルート}\.vs\config* Visual Studio ソリューションで任意の web アプリケーション プロジェクトを開始するとします。
