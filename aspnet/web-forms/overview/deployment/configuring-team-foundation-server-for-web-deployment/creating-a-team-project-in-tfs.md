---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: TFS でチーム プロジェクトの作成 |Microsoft Docs
author: jrjlee
description: このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 9218a22ff221dc7067662c58ccd3e758fca493b7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838414"
---
<a name="creating-a-team-project-in-tfs"></a>TFS でチーム プロジェクトの作成
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

## <a name="task-overview"></a>タスクの概要

プロビジョニングして、TFS で新しいチーム プロジェクトを使用するには、これらの大まかな手順を完了する必要があります。

- 新しいチーム プロジェクトを作成したユーザーにアクセス許可を付与します。
- チーム プロジェクトを作成します。
- プロジェクトで作業するチーム メンバーにアクセス許可を付与します。
- 一部のコンテンツで確認します。

このトピックがこれらの手順を実行する方法を示し、ユーザーと各プロシージャを担当する可能性のあるジョブの役割は識別します。 によって、組織の構造、これらの各タスクがありますを他のユーザーの責任を認識します。

タスクとチュートリアルでは、このトピックでは、インストールされているし、TFS が構成されているし、構成プロセスの一部としてチーム プロジェクト コレクションを作成したことを想定しています。 詳細については、このような想定とシナリオの一般的な背景情報をご覧ください[Web 配置の TFS のビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)します。

## <a name="grant-permissions-to-the-team-project-creator"></a>チーム プロジェクトの作成者のアクセス許可します。

新しいチーム プロジェクトを作成するためには、これらのアクセス許可が必要です。

- 必要があります、**新しいプロジェクトを作成**TFS アプリケーション層に対する権限。 一般にユーザーを追加してこのアクセス許可を付与する、**プロジェクト コレクション管理者**TFS グループ。 **Team Foundation 管理者**グローバル グループにもこのアクセス許可が含まれています。
- TFS のチーム プロジェクト コレクションに対応する SharePoint サイト コレクション内の新しいチーム サイトを作成するアクセス許可が必要です。 通常の SharePoint グループにユーザーを追加することでこのアクセス許可を付与する**フルコントロール**権限を SharePoint サイト コレクション。
- メンバーである必要がある SQL Server Reporting Services の機能を使用している場合、 **Team Foundation Content Manager** Reporting services のロール。

### <a name="who-performs-these-procedures"></a>これらの手順を実行しますか。

通常、ユーザーまたは TFS の配置を管理グループはまた、これらの手順を実行します。

これは、高い特権を持つ権限のセットであるため、新しいチーム プロジェクトはごく一部のユーザーによって TFS の配置を管理するための責任で通常作成されます。 開発者が通常は付与されない新しいチーム プロジェクトを作成するために必要なアクセス許可。

### <a name="grant-permissions-in-tfs"></a>TFS のアクセス許可の付与

ユーザーを追加する最初の高レベルのタスクは、新しいチーム プロジェクトを作成するユーザーを有効にする場合、**プロジェクト コレクション管理者**チーム プロジェクト コレクションのグループ化します。

**プロジェクト コレクション管理者グループにユーザーを追加するには**

1. TFS サーバー上で、**開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft Team Foundation Server 2010**、順にクリックします**Team Foundation管理コンソール**します。
2. ナビゲーション ツリー ビューで、展開**アプリケーション層**、 をクリックし、**チーム プロジェクト コレクション**します。

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. **チーム プロジェクト コレクション**ウィンドウで、チーム プロジェクトを管理するコレクションを選択します。

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. **全般**] タブで [**グループ メンバーシップ**します。

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. **グローバル グループ**ダイアログ ボックスで、**プロジェクト コレクション管理者**グループ化、およびクリックして**プロパティ**します。
6. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、 をクリックし、**追加**します。

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. **ユーザー、コンピューター、またはグループ**新しいチーム プロジェクトを作成しユーザーのユーザー名 をクリックして ダイアログ ボックスで、「**名前の確認**、順にクリックします**OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**します。
9. **グローバル グループ**ダイアログ ボックスで、をクリックして**閉じる**します。

### <a name="grant-permissions-in-sharepoint-services"></a>SharePoint Services のアクセス許可の付与

次に、TFS チーム プロジェクト コレクションに対応する SharePoint サイト コレクションで新しいチーム サイトを作成するユーザーのアクセス許可を付与する必要があります。

**SharePoint サイト コレクションに対するフル コントロール権限を付与**

1. Team Foundation Server 管理コンソールで、**チーム プロジェクト コレクション** ページで、管理するチーム プロジェクト コレクションを選択します。
2. **SharePoint サイト** タブの値に注意してください、**現在既定サイトの場所**URL。

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Internet Explorer を開きし、手順 2 で書き留めた URL に移動します。

    > [!NOTE]
    > しないにログオンしている Windows チーム プロジェクト コレクションを作成したユーザーとしての場合は、このユーザーとして続行するには SharePoint にサインインします。 必要があります。
4. **サイトの操作** メニューのをクリックして**サイト設定**します。

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. **サイト設定** ページ **ユーザーおよびアクセス許可**、 をクリックして**ユーザーとグループ**します。
6. 左側のナビゲーション パネル で、**グループ**します。

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. **ユーザーとグループ: すべてのグループ**] ページで [**このサイトのグループの設定**します。

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > 表示される、 <strong>HTTP 404 Not Found</strong>エラー二重 HTTP エンコード バグが発生しました。 この場合、この URL に置き換えます。   
   > `[site_collection_URL]/_layouts/permsetup.aspx` 例えば：  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. **このサイトのグループの設定**ページで、チーム プロジェクトを作成するユーザーを追加、**所有者**グループ化、およびクリックして **[ok]** します。

    ![](creating-a-team-project-in-tfs/_static/image10.png)

チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するための詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可の設定](https://msdn.microsoft.com/library/dd547204.aspx)します。

## <a name="create-a-new-team-project-and-add-users"></a>新しいチーム プロジェクトを作成し、ユーザーの追加

必要なアクセス許可がある場合とを使用できます、**チーム エクスプ ローラー**新しいチーム プロジェクトを作成する Visual Studio 2010 でのウィンドウ。 このアプローチでは、必要なすべての情報を収集し、TFS、SharePoint、および SQL Server Reporting Services で必要なタスクを実行するウィザードを提供します。 また、追加し、コンテンツを変更できるように、開発者チームのメンバーに、新しいチーム プロジェクトに対するアクセス許可を付与する必要があります。

### <a name="who-performs-these-procedures"></a>これらの手順を実行しますか。

通常、TFS 管理者または開発者のチーム リーダーは、これらの手順を実行します。

### <a name="create-a-new-team-project"></a>新しいチーム プロジェクトを作成します。

次の手順では、TFS 2010 で、新しいチーム プロジェクトを作成する方法について説明します。

**新しいチーム プロジェクトを作成するには**

1. **開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft Visual Studio 2010**、右クリックして**Microsoft Visual Studio 2010**、クリックして**管理者として実行**します。

    > [!NOTE]
    > Visual Studio 2010 を管理者として実行しない、最後の手順で、新しいチーム プロジェクト ウィザードは失敗します。
2. 場合、**ユーザー アカウント制御** ダイアログ ボックスが表示されたら、をクリックして**はい**します。
3. Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**します。

    > [!NOTE]
    > TFS サーバーへの接続を既に構成した場合は、手順 4 ~ 7 を省略できます。
4. **チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**します。
5. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**します。
6. **Team Foundation Server の追加** ダイアログ ボックスでは、TFS インスタンスの詳細を入力し、順にクリックします**OK**します。

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**します。
8. **チーム プロジェクトへの接続**ダイアログ ボックスで、TFS インスタンスを接続するチームを選択するプロジェクト コレクションに追加し、クリックする**Connect**します。

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. **チーム エクスプ ローラー**ウィンドウで、右クリックし、チーム プロジェクト コレクション をクリックして**新しいチーム プロジェクト**します。

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. **新しいチーム プロジェクト**ダイアログ ボックスで、名前と、チーム プロジェクトの説明を指定し、順にクリックします**次**します。

    > [!NOTE]
    > チーム プロジェクトには、スペースが含まれている場合は、するように、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して出力パスからパッケージをデプロイするときに、いくつかの問題を発生する可能性があります。 パスにスペースによりするよりずっと困難で Web Deploy コマンドを実行します。

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. **プロセス テンプレートを選択**をクリックして、開発プロセスの管理を使用するプロセス テンプレートの選択 ページで、**次**。

    > [!NOTE]
    > Tfs プロセス テンプレートの詳細については、次を参照してください。[プロセス テンプレートとツール](https://msdn.microsoft.com/vstudio/aa718795)します。
12. **チーム サイトの設定** ページで変更せずに、既定の設定のままにしてをクリックし、**次**します。
13. この設定を作成、または TFS のチーム プロジェクトに関連付けられている SharePoint チーム サイトを識別します。 開発チームは、ドキュメントの管理、スレッドのディスカッションに参加、wiki ページを作成、およびコードに関連していないその他の各種のタスクを実行する、このサイトを使用できます。 詳細については、次を参照してください。 [SharePoint 製品との間の相互作用と Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx)します。
14. **ソース管理設定の指定** ページで変更せずに、既定の設定のままにしてをクリックし、**次**。
15. この設定は、識別またはコンテンツのルート フォルダーとして動作する TFS フォルダー階層内の場所を作成します。
16. **チーム プロジェクト設定の確認**] ページで [**完了**します。
17. ときに、新しいチーム プロジェクトが正常に作成で、**チーム プロジェクト作成**] ページで [**閉じる**します。

### <a name="add-users-to-a-team-project"></a>チーム プロジェクトにユーザーを追加します。

新しいチーム プロジェクトを作成するので、追加して、コンテンツの共同作業を開始することをユーザーに権限を付与できます。

**チーム プロジェクトにユーザーを追加するには**

1. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクトを右クリックし、 をポイント**チーム プロジェクトの設定**、順にクリックします**グループ メンバーシップ**します。

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. 追加、変更、およびソース管理下にあるコードを削除するユーザーを有効にするのに自分を追加する必要があります、**共同作成者**グループ。
3. **プロジェクト グループ**ダイアログ ボックスで、**共同作成者**グループ化、およびクリックして**プロパティ**します。

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、 をクリックし、**追加**します。

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. **ユーザー、コンピューター、またはグループ**、チーム プロジェクトに追加するユーザーのユーザー名 をクリックして ダイアログ ボックスで、「**名前の確認**、順にクリックします**OK**します。

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**します。
7. **プロジェクト グループ**ダイアログ ボックスで、をクリックして**閉じる**します。

## <a name="conclusion"></a>まとめ

この時点で、新しいチーム プロジェクトを使用する準備が、開発者チームは、コンテンツを追加して、開発プロセスでの共同作業を開始できます。

次のトピックでは、[をソース管理に追加するコンテンツ](adding-content-to-source-control.md)、コンテンツ ソースのコントロールを追加する方法について説明します。

## <a name="further-reading"></a>関連項目

TFS でチーム プロジェクトを作成する方法より広範なガイダンスについては、次を参照してください。[チーム プロジェクト作成](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx)です。 チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するための詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可の設定](https://msdn.microsoft.com/library/dd547204.aspx)します。 チーム プロジェクトにユーザーを追加する方法の詳細については、次を参照してください。[チーム プロジェクトへのユーザーの追加](https://msdn.microsoft.com/library/bb558971.aspx)します。

> [!div class="step-by-step"]
> [前へ](configuring-team-foundation-server-for-web-deployment.md)
> [次へ](adding-content-to-source-control.md)
