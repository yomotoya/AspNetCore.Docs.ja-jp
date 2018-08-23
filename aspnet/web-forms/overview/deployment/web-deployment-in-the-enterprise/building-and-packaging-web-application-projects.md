---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: ビルドと Web アプリケーション プロジェクトをパッケージ化 |Microsoft Docs
author: jrjlee
description: Web アプリケーション プロジェクトをリモート サーバー環境にデプロイするときに、プロジェクトをビルドし、web デプロイの packa を生成するは、まず.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 406b8e6daf47196eb98700efe41e34c02d5682d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833520"
---
<a name="building-and-packaging-web-application-projects"></a>ビルドと Web アプリケーション プロジェクトをパッケージ化
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> リモート サーバー環境に web アプリケーション プロジェクトを配置するときに、最初のタスクは、プロジェクトをビルドし、web 配置パッケージを生成します。 このトピックでは、web アプリケーション プロジェクトのビルド プロセスのしくみについて説明します。 具体的には、それについて説明します。
> 
> - どの Web 発行パイプライン (WPP) は、デプロイ機能を含めるビルド プロセスを拡張します。
> - 方法 (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールには、展開パッケージへの web アプリケーションがオンにします。
> - ビルドとパッケージ化プロセスの動作と、どのようなファイルが作成されます。


Visual Studio 2010 では、web アプリケーション プロジェクトのビルドおよび配置プロセスは、WPP によってサポートされます。 WPP では、一連の MSBuild の機能を拡張し、有効にすると、Web 配置と統合される Microsoft Build Engine (MSBuild) ターゲットを提供します。 Visual Studio は、web アプリケーション プロジェクトのプロパティ ページにこの拡張機能を確認できます。 **パッケージ化/発行 Web**  ページで、と共に、**パッケージ化/発行 SQL**ページで、web アプリケーション プロジェクトがデプロイのパッケージ化されたビルド プロセスが完了する方法を構成することができます。

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP のしくみ

C# のプロジェクト ファイルについて見てするかどうか、ベースの web アプリケーション プロジェクトでは、2 つの .targets ファイルをインポートすること確認できます。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


最初の**インポート**ステートメントはすべての Visual c# プロジェクトに共通です。 このファイルは、 *Microsoft.CSharp.targets*ターゲットと Visual c# に固有のタスクが含まれています。 たとえば、C# の場合、コンパイラ (**Csc**) ここではタスクが呼び出されます。 *Microsoft.CSharp.targets*ファイルのインポートではさらに、 *Microsoft.Common.targets*ファイル。 など、すべてのプロジェクトに共通のターゲット定義**ビルド**、**リビルド**、**実行**、**コンパイル**と**クリーン**. 2 番目の**インポート**ステートメントは、web アプリケーション プロジェクトに固有です。 *Microsoft.WebApplication.targets*ファイルのインポートではさらに、 *Microsoft.Web.Publishing.targets*ファイル。 *Microsoft.Web.Publishing.targets*本質的にファイル*は*WPP します。 ターゲットとするような定義が**パッケージ**と**MSDeployPublish**、さまざまな展開タスクを完了への Web 配置を呼び出すことです。

Contact Manager のサンプル ソリューションで、これらの追加のターゲットの使用方法について理解を開き、 *Publish.proj*ファイルを開きを見て、 **BuildProjects**ターゲット。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


このターゲットを使用して、 **MSBuild**さまざまなプロジェクトをビルドするタスク。 通知、 **DeployOnBuild**と**DeployTarget**プロパティ。

- **DeployOnBuild = true** 「したいビルドが正常に完了したときに、追加のターゲットを実行します」プロパティが実質的には。
- **DeployTarget**プロパティは、ときに実行するターゲットの名前を識別、 **DeployOnBuild**プロパティは等しく**true**します。 MSBuild を実行することを指定しているこの例では、**パッケージ**プロジェクトのビルド後のターゲット。

**パッケージ**でターゲットが定義されている、 *Microsoft.Web.Publishing.targets*ファイル。 基本的には、このターゲットは、web アプリケーション プロジェクトのビルド出力を受け取るし、IIS web サーバーにパブリッシュできる web 配置パッケージに変換されます。

> [!NOTE]
> プロジェクト ファイルを表示する (たとえば、 <em>ContactManager.Mvc.csproj</em>) Visual Studio 2010 では、最初にする必要があるソリューションからプロジェクトをアンロードします。 <strong>ソリューション エクスプ ローラー</strong>ウィンドウで、プロジェクト ノードを右クリックし、順にクリックします<strong>プロジェクトのアンロード</strong>します。 もう一度、プロジェクト ノードを右クリックし、<strong>編集</strong><em>[プロジェクト ファイル]</em>)。 プロジェクト ファイルは、その生の XML 形式で開かれます。 完成したプロジェクトの再読み込みしてください。  
> MSBuild のターゲットのタスクの詳細については、<strong>インポート</strong>ステートメントを参照してください[プロジェクト ファイルを理解する](understanding-the-project-file.md)します。 プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内で、Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi、William Bartholomew、ISBN: 978-0-7356-4524-0。


## <a name="what-is-a-web-deployment-package"></a>Web デプロイ パッケージとは何ですか。

ビルドは、Visual Studio 2010 を使用して、または MSBuild を直接使用して、web アプリケーション プロジェクトをデプロイすると、最終的な結果は通常、 *web 配置パッケージ*します。 Web デプロイ パッケージとは、.zip ファイルです。 IIS を含むすべてのものと、web アプリケーションを再作成するために必要な Web 配置を含みます。

- コンテンツ、リソース ファイル、構成ファイル、JavaScript とカスケード スタイル シート (CSS) のリソース、およびなどを含む web アプリケーションのコンパイル済み出力します。
- Web アプリケーション プロジェクトおよびすべてのアセンブリは、ソリューション内のプロジェクトを参照します。
- Web アプリケーションをデプロイするすべてのデータベースを生成する SQL スクリプトです。

Web デプロイ パッケージが生成されると、さまざまな方法での IIS web サーバーに発行することができます。 たとえば、対象となる、Web デプロイのリモート エージェント サービスまたは送信先の web サーバーで Web 配置ハンドラーによってリモートで展開できるまたは IIS マネージャーを使用して手動で移行先の web サーバー上のパッケージをインポートすることができます。 展開する方法の詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)します。

## <a name="how-does-the-build-process-work"></a>ビルド プロセスのしくみ

これは、ビルドして、web アプリケーション プロジェクトをパッケージ化するときの動作を示しています。

![](building-and-packaging-web-application-projects/_static/image2.png)

Web アプリケーション プロジェクトをビルドすると、ビルド プロセスは、という名前のファイルを生成します。 *[プロジェクト名]。SourceManifest.xml*します。 プロジェクト ファイルと、ビルド出力と共にこの*します。SourceManifest.xml*ファイルは Web Deploy web デプロイ パッケージに含める必要があること。 これらの入力を使用して、という名前の web デプロイ パッケージを生成 Web Deploy *[プロジェクト名] .zip*します。

ビルド プロセスでは、web デプロイ パッケージ、パッケージを使用するに役立つ 2 つのファイルが生成されます。

- *. Deploy.cmd*ファイルには、リモートの IIS web サーバーに web 配置パッケージを公開する Web Deploy (MSDeploy.exe) のパラメーター化コマンドのセットが含まれます。 実行している、 *. deploy.cmd*ファイルに適切なパラメーターは、通常をすばやくし、容易に代替、MSDeploy.exe を手動で作成するコマンド自分でします。
- *SetParameters.xml*ファイル MSDeploy.exe コマンドにパラメーター値のセットを提供します。 これらの値は、IIS web アプリケーションをサービス エンドポイントの値は、パッケージを展開してで定義されている接続文字列の名前などのプロパティを含める、 *web.config*ファイル、および展開プロパティプロジェクトのプロパティ ページで定義されている値。

*SetParameters.xml*ファイルは展開プロセスを管理するキー。 このファイルは、web アプリケーション プロジェクトの内容に応じて動的に生成されます。 接続文字列を追加する場合など、 *web.config*ファイル、ビルド プロセスは自動的に接続文字列を検出、展開の状況に応じて、パラメーター化および内のエントリを作成、 *SetParameters.xml*展開プロセスの一環として、接続文字列を変更することを許可するファイル。 次のトピックでは、 [Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)をさらに詳しくは、このファイルの役割を説明しを変更するビルドおよび配置中にさまざまな方法について説明します。

> [!NOTE]
> Visual Studio 2010 で、WPP プリコンパイルをパッケージ化する前に web アプリケーションのページはできません。 次のバージョンの Visual Studio と、WPP は、パッケージ化オプションとして web アプリケーションをプリコンパイルする機能が含まれます。


## <a name="conclusion"></a>まとめ

このトピックでは、Visual Studio 2010 で web アプリケーション プロジェクトのビルドとパッケージ化プロセスの概要が提供されます。 WPP を使用する MSBuild から Web Deploy コマンドを呼び出す方法を説明し、ビルドとパッケージ化プロセスの動作について説明しました。

Web 配置パッケージを作成したら、次の手順は、それをデプロイします。 詳細については、これは、次を参照してください。 [Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)と[Web パッケージを展開する](deploying-web-packages.md)します。

## <a name="further-reading"></a>関連項目

このチュートリアルでは、次のトピック[Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)と[Web パッケージを展開する](deploying-web-packages.md)、作成した web パッケージを使用する方法に関するガイダンスを提供します。 このシリーズの最終チュートリアル[高度なエンタープライズ Web 展開](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)、カスタマイズをパッケージ化プロセスのトラブルシューティングを行う方法についてガイダンスを提供します。

プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内で、Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi、William Bartholomew、ISBN: 978-0-7356-4524-0。

> [!div class="step-by-step"]
> [前へ](understanding-the-build-process.md)
> [次へ](configuring-parameters-for-web-package-deployment.md)
