---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Web パッケージ展開のパラメーターの構成 |Microsoft Docs
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) web アプリケーションの名前、接続文字列、およびサービスのエンドポイントなどのパラメーター値を設定する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386550"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Web パッケージ展開のパラメーターの構成
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、リモートの IIS web サーバーに web パッケージを展開するときに、インターネット インフォメーション サービス (IIS) web アプリケーションの名前、接続文字列、およびサービスのエンドポイントなどのパラメーター値を設定する方法について説明します。


Web アプリケーション プロジェクト、ビルドおよびパッケージ化プロセスを作成する場合に、3 つのキー ファイルが生成されます。

- A *[プロジェクト名] .zip*ファイル。 これは、web アプリケーション プロジェクトの web 配置パッケージです。 このパッケージには、すべてのアセンブリ、ファイル、データベースのスクリプト、およびリモートの IIS web サーバーに web アプリケーションを再作成に必要なリソースが含まれています。
- A *[プロジェクト名].deploy.cmd*ファイル。 これには、一連リモートの IIS web サーバーに web 配置パッケージを公開する Web Deploy (MSDeploy.exe) のパラメーター化されたコマンドにはが含まれています。
- A *[プロジェクト名]。SetParameters.xml*ファイル。 これは、MSDeploy.exe コマンドにパラメーター値のセットを提供します。 このファイル内の値を更新し、Web Deploy をパラメーターとして渡す、コマンド ライン web パッケージを展開するときにできます。

> [!NOTE]
> ビルドとパッケージ化プロセスの詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。


*SetParameters.xml*ファイルは、web アプリケーションのプロジェクト ファイルと、プロジェクト内のすべての構成ファイルから動的に生成します。 ビルドおよび Web 発行パイプライン (WPP)、プロジェクトをパッケージ化する場合、変換先の IIS web アプリケーションのように、デプロイ環境とデータベース接続文字列の間を変更する可能性のある変数の多くが自動的に検出します。 これらの値が自動的に web デプロイ パッケージでパラメーター化されたに追加された、 *SetParameters.xml*ファイル。 接続文字列を追加する場合など、 *web.config*ファイル、web アプリケーション プロジェクトでビルド プロセスは、この変更を検出およびエントリを追加、 *SetParameters.xml*ファイルそれに応じて。

多くの場合、この自動パラメーター化は、十分になります。 ただし、ユーザーがアプリケーションやサービスのエンドポイント Url のように、展開環境の間には、その他の設定を変更する必要がある、展開パッケージでこれらの値をパラメーター化し、を対応するエントリを追加するWPPを指示する*SetParameters.xml*ファイル。 次のセクションでは、これを行う方法について説明します。

### <a name="automatic-parameterization"></a>自動パラメーター化

ビルドし、web アプリケーションをパッケージ化すると、WPP をこれらの操作は自動的にパラメーター化します。

- 変換先の IIS の web アプリケーションのパスと名前。
- すべての接続文字列、 *web.config*ファイル。
- 追加するすべてのデータベースの接続文字列、**パッケージ化/発行 SQL**プロジェクトのプロパティ ページ タブ。

たとえば、ビルドおよびパッケージ化した場合、 [Contact Manager](the-contact-manager-solution.md) WPP、何らかの方法でパラメーター化のプロセスに触れることがなく、サンプル ソリューションでは、これが生成*ContactManager.Mvc.SetParameters.xml*ファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


この場合、次のようになります。

- **IIS Web アプリケーション名**パラメーターは、web アプリケーションを展開する IIS のパス。 既定値を取得、**パッケージ化/発行 Web**プロジェクトのプロパティ ページでページ。
- **ApplicationServices-web.config Connection String**から生成されたパラメーターを**connectionStrings/追加**内の要素、 *web.config*ファイル。 これは、メンバーシップ データベースに接続するため、アプリケーションが使用する接続文字列を表します。 ここでされますを指定した値に代入され、デプロイされている*web.config*ファイル。 配置前から既定値を取得*web.config*ファイル。

WPP には、これらのプロパティを生成する展開パッケージにもパラメーター化します。 展開パッケージをインストールするときに、これらのプロパティの値を指定できます。 」の説明に従って手動で IIS マネージャーから、パッケージをインストールするかどうか[Web パッケージを手動でインストール](manually-installing-web-packages.md)、インストール ウィザードでは、すべてのパラメーターの値を指定するように求められます。 使用してリモート パッケージをインストールする場合、 *. deploy.cmd* 」の説明に従って、ファイル[Web パッケージを展開する](deploying-web-packages.md)、次のようになります Web Deploy *SetParameters.xml*ファイルをパラメーター値を指定します。 内の値を編集することができます、 *SetParameters.xml*ファイルを手動で、または自動のビルドと展開プロセスの一環としてファイルをカスタマイズすることができます。 このプロセスは、このトピックで後で詳しく説明します。

### <a name="custom-parameterization"></a>カスタムのパラメーター化

複雑な展開シナリオで多くの場合、プロジェクトを展開する前に、追加のプロパティをパラメーター化するします。 一般に、任意のプロパティと変換先の環境間で変化する設定がパラメーター化する必要があります。 これらを含めることができます。

- サービスのエンドポイント、 *web.config*ファイル。
- アプリケーションの設定、 *web.config*ファイル。
- その他の宣言型プロパティには、指定するユーザーが求められます。

これらのプロパティをパラメーター化する最も簡単な方法が追加するには、 *'parameters.xml'* ファイルを web アプリケーション プロジェクトのルート フォルダー。 たとえば、連絡先マネージャー ソリューション、ContactManager.Mvc プロジェクトが含まれています、 *'parameters.xml'* ルート フォルダー内のファイル。

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

このファイルを開く場合、1 つが含まれている表示されます**パラメーター**エントリ。 エントリを見つけてで ContactService Windows Communication Foundation (WCF) サービスのエンドポイント URL をパラメーター化、XML Path Language (XPath) クエリを使用して、 *web.config*ファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


展開パッケージにエンドポイントの URL をパラメーター化だけでなく、WPP も追加に対応するエントリ、 *SetParameters.xml*展開パッケージに生成されるファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


展開パッケージを手動でインストールする場合は、IIS マネージャー、サービス エンドポイントのアドレスを自動的にパラメーター化されたプロパティの横を求められます。 実行して、展開パッケージをインストールする場合、 *. deploy.cmd*ファイルを編集できます、 *SetParameters.xml*の値と共に、サービスのエンドポイント アドレスの値を指定するファイル、自動的にパラメーター化されたプロパティです。

作成する方法の詳細について、 *'parameters.xml'* ファイルを参照してください[方法: デプロイの設定時に、パッケージの構成を使用してパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)します。 という名前のプロシージャ**Web.config ファイルの設定のデプロイ パラメーターを使用する**手順について説明します。

## <a name="modifying-the-setparametersxml-file"></a>SetParameters.xml ファイルの変更

Web アプリケーションのパッケージを手動で展開する予定のかどうか&#x2014;実行するか、 *. deploy.cmd*ファイルまたはコマンドラインから MSDeploy.exe を実行して&#x2014;停止を手動で編集するには何もない、 *SetParameters.xml*展開する前にファイル。 ただし、エンタープライズ規模のソリューションで作業する場合は、大規模で自動化されたビルドと展開プロセスの一部として web アプリケーション パッケージを配置する必要があります。 このシナリオで Microsoft Build Engine (MSBuild) を変更する必要があります、 *SetParameters.xml*ファイル。 MSBuild を使用してこれを行う**XmlPoke**タスク。

[Contact Manager サンプル ソリューション](the-contact-manager-solution.md)このプロセスを示しています。 後のコード例は、この例に関連する詳細のみを表示する編集されています。

> [!NOTE]
> サンプル ソリューションでは、および一般的なカスタムのプロジェクト ファイルの概要で、プロジェクト ファイルのモデルのより広範な概要については、次を参照してください。[プロジェクト ファイルを理解する](understanding-the-project-file.md)と[ビルド プロセスを理解する](understanding-the-build-process.md)します。


最初に、関心のあるパラメーターの値は、環境固有のプロジェクト ファイル内のプロパティとして定義されます (たとえば、 *Env Dev.proj*)。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> 独自のサーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。


次に、 *Publish.proj*ファイルは、これらのプロパティをインポートします。 ため、各*SetParameters.xml*ファイルが関連付けられている、 *. deploy.cmd*をそれぞれ呼び出すプロジェクト ファイルが最終的にするファイル、および *. deploy.cmd*ファイル、プロジェクトファイルには、MSBuild が作成されます*項目*各 *. deploy.cmd*として関心のあるプロパティを定義ファイルを開き*項目メタデータ*します。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


この場合、次のようになります。

- **ParametersXml**メタデータ値の位置を示す、 *SetParameters.xml*ファイル。
- **IisWebAppName**値は、web アプリケーションを配置する IIS パス。
- **MembershipDBConnectionString**値は、メンバーシップ データベースの接続文字列と**MembershipDBConnectionName**値は、**名前**属性内の対応するパラメーターの*SetParameters.xml*ファイル。
- **ServiceEndpointValue**値は、移行先サーバー上の WCF サービスのエンドポイント アドレスと**ServiceEndpointParamName**値に対応するパラメーターの name 属性は、*SetParameters.xml*ファイル。

最後に、 *Publish.proj*ファイル、 **PublishWebPackages**対象には、 **XmlPoke**でこれらの値を変更するタスク、 *SetParameters.xml*ファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


各わかります**XmlPoke**タスクでは、4 つの属性の値を指定します。

- **XmlInputPath**属性を変更するファイルの検索場所をタスクに指示します。
- **クエリ**属性が変更する XML ノードを識別する XPath クエリ。
- **値**属性が選択されている XML ノードに挿入する新しい値。
- **条件**属性は、条件のロックを実行する必要がありますまたはタスクが実行されません。 Null または空の値を挿入しようとしていないことが条件では、このような場合は、により、 *SetParameters.xml*ファイル。

## <a name="conclusion"></a>まとめ

このトピックには、役割が説明されている、 *SetParameters.xml*ファイルし、web アプリケーション プロジェクトをビルドするときの生成方法について説明します。 追加することで追加の設定をパラメーター化する方法について説明しました、 *'parameters.xml'* ファイルをプロジェクト。 変更する方法を説明し、 *SetParameters.xml*ファイルを使用して、拡大、自動化されたビルド プロセスの一部として、 **XmlPoke**プロジェクト ファイルで作業します。

次のトピックでは、 [Web パッケージを展開する](deploying-web-packages.md)、いずれかを実行して web パッケージを展開する方法について説明、 *. deploy.cmd*ファイルまたは直接 MSDeploy.exe を使用してコマンドします。 どちらの場合も、指定することができます、 *SetParameters.xml*デプロイ パラメーターとしてファイル。

## <a name="further-reading"></a>関連項目

Web パッケージを作成する方法については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。 実際に web パッケージをデプロイする方法のガイダンスについては、次を参照してください。 [Web パッケージを展開する](deploying-web-packages.md)します。 作成する方法についてステップ バイ ステップ チュートリアルについては、 *'parameters.xml'* ファイルを参照してください[方法: デプロイの設定時に、パッケージの構成を使用してパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)します。

Web デプロイのパラメーター化の一般的なについては、次を参照してください。[アクションで Web デプロイ Parameterization](https://go.microsoft.com/?linkid=9805119) (ブログの投稿)。

> [!div class="step-by-step"]
> [前へ](building-and-packaging-web-application-projects.md)
> [次へ](deploying-web-packages.md)
