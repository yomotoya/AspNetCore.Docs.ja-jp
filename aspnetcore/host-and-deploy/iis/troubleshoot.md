---
title: IIS での ASP.NET Core のトラブルシューティング
author: guardrex
description: ASP.NET Core アプリのインターネット インフォメーション サービス (IIS) の展開に関する問題を診断する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 1fa90737aadebe3f714c702fbce649629d79dcd4
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264551"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS での ASP.NET Core のトラブルシューティング

作成者: [Luke Latham](https://github.com/guardrex)

この記事では、[インターネット インフォメーション サービス (IIS)](/iis) を使用してホストしている場合に、ASP.NET Core アプリの起動時に関する問題を診断する方法について説明します。 この記事の情報は、Windows Server および Windows デスクトップ上の IIS でのホスティングに適用されます。

::: moniker range=">= aspnetcore-2.2"

Visual Studio では、ASP.NET Core プロジェクトのデバッグ時に [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) のホスティングが既定の設定です。 このトピックのアドバイスを参照すると、ローカルでのデバッグ時に発生する *502.5 処理エラー*または *500.30 - 開始エラー*を解決できます。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Visual Studio では、ASP.NET Core プロジェクトのデバッグ時に [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) のホスティングが既定の設定です。 このトピックのアドバイスを参照すると、ローカルでのデバッグ時に発生する *502.5 処理エラー*を解決できます。

::: moniker-end

その他のトラブルシューティング トピック:

<xref:host-and-deploy/azure-apps/troubleshoot>App Service は [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) と IIS を使用してアプリをホストしますが、App Service 固有の手順については、この専用のトピックを参照してください。

<xref:fundamentals/error-handling> ローカル システム上で開発しているときに発生する ASP.NET Core アプリのエラーを処理する方法について説明しています。

[Visual Studio を使用したデバッグについて理解する](/visualstudio/debugger/getting-started-with-the-debugger) このトピックでは、Visual Studio デバッガーの機能を紹介しています。

[Visual Studio Code でのデバッグ](https://code.visualstudio.com/docs/editor/debugging) Visual Studio Code に組み込まれているデバッグのサポートについて説明します。

## <a name="app-startup-errors"></a>アプリ起動時のエラー

### <a name="5025-process-failure"></a>502.5 処理エラー

ワーカー プロセスが失敗します。 アプリは起動しません。

ASP.NET Core モジュールはバックエンドのドットネット プロセスの開始を試みますが、開始に失敗します。 プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断できます。

一般的なエラー条件は、存在しないバージョンの ASP.NET Core 共有フレームワークが対象にされていて、アプリが正しく構成されていないことです。 対象のコンピューターにどのバージョンの ASP.NET Core 共有フレームワークがインストールされているかを確認します。

正しく構成されていないホスティングやアプリが原因でワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 インプロセス起動エラー

ワーカー プロセスが失敗します。 アプリは起動しません。

ASP.NET Core モジュールは .NET Core CLR の開始をインプロセスで試みますが、開始に失敗します。 プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断できます。

一般的なエラー条件は、存在しないバージョンの ASP.NET Core 共有フレームワークが対象にされていて、アプリが正しく構成されていないことです。 対象のコンピューターにどのバージョンの ASP.NET Core 共有フレームワークがインストールされているかを確認します。

### <a name="5000-in-process-handler-load-failure"></a>500.0 インプロセス ハンドラーの読み込みエラー

ワーカー プロセスが失敗します。 アプリは起動しません。

ASP.NET Core モジュールは .NET Core CLR の検出に失敗し、インプロセス要求ハンドラー (*aspnetcorev2_inprocess.dll*) を検出します。 次の点をご確認ください。

* アプリが [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet パッケージまたは [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)を対象としている。
* アプリが対象としているバージョンの ASP.NET Core 共有フレームワークが対象のコンピューターにインストールされている。

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 アウト プロセス ハンドラーの読み込みエラー

ワーカー プロセスが失敗します。 アプリは起動しません。

ASP.NET Core モジュールは、アウト プロセスのホスティング要求ハンドラーの検索に失敗します。 *aspnetcorev2_outofprocess.dll* が *aspnetcorev2.dll* の隣のサブフォルダーにあることを確認してください。

::: moniker-end

### <a name="500-internal-server-error"></a>500 内部サーバー エラー

アプリは起動しますが、エラーのためにサーバーは要求を実行できません。

このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。 応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。 通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。 サーバーから見るとそれは正しいことです。 アプリは起動しましたが、有効な応答を生成できません。 問題を解決するには、[サーバー上のコマンド プロンプトでアプリを実行する](#run-the-app-at-a-command-prompt)か、[ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>アプリケーションの起動に失敗しました (ErrorCode '0x800700c1')

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

アプリのアセンブリ (*.dll*) を読み込めなかったため、アプリの起動に失敗しました。

このエラーは、公開されたアプリと w3wp/iisexpress プロセスの間でビットの不一致がある場合に発生します。

アプリ プールの 32 ビット設定が正しいことを確認してください。

1. IIS マネージャーの **[アプリケーション プール]** でそのアプリ プールを選択します。
1. **[アクション]** パネルの **[アプリケーション プールの編集]** で **[詳細設定]** を選択します。
1. **[32 ビット アプリケーションの有効化]** を設定します。
   * 32 ビット (x86) アプリを展開する場合は、この値を `True` に設定します。
   * 64 ビット (x64) アプリを展開する場合は、この値を `False` に設定します。

### <a name="connection-reset"></a>接続のリセット

ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。 このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。 この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。 この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。

## <a name="default-startup-limits"></a>既定の起動制限

ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。 既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。 モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。

## <a name="troubleshoot-app-startup-errors"></a>アプリの起動エラーのトラブルシューティング

### <a name="enable-the-aspnet-core-module-debug-log"></a>ASP.NET Core モジュールのデバッグ ログを有効にする

ASP.NET Core モジュールのデバッグ ログを有効にするには、次のハンドラー設定をアプリの *web.config* ファイルに追加します。

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

ログに指定されたパスが存在することと、アプリ プールの ID にその場所への書き込みアクセス許可があることを確認します。

### <a name="application-event-log"></a>アプリケーション イベント ログ

アプリケーション イベント ログにアクセスします。

1. [スタート] メニューを開き、**イベント ビューアー**を検索し、**イベント ビューアー** アプリを選択します。
1. **イベント ビューアー**で **[Windows ログ]** ノードを開きます。
1. **[Application]** を選択して、アプリケーション イベント ログを開きます。
1. 失敗したアプリに関連するエラーを検索します。 エラーの *[ソース]* 列には *IIS AspNetCore Module* または *IIS Express AspNetCore Module* の値が表示されます。

### <a name="run-the-app-at-a-command-prompt"></a>コマンド プロンプトでアプリを実行する

多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。 ホスティング システムのコマンド プロンプトでアプリを実行すると、エラーの原因がわかることがあります。

#### <a name="framework-dependent-deployment"></a>フレームワークに依存する展開

アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:

1. コマンド プロンプトで展開フォルダーに移動し、*dotnet.exe* 使用してアプリのアセンブリを実行して、アプリを実行します。 コマンド `dotnet .\<assembly_name>.dll` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。
1. エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。
1. アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。 既定のホストと post を使用して `http://localhost:5000/` に要求を行います。 アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はホスティングの構成に関連している可能性が高く、アプリ内が原因の可能性は低くなります。

#### <a name="self-contained-deployment"></a>自己完結型の展開

アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:

1. コマンド プロンプトで、展開フォルダーに移動し、アプリの実行可能ファイルを実行します。 コマンド `<assembly_name>.exe` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。
1. エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。
1. アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。 既定のホストと post を使用して `http://localhost:5000/` に要求を行います。 アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はホスティングの構成に関連している可能性が高く、アプリ内が原因の可能性は低くなります。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core モジュールの stdout ログ

stdout ログを有効にして表示するには:

1. ホスティング システム上のサイトの展開フォルダーに移動します。
1. *logs* フォルダーが存在しない場合は、フォルダーを作成します。 MSBuild で展開フォルダーに *logs* フォルダーを自動的に作成されるようにする手順については、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。
1. *web.config* ファイルを編集します。 **stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** のパスを *logs* フォルダー (たとえば `.\logs\stdout`) を指すように変更します。 パスの `stdout` はログ ファイル名のプレフィックスです。 ログ ファイルの作成時には、タイムスタンプ、プロセス ID、およびファイルの拡張子が自動的に追加されます。 ファイル名のプレフィックスとして `stdout` を使用すると、一般的なログ ファイルの名前は *stdout_20180205184032_5412.log* になります。
1. アプリケーション プールの ID に *logs* フォルダーへの書き込みアクセス許可があることを確認します。
1. 更新した *web.config* ファイルを保存します。
1. アプリに対して要求します。
1. *logs* フォルダーに移動します。 最新の stdout ログを探して開きます。
1. ログのエラーを調べます。

> [!IMPORTANT]
> トラブルシューティングが完了したら、stdout ログを無効にします。

1. *web.config* ファイルを編集します。
1. **stdoutLogEnabled** を `false` に設定します。
1. ファイルを保存します。

> [!WARNING]
> stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。 ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。
>
> ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。 詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。

## <a name="enable-the-developer-exception-page"></a>開発者例外ページを有効にする

開発環境でアプリを実行するには、`ASPNETCORE_ENVIRONMENT` [環境変数を web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) に追加することができます。 アプリの起動時にホスト ビルダー上で `UseEnvironment` によって環境がオーバーライドされない限り、環境変数を設定すると、アプリの実行時に[開発者例外ページ](xref:fundamentals/error-handling)が表示されます。

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

`ASPNETCORE_ENVIRONMENT` の環境変数を設定する方法は、インターネットに公開されないステージング サーバーやテスト用サーバーの場合にのみお勧めされます。 トラブルシューティング後は、必ず *web.config* ファイルからこの環境変数を削除してください。 *web.config* で環境変数を設定する方法の詳細については、[aspNetCore の environmentVariables 子要素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)を参照してください。

## <a name="common-startup-errors"></a>起動時の一般的なエラー

以下を参照してください。<xref:host-and-deploy/azure-iis-errors-reference> このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。

## <a name="obtain-data-from-an-app"></a>アプリからデータを取得する

アプリが要求に応答できる場合は、ターミナル インライン ミドルウェアを使用して、要求、接続、その他のデータをアプリから取得します。 詳細およびサンプル コードについては、「<xref:test/troubleshoot#obtain-data-from-an-app>」を参照してください。

## <a name="create-a-dump"></a>ダンプを作成する

"*ダンプ*" とはシステムのメモリのスナップショットであり、アプリのクラッシュ、起動の失敗、遅いアプリの原因を突き止めるのに役立ちます。

### <a name="app-crashes-or-encounters-an-exception"></a>アプリのクラッシュまたは例外の発生

[Windows エラー報告 (WER)](/windows/desktop/wer/windows-error-reporting) からダンプを取得して分析します。

1. `c:\dumps` にクラッシュ ダンプ ファイルを保持するフォルダーを作成します。 アプリ プールには、そのフォルダーへの書き込みアクセス権が必要です。
1. [EnableDumps PowerShell スクリプト](https://github.com/aspnet/Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1) を実行します。
   * アプリで[インプロセス ホスティング モデル](xref:fundamentals/servers/index#in-process-hosting-model)が使われている場合は、*w3wp.exe* に対するスクリプトを実行します。

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * アプリで[アウト プロセス ホスティング モデル](xref:fundamentals/servers/index#out-of-process-hosting-model)が使われている場合は、*dotnet.exe* に対するスクリプトを実行します。

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. クラッシュが発生する条件の下でアプリを実行します。
1. クラッシュが発生した後、[DisableDumps PowerShell スクリプト](https://github.com/aspnet/Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1)を実行します。
   * アプリで[インプロセス ホスティング モデル](xref:fundamentals/servers/index#in-process-hosting-model)が使われている場合は、*w3wp.exe* に対するスクリプトを実行します。

     ```console
     .\DisableDumps w3wp.exe
     ```

   * アプリで[アウト プロセス ホスティング モデル](xref:fundamentals/servers/index#out-of-process-hosting-model)が使われている場合は、*dotnet.exe* に対するスクリプトを実行します。

     ```console
     .\DisableDumps dotnet.exe
     ```

アプリがクラッシュし、ダンプの収集が完了したら、アプリを普通に終了してかまいません。 PowerShell スクリプトにより、アプリごとに最大 5 つのダンプを収集する WER が構成されます。

> [!WARNING]
> クラッシュ ダンプでは、大量のディスク領域 (1 つにつき最大、数ギガバイト) が使用される場合があります。

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>アプリが起動時または正常な実行中にハングまたは失敗する

アプリが起動時または正常な実行中に "*ハング*" (応答を停止するがクラッシュしない) または失敗するときは、「[User-Mode Dump Files:Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)」(ユーザー モード ダンプ ファイル: 最適なツールの選択) を参照し、適切なツールを選択してダンプを生成します。

### <a name="analyze-the-dump"></a>ダンプを分析する

ダンプはいくつかの方法で分析できます。 詳細については、「[Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)」(ユーザー モード ダンプ ファイルの分析) を参照してください。

## <a name="remote-debugging"></a>リモート デバッグ

Visual Studio ドキュメントの「[Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)」(Visual Studio 2017 でリモート IIS コンピューター上の ASP.NET Core をリモート デバッグする) を参照してください。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) は、IIS でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。 Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。 詳細については、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」を参照してください。

## <a name="additional-advice"></a>その他の注意事項

開発コンピューター上の .NET Core SDK またはアプリ内のパッケージ バージョンのいずれかをアップグレードした直後に、機能しているアプリが失敗することがあります。 場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。 これらの問題のほとんどは、次の手順で解決できます。

1. *bin* フォルダーと *obj* フォルダーを削除します。
1. *%UserProfile%\\.nuget\\packages* と *%LocalAppData%\\Nuget\\v3-cache* のパッケージ キャッシュをクリアします。
1. プロジェクトを復元してリビルドします。
1. アプリを再展開する前に、サーバー上の以前の展開が完全に削除されていることを確認します。

> [!TIP]
> パッケージ キャッシュをクリアするには、コマンド プロンプトから `dotnet nuget locals all --clear` を実行する方法が便利です。
>
> パッケージ キャッシュをクリアするには、[nuget.exe](https://www.nuget.org/downloads) ツールを使用して `nuget locals all -clear` コマンドを実行する方法もあります。 *nuget.exe* は、Windows デスクトップ オペレーティング システムにバンドルされているインストールではなく、[NuGet Web サイト](https://www.nuget.org/downloads)から別に入手する必要があります。

## <a name="additional-resources"></a>その他の技術情報

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
