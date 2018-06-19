---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: ビルドおよび Web アプリケーション プロジェクトをパッケージ化 |Microsoft ドキュメント
author: jrjlee
description: リモート サーバー環境に web アプリケーション プロジェクトを配置する場合は、最初のタスクが、プロジェクトをビルドし、web 配置 packa を生成しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: d630e1776607bd0bd7c61e1f0f7234ef58c7533b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892310"
---
<a name="building-and-packaging-web-application-projects"></a>ビルドおよび Web アプリケーション プロジェクトをパッケージ化
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> リモート サーバー環境に web アプリケーション プロジェクトを配置する場合は、最初のタスクは、プロジェクトをビルドし、web 配置パッケージの生成です。 このトピックでは、web アプリケーション プロジェクトのビルド プロセスのしくみについて説明します。 具体的には、それについて説明します。
> 
> - どの Web 発行パイプライン (WPP) は、展開の機能が含まれてに、ビルド プロセスを拡張します。
> - どのように (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールには、展開パッケージに、web アプリケーションがオンにします。
> - ビルドおよびパッケージ化プロセスの動作と、どのようなファイルが作成されます。


Visual Studio 2010 web アプリケーション プロジェクトのビルドおよび配置プロセスでは、WPP です。 WPP は、MSBuild の機能を拡張して、Web 配置と統合できるようにする Microsoft Build Engine (MSBuild) ターゲットのセットを提供します。 Visual Studio 内で、web アプリケーション プロジェクトに、プロパティ ページでこの拡張機能を確認できます。 **パッケージ化/発行 Web**  ページと連携して、**パッケージ化/発行 SQL**  ページで、web アプリケーション プロジェクトは展開のパッケージ化ビルド プロセスが完了する方法を構成できます。

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP はどのように機能するのですか。

C# プロジェクト ファイルを見てを行うかどうか、ベースの web アプリケーション プロジェクトでは、2 つの .targets ファイルをインポートすることが確認できます。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


最初の**インポート**ステートメントは、すべての Visual c# プロジェクトに共通します。 このファイル*Microsoft.CSharp.targets*、ターゲットおよび Visual c# に固有のタスクが含まれています。 たとえば、c# コンパイラ (**Csc**) ここではタスクが呼び出されます。 *Microsoft.CSharp.targets*ファイルのインポートではさらに、 *Microsoft.Common.targets*ファイル。 同様に、すべてのプロジェクトに共通のターゲットを定義**ビルド**、**リビルド**、**実行**、**コンパイル**、および**クリーン**. 2 番目**インポート**ステートメントは、web アプリケーション プロジェクトに固有です。 *ある Microsoft.WebApplication.targets*ファイルのインポートではさらに、 *Microsoft.Web.Publishing.targets*ファイル。 *Microsoft.Web.Publishing.targets*本質的にファイル*は*WPP です。 同様に、ターゲットを定義する**パッケージ**と**MSDeployPublish**、Web デプロイのさまざまな展開タスクの完了を呼び出すことです。

連絡先のマネージャーのサンプル ソリューションで、これらの追加のターゲットの使用方法について理解を開き、 *Publish.proj*ファイルし、を見て、 **BuildProjects**ターゲットです。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


このターゲットを使用して、 **MSBuild**をさまざまなプロジェクトをビルドするタスク。 通知、 **DeployOnBuild**と**DeployTarget**プロパティ。

- **DeployOnBuild = true**プロパティは本来「するビルドが正常に完了したときに、追加のターゲットを実行します」という意味。
- **DeployTarget**プロパティを識別するときに実行するターゲットの名前、 **DeployOnBuild**プロパティと等しい**true**です。 この場合、MSBuild を実行することを指定している、**パッケージ**プロジェクトのビルド後のターゲットです。

**パッケージ**でターゲットが定義されている、 *Microsoft.Web.Publishing.targets*ファイル。 基本的には、このターゲットは、web アプリケーション プロジェクトのビルド出力を受け取るし、IIS web サーバーにパブリッシュできる web 配置パッケージに変換します。 します。

> [!NOTE]
> プロジェクト ファイルを表示する (たとえば、 <em>ContactManager.Mvc.csproj</em>) Visual Studio 2010 では、まずソリューションからプロジェクトをアンロードします。 <strong>ソリューション エクスプ ローラー</strong>ウィンドウで、プロジェクト ノードを右クリックし、をクリックして<strong>プロジェクトのアンロード</strong>です。 もう一度、プロジェクト ノードを右クリックし、をクリックして<strong>編集</strong><em>[プロジェクト ファイル]</em>)。 プロジェクト ファイルは、その生の XML 形式で開かれます。 完了したら、プロジェクトの再読み込みしてください。  
> 詳細については、MSBuild のターゲット、タスク、および<strong>インポート</strong>ステートメントを参照してください[プロジェクト ファイルを理解する](understanding-the-project-file.md)です。 プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。


## <a name="what-is-a-web-deployment-package"></a>Web 配置パッケージは何ですか。

ビルドし、Visual Studio 2010 を使用するか、直接 MSBuild を使用して、web アプリケーション プロジェクトを展開すると、最終的な結果は、通常、 *web 配置パッケージ*です。 Web 配置パッケージは、.zip ファイルです。 IIS を含むすべてのものと、web アプリケーションを再作成するために必要な Web 配置を含みます。

- コンテンツ、リソース ファイル、構成ファイル、JavaScript およびカスケード スタイル シート (CSS) のリソース、およびなどを含む web アプリケーションのコンパイル済みの出力。
- Web アプリケーション プロジェクトおよびすべてのアセンブリは、ソリューション内のプロジェクトを参照します。
- Web アプリケーションを配置する任意のデータベースを生成する SQL スクリプト。

Web 配置パッケージが生成されると、さまざまな方法で IIS web サーバーに発行することができます。 たとえば、Web デプロイのリモート エージェント サービスまたは Web 配置ハンドラーが、ターゲット web サーバーで、対象としてリモートで展開することができます。 または IIS マネージャーを使用して、ターゲット web サーバー上のパッケージを手動でインポートすることができます。 展開に対するこれらの方法の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)です。

## <a name="how-does-the-build-process-work"></a>ビルド処理のしくみ

これは、ビルド、および web アプリケーション プロジェクトをパッケージ化するときの動作を示しています。

![](building-and-packaging-web-application-projects/_static/image2.png)

ビルド プロセスがという名前のファイルを生成する web アプリケーション プロジェクトをビルドするときに *[プロジェクト名]。SourceManifest.xml*です。 プロジェクト ファイルとビルド出力と一緒にこの*です。SourceManifest.xml*ファイル指示 Web Deploy web 展開パッケージに含める必要があること。 これらの入力を使用して、という名前の web 配置パッケージを生成 Web Deploy *[プロジェクト名] .zip*です。

Web 配置パッケージと共にビルド プロセスには、パッケージを使用するのに役立つ 2 つのファイルが生成されます。

- *. Deploy.cmd*ファイルには、リモートの IIS web サーバーに web 配置パッケージを発行する Web Deploy (MSDeploy.exe) のパラメーター化コマンドのセットが含まれています。 実行している、 *. deploy.cmd*ファイルに適切なパラメーターは、通常、早く簡単および代替が容易になります、MSDeploy.exe を手動で構築するコマンド示します自分でします。
- *SetParameters.xml*ファイルは、MSDeploy.exe コマンドにパラメーター値のセットを提供します。 これらの値をサービス エンドポイントの値は、パッケージを展開して、接続文字列で定義されて、IIS の web アプリケーションの名前などのプロパティを含める、 *web.config*ファイル、および任意の展開プロパティプロジェクト プロパティ ページで定義されている値。

*SetParameters.xml*ファイルが配置プロセスを管理するキー。 このファイルは、web アプリケーション プロジェクトの内容に応じて動的に生成されます。 たとえばへの接続文字列を追加する場合、 *web.config*ファイル、ビルド プロセスは自動的に接続文字列を検出、展開を同様に、パラメーター化および内のエントリを作成、 *SetParameters.xml*展開プロセスの一部として、接続文字列を変更することを許可するファイル。 次のトピックでは、 [Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)さらに詳しくは、このファイルの役割について説明、および変更には、することができます、ビルドおよび配置中にさまざまな方法について説明します。

> [!NOTE]
> Visual Studio 2010 で、WPP はサポートされませんパッケージ化する前に web アプリケーションのページをプリコンパイルします。 次のバージョンの Visual Studio と WPP パッケージング オプションとして、web アプリケーションをプリコンパイルする機能が含まれます。


## <a name="conclusion"></a>まとめ

このトピックでは、Visual Studio 2010 で web アプリケーション プロジェクトのビルドとパッケージ化プロセスの概要が用意されています。 WPP を使用する MSBuild から、Web Deploy コマンドを呼び出す方法が説明されているし、ビルドとパッケージ化プロセスの動作に説明します。

Web 配置パッケージを作成したら、次の手順では、展開を開始します。 詳細については、次を参照してください。 [Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)と[Web パッケージを展開する](deploying-web-packages.md)です。

## <a name="further-reading"></a>関連項目

このチュートリアルでは、次のトピック[Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)と[Web パッケージを展開する](deploying-web-packages.md)、作成した web パッケージを使用する方法についてガイダンスを提供します。 このシリーズの最終的なチュートリアル[高度なエンタープライズ Web 展開](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)、カスタマイズおよびパッケージ化プロセスのトラブルシューティングを行う方法について説明します。

プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。

> [!div class="step-by-step"]
> [前へ](understanding-the-build-process.md)
> [次へ](configuring-parameters-for-web-package-deployment.md)
