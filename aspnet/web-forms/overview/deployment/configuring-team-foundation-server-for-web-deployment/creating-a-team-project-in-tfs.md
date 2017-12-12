---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: "TFS でチーム プロジェクトの作成 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 4cb0d72330086ecb8cd9e6fb70ce0a57698dda5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-team-project-in-tfs"></a>TFS でチーム プロジェクトの作成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

## <a name="task-overview"></a>タスクの概要

プロビジョニングし、TFS で、新しいチーム プロジェクトを使用するには、これらの大まかな手順を完了する必要があります。

- 新しいチーム プロジェクトを作成したユーザーに権限を付与します。
- チーム プロジェクトを作成します。
- プロジェクトで作業するチーム メンバーに権限を付与します。
- 一部のコンテンツで確認してください。

このトピックの内容が、次の手順を実行する方法を示すし、ユーザーおよび各プロシージャを担当する可能性が高いジョブ ロールが識別します。 、、組織の構造に応じてこれらの各タスク場合がある別のユーザーの責任において、注意してください。

タスクとここでチュートリアルで作成した構成プロセスの一部としてチーム プロジェクト コレクションおよびインストールし、TFS を構成したことを想定しています。 これらの前提条件の詳細については、シナリオでは、全般的な背景情報については、次を参照してください。 [Web 配置を TFS のビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)です。

## <a name="grant-permissions-to-the-team-project-creator"></a>チーム プロジェクト作成者に権限を付与します。

新しいチーム プロジェクトを作成するのには、これらのアクセス許可が必要です。

- 必要があります、**プロジェクトを新規作成**が TFS アプリケーション層にアクセスを許可します。 一般にユーザーを追加することでこのアクセス許可を付与する、**プロジェクト コレクション管理者**TFS グループ。 **Team Foundation 管理者**グローバル グループにもこのアクセス許可が含まれています。
- TFS チーム プロジェクト コレクションに対応する SharePoint サイト コレクション内の新しいチーム サイトを作成する権限が必要です。 SharePoint グループにユーザーを追加することでこのアクセス許可を付与する通常**フル コントロール**サイト コレクションを SharePoint の権限です。
- SQL Server Reporting Services 機能を使用している場合は、メンバーをする必要があります、 **Team Foundation Content Manager** Reporting Services のロール。

### <a name="who-performs-these-procedures"></a>ユーザーには、これらの手順を実行しますか。

通常、人や、TFS の配置を管理するグループはまた、これらの手順を実行します。

これは権限の高い特権を持つセットであるため、新しいチーム プロジェクトはユーザーの小さなサブセットによって TFS の配置の管理を行う通常作成されます。 開発者は通常与えてはいけません新しいチーム プロジェクトの作成に必要なアクセス許可。

### <a name="grant-permissions-in-tfs"></a>TFS のアクセス許可を付与します。

新しいチーム プロジェクトを作成するユーザーを有効にするには、最初の高レベルのタスクがユーザーを追加する、**プロジェクト コレクション管理者**チーム プロジェクト コレクションにグループ化します。

**プロジェクト コレクション管理者グループにユーザーを追加するには**

1. TFS サーバー上で、**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft Team Foundation Server 2010**、クリックして**Team Foundation管理コンソール**です。
2. ナビゲーション ツリー ビューで、展開**アプリケーション層**、クリックして**チーム プロジェクト コレクション**です。

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. **チーム プロジェクト コレクション**ペインで、チーム プロジェクトを管理するコレクションを選択します。

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. **全般** タブで、をクリックして**グループ メンバーシップ**です。

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. **グローバル グループ**ダイアログ ボックスで、**プロジェクト コレクション管理者**グループ化、およびクリックして**プロパティ**です。
6. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、クリックして**追加**です。

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. **ユーザー、コンピューター、またはグループ**新しいチーム プロジェクトを作成できるようにユーザーのユーザー名をクリックしてダイアログ ボックスで、「**名前の確認**、順にクリック**OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**です。
9. **グローバル グループ**ダイアログ ボックスで、をクリックして**閉じる**です。

### <a name="grant-permissions-in-sharepoint-services"></a>SharePoint サービスにアクセス許可の付与

次に、TFS チーム プロジェクト コレクションに対応する SharePoint サイト コレクションで新しいチーム サイトを作成するユーザー権限を付与する必要があります。

**SharePoint サイト コレクションに対するフル コントロール権限を付与**

1. Team Foundation Server 管理コンソールでの**チーム プロジェクト コレクション** ページで、管理するチーム プロジェクト コレクションを選択します。
2. **SharePoint サイト** タブの値に注意してください、**既定のサイトは、常に現在**URL。

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Internet Explorer を開き、手順 2 で書き留めた URL に進みます。

    > [!NOTE]
    > しているログオンしていない場合 Windows をチーム プロジェクト コレクションを作成したユーザーとして、このユーザーに続行するのには SharePoint にサインインします。 する必要があります。
4. **サイトの操作** メニューのをクリックして**サイト設定**です。

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. **サイト設定** ページの **ユーザーと権限**、 をクリックして**ユーザーとグループ**です。
6. 左側のナビゲーション パネル で、**グループ**です。

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. **ユーザーとグループ: すべてのグループ**] ページで [**このサイトのグループの設定**です。

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > 表示される、 **HTTP 404 Not Found**二重 HTTP エンコード バグによるエラーが発生します。 この場合、この URL に置き換えます。   
    > [*サイト コレクション URL*]/\_layouts/permsetup.aspx  
    > 例:  
    > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. **このサイトのグループの設定**ページで、チーム プロジェクトを作成したユーザーを追加、**所有者**グループ化、およびをクリックして**OK**です。

    ![](creating-a-team-project-in-tfs/_static/image10.png)

チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するユーザーを有効にする方法の詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可を設定](https://msdn.microsoft.com/en-us/library/dd547204.aspx)です。

## <a name="create-a-new-team-project-and-add-users"></a>新しいチーム プロジェクトを作成し、ユーザーの追加

必要なアクセス許可がある場合を使用して、**チーム エクスプ ローラー**新しいチーム プロジェクトを作成する Visual Studio 2010 でのウィンドウ。 この方法は、必要なすべての情報を収集し、TFS、SharePoint、および SQL Server Reporting Services で必要なタスクを実行するウィザードを提供します。 またを追加し、内容を変更するようにする、開発者チームのメンバーに、新しいチーム プロジェクトに対するアクセス許可を付与する必要があります。

### <a name="who-performs-these-procedures"></a>ユーザーには、これらの手順を実行しますか。

通常、TFS 管理者または開発者のチーム リーダーは、これらの手順を実行します。

### <a name="create-a-new-team-project"></a>新しいチーム プロジェクトを作成します。

次の手順では、TFS 2010 で、新しいチーム プロジェクトを作成する方法について説明します。

**新しいチーム プロジェクトを作成するには**

1. **開始** メニューのをポイント**すべてのプログラム**をクリックして**Microsoft Visual Studio 2010**を右クリックして**Microsoft Visual Studio 2010**、クリックして**管理者として実行**です。

    > [!NOTE]
    > 管理者として Visual Studio 2010 を実行しない、最後の手順で、新しいチーム プロジェクト ウィザードは失敗します。
2. 場合、**ユーザー アカウント制御** ダイアログ ボックスが表示されたら、をクリックして**はい**です。
3. Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**です。

    > [!NOTE]
    > 既に TFS サーバーへの接続を構成した場合は、手順 4. ~ 7. を省略できます。
4. **チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**です。
5. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**です。
6. **Team Foundation Server の追加**ダイアログ ボックスでは、TFS インスタンスの詳細を提供し、をクリックして**OK**です。

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**です。
8. **チーム プロジェクトへ接続** ダイアログ ボックスで、TFS インスタンスが接続するチームを選択するプロジェクト コレクションに追加し、をクリックする**接続**です。

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. **チーム エクスプ ローラー**ウィンドウで、右クリックし、チーム プロジェクト コレクション、をクリックして**新しいチーム プロジェクト**です。

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. **新しいチーム プロジェクト**ダイアログ ボックスでは、名前と、チーム プロジェクトの説明を入力し、をクリックして**次**です。

    > [!NOTE]
    > チーム プロジェクトには、スペースが含まれている場合 (Web 配置) インターネット インフォメーション サービス (IIS) Web 展開ツールを使用して出力パスからパッケージを配置に到達するといくつかの問題に直面可能性があります。 パスにスペースが困難よりずっと Web Deploy コマンドを実行します。

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. **プロセス テンプレートの選択** ページで、使用して、開発プロセスを管理し、をクリックするプロセス テンプレートを選択**次**です。

    > [!NOTE]
    > TFS のプロセス テンプレートの詳細については、次を参照してください。[プロセス テンプレートとツール](https://msdn.microsoft.com/en-us/vstudio/aa718795)です。
12. **チーム サイトの設定** ページで変更せずに、既定の設定のままにしてをクリックして**次**です。
13. この設定は、作成すると、または TFS チーム プロジェクトに関連付けられている SharePoint チーム サイトを識別します。 開発チームは、ドキュメントの管理、ディスカッション スレッドに参加、wiki ページを作成するコードに関連付けられていないその他の各種のタスクを実行し、このサイトを使用できます。 詳細については、次を参照してください。 [Team Foundation Server と SharePoint 製品との間の相互作用](https://msdn.microsoft.com/en-us/library/ms253177.aspx)です。
14. **ソース管理の設定の指定** ページで変更せずに、既定の設定のままにしてをクリックして**次**です。
15. この設定を識別または、TFS フォルダー階層内のあるコンテンツのルート フォルダーとして機能する場所を作成します。
16. **チーム プロジェクトの設定の確認**] ページで [**完了**です。
17. ときに、新しいチーム プロジェクトが正常が作成した、**チーム プロジェクト作成** ページで、をクリックして**閉じる**です。

### <a name="add-users-to-a-team-project"></a>チーム プロジェクトにユーザーを追加します。

これで、新しいチーム プロジェクトを作成した、追加して、コンテンツに対する共同作業を開始するようにするユーザーに権限を付与できます。

**ユーザーをチーム プロジェクトに追加するには**

1. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクトを右クリックし、順にポイント**チーム プロジェクトの設定**、順にクリック**グループ メンバーシップ**です。

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. 追加、変更、およびソース管理下にあるコードを削除するユーザーを有効にするには、その人に追加する必要があります、**コントリビューター**グループ。
3. **プロジェクト グループ**ダイアログ ボックスで、**コントリビューター**グループ化、およびをクリックして**プロパティ**です。

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、クリックして**追加**です。

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. **ユーザー、コンピューター、またはグループ**をチーム プロジェクトに追加するユーザーのユーザー名をクリックしてダイアログ ボックスで、型**名前の確認**、クリックして**ok**です。

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. **Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**です。
7. **プロジェクト グループ**ダイアログ ボックスで、をクリックして**閉じる**です。

## <a name="conclusion"></a>まとめ

この時点では、使用するには、新しいチーム プロジェクト準備が整うし、開発者チームは、コンテンツを追加して、開発プロセスで共同作業を開始できます。

次のトピックでは、[をソース管理に追加するコンテンツ](adding-content-to-source-control.md)、ソース管理にコンテンツを追加する方法について説明します。

## <a name="further-reading"></a>関連項目

TFS でチーム プロジェクトを作成する方法より広範なガイダンスについては、次を参照してください。[チーム プロジェクトの作成](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx)です。 チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するユーザーを有効にする方法の詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可を設定](https://msdn.microsoft.com/en-us/library/dd547204.aspx)です。 ユーザーをチーム プロジェクトに追加する方法の詳細については、次を参照してください。[チーム プロジェクトへのユーザーの追加](https://msdn.microsoft.com/en-us/library/bb558971.aspx)です。

>[!div class="step-by-step"]
[前へ](configuring-team-foundation-server-for-web-deployment.md)
[次へ](adding-content-to-source-control.md)
