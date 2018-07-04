---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Visual Studio を使用して ASP.NET Web 展開: 実稼働環境に展開する |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 5bc39162bc53dc27230f8ccc55266212a51b6380
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369646"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Visual Studio を使用して ASP.NET Web 展開: 運用環境にデプロイ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、Microsoft Azure アカウントを設定する、ステージングと運用環境を作成およびステージングに ASP.NET web アプリケーションを配置して、Visual Studio の 1 回のクリックを使用して、運用環境の発行機能。

場合は、サード パーティのホスティング プロバイダーにデプロイできます。 このチュートリアルで説明する手順のほとんどは、する点を除いて、各プロバイダーは独自のユーザー インターフェイスのアカウントと web サイトの管理、ホスティング プロバイダーまたは Azure では、同じです。 ホスティング プロバイダーを見つけることができます、[プロバイダーのギャラリー](https://www.microsoft.com/web/hosting) Microsoft.com web サイト。

お知らせ: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しない、このチュートリアル シリーズでトラブルシューティングのページを確認することを確認します。

## <a name="get-a-microsoft-azure-account"></a>Microsoft Azure アカウントを取得します。

Azure アカウントがない場合は、ほんの数分で無料試用版アカウントを作成できます。 詳細については、次を参照してください。 [Azure 無料試用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。

## <a name="create-a-staging-environment"></a>ステージング環境を作成します。

> [!NOTE]
> このチュートリアルを作成したので、Azure App Service には、ステージングと運用環境を作成するためのプロセスの多くを自動化する新しい機能が追加されました。 参照してください[を設定すると、Azure App Service で web アプリのステージング環境](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/)します。


説明したように、[環境のテストのチュートリアルへのデプロイ](deploying-to-iis.md)最も信頼性の高いテスト環境は運用環境の web サイトと同様にしているホスティング プロバイダーの web サイト。 多くのホスティング プロバイダーには、重要な追加の費用に対してこの利点を比較検討する必要が、Azure でステージング アプリとして追加の無料の web アプリを作成することができます。 データベースが必要であり、実稼働データベースの経費をそのため、追加の費用なし] または [最小です。 Azure でお支払いいただきます。 各データベースに対してではなく、使用するデータベース記憶域の量、ステージング環境で使用する追加のストレージの量を最小限になります。

説明したように、[テスト環境のチュートリアルへのデプロイ](deploying-to-iis.md)、ステージング環境と運用環境の 1 つのデータベースに、2 つのデータベースをデプロイすることにします。 それらを分離する場合は、プロセスは同じになりますが、環境ごとに、追加のデータベースを作成し、発行プロファイルを作成するときに、各データベースの正しいコピー先文字列が選択されます。 します。

チュートリアルのこのセクションでは、web アプリとステージング環境では、使用するデータベースを作成し、ステージングへのデプロイを作成し、実稼働環境に展開する前にテストがあります。

> [!NOTE]
> 次の手順では、Azure 管理ポータルを使用して Azure App Service で web アプリを作成する方法を示します。 Azure SDK の最新のバージョンで行うことができますもこのサーバー エクスプ ローラーを使用して、Visual Studio を離れることがなく。 Visual Studio 2013 での発行 ダイアログ ボックスから直接 web アプリを作成することもできます。 詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成します。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. [Azure 管理ポータル](https://manage.windowsazure.com/)、 をクリックして**Websites**、順にクリックします**新規**します。
2. クリックして**web サイト**、 をクリックし、**カスタム作成**します。

    **新規 web サイト - カスタム作成**ウィザードが開きます。 **カスタム作成**ウィザードでは、同時に web サイトとデータベースを作成することができます。
3. **Web サイトの作成**手順と、ウィザードの文字列を入力、 **URL**アプリケーションの一意の URL として使用する ボックスのステージング環境です。 たとえば、ContosoUniversity staging123 (ContosoUniversity ステージングが実行される場合に一意にする最後にランダムな数値を含む) を入力します。

    完全な URL は、ここで入力とテキスト ボックスの横に表示されるサフィックスで構成されます。
4. **リージョン**ドロップダウン リストで、地に最も近いリージョンを選択します。

    この設定では、web アプリが実行されるデータ センターを指定します。
5. **データベース**ドロップダウン リストで、選択**新しい SQL データベースの作成**です。
6. **DB 接続文字列名**ボックスで、既定値をそのまま*DefaultConnection*します。
7. ボックスの下部にある右を指している矢印をクリックします。

    次の図は、 **web サイトの作成** ダイアログ ボックスで、サンプルの値。 URL と入力したリージョンは別になります。

    ![Web サイトを作成します。](deploying-to-production/_static/image1.png)

    ウィザード、**データベース設定を指定**手順。
8. **名前**ボックスに、入力*ContosoUniversity*さらにそれを一意にするなどのランダムな数値*ContosoUniversity123*します。
9. **Server**ボックスで、**新しい SQL Database Server**します。
10. 管理者名とパスワードを入力します。

    既存の名前とパスワードでを入力することはありません。 新しい名前と後、データベースにアクセスするときに使用するには、現在定義しているパスワードを入力してください。
11. **リージョン**ボックスで、web アプリの選択したのと同じリージョンを選択します。

    Web サーバーとデータベース サーバーと同じリージョンに置くことは最適なパフォーマンスを提供し、費用を最小限に抑えられます。
12. 完了することを示すボックスの下部にあるチェック マークをクリックします。

    次の図は、**データベース設定を指定** ダイアログ ボックスで、サンプルの値。 入力した値が異なる場合があります。

    ![データベースの設定手順の新しい web サイト - データベースとともに作成ウィザード](deploying-to-production/_static/image2.png)

    管理ポータルの web サイトのページに戻り、**状態**列は、web アプリが作成されていることを示しています。 (通常、1 分未満)、しばらくしてから、**状態**列は、web アプリが正常に作成されたことを示しています。 自分のアカウントにある web アプリの数を横に表示、左側のナビゲーション バーで、 **Websites**アイコン、およびデータベースの数が横に表示されます、 **SQL データベース**アイコン。

    ![管理ポータルで作成された web サイトの Web サイトのページ](deploying-to-production/_static/image3.png)

    Web アプリ名は、別の図に例のアプリからになります。

## <a name="deploy-the-application-to-staging"></a>ステージング環境にアプリケーションをデプロイします。

Web アプリとステージング環境のデータベースを作成して、プロジェクトをデプロイできます。

> [!NOTE]
> この手順でダウンロードして、発行プロファイルを作成する方法を示します、 *.publishsettings*ファイルで、サード パーティのホスティング プロバイダーも Azure のだけでなく動作します。 最新の Azure SDK することもできます、Visual Studio から直接 Azure に接続し、Azure アカウント内にある web アプリの一覧から選択します。 Visual Studio 2013 ではから Azure にサインインすることができます、 **Web の発行**ダイアログから、または、**サーバー エクスプ ローラー**ウィンドウ。 詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)です。


### <a name="download-the-publishsettings-file"></a>.Publishsettings ファイルをダウンロードします。

1. 先ほど作成した web アプリの名前をクリックします。

    ![ダッシュ ボードに移動するサイトをクリックします。](deploying-to-production/_static/image4.png)
2. **概要**で、**ダッシュ ボード** タブで **発行プロファイルのダウンロード**。

    ![リンクの発行プロファイルをダウンロードします。](deploying-to-production/_static/image5.png)

    この手順では、すべてのアプリケーション、web アプリをデプロイするために必要な設定を含むファイルをダウンロードします。 この情報を手動で入力する必要はありませんので、Visual Studio をこのファイルをインポートします。
3. 保存、 *.publishsettings* Visual Studio からアクセスできるフォルダー内のファイル。

    ![.publishsettings ファイルの保存](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > セキュリティ -、 *.publishsettings*ファイルには、Azure サブスクリプションとサービスの管理に使用される (エンコードされていない) 資格情報が含まれています。 このファイルのセキュリティのベスト プラクティスでは、(たとえば libraries \documents フォルダーにある) でソース ディレクトリの外に一時的に格納し、インポートが完了したら、削除します。 アクセスした悪意のあるユーザー、 *.publishsettings*ファイルは、編集、作成、および Azure サービスを削除します。

### <a name="create-a-publish-profile"></a>発行プロファイルを作成します。

1. Visual Studio で、ContosoUniversity プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニュー。

    **Web の発行**ウィザードが開きます。
2. をクリックして、**プロファイル**タブ。
3. **[インポート]** をクリックします。
4. 移動し、 *.publishsettings*クリックして、以前ダウンロードしたファイルに**オープン**します。

    ![インポートの発行の設定 ダイアログ ボックス](deploying-to-production/_static/image7.png)
5. **接続**] タブで [**接続の検証**設定が正しいことを確認します。

    横に緑色のチェック マークが表示、接続が検証されると、**接続の検証**ボタンをクリックします。

    一部のホスティング プロバイダーをクリックすると**接続の検証**、表示される、**証明書エラー**  ダイアログ ボックス。 を行った場合は、サーバー名が期待どおりであることを確認します。 サーバー名が正しい場合は、選択**Visual Studio の今後のセッションにこの証明書を保存**クリック**Accept**します。 (このエラーは、ホスティング プロバイダーをデプロイしている URL の SSL 証明書を購入する必要を回避するために選択したことを意味します。 有効な証明書を使用してセキュリティで保護された接続を確立する場合は、連絡、ホスティング プロバイダー)。
6. **[次へ]** をクリックします。

    ![接続成功アイコンと [接続] タブで [次へ] ボタンをクリックします。](deploying-to-production/_static/image8.png)
7. **設定**] タブで、展開**ファイル発行オプション**、し、[**アプリからファイルを除外する\_データ フォルダー**します。

    その他のオプションについて**ファイル発行オプション**を参照してください、 [IIS への配置](deploying-to-iis.md)チュートリアル。 画面は、この手順と、次のデータベースの構成手順の結果が、データベースの構成手順の最後に表示されます。
8. **DefaultConnection**で、**データベース**セクションで、メンバーシップ データベースに対するデータベース デプロイを構成します。
9. 1. 選択**Update database**します。

        **リモート接続文字列**すぐ下のボックス**DefaultConnection** .publishsettings ファイルから接続文字列が入力されます。接続文字列がプレーン テキストで格納されている SQL Server 資格情報を含む、 *.pubxml*ファイル。 完全に保存しないようにする場合は、データベースを展開した後、発行プロファイルから削除して、代わりに Azure に保存します。 詳細については、次を参照してください。[保護する方法、ASP.NET データベース接続文字列のソースから Azure にデプロイするときに](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman のブログ。
      2. クリックして**データベース更新を構成する**します。
      3. **データベース更新を構成する**ダイアログ ボックスで、をクリックして**SQL スクリプトの追加**します。
      4. **SQL スクリプトの追加**ボックスに移動、 *aspnet-データ-prod.sql*する前に、ソリューション フォルダーに保存し、スクリプト**オープン**します。
      5. 閉じる、**データベース更新を構成する** ダイアログ ボックス。
10. **SchoolContext**で、**データベース**セクションで、**実行 Code First Migrations (アプリケーションの起動時に実行)** します。

    Visual Studio に表示**Code First Migrations の実行**の代わりに**Update Database**の`DbContext`クラス。 移行ではなく dbDacFx プロバイダーを使用して、使用してアクセスするデータベースを配置するかどうか、`DbContext`クラスを参照してください[移行せず、Code First のデータベースをデプロイする方法でしょうか](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)for Visual Studio Web 配置の faq。MSDN 上の ASP.NET。

    **設定** タブでは、次の例のようになります。

    ![ステージング環境の設定 タブ](deploying-to-production/_static/image9.png)
11. プロファイルを保存して、変更するには、次の手順を実行*ステージング*:

    1. をクリックして、**プロファイル**タブをクリックし、をクリックし、**プロファイルの管理**します。
    2. インポートでは、FTP 用と Web デプロイのための 2 つの新しいプロファイルを作成します。 Web デプロイ プロファイルを構成する: このプロファイルの名前を変更*ステージング*します。

        ![ステージング環境にプロファイル名の変更します。](deploying-to-production/_static/image10.png)
    3. 閉じる、 **Web の発行プロファイルの編集** ダイアログ ボックス。
    4. 閉じる、 **Web の発行**ウィザード。

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>環境のインジケーターの発行プロファイルの変換を構成します。

> [!NOTE]
> このセクションでは、環境のインジケーターの Web.config 変換を設定する方法を示します。 インジケーターがあるため、`<appSettings>`要素では、Azure App Service にデプロイするときに、変換を指定するための別の方法があります。 詳細については、次を参照してください。[を指定する Web.config の設定を Azure で](web-config-transformations.md#watransforms)します。


1. **ソリューション エクスプ ローラー**、展開**プロパティ**、順に展開**PublishProfiles**します。
2. 右クリック*Staging.pubxml*、 をクリックし、**構成変換の追加**します。

    ![ステージング用の構成変換を追加します。](deploying-to-production/_static/image11.png)

    Visual Studio によって作成、 *Web.Staging.config*変換ファイルを開きます。
3. *Web.Staging.config*ファイルの変換を開始した直後に次のコードを挿入`configuration`タグ。

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    ステージング環境の発行プロファイルを使用する場合、この変換は、"Prod"するインジケーターを環境を設定します。 デプロイされた web アプリで、"Contoso University"H1 見出し"(Dev)"などの任意のサフィックスまたは"(Test)"は表示されません。
4. 右クリックし、 *Web.Staging.config*ファイルを**プレビュー変換**を組み込んだ変換が期待どおりの変更を生成するかどうかを確認します。

    **Web.config プレビュー**ウィンドウには、両方を適用した結果が表示されます、 *Web.Release.config*変換と*Web.Staging.config*を変換します。

### <a name="prevent-public-use-of-the-test-app"></a>テスト アプリをパブリックに使用できないようにします。

ステージングのアプリの重要な考慮事項は、インターネット上でライブになりますが、それを使用する公開したくないことです。 ユーザーが検索し、使用する可能性を最小限に抑えるには、次の方法の 1 つ以上を使用できます。

- ステージングのアプリをステージング環境のテストに使用する IP アドレスからのみアクセスを許可するファイアウォール規則を設定します。
- 推測することはできませんを難読化された URL を使用します。
- 作成、 *robots.txt*ファイルを検索エンジンがクロール テスト アプリとレポートのリンクを検索結果にすることを確認します。

これらのメソッドの最初が最も効果的ですが、Azure App Service ではなく Azure クラウド サービスをデプロイすることが必要になるため、このチュートリアルでは説明しません。 クラウド サービスの詳細についてと Azure で IP 制限は、次を参照してください。 [Azure によって提供されるオプションをホストしているコンピューティング](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)と[Web ロールへのアクセスを特定の IP アドレスをブロック](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx)します。 サード パーティのホスティング プロバイダーに展開する場合は、IP 制限を実装する方法について説明を検索するプロバイダーに問い合わせてください。

このチュートリアルで作成します、 *robots.txt*ファイル。

1. **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**新しい項目の追加**します。
2. 新規作成**テキスト ファイル**という*robots.txt*、し、次のテキストを配置します。

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent`行は、ファイル内のルールがすべて検索エンジン web クローラー (ロボット) に適用される検索エンジンに指示し、`Disallow`行では、サイトのページをクロールないことを指定します。

    検索エンジンを運用環境のデプロイからこのファイルを除外する必要があるため、実稼働アプリをカタログ化するようにします。 構成することを行う運用環境の設定は、作成するときにプロファイルを発行します。

### <a name="deploy-to-staging"></a>ステージングへのデプロイします。

1. 開く、 **Web の発行**を Contoso University のプロジェクトを右クリックしてウィザード**発行**します。
2. 必ず、**ステージング**プロファイルが選択されています。
3. **[発行]** をクリックします。

    **出力**ウィンドウが実行されたどのようなデプロイ操作を示しています。 され、展開が正常に完了を報告します。 デプロイされた web アプリの URL に既定のブラウザーが自動的に開きます。

## <a name="test-in-the-staging-environment"></a>ステージング環境でのテストします。

環境のインジケーターが存在しないことに注意してください (がない"(Test)"または"(Dev)"ことを示していますの H1 見出しの後、 *Web.config*環境インジケーターの変換が成功しました。

![ホーム ページのステージング](deploying-to-production/_static/image12.png)

実行、**学生**ページに配置されているデータベースに学生がないことを確認します。

実行、 **Instructors** Code First シード インストラクター データでデータベースを確認します。

選択**受講者の追加**から、**学生** メニューの は、生徒を追加しで新しい学生を表示し、**受講者**データベースに正常に記述できることを確認する ページ.

**コース**] ページで [**更新クレジット**します。 **更新クレジット**ページには、管理者のアクセス許可が必要ですので、**ログで**ページが表示されます。 以前 ("admin"および"prodpwd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示され、前のチュートリアルで作成した管理者アカウントがテスト環境に正しくデプロイされたことを確認します。

ELMAH は追跡、および、ELMAH のエラー レポートを要求し、エラーが発生する、無効な URL を要求します。 サード パーティのホスティング プロバイダーに展開する場合、レポートが空で、前のチュートリアルを同じ理由から空である可能性がありますわかります。 ホスティング プロバイダーのアカウントの管理ツールを使用して、ログ フォルダーに書き込む ELMAH を有効にするフォルダーのアクセス許可を構成する必要があります。

実稼働に使用するものと同じように web アプリにクラウドで作成したアプリケーションが実行されているようになりました。 すべてが正しく動作しているために、次の手順は、運用環境に配置です。

## <a name="deploy-to-production"></a>運用環境にデプロイします。

運用 web アプリを作成して、運用環境にデプロイするプロセスは、ステージング環境の場合と同様を除外する必要がある点が異なります、 *robots.txt*展開からします。 そのためには、発行プロファイル ファイルを編集します。

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>運用環境を作成し、運用環境の発行プロファイル

1. Azure では、ステージングのために使用した同じ手順を次に、運用 web アプリとデータベースを作成します。

    データベースを作成するときに、先ほど作成した同じサーバー上に選択または新しいサーバーを作成できます。
2. ダウンロード、 *.publishsettings*ファイル。
3. 運用環境にインポートすることで、発行プロファイルを作成 *.publishsettings*ファイルをステージングに使用した同じ手順を実行します。

    データ デプロイ スクリプトを構成することを忘れないでください**DefaultConnection**で、**データベース**のセクション、**設定**タブ。
4. 発行プロファイルの名前を変更*運用*します。
5. ステージングのために使用した同じ手順に従って、環境のインジケーターの発行プロファイルの変換を構成するには.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Robots.txt を除外する .pubxml ファイルを編集します。

発行プロファイル ファイルの名前は&lt;profilename&gt;*.pubxml*に存在し、 *PublishProfiles*フォルダー。 *PublishProfiles*フォルダーは、*プロパティ*フォルダー c# web アプリケーション プロジェクトで、 *My Project*フォルダーまたは VB の web アプリケーション プロジェクトで、*アプリ\_データ*web アプリ プロジェクトのフォルダー。 各 *.pubxml*ファイルには、いずれかに適用される設定が含まれています。 プロファイルを公開します。 Web の発行ウィザードで入力した値は、これらのファイルに格納され、Visual Studio UI で利用可能になっていない設定を作成または変更し、編集します。

既定では、 *.pubxml*発行プロファイルを作成し、プロジェクトから除外することができますと Visual Studio は現在でも使用、プロジェクトにファイルが含まれています。 Visual Studio では、 *PublishProfiles*フォルダー *.pubxml*プロジェクトに含まれているかどうかに関係なく、ファイル。

各 *.pubxml*ファイルがある、 *. >.pubxml.user*ファイル。 *. >.pubxml.user*を選択した場合、ファイルが暗号化されたパスワードを含む、**パスワードの保存**オプション、および既定では、プロジェクトから除外されます。

A *.pubxml*ファイルには、特定の発行プロファイルに関連する設定が含まれています。 すべてのプロファイルに適用される設定を構成する場合を作成、 *. wpp.targets*ファイル。 ビルド プロセスは、これらのファイルをインポート、 *.csproj*または *.vbproj*プロジェクト ファイル、プロジェクト ファイルで構成できる設定のほとんどは、これらのファイルで構成できます。 詳細については *.pubxml*ファイルと *. wpp.targets*ファイルを参照してください[方法: 発行プロファイル (.pubxml) ファイルでの展開設定の編集、および wpp.targets Visual Studio でのファイル。Web プロジェクト](https://msdn.microsoft.com/library/ff398069.aspx)します。

1. **ソリューション エクスプ ローラー**、展開**プロパティ**展開**PublishProfiles**します。
2. 右クリック*Production.pubxml*クリック**オープン**。

    ![.Pubxml ファイルを開く](deploying-to-production/_static/image13.png)
3. 右クリック*Production.pubxml*クリック**オープン**。
4. 終了する直前に次の行を追加`PropertyGroup`要素。

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    .Pubxml ファイルは、次の例のようになります。

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    ファイルとフォルダーを除外する方法の詳細については、次を参照してください。[できますしないようにする特定のファイルまたはフォルダーの展開からでしょうか。](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)で、 **for Visual Studio および ASP.NET Web 配置の FAQ** msdn です。

### <a name="deploy-to-production"></a>運用環境にデプロイします。

1. 開く、 **Web の発行**ウィザードことを確認します、**運用**発行プロファイルが選択されているし、をクリックし、**プレビューの開始**で、**プレビュー**ことを確認するには、タブ、 *robots.txt*ファイルは、運用アプリにコピーされません。

    ![運用環境に発行するファイルのプレビュー](deploying-to-production/_static/image14.png)

    コピーされるファイルの一覧を確認します。 表示されているすべての *.cs*など、ファイル *. aspx.cs*、 *. aspx.designer.cs*、 *Master.cs*、および*Master.designer.cs*ファイルは省略されます。 このコードすべてをコンパイルした後、 *ContosoUniversity.dll*と*ContosUniversity.pdb*で見つかるファイル、 *bin*フォルダー。 ため、のみ、 *.dll*に必要な実行して、アプリケーションでは、前に指定、アプリケーションの実行に必要なファイルのみを配置すること、no *.cs*ファイルは、先にコピーされました環境。 *Obj*フォルダーと *[contosouniversity.csproj]* と *. csproj.user*ファイルは同じ理由から省略されます。

    クリックして**発行**実稼働環境に配置します。
2. ステージング環境で使用するのと同じ手順を実行して、運用環境でテストします。

    すべての URL 以外のステージング環境とがない場合と同じですが、 *robots.txt*ファイル。

## <a name="summary"></a>まとめ

これで正常にデプロイして、web アプリをテストし、入手可能になったパブリック インターネット経由で。

![運用環境のホーム ページ](deploying-to-production/_static/image15.png)

次のチュートリアルでは、アプリケーション コードを更新し、テスト、ステージング、実稼働環境に変更をデプロイします。

> [!NOTE]
> アプリケーションを運用環境で使用中には、復旧計画を実装する必要があります。 つまり、する必要がありますが定期的にデータベースのバックアップ、運用環境のアプリから、セキュリティで保護された記憶域の場所に、このようなバックアップのいくつかの世代を保存する必要があります。 データベースを更新するときに、変更の直前のバックアップ コピーをする必要があります。 次に、設定を間違えたして運用環境に配置した後に検出されるまでそのしない場合が破損する前に、の状態にデータベースを復旧できます。 詳細については、次を参照してください。 [Azure SQL Database のバックアップと復元](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx)します。
> 
> 
> [!NOTE]
> このチュートリアル SQL Server を展開しているエディションは、Azure SQL Database は。 展開プロセスは他のエディションの SQL Server に似ていますが、実際の運用アプリケーション必要があります特別なコードを Azure SQL Database の一部のシナリオで。 詳細については、次を参照してください。 [Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)と[SQL Server および Azure SQL Database の使い分け](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)します。
> 
> [!div class="step-by-step"]
> [前へ](setting-folder-permissions.md)
> [次へ](deploying-a-code-update.md)
