---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: 展開からのファイルとフォルダーの除外 |Microsoft Docs
author: jrjlee
description: このトピックでは、方法できますから除外するファイルとフォルダー web 配置パッケージをビルドして、web アプリケーション プロジェクトをパッケージ化について説明します。
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33eecfea86842a93eac705e7823f1eba9519670e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816679"
---
<a name="excluding-files-and-folders-from-deployment"></a>展開からのファイルとフォルダーの除外
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、方法できますから除外するファイルとフォルダー web 配置パッケージをビルドして、web アプリケーション プロジェクトをパッケージ化について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="overview"></a>概要

Visual Studio 2010 で web アプリケーション プロジェクトをビルドするときに Web 発行パイプライン (WPP) では、配置可能な web のパッケージにコンパイルされた web アプリケーションをパッケージ化して、このビルド プロセスを拡張することができます。 この web パッケージを展開するリモートの IIS web サーバーにインターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用したり、IIS マネージャーで手動で web パッケージをインポートできます。 このパッケージ化プロセスが説明した[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)します。

Web の内容を取得制御する方法をパッケージ化しますか。 基になるプロジェクト ファイルを使って Visual Studio でプロジェクトの設定は、多くのシナリオのための十分な制御を提供します。 しかし、場合によっては、特定の送信先の環境に web パッケージの内容を調整する場合があります。 たとえば、テスト環境にアプリケーションのデプロイがステージング環境または運用環境にアプリケーションをデプロイするときに、フォルダーを除外する場合にログ ファイル用のフォルダーを追加する可能性があります。 このトピックでは、これを行う方法を示します。

## <a name="what-gets-included-by-default"></a>既定で内容を取得しますか。

Visual Studio で web アプリケーション プロジェクトのプロパティを構成するときに、**配置する項目**ボックスの一覧、**パッケージ化/発行 Web**ページでは、web 配置に含める対象を指定できます。パッケージです。 既定では、この設定は**このアプリケーションの実行に必要なファイルのみ**します。

![](excluding-files-and-folders-from-deployment/_static/image1.png)

選択すると**このアプリケーションの実行に必要なファイルのみ**、WPP は web のパッケージにファイルを追加する必要がありますを決定しようとしています。 バインディングには、以下の項目が含まれます。

- プロジェクトのすべてのビルドを出力します。
- すべてのファイルがビルド アクションでマークされた**コンテンツ**します。

> [!NOTE]
> このファイルに含めるファイルを決定するロジックが含まれています。   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>特定のファイルとフォルダーを除外

場合によっては、細かい制御ファイルとフォルダーを展開する必要があります。 も先に除外するファイルは、次の時間、さんと除外は、すべての展開先環境に適用されます、単に設定することができる場合、**ビルド アクション**それぞれのファイルに**None**します。

**特定のファイルを配置から除外するには**

1. **ソリューション エクスプ ローラー**ウィンドウでは、ファイルを右クリックし、順にクリックします**プロパティ**します。
2. **プロパティ**ウィンドウで、**ビルド アクション**行で、 **None**します。

ただし、このアプローチは常に便利です。 たとえばをどのファイルを変更することがあり、フォルダーは、移行先環境に従って、Visual Studio の外部から含まれています。 たとえば、連絡先マネージャーのサンプル ソリューションでは、ContactManager.Mvc プロジェクトの内容を見てをみましょう。

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- 内部のフォルダーには、開発者は作成、ドロップ、および開発用のローカルのデータベースの設定を使用していくつかの SQL スクリプトが含まれています。 このフォルダーではありませんは、ステージング環境または運用環境に展開してください。
- Scripts フォルダーには、いくつかの JavaScript ファイルが含まれています。 これらのファイルの多くは、デバッグをサポートまたは Visual Studio の IntelliSense を提供する純粋に含まれています。 これらのファイルの一部はステージング環境または運用環境に展開されませんする必要があります。 ただし、トラブルシューティングを容易に開発テスト環境に展開したい場合があります。

特定のファイルとフォルダーを除外するプロジェクト ファイルを操作することが簡単な方法は。 WPP にはという名前のアイテムの一覧を作成することにより、ファイルやフォルダーを除外するためのメカニズムが含まれています**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**します。 このメカニズムを拡張するには、これらのリストに、独自の項目を追加します。 これを行うには、これらの大まかな手順を完了する必要があります。

1. という名前のカスタム プロジェクト ファイルを作成する *[プロジェクト名].wpp.targets*プロジェクト ファイルと同じフォルダーにします。

    > [!NOTE]
    > *. Wpp.targets*ファイルは、web アプリケーション プロジェクト ファイルと同じフォルダーに移動する必要がある&#x2014;など*ContactManager.Mvc.csproj*&#x2014;任意のカスタムと同じフォルダー内ではなくプロジェクト ファイルを使用して、ビルドおよび展開プロセスを制御します。
2. *. Wpp.targets*ファイルを追加、 **ItemGroup**要素。
3. **ItemGroup**要素を追加**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**特定のファイルと必要なフォルダーを除外する項目。

これは、この基本的な構造 *. wpp.targets*ファイル。


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


各項目がという名前の項目メタデータ要素が含まれることに注意してください**FromTarget**します。 これは、任意の値であり、ビルド プロセスには影響しません特定のファイルまたはフォルダーが省略された理由を示すためにだけ機能する場合、ユーザーが、ビルド ログをレビューします。

## <a name="excluding-files-and-folders-from-a-web-package"></a>Web パッケージからのファイルとフォルダーの除外

次の手順は、追加する方法を示します、 *. wpp.targets*ファイル、web アプリケーション プロジェクトとプロジェクトをビルドするときに、web パッケージから特定のファイルとフォルダーを除外するファイルを使用する方法。

**Web 配置パッケージからファイルとフォルダーを除外するには**

1. Visual Studio 2010 でソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、web アプリケーションのプロジェクト ノードを右クリックして (たとえば、 **ContactManager.Mvc**)、 をポイント**追加**をクリックして**新しい項目の**します。
3. **新しい項目の追加**ダイアログ ボックスで、 **XML ファイル**テンプレート。
4. **名前**ボックスに「 *[プロジェクト名] * * *.wpp.targets** (たとえば、 **ContactManager.Mvc.wpp.targets**) 順にクリックします**追加**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > プロジェクトのルート ノードに新しい項目を追加する場合は、プロジェクト ファイルと同じフォルダーにファイルが作成されます。 これは、Windows エクスプ ローラーでフォルダーを開くことで確認できます。
5. ファイルで、追加、**プロジェクト**要素と**ItemGroup**要素。

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Web パッケージからフォルダーを除外する場合は、追加、 **ExcludeFromPackageFolders**要素を**ItemGroup**要素。

   1. **Include**属性で、除外するフォルダーのセミコロン区切りの一覧を指定します。
   2. **FromTarget**メタデータ要素名などののフォルダーが除外されている理由を示す意味のある値を指定、 *. wpp.targets*ファイル。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Web パッケージからファイルを除外する場合は、追加、 **ExcludeFromPackageFiles**要素を**ItemGroup**要素。

   1. **Include**属性で、除外するファイルのセミコロン区切りの一覧を指定します。
   2. **FromTarget**メタデータ要素を意味のある理由のファイル除外されているなどの名前を示す値を指定、 *. wpp.targets*ファイル。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[プロジェクト名].wpp.targets*ファイルのこのようになります。

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. 保存して閉じます、 *[プロジェクト名].wpp.targets*ファイル。

次回ビルドとパッケージ、web アプリケーション プロジェクト、WPP を自動的に検出、 *. wpp.targets*ファイル。 そのファイルと指定したフォルダーは、web パッケージに含まれません。

## <a name="conclusion"></a>まとめ

このトピックでは、カスタムを作成して、web パッケージをビルドするときに、特定のファイルとフォルダーを除外する方法を説明しました。 *。 wpp.targets* web アプリケーション プロジェクト ファイルと同じフォルダー内のファイル。

## <a name="further-reading"></a>関連項目

カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。 パッケージ化と展開プロセスの詳細については、次を参照してください[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)、 [Web パッケージ展開の構成パラメーター](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)、および[。Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)します。

> [!div class="step-by-step"]
> [前へ](deploying-membership-databases-to-enterprise-environments.md)
> [次へ](taking-web-applications-offline-with-web-deploy.md)
