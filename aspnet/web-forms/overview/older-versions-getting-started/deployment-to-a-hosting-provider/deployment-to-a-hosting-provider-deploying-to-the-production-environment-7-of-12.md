---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 7 - 運用環境に展開する |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ca7aa9070da98b8790ed8791dd009580fc6a4191
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835848"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 7 - 運用環境に展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、ホスティング プロバイダーを持つアカウントを設定し、ASP.NET のデプロイを Visual Studio の 1 回のクリックを使用して実稼働環境に web アプリケーションが機能を公開します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="selecting-a-hosting-provider"></a>ホスティング プロバイダーの選択

Contoso University アプリケーションとこのチュートリアル シリーズでは、ASP.NET 4 と Web 配置をサポートするプロバイダーが必要です。 特定のホスティング企業は、チュートリアルでは、ライブ web サイトへのデプロイの包括的なエクスペリエンスを示す可能性がありますように選択されました。 各ホスティング プロバイダーのさまざまな機能を提供して、サーバーへのデプロイの経験がやや異なります。 ただし、このチュートリアルで説明されているプロセスは、全体的なプロセスの一般的なものです。 このチュートリアルでは、Cytanium.com で使用するホスティング プロバイダーは、使用可能な次のいずれかとを承認または推奨、このチュートリアルでは、その使用は構成されません。

ホスティング プロバイダーを選択する準備ができたら、機能と価格を比較できます、[プロバイダーのギャラリー](https://www.microsoft.com/web/hosting) Microsoft.com/web サイト上です。

## <a name="creating-an-account"></a>アカウントを作成します。

選択したプロバイダーのアカウントを作成します。 場合のサポート、完全な SQL Server データベースは、追加された余分なこのチュートリアルでは、用に選択する必要はありませんの必要になる、 [SQL Server への移行](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)このシリーズの後半のチュートリアル。

これらのチュートリアルについては、新しいドメイン名を登録する必要はありません。 プロバイダーによって、サイトに割り当てられている一時的な URL を使用して展開の成功を確認するテストすることができます。

アカウントが作成された後に通常を展開し、サイトを管理するために必要なすべての情報を含むウェルカム メールが表示されます。 ホスティング プロバイダーに送信する情報はここに表示される内容のようになります。 新しいアカウント所有者に送信される Cytanium ウェルカム メールには、次の情報が含まれています。

- サイトの設定を管理することができます、プロバイダーのコントロール パネル サイトの URL。 ウェルカム メールを簡単に参照のこの部分では、ID と指定したパスワードが含まれます。 (両方が変更されて次の図のデモの値。)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- 既定の .NET Framework のバージョンとそれを変更する方法についての情報。 多くホスティング サイト既定 2.0 では、2.0、3.0、または 3.5 を .NET Framework を対象とする ASP.NET アプリケーションで動作します。 Contoso University が、.NET Framework 4 アプリケーションがあるこの設定を変更するためです。 (ASP.NET 4.5 のアプリケーションの .NET 4.0 の設定を使用します)。

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Web サイトへのアクセスに使用できる一時的な URL です。 このアカウントが作成されたときに、"contosouniversity.com"は、既存のドメイン名として入力されました。 そのため、一時的な URL は`http://contosouniversity.com.vserver01.cytanium.com`します。

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- データベース、およびそれらにアクセスするために必要な接続文字列を設定する方法の詳細情報:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- ツールと、サイトの展開の設定について説明します。 (Cytanium から受信した電子メールについても触れます WebMatrix で、ここでは省略されます。)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>.NET Framework のバージョンの設定

Cytanium ウェルカム メールには、.NET Framework のバージョンを変更する方法の手順へのリンクが含まれています。 これらの手順では、Cytanium コントロール パネルから実行するこれについて説明します。 他のプロバイダーに異なって表示される、コントロール パネルのサイトがあるかを別の方法でこれを行うことを指示する場合があります。

コントロール パネルの URL に移動します。 ユーザー名とパスワードを使用してログインした、コントロール パネルが表示されます。

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

**をホストしているスペース**ボックス Web アイコンの上にポインターを保持し、 **Web サイト** メニューから。

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

**Websites**ボックスで、 **contosouniversity.com** (アカウントを作成したときに使用したサイトの名前)。

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

**Web サイトのプロパティ**ボックスで、選択、**拡張**タブ。

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

ASP.NET からの変更**2.0 の統合パイプライン**に**4.0 (統合されたパイプライン)**、 をクリックし、**更新**。

## <a name="publishing-to-the-hosting-provider"></a>ホスティング プロバイダーへの発行

ホスティング プロバイダーからウェルカム メールには、すべてのプロジェクトを発行するために必要な設定が含まれていて、発行プロファイルにその情報を手動で入力することができます。 プロバイダーへのデプロイを構成するのには、簡単かつ少ないエラーが発生しやすいメソッドを使用しますが、: ダウンロードします、 *.publishsettings*ファイルし、発行プロファイルにインポートします。

ブラウザーで Cytanium のコントロール パネルに移動し、選択**Web**選び**Web サイト。**

![コントロール パネルの Web サイトを選択します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

選択、 **contosouniversity.com** web サイト。

![Contosouniversity.com を選択すると、コントロール パネル](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

選択、 **Web 公開**タブ。

![コントロール パネル Web の発行 タブ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Web の発行ユーザー名とパスワードを入力して使用する資格情報を作成します。 コントロール パネルへのログオンに使用すると同じ資格情報を入力することができます。 クリックして**を有効にする**します。

![コントロール パネルの 発行の資格情報を作成します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

クリックして**この web サイトの発行プロファイルのダウンロード**します。

![発行プロファイルのコントロール パネルのダウンロード](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

開くか、ファイルを保存するメッセージが表示されたら、それを保存します。

![発行プロファイル ファイルを保存します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

**ソリューション エクスプ ローラー** Visual Studio では、ContosoUniversity プロジェクトを右クリックして**発行**します。 **Web の発行**のダイアログ ボックスが表示されます、**プレビュー**タブで、**テスト**プロファイルを選択するために使用する最後のプロファイルであるためです。

選択、**プロファイル** タブをクリックして**インポート**します。

![公開 Web ウィザードのインポート ボタン](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

**発行設定のインポート**ダイアログ ボックスで、 *.publishsettings*ファイルをダウンロードして、クリックして**オープン**します。 ウィザードでは、すべての入力フィールドに接続 タブに進みます。

![公開 Web ウィザードの [接続] タブ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings ファイルが宛先 URL ボックスにサイトの計画的な永続的な URL を配置するが、そのドメインをまだ購入していない場合は場合に、一時的な URL で値を置き換えます。 この例では、URL は *[ http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com)します。* このボックスの唯一の目的では、ブラウザーに自動的に開きます後に正常にデプロイ後にどのような URL を指定します。 場合は、空白のみの結果は、ブラウザーは、デプロイ後に自動的に開始されませんが。

クリックして**接続の検証**設定が正しいことと、サーバーに接続できることを確認します。 前述のよう、緑色のチェック マークは、接続が成功したことを確認します。

接続の検証をクリックすると、**証明書エラー**  ダイアログ ボックス。 を行った場合は、サーバー名が期待どおりであることを確認します。 場合は、選択**Visual Studio の今後のセッションにこの証明書を保存**クリック**Accept**します。 (このエラーは、ホスティング プロバイダーをデプロイしている URL の SSL 証明書を購入する必要を回避するために選択したことを意味します。 有効な証明書を使用してセキュリティで保護された接続を確立する場合は、連絡、ホスティング プロバイダー)。

![証明書のエラー](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

**[次へ]** をクリックします。

**データベース**のセクション、**設定** タブで、同じ入力発行プロファイルのテスト用に入力した値。 ドロップダウン リストでは、必要な接続文字列があります。

- 接続文字列 ボックスで**SchoolContext、** を選択します `Data Source=|DataDirectory|School-Prod.sdf`
- **SchoolContext**を選択します**適用の Code First Migrations**します。
- 接続文字列 ボックスで**DefaultConnection**を選択します `Data Source=|DataDirectory|aspnet-Prod.sdf`
- **DefaultConnection**のままに**Update database**オフにします。

![発行ウィザードの設定 タブ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

**[次へ]** をクリックします。

**プレビュー** ] タブで [**プレビューの開始**にコピーされるファイルの一覧を参照してください。 ローカル コンピューターの IIS にデプロイされたときに説明するものと同じリストを参照してください。

発行する前に、Web.Production.config 変換ファイルが適用できるようにプロファイルの名前を変更します。 選択、**プロファイル** タブでをクリックし、**プロファイルの管理**します。

![公開 Web ウィザードの プロファイルの管理](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

**Web の発行プロファイルの編集** ダイアログ ボックス、実稼働プロファイルを選択し、をクリックして**の名前を変更**、および運用環境にプロファイル名を変更します。 クリックして**閉じる**します。

![Web の発行プロファイル ダイアログ ボックスを編集します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

**[発行]** をクリックします。

アプリケーションは、ホスティング プロバイダーに発行されます。 結果を示しています、**出力**ウィンドウ。

![デプロイ後に出力ウィンドウ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

入力した URL にブラウザーが自動的に開きます**送信先 URL**ボックスに、**接続**のタブ、 **Web の発行**ウィザード。 Visual Studio で、サイトを実行すると、ここではない"(Test)"として同じのホーム ページを参照してくださいまたはタイトル バーに"(Dev)"環境のインジケーター。 これが示す環境インジケーター *Web.config*変換が正常に動作します。

> [!NOTE]
> まだ見出しに"(Test)"を表示、削除、 *obj*フォルダーから ContosoUniversity プロジェクトと再デプロイします。 ソフトウェアのプレリリース バージョンは、以前に適用された変換ファイル (Web.Test.config) 可能性があります適用されるもう一度が、実稼働プロファイルを使用しています。


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

データベースへのアクセスを原因となるページを実行する前に、Elmah が発生したエラーを記録できることを確認します。

## <a name="setting-folder-permissions-for-elmah"></a>Elmah フォルダー アクセス許可の設定

このシリーズで前のチュートリアルから留意する必要は、アプリケーションが Elmah でエラー ログ ファイルを格納する場所、アプリケーションでのフォルダーに対する書き込みアクセス許可を持つことを確認する必要があります。 デプロイしたときに IIS をローカル コンピューターに、これらのアクセス許可が手動で設定します。 このセクションでは、Cytanium にアクセス許可を設定する方法を確認します。 (一部のホスティング プロバイダーをこれを行う有効にできません。 書き込みのアクセス許可を持つ 1 つまたは複数の事前定義されたフォルダーを提供することがあります。 その場合は必要がありますを指定したフォルダーを使用するアプリケーションを変更する。)

Cytanium コントロール パネルの フォルダーのアクセス許可を設定できます。 コントロール パネルの URL を選択し**ファイル マネージャー**します。

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

**ファイル マネージャー**ボックスで、 **contosouniversity.com**し**wwwrooot**にアプリケーションのルート フォルダーを参照してください。 横に錠前のアイコンをクリックして**Elmah**します。

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

**ファイル**/**フォルダーのアクセス許可**ウィンドウで、**読み取り**と**書き込み**のチェック ボックス**contosouniversity.com**クリック**アクセス許可の設定**します。

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Elmah にへの書き込みアクセスがあることを確認、 *Elmah*フォルダーで、エラーの原因と、Elmah のエラー レポートを表示します。 ように、無効な URL を要求*Studentsxxx.aspx*します。 前に、わかり、 *GenericErrorPage.aspx*ページ。 をクリックして、 **Log Out**リンク、および実行し、 *Elmah.axd*します。 取得する、**ログで**ことを検証しますが最初に、ページ、 *Web.config*変換は、Elmah の承認を正常に追加します。 ログインした後だけに発生したエラーを表示するレポートを参照してください。

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>実稼働環境でのテスト

実行、**学生**ページ。 アプリケーションは、最初に、データベースを作成する Code First Migrations をトリガーする School データベースにアクセスしようとします。 少しの遅延の後に、ページが表示されたら、受講者がないことを示します。

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

実行、 **Instructors**シード データが、データベースに、インストラクターのデータを正常に挿入することを確認します。

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

データベースの更新が、運用環境で作業しますが、通常たくない、実稼働データベースにテスト データを入力することを確認するテスト環境で実行したようです。 このチュートリアルでは、テストで行った同じメソッドを使用します。 そのデータベースを検証するメソッドを検出する実際のアプリケーションで更新が成功したテスト データを実稼働データベースに導入することがなく。 一部のアプリケーションでは、何か追加し、削除するは実用的場合があります。

学生を追加しで入力したデータを表示、**学生**ページをデータベース内のデータを更新することを確認します。

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

検証を選択して承認規則が正常に動作する**更新クレジット**から、**コース**メニュー。 **ログで**ページが表示されます。 管理者アカウントの資格情報を入力し、をクリックして**ログで**、および**更新クレジット**ページが表示されます。

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

ログインが成功した場合、**更新クレジット**ページが表示されます。 これは、(1 人の管理者アカウント) で ASP.NET メンバーシップ データベースが正常にデプロイされたことを示します。

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

これで正常にデプロイして、サイトをテストし、入手可能になったパブリック インターネット経由で。

## <a name="creating-a-more-reliable-test-environment"></a>信頼性の高いテスト環境を作成します。

説明したように、[テスト環境に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルでは、最も信頼性の高いテスト環境は運用環境のアカウントと同様にしているホスティング プロバイダーに 2 つ目のアカウントになります。 これは、2 つ目のホスティング アカウントにサインアップする必要がありますので、テスト環境としてローカルの IIS を使用してよりも高価になります。 実稼働サイトのエラーや障害が防止されますが、場合は、コストに値するであります。

作成とテスト アカウントにデプロイ プロセスのほとんどは、どのような実行した運用環境に配置と同様です。

- 作成、 *Web.config*変換ファイル。
- ホスティング プロバイダーでは、アカウントを作成します。
- 新しい発行プロファイルを作成し、テスト アカウントにデプロイします。

### <a name="preventing-public-access-to-the-test-site"></a>テスト サイトへのパブリック アクセスを防止

テスト アカウントの重要な考慮事項は、インターネット上でライブになりますが、それを使用する公開したくないことです。 プライベート サイトを保持するには、次の方法の 1 つ以上を使用できます。

- テストに使用する IP アドレスからのみテスト サイトへのアクセスを許可するファイアウォール規則を設定するホスティング プロバイダーにお問い合わせください。
- パブリック サイトの URL のようなしないように URL を偽装します。
- 使用して、 *robots.txt*ファイルを検索エンジンがクロール テスト サイトとレポートのリンクを検索結果で確認します。

これらのメソッドの最初の明らかに、最も安全ですが、するための手順は各ホスティング プロバイダーに固有でありはこのチュートリアルでは説明されません。 テスト アカウントの URL を参照する、IP アドレスのみを許可するホスティング プロバイダーに配置することは場合、理論的にがない検索エンジンをクロールすることを気にします。 その場合でも、展開しますが、 *robots.txt*場合に、そのファイアウォール規則が誤ってオフ、ファイルのバックアップとしてをお勧めします。

*Robots.txt*ファイルがプロジェクト フォルダーに移動してことで、次のテキストが必要があります。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent`行は、ファイル内のルールがすべて検索エンジン web クローラー (ロボット) に適用される検索エンジンに指示し、`Disallow`行では、サイトのページをクロールないことを指定します。

検索エンジンを運用環境のデプロイからこのファイルを除外する必要があるため、実稼働サイトをカタログ化する可能性がありますようにします。 そのためには、次を参照してください。**できますしないようにする特定のファイルまたはフォルダーの展開からでしょうか。** で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)します。 運用環境の発行プロファイルに対してのみ、除外を指定することを確認します。

2 つ目のホスト アカウントの作成は、テスト環境は必要ありませんが、追加費用価値がありますを使用するアプローチです。 以下のチュートリアルでは、テスト環境として IIS を使用して引き続きします。

次のチュートリアルでは、アプリケーション コードを更新し、変更をテストおよび運用環境に配置します。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
