---
title: "Azure App Service での ASP.NET Core のホスト"
author: guardrex
description: "役に立つリソースへのリンクを使用して Azure App Service で ASP.NET Core アプリをホストする方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: cefbc27c8091a2ed1441663e3779d67aae2c64dd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Azure App Service での ASP.NET Core のホスト

[Azure App Service](https://azure.microsoft.com/services/app-service/) は ASP.NET Core を含む Web アプリをホストするための [Microsoft クラウド コンピューティング プラットフォーム サービス](https://azure.microsoft.com/)です。

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a>役に立つリソース

Azure の「[Web Apps のドキュメント](/azure/app-service/)」は、Azure アプリのドキュメント、チュートリアル、サンプル、ハウツー ガイド、その他のリソースのホームです。 ASP.NET Core アプリのホスティングに関連する次の 2 つのチュートリアルは特に重要です。

[クイック スタート: Azure に ASP.NET Core Web アプリを作成する](/azure/app-service/app-service-web-get-started-dotnet)  
Visual Studio を使用して ASP.NET Core Web アプリを作成し、Windows の Azure App Service に配置します。

[クイック スタート: App Service on Linux での .NET Core Web アプリの作成](/azure/app-service/containers/quickstart-dotnetcore)  
コマンド ラインを使用して ASP.NET Core Web アプリを作成し、Linux の Azure App Service に配置します。

ASP.NET Core のドキュメントでは、次の記事を参照できます。

[Visual Studio による Azure への公開](xref:tutorials/publish-to-azure-webapp-using-vs)  
Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。

[CLI ツールによる Azure への公開](xref:tutorials/publish-to-azure-webapp-using-cli)  
Git コマンド ライン クライアントを使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。

[Visual Studio と Git による Azure への継続的配置](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。

[VSTS による Azure への継続的配置](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
ASP.NET Core アプリ用に CI ビルドを設定し、Azure App Service に継続的配置リリースを作成します。

[Azure Web アプリのサンドボックス](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Azure アプリのプラットフォームで適用される Azure App Service ランタイム実行の制限事項について説明します。

## <a name="application-configuration"></a>アプリケーション構成

ASP.NET Core 2.0 以降、[Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) の次の 3 つのパッケージから Azure App Service に配置されたアプリの自動ログ記録機能が提供されます。

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) は [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) を使用して Azure App Service と ASP.NET Core の Light-Up 統合を提供します。 追加されるログ記録機能は `Microsoft.AspNetCore.AzureAppServicesIntegration` パッケージによって提供されます。
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) は [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) を実行して、`Microsoft.Extensions.Logging.AzureAppServices` パッケージに Azure App Service 診断ログ記録プロバイダーを追加します。
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) はロガー実装を提供することで、Azure App Service 診断ログとログ ストリーミング機能をサポートします。

## <a name="monitoring-and-logging"></a>監視およびログ記録

監視、ログ記録、トラブルシューティングに関する情報は、次の記事を参照してください。

[Azure App Service でアプリを監視する方法](/azure/app-service/web-sites-monitor)  
アプリと App Service プランに関するクォータとメトリックを確認する方法を説明します。

[Azure App Service の Web アプリの診断ログの有効化](/azure/app-service/web-sites-enable-diagnostic-log)  
HTTP 状態コード、失敗した要求、Web サーバー アクティビティの診断ログを有効にしてアクセスする方法を説明します。

[ASP.NET Core でのエラー処理の概要](xref:fundamentals/error-handling)  
ASP.NET Core アプリでエラーを処理するための一般的な手法について理解します。

[Azure App Service での ASP.NET Core のトラブルシューティング](xref:host-and-deploy/azure-apps/troubleshoot)  
ASP.NET Core アプリを使用した Azure App Service の配置に関する問題を診断する方法を説明します。

[Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス](xref:host-and-deploy/azure-iis-errors-reference)  
Azure App Service/IIS によってホストされるアプリの一般的な配置の構成エラーのトラブルシューティングに関するアドバイスを参照してください。

## <a name="data-protection-key-ring-and-deployment-slots"></a>データ保護キー リングと展開スロット

[データ保護キー](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)は *%HOME%\ASP.NET\DataProtection-Keys* フォルダーに保存されます。 このフォルダーはネットワーク ストレージにバックアップされ、アプリをホストしているすべてのマシンで同期されています。 保存中のキーは保護されていません。 このフォルダーから、単一の展開スロットのアプリのすべてのインスタンスにキー リングが提供されます。 ステージングや運用などの別の展開スロットでは、キー リングが共有されません。

展開スロットを切り替えると、データ保護を使用しているすべてのシステムが前のスロット内のキー リングを使用して格納されたデータを復号化できなくなります。 ASP.NET Cookie ミドルウェアは、その Cookie の保護にデータ保護を使用します。 これにより、ユーザーが標準の ASP.NET Cookie ミドルウェアを使用するアプリからサインアウトします。 スロットに依存しないキー リング ソリューションの場合、次のような外部キー リング プロバイダーを使用します。

* Azure Blob Storage
* Azure Key Vault
* SQL ストア
* Redis Cache

詳細については、「[キー記憶域プロバイダー](xref:security/data-protection/implementation/key-storage-providers)」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* [Web Apps の概要 (5 分間の概要ビデオ)](/azure/app-service/app-service-web-overview)
* [Azure App Service: .NET アプリのホスティングに最適な場所 (55 分間の概要ビデオ)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service の診断とトラブルシューティング (12 分間のビデオ)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure App Service の診断の概要](/azure/app-service/app-service-diagnostics)

Windows Server の Azure App Service では[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) が使用されます。 次のトピックは、基になる IIS テクノロジと関連しています。

* [IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index)
* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [IIS モジュールと ASP.NET Core の使用](xref:host-and-deploy/iis/modules)
* [Microsoft TechNet ライブラリ: Windows Server](/windows-server/windows-server-versions)
