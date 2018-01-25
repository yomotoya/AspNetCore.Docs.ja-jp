---
uid: whitepapers/aspnet-web-deployment-content-map
title: "ASP.NET Web 展開 - 推奨リソース |Microsoft ドキュメント"
author: rick-anderson
description: "このトピックでは、ドキュメントへのリンクを展開する方法に関するリソース (発行) ASP.NET web アプリケーションを Visual Studio 2010、Visual Web De を使用して IIS をしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web 展開 - 推奨リソース
====================
> このトピックでは、ドキュメントへのリンクを展開する方法に関するリソース (発行) ASP.NET web アプリケーションは、IIS を Visual Studio 2010、Visual Web Developer 2010、およびそれ以降のバージョンを使用します。
> 
> Post、優れたブログがわかっている場合[stackoverflow](http://stackoverflow.com)スレッド、またはその他のリンクが、役に立つを[電子メールの送信](mailto:aspnetue@microsoft.com?subject=Deployment Content Map)リンクを使用します。
> 
> > [!NOTE] 
> > 
> > 最新リリースをインストールする場合にのみ使用可能な展開機能について説明するこれらのリソースの多く、 [Visual Studio Web 発行 Update](https://go.microsoft.com/fwlink/?LinkID=208120)です。 一部の機能は、Visual Studio 2012 または Visual Studio 2013 でのみ使用できます。


このトピックは、次のセクションで構成されています。

- [Web プロジェクトの配置オプションを理解します。](#understanding)
- [ホスティング プロバイダー、ASP.NET アプリケーションの検索](#findinghosting)
- [Visual Studio から web アプリケーションを配置します。](#fromvs)
- [作成および web 配置パッケージをインストールした web アプリケーションの配置](#package)
- [継続的インテグレーション (CI) プロセスを使用して web アプリケーションを配置します。](#ci)
- [Web.config 変換を使用して、配置時に対象の Web.config ファイルまたは app.config ファイルでの設定を変更するには](#transforms)
- [コピー先の web アプリケーションの展開時に設定を変更するパラメーターの Web Deploy を使用してください。](#webdeployparms)
- [配置時にオフラインはことを確認して、アプリケーションを作成します。](#appoffline)
- [データベースまたは変更を配置する web アプリケーションの配置の一部としてデータベース](#databasewithweb)
- [Web アプリケーションの展開から個別にデータベースを展開します。](#databaseseparate)
- [メンバーシップやプロファイリングなどサービス ASP.NET アプリケーションを使用する web アプリケーションを展開します。](#aspnetmembership)
- [展開のプリコンパイル](#precompiling)
- [イントラネット web アプリケーションを展開します。](#intranet)
- [すぐが自動化されていない一般的な展開タスクを自動化します。](#automating)
- [開発者は、Web Deploy を使用して web アプリケーションを展開できるように、web サーバーを構成します。](#configuringservers)
- [ホスティング プロバイダー用のサーバーの構成](#hostingprovider)
- [展開に関する問題のトラブルシューティング](#troubleshooting)
- [特定の展開の問題に関するヘルプを表示](#gettinghelp)
- [その他のリソース](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Web プロジェクトの配置オプションを理解します。

- [Visual Studio と ASP.NET の web 配置の概要](https://msdn.microsoft.com/library/dd394698.aspx)(MSDN)。
- [Windows Azure Web サイトを展開する方法](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)です。 Web プロジェクトを Windows Azure の Web サイトを展開するためのリソースへのオプションとのリンクをについて説明します[した継続的配信](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)(から自動[ソース コントロール](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) および Visual Studio を使用します。
- [Visual Studio 2012 Web の発行機能強化](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md)(Scott Hanselman によるビデオ)。
- [VS 2010 で Web 配置の概要 Post](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi のブログ)。 古いブログの投稿が Visual Studio 2010 リソースの一部の Visual Studio 2012 用関連のある情報にリンクします。


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>ホスティング プロバイダー、ASP.NET アプリケーションの検索

- [ASP.NET ホスト](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Visual Studio から web アプリケーションを配置します。

- [Windows Azure Web サイトを展開する方法](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)です。 オプションについて説明し、web プロジェクトを Windows Azure Web サイトを展開するためのリソースへのリンクを提供します。 Visual Studio からの展開に関するセクションが含まれます。
- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)です。 12 の部分から成るチュートリアルのシリーズでは、SQL Server データベースと web アプリケーションを展開する方法を示します。 データベースの展開は、dbDacFx プロバイダーと Entity Framework Code First Migrations を使用します。 に関する情報も含まれます[Web.config ファイルの変換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)、[個々 のファイルを展開する](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles)、[コマンドライン配置](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)、および[する方法Visual Studio web をカスタマイズ .pubxml ファイルを編集してパイプラインの発行](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)です。 Web フォーム、MVC、Web API など、すべての ASP.NET web プロジェクトに適用)。
- [方法: Web プロジェクトを使用して 1 回のクリックの発行 Visual Studio での展開](https://msdn.microsoft.com/library/dd465337.aspx)(Visual Studio Web 発行ウィザードの情報を参照)。
- [SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションを配置する](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)です。 これは、以前のバージョンの**Visual Studio を使用して ASP.NET Web 配置**このセクションの上部に表示します。 主に便利です今すぐ SQL Server Compact データベースを展開する方法と SQL Server Compact から SQL Server のフル ライセンス版に移行する方法についてはします。
- [.NET 多層アプリケーションを使用してストレージ テーブル、キュー、および Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure サイト)。 5 部構成のチュートリアル シリーズでは、MVC プロジェクトを作成し、Windows Azure クラウド サービスに展開する方法を示します。


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>作成および web 配置パッケージをインストールした web アプリケーションの配置

- [方法: Visual Studio での Web 配置パッケージの作成](https://msdn.microsoft.com/library/dd465323.aspx)(MSDN)。
- [方法: Visual Studio によって deploy.cmd ファイルを使用して、展開パッケージが作成されたインストール](https://msdn.microsoft.com/library/ff356104.aspx)(MSDN)。
- [Web Deploy パッケージを使用して開発ボックス上の iis とサード パーティ製のホストに展開する](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx)(Sayed Hashimi ブログ)。 IIS マネージャーを使用して、ローカル コンピューター上の IIS に展開パッケージをインストールし、ホストにする企業の方法は、IIS マネージャーをリモート管理をサポートします。
- [構築、Web 配置パッケージから Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET web サイト)。 コマンドラインのパッケージの作成とインストールのための手順を説明します。
- [パッケージの 1 回発行 Anywhere](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (Sayed Hashimi ブログ)。 1 つのパッケージを展開するには複数のサーバーにできるように複数の展開先環境用の Web.config ファイルを変換するプロセスを自動化する NuGet パッケージを紹介します。 参照、 [PackageWeb ビデオ](https://www.youtube.com/watch?v=-LvUJFI8CzM)Sayed Hashimi でします。

次のセクションを参照してください。


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>継続的インテグレーション (CI) プロセスを使用して web アプリケーションを配置します。

- [継続的インテグレーションと継続的な配信 (Windows Azure と実際のクラウド アプリのビルド)。](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) 継続的インテグレーションと継続的な配信を導入する電子書籍章します。
- [Windows Azure Web サイトを展開する方法](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)です。 Windows Azure Web サイトを web プロジェクトを展開するためのリソースには、オプションおよびリンクをについて説明します。 ソース管理からデプロイを自動化することに関するセクションが含まれます。
- [エンタープライズのシナリオで Web アプリケーションを展開する](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。 40 の部分から成るチュートリアルのシリーズでは、Visual Studio 2010 および Team Foundation Server 2010 を使用して CI プロセスで展開を自動化する方法を示します。
- [Microsoft Build Engine 内: MSBuild および Sayed Hashimi して William Bartholomew の Team Foundation ビルドを使用して](http://msbuildbook.com)です。 これは、web リソースではなく、本が不可欠のガイドでは、MSBuild を継続的な統合シナリオを構成する方法を学習します。
- [MSBuild の拡張機能パック](https://github.com/mikefourie/MSBuildExtensionPack)です。 展開タスクが含まれます。
- [Team Foundation ビルドのカスタマイズ ガイドライン](https://aka.ms/vsarsolutions)です。 Team Foundation Server を設定する方法 ALM Rangers によってドキュメントでは、web 配置の説明し、チュートリアルとビデオが含まれています。
- [CI サーバーから SlowCheetah XML を変換](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx)(Sayed Hashimi ブログ)。 Visual Studio アドインの app.config ファイルとその他の XML ファイルに変換するための SlowCheetah を使用する方法について説明します。

関連項目[ことを確認して、アプリケーションがオフラインの展開時に](aspnet-web-deployment-content-map.md#appoffline)このページの後半。


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Web.config 変換を使用して、配置時に対象の Web.config ファイルまたは app.config ファイルでの設定を変更するには

- [Web.config ファイルの変換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)です。
- [Visual Studio を使用して Web プロジェクトの配置の Web.config 変換構文](https://msdn.microsoft.com/library/dd465326.aspx)(MSDN)。
- [ツール 2012.2 - web.config 変換ファイルを web](https://www.youtube.com/watch?v=HdPK8mxpKEI) (Sayed Hashimi による YouTube ビデオ)。 設定し、Web.config の変換をプレビューする方法を示します。
- [Web.config 変換を無効にする方法は?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN)。
- [Web.config 変換の代わりに Web 配置パラメーターを使用する必要がありますと](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN)。
- [Codeplex.com のリリース (XML ドキュメントの変換) XDT](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (.NET Web 開発ツールとツールのブログ)。 Web.config ファイルの変換エンジンのソース コードの可用性をアナウンスして、それを使用するいくつかのツールを示しています。
- [Windows Azure Web サイト: アプリケーションの文字列と接続文字列作業](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)(Microsoft Azure のブログ)。 代わりに Web.config を変換先の環境は Windows Azure Web サイトを変換する場合`appSettings`または`connectionStrings`です。


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>コピー先の web アプリケーションの展開時に設定を変更するパラメーターの Web Deploy を使用してください。

- [方法: Web 配置パッケージのパラメーターの展開を使用して Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN)。
- [MSDeploy: 発行プロファイルに基づいて発行時におけるアプリの設定を更新する方法](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx)(Sayed Hashimi ブログ)。 番組が Visual Studio に Web 配置パラメーターを統合方法発行プロファイルできます。
- [パラメーター化の展開を web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET web サイト)。
- [アクションのパラメーター化の展開を web](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi のブログ)。
- [Web 展開パラメーター化とします。Web.config 変換](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html)(Vishal Joshi のブログ)。
- [Windows Azure Web サイト: アプリケーションの文字列と接続文字列作業](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)(Microsoft Azure のブログ)。 送信先の環境は Windows Azure Web サイトと、パラメーター化する場合、代わりに Web 展開パラメーター`appSettings`または`connectionStrings`です。


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>配置時にオフラインはことを確認して、アプリケーションを作成します。

- [Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開する](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)です。 セクションを参照して**オフライン展開中にアプリケーションを実行します。**
- [アプリケーションをオフラインにパブリッシュする前に](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing)(IIS.net サイト)。 アプリの処理を自動化する Web Deploy 3.0 に組み込まれている機能についての説明\_offline.htm ファイル。 この機能は、カスタム アプリケーションでは機能しません\_offline.htm ファイル。
- [発行時にオフライン web アプリを実行する](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)(Sayed Hashimi ブログ)。 カスタム アプリを使用するプロセスを自動化する方法\_offline.htm ファイル。
- [オフライン アプリと usechecksum の更新プログラムの発行を web](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (Microsoft の Web 開発のブログ)。 アプリの使用を自動化するための別のオプション\_offline.htm ファイル。
- [Web 展開 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net サイト)。 新しい展開 3.5 で Web アプリの機能をカスタム\_offline.htm ファイル。


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>データベースまたは変更を配置する web アプリケーションの配置の一部としてデータベース

- [Visual Studio でデータベースの配置を構成する](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)(MSDN)。 Web プロジェクトでデータベースを展開するためのオプションの概要です。
- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)です。 12 の部分から成るチュートリアル シリーズ、dbDacFx プロバイダーと Entity Framework Code First Migrations を使用してデータベースの配置を示します。
- [方法: Web 配置を 1 回のクリックを使用してプロジェクトが Visual Studio で発行](https://msdn.microsoft.com/library/dd465337.aspx)(MSDN)。
- [Windows Azure Web サイトにメンバーシップ、OAuth、および SQL データベースでのセキュリティで保護された ASP.NET MVC 5 アプリを展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 ビルドされ、単一の SQL Server を使用するアプリケーションを展開する方法についてのチュートリアルを長いデータベースの両方のメンバーシップとアプリケーションのデータです。
- [SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションを配置する](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)です。 12 の部分から成るチュートリアルのシリーズでは、SQL Server Compact データベースを展開する方法と SQL Server Compact から SQL Server のフル ライセンス版に移行する方法を示します。

によって作成および web 配置パッケージをインストールし、このページの継続的インテグレーション (CI) プロセスを使用して web アプリケーションを配置する web アプリケーションの配置も参照してください。


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Web アプリケーションの展開から個別にデータベースを展開します。

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN)。
- [SQL Server データベース プロジェクトのデータを含む](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)(SQL Server Data Tools チームのブログ)。 データベースを展開するときに、スキーマとデータの両方を展開する方法です。
- [Windows Azure にデータベースを展開する方法](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate)(Microsoft Azure サイト)
- [Windows Azure SQL データベース (以前の SQL Azure) へのデータベース移行](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx)(MSDN)。
- [SSDT を使用して SQL Azure にデータベースを移行する](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx)(SQL Server Data Tools チームのブログ)。
- [Windows Azure に移行するデータ セントリックなアプリケーション](https://msdn.microsoft.com/library/jj156154.aspx)(MSDN)。
- [Windows Azure SQL データベースに SQL Server データベースを移行する](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx)(MSDN)。


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>メンバーシップやプロファイリングなどサービス ASP.NET アプリケーションを使用する web アプリケーションを展開します。

- [Windows Azure Web サイトにメンバーシップ、OAuth、および SQL データベースでのセキュリティで保護された ASP.NET MVC 5 アプリを展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 ビルドされ、単一の SQL Server を使用するアプリケーションを展開する方法についてのチュートリアルを長いデータベースの両方のメンバーシップとアプリケーションのデータです。
- [ASP.NET Identity](https://asp.net/identity/)です。 ASP.NET Id のリソース。
- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)です。 12 の部分から成るチュートリアルのシリーズでは、ASP.NET メンバーシップ データベースを配置する方法を示します。
- [アプリケーション サービスを使用する web サイトを構成する](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md)です。 Web サイトのプロジェクトも web アプリケーション プロジェクトに関連します。
- [ユーザーと、実稼働 web サイト ロール](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md)です。 Web サイトのプロジェクトも web アプリケーション プロジェクトに関連します。


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>展開のプリコンパイル

- [ASP.NET Web アプリケーション プロジェクトのプリコンパイル概要](https://msdn.microsoft.com/library/aa983464.aspx)(MSDN)。
- [[Web] タブのパッケージ化/発行、プロジェクト プロパティ](https://msdn.microsoft.com/library/dd410108.aspx)(MSDN)。
- [[詳細設定] ダイアログ ボックスをプリコンパイル](https://msdn.microsoft.com/library/hh475319.aspx)(MSDN)。


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>イントラネット web アプリケーションを展開します。

- [Visual Studio 2013 で ASP.NET を使用したオンプレミスの組織の認証オプション (ADFS) を使用して](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)(ブログ Vittorio Bertocci によって)。
- [ASP.NET MVC を使用して、イントラネット サイトを作成する方法](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)(MSDN)。 Visual Studio 2010 の場合は、以前のチュートリアル書き込まれるには、イントラネット プロジェクト テンプレートが Visual Studio 2013 で導入された大きな変更は反映されません。


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>すぐが自動化されていない一般的な展開タスクを自動化します。

- [Visual Studio を使用した ASP.NET Web 展開: 余分なファイルの配置](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)です。
- [Web の発行フォルダーのアクセス許可を設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)(Sayed Hashimi ブログ)。
- [Web プロジェクトのパッケージのレジストリ設定を含めるように、ターゲット ファイルを拡張する方法](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx)(Web 開発ツールのブログ)。
- [XML (Web.config) 変換を拡張する](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx)(Sayed Hashimi ブログ)。 カスタムの XDT トランス フォームを作成する方法を示します。
- [Web 配置ツール (MSDeploy) カスタム プロバイダーを 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (Sayed Hashimi ブログ)。 Web Deploy のカスタム プロバイダーを作成する方法を示します。
- [パッケージ化し、COM コンポーネントを展開する方法](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx)(Web 開発ツールのブログ)。
- [.NET アセンブリのパッケージ方法](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx)(Web 開発ツールのブログ)。 GAC にアセンブリを展開する方法。
- [すべての初期化、Windows Azure 仮想マシンの Web サーバーを IIS、Web デプロイおよびその他の機能をスクリプト](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff)(Tugberk Ugurlu のブログ)。


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>開発者は、Web Deploy を使用して web アプリケーションを展開できるように、web サーバーを構成します。

- [管理者と管理者以外の展開のインストールと構成の Web Deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net サイト)。


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>ホスティング プロバイダー用のサーバーの構成

- [Microsoft ASP.NET 4 ホスティング デプロイ ガイド](https://go.microsoft.com/fwlink/?LinkId=191365)(Microsoft ダウンロード センター)。
- [プロファイル XML ファイルを生成](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file)(IIS.net サイト)。


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>展開に関する問題のトラブルシューティング

- [Visual Studio での Windows Azure Web サイトのトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)(Microsoft Azure サイト)。
- [Visual Studio を使用した ASP.NET Web 展開: トラブルシューティング](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md)です。
- [Web の一般的な問題をトラブルシューティング展開](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy)です。
- [展開のエラー コードを web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net サイト)。
- [Web 配置に関する FAQ for Visual Studio と ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN)。
- [IIS と ASP.NET 開発サーバー間の相違点をコア](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)です。
- [一般的な構成の違い開発および運用](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md)です。
- [中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)(Rolla サイトから 4 Guys)。


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>特定の展開の問題に関するヘルプを表示

- [ASP.NET の構成と展開フォーラム](https://forums.asp.net/26.aspx/1?Configuration and Deployment)です。
- [StackOverflow.com](http://www.StackOverflow.com)です。


<a id="additional"></a>


## <a name="additional-resources"></a>その他のリソース

このセクションでは、Visual Studio および IIS の展開ツールを使用する方法について詳しく学ぶに便利な追加のリソースへのリンクを提供します。

しばしば、次のブログには、Visual Studio web 配置に関する情報が含まれます。

- [Microsoft ブログで web 開発ツール](https://blogs.msdn.com/b/webdevtools/)です。
- [Sayed Hashimi ブログ](http://www.sedodream.com/)です。

次のリソースは、Web Deploy、Visual Studio を使用して web アプリケーション プロジェクトの展開タスクを実行する IIS フレームワークに関するドキュメントを提供します。 Web 展開に関する質問を投稿することができます、 [Web 配置ツールのフォーラム](https://go.microsoft.com/fwlink/?LinkId=149411)IIS.net web サイトです。

- [Web の概要展開](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy)です。
- [Web の構成のインストールと展開](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy)です。
- [Web を自動化するための PowerShell スクリプトは、セットアップを展開](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup)です。
- [Web 配置ツール](https://go.microsoft.com/fwlink/?LinkId=151481)です。 TechNet サイトで Web Deploy のドキュメントのコンテンツ ノードの最上位レベルのテーブルです。 参照情報が TechNet の年のページが更新されていないのほとんどが含まれます。
- [Microsoft.Web.Deployment Namespace](https://go.microsoft.com/fwlink/?LinkId=148630)です。 API のドキュメントがバージョン 1.0 から更新されていません。
- [Microsoft Web 展開チームのブログ](https://blogs.iis.net/msdeploy/default.aspx)です。
- [IIS.net web サイト タブを発行](https://www.iis.net/learn/publish)です。
