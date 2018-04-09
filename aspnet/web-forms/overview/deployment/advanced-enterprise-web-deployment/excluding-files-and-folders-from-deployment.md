---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: ファイルとフォルダーの展開から除外 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、方法できますを除外するファイルとフォルダー web 展開パッケージから作成および web アプリケーション プロジェクトをパッケージ化するときについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c435448bf057bbef9127d66ffda24a07729f2322
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="excluding-files-and-folders-from-deployment"></a>ファイルとフォルダーの展開から除外
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、方法できますを除外するファイルとフォルダー web 展開パッケージから作成および web アプリケーション プロジェクトをパッケージ化するときについて説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="overview"></a>概要

Visual Studio 2010 で web アプリケーション プロジェクトをビルドすると、Web 発行パイプライン (WPP) では、配置可能な web のパッケージにコンパイルされた web アプリケーションをパッケージ化してこのビルド プロセスを拡張することができます。 この web パッケージを展開するリモートの IIS web サーバーにインターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用したり、IIS マネージャーで手動で web のパッケージをインポートできます。 このパッケージで解説されて[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)です。

Web の内容を取得制御する方法をパッケージ化しますか。 基になるプロジェクト ファイルを使って Visual Studio で、プロジェクトの設定は、シナリオの多くの十分な制御を提供します。 しかし、場合によっては、特定の展開先環境は、web パッケージの内容を調整する場合があります。 たとえば、テスト環境にアプリケーションを配置するが、ステージングまたは運用環境へのアプリケーションを展開するときに、フォルダーを除外するときに、ログ ファイル用のフォルダーを含めることができます。 このトピックでは、これを行う方法を示します。

## <a name="what-gets-included-by-default"></a>既定で含まれるものを取得しますか。

Visual Studio で、web アプリケーション プロジェクトのプロパティを構成するときに、**を展開する項目**ボックスの一覧、**パッケージ化/発行 Web**ページでは、web 配置に含める対象を指定できます。パッケージです。 既定では、この設定は**このアプリケーションの実行に必要なファイルのみ**です。

![](excluding-files-and-folders-from-deployment/_static/image1.png)

選択すると**このアプリケーションの実行に必要なファイルのみ**、WPP は web のパッケージにどのファイルを追加する必要がありますを決定しようとしています。 バインディングには、以下の項目が含まれます。

- すべてのビルドのプロジェクトを出力します。
- ビルド アクションが付いているすべてのファイル**コンテンツ**です。

> [!NOTE]
> 含めるファイルを決定するロジックは、このファイルに含まれます。   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>特定のファイルとフォルダーを除外

場合によっては、ファイルとフォルダーを展開する詳細に制御を必要があります。 個の除外するファイルは時間がわかっており、排他領域はすべての展開先環境に適用されます、単に設定することができます、**ビルド アクション**それぞれのファイルに**None**です。

**特定のファイルを配置から除外するには**

1. **ソリューション エクスプ ローラー**  ウィンドウでは、ファイルを右クリックし、をクリックして**プロパティ**です。
2. **プロパティ** ウィンドウで、**ビルド アクション**行で、 **None**です。

ただし、この方法はありません常に便利です。 たとえば、ファイルを変更することも、フォルダーに従って、展開先の環境と Visual Studio の外部から付属しています。 たとえば、連絡先のマネージャーのサンプル ソリューションで見て ContactManager.Mvc プロジェクトの内容。

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- 内部のフォルダーには、開発者が作成、drop、および開発する目的でローカル データベースへの追加に使用するいくつかの SQL スクリプトが含まれています。 このフォルダーでは nothing をステージングまたは運用環境に展開されます。
- Scripts フォルダーには、いくつかの JavaScript ファイルが含まれています。 これらのファイルの多くは、デバッグをサポートまたは Visual Studio の IntelliSense を提供する純粋な含まれます。 これらのファイルの一部はステージング環境または実稼働環境に展開されません必要があります。 ただし、トラブルシューティングを容易にする開発者テスト環境に配置することがあります。

特定のファイルとフォルダーを除外するプロジェクト ファイルを操作することが簡単な方法は。 WPP にはという名前の項目リストを構築して、ファイルとフォルダーを除外するためのメカニズムが含まれています**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**です。 このメカニズムは、これらのリストに、独自の項目を追加することによって拡張できます。 これを行うには、これらの大まかな手順を完了する必要があります。

1. という名前のカスタム プロジェクト ファイルを作成する*[プロジェクト名].wpp.targets*は、プロジェクト ファイルと同じフォルダーにします。

    > [!NOTE]
    > *. Wpp.targets*ファイルは、web アプリケーション プロジェクト ファイルと同じフォルダーに移動する必要がある&#x2014;など*ContactManager.Mvc.csproj*&#x2014;任意のカスタムと同じフォルダー内ではなくプロジェクト ファイルを使用してビルドおよび配置プロセスを制御します。
2. *. Wpp.targets*ファイルに追加し、 **ItemGroup**要素。
3. **ItemGroup**要素を追加**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**特定のファイルおよび必要に応じてフォルダーを除外する項目。

これは、この基本的な構造*. wpp.targets*ファイル。


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


各項目にという名前の項目メタデータ要素が含まれているメモ**FromTarget**です。 これは、オプションの値であり、ビルド プロセスには影響しません単に特定のファイルやフォルダーを省略した理由を示すために機能している場合は、ビルド ログをレビュー誰か。

## <a name="excluding-files-and-folders-from-a-web-package"></a>Web パッケージからファイルとフォルダーを除外

次の手順は、追加する方法を示します、 *. wpp.targets*ファイルを web アプリケーション プロジェクトとプロジェクトをビルドするときに、web パッケージから特定のファイルとフォルダーを除外するファイルを使用する方法です。

**Web 配置パッケージからファイルとフォルダーを除外するには**

1. Visual Studio 2010 でソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、web アプリケーションのプロジェクト ノードを右クリックし (たとえば、 **ContactManager.Mvc**)、 をポイント**追加**をクリックして**新しい項目の**します。
3. **新しい項目の追加**ダイアログ ボックスで、 **XML ファイル**テンプレート。
4. **名前**ボックスに、入力*[プロジェクト名] * * *.wpp.targets** (たとえば、 **ContactManager.Mvc.wpp.targets**)、をクリックして**追加**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > プロジェクトのルート ノードに新しい項目を追加する場合は、プロジェクト ファイルと同じフォルダーにファイルが作成されます。 これは、Windows エクスプ ローラーでフォルダーを開くことで確認できます。
5. ファイルの追加、**プロジェクト**要素および**ItemGroup**要素。

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Web パッケージからフォルダーを除外する場合は、追加、 **ExcludeFromPackageFolders**要素を**ItemGroup**要素。

   1. **Include**属性を除外するフォルダーのリストをセミコロンで区切って指定します。
   2. **FromTarget**メタデータ要素の名のように、フォルダーが除外されている理由を示すために意味のある値を指定して、 *. wpp.targets*ファイル。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Web パッケージからファイルを除外する場合は、追加、 **ExcludeFromPackageFiles**要素を**ItemGroup**要素。

   1. **Include**属性を除外するファイルのリストをセミコロンで区切って指定します。
   2. **FromTarget**メタデータ要素理由ファイル除外されているなどの名前を示すために意味のある値を指定して、 *. wpp.targets*ファイル。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[プロジェクト名].wpp.targets*ファイルのこのようになります。

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. 保存して閉じます、 *[プロジェクト名].wpp.targets*ファイル。

次回のビルドとパッケージを web アプリケーション プロジェクト、WPP は自動的に検出、 *. wpp.targets*ファイル。 そのファイルと指定したフォルダーは、web のパッケージに含まれません。

## <a name="conclusion"></a>まとめ

このトピックには、カスタムを作成することで、web のパッケージをビルドするときに、特定のファイルとフォルダーを除外する方法が説明されている*. wpp.targets* web アプリケーション プロジェクト ファイルと同じフォルダー内のファイルです。

## <a name="further-reading"></a>関連項目

カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、展開プロセスを制御する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。 パッケージと展開プロセスの詳細については、次を参照してください[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)、 [Web パッケージの展開の構成パラメーター](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)、および[。Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)です。

> [!div class="step-by-step"]
> [前へ](deploying-membership-databases-to-enterprise-environments.md)
> [次へ](taking-web-applications-offline-with-web-deploy.md)
