---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.NET Web 展開 - 推奨リソース |Microsoft Docs
author: rick-anderson
description: このトピックではドキュメントへのリンクを展開する方法についてのリソース (発行) ASP.NET web アプリケーションを Visual Studio 2010、Visual Web De を使用して IIS をしています.
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 6df0c9d2f38ad1d39abd62787c600ef80da8e8e0
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836222"
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web 展開 - 推奨リソース
====================
> このトピックではドキュメントへのリンクを展開する方法についてのリソース (発行) ASP.NET web アプリケーションを Visual Studio 2010、Visual Web Developer 2010、およびそれ以降のバージョンを使用して IIS をします。
> 
> 優れたブログ記事「わかっている場合[stackoverflow](http://stackoverflow.com)スレッド、または他リンクに役立つ、[電子メールの送信](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map)リンクを使用します。
> 
> > [!NOTE] 
> > 
> > 最新のリリースをインストールする場合にのみ使用可能な展開機能について説明するこれらのリソース、 [Visual Studio Web 発行の更新](https://go.microsoft.com/fwlink/?LinkID=208120)します。 一部の機能は、Visual Studio 2012 または Visual Studio 2013 でのみ使用できます。


このトピックは、次のセクションで構成されています。

- [Web プロジェクトの配置オプションを理解します。](#understanding)
- [ホスティング プロバイダーの ASP.NET アプリケーションの検索](#findinghosting)
- [Visual Studio から web アプリケーションを展開します。](#fromvs)
- [作成と web 配置パッケージをインストールした web アプリケーションの配置](#package)
- [継続的インテグレーション (CI) プロセスを使用して web アプリケーションを展開します。](#ci)
- [Web.config 変換を使用して、デプロイ時に対象の Web.config ファイルまたは app.config ファイルで設定を変更するには](#transforms)
- [Web 配置パラメーターを使用して、デプロイ時に、コピー先の web アプリケーションの設定を変更するには](#webdeployparms)
- [デプロイ時にオフラインはことを確認して、アプリケーションを作成します。](#appoffline)
- [データベースまたは変更をデプロイする web アプリケーションのデプロイの一部としてデータベース](#databasewithweb)
- [Web アプリケーションのデプロイから個別にデータベースを展開します。](#databaseseparate)
- [サービスのメンバーシップやプロファイリングなどの ASP.NET アプリケーションを使用する web アプリケーションを展開します。](#aspnetmembership)
- [展開のためのプリコンパイル](#precompiling)
- [イントラネット web アプリケーションを展開します。](#intranet)
- [すぐが自動化されていない展開の一般的なタスクを自動化します。](#automating)
- [開発者は、Web Deploy を使用して web アプリケーションをデプロイできるように、web サーバーを構成します。](#configuringservers)
- [ホスティング プロバイダーのサーバーを構成します。](#hostingprovider)
- [展開に関する問題のトラブルシューティング](#troubleshooting)
- [特定の展開の質問のヘルプ](#gettinghelp)
- [その他のリソース](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Web プロジェクトの配置オプションを理解します。

- [Visual Studio および ASP.NET の web 配置の概要](https://msdn.microsoft.com/library/dd394698.aspx)(MSDN)。
- [Windows Azure の Web サイトをデプロイする方法](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)します。 Windows Azure Websites、web プロジェクトをデプロイするためのリソースへのオプションとのリンクをについて説明します[継続的デリバリー](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (から自動[ソース管理](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) と Visual Studio を使用します。
- [Visual Studio 2012 Web 公開機能の強化](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md)(Scott Hanselman によるビデオ)。
- [VS 2010 で Web 配置の概要の Post](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi のブログ)。 以前のブログ投稿が Visual Studio 2010 のリソースの一部は、for Visual Studio 2012 関連のある情報にリンクします。


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>ホスティング プロバイダーの ASP.NET アプリケーションの検索

- [ASP.NET ホスティング](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Visual Studio から web アプリケーションを展開します。

- [Windows Azure の Web サイトをデプロイする方法](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)します。 オプションについて説明し、Windows Azure Web サイトに web プロジェクトをデプロイするためのリソースへのリンクを提供します。 Visual Studio からの展開についてのセクションが含まれています。
- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)します。 12 部構成のチュートリアル シリーズでは、SQL Server データベースを web アプリケーションをデプロイする方法を示します。 データベースの配置には、dbDacFx プロバイダーと Entity Framework Code First Migrations の両方が使用されます。 に関する情報も含まれます[Web.config ファイルの変換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)、[個々 のファイルを展開する](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles)、[コマンドライン配置](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)、および[する方法Visual Studio web カスタマイズ .pubxml ファイルを編集してパイプラインを発行](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)します。 Web フォーム、MVC、Web API など、すべての ASP.NET web プロジェクトに適用)。
- [方法: Web プロジェクトを使用して 1 回のクリックの発行 Visual Studio でのデプロイ](https://msdn.microsoft.com/library/dd465337.aspx)(Visual Studio Web 発行ウィザードの情報を参照)。
- [SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションを展開する](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)します。 これは、以前のバージョンの**Visual Studio を使用して ASP.NET Web 配置**このセクションの上部に表示します。 SQL Server Compact データベースをデプロイする方法と SQL Server Compact から SQL Server の製品版に移行する方法については主に便利なここでは。
- [.NET 多層アプリケーションを使用してストレージ テーブル、キュー、および Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure サイト)。 チュートリアル シリーズの 5 つのパートでは、MVC プロジェクトを作成し、Windows Azure のクラウド サービスにデプロイする方法を示します。


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>作成と web 配置パッケージをインストールした web アプリケーションの配置

- [方法: Visual Studio で Web 配置パッケージを作成する](https://msdn.microsoft.com/library/dd465323.aspx)(MSDN)。
- [方法: Visual Studio によって deploy.cmd ファイルを使用して展開パッケージを作成するインストール](https://msdn.microsoft.com/library/ff356104.aspx)(MSDN)。
- [Web 配置パッケージを使用して、開発ボックス上の IIS とサード パーティ製のホストを展開する](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx)(Sayed Hashimi's ブログ)。 IIS マネージャーを使用して、ローカル コンピューター上の IIS に展開パッケージをインストールし、ホストにする企業の方法では、リモート管理用 IIS マネージャーをサポートしています。
- [構築、Web 展開パッケージから Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET web サイト)。 コマンド ライン パッケージの作成およびインストール手順が含まれています。
- [パッケージの 1 回発行 Anywhere](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (Sayed Hashimi's ブログ)。 複数のサーバーに 1 つのパッケージをデプロイできるように、複数の展開先環境用の Web.config ファイルを変換するプロセスを自動化する NuGet パッケージを紹介します。 参照してください、 [PackageWeb ビデオ](https://www.youtube.com/watch?v=-LvUJFI8CzM)Sayed Hashimi でします。

次のセクションを参照してください。


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>継続的インテグレーション (CI) プロセスを使用して web アプリケーションを展開します。

- [継続的インテグレーションと継続的デリバリー (Windows Azure で現実世界のクラウド アプリの構築) します。](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) 継続的インテグレーションと継続的デリバリーを導入する電子書籍の章。
- [Windows Azure の Web サイトをデプロイする方法](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)します。 Windows Azure Web サイトに web プロジェクトをデプロイするためのリソースには、オプションおよびリンクをについて説明します。 ソース管理からのデプロイの自動化に関するセクションが含まれています。
- [エンタープライズのシナリオで Web アプリケーションを展開する](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)します。 40 の部分で構成のチュートリアル シリーズでは、Visual Studio 2010 および Team Foundation Server 2010 を使用して CI プロセスの展開を自動化する方法を示します。
- [Microsoft Build Engine: 内Using MSBuild and Team Foundation ビルド、Sayed Hashimi、William Bartholomew](http://msbuildbook.com)します。 これは、書籍では、web リソースではなく、ですが、MSBuild を継続的な統合を構成する方法を学習するため、重要なガイドでは。
- [MSBuild の拡張機能パック](https://github.com/mikefourie/MSBuildExtensionPack)します。 展開タスクが含まれています。
- [Team Foundation ビルドのカスタマイズ ガイド](https://aka.ms/vsarsolutions)します。 Team Foundation Server の設定に関する ALM Rangers によってこのドキュメントでは、web 配置について説明して、チュートリアルとビデオが含まれています。
- [CI サーバーからの変換を SlowCheetah XML](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (Sayed Hashimi's ブログ)。 Visual Studio アドインの app.config ファイルとその他の XML ファイルに変換するための SlowCheetah を使用する方法について説明します。

参照してください[デプロイ中にオフラインがアプリケーションを必ず](aspnet-web-deployment-content-map.md#appoffline)このページの後半。


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Web.config 変換を使用して、デプロイ時に対象の Web.config ファイルまたは app.config ファイルで設定を変更するには

- [Web.config ファイルの変換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)します。
- [Visual Studio を使用して Web プロジェクトの配置の Web.config 変換構文](https://msdn.microsoft.com/library/dd465326.aspx)(MSDN)。
- [Web ツール 2012.2 - web.config 変換](https://www.youtube.com/watch?v=HdPK8mxpKEI)(Sayed Hashimi による YouTube ビデオ)。 設定して、Web.config の変換をプレビューする方法を示します。
- [Web.config 変換を無効にする方法は?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN)。
- [Web.config 変換の代わりに Web 配置パラメーターを使用する必要がありますと](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN)。
- [Codeplex.com のリリース (XML ドキュメントの変換) XDT](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (.NET Web 開発とツールのブログ)。 Web.config ファイル変換エンジンのソース コードの公開を発表し、それを使用するいくつかのツールを一覧表示します。
- [Windows Azure Web サイト:アプリケーション文字列と接続文字列の動作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)(Microsoft Azure のブログ)。 代わりに Web.config 変換の移行先環境が Windows Azure Web サイトを変換する場合`appSettings`または`connectionStrings`します。


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Web 配置パラメーターを使用して、デプロイ時に、コピー先の web アプリケーションの設定を変更するには

- [方法: Web 配置パッケージで使用して Web 配置パラメーター](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN)。
- [MSDeploy:アプリ発行の設定を更新する方法は、発行プロファイルに基づく](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx)(Sayed Hashimi's ブログ)。 示しています Visual Studio に Web 配置パラメーターを統合する方法は、プロファイルを発行します。
- [Web デプロイのパラメーター化](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization)(IIS.NET web サイト)。
- [アクションのパラメーター化の展開を web](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi のブログ)。
- [Web デプロイのパラメーター化とします。Web.config 変換](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html)(Vishal Joshi のブログ)。
- [Windows Azure Web サイト:アプリケーション文字列と接続文字列の動作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)(Microsoft Azure のブログ)。 移行先環境が Windows Azure Web サイトと、パラメーター化する場合の代わりに Web 配置パラメーター`appSettings`または`connectionStrings`します。


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>デプロイ時にオフラインはことを確認して、アプリケーションを作成します。

- [Visual Studio を使用して ASP.NET Web の展開:コードの更新を展開する](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)します。 セクションを参照して**デプロイ時にアプリケーションをオフラインです。**
- [オフラインにすること、アプリケーションの発行する前に](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing)(IIS.net サイト)。 アプリの処理を自動化する Web Deploy 3.0 に組み込まれている機能について説明します\_offline.htm ファイル。 カスタム アプリでこの機能は機能しません\_offline.htm ファイル。
- [発行時に、web アプリをオフラインを実行する](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)(Sayed Hashimi's ブログ)。 カスタム アプリを使用してプロセスを自動化する方法\_offline.htm ファイル。
- [Web アプリをオフラインと usechecksum の更新の発行](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx)(Microsoft の Web 開発のブログ)。 アプリの使用を自動化するためのもう 1 つのオプション\_offline.htm ファイル。
- [Web デプロイ 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net サイト)。 カスタム アプリの Web デプロイ 3.5 の新機能\_offline.htm ファイル。


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>データベースまたは変更をデプロイする web アプリケーションのデプロイの一部としてデータベース

- [Visual Studio でデータベースの配置を構成する](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)(MSDN)。 Web プロジェクトを含むデータベースを展開するためのオプションの概要。
- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)します。 12 部構成のチュートリアル シリーズでは、dbDacFx プロバイダーと Entity Framework Code First Migrations を使用して、データベースの配置を示しています。
- [方法: Web 展開 1 回のクリックを使用してプロジェクトが Visual Studio で発行](https://msdn.microsoft.com/library/dd465337.aspx)(MSDN)。
- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC 5 アプリを Windows Azure の Web サイトにデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 構築し、単一の SQL Server を使用するアプリケーションを展開する長いチュートリアルは、両方のメンバーシップとアプリケーション データをデータベースします。
- [SQL Server compact の Visual Studio を使用して ASP.NET Web アプリケーションを展開する](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)します。 12 部構成のチュートリアル シリーズでは、SQL Server Compact データベースをデプロイする方法と SQL Server Compact から SQL Server の製品版に移行する方法を示します。

作成し、web デプロイ パッケージをインストールしてこのページの前の継続的インテグレーション (CI) プロセスを使用して web アプリケーションを配置する web アプリケーションの展開にも参照してください。


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Web アプリケーションのデプロイから個別にデータベースを展開します。

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN)。
- [SQL Server データベース プロジェクトのデータを含む](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)(SQL Server Data Tools チーム ブログ)。 データベースをデプロイするときに、スキーマとデータの両方をデプロイする方法。
- [Windows Azure にデータベースをデプロイする方法](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate)(Microsoft Azure サイト)
- [Windows Azure SQL Database (旧 SQL Azure) へのデータベース移行](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx)(MSDN)。
- [SSDT を使用して SQL Azure にデータベースを移行する](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx)(SQL Server Data Tools チーム ブログ)。
- [Windows Azure にデータを中心としたアプリケーションを移行する](https://msdn.microsoft.com/library/jj156154.aspx)(MSDN)。
- [Windows Azure SQL データベースに SQL Server データベースを移行する](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx)(MSDN)。


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>サービスのメンバーシップやプロファイリングなどの ASP.NET アプリケーションを使用する web アプリケーションを展開します。

- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC 5 アプリを Windows Azure の Web サイトにデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 構築し、単一の SQL Server を使用するアプリケーションを展開する長いチュートリアルは、両方のメンバーシップとアプリケーション データをデータベースします。
- [ASP.NET Identity](https://asp.net/identity/)します。 ASP.NET Identity のリソース。
- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)します。 12 部構成のチュートリアル シリーズでは、ASP.NET メンバーシップ データベースをデプロイする方法を説明します。
- [アプリケーション サービスを使用する web サイトを構成する](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md)します。 Web サイトのプロジェクトも web アプリケーション プロジェクトに関連します。
- [ユーザーと実稼働 web サイト ロール](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md)します。 Web サイトのプロジェクトも web アプリケーション プロジェクトに関連します。


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>展開のためのプリコンパイル

- [ASP.NET Web アプリケーション プロジェクト プリコンパイルの概要](https://msdn.microsoft.com/library/aa983464.aspx)(MSDN)。
- [パッケージ/発行 タブで、プロジェクト プロパティ](https://msdn.microsoft.com/library/dd410108.aspx)(MSDN)。
- [[詳細設定] ダイアログ ボックスをプリコンパイル](https://msdn.microsoft.com/library/hh475319.aspx)(MSDN)。


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>イントラネット web アプリケーションを展開します。

- [Visual Studio 2013 で ASP.NET を使用したオンプレミス組織認証オプション (ADFS) を使用して](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)(Vittorio bertocci ブログです。)。
- [ASP.NET MVC を使用してイントラネット サイトを作成する方法](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)(MSDN)。 Visual Studio 2010 では、以前のチュートリアル書き込まれるには、イントラネット プロジェクト テンプレートが Visual Studio 2013 で導入された大きな変更は反映されません。


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>すぐが自動化されていない展開の一般的なタスクを自動化します。

- [Visual Studio を使用して ASP.NET Web の展開:余分なファイルを展開する](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)します。
- [Web の発行フォルダーのアクセス許可を設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)(Sayed Hashimi's ブログ)。
- [Web プロジェクトのパッケージのレジストリ設定を含めるようにターゲット ファイルを拡張する方法](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx)(Web 開発ツールのブログ)。
- [拡張 XML (Web.config) 変換](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx)(Sayed Hashimi's ブログ)。 カスタム XDT 変換を作成する方法を示します。
- [Web 配置ツール (MSDeploy) カスタム プロバイダーを 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (Sayed Hashimi's ブログ)。 Web Deploy のカスタム プロバイダーを作成する方法を示します。
- [パッケージ化し、COM コンポーネントを展開する方法](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx)(Web 開発ツールのブログ)。
- [.NET アセンブリのパッケージ方法](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx)(Web 開発ツールのブログ)。 アセンブリを GAC に展開する方法。
- [すべての初期化、Windows の Azure VM の Web サーバーの IIS、Web デプロイ、およびその他のものをスクリプト](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff)(Tugberk Ugurlu のブログ)。


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>開発者は、Web Deploy を使用して web アプリケーションをデプロイできるように、web サーバーを構成します。

- [管理者と管理者以外の展開のインストールと構成の Web Deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net サイト)。


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>ホスティング プロバイダーのサーバーを構成します。

- [Microsoft ASP.NET 4 ホスティング展開ガイド](https://go.microsoft.com/fwlink/?LinkId=191365)(Microsoft ダウンロード センター)。
- [プロファイル XML ファイルを生成](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file)(IIS.net サイト)。


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>展開に関する問題のトラブルシューティング

- [Visual Studio での Windows Azure Web サイトのトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)(Microsoft Azure サイト)。
- [Visual Studio を使用して ASP.NET Web の展開:トラブルシューティング](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md)します。
- [Web を使用した一般的な問題のトラブルシューティングのデプロイ](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy)します。
- [エラー コードのデプロイを web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net サイト)。
- [Web 配置の FAQ for Visual Studio および ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN)。
- [IIS と ASP.NET 開発サーバーの間の相違点をコア](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)します。
- [開発と運用の間の一般的な構成違い](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md)します。
- [中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)(4 Guys Rolla サイトから)。


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>特定の展開の質問のヘルプ

- [ASP.NET の構成と展開フォーラム](https://forums.asp.net/26.aspx/1?Configuration and Deployment)します。
- [StackOverflow.com](http://www.StackOverflow.com)します。


<a id="additional"></a>


## <a name="additional-resources"></a>その他のリソース

このセクションでは、Visual Studio および IIS の展開ツールを使用する方法について詳しく学ぶのために役立つその他のリソースへのリンクを提供します。

次のブログは、頻繁に、Visual Studio の web 展開に関する情報を含みます。

- [Microsoft のブログでの web 開発ツール](https://blogs.msdn.com/b/webdevtools/)します。
- [Sayed Hashimi's ブログ](http://www.sedodream.com/)します。

次のリソースは、Web Deploy、Visual Studio を使用して web アプリケーション プロジェクトの配置タスクを実行する IIS フレームワークに関するドキュメントを提供します。 Web 配置について質問することができます、 [Web 配置ツールのフォーラム](https://go.microsoft.com/fwlink/?LinkId=149411)IIS.net web サイト。

- [Web の概要展開](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy)します。
- [インストールと構成の Web デプロイ](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy)します。
- [Web を自動化するための PowerShell スクリプトは、セットアップをデプロイ](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup)します。
- [Web 配置ツール](https://go.microsoft.com/fwlink/?LinkId=151481)します。 TechNet サイトの Web 配置ドキュメントの目次ノードの最上位レベルのテーブル。 年のページが更新されていない TechNet の大半には役立つ情報が含まれています。
- [Microsoft.Web.Deployment Namespace](https://go.microsoft.com/fwlink/?LinkId=148630)します。 バージョン 1.0 から API のドキュメントが更新されていません。
- [Microsoft Web 展開チームのブログ](https://blogs.iis.net/msdeploy/default.aspx)します。
- [IIS.net web サイト タブを発行](https://www.iis.net/learn/publish)します。
