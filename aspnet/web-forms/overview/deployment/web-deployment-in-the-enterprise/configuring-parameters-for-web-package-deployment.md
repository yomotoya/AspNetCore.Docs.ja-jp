---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Web パッケージの展開のパラメーターの構成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) web アプリケーション名、接続文字列、およびサービス エンドポイントと同様に、パラメーターの値を設定する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7be08f1a1fb7232911a44cf64e2e784dbb95ff48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880402"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Web パッケージの展開のパラメーターの構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、リモートの IIS web サーバーに web パッケージを展開するときは、インターネット インフォメーション サービス (IIS) web アプリケーション名、接続文字列、およびサービス エンドポイントと同様に、パラメーターの値を設定する方法について説明します。


Web アプリケーション プロジェクト、ビルドおよびパッケージ化プロセスを作成する場合に、次の 3 つのキー ファイルが生成されます。

- A *[プロジェクト名] .zip*ファイル。 これは、web アプリケーション プロジェクトの web 配置パッケージです。 このパッケージには、すべてのアセンブリ、ファイル、データベース スクリプト、およびリモートの IIS web サーバーに web アプリケーションを再作成に必要なリソースが含まれています。
- A *[プロジェクト名].deploy.cmd*ファイル。 これには、リモートの IIS web サーバーに web 配置パッケージを発行する Web Deploy (MSDeploy.exe) のパラメーター化コマンドのセットが含まれています。
- A *[プロジェクト名]。SetParameters.xml*ファイル。 これは、MSDeploy.exe コマンドにパラメーター値のセットを提供します。 このファイル内の値を更新し、Web Deploy にパラメーターとして渡す、コマンド ライン web パッケージを展開するときにできます。

> [!NOTE]
> ビルドとパッケージ化処理の詳細については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。


*SetParameters.xml*ファイルは、web アプリケーション プロジェクト ファイルと、プロジェクト内のすべての構成ファイルから動的に生成します。 ビルドおよび Web 発行パイプライン (WPP)、プロジェクトをパッケージ化する場合が多くを移行先の IIS web アプリケーションと同様に、デプロイ環境と任意のデータベース接続文字列の間で変更する可能性のある変数の自動的に検出します。 これらの値が自動的に web 配置パッケージのパラメーター化されに追加された、 *SetParameters.xml*ファイル。 たとえばへの接続文字列を追加する場合、 *web.config*ファイル、web アプリケーション プロジェクトでビルド プロセスがこの変更を検出しへのエントリを追加、 *SetParameters.xml*ファイルそれに従っています。

多くの場合では、この自動パラメーター化を十分になります。 ただし、ユーザーがアプリケーションの設定またはサービス エンドポイントの Url と同様に、デプロイ環境間での他の設定を変更する必要があるへの展開パッケージでこれらの値をパラメーター化を対応するエントリを追加する、 WPPを通知*SetParameters.xml*ファイル。 以降のセクションでは、これを行う方法を説明します。

### <a name="automatic-parameterization"></a>自動パラメーター化

ビルドし、web アプリケーションをパッケージ化すると、WPP は自動的には次の作業をパラメーターします。

- 移行先の IIS の web アプリケーションのパスと名前。
- 任意の接続文字列、 *web.config*ファイル。
- すべてのデータベースに追加する接続文字列、**パッケージ化/発行 SQL**プロジェクトのプロパティ ページ タブ。

たとえば、ビルドおよびパッケージ化する場合は、[連絡先のマネージャー](the-contact-manager-solution.md) WPP 何らかの方法でパラメーター化プロセスに触れることがなくサンプル ソリューションではこれが生成*ContactManager.Mvc.SetParameters.xml*ファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


この場合、次のようになります。

- **IIS Web アプリケーション名**パラメーターは、web アプリケーションを展開する IIS のパス。 既定値はから取得、**パッケージ化/発行 Web**プロジェクトのプロパティ ページでページ。
- **Web.config ApplicationServices 接続文字列**パラメーターはから生成された、 **connectionStrings/追加**内の要素、 *web.config*ファイル。 メンバーシップ データベースに接続するアプリケーションで使用される接続文字列を表します。 ここでは、指定した値に代入され、配置された*web.config*ファイル。 配置前から既定値を取得*web.config*ファイル。

WPP には、これらのプロパティを生成する展開パッケージにもパラメーター化します。 展開パッケージをインストールするときに、これらのプロパティの値を指定できます。 」の説明に従って手動で IIS マネージャーで、パッケージをインストールする[Web パッケージを手動でインストールする](manually-installing-web-packages.md)、インストール ウィザードでは、すべてのパラメーターの値を指定するように求められます。 使用してリモート パッケージをインストールする場合、 *. deploy.cmd*ファイル、」の説明に従って[Web パッケージを展開する](deploying-web-packages.md)、次のようになります Web Deploy *SetParameters.xml*ファイルの名前をパラメーターの値を提供します。 内の値を編集することができます、 *SetParameters.xml*ファイルを手動で、または自動のビルドおよび配置プロセスの一環としてファイルをカスタマイズすることができます。 このプロセスは、このトピックで後で詳しく説明します。

### <a name="custom-parameterization"></a>カスタム パラメーター化

複雑な展開シナリオで多くの場合にしておく、プロジェクトを展開する前に、追加のプロパティをパラメーター化します。 一般に、任意のプロパティと展開先環境間で変化する設定をパラメーター化する必要があります。 これらを含めることができます。

- サービスのエンドポイント、 *web.config*ファイル。
- アプリケーション設定で、 *web.config*ファイル。
- その他の宣言型プロパティを指定するユーザーを表示します。

これらのプロパティをパラメーター化する最も簡単な方法は追加する、 *parameters.xml*ファイルを web アプリケーション プロジェクトのルート フォルダー。 たとえば、ソリューションでは、連絡先のマネージャー、ContactManager.Mvc プロジェクトが含まれています、 *parameters.xml*ルート フォルダー内のファイルです。

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

このファイルを開くと、表示されます、1 つが含まれている**パラメーター**エントリです。 エントリを探して、ContactService Windows Communication Foundation (WCF) サービスのエンドポイントの URL をパラメーター化 XML Path Language (XPath) クエリを使用して、 *web.config*ファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


展開パッケージにエンドポイントの URL をパラメーター化だけでなく、WPP も追加に対応するエントリ、 *SetParameters.xml*展開パッケージと共に生成されるファイルです。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


展開パッケージを手動でインストールする場合は、IIS マネージャーのサービス エンドポイントのアドレスと共に自動的にパラメーター化されたプロパティを求められます。 実行して、展開パッケージをインストールする場合、 *. deploy.cmd*ファイルを編集、 *SetParameters.xml*の値と共にサービス エンドポイントのアドレスの値を指定するファイル、自動的にパラメーター化されたプロパティです。

作成する方法の詳細について、 *parameters.xml*ファイルを参照してください[する方法: 展開の設定時に、パッケージの構成を使用するパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)です。 という名前のプロシージャ**Web.config ファイルの設定をデプロイのパラメーターを使用する**手順に沿って説明します。

## <a name="modifying-the-setparametersxml-file"></a>SetParameters.xml ファイルを変更します。

Web アプリケーションのパッケージを手動で展開する予定のかどうか&#x2014;実行するか、 *. deploy.cmd*ファイルまたはコマンドラインから MSDeploy.exe を実行して&#x2014;停止を手動で編集するには nothing を使用する必要がある、 *SetParameters.xml*展開する前にファイル。 ただし、エンタープライズ規模ソリューションで、使用している場合より大きな、自動化されたビルドおよび配置プロセスの一部として web アプリケーション パッケージを配置する必要があります。 このシナリオでは、Microsoft Build Engine (MSBuild) を変更する必要があります、 *SetParameters.xml*のためにファイル。 MSBuild を使用してこれを行う**XmlPoke**タスク。

[連絡先のマネージャーのサンプル ソリューション](the-contact-manager-solution.md)このプロセスを示しています。 次のコード例は、この例に関連する詳細のみを表示する編集されました。

> [!NOTE]
> プロジェクト ファイルのモデルのサンプル ソリューションと全般にカスタム プロジェクト ファイルの概要についての広範な概要については、次を参照してください。[プロジェクト ファイルを理解する](understanding-the-project-file.md)と[ビルド プロセスの理解](understanding-the-build-process.md)です。


最初に、目的のパラメーター値は、環境固有のプロジェクト ファイル内のプロパティとして定義されます (たとえば、 *Env Dev.proj*)。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。


次に、 *Publish.proj*ファイルは、これらのプロパティをインポートします。 各*SetParameters.xml*ファイルに関連付けられている、 *. deploy.cmd*ファイル、最終的にする、プロジェクト ファイルをそれぞれ呼び出す *. deploy.cmd*ファイル、プロジェクトファイルを作成、MSBuild*項目*ごと *. deploy.cmd*ファイルし、としての目的のプロパティを定義*項目メタデータ*です。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


この場合、次のようになります。

- **ParametersXml**メタデータ値の位置を示す、 *SetParameters.xml*ファイル。
- **IisWebAppName**値は、web アプリケーションを展開する、IIS パス。
- **MembershipDBConnectionString**値は、メンバーシップ データベースの接続文字列と**MembershipDBConnectionName**値は、**名前**属性対応するパラメーターの*SetParameters.xml*ファイル。
- **ServiceEndpointValue**値は、移行先サーバー上の WCF サービスのエンドポイント アドレス、および**ServiceEndpointParamName**値は、対応するパラメーター内の name 属性*SetParameters.xml*ファイル。

最後に、 *Publish.proj*ファイル、 **PublishWebPackages**対象には、 **XmlPoke**でこれらの値を変更するタスク、 *SetParameters.xml*ファイル。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


各わかります**XmlPoke**タスクは、4 つの属性の値を指定します。

- **XmlInputPath**属性を変更するファイルを検索する位置をタスクに通知します。
- **クエリ**属性は、XPath クエリを変更する XML ノードを識別します。
- **値**属性が選択されている XML ノードに挿入する新しい値。
- **条件**属性は、条件のロックを実行する必要がありますまたはタスクが実行されません。 このような場合は、条件によりに null または空の値を挿入しようとしていないこと、 *SetParameters.xml*ファイル。

## <a name="conclusion"></a>まとめ

このトピックには、役割が説明されている、 *SetParameters.xml*ファイルし、web アプリケーション プロジェクトをビルドするときの生成方法を説明します。 追加の設定を追加することによってパラメーター化できる方法について説明しましたが、 *parameters.xml*ファイルをプロジェクト。 変更する方法も説明、 *SetParameters.xml*ファイルを使用してより大規模で自動化されたビルド プロセスの一環として、 **XmlPoke**プロジェクト ファイル内のタスクです。

次のトピックでは、 [Web パッケージを展開する](deploying-web-packages.md)、実行するか web パッケージを展開する方法について説明、 *. deploy.cmd* MSDeploy.exe を使用してコマンドを直接またはします。 どちらの場合を指定できます、 *SetParameters.xml*展開パラメーター ファイル。

## <a name="further-reading"></a>関連項目

Web パッケージを作成する方法については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。 実際には、web のパッケージを配置する方法のガイダンスについては、次を参照してください。 [Web パッケージを展開する](deploying-web-packages.md)です。 作成する方法についてステップ バイ ステップ チュートリアルについては、 *parameters.xml*ファイルを参照してください[する方法: 展開の設定時に、パッケージの構成を使用するパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)です。

Web Deploy でパラメーターの一般的なについては、次を参照してください。[アクションで Web 展開パラメーター](https://go.microsoft.com/?linkid=9805119) (ブログの投稿)。

> [!div class="step-by-step"]
> [前へ](building-and-packaging-web-application-projects.md)
> [次へ](deploying-web-packages.md)
