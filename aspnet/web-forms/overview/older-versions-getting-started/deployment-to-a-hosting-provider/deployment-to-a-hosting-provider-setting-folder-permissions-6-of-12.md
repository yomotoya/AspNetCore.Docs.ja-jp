---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: フォルダー アクセス許可の設定 - 12 の 6 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 573e75221a1c0018bded7544e584b0c75f47d607
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: フォルダー アクセス許可の設定 - 12 の 6
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでのフォルダーのアクセス許可を設定する、 *Elmah*フォルダーに展開された web サイト、アプリケーションは、そのフォルダー内のログ ファイルを作成できるようにします。

Visual Studio 開発サーバー (Cassini) を使用して Visual Studio で web アプリケーションをテストする場合は、自分のユーザー名、アプリケーションが実行されます。 開発用コンピューターの管理者である可能性があり、任意のフォルダー内のファイルを何も操作する全権限を持っています。 IIS 下で実行されるアプリケーションとき、に、サイトが割り当てられているアプリケーション プールに対して定義された id でが実行されます。 これは、通常、アクセス許可が制限されているシステム定義のアカウントです。 既定ではこれがに対する読み取りし、実行アクセス許可、web アプリケーションのファイルとフォルダーが書き込みアクセス権がありません。

アプリケーションが作成するか、web アプリケーションで必要な更新プログラムのファイルは、共通の問題になります。 内の XML ファイルを作成している Elmah Contoso 大学のアプリケーションで、 *Elmah*エラーについての詳細を保存するのにはフォルダーです。 Elmah のようなものを使用しない場合でも、サイトは、ユーザー ファイルをアップロードしたり、サイト内のフォルダーにデータを書き込むその他のタスクを実行を使用できます可能性があります。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="testing-error-logging-and-reporting"></a>エラー ログとレポートのテスト

どのアプリケーションが正常に動作 IIS で (ただしした Visual Studio でテストする場合) を表示する、通常 Elmah、によってログが記録され、Elmah エラー ログを開いて詳細を表示するエラーが発生することができます。 Elmah できなかった場合、XML ファイルを作成し、エラーの詳細を格納する、空のエラー レポートを参照してください。

ブラウザーを開きに移動`http://localhost/ContosoUniversity`と同様に無効な URL を次に、要求*Studentsxxx.aspx*です。 代わりにシステムが生成したエラー ページを参照してください、 *GenericErrorPage.aspx*ためにのページ、 `customErrors` Web.config ファイルの設定"RemoteOnly"は、ローカル IIS を実行しています。

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

今すぐ実行*Elmah.axd*エラー レポートを表示します。 Elmah での XML ファイルを作成できなかったために空のエラー ログ ページを参照してください、 *Elmah*フォルダー。

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Elmah フォルダーに対する書き込み権限の設定

フォルダーのアクセス許可を手動で設定することができますか、自動展開プロセスの部分を行うことができます。 複雑な MSBuild コードでは、自動ことが必要ですし、展開するだけ最初の時間に行う必要があるため、このチュートリアルでは、それを手動で実行する方法だけではします。 (展開プロセスのこの部分を作成する方法については、次を参照してください[Web 公開 フォルダーのアクセス許可の設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)Sayed Hashimi ブログです。)。

**Windows エクスプ ローラー**に移動*C:\inetpub\wwwroot\ContosoUniversity*です。 右クリックし、 *Elmah*フォルダーを選択**プロパティ**、クリックして、**セキュリティ**タブです。

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(表示されない場合**DefaultAppPool**で、**グループまたはユーザー名**一覧は、おそらく使用したこのチュートリアルで指定した期間よりもその他の方法お使いのコンピューターに IIS および ASP.NET 4 を設定します。 その場合は、どのような id は、Contoso 大学アプリケーションに割り当てられているアプリケーション プールで使用し、その id を書き込みアクセス許可を付与します。 このチュートリアルの最後にアプリケーション プールの id に関するリンクを参照します。)

**[編集]** をクリックします。 **Elmah のアクセス許可**ダイアログ ボックスで、 **DefaultAppPool**、し、選択、**書き込み** チェック ボックス、**許可**列です。

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

をクリックして**OK**両方のダイアログ ボックス。

## <a name="retesting-error-logging-and-reporting"></a>エラー ログとレポートを再テスト

(無効な URL を要求する) と同じ方法で再度エラーが発生してテストし、実行、**エラー ログ**ページ。 この時間 ページで、エラーが表示されます。

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

記述することも必要なアクセス許可で、*アプリ\_データ*フォルダー、そのフォルダーに SQL Server Compact データベース ファイルがあるし、それらのデータベースのデータを更新できるようにするためです。 その場合は、ただし、する必要はありません、展開プロセスが自動的に書き込みアクセス許可を設定の追加は何も操作、*アプリ\_データ*フォルダーです。

完了しましたの Contoso 大学のために必要なタスクはすべて、ローカル コンピューター上で IIS で正しく動作します。 チュートリアルでは、[次へ] はサイト パブリックに利用できるようにするホスティング プロバイダーに配置します。

## <a name="more-information"></a>説明

この例では、Elmah がログ ファイルを保存できない理由は明らかでした。 IIS のトレースを使用するには、問題の原因がすぐに見つからないもの; できない場合参照してください[トラブルシューティング失敗した要求を使用してトレース IIS 7 で](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net サイトです。

アプリケーション プール id へのアクセス許可を付与する方法の詳細については、次を参照してください。[アプリケーション プール Id](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)と[ファイル システム Acl を IIS にコンテンツをセキュリティで保護](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net サイトです。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
