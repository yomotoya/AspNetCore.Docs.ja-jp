---
title: IIS で ASP.NET Core をトラブルシューティングします。
author: guardrex
description: ASP.NET Core アプリケーションのインターネット インフォメーション サービス (IIS) の展開に関する問題を診断する方法を説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e44892d2022ca1a176cee9d027e220e196c6572d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>IIS で ASP.NET Core をトラブルシューティングします。

作成者: [Luke Latham](https://github.com/guardrex)

この記事では手順 ASP.NET Core を診断する方法のアプリ起動時の問題でホストする場合[インターネット インフォメーション サービス (IIS)](/iis)です。 この記事の情報は、Windows Server および Windows デスクトップ上の IIS でホストに適用されます。

ASP.NET Core プロジェクトは、Visual Studio で、 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)デバッグ中にホストします。 A *502.5 プロセス エラー* troubleshooted このトピックで紹介するヒントを使用してデバッグすることができるローカルで発生します。

関連するトラブルシューティング トピック:

[Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)  
App Service を使用しますが、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)し、IIS ホストのアプリを App Service に固有の方法については、専用のトピックを参照してください。

[エラーを処理します。](xref:fundamentals/error-handling)  
ローカル システムでの開発時に ASP.NET Core アプリケーションでエラーを処理する方法を検出します。

[Visual Studio を使用したデバッグについて理解する](/visualstudio/debugger/getting-started-with-the-debugger)  
このトピックでは、Visual Studio デバッガーの機能を紹介します。

## <a name="app-startup-errors"></a>アプリの起動エラー

**502.5 プロセス障害**  
ワーカー プロセスは失敗します。 アプリが起動しません。

ASP.NET Core モジュールは、ワーカー プロセスを開始しようとしていますが、開始に失敗しました。 プロセスの起動エラーの原因は、内のエントリから通常決定できます、[アプリケーション イベント ログ](#application-event-log)と[ASP.NET Core モジュール stdout ログ](#aspnet-core-module-stdout-log)です。

*502.5 プロセス エラー*をホストしているか、アプリ構成が正しくないと、ワーカー プロセスが失敗する場合、エラー ページが返されます。

![ブラウザー ウィンドウ 502.5 プロセスのエラー ページの表示](troubleshoot/_static/process-failure-page.png)

**500 内部サーバー エラー**  
アプリが起動、エラーが発生して、サーバー要求を満たすことができます。

このエラーは、スタートアップ時または応答の作成中に、アプリのコード内で発生します。 応答にコンテンツを含んでいない可能性がありますか、応答が表示されます、 *500 Internal Server Error*ブラウザーにします。 アプリケーション イベント ログは、通常、アプリが正常に開始されたことを示しています。 サーバーの観点からは正しい動作です。 アプリが起動しますが、有効な応答を生成できません。 [コマンド プロンプトで、アプリを実行](#run-the-app-at-a-command-prompt)サーバー上または[ASP.NET コア モジュールの標準出力ログを有効にする](#aspnet-core-module-stdout-log)問題をトラブルシューティングします。

**接続のリセット**

サーバーが送信するためには遅すぎるはヘッダーが送信された後にエラーが発生した場合、 **500 Internal Server Error**エラーが発生します。 これは多くの場合、複合オブジェクトの応答のシリアル化中にエラーが発生したときに発生します。 としてこの種類のエラーが表示されます、*接続のリセット*クライアントでエラーが発生します。 [アプリケーションのログ記録](xref:fundamentals/logging/index)この種のエラーのトラブルシューティングに役立つことができます。

## <a name="default-startup-limits"></a>既定のスタートアップの制限

ASP.NET Core モジュールが、既定値で構成されている*startupTimeLimit* 120 秒です。 既定値のままにするとアプリ、モジュール、プロセスのエラーをログに記録する前に開始に最大 2 分かかる場合があります。 モジュールを構成する方法の詳細については、次を参照してください。 [aspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)です。

## <a name="troubleshoot-app-startup-errors"></a>アプリの起動エラーをトラブルシューティングします。

### <a name="application-event-log"></a>アプリケーション イベント ログ

アプリケーション イベント ログにアクセスします。

1. [スタート] メニューを開き、検索**イベント ビューアー**、クリックして、**イベント ビューアー**アプリ。
1. **イベント ビューアー**を開き、 **Windows ログ**ノード。
1. 選択**アプリケーション**をアプリケーション イベント ログを開きます。
1. 問題のあるアプリに関連付けられたエラーを検索します。 エラーの値を持つ*IIS AspNetCore モジュール*または*IIS Express AspNetCore モジュール*で、*ソース*列です。

### <a name="run-the-app-at-a-command-prompt"></a>コマンド プロンプトで、アプリを実行します。

多くのスタートアップ エラーは、アプリケーション イベント ログでの有用な情報を得られない。 ホスト システムで、コマンド プロンプトで、アプリを実行していくつかのエラーの原因を見つけることができます。

**フレームワークに依存する展開**

アプリの場合、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. コマンド プロンプトでの展開フォルダに移動しを持つアプリのアセンブリを実行することによって、アプリを実行*dotnet.exe*です。 次のコマンドでのアプリのアセンブリの名前に置き換えます\<アセンブリ名 >:`dotnet .\<assembly_name>.dll`です。
1. コンソール アプリから、すべてのエラーを示す出力がコンソール ウィンドウに書き込まれます。
1. アプリへの要求を行うときにエラーが発生した場合は、ホストと Kestrel がリッスンするポートに要求します。 既定のホストと post を使用して要求を行う`http://localhost:5000/`です。 応答した場合、アプリ通常 Kestrel エンドポイント アドレスで、問題がより多く関連のリバース プロキシの構成、およびアプリ内可能性が低い。

**自己完結型の配置**

アプリの場合、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd):

1. コマンド プロンプトでの展開フォルダに移動し、アプリの実行可能ファイルを実行します。 次のコマンドでのアプリのアセンブリの名前に置き換えます\<アセンブリ名 >:`<assembly_name>.exe`です。
1. コンソール アプリから、すべてのエラーを示す出力がコンソール ウィンドウに書き込まれます。
1. アプリへの要求を行うときにエラーが発生した場合は、ホストと Kestrel がリッスンするポートに要求します。 既定のホストと post を使用して要求を行う`http://localhost:5000/`です。 応答した場合、アプリ通常 Kestrel エンドポイント アドレスで、問題がより多く関連のリバース プロキシの構成、およびアプリ内可能性が低い。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core モジュール stdout ログ

有効にし、標準出力ログを表示します。

1. ホスト システムに、サイトの展開フォルダに移動します。
1. 場合、*ログ*フォルダーが存在しない、フォルダーを作成します。 MSBuild を有効にする方法については、作成する、*ログ*展開内のフォルダーは自動的を参照してください、[ディレクトリ構造](xref:host-and-deploy/directory-structure)トピックです。
1. 編集、 *web.config*ファイル。 設定**stdoutLogEnabled**に`true`を変更して、 **stdoutLogFile**を指すパス、*ログ*フォルダー (たとえば、 `.\logs\stdout`)。 `stdout` パスは、ログ ファイル名のプレフィックスです。 ログが作成されるときは、タイムスタンプ、プロセス id、およびファイルの拡張子が自動的に追加します。 使用して`stdout`一般的なログ ファイルの名前は、ファイル名のプレフィックスとして*stdout_20180205184032_5412.log*です。 
1. 更新された保存*web.config*ファイル。
1. アプリへの要求を作成します。
1. 移動し、*ログ*フォルダーです。 検索して、最新の標準出力ログを開きます。
1. エラー ログを調査します。

**重要**。 標準出力ログのトラブルシューティングが完了したら無効にします。

1. 編集、 *web.config*ファイル。
1. 設定**stdoutLogEnabled**に`false`です。
1. ファイルを保存します。

> [!WARNING]
> エラーを標準出力ログを無効にするは、アプリケーションまたはサーバーの障害につながります。 ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。
>
> ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。 詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。

## <a name="enabling-the-developer-exception-page"></a>開発者の例外のページを有効にします。

`ASPNETCORE_ENVIRONMENT` [環境変数は web.config に追加できる](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)開発環境でアプリケーションを実行します。 環境はありませんでアプリのスタートアップでオーバーライドされた場合に限り`UseEnvironment`ホスト ビルダーで、環境変数を設定することができます、[開発者例外ページ](xref:fundamentals/error-handling)に表示されるときに、アプリを実行します。

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

環境変数を設定`ASPNETCORE_ENVIRONMENT`ステージングと、インターネットに公開されないサーバーのテストで使用するためのみお勧めします。 環境変数からの削除、 *web.config*トラブルシューティング後のファイルです。 環境変数の設定の詳細について*web.config*を参照してください[aspNetCore の子要素を environmentVariables](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)です。

## <a name="common-startup-errors"></a>起動の一般的なエラー 

参照してください、 [ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)です。 ほとんどのアプリの起動を妨げる一般的な問題は、リファレンス トピックについて説明します。

## <a name="slow-or-hanging-app"></a>速度低下またはハングしているアプリ

アプリは、緩やかに変化が応答または要求でハング、ときに取得し、分析、[ダンプ ファイル](/visualstudio/debugger/using-dump-files)です。 ダンプ ファイルは、次のツールを使用して取得できます。

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Windows 用デバッグ ツールをダウンロード](https://developer.microsoft.com/windows/hardware/download-windbg)、[を使用して WinDbg のデバッグ](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>リモート デバッグ

参照してください[リモート デバッグ ASP.NET Core の IIS のリモート コンピューターに Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio のマニュアルでします。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/)エラー ログとレポート機能を含む、IIS でホストされているアプリから製品利用統計情報を提供します。 Application Insights は、アプリケーションのログ記録機能が使用可能になるときに、アプリの起動後に発生したエラーに関するレポートのみできます。 詳細については、次を参照してください。 [ASP.NET Core の Application Insights](/azure/application-insights/app-insights-asp-net-core)です。

## <a name="additional-troubleshooting-advice"></a>追加のトラブルシューティングのアドバイス

機能しているアプリが失敗する、開発コンピューターまたはパッケージのバージョンで、アプリ内でいずれか、.NET Core SDK をアップグレードした後すぐにします。 場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。 これらの問題のほとんどは、次の手順に従って修正できます。

1. 削除、 *bin*と*obj*フォルダーです。
1. パッケージをキャッシュするチェック ボックスをオフ*%userprofile%\\.nuget\\パッケージ*と*%localappdata%\\Nuget\\v3 キャッシュ*です。
1. 復元し、プロジェクトをリビルドします。
1. アプリを再展開する前に、サーバー上の以前の展開が完全に削除されたことを確認します。

> [!TIP]
> パッケージのキャッシュを消去する便利な方法は、実行する`dotnet nuget locals all --clear`コマンド プロンプトからです。
> 
> パッケージのキャッシュをクリアする行うこともできますを使用して、 [nuget.exe](https://www.nuget.org/downloads)ツールとコマンドを実行する`nuget locals all -clear`です。 *nuget.exe* Windows デスクトップ オペレーティング システムにバンドルされている、インストールされていないしから個別に取得する必要があります、 [NuGet の web サイト](https://www.nuget.org/downloads)です。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core でのエラー処理の概要](xref:fundamentals/error-handling)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)
