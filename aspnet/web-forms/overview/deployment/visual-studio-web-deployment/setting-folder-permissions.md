---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Visual Studio を使用して ASP.NET Web 展開: フォルダーのアクセス許可の設定 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: a0c4f9f7cf30c1fc6a06c2cf798dc7ed04585504
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383650"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Visual Studio を使用して ASP.NET Web 展開: フォルダーのアクセス許可の設定
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、フォルダーのアクセス許可の設定、 *Elmah*フォルダーにデプロイされた web サイト、アプリケーションは、そのフォルダー内のログ ファイルを作成できるようにします。

Visual Studio 開発サーバー (Cassini) または IIS Express を使用して Visual Studio の web アプリケーションをテストする場合は、自分のユーザー名、アプリケーションが実行されます。 開発用コンピューターの管理者であるほとんどの場合と、任意のフォルダー内の任意のファイルに何もする完全な権限を持ちます。 アプリケーション プールに割り当てられているサイトに対して定義されている id の下で実行されます IIS 下でアプリケーションを実行します。 これは、通常、アクセス許可が制限されているシステム定義のアカウントです。 既定でに読み取りし、web アプリケーションのファイルおよびフォルダーに対する execute 権限が、書き込みアクセス権がありません。

これは、場合は、アプリケーションを作成するか、web アプリケーションで必要な更新プログラムのファイルは、共通が問題になります。 Elmah で XML ファイルの作成、Contoso University アプリケーションで、 *Elmah*エラーの詳細を保存するにはフォルダー。 Elmah のようなものを使用しない場合でも、サイトは、ユーザーがファイルをアップロードまたはサイトのフォルダーにデータを書き込むことが他のタスクを実行できるように可能性があります。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="test-error-logging-and-reporting"></a>テスト エラーのログ記録とレポート

どのアプリケーションが動作しない正しく IIS で (ただし、Visual Studio でテストされたときに行った) を表示する、Elmah は、によってログが記録される通常を開き、Elmah のエラー ログの詳細を表示するエラーが発生することができます。 Elmah が XML ファイルを作成し、エラーの詳細を格納することでない場合は、空のエラー レポートを参照してください。

ブラウザーを開きに移動`http://localhost/ContosoUniversity`のように、無効な URL を要求し、 *Studentsxxx.aspx*します。 代わりにシステムが生成したエラー ページを参照してください、 *GenericErrorPage.aspx*ため、ページ、 `customErrors` Web.config ファイルの設定は"RemoteOnly"と、ローカル IIS を実行しています。

![HTTP 404 エラー ページ](setting-folder-permissions/_static/image1.png)

今すぐ実行*Elmah.axd*エラー レポートを表示します。 管理者アカウントの資格情報でログインした後 (&quot;管理者&quot;と&quot;devpwd&quot;)、Elmah での XML ファイルを作成できなかったために、空のエラー ログのページを参照してください、 *Elmah*フォルダー。

![エラー ログ空](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Elmah フォルダーに対する書き込みアクセス許可の設定

フォルダーのアクセス許可を手動で設定することができますか、自動展開プロセスの部分を行うことができます。 複雑な MSBuild コードでは、自動を行う必要があり、次の手順を手動で行う方法、最初にデプロイするときに行うだけであるためです。 (展開プロセスのこの部分を作成する方法については、次を参照してください[で Web の発行フォルダーのアクセス許可の設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)Sayed Hashimi's ブログ。)。

1. **ファイル エクスプ ローラー**に移動します*C:\inetpub\wwwroot\ContosoUniversity*します。 右クリックし、 *Elmah*フォルダーで、**プロパティ**を選び、**セキュリティ**タブ。
2. **[編集]** をクリックします。
3. **Elmah のアクセス許可**ダイアログ ボックスで、 **DefaultAppPool**を選び、**書き込み** チェック ボックス、**許可**列。

    ![ELMAH フォルダーのアクセス許可](setting-folder-permissions/_static/image3.png)

    (表示されない場合**DefaultAppPool**で、**グループまたはユーザー名**一覧で、おそらくメソッドを使用していくつかその他のこのチュートリアルでは指定されているコンピューターに IIS および ASP.NET 4 を設定します。 その場合は、どのような id は、Contoso University アプリケーションに割り当てられているアプリケーション プールに使用されます。 確認し、その id に書き込みアクセス権を付与します。 このチュートリアルの最後にアプリケーション プールの id に関するリンクを表示します。)クリックして**OK**両方のダイアログ ボックス。

## <a name="retest-error-logging-and-reporting"></a>エラーのログ記録とレポートを再テストします。

もう一度 (無効な URL を要求する) と同様に、エラーを発生させることによってテストし、実行、**エラー ログ**ページ。 この時間、ページで、エラーが表示されます。

![ELMAH エラー ログ ページ](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>まとめ

完了しましたの Contoso University のために必要なタスクをすべてローカル コンピューターに IIS で正常に動作します。 次のチュートリアルでは、サイト パブリックに利用できるようにする Azure にデプロイします。

## <a name="more-information"></a>詳細情報

この例では、Elmah されたログ ファイルを保存できない理由は明らかでした。 問題の原因が; ほど明白ではない場合に IIS のトレースを使用することができます。参照してください[トラブルシューティング失敗した要求を使用してトレース IIS 7 で](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net サイト。

アプリケーション プール id へのアクセス許可を付与する方法についての詳細については、次を参照してください。[アプリケーション プール Id](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)と[ファイル システム Acl を IIS 内のコンテンツをセキュリティで保護](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net サイト。

> [!div class="step-by-step"]
> [前へ](deploying-to-iis.md)
> [次へ](deploying-to-production.md)
