---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: パッケージ化プロセスのトラブルシューティング |Microsoft Docs
author: jrjlee
description: このトピックでは、m EnablePackageProcessLoggingAndAssert プロパティを使用してパッケージ化プロセスの詳細情報を収集する方法について説明しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22be1ccc5a1ec52d143bedffd79264918c1a45e1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828909"
---
<a name="troubleshooting-the-packaging-process"></a>パッケージ化プロセスのトラブルシューティング
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックを使用してパッケージ化プロセスの詳細情報を収集する方法について説明します、 **EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 内のプロパティ。
> 
> 設定すると、 **EnablePackageProcessLoggingAndAssert**プロパティを**true**MSBuild は。
> 
> - ビルド ログには、パッケージ化プロセスに関する追加情報を追加します。
> - パッケージ一覧に重複するファイルが見つかった場合は、特定の条件下でエラーをたとえば、ログインします。
> - ログ ディレクトリを作成、 *ProjectName*\_Package フォルダーをパッケージ化するファイルに関する情報を記録します。
> 
> パッケージ化プロセスが失敗している場合、または web デプロイ パッケージに必要なファイルが含まれていない、できます情報を使用するこのプロセスと pinpoint のトラブルシューティングを行うことが正しくないものです。
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert**プロパティの動作を使用して、プロジェクトをビルドする場合のみ、**デバッグ**構成します。 その他の構成では、プロパティは無視されます。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>EnablePackageProcessLoggingAndAssert プロパティについてください。

[パッケージの Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)Web 発行パイプライン (WPP) が一連の MSBuild の機能を拡張し、インターネット インフォメーション サービス (IIS) Web との統合を有効にする MSBuild ターゲットを提供する方法の説明配置ツール (Web 配置)。 Web アプリケーション プロジェクトをパッケージ化するときに呼び出す WPP ターゲット。

WPP ターゲットの多くは、追加情報を記録する条件付きロジックを含めるときに、 **EnablePackageProcessLoggingAndAssert**プロパティに設定されて**true**します。 確認する場合など、**パッケージ**ターゲットを確認できます、追加のログ ディレクトリを作成し、場合に、ファイルの一覧をテキスト ファイルに書き込みます**EnablePackageProcessLoggingAndAssert**がと等しい**true**します。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> WPP ターゲットが定義されている、 *Microsoft.Web.Publishing.targets* %PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダーにファイル。 このファイルを開くし、Visual Studio 2010 または任意の XML エディターでターゲットを確認できます。 ファイルの内容を変更しないようにしてください。


## <a name="enabling-the-additional-logging"></a>追加のログ記録を有効にします。

値を指定することができます、 **EnablePackageProcessLoggingAndAssert**プロジェクトをビルドする方法に応じて、さまざまな方法でプロパティ。

値を指定するには、コマンドラインからプロジェクトをビルドする場合、 **EnablePackageProcessLoggingAndAssert**コマンドライン引数としてのプロパティ。


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


含めることができます、プロジェクトをビルドし、カスタムのプロジェクト ファイルを使用する場合、 **EnablePackageProcessLoggingAndAssert**値、**プロパティ**の属性、 **MSBuild**タスク。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


値を指定するには、プロジェクトをビルドし、Team Foundation Server (TFS) ビルド定義を使用する場合、 **EnablePackageProcessLoggingAndAssert**プロパティ、 **MSBuild 引数**行。![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> 作成して、ビルド定義の構成の詳細については、次を参照してください。[配置を作成する、ビルド定義をサポートしています](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)します。


または、ビルドのたびに、パッケージを含める場合は、設定する web アプリケーション プロジェクトのプロジェクト ファイルを変更できます、 **EnablePackageProcessLoggingAndAssert**プロパティを**true**します。 最初にプロパティを追加する必要があります**PropertyGroup** .csproj または .vbproj ファイル内の要素。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>ログ ファイルの確認

ビルドおよび web アプリケーション プロジェクトをパッケージ化する場合**EnablePackageProcessLoggingAndAssert**に設定**true**、MSBuild は、名前付きのログ追加フォルダーを作成、 *ProjectName*\_パッケージ フォルダー。 ログ フォルダーには、さまざまなファイルが含まれています。

![](troubleshooting-the-packaging-process/_static/image2.png)

表示されているファイルの一覧には、プロジェクトとビルド プロセスには、点は異なります。 ただし、これらのファイルは通常、プロセスのさまざまな段階でのパッケージ化、WPP を収集するファイルの一覧を記録する使用されます。

- *PreExcludePipelineCollectFilesPhaseFileList.txt*ファイルが指定されているファイルの除外を削除する前にパッケージ化の MSBuild を収集するファイルを示します。
- *AfterExcludeFilesFilesList.txt*の除外が指定されているファイルが削除された後に変更されたファイルの一覧が含まれます。

    > [!NOTE]
    > ファイルとフォルダーをパッケージ化プロセスから除外する方法の詳細については、次を参照してください。[展開から除外ファイルとフォルダー](excluding-files-and-folders-from-deployment.md)します。
- *AfterTransformWebConfig.txt*ファイルは、後のパッケージ化に収集されたファイルを一覧表示*Web.config*変換が実行されています。 この一覧の任意の構成に固有の*Web.config*など、ファイルを変換*Web.Debug.config*と*Web.Release.config*のファイルの一覧から除外されますパッケージ化します。 1 つの変換*Web.config*代わりに含まれます。
- *PostAutoParameterizationWebConfigConnectionStrings.txt*ファイルでは、ファイルの一覧を含む接続文字列の後、 *Web.config*ファイルがパラメーター化されています。 これは、プロセス、パッケージを展開するときに、ターゲット環境の適切な設定で、接続文字列を置換することができます。
- *Prepackage.txt*ファイルには、パッケージに含まれるファイルの完成したビルド前の一覧が含まれています。

> [!NOTE]
> 追加のログ ファイルの名前は、通常、WPP ターゲットに対応します。 これらのターゲットを調べることで確認できます、 *Microsoft.Web.Publishing.targets* %PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダーにファイル。


Web パッケージの内容が期待どおりでないこれらのファイルを確認する、便利特定プロセスにどのような点は問題が発生することができます。

## <a name="conclusion"></a>まとめ

このトピックでは、使用する方法を説明、 **EnablePackageProcessLoggingAndAssert**パッケージ化プロセスのトラブルシューティングを行う MSBuild のプロパティ。 ビルド プロセスにプロパティ値を入力するさまざまな方法を説明し、プロパティを設定するときに記録される追加情報が記載されている**true**します。

## <a name="further-reading"></a>関連項目

カスタム MSBuild プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。 WPP とパッケージ化プロセスを管理する方法の詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)します。 Web 展開パッケージから特定のファイルとフォルダーを除外する方法のガイダンスについては、次を参照してください。[展開から除外ファイルとフォルダー](excluding-files-and-folders-from-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](running-windows-powershell-scripts-from-msbuild-project-files.md)
