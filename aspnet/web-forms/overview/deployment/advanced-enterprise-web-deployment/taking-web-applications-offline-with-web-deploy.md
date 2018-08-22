---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Web アプリケーションをオフラインに Web 配置 |Microsoft Docs
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) Web 展開マネージャーを使用して自動展開の期間の web アプリケーションをオフラインする方法について説明しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 9b18a48914a74a7fea85278d0e601f6f84376b23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826496"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Web アプリケーションをオフラインに Web デプロイします。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、インターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用して自動展開の期間の web アプリケーションをオフラインする方法について説明します。 Web アプリケーションを参照するユーザーをリダイレクト、*アプリ\_offline.htm*デプロイが完了するまでのファイルします。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

多数のシナリオでは、データベースや web サービスなどの関連コンポーネントを変更するときに、web アプリケーションをオフラインにします。 通常、IIS および ASP.NET では、そのために、という名前のファイルを配置する*アプリ\_offline.htm* IIS web サイトまたは web アプリケーションのルート フォルダーにします。 *アプリ\_offline.htm*ファイルは、標準の HTML ファイルでありは、通常は、サイトはメンテナンスのため一時的に使用できないことをユーザーに忠告する単純なメッセージが含まれます。 中に、*アプリ\_offline.htm* web サイトのルート フォルダーにファイルが存在する、IIS がファイルにすべての要求を自動的にリダイレクトされます。 更新が完了したらを削除する、*アプリ\_offline.htm*ファイルと、web サイトは、通常どおり要求の処理が再開されます。

ターゲット環境に自動またはシングル ステップの展開を実行する Web Deploy を使用するときは、追加と削除を組み込むことができます、*アプリ\_offline.htm*展開プロセスにファイル。 これを行うには、これらの高度なタスクを完了する必要があります。

- Microsoft Build Engine (MSBuild) プロジェクト ファイルで使用して MSBuild ターゲットを作成、展開プロセスを制御することをコピー、*アプリ\_offline.htm*任意の展開タスクの前に、移行先サーバーにファイル開始します。
- 削除する別の MSBuild ターゲットを追加、*アプリ\_offline.htm*すべての展開タスクが完了したら、移行先サーバーからのファイル。
- Web アプリケーション プロジェクトで作成、 *. wpp.targets*によりファイル、*アプリ\_offline.htm*ファイルは、Web Deploy が呼び出されたときに、展開パッケージに追加されます。

このトピックでは、これらの手順を実行する方法を説明します。 タスクとこのトピックの「チュートリアルを少なくとも 1 つの web アプリケーション プロジェクトを含むソリューションを既に作成していて、」の説明に従って、展開プロセスを制御するカスタムのプロジェクト ファイルを使用することを想定しています[の Web 展開、。エンタープライズ](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)します。 また、使用することができます、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションのトピックで例を実行します。

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>アプリの追加\_オフライン ファイルを Web アプリケーション プロジェクト

追加する最初のタスクを完了する必要があるは、*アプリ\_オフライン*ファイルを web アプリケーション プロジェクト。

- ファイルが、開発プロセスに干渉するを防ぐために (完全にオフラインにするアプリケーションをたくない) を呼び出す必要があります、何か他にも*アプリ\_offline.htm*します。 たとえば、ファイルを名前*アプリ\_オフライン template.htm*します。
- ファイルとしてデプロイされていることを防ぐためには、ビルド アクションを設定する必要があります**None**します。

**アプリを追加する\_オフライン ファイルを web アプリケーション プロジェクト**

1. Visual Studio 2010 でソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、web アプリケーション プロジェクトを右クリックし、 をポイント**追加**、 をクリックし、**新しい項目の**します。
3. **新しい項目の追加**ダイアログ ボックスで、 **HTML ページ**します。
4. **名前**ボックスに「**アプリ\_template.htm オフライン** をクリックし、**追加**します。

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. アプリケーションが使用できないことをユーザーに通知する簡単な HTML を追加し、ファイルを保存します。 サーバー側のタグを含めないでください (たとえば、任意のタグが付いている"asp:")。 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. **ソリューション エクスプ ローラー**ウィンドウで、新しいファイルを右クリックし、順にクリックします**プロパティ**します。
7. **プロパティ**ウィンドウで、**ビルド アクション**行で、 **None**します。

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>展開して、アプリの削除\_オフライン ファイル

次の手順では、展開プロセスの開始時、移行先サーバーにファイルをコピーし、最後に削除して、デプロイ ロジックを変更します。

> [!NOTE]
> 次の手順を使用しているカスタム MSBuild プロジェクト ファイルをデプロイ プロセスを制御する」の説明に従って前提としています。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。 Visual Studio から直接デプロイする場合、は、別のアプローチを使用する必要があります。 Sayed Ibrahim Hashimi で 1 つの方法を説明する[かかる Your Web App オフライン中に公開する方法](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)します。


展開する、*アプリ\_オフライン*ファイル変換先の IIS の web サイトに MSDeploy.exe を使用して呼び出す必要があります、 [Web Deploy **contentPath**プロバイダー](https://technet.microsoft.com/library/dd569034(WS.10).aspx)します。 **ContentPath**プロバイダーは、サポートの物理ディレクトリのパスと IIS の web サイトまたはアプリケーションのパスの両方と、Visual Studio プロジェクトのフォルダーと IIS web アプリケーションの間のファイルを同期するための理想的な選択肢になります。 ファイルを展開するには、MSDeploy コマンドのようにこのする必要があります。


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


展開プロセスの最後に、接続先サイトから、ファイルを削除するには、MSDeploy コマンドのようにこのする必要があります。


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


ビルドと展開プロセスの一環としてこれらのコマンドを自動化するには、カスタム MSBuild プロジェクト ファイルに統合する必要があります。 次の手順では、これを行う方法について説明します。

**展開し、アプリを削除する\_オフライン ファイル**

1. Visual Studio 2010 で、デプロイ プロセスを制御する MSBuild プロジェクト ファイルを開きます。 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションでは、これは、 *Publish.proj*ファイル。
2. ルートに**プロジェクト**要素を新規作成**PropertyGroup**の変数を格納する要素、*アプリ\_オフライン*展開。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **ソース ルート**でプロパティが他の場所で定義されている、 *Publish.proj*ファイル。 現在のパスを基準とソース コンテンツのルート フォルダーの場所を示します&#x2014;の位置を基準、 *Publish.proj*ファイル。
4. **ContentPath**デプロイする前に、ソース ファイルへの絶対パスを取得する必要があるためプロバイダーは相対ファイル パスを受け入れません。 使用することができます、 [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx)これを行うタスクです。
5. 新しい追加**ターゲット**という名前の要素**GetAppOfflineAbsolutePath**します。 このターゲットを使用して、 **ConvertToAbsolutePath**への絶対パスを取得するタスク、*アプリ\_テンプレート オフライン*プロジェクト フォルダー内のファイル。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. このターゲットへの相対パスを受け取り、*アプリ\_テンプレート オフライン*プロジェクト フォルダー内のファイルに新しいプロパティを絶対ファイル パスとして保存します。 **BeforeTargets**属性は、前に実行するには、このターゲットにすることを指定します、 **DeployAppOffline**ターゲットで、次の手順で作成します。
7. という名前の新しいターゲットを追加**DeployAppOffline**します。 このターゲット内でデプロイする MSDeploy.exe コマンドを呼び出す、*アプリ\_オフライン*ファイル変換先の web サーバーにします。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. この例で、 **ContactManagerIisPath**プロパティがプロジェクト ファイルで他の場所で定義されています。 これは、単に、IIS アプリケーション パス、フォームで *[IIS web サイト名]/[アプリケーション名]* します。 ターゲットの条件を含めることができますを切り替える、*アプリ\_オフライン*プロパティ値を変更またはコマンド ライン パラメーターを提供することによって展開をオンまたはオフ。
9. という名前の新しいターゲットを追加**DeleteAppOffline**します。 このターゲット内で削除する MSDeploy.exe コマンドを呼び出す、*アプリ\_オフライン*送信先の web サーバーからのファイル。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. 最後のタスクでは、プロジェクト ファイルの実行中に適切な時点でこれらの新しいターゲットを呼び出します。 これは、さまざまな方法で行うことができます。 たとえば、 *Publish.proj*ファイル、 **FullPublishDependsOn**プロパティを実行する必要があるターゲットの一覧を指定しますで注文するときに、 **FullPublish**既定ターゲットが呼び出されます。
11. 呼び出す MSBuild プロジェクト ファイルを変更、 **DeployAppOffline**と**DeleteAppOffline**パブリッシュ処理に適切な時点でのターゲット。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

カスタム MSBuild プロジェクト ファイルを実行すると、*アプリ\_オフライン*ファイルは、ビルドの成功後すぐに、サーバーに配置されます。 削除されます、サーバーからすべての展開タスクが完了します。

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>アプリの追加\_の展開パッケージにオフライン ファイル

デプロイの構成方法に応じて、既存の IIS の転送先にコンテンツ web アプリケーション&#x2014;など、*アプリ\_offline.htm*ファイル&#x2014;web を展開するときに自動的に削除することがあります先にパッケージします。 いることを確認する、*アプリ\_offline.htm*ファイルは、デプロイ時間の場所には、さらに、ファイルの先頭に直接展開する web デプロイ パッケージ自体内でファイルを含める必要があります。展開するプロセス。

- かどうか、このトピックで前のタスクに従った、なりましたね、*アプリ\_offline.htm*ファイルを別のファイル名の下で web アプリケーション プロジェクト (を使用して*アプリ\_オフライン template.htm*) ビルド アクションを設定したが**None**します。 これらの変更は、開発に干渉することと、デバッグからファイルを防ぐために必要です。 その結果、いることを確認するパッケージ化プロセスをカスタマイズする必要があります、*アプリ\_offline.htm*ファイルが web デプロイ パッケージに含まれています。

Web 発行パイプライン (WPP) という項目一覧を使用して**FilesForPackagingFromProject** web 展開パッケージに含める必要があるファイルの一覧を作成します。 Web パッケージの内容をカスタマイズするには、この一覧に、独自の項目を追加します。 これを行うには、これらの大まかな手順を完了する必要があります。

1. という名前のカスタム プロジェクト ファイルを作成する *[プロジェクト名].wpp.targets*プロジェクト ファイルと同じフォルダーにします。

    > [!NOTE]
    > *. Wpp.targets*ファイルは、web アプリケーション プロジェクト ファイルと同じフォルダーに移動する必要がある&#x2014;など*ContactManager.Mvc.csproj*&#x2014;任意のカスタムと同じフォルダー内ではなくプロジェクト ファイルを使用して、ビルドおよび展開プロセスを制御します。
2. *. Wpp.targets*ファイルで実行される新しい MSBuild ターゲットを作成する*する前に*、 **CopyAllFilesToSingleFolderForPackage**ターゲット。 これは、パッケージに含める項目の一覧をビルドする WPP ターゲットです。
3. 新しいターゲットを作成、 **ItemGroup**要素。
4. **ItemGroup**要素を追加、 **FilesForPackagingFromProject**を指定し、*アプリ\_offline.htm*ファイル。

*. Wpp.targets*このファイルのようになります。


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


この例での重要な点を次に示します。

- **BeforeTargets**属性内の直前に実行するかを指定することによって、WPP にこのターゲットの挿入、 **CopyAllFilesToSingleFolderForPackage**ターゲット。
- **FilesForPackagingFromProject**項目は、 **DestinationRelativePath**からファイルの名前を変更するメタデータ値*アプリ\_オフライン template.htm**アプリ\_offline.htm*ように、一覧に追加されます。

次の手順は、これを追加する方法を示します *. wpp.targets*ファイルを web アプリケーション プロジェクト。

**追加する、です web 展開パッケージに wpp.targets ファイル。**

1. Visual Studio 2010 でソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、web アプリケーションのプロジェクト ノードを右クリックして (たとえば、 **ContactManager.Mvc**)、 をポイント**追加**をクリックして**新しい項目の**します。
3. **新しい項目の追加**ダイアログ ボックスで、 **XML ファイル**テンプレート。
4. **名前**ボックスに「 *[プロジェクト名] * * *.wpp.targets** (たとえば、 **ContactManager.Mvc.wpp.targets**) 順にクリックします**追加**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > プロジェクトのルート ノードに新しい項目を追加する場合は、プロジェクト ファイルと同じフォルダーにファイルが作成されます。 これは、Windows エクスプ ローラーでフォルダーを開くことで確認できます。
5. ファイルで、前に説明した MSBuild マークアップを追加します。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. 保存して閉じます、 *[プロジェクト名].wpp.targets*ファイル。

次回ビルドとパッケージ、web アプリケーション プロジェクト、WPP を自動的に検出、 *. wpp.targets*ファイル。 *アプリ\_オフライン template.htm*ファイルとして生成される web デプロイ パッケージに含めるは*アプリ\_offline.htm*します。

> [!NOTE]
> デプロイが失敗した場合、*アプリ\_offline.htm*ファイルが配置に保持され、アプリケーションがオフラインのままにします。 これは、通常、目的の動作です。 アプリケーションをオンラインに戻す、削除して、*アプリ\_offline.htm* web サーバーからのファイル。 また、エラーを修正して、展開を成功させるを実行する場合、*アプリ\_offline.htm*ファイルは削除されます。


## <a name="conclusion"></a>まとめ

このトピックにパブリッシュすることによって、展開の期間の web アプリケーションをオフラインを実行する方法が説明されている、*アプリ\_offline.htm*削除することで、展開プロセスの開始時、移行先サーバーにファイルを開き、終わり。 含める方法についても説明、*アプリ\_offline.htm* web 展開パッケージ内のファイル。

## <a name="further-reading"></a>関連項目

パッケージ化と展開プロセスの詳細については、次を参照してください[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)、 [Web パッケージ展開の構成パラメーター](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)、および[。Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)します。

これらのチュートリアルで説明されているカスタム MSBuild プロジェクト ファイル手法を使用して、発行時に、アプリケーションをオフラインに少し異なる方法を使用する必要がありますのではなく、Visual Studio から直接 web アプリケーションを発行する場合プロセスです。 詳細については、次を参照してください。[発行時に、web アプリをオフラインを実行する](https://go.microsoft.com/?linkid=9805135)(ブログの投稿)。

> [!div class="step-by-step"]
> [前へ](excluding-files-and-folders-from-deployment.md)
> [次へ](running-windows-powershell-scripts-from-msbuild-project-files.md)
