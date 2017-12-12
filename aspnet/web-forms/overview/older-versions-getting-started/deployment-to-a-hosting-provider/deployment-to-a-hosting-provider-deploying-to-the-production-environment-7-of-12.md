---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 7 - 実稼働環境に展開する |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ad44968975b7929f5b0f70334deabc7238797402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 7 - 実稼働環境に展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、ホスティング プロバイダーを持つアカウントを設定して配置、ASP.NET web アプリケーションを Visual Studio の 1 回のクリックを使用して実稼働環境には、機能を公開します。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="selecting-a-hosting-provider"></a>ホスティング プロバイダーの選択

Contoso 大学アプリケーションとこのチュートリアルの系列では、ASP.NET 4 および Web Deploy をサポートするプロバイダーが必要です。 チュートリアルでは、でしたライブ web サイトを展開する完全なエクスペリエンスを示すように、特定のホスティング企業が選択されました。 各ホスティング企業が、さまざまな機能を提供し、ある程度のサーバーに展開する場合の動作が異なります。 ただし、このチュートリアルで説明するプロセスは通常、全体的なプロセスです。 Cytanium.com、このチュートリアルに使用されるホスティング プロバイダーは、使用可能な次のいずれかと、このチュートリアルで使用するにはなりません承認または推奨します。

ホスティング プロバイダーを選択する準備ができたら、機能との価格を比較することができます、[プロバイダーのギャラリー](https://www.microsoft.com/web/hosting) Microsoft.com/web サイト上。

## <a name="creating-an-account"></a>アカウントを作成します。

選択したプロバイダーのアカウントを作成します。 場合のサポート、完全な SQL Server データベースは、追加された余分なこのチュートリアルを選択する必要はありませんの必要になります、 [SQL Server への移行](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)このシリーズの後でチュートリアルです。

これらのチュートリアルについては、新しいドメイン名を登録する必要はありません。 プロバイダーによって、サイトに割り当てられている一時的な URL を使用して展開の成功を確認するテストすることができます。

アカウントが作成されたら、通常を展開し、サイトを管理するために必要なすべての情報を含む「ようこそ」電子メールを受信します。 ホスティング プロバイダーに送信する情報はここに表示される内容のようになります。 新しいアカウント所有者に送信される Cytanium ようこそ電子メールには、次の情報が含まれています。

- サイトの URL をプロバイダーのコントロール パネル、サイトの設定を管理できます。 ようこそ電子メールを簡単に参照のこの部分では、ID と指定したパスワードが含まれています。 (両方が変更されて次の図のデモ値にします。)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- 既定の .NET Framework のバージョンとそれを変更する方法についてはします。 多くホスティング サイト既定 2.0、2.0、3.0、または 3.5、.NET Framework を対象とする ASP.NET アプリケーションで動作します。 Contoso 大学が .NET Framework 4 アプリケーションでは、この設定を変更する必要があるためです。 (ASP.NET 4.5 アプリケーションの .NET 4.0 の設定を使用します)。

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Web サイトにアクセスに使用できる一時的な URL です。 このアカウントが作成されたときに、"contosouniversity.com"は、既存のドメイン名として入力されました。 そのため、一時的な URL は`http://contosouniversity.com.vserver01.cytanium.com`します。

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- 詳細については、データベースとそれらにアクセスするために必要な接続文字列を設定する方法。

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- ツールと設定については、サイトを展開するためです。 (Cytanium から受信した電子メールについても触れます WebMatrix で、ここでは省略します。)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>.NET Framework のバージョンを設定

Cytanium ようこそ電子メールには、.NET Framework のバージョンを変更する方法の手順へのリンクが含まれています。 これらの手順では、Cytanium コントロール パネルから行うことこれを説明します。 その他のプロバイダーが異なって表示される、コントロール パネルのサイトともこれを行うには異なる方法で指示することがあります。

コントロール パネルの URL に移動します。 ユーザー名とパスワードを使用してログインには、コントロール パネルでを参照してください。

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

**をホストしているスペース**ボックス Web アイコンの上にポインターを保持し、 **Websites**  メニューからです。

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

**Websites**ボックスで、クリックして**contosouniversity.com** (アカウントを作成したときに使用したサイトの名前)。

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

**Web サイトのプロパティ**ボックスで、選択、**拡張**タブです。

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

ASP.NET からの変更**2.0 の統合パイプライン**に**4.0 (統合パイプライン)**、順にクリック**更新**です。

## <a name="publishing-to-the-hosting-provider"></a>ホスティング プロバイダーへの発行

ホスティング プロバイダーからのようこそ電子メールには、すべてのプロジェクトを発行するために必要な設定が含まれていて、発行プロファイルにその情報を手動で入力することができます。 プロバイダーへの展開を構成する、簡単かつ以下のエラーが発生しやすいメソッドを使用します: ダウンロード、 *.publishsettings*ファイルし、発行プロファイルにインポートします。

ブラウザーで Cytanium コントロール パネルに移動し、選択**Web**し、 **Web サイトです。**

![コントロール パネルの Web サイトを選択します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

選択、 **contosouniversity.com** web サイトです。

![Contosouniversity.com を選択すると、コントロール パネル](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

選択、 **Web 公開**タブです。

![コントロール パネル Web の発行 タブ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Web ユーザー名とパスワードを入力して発行に使用する資格情報を作成します。 コントロール パネルへのログオンに使用する同じ資格情報を入力することができます。 をクリックして**を有効にする**です。

![コントロール パネルの 発行の資格情報を作成します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

をクリックして**この web サイトの発行プロファイルのダウンロード**です。

![発行プロファイルのコントロール パネルのダウンロード](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

開くか、ファイルを保存するメッセージが表示されたら、それを保存します。

![発行プロファイル ファイルを保存します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

**ソリューション エクスプ ローラー** Visual Studio で、ContosoUniversity プロジェクトを右クリックし、選択**発行**です。 **Web の発行**上 ダイアログ ボックスを開きます、**プレビュー**タブで、**テスト**プロファイルを選択するために使用する最後のプロファイルであるためです。

選択、**プロファイル** タブをクリックして**インポート**です。

![公開 Web ウィザードのインポート ボタン](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

**発行設定のインポート**ダイアログ ボックスで、 *.publishsettings*ファイルをダウンロードして、をクリックして**開く**です。 ウィザードは、すべての入力フィールドで、[接続] タブに進みます。

![公開 Web ウィザードの [接続] タブ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings ファイルは、送信先 URL ボックスに、サイトの計画的な永続的な URL をなりますが、そのドメインをまだ購入していない場合は場合、は、一時的な URL を使用して値を置き換えます。 この例では、URL は *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com)です。*このボックスの唯一の目的では、ブラウザーに自動的に開きます後に正常に展開後にどのような URL を指定します。 空のまま、のみの結果がブラウザーを展開後に自動的に起動しません。

をクリックして**接続の検証**設定が正しいことと、サーバーに接続できることを確認します。 前述の緑のチェック マークは、接続が成功したことを確認します。

接続の検証 をクリックするときに表示される、**証明書エラー**  ダイアログ ボックス。 を行う場合は、サーバー名が予期したとおりであることを確認します。 場合は、選択**Visual Studio の今後のセッションにこの証明書を保存** をクリック**Accept**です。 (このエラーを展開している URL の SSL 証明書を購入する必要を避けるために、ホスティング プロバイダーが選択されていることを意味します。 有効な証明書を使用してセキュリティで保護された接続を確立する場合は、連絡、ホスティング プロバイダーです。)

![証明書のエラー](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

**[次へ]**をクリックします。

**データベース**のセクションで、**設定** タブで、同じ入力発行プロファイルのテスト用に入力した値です。 ドロップダウン リストに必要な接続文字列があります。

- 接続文字列 ボックスで**SchoolContext、**選択`Data Source=|DataDirectory|School-Prod.sdf`
- **SchoolContext****適用の Code First Migrations**です。
- 接続文字列 ボックスで**DefaultConnection**を選択`Data Source=|DataDirectory|aspnet-Prod.sdf`
- **DefaultConnection**のままにして**データベースの更新**オフにします。

![公開 Web ウィザードの設定 タブ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

**[次へ]**をクリックします。

**プレビュー**  タブで、をクリックして**開始プレビュー**コピーされるファイルの一覧を表示します。 先ほど見たとき、ローカル コンピューター上の IIS に配置されたものと同じ一覧が参照してください。

発行する前に、Web.Production.config 変換ファイルが適用できるようにプロファイルの名前を変更します。 選択、**プロファイル** タブでをクリックし、**プロファイルの管理**です。

![公開 Web ウィザードの プロファイルの管理](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

**Web の発行プロファイルの編集** ダイアログ ボックス、実稼働のプロファイルを選択し、をクリックして**の名前を変更**、実稼働環境に、プロファイルの名前を変更します。 をクリックして**閉じる**です。

![Web 発行プロファイル ダイアログ ボックスを編集します。](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

**[発行]**をクリックします。

アプリケーションは、ホスティング プロバイダーに発行されます。 結果を示しています、**出力**ウィンドウです。

![配置後の出力 ウィンドウ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

入力した URL をブラウザーが自動的に開く**送信先 URL**ボックスに、**接続**のタブ、 **Web の発行**ウィザード。 Visual Studio で、サイトを実行すると、ここではない「(テスト)」として同じのホーム ページを表示またはタイトル バーに「(開発)」環境インジケーター。 これが示す環境インジケーター *Web.config*変換が正常に動作します。

> [!NOTE]
> まだ見出しに"(Test)"を表示、削除、 *obj*フォルダーから ContosoUniversity プロジェクトと再展開します。 ソフトウェアのプレリリース バージョンは、以前に適用された変換ファイル (Web.Test.config) 適用可能性があります取得もう一度が、実稼働のプロファイルを使用しています。


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

データベースへのアクセスを原因となるページを実行する前に、Elmah が発生したエラーをログに記録できることを確認します。

## <a name="setting-folder-permissions-for-elmah"></a>Elmah のフォルダーのアクセス許可の設定

前のチュートリアルの次の系列に留意するようアプリケーションが Elmah がエラー ログ ファイルを格納する場所、アプリケーションで、フォルダーに対する書き込みアクセス許可を持つことを確認する必要があります。 展開したときに IIS をローカル コンピューターに、これらのアクセス許可が手動で設定します。 このセクションでは、Cytanium にアクセス許可を設定する方法が表示されます。 (一部のホスティング プロバイダーを有効にできません。 書き込みアクセス許可を持つ 1 つまたは複数の事前定義されたフォルダーを提供している可能性があります。 そのケースで必要がありますを指定したフォルダーを使用して、アプリケーションを変更します。)

Cytanium コントロール パネルの フォルダーのアクセス許可を設定することができます。 コントロールへの URL を選択して移動**ファイル マネージャー**です。

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

**ファイル マネージャー**ボックスで、 **contosouniversity.com**し**wwwrooot**にアプリケーションのルート フォルダーを参照してください。 南京錠のアイコンをクリックして横に**Elmah**です。

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

**ファイル**/**フォルダーのアクセス許可**ウィンドウで、**読み取り**と**書き込み**のチェック ボックスを**contosouniversity.com**  をクリック**アクセス許可の設定**です。

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Elmah への書き込みアクセスがあることを確認、 *Elmah*フォルダーで、エラーの原因と Elmah エラー レポートを表示します。 要求するように無効な URL *Studentsxxx.aspx*です。 前に、表示される、 *GenericErrorPage.aspx*ページ。 クリックして、 **Log Out**リンク、および実行し、 *Elmah.axd*です。 取得する、**ログで**を検証するかを最初に、ページ、 *Web.config*変換が正常に Elmah 承認を追加します。 ログインした後に発生するエラーを表示するレポートを参照してください。

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>実稼働環境でのテスト

実行、**受講者**ページ。 アプリケーションでは、最初に、データベースを作成する Code First Migrations をトリガーする School データベースにアクセスしようとしています。 現在の遅延後に、ページが表示されたら、受講者がないことを示します。

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

実行、**講習においてインストラクター**シード データが、データベースのインストラクター データを正常に挿入することを確認します。

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

テスト環境で行ったようを実稼働環境で作業するデータベースの更新が通常たくないテスト データ、実稼働データベースを入力することを確認します。 このチュートリアルでは、テストで行ったのと同じメソッドを使用します。 そのデータベースを検証するメソッドを検出することができます、実際のアプリケーションの更新が成功した、実稼働データベースにテスト データを伴います。 一部のアプリケーションでは、何かを追加し、それを削除することは実用的場合があります。

受講者を追加し、入力したデータを表示、**受講者**ページをデータベース内のデータを更新することを確認します。

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

検証を選択して承認規則が正しく動作している**更新クレジット**から、**コース**メニュー。 **ログで**ページが表示されます。 管理者アカウントの資格情報を入力し、をクリックして**ログで**、および**更新クレジット**ページが表示されます。

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

ログインが成功した場合、**更新クレジット**ページが表示されます。 これは、(1 人の管理者アカウント) の ASP.NET メンバーシップ データベースが正常に展開されたことを示します。

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

これで正常に展開およびテストが、サイトとは使用可能なパブリック インターネット経由でします。

## <a name="creating-a-more-reliable-test-environment"></a>信頼性の高いテスト環境を作成します。

説明に従って、[テスト環境に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルでは、最も信頼性の高いテスト環境には、運用アカウントと同じようには、ホスティング プロバイダーに 2 番目のアカウントがあるとします。 2 つ目のホスト アカウントにサインアップする必要がありますので、テスト環境としてローカルの IIS を使用するよりも高価になります。 実稼働サイト エラー、または故障が防止されますが場合、は、価値のコストであることを決定する可能性があります。

作成して、テスト アカウントへの展開のプロセスのほとんどは、新機能を既にインストールしている実稼働環境に展開すると同様です。

- 作成、 *Web.config*変換ファイル。
- ホスティング プロバイダーには、アカウントを作成します。
- 新しい発行プロファイルを作成し、テスト アカウントに展開します。

### <a name="preventing-public-access-to-the-test-site"></a>テスト サイトへのパブリック アクセス防止

テスト アカウントの重要な考慮事項は、インターネットでは、ライブされますが、これを使用する公開したくないです。 サイトを非公開には、次の方法の 1 つ以上を使用できます。

- テストに使用する IP アドレスからのみテスト サイトへのアクセスを許可するファイアウォール規則を設定するホスティング プロバイダーに問い合わせてください。
- パブリックのサイトの URL に似ていますがないように、URL を偽装します。
- 使用して、 *robots.txt*こと検索エンジンがクロール テスト サイトとレポートのリンクを 検索結果を確認するファイル。

これらのメソッドの最初の明らかに、最も安全ですが、するための手順は、各ホスティング プロバイダーに固有と、このチュートリアルでは扱いませんがします。 した場合は、テスト アカウントの URL を参照する、IP アドレスのみを許可するホスティング プロバイダーに、理論上がない検索エンジンがそれをクロールについて心配します。 その場合でも、展開しますが、 *robots.txt*場合に、そのファイアウォール規則がオフになってしまったこと、ファイルのバックアップとしてことをお勧めします。

*Robots.txt*ファイルがプロジェクト フォルダーに移動し、ことで、次のテキストを持つ必要があります。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent`行は、ファイル内のルールがすべて検索エンジン web クローラー (ロボット) に適用される検索エンジンに指示し、`Disallow`行は、サイトのページをクロールないことを指定します。

おそらくたくを運用環境のデプロイからこのファイルを除外する必要があるため、実稼働サイトをカタログ化する検索エンジンです。 そのためには、次を参照してください。**できますものを除外する特定のファイルまたはフォルダーの展開から?**で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)です。 発行プロファイルの運用環境用にのみ、排他領域を指定することを確認します。

2 つ目のホスト アカウントの作成は、テスト環境は必要ありませんが、追加の経費価値がありますを使用するアプローチです。 次のチュートリアルでは、IIS を使用して、テスト環境に進みます。

チュートリアルでは、[次へ]、アプリケーション コードを更新し、変更をテスト環境や実稼働環境に配置されます。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[次へ](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
