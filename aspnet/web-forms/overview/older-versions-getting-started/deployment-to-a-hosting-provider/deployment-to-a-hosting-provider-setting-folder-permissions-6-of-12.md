---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: フォルダー権限の設定 - 12 の 6 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 81d5107c7e25a10218d13175c47913c94e9ab3fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827632"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: フォルダー権限の設定 - 12 の 6
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、フォルダーのアクセス許可の設定、 *Elmah*フォルダーにデプロイされた web サイト、アプリケーションは、そのフォルダー内のログ ファイルを作成できるようにします。

Visual Studio 開発サーバー (Cassini) を使用して Visual Studio の web アプリケーションをテストする場合は、自分のユーザー名、アプリケーションが実行されます。 開発用コンピューターの管理者であるほとんどの場合と、任意のフォルダー内の任意のファイルに何もする完全な権限を持ちます。 アプリケーション プールに割り当てられているサイトに対して定義されている id の下で実行されます IIS 下でアプリケーションを実行します。 これは、通常、アクセス許可が制限されているシステム定義のアカウントです。 既定でに読み取りし、web アプリケーションのファイルおよびフォルダーに対する execute 権限が、書き込みアクセス権がありません。

これは、場合は、アプリケーションを作成するか、web アプリケーションで必要な更新プログラムのファイルは、共通が問題になります。 Elmah で XML ファイルの作成、Contoso University アプリケーションで、 *Elmah*エラーの詳細を保存するにはフォルダー。 Elmah のようなものを使用しない場合でも、サイトは、ユーザーがファイルをアップロードまたはサイトのフォルダーにデータを書き込むことが他のタスクを実行できるように可能性があります。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="testing-error-logging-and-reporting"></a>エラー ログとレポートのテスト

どのアプリケーションが動作しない正しく IIS で (ただし、Visual Studio でテストされたときに行った) を表示する、Elmah は、によってログが記録される通常を開き、Elmah のエラー ログの詳細を表示するエラーが発生することができます。 Elmah が XML ファイルを作成し、エラーの詳細を格納することでない場合は、空のエラー レポートを参照してください。

ブラウザーを開きに移動`http://localhost/ContosoUniversity`のように、無効な URL を要求し、 *Studentsxxx.aspx*します。 代わりにシステムが生成したエラー ページを参照してください、 *GenericErrorPage.aspx*ため、ページ、 `customErrors` Web.config ファイルの設定は"RemoteOnly"と、ローカル IIS を実行しています。

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

今すぐ実行*Elmah.axd*エラー レポートを表示します。 Elmah での XML ファイルを作成できなかったために、空のエラー ログのページを参照してください、 *Elmah*フォルダー。

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Elmah フォルダーに対する書き込み権限の設定

フォルダーのアクセス許可を手動で設定することができますか、自動展開プロセスの部分を行うことができます。 複雑な MSBuild コードでは、自動を行う必要があり、のみ最初の時間に行う必要があるため、展開する、このチュートリアルでは、手動で実行する方法だけでは。 (展開プロセスのこの部分を作成する方法については、次を参照してください[で Web の発行フォルダーのアクセス許可の設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)Sayed Hashimi's ブログ。)。

**Windows エクスプ ローラー**に移動します*C:\inetpub\wwwroot\ContosoUniversity*します。 右クリックし、 *Elmah*フォルダーで、**プロパティ**を選び、**セキュリティ**タブ。

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(表示されない場合**DefaultAppPool**で、**グループまたはユーザー名**一覧で、おそらくメソッドを使用していくつかその他のこのチュートリアルでは指定されているコンピューターに IIS および ASP.NET 4 を設定します。 その場合は、どのような id は、Contoso University アプリケーションに割り当てられているアプリケーション プールに使用されます。 確認し、その id に書き込みアクセス権を付与します。 このチュートリアルの最後にアプリケーション プールの id に関するリンクを表示します。)

**[編集]** をクリックします。 **Elmah のアクセス許可**ダイアログ ボックスで、 **DefaultAppPool**を選び、**書き込み** チェック ボックス、**許可**列。

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

クリックして**OK**両方のダイアログ ボックス。

## <a name="retesting-error-logging-and-reporting"></a>エラー ログとレポートを再テスト

もう一度 (無効な URL を要求する) と同様に、エラーを発生させることによってテストし、実行、**エラー ログ**ページ。 この時間、ページで、エラーが表示されます。

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

記述することも必要なアクセス許可で、*アプリ\_データ*フォルダー、そのフォルダーに SQL Server Compact データベース ファイルがあるこれらのデータベースのデータを更新できるようにするためです。 その場合は、ただし、お持ちで、展開プロセスが自動的に書き込みアクセス許可を設定するために特別なことを行うには*アプリ\_データ*フォルダー。

完了しましたの Contoso University のために必要なタスクをすべてローカル コンピューターに IIS で正常に動作します。 次のチュートリアルでは、サイト パブリックに利用できるようにするホスティング プロバイダーへのデプロイします。

## <a name="more-information"></a>説明

この例では、Elmah されたログ ファイルを保存できない理由は明らかでした。 問題の原因が; ほど明白ではない場合に IIS のトレースを使用することができます。参照してください[トラブルシューティング失敗した要求を使用してトレース IIS 7 で](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net サイト。

アプリケーション プール id へのアクセス許可を付与する方法についての詳細については、次を参照してください。[アプリケーション プール Id](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)と[ファイル システム Acl を IIS 内のコンテンツをセキュリティで保護](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net サイト。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
