---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: "Visual Studio を使用した ASP.NET Web 展開: フォルダーのアクセス許可の設定 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 19bef5ff97fd5b79135df8ca9bd6bd316594cc5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Visual Studio を使用した ASP.NET Web 展開: フォルダーのアクセス許可の設定
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでのフォルダーのアクセス許可を設定する、 *Elmah*フォルダーに展開された web サイト、アプリケーションは、そのフォルダー内のログ ファイルを作成できるようにします。

Visual Studio 開発サーバー (Cassini) または IIS Express を使用して Visual Studio で web アプリケーションをテストする場合は、自分のユーザー名、アプリケーションが実行されます。 開発用コンピューターの管理者である可能性があり、任意のフォルダー内のファイルを何も操作する全権限を持っています。 IIS 下で実行されるアプリケーションとき、に、サイトが割り当てられているアプリケーション プールに対して定義された id でが実行されます。 これは、通常、アクセス許可が制限されているシステム定義のアカウントです。 既定ではこれがに対する読み取りし、実行アクセス許可、web アプリケーションのファイルとフォルダーが書き込みアクセス権がありません。

アプリケーションが作成するか、web アプリケーションで必要な更新プログラムのファイルは、共通の問題になります。 内の XML ファイルを作成している Elmah Contoso 大学のアプリケーションで、 *Elmah*エラーについての詳細を保存するのにはフォルダーです。 Elmah のようなものを使用しない場合でも、サイトは、ユーザー ファイルをアップロードしたり、サイト内のフォルダーにデータを書き込むその他のタスクを実行を使用できます可能性があります。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。

## <a name="test-error-logging-and-reporting"></a>テストのエラー ログとレポート

どのアプリケーションが正常に動作 IIS で (ただしした Visual Studio でテストする場合) を表示する、通常 Elmah、によってログが記録され、Elmah エラー ログを開いて詳細を表示するエラーが発生することができます。 Elmah できなかった場合、XML ファイルを作成し、エラーの詳細を格納する、空のエラー レポートを参照してください。

ブラウザーを開きに移動`http://localhost/ContosoUniversity`と同様に無効な URL を次に、要求*Studentsxxx.aspx*です。 代わりにシステムが生成したエラー ページを参照してください、 *GenericErrorPage.aspx*ためにのページ、 `customErrors` Web.config ファイルの設定"RemoteOnly"は、ローカル IIS を実行しています。

![HTTP 404 エラー ページ](setting-folder-permissions/_static/image1.png)

今すぐ実行*Elmah.axd*エラー レポートを表示します。 管理者アカウントの資格情報でログインした後 (&quot;admin&quot;と&quot;devpwd&quot;)、Elmah での XML ファイルを作成できなかったために空のエラー ログ ページを参照してください、 *Elmah*フォルダー。

![エラー ログ空](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Elmah フォルダーに対する書き込みアクセス許可の設定

フォルダーのアクセス許可を手動で設定することができますか、自動展開プロセスの部分を行うことができます。 複雑な MSBuild コードでは、自動ことが必要ですし、次の手順を行う方法、手動でのみ、初めてに配置するときに行う必要がある、ためです。 (展開プロセスのこの部分を作成する方法については、次を参照してください[Web 公開 フォルダーのアクセス許可の設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)Sayed Hashimi ブログです。)。

1. **ファイル エクスプ ローラー**に移動*C:\inetpub\wwwroot\ContosoUniversity*です。 右クリックし、 *Elmah*フォルダーを選択**プロパティ**、クリックして、**セキュリティ**タブです。
2. **[編集]** をクリックします。
3. **Elmah のアクセス許可**ダイアログ ボックスで、 **DefaultAppPool**、し、選択、**書き込み** チェック ボックス、**許可**列です。

    ![ELMAH フォルダーに対するアクセス許可](setting-folder-permissions/_static/image3.png)

    (表示されない場合**DefaultAppPool**で、**グループまたはユーザー名**一覧は、おそらく使用したこのチュートリアルで指定した期間よりもその他の方法お使いのコンピューターに IIS および ASP.NET 4 を設定します。 その場合は、どのような id は、Contoso 大学アプリケーションに割り当てられているアプリケーション プールで使用し、その id を書き込みアクセス許可を付与します。 このチュートリアルの最後にアプリケーション プールの id に関するリンクを参照します。)をクリックして**OK**両方のダイアログ ボックス。

## <a name="retest-error-logging-and-reporting"></a>エラー ログとレポートを再テストします。

(無効な URL を要求する) と同じ方法で再度エラーが発生してテストし、実行、**エラー ログ**ページ。 この時間 ページで、エラーが表示されます。

![ELMAH エラー ログ ページ](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>概要

完了しましたの Contoso 大学のために必要なタスクはすべて、ローカル コンピューター上で IIS で正しく動作します。 チュートリアルでは、[次へ] は、サイト パブリックに利用できるようにする Azure にデプロイします。

## <a name="more-information"></a>詳細情報

この例では、Elmah がログ ファイルを保存できない理由は明らかでした。 IIS のトレースを使用するには、問題の原因がすぐに見つからないもの; できない場合参照してください[トラブルシューティング失敗した要求を使用してトレース IIS 7 で](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net サイトです。

アプリケーション プール id へのアクセス許可を付与する方法の詳細については、次を参照してください。[アプリケーション プール Id](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)と[ファイル システム Acl を IIS にコンテンツをセキュリティで保護](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net サイトです。

>[!div class="step-by-step"]
[前へ](deploying-to-iis.md)
[次へ](deploying-to-production.md)
