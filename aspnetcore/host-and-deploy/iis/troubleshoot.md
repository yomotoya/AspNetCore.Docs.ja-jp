---
title: IIS での ASP.NET Core のトラブルシューティング
author: guardrex
description: ASP.NET Core アプリのインターネット インフォメーション サービス (IIS) の展開に関する問題を診断する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: f22914c9b0d6d1902dd37c9b21b80a18894c97e7
ms.sourcegitcommit: d1c4580f56656b503cf528ec9f5ba570d790b57d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "41751644"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS での ASP.NET Core のトラブルシューティング

作成者: [Luke Latham](https://github.com/guardrex)

この記事では、[インターネット インフォメーション サービス (IIS)](/iis) を使用してホストしている場合に、ASP.NET Core アプリの起動時に関する問題を診断する方法について説明します。 この記事の情報は、Windows Server および Windows デスクトップ上の IIS でのホスティングに適用されます。

Visual Studio では、ASP.NET Core プロジェクトのデバッグ時に [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) のホスティングが既定の設定です。 このトピックのアドバイスを参照すると、ローカルでのデバッグ時に発生する *502.5 処理エラー*を解決できます。

その他のトラブルシューティング トピック:

[Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)  
App Service は[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)と IIS を使用してアプリをホストしますが、App Service 固有の手順については、この専用のトピックを参照してください。

[エラーの処理](xref:fundamentals/error-handling)  
ローカル システム上で開発しているときに発生する ASP.NET Core アプリのエラーを処理する方法について説明しています。

[Visual Studio を使用したデバッグについて理解する](/visualstudio/debugger/getting-started-with-the-debugger)  
このトピックでは、Visual Studio デバッガーの機能を紹介しています。

[Visual Studio Code でのデバッグ](https://code.visualstudio.com/docs/editor/debugging)  
Visual Studio Code に組み込まれているデバッグのサポートについて説明します。

## <a name="app-startup-errors"></a>アプリ起動時のエラー

**502.5 処理エラー**  
ワーカー プロセスが失敗します。 アプリは起動しません。

ASP.NET Core モジュールはワーカー プロセスの開始を試みますが、開始に失敗します。 プロセス起動時のエラーの原因は、通常、[アプリケーション イベント ログ](#application-event-log)と [ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)のエントリから判断できます。

正しく構成されていないホスティングやアプリが原因でワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

**500 内部サーバー エラー**  
アプリは起動しますが、エラーのためにサーバーは要求を実行できません。

このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。 応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。 通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。 サーバーから見るとそれは正しいことです。 アプリは起動しましたが、有効な応答を生成できません。 問題を解決するには、[サーバー上のコマンド プロンプトでアプリを実行する](#run-the-app-at-a-command-prompt)か、[ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。

**接続のリセット**

ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。 このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。 この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。 この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。

## <a name="default-startup-limits"></a>既定の起動制限

ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。 既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。 モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。

## <a name="troubleshoot-app-startup-errors"></a>アプリの起動エラーのトラブルシューティング

### <a name="application-event-log"></a>アプリケーション イベント ログ

アプリケーション イベント ログにアクセスします。

1. [スタート] メニューを開き、**イベント ビューアー**を検索し、**イベント ビューアー** アプリを選択します。
1. **イベント ビューアー**で **[Windows ログ]** ノードを開きます。
1. **[Application]** を選択して、アプリケーション イベント ログを開きます。
1. 失敗したアプリに関連するエラーを検索します。 エラーの *[ソース]* 列には *IIS AspNetCore Module* または *IIS Express AspNetCore Module* の値が表示されます。

### <a name="run-the-app-at-a-command-prompt"></a>コマンド プロンプトでアプリを実行する

多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。 ホスティング システムのコマンド プロンプトでアプリを実行すると、エラーの原因がわかることがあります。

**フレームワークに依存する展開**

アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)の場合:

1. コマンド プロンプトで展開フォルダーに移動し、*dotnet.exe* 使用してアプリのアセンブリを実行して、アプリを実行します。 コマンド `dotnet .\<assembly_name>.dll` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。
1. エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。
1. アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。 既定のホストと post を使用して `http://localhost:5000/` に要求を行います。 アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はリバース プロキシ設定に関連している可能性が高く、アプリ内が原因の可能性は低くなります。

**自己完結型の展開**

アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合:

1. コマンド プロンプトで、展開フォルダーに移動し、アプリの実行可能ファイルを実行します。 コマンド `<assembly_name>.exe` の \<assembly_name> にアプリのアセンブリの名前を指定して実行します。
1. エラーを示すアプリからのコンソール出力は、すべてコンソール ウィンドウに書き込まれます。
1. アプリへの要求時にエラーが発生した場合は、Kestrel がリッスンしているホストとポートに要求が送信されます。 既定のホストと post を使用して `http://localhost:5000/` に要求を行います。 アプリが Kestrel のエンドポイント アドレスで正常に応答する場合、問題はリバース プロキシ設定に関連している可能性が高く、アプリ内が原因の可能性は低くなります。

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

**重要**。 トラブルシューティングが完了したら、stdout ログを無効にします。

1. *web.config* ファイルを編集します。
1. **stdoutLogEnabled** を `false` に設定します。
1. ファイルを保存します。

> [!WARNING]
> stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。 ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。
>
> ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。 詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。

## <a name="enabling-the-developer-exception-page"></a>開発者例外ページを有効にする

開発環境でアプリを実行するには、`ASPNETCORE_ENVIRONMENT` [環境変数を web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) に追加することができます。 アプリの起動時にホスト ビルダー上で `UseEnvironment` によって環境がオーバーライドされない限り、環境変数を設定すると、アプリの実行時に[開発者例外ページ](xref:fundamentals/error-handling)が表示されます。

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

`ASPNETCORE_ENVIRONMENT` の環境変数を設定する方法は、インターネットに公開されないステージング サーバーやテスト用サーバーの場合にのみお勧めされます。 トラブルシューティング後は、必ず *web.config* ファイルからこの環境変数を削除してください。 *web.config* で環境変数を設定する方法の詳細については、[aspNetCore の environmentVariables 子要素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)を参照してください。

## <a name="common-startup-errors"></a>起動時の一般的なエラー 

[ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)に関するページを参照してください。 このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。

## <a name="slow-or-hanging-app"></a>アプリの速度低下またはハング

要求に対してアプリの反応が遅い場合、またはハングする場合は、[ダンプ ファイル](/visualstudio/debugger/using-dump-files)を取得して分析します。 ダンプ ファイルは、次のいずれかのツールを使用して取得できます。

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: 「[Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg)」(Windows 用デバッグ ツールのダウンロード)、「[Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)」(WinDbg を使用したデバッグ)

## <a name="remote-debugging"></a>リモート デバッグ

Visual Studio ドキュメントの「[Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)」(Visual Studio 2017 でリモート IIS コンピューター上の ASP.NET Core をリモート デバッグする) を参照してください。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) は、IIS でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。 Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。 詳細については、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」を参照してください。

## <a name="additional-troubleshooting-advice"></a>その他のトラブルシューティングのアドバイス

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

* [ASP.NET Core でのエラー処理の概要](xref:fundamentals/error-handling)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)
