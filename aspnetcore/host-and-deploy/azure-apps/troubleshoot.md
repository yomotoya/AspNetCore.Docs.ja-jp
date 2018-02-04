---
title: "Azure App Service の ASP.NET Core をトラブルシューティングします。"
author: guardrex
description: "ASP.NET Core Azure App Service の配置に関する問題を診断する方法を学習します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 144af8e93bb935d07fd064d5f45b40faea4a2664
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Azure App Service の ASP.NET Core をトラブルシューティングします。

作成者: [Luke Latham](https://github.com/guardrex)

この記事は、Azure App Service の診断ツールを使用してアプリの起動の問題、ASP.NET Core を診断する方法の手順を説明します。 他のトラブルシューティング アドバイスについては、次を参照してください。 [Azure App Service の診断の概要](/azure/app-service/app-service-diagnostics)と[する方法: Azure App Service でアプリの監視](/azure/app-service/web-sites-monitor)、Azure ドキュメントでします。

## <a name="app-startup-errors"></a>アプリの起動エラー

**502.5 プロセス障害**  
ワーカー プロセスは失敗します。 アプリが起動しません。

[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ワーカー プロセスを開始する試行の開始に失敗します。 多くの場合、アプリケーション イベント ログを調べることは、この種の問題のトラブルシューティングに役立ちます。 」で説明されて、ログへのアクセス、[アプリケーション イベント ログ](#application-event-log)セクションです。

*502.5 プロセス エラー*正しく構成されていないアプリにより、ワーカー プロセスが失敗する場合、エラー ページが返されます。

![ブラウザー ウィンドウ 502.5 プロセスのエラー ページの表示](troubleshoot/_static/process-failure-page.png)

**500 内部サーバー エラー**  
アプリが起動、エラーが発生して、サーバー要求を満たすことができます。

このエラーは、スタートアップ時または応答の作成中に、アプリのコード内で発生します。 応答にコンテンツを含んでいない可能性がありますか、応答が表示されます、 *500 Internal Server Error*ブラウザーにします。 アプリケーション イベント ログは、通常、アプリが正常に開始されたことを示しています。 サーバーの観点からは正しい動作です。 アプリが起動しますが、有効な応答を生成できません。 [Kudu コンソールで、アプリを実行](#run-the-app-in-the-kudu-console)または[ASP.NET コア モジュールの標準出力ログを有効にする](#aspnet-core-module-stdout-log)して問題をトラブルシューティングします。

## <a name="troubleshoot-app-startup-errors"></a>アプリの起動エラーをトラブルシューティングします。

### <a name="application-event-log"></a>アプリケーション イベント ログ

アプリケーション イベント ログにアクセスするには、使用、**診断し、問題を解決して**ブレードで、Azure ポータルで。

1. Azure ポータルでのアプリのブレードを開く、 **App Services**ブレードです。
1. 選択、**診断し、問題を解決して**ブレードです。
1. **問題カテゴリの選択**、select、 **Web アプリを**ボタンをクリックします。
1. **推奨されるソリューション**のウィンドウを開く**アプリケーション イベント ログを開く**です。 選択、**アプリケーション イベント ログを開く**ボタンをクリックします。
1. によって提供される最新のエラーを調べて、 *IIS AspNetCoreModule*で、**ソース**列です。

使用する代わりに、**診断し、問題を解決して**を使用して直接アプリケーション イベント ログ ファイルを調べるには、ブレード[Kudu](https://github.com/projectkudu/kudu/wiki):

1. 選択、**高度なツール**ブレードに、**開発ツール**領域。 選択、**移動&rarr;**ボタンをクリックします。 Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。
1. ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。
1. 開く、 **LogFiles**フォルダーです。
1. の横にある鉛筆アイコンを選択、 *eventlog.xml*ファイル。
1. ログを調べます。 最新のイベントのログの末尾までスクロールします。

### <a name="run-the-app-in-the-kudu-console"></a>Kudu コンソールで、アプリを実行します。

多くのスタートアップ エラーは、アプリケーション イベント ログでの有用な情報を得られない。 アプリを実行することができます、 [Kudu](https://github.com/projectkudu/kudu/wiki)エラーを検出するリモートの実行コンソール。

1. 選択、**高度なツール**ブレードに、**開発ツール**領域。 選択、**移動&rarr;**ボタンをクリックします。 Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。
1. ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。
1. パスのフォルダーを開いて**サイト** > **wwwroot**です。
1. コンソールで、アプリのアセンブリを実行することによって、アプリを実行*dotnet.exe*です。 次のコマンドでのアプリのアセンブリの名前に置き換えます`<assembly_name>`:
   ```console
   dotnet .\<assembly_name>.dll
   ```
1. コンソール アプリから、すべてのエラーを示す出力は、Kudu コンソールにパイプします。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core モジュール stdout ログ

ASP.NET Core モジュールは、標準出力ログは、多くの場合、有用なエラー メッセージがアプリケーション イベント ログが見つかりません。 を記録します。 有効にし、標準出力ログを表示します。

1. 移動し、**診断し、問題を解決して**ブレードで、Azure ポータルでします。
1. **問題カテゴリの選択**、select、 **Web アプリを**ボタンをクリックします。
1. **推奨されるソリューション** > **標準出力ログのリダイレクトを有効にする**、ボタンをクリックして**Web.Config を編集する Kudu コンソールを開く**です。
1. Kudu**診断コンソール**、パスにフォルダーを開く**サイト** > **wwwroot**です。 公開まで下にスクロール、 *web.config*一覧の下部にあるファイルです。
1. 次に、鉛筆アイコンをクリックして、 *web.config*ファイル。
1. 設定**stdoutLogEnabled**に`true`を変更して、 **stdoutLogFile**へのパス:`\\?\%home%\LogFiles\stdout`です。
1. 選択**保存**を保存、更新された*web.config*ファイル。
1. アプリへの要求を作成します。
1. Azure ポータルに戻ります。 選択、**高度なツール**ブレードに、**開発ツール**領域。 選択、**移動&rarr;**ボタンをクリックします。 Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。
1. ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。
1. 選択、 **LogFiles**フォルダーです。
1. 検査、 **Modified**最新の変更日にログに記録列と、標準出力を編集する鉛筆アイコンを選択します。
1. ログ ファイルが開き、エラーが表示されます。

**大事な！** 標準出力ログのトラブルシューティングが完了したら無効にします。

1. Kudu**診断コンソール**パスに戻り、**サイト** > **wwwroot**を表示する、 *web.config*ファイル。 開く、 **web.config**鉛筆アイコンを選択してもう一度ファイル。
1. 設定**stdoutLogEnabled**に`false`です。
1. 選択**保存**ファイルを保存します。

> [!WARNING]
> エラーを標準出力ログを無効にするは、アプリケーションまたはサーバーの障害につながります。 ログ ファイルのサイズに制限はありませんか、作成されるログ ファイルの数があります。
>
> ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。 詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。

## <a name="common-startup-errors"></a>起動の一般的なエラー 

参照してください、 [ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)です。 ほとんどのアプリの起動を妨げる一般的な問題は、リファレンス トピックについて説明します。

## <a name="process-dump-for-a-slow-or-hanging-app"></a>アプリの速度低下またはハングしているプロセスのダンプ

参照するアプリでは、応答が遅くなると、要求でハング、 [Azure App Service のトラブルシューティング低速の web アプリケーション パフォーマンスの問題](/azure/app-service/app-service-web-troubleshoot-performance-degradation)のガイダンスをデバッグします。

## <a name="remote-debugging"></a>リモート デバッグ

参照してください[リモート デバッグのトラブルシューティング: Visual Studio を使用して Azure App service web アプリの web アプリ セクション](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)Azure ドキュメントでします。

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/)エラー ログとレポート機能を含む、Azure App Service でホストされているアプリから製品利用統計情報を提供します。 Application Insights は、アプリケーションのログ記録機能が使用可能になるときに、アプリの起動後に発生したエラーに関するレポートのみできます。 詳細については、次を参照してください。 [ASP.NET Core の Application Insights](/azure/application-insights/app-insights-asp-net-core)です。

## <a name="monitoring-blades"></a>ブレードの監視

監視ブレードでは、このトピックで説明した方法にエクスペリエンスをトラブルシューティング代替手段を提供します。 このブレードは、500 番台のエラーの診断に使用できます。

ASP.NET Core Extensions がインストールされていることを確認します。 拡張機能がインストールされていない場合は、手動でインストールします。

1. **開発ツール**ブレード セクションで、select、**拡張**ブレードです。
1. **ASP.NET Core Extensions**一覧に表示する必要があります。
1. 拡張機能がインストールされていない場合は、選択、**追加**ボタンをクリックします。
1. 選択、 **ASP.NET Core Extensions**一覧からです。
1. 選択**OK**法的条項に同意します。
1. 選択**OK**上、**拡張機能を追加**ブレードです。
1. 情報のポップアップ メッセージは、拡張機能が正常にインストールされている場合を示します。

Stdout のログ記録が有効でない場合は、次の手順に従います。

1. Azure ポータルで、選択、**高度なツール**ブレードに、**開発ツール**領域。 選択、**移動&rarr;**ボタンをクリックします。 Kudu コンソールは、新しいブラウザー タブまたはウィンドウで開きます。
1. ページの上部にあるナビゲーション バーを使用して開きます**デバッグ コンソール**選択**CMD**です。
1. パスのフォルダーを開いて**サイト** > **wwwroot**表示まで下にスクロールし、 *web.config*一覧の下部にあるファイルです。
1. 次に、鉛筆アイコンをクリックして、 *web.config*ファイル。
1. 設定**stdoutLogEnabled**に`true`を変更して、 **stdoutLogFile**へのパス:`\\?\%home%\LogFiles\stdout`です。
1. 選択**保存**を保存、更新された*web.config*ファイル。

診断ログの記録をアクティブ化する手順に従います。

1. Azure ポータルで、選択、**診断ログ**ブレードです。
1. 選択、**で**スイッチを**アプリケーションのログ記録 (ファイル システム)**と**エラー メッセージの詳細な**です。 選択、**保存**ブレードの上部にあるボタンをクリックします。
1. 失敗要求イベントのバッファリング (FREB) ログ記録とも呼ばれる、失敗した要求トレースを含めるには 、**で**の切り替える**失敗した要求トレース**です。 
1. 選択、**ログ ストリーム**ブレードで、すぐに 下にある、**診断ログ**ポータルのブレードです。
1. アプリへの要求を作成します。
1. ログ ストリーム データ内で、エラーの原因が示されます。

**大事な！** 標準出力ログのトラブルシューティングが完了したら無効にすることを確認します。 手順を参照して、 [ASP.NET Core モジュール stdout ログ](#aspnet-core-module-stdout-log)セクションです。

失敗した要求トレース ログ (FREB ログ) を参照してください。

1. 移動し、**診断し、問題を解決して**ブレードで、Azure ポータルでします。
1. 選択**失敗した要求トレース ログ**から、 **SUPPORT TOOLS**サイド バーの領域。

参照してください[失敗した要求トレースのセクションのトピックの Azure App Service web アプリの診断ログを有効に](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)と[Azure で Web アプリのアプリケーションのパフォーマンスに関する Faq: 失敗した要求トレースを有効にする方法ですか?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)詳細についてはします。

詳細については、次を参照してください。 [Azure App service web アプリの診断ログ記録を有効にする](/azure/app-service/web-sites-enable-diagnostic-log)です。

> [!WARNING]
> エラーを標準出力ログを無効にするは、アプリケーションまたはサーバーの障害につながります。 ログ ファイルのサイズに制限はありませんか、作成されるログ ファイルの数があります。
>
> ASP.NET Core アプリケーションの日常的なログ記録、ログ ファイルのサイズを制限し、ログを回転するログ記録ライブラリを使用します。 詳細については、次を参照してください。[サード パーティ製のログ記録プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)です。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core でのエラー処理の概要](xref:fundamentals/error-handling)
* [Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)
* [Visual Studio を使用して Azure App service web アプリをトラブルシューティングします。](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [「502 無効なゲートウェイ」と「503 サービス利用不可」では、Azure web アプリの HTTP エラーをトラブルシューティングします。](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Azure App Service で低速の web アプリのパフォーマンスに関する問題をトラブルシューティングします。](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure で Web アプリのアプリケーションのパフォーマンスに関する Faq](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Friday: Azure アプリケーション サービスの診断とトラブルシューティング エクスペリエンス (12 分のビデオ)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
