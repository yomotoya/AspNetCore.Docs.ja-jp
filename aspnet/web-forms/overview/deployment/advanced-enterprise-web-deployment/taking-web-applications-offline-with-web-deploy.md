---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: "Web アプリケーションをオフラインでの web 配置 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、インターネット インフォメーション サービス (IIS) Web 展開マネージャーを使用する自動展開の間オフライン web アプリケーションを実行する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: a0c59245eedbf53f367949e12dd83e2611f44fc4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Web アプリケーションをオフラインでの web 配置します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用する自動展開の間オフライン web アプリケーションを実行する方法について説明します。 Web アプリケーションを参照するユーザーにリダイレクト、*アプリ\_offline.htm*ファイルの展開が完了するまでです。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

多数のシナリオでは、データベースや web サービスと同様に、関連するコンポーネントを変更するときに、web アプリケーションをオフラインにします。 通常、IIS および ASP.NET ですることにより実現という名前のファイルを配置する*アプリ\_offline.htm* IIS web サイトまたは web アプリケーションのルート フォルダーにします。 *アプリ\_offline.htm*ファイルは標準の HTML ファイルでありは通常、サイトはメンテナンスのため一時的に使用できないことをユーザーに通知する簡単なメッセージが含まれます。 中に、*アプリ\_offline.htm* web サイトのルート フォルダーにファイルが存在し、IIS がファイルにすべての要求を自動的にリダイレクトされます。 削除する更新プログラムの作業が終了したら、ときに、*アプリ\_offline.htm*ファイルと、web サイトは、要求を通常どおり処理が再開されます。

ターゲット環境に自動または 1 ステップの展開を実行する Web Deploy を使用するときにの追加と削除を組み込むことができます、*アプリ\_offline.htm*展開プロセスにファイル。 これを行うには、これらの高度なタスクを完了する必要があります。

- Microsoft Build Engine (MSBuild) プロジェクト ファイルで使用して MSBuild ターゲットを作成、展開プロセスを制御することはコピー、*アプリ\_offline.htm*展開タスクの前に移行先サーバーにファイル開始します。
- 削除するもう 1 つの MSBuild ターゲットを追加、*アプリ\_offline.htm*すべての展開タスクが完了したら、移行先サーバーからファイルです。
- Web アプリケーション プロジェクト内で作成、 *. wpp.targets*確実にファイル、*アプリ\_offline.htm* Web Deploy が呼び出されると、ファイルが、展開パッケージに追加します。

このトピックでは、これらの手順を実行する方法を示します。 タスクとこのトピックの「チュートリアルと少なくとも 1 つの web アプリケーション プロジェクトを含むソリューションを既に作成済み」の説明に従って、展開プロセスを制御するカスタム プロジェクト ファイルを使用すること[Web 配置で、エンタープライズ](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。 また、使用することができます、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションのトピックで例を実行します。

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>アプリの追加\_オフライン ファイルを Web アプリケーション プロジェクト

追加するは、最初のタスクを完了する必要があります、*アプリ\_オフライン*ファイルを web アプリケーション プロジェクト。

- ファイルが、開発プロセスに干渉しないようにする (永久にオフラインになるアプリケーションをたくない) を呼び出す必要があります、もの以外の*アプリ\_offline.htm*です。 たとえば、ファイルの名前をでした*アプリ\_オフライン template.htm*です。
- ファイルとして配置されていることを防ぐためには、ビルド アクションを設定必要があります**None**です。

**アプリを追加する\_オフライン ファイルを web アプリケーション プロジェクト**

1. Visual Studio 2010 でソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、web アプリケーション プロジェクトを右クリックし、順にポイント**追加**、クリックして**新しい項目の**します。
3. **新しい項目の追加**ダイアログ ボックスで、 **HTML ページ**です。
4. **名前**ボックスに、入力**アプリ\_オフライン template.htm**、クリックして**追加**です。

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. アプリケーションが使用できないことをユーザーに通知する簡単な HTML を追加し、ファイルを保存します。 任意のサーバー側のタグを含めないでください (たとえば、任意のタグが付いている"asp:") です。 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. **ソリューション エクスプ ローラー**  ウィンドウでは、新しいファイルを右クリックし、をクリックして**プロパティ**です。
7. **プロパティ** ウィンドウで、**ビルド アクション**行で、 **None**です。

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>展開して、アプリを削除する\_オフライン ファイル

次の手順では、展開プロセスの開始時に移行先サーバーにファイルをコピーし、最後に削除するような配置ロジックを変更します。

> [!NOTE]
> 次の手順は、ことを使用するカスタム MSBuild プロジェクト ファイル、配置プロセスを制御する」の説明に従って前提としています。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。 Visual Studio から直接配置する場合、は、別のアプローチを使用する必要があります。 Sayed Ibrahim Hashimi でこのような 1 つの方法を説明する[かかる、Web アプリ オフライン中に発行する方法](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)です。


展開する、*アプリ\_オフライン*ファイル変換先の IIS の web サイトに MSDeploy.exe を使用して呼び出す必要があります、 [Web Deploy **contentPath**プロバイダー](https://technet.microsoft.com/en-us/library/dd569034(WS.10).aspx)です。 **ContentPath**プロバイダーをサポートしている物理ディレクトリのパスと IIS の web サイトまたはアプリケーションのパスの両方を Visual Studio プロジェクト フォルダーと、IIS web アプリケーションの間でファイルを同期する理想的な選択肢になります。 ファイルを展開するには、MSDeploy コマンドのようにこの必要があります。


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


展開プロセスの最後に、移行先サイトから、ファイルを削除するには、MSDeploy コマンドのようにこれ必要があります。


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


ビルドおよび配置プロセスの一部としてこれらのコマンドを自動化するには、カスタム MSBuild プロジェクト ファイルに統合する必要があります。 次の手順では、これを行う方法について説明します。

**展開し、アプリ削除\_オフライン ファイル**

1. Visual Studio 2010 で、展開プロセスを制御する MSBuild プロジェクト ファイルを開きます。 [連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションでは、これは、 *Publish.proj*ファイル。
2. ルートで**プロジェクト**要素を新規作成**PropertyGroup**の変数を格納する要素、*アプリ\_オフライン*展開。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot**でプロパティが別の場所で定義されている、 *Publish.proj*ファイル。 現在のパス & #x 2014 基準とした; つまりの場所に対する相対、ソース コンテンツのルート フォルダーの場所を示す、 *Publish.proj*ファイル。
4. **ContentPath**プロバイダー受け入れません相対ファイル パスを展開する前に、ソース ファイルへの絶対パスを取得する必要があります。 使用することができます、 [ConvertToAbsolutePath](https://msdn.microsoft.com/en-us/library/bb882668.aspx)これを行うタスクです。
5. 新しい**ターゲット**という名前の要素**GetAppOfflineAbsolutePath**です。 このターゲット内で使用して、 **ConvertToAbsolutePath**への絶対パスを取得するタスク、*アプリ\_オフライン テンプレート*プロジェクト フォルダー内のファイルです。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. このターゲットへの相対パスを受け取り、*アプリ\_オフライン テンプレート*プロジェクト フォルダーにファイルし、絶対ファイル パスと新しいプロパティに保存します。 **BeforeTargets**属性を指定する前に実行するには、このターゲットを表示する、 **DeployAppOffline**ターゲットで、次の手順で作成します。
7. という名前の新しいターゲットを追加**DeployAppOffline**です。 このターゲット内には、展開する場合、MSDeploy.exe コマンドを呼び出す、*アプリ\_オフライン*ファイルを移行先の web サーバー。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. この例では、 **ContactManagerIisPath**プロパティがプロジェクト ファイルで別の場所で定義されています。 これは、単に、IIS アプリケーション パス、フォームで*[IIS web サイト名]/[アプリケーション名]*です。 ターゲットの条件を含めることができますを切り替える、*アプリ\_オフライン*プロパティ値の変更またはコマンド ライン パラメーターを提供することによって、オンまたはオフを展開します。
9. という名前の新しいターゲットを追加**DeleteAppOffline**です。 このターゲット内を削除する場合、MSDeploy.exe コマンドを呼び出す、*アプリ\_オフライン*ターゲット web サーバーからファイルです。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. 最後のタスクでは、プロジェクト ファイルの実行中に適切な時点で新しいターゲットを呼び出します。 これは、さまざまな方法で行うことができます。 たとえば、 *Publish.proj*ファイル、 **FullPublishDependsOn**プロパティは、実行する必要があるターゲット ユーザーの一覧を示すで注文するときに、 **FullPublish**既定ターゲットが呼び出されます。
11. 呼び出すには、MSBuild プロジェクト ファイルを変更、 **DeployAppOffline**と**DeleteAppOffline**パブリッシュ処理の適切な時点でのターゲットです。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

カスタムの MSBuild プロジェクト ファイルを実行すると、*アプリ\_オフライン*ファイルはビルドの成功後すぐに、サーバーに配置されます。 削除されますサーバーのすべての展開タスクが完了します。

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>アプリの追加\_の展開パッケージにオフライン ファイル

同様に、IIS web アプリケーションの移行先 & #x 2014; で、展開、既存の内容を構成する方法に応じて、*アプリ\_offline.htm* file & #x 2014; web を展開するときに自動的に削除することがあります変換先にパッケージします。 いることを確認する、*アプリ\_offline.htm*ファイルはそのまま残ります、展開中に、ファイルの先頭に直接展開するのにさらに、web 配置パッケージ自体内でファイルをインクルードする必要があります展開プロセスです。

- かどうかは、このトピックで前のタスクを完了済みを追加した、*アプリ\_offline.htm*ファイルを別のファイル名の下にある web アプリケーション プロジェクト (を使用した*アプリ\_オフライン template.htm*) をビルド アクションを設定するありますと**None**です。 これらの変更は、開発に干渉することと、デバッグからファイルを防ぐために必要です。 その結果、ことを確認するパッケージ化プロセスをカスタマイズする必要があります、*アプリ\_offline.htm* web 展開パッケージにファイルが含まれています。

Web 発行パイプライン (WPP) という名前の項目リストを使用して**FilesForPackagingFromProject** web 展開パッケージに含める必要があるファイルの一覧を作成します。 この一覧に、独自の項目を追加することで、web のパッケージの内容をカスタマイズできます。 これを行うには、これらの大まかな手順を完了する必要があります。

1. という名前のカスタム プロジェクト ファイルを作成する*[プロジェクト名].wpp.targets*は、プロジェクト ファイルと同じフォルダーにします。

    > [!NOTE]
    > *. Wpp.targets* web アプリケーション プロジェクト ファイル & #x 2014; と同じフォルダーに移動する必要があるファイルなど、 *ContactManager.Mvc.csproj*(& a) いずれかとして #x; 2014 ではなく、同じフォルダー管理、ビルドおよび展開プロセスを使用するカスタム プロジェクト ファイルです。
2. *. Wpp.targets*ファイルを実行する新しい MSBuild ターゲットを作成*する前に*、 **CopyAllFilesToSingleFolderForPackage**ターゲットです。 これは、WPP ビルドするターゲットをパッケージに含める項目の一覧です。
3. 新しいターゲットを作成、 **ItemGroup**要素。
4. **ItemGroup**要素を追加、 **FilesForPackagingFromProject**を指定し、*アプリ\_offline.htm*ファイル。

*. Wpp.targets*このファイルのようになります。


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


この例での重要な点を次に示します。

- **BeforeTargets**属性を挿入する直前に実行するかを指定することによって、WPP にこのターゲット、 **CopyAllFilesToSingleFolderForPackage**ターゲットです。
- **FilesForPackagingFromProject**項目の使用、 **DestinationRelativePath**からファイルの名前を変更するメタデータ値*アプリ\_オフライン template.htm**アプリ\_offline.htm*ように、一覧に追加されます。

次の手順は、これを追加する方法を示します*. wpp.targets*ファイルを web アプリケーション プロジェクト。

**追加する、です web 配置パッケージに wpp.targets ファイル。**

1. Visual Studio 2010 でソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、web アプリケーションのプロジェクト ノードを右クリックし (たとえば、 **ContactManager.Mvc**)、 をポイント**追加**をクリックして**新しい項目の**します。
3. **新しい項目の追加**ダイアログ ボックスで、 **XML ファイル**テンプレート。
4. **名前**ボックスに、入力*[プロジェクト名]***. wpp.targets** (たとえば、 **ContactManager.Mvc.wpp.targets**)、をクリックして**追加**です。

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > プロジェクトのルート ノードに新しい項目を追加する場合は、プロジェクト ファイルと同じフォルダーにファイルが作成されます。 これは、Windows エクスプ ローラーでフォルダーを開くことで確認できます。
5. ファイルでは、前に説明した MSBuild マークアップを追加します。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. 保存して閉じます、 *[プロジェクト名].wpp.targets*ファイル。

次回のビルドとパッケージを web アプリケーション プロジェクト、WPP は自動的に検出、 *. wpp.targets*ファイル。 *アプリ\_オフライン template.htm*ファイルは、結果として得られる web 展開パッケージとして含ま*アプリ\_offline.htm*です。

> [!NOTE]
> 展開に失敗した場合、*アプリ\_offline.htm*位置が変わりませんファイルと、アプリケーションはオフラインのままです。 これは、通常、目的の動作です。 アプリケーションに戻すことをオンラインに戻す、削除する、*アプリ\_offline.htm* web サーバーからファイルです。 また、エラーを修正して、展開を成功させるを実行する場合、*アプリ\_offline.htm*ファイルは削除されます。


## <a name="conclusion"></a>まとめ

このトピックには、発行して、展開の間オフライン web アプリケーションを実行する方法が説明されている、*アプリ\_offline.htm*展開プロセスの開始時に、移行先サーバーへのファイルし、削除することで、終わり。 インクルードする方法についても説明、*アプリ\_offline.htm* web 展開パッケージ内のファイルです。

## <a name="further-reading"></a>関連項目

パッケージと展開プロセスの詳細については、次を参照してください[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)、 [Web パッケージの展開の構成パラメーター](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)、および[。Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)です。

これらのチュートリアルで説明されているカスタム MSBuild プロジェクト ファイル アプローチを使用する必要があります、少し異なる方法を使用してアプリケーションを発行中に、オフラインを実行するのではなく、Visual Studio から直接 web アプリケーションを発行する場合プロセスです。 詳細については、次を参照してください。[発行時にオフライン web アプリを実行する](https://go.microsoft.com/?linkid=9805135)(ブログの投稿)。

>[!div class="step-by-step"]
[前へ](excluding-files-and-folders-from-deployment.md)
[次へ](running-windows-powershell-scripts-from-msbuild-project-files.md)
