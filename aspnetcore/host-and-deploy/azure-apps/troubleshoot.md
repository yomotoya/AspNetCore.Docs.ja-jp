---
title: Azure App Service での ASP.NET Core 起動エラーのトラブルシューティング
author: guardrex
description: ASP.NET Core Azure App Service の配置に関する問題を診断する方法を学習します。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 05bb024f5b0d2b554cc861c250a92fd7ae23437f
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090746"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Azure App Service での ASP.NET Core のトラブルシューティング

作成者: [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

この記事では、Azure App Service の診断ツールを使って ASP.NET Core アプリの起動時の問題を診断する方法の手順を説明します。 トラブルシューティングの役に立つ他の情報については、Azure ドキュメントの「[Azure App Service 診断の概要](/azure/app-service/app-service-diagnostics)」および「[Azure App Service でアプリを監視する方法](/azure/app-service/web-sites-monitor)」をご覧ください。

## <a name="app-startup-errors"></a>アプリ起動時のエラー

**502.5 処理エラー**  
ワーカー プロセスが失敗します。 アプリは起動しません。

[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)はワーカー プロセスの開始を試みますが、開始に失敗します。 この種の問題のトラブルシューティングには、アプリケーション イベント ログを調べると役に立つことがよくあります。 ログへのアクセスについては、「[アプリケーション イベント ログ](#application-event-log)」セクションで説明します。

正しく構成されていないアプリによりワーカー プロセスが失敗する場合、"*502.5 処理エラー*" のエラー ページが返されます。

![502.5 処理エラー ページが表示されているブラウザー ウィンドウ](troubleshoot/_static/process-failure-page.png)

**500 内部サーバー エラー**  
アプリは起動しますが、エラーのためにサーバーは要求を実行できません。

このエラーは、起動時または応答の作成中に、アプリのコード内で発生します。 応答にコンテンツが含まれていないか、またはブラウザーに "*500 内部サーバー エラー*" という応答が表示される可能性があります。 通常、アプリケーション イベント ログではアプリが正常に起動したことが示されます。 サーバーから見るとそれは正しいことです。 アプリは起動しましたが、有効な応答を生成できません。 問題のトラブルシューティングを行うには、[Kudu コンソールでアプリを実行する](#run-the-app-in-the-kudu-console)か、または [ASP.NET Core モジュールの stdout ログを有効にします](#aspnet-core-module-stdout-log)。

**接続のリセット**

ヘッダー送信後にエラーが発生した場合、サーバーが **500 内部サーバー エラー**を送信するには遅すぎます。 このような状況は、応答に対する複雑なオブジェクトのシリアル化中にエラーが起きたときによく発生します。 この種のエラーは、クライアントでは "*接続リセット*" エラーとして表示されます。 この種のエラーのトラブルシューティングには、[アプリケーション ログ](xref:fundamentals/logging/index)が役に立つことがあります。

## <a name="default-startup-limits"></a>既定の起動制限

ASP.NET Core モジュールの *startupTimeLimit* は、既定では 120 秒に構成されます。 既定値のままにした場合、モジュールで処理エラーが記録されるまでに、アプリは最大で 2 分を起動にかけることができます。 モジュールの構成の詳細については、「[AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)」を参照してください。

## <a name="troubleshoot-app-startup-errors"></a>アプリの起動エラーのトラブルシューティング

### <a name="application-event-log"></a>アプリケーション イベント ログ

アプリケーション イベント ログにアクセスするには、Azure portal の **[問題の診断と解決]** ブレードを使います。

1. Azure portal の **[App Services]** ブレードで、アプリのブレードを開きます。
1. **[問題の診断と解決]** ブレードを選びます。
1. **[SELECT PROBLEM CATEGORY]\(問題カテゴリの選択\)** で、**[Web App Down]\(Web アプリのダウン\)** ボタンを選びます。
1. **[Suggested Solutions]\(推奨される解決方法\)** で、, open the pane for **[Open Application Event Logs]\(アプリケーション イベント ログを開く\)** のウィンドウを開きます。 **[Open Application Event Logs]\(アプリケーション イベント ログを開く\)** ボタンを選びます。
1. **[Source]\(ソース\)** 列で、*IIS AspNetCoreModule* によって提供された最新のエラーを調べます。

**[問題の診断と解決]** ブレードを使う代わりに、[Kudu](https://github.com/projectkudu/kudu/wiki) を使ってアプリケーション イベント ログ ファイルを直接調べることもできます。

1. **[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。 **[Go&rarr;]** ボタンを選びます。 新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。
1. ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。
1. **LogFiles** フォルダーを開きます。
1. *eventlog.xml* ファイルの横にある鉛筆アイコンを選びます。
1. ログを調べます。 ログの末尾までスクロールし、最新のイベントを確認します。

### <a name="run-the-app-in-the-kudu-console"></a>Kudu コンソールでアプリを実行します。

多くの起動時エラーでは、アプリケーション イベント ログに役に立つ情報が生成されません。 [Kudu](https://github.com/projectkudu/kudu/wiki) のリモート実行コンソールでアプリを実行すると、エラーを検出することができます。

1. **[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。 **[Go&rarr;]** ボタンを選びます。 新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。
1. ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。
1. パス **site** > **wwwroot** へのフォルダーを開きます。
1. コンソールで、アプリのアセンブリを実行することによってアプリを実行します。
   * アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)である場合、*dotnet.exe* を使ってアプリのアセンブリを実行します。 次のコマンドで、`<assembly_name>` をアプリのアセンブリの名前に置き換えます。`dotnet .\<assembly_name>.dll`
   * アプリが[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)である場合は、アプリの実行可能ファイルを実行します。 次のコマンドで、`<assembly_name>` をアプリのアセンブリの名前に置き換えます。`<assembly_name>.exe`
1. エラーを示すアプリからのコンソール出力はすべて、Kudu コンソールにパイプされます。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core モジュールの stdout ログ

ASP.NET Core モジュールの stdout には、アプリケーション イベント ログでは見つからない有用なエラー メッセージが記録されることがよくあります。 stdout ログを有効にして表示するには:

1. Azure portal で **[問題の診断と解決]** ブレードに移動します。
1. **[SELECT PROBLEM CATEGORY]\(問題カテゴリの選択\)** で、**[Web App Down]\(Web アプリのダウン\)** ボタンを選びます。
1. **[Suggested Solutions]\(推奨される解決方法\)** > **[Enable Stdout Log Redirection]\(Stdout ログのリダイレクトを有効にする\)** で、ボタンをクリックして **[Open Kudu Console to edit Web.Config]\(Kudu コンソールを開いて Web.Config を編集する\)** ボタンを選びます。
1. Kudu の**診断コンソール**で、パス **site** > **wwwroot** へのフォルダーを開きます。 下にスクロールして、一覧の最後にある *web.config* ファイルを表示します。
1. *web.config* ファイルの隣の鉛筆アイコンをクリックします。
1. **stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** パスを `\\?\%home%\LogFiles\stdout` に変更します。
1. **[保存]** を選んで、更新した *web.config* ファイルを保存します。
1. アプリに対して要求します。
1. Azure portal に戻ります。 **[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。 **[Go&rarr;]** ボタンを選びます。 新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。
1. ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。
1. **LogFiles** フォルダーを選びます。
1. **[Modified]\(変更日\)** 列を調べて、変更日が最新の stdout ログの鉛筆アイコンを選んで編集します。
1. ログ ファイルが開くと、エラーが表示されます。

**重要**。 トラブルシューティングが完了したら、stdout ログを無効にします。

1. Kudu の**診断コンソール** で、パス **site** > **wwwroot** に戻り、*web.config* ファイルを表示します。 鉛筆アイコンを選んで **web.config** ファイルを再び開きます。
1. **stdoutLogEnabled** を `false` に設定します。
1. **[保存]** を選んでファイルを保存します。

> [!WARNING]
> stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。 ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。 stdout ログは、アプリ起動時の問題のトラブルシューティングにのみ使ってください。
>
> 起動後の ASP.NET Core アプリでの一般的なログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。 詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。

## <a name="common-startup-errors"></a>起動時の一般的なエラー 

以下を参照してください。<xref:host-and-deploy/azure-iis-errors-reference> このリファレンス トピックでは、アプリの起動を妨げる一般的な問題のほとんどが説明されています。

## <a name="slow-or-hanging-app"></a>アプリの速度低下またはハング

要求に対してアプリの応答が遅いとき、またはアプリがハングするときのデバッグ ガイダンスについては、「[Azure App Service での Web アプリのパフォーマンス低下に関する問題のトラブルシューティング](/azure/app-service/app-service-web-troubleshoot-performance-degradation)」をご覧ください。

## <a name="remote-debugging"></a>リモート デバッグ

次のトピックを参照してください。

* [「Visual Studio を使用した Azure App Service のトラブルシューティング」の「Web アプリのリモート デバッグ」セクション](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure ドキュメント)
* [Visual Studio 2017 を使用して Azure 内の IIS 上の ASP.NET Core をリモート デバッグする](/visualstudio/debugger/remote-debugging-azure) (Visual Studio ドキュメント)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) は、Azure App Service でホストされているアプリからのテレメトリを提供し、エラー ログやレポート機能を備えています。 Application Insights は、アプリのログ機能が使用可能になってアプリが起動した後で発生したエラーのみをレポートできます。 詳しくは、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」をご覧ください。

## <a name="monitoring-blades"></a>監視ブレード

監視ブレードでは、このトピックでこれまでに説明した方法に代わるトラブルシューティング エクスペリエンスが提供されます。 これらのブレードは、500 番台のエラーの診断に使用できます。

ASP.NET Core 拡張機能がインストールされていることを確認してください。 拡張機能がインストールされていない場合は、手動でインストールします。

1. **[開発ツール]** ブレード セクションで、**[拡張機能]** ブレードを選びます。
1. **[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** が一覧に表示されます。
1. 拡張機能がインストールされていない場合は、**[追加]** ボタンを選びます。
1. 一覧から **[ASP.NET Core Extensions]\(ASP.NET Core 拡張機能\)** を選びます。
1. **[OK]** を選んで法的条項に同意します。
1. **[拡張機能の追加]** ブレードで **[OK]** を選びます。
1. 拡張機能が正常にインストールされると、情報ポップアップ メッセージで通知されます。

stdout ログが有効になっていない場合は、次の手順のようにします。

1. Azure portal の **[開発ツール]** 領域で **[高度なツール]** ブレードを選びます。 **[Go&rarr;]** ボタンを選びます。 新しいブラウザー タブまたはウィンドウで Kudu コンソールが開きます。
1. ページの上部にあるナビゲーション バーを使って **[デバッグ コンソール]** を開き、**[CMD]** を選びます。
1. パス **site** > **wwwroot** へのフォルダーを開き、下にスクロールして、一覧の最後にある *web.config* ファイルを表示します。
1. *web.config* ファイルの隣の鉛筆アイコンをクリックします。
1. **stdoutLogEnabled** を `true` に設定し、**stdoutLogFile** パスを `\\?\%home%\LogFiles\stdout` に変更します。
1. **[保存]** を選んで、更新した *web.config* ファイルを保存します。

続けて診断ログをアクティブにします。

1. Azure portal で **[診断ログ]** ブレードを選びます。
1. **[アプリケーション ログ (ファイル システム)]** および **[詳細なエラー メッセージ]** の **[オン]** スイッチを選びます。 ブレードの上部にある **[保存]** ボタンを選びます。
1. 失敗した要求イベントのバッファ処理 (FREB) とも呼ばれる失敗した要求のトレースを含めるには、**[失敗した要求のトレース]** で **[オン]** スイッチを選びます。 
1. ポータルで **[診断ログ]** ブレードのすぐ下にある **[ログ ストリーム]** ブレードを選びます。
1. アプリに対して要求します。
1. ログ ストリーム データ内で、エラーの原因が示されます。

**重要**。 トラブルシューティングが完了したら、stdout ログを無効にしてください。 「[ASP.NET Core モジュールの stdout ログ](#aspnet-core-module-stdout-log)」セクションの説明をご覧ください。

失敗した要求のトレース ログ (FREB ログ) を見るには:

1. Azure portal で **[問題の診断と解決]** ブレードに移動します。
1. サイド バーの **[SUPPORT TOOLS]\(サポート ツール\)** 領域で、**[Failed Request Tracing Logs]\(失敗した要求のトレース ログ\)** を選びます。

詳しくは、[「Azure App Service の Web アプリの診断ログの有効化」トピックの「失敗した要求トレース」セクション](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)および[「Azure での Web アプリのアプリケーションパフォーマンスに関するよくあるご質問」の「失敗した要求トレースをオンにするにはどうすればよいですか?」](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)をご覧ください。

詳しくは、「[Azure App Service の Web アプリの診断ログの有効化](/azure/app-service/web-sites-enable-diagnostic-log)」をご覧ください。

> [!WARNING]
> stdout ログを無効にしないと、アプリまたはサーバーで障害が発生する可能性があります。 ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。
>
> ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。 詳細については、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Azure Web Apps での "502 bad gateway" および "503 service unavailable" の HTTP エラーのトラブルシューティング](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Azure App Service での Web アプリのパフォーマンス低下に関する問題のトラブルシューティング](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure での Web アプリのアプリケーションパフォーマンスに関するよくあるご質問](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web アプリのサンドボックス (App Service ランタイムの実行の制限)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
