---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: パッケージ化プロセスのトラブルシューティング |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、M に EnablePackageProcessLoggingAndAssert プロパティを使用して、パッケージ化プロセスに関する詳細情報を収集する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="troubleshooting-the-packaging-process"></a>パッケージ化プロセスのトラブルシューティング
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックを使用して、パッケージ化プロセスに関する詳細情報を収集する方法について説明します、 **EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 内のプロパティです。
> 
> 設定すると、 **EnablePackageProcessLoggingAndAssert**プロパティを**true**MSBuild は。
> 
> - ビルド ログをパッケージ化プロセスに関する追加情報を追加します。
> - パッケージ リストに重複するファイルが見つかった場合は、特定の状況では、エラーをたとえば、ログインします。
> - ログ ディレクトリを作成、 *ProjectName*\_フォルダーをパッケージ化し、パッケージ化するファイルに関する情報を記録に使用します。
> 
> パッケージ化プロセスが失敗している場合、または web 展開パッケージに必要なファイルが含まれていない、行うこともできますこの情報プロセスおよび pinpoint のトラブルシューティングで作業がうまくいかないです。
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert**プロパティは、使用して、プロジェクトをビルドする場合にのみ機能、**デバッグ**構成します。 その他の構成では、プロパティは無視されます。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>EnablePackageProcessLoggingAndAssert プロパティについてください。

[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)Web 発行パイプライン (WPP) が MSBuild の機能を拡張して、インターネット インフォメーション サービス (IIS) Web と統合できるようにする MSBuild のターゲットのセットを提供する方法の説明配置ツール (Web 配置) します。 Web アプリケーション プロジェクトをパッケージ化するときは、WPP ターゲットを呼び出しています。

WPP ターゲットの多くが追加情報をログに記録する条件付きロジックを含めるときに、 **EnablePackageProcessLoggingAndAssert**プロパティに設定されている**true**です。 たとえば、確認する場合、**パッケージ**ターゲットを確認できます、追加のログ ディレクトリを作成し、場合に、ファイルの一覧をテキスト ファイルに書き込みます**EnablePackageProcessLoggingAndAssert** と等しい**true**です。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> WPP ターゲットが定義されている、 *Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダー内のファイルです。 このファイルを開くし、Visual Studio 2010 または任意の XML エディター内のターゲットを確認できます。 ファイルの内容を変更しないようにしてください。


## <a name="enabling-the-additional-logging"></a>追加のログ記録を有効にします。

値を指定することができます、 **EnablePackageProcessLoggingAndAssert**プロジェクトをビルドする方法に応じて、さまざまな方法でプロパティです。

値を指定するには、コマンドラインからプロジェクトをビルドする場合、 **EnablePackageProcessLoggingAndAssert**コマンドライン引数のプロパティ。


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


プロジェクトをビルドするカスタム プロジェクト ファイルを使用している場合は、含める、 **EnablePackageProcessLoggingAndAssert**値で、**プロパティ**の属性、 **MSBuild**タスク。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Team Foundation Server (TFS) ビルド定義を使用して、プロジェクトを作成する場合は、値を指定することができます、 **EnablePackageProcessLoggingAndAssert**プロパティに、**の MSBuild 引数**行。![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> 作成とビルド定義の構成の詳細については、次を参照してください。[展開を作成するビルド定義をサポートしている](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)です。


または、ビルドするたびに、パッケージを含める場合は、設定を web アプリケーション プロジェクトのプロジェクト ファイルを変更できます、 **EnablePackageProcessLoggingAndAssert**プロパティを**true**です。 最初にプロパティを追加する必要があります**PropertyGroup** .csproj または .vbproj ファイル内の要素。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>ログ ファイルの確認

ビルドおよび web アプリケーション プロジェクトをパッケージ化する場合**EnablePackageProcessLoggingAndAssert** 'éý' **true**、MSBuild には、名前付きのログ追加フォルダーが作成され、 *ProjectName*\_パッケージ フォルダーです。 ログ フォルダーには、さまざまなファイルが含まれています。

![](troubleshooting-the-packaging-process/_static/image2.png)

表示されているファイルの一覧は、プロジェクトのビルド プロセスで処理によって異なります。 ただし、これらのファイルは通常のプロセスのさまざまな段階で、パッケージング、WPP を収集するファイルの一覧を記録する使用されます。

- *PreExcludePipelineCollectFilesPhaseFileList.txt*ファイルを除外対象として指定されているファイルを削除する前にパッケージ化 MSBuild を収集するファイルを一覧表示します。
- *AfterExcludeFilesFilesList.txt*除外対象として指定されているファイルが削除された後に変更されたファイルの一覧が含まれます。

    > [!NOTE]
    > パッケージ化プロセスからのファイルとフォルダーを除外する方法の詳細については、次を参照してください。[除外ファイルおよびフォルダーの展開から](excluding-files-and-folders-from-deployment.md)です。
- *AfterTransformWebConfig.txt*ファイルには、いずれかの後のパッケージ化に収集されたファイルが一覧表示*Web.config*変換が実行されています。 この一覧で、任意の構成固有*Web.config*と同様に、ファイルを変換*web.debug.config となります*と*Web.Release.config*のファイルの一覧から除外されますパッケージ化します。 1 つの変換*Web.config*代わりに含まれています。
- *PostAutoParameterizationWebConfigConnectionStrings.txt*ファイル内の接続文字列の後にファイルの一覧を含む、 *Web.config*ファイルがパラメーター化されました。 これは、パッケージを展開するときに、対象となる環境の適切な設定で、接続文字列を置き換えることができます、プロセスです。
- *Prepackage.txt*ファイルには、パッケージに含まれるファイルの完成したビルド前の一覧が含まれています。

> [!NOTE]
> 追加のログ ファイルの名前は、通常、WPP ターゲットに対応します。 これらのターゲットを確認するには確認するには、 *Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダー内のファイルです。


Web パッケージの内容が期待どおりでないこれらのファイルを確認する、便利な方法、プロセスの処理にどのような点が発生しました。 を特定することができます。

## <a name="conclusion"></a>まとめ

このトピックの説明を使用する方法、 **EnablePackageProcessLoggingAndAssert**パッケージ化処理のトラブルシューティングを MSBuild でのプロパティです。 ビルド プロセスにプロパティ値を入力するさまざまな方法が説明されているし、プロパティを設定するときに記録されている追加の情報が記載されている**true**です。

## <a name="further-reading"></a>関連項目

展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。 WPP とパッケージ化プロセスの管理方法の詳細については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)です。 Web 配置パッケージから特定のファイルとフォルダーを除外する方法については、次を参照してください。[除外ファイルおよびフォルダーの展開から](excluding-files-and-folders-from-deployment.md)です。

> [!div class="step-by-step"]
> [前へ](running-windows-powershell-scripts-from-msbuild-project-files.md)
