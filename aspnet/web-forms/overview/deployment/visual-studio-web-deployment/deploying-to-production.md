---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "Visual Studio を使用した ASP.NET Web 展開: 実稼働環境に展開する |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 2a8b165c149ceacba49a193c25e4a66a701ea0d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Visual Studio を使用した ASP.NET Web 展開: 実稼働環境に展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは Microsoft Azure アカウントをセットアップ、ステージングと運用環境を作成およびステージングする ASP.NET web アプリケーションを展開して、Visual Studio の 1 回のクリックを使用して実稼働環境の機能を公開します。

を使用する場合は、サード パーティのホスティング プロバイダーに展開できます。 このチュートリアルで説明する手順のほとんどは、各プロバイダーがあるアカウントと web サイト管理のための独自のユーザー インターフェイスを持つ点を除いて、ホスティング プロバイダーまたは Azure で同じです。 ホスティング プロバイダーを見つけることができます、[プロバイダーのギャラリー](https://www.microsoft.com/web/hosting) Microsoft.com web サイトです。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しないは、このチュートリアル シリーズのトラブルシューティングのページを確認することを確認します。

## <a name="get-a-microsoft-azure-account"></a>Microsoft Azure アカウントを取得します。

既に Azure アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Azure 無料評価版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。

## <a name="create-a-staging-environment"></a>ステージング環境を作成します。

> [!NOTE]
> このチュートリアルが書き込まれるため、Azure App Service はステージング環境と実稼働環境を持つでプロセスの多くを自動化する新機能を追加します。 参照してください[ステージング環境の Azure App service web アプリ設定](https://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/)です。


説明に従って、[環境のテストのチュートリアルへの展開](deploying-to-iis.md)最も信頼性の高いテスト環境は、実稼働 web サイトと同様にしているホスティング プロバイダーに web サイトです。 大きな追加コストに対するこれの利点を比較検討する必要が多くのホスティング プロバイダーですが Azure のステージング アプリとして追加の無料の web アプリを作成することができます。 また、データベースが必要し、そのため、実稼働データベースの経費経由での追加の費用は none いずれかになりますまたは最低限です。 Azure でした各データベースではなく、使用するデータベースの記憶域の量に対してお支払いステージングを使用して追加の記憶域の量が最小限に抑えられます。

説明に従って、[テスト環境のチュートリアルへの展開](deploying-to-iis.md)ステージングと運用データベースを 1 つに、2 つのデータベースを展開することで、します。 別に保持する場合は、プロセス同じになりますが、それぞれの環境の他のデータベースを作成し、発行プロファイルを作成するときに、各データベースの正しいコピー先文字列が選択されます。 します。

チュートリアルのこのセクションでは、web アプリとステージング環境で使用するデータベースを作成しますおよびをステージング展開を作成して、実稼働環境に展開する前にテストがあります。

> [!NOTE]
> 次の手順では、Azure 管理ポータルを使用して、Azure App service web アプリを作成する方法を示します。 Azure SDK の最新のバージョンで行うことができますもこのサーバー エクスプ ローラーを使用して、Visual Studio を離れずにします。 Visual Studio 2013 で、発行ダイアログ ボックスから直接 web アプリを作成することもできます。 詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成します。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. [Azure 管理ポータル](https://manage.windowsazure.com/)をクリックして**web サイト**、順にクリック**新規**です。
2. をクリックして**web サイト**、クリックして**カスタム作成**です。

    **新しい web サイト - カスタム作成**ウィザードが開きます。 **カスタム作成**ウィザードでは、同時に、web サイトとデータベースを作成することができます。
3. **Web サイトの作成**ステップのウィザードで文字列を入力してください。、 **URL** 、アプリケーションの一意の URL として使用する ボックスのステージング環境。 たとえば、ContosoUniversity staging123 (ContosoUniversity ステージングが実行される場合に一意にする、最後の乱数を含む) を入力します。

    完全な URL は、ここに入力したものとテキスト ボックスの横に表示されるサフィックスで構成されます。
4. **地域**ドロップダウン リストで、自分に最も近い地域を選択します。

    この設定は、データ センターで、web アプリは実行を指定します。
5. **データベース**ドロップダウン リストで、選択**新しい SQL データベースの作成**です。
6. **DB 接続文字列名**ボックスで、既定値をそのまま*DefaultConnection*です。
7. 右側のボックスの下部にある矢印をクリックします。

    次の図は、 **web サイトの作成** ダイアログ ボックス内のサンプル値。 URL と入力した地域されるが異なります。

    ![Web サイトを作成します。](deploying-to-production/_static/image1.png)

    ウィザード、**データベース設定を指定**手順です。
8. **名前**ボックスに、入力*ContosoUniversity*乱数を一意になるよう、たとえばプラス*ContosoUniversity123*です。
9. **サーバー**ボックスで、**新しい SQL データベース サーバー**です。
10. 管理者名とパスワードを入力します。

    既存の名前とパスワードをここに入力されません。 新しい名前と、データベースにアクセスするときに後で使用するには、ここで定義しているパスワードを入力しているか。
11. **地域**ボックスで、web アプリを選択したのと同じリージョンを選択します。

    同じリージョンに、web サーバーとデータベース サーバーを維持し、最適なパフォーマンスを提供し、経費を最小限に抑えられます。
12. 完了したらを示すために、ボックスの下部にあるチェック マークをクリックします。

    次の図は、**データベース設定を指定** ダイアログ ボックス内のサンプル値。 入力した値が異なる場合があります。

    ![データベースの設定手順の新しい web サイトのデータベースのウィザードで作成します。](deploying-to-production/_static/image2.png)

    管理ポータル ページに戻る、web サイト、および**ステータス**列は、web アプリが作成されていることを示しています。 (通常よりも小さい 1 分間)、しばらくしてから、**ステータス**列は、web アプリが正常に作成されたことを示しています。 左側にあるナビゲーション バーで、アカウント内にある web アプリの数が表示されます の横に、 **Websites**アイコン、およびデータベースの数が横に表示、 **SQL データベース**アイコン。

    ![管理ポータルで作成された web サイトの Web サイト ページ](deploying-to-production/_static/image3.png)

    Web アプリ名は、図の例のアプリを異なるになります。

## <a name="deploy-the-application-to-staging"></a>ステージングするアプリケーションを配置します。

Web アプリとステージング環境のデータベースを作成してプロジェクトを配置することができます。

> [!NOTE]
> これらの手順は、ダウンロードして発行プロファイルを作成する方法を示します、 *.publishsettings*ファイルで、サード パーティのホスティング プロバイダーが Azure 用だけでなく動作します。 最新の Azure SDK も使用する、Visual Studio から Azure に直接接続し、Azure アカウントにある web アプリの一覧から選択できます。 Visual Studio 2013 ではから Azure にサインインすることができます、 **Web Publish**ダイアログから、または、**サーバー エクスプ ローラー**ウィンドウです。 詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)です。


### <a name="download-the-publishsettings-file"></a>.Publishsettings ファイルをダウンロードします。

1. 作成した web アプリの名前をクリックします。

    ![ダッシュ ボードに移動するサイトをクリックします。](deploying-to-production/_static/image4.png)
2. [**クイック グランス**で、**ダッシュ ボード**] タブで、をクリックして**発行プロファイルのダウンロード**です。

    ![発行プロファイルのリンクをダウンロードします。](deploying-to-production/_static/image5.png)

    この手順では、すべてのアプリケーション、web アプリを配置するために必要な設定を含むファイルをダウンロードします。 この情報を手動で入力する必要はありませんは、Visual Studio にこのファイルをインポートします。
3. 保存、 *.publishsettings* Visual Studio からアクセスできるフォルダー内のファイルです。

    ![.publishsettings ファイルを保存します。](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > セキュリティ -、 *.publishsettings*ファイルには、Azure のサブスクリプションおよびサービスの管理に使用される (エンコードされていない) 資格情報が含まれています。 このファイルのセキュリティのベスト プラクティスは、(たとえば、\documents フォルダー) に、ソース ディレクトリの外部一時的に格納し、インポートが完了した後で削除するのにです。 アクセスを取得した悪意のあるユーザー、 *.publishsettings*ファイルは、編集、作成、および Azure サービスを削除します。

### <a name="create-a-publish-profile"></a>発行プロファイルを作成します。

1. Visual Studio でプロジェクトを右クリックして、ContosoUniversity で**ソリューション エクスプ ローラー**選択と**発行**コンテキスト メニュー。

    **Web の発行**ウィザードが開きます。
2. クリックして、**プロファイル**タブです。
3. **[インポート]**をクリックします。
4. 移動し、 *.publishsettings*ファイルを以前にダウンロードし、クリックして**開く**です。

    ![発行設定のインポート ダイアログ ボックス](deploying-to-production/_static/image7.png)
5. **接続** タブで、をクリックして**接続の検証**設定が正しいことを確認します。

    緑のチェック マークが横に示すように、接続が検証されると、**接続の検証**ボタンをクリックします。

    ホスティング プロバイダーによってをクリックすると**接続の検証**、表示される、**証明書エラー**  ダイアログ ボックス。 を行う場合は、サーバー名が予期したとおりであることを確認します。 サーバー名が正しい場合は、選択**Visual Studio の今後のセッションにこの証明書を保存** をクリック**Accept**です。 (このエラーを展開している URL の SSL 証明書を購入する必要を避けるために、ホスティング プロバイダーが選択されていることを意味します。 有効な証明書を使用してセキュリティで保護された接続を確立する場合は、連絡、ホスティング プロバイダーです。)
6. **[次へ]**をクリックします。

    ![接続成功アイコンと [接続] タブで [次へ] ボタンをクリックします。](deploying-to-production/_static/image8.png)
7. **設定**] タブで、展開**ファイルの発行オプション**、し、[**アプリからファイルを除外する\_データ フォルダー**です。

    その他のオプションについては**ファイルの発行オプション**を参照してください、 [IIS に展開する](deploying-to-iis.md)チュートリアルです。 画面は、この手順と、次のデータベースの構成手順の結果は、データベースの構成手順の最後に表示されます。
8. **DefaultConnection**で、**データベース**セクションで、メンバーシップ データベースに対するデータベース デプロイを構成します。
9. 1. 選択**更新データベース**です。

        **リモート接続文字列**すぐ下のボックス**DefaultConnection** .publishsettings ファイルから接続文字列が入力されます。接続文字列には、プレーン テキストで格納されている SQL Server 資格情報が含まれている、 *.pubxml*ファイル。 保存するように完全にしないようにする場合は、データベースを配置した後、発行プロファイルから削除して、代わりに Azure に保存します。 詳細については、次を参照してください。[保護する方法、ASP.NET データベース接続文字列のソースから Azure に展開するときに](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman ブログ。
    2. をクリックして**データベース更新を構成する**です。
    3. **データベース更新を構成する**ダイアログ ボックスで、をクリックして**SQL スクリプトの追加**です。
    4. **SQL スクリプトの追加**ボックスに移動、 *aspnet データ-prod.sql*ソリューション フォルダーに以前に保存し、をクリックし、スクリプトを**開く**です。
    5. 閉じる、**データベース更新を構成する** ダイアログ ボックス。
10. **SchoolContext**で、**データベース**セクションで、**実行 Code First Migrations (アプリケーション開始時に実行されます)**です。

    Visual Studio に表示**Code First Migrations を実行**の代わりに**更新データベース**の`DbContext`クラスです。 移行ではなく dbDacFx プロバイダーを使用して、使用してアクセスするデータベースを配置するかどうか、`DbContext`クラスを参照してください[移行なしの Code First のデータベースを展開する方法ですか?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations) for Visual Studio Web 配置の faqMSDN で ASP.NET です。

    **設定** タブでは次の例のようになります。

    ![ステージングのための設定 タブ](deploying-to-production/_static/image9.png)
11. プロファイルを保存して、変更するには、次の手順を実行*ステージング*:

    1. クリックして、**プロファイル** タブをクリックして**プロファイルの管理**です。
    2. インポートでは、ftp および Web Deploy 用の 2 つの新しいプロファイルを作成します。 Web Deploy プロファイルを構成する: このプロファイルの名前を変更*ステージング*です。

        ![ステージングするプロファイルの名前を変更します。](deploying-to-production/_static/image10.png)
    3. 閉じる、 **Web の発行プロファイルの編集** ダイアログ ボックス。
    4. 閉じる、 **Web の発行**ウィザード。

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>環境のインジケーターの発行プロファイルの変換を構成します。

> [!NOTE]
> このセクションでは、環境のインジケーターの Web.config 変換の設定方法を示します。 インジケーターがあるため、 `<appSettings>` Azure App Service に配置するときに、変換を指定するための別の方法がある要素。 詳細については、次を参照してください。 [Azure で指定する Web.config 設定](web-config-transformations.md#watransforms)です。


1. **ソリューション エクスプ ローラー**、展開**プロパティ**の順に展開および**PublishProfiles**です。
2. 右クリック*Staging.pubxml*、クリックして**Config 変換の追加**です。

    ![ステージングのための構成変換を追加します。](deploying-to-production/_static/image11.png)

    Visual Studio によって作成、 *Web.Staging.config*変換ファイルを開きます。
3. *Web.Staging.config*変換ファイルが、開始直後に次のコードを挿入`configuration`タグ。

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    ステージング発行プロファイルを使用する場合、この変換は、"Prod"するインジケーターを環境を設定します。 展開された web アプリは、「Contoso 大学」H1 見出しの後に、"(Dev)"などの任意のサフィックスや"(Test)"表示されません。
4. 右クリックし、 *Web.Staging.config*ファイルし、をクリックして**プレビュー変換**ようにコーディングした変換が必要な変更を生成するかどうかを確認します。

    **Web.config プレビュー**ウィンドウは、両方に適用した結果を示しています、 *Web.Release.config*変換と*Web.Staging.config*を変換します。

### <a name="prevent-public-use-of-the-test-app"></a>テスト アプリの公開の使用を防止します。

ステージングのアプリの重要な考慮事項は、インターネットでは、ライブされますが、これを使用する公開したくないです。 ユーザーを検索し、使用する可能性を最小限に抑えるには、次の方法の 1 つ以上を使用できます。

- ステージングのテストに使用する IP アドレスからのみステージング アプリへのアクセスを許可するファイアウォール規則を設定します。
- 推測することはできませんを難読化された URL を使用します。
- 作成、 *robots.txt*ファイルを検索エンジンがクロール テスト アプリケーションとレポートのリンクに検索結果にことを確認します。

これらのメソッドの最初のでは、最も有効ですが、Azure App Service ではなく Azure クラウド サービスに展開することが必要になるために、このチュートリアルで説明されていません。 詳細については、クラウド サービスと Azure での IP 制限では、次を参照してください。 [Azure によって提供されるオプションをホストしているコンピューティング](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)と[特定の IP アドレス、Web ロールへのアクセスをブロック](https://msdn.microsoft.com/en-us/library/windowsazure/jj154098.aspx)です。 サード パーティのホスティング プロバイダーに配置する場合は、IP 制限を実装する方法を検索するプロバイダーに問い合わせてください。

このチュートリアルを作成、 *robots.txt*ファイル。

1. **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**新しい項目の追加**です。
2. 新規作成**テキスト ファイル**という*robots.txt*、し、次のテキストを配置します。

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent`行は、ファイル内のルールがすべて検索エンジン web クローラー (ロボット) に適用される検索エンジンに指示し、`Disallow`行は、サイトのページをクロールないことを指定します。

    検索エンジンを運用環境のデプロイからこのファイルを除外する必要があるため、運用アプリをカタログ化するようにします。 構成することを行うには、実稼働環境で設定は、作成するときにプロファイルを発行します。

### <a name="deploy-to-staging"></a>ステージング展開します。

1. 開く、 **Web の発行**を Contoso 大学プロジェクトを右クリックしをクリックするとウィザード**発行**です。
2. 確認して、**ステージング**プロファイルを選択します。
3. **[発行]**をクリックします。

    **出力**ウィンドウどのような展開アクションを実行した示し、展開の成功した完了を報告します。 既定のブラウザーは、展開された web アプリの URL を自動的に開きます。

## <a name="test-in-the-staging-environment"></a>ステージング環境でのテストします。

環境のインジケーターが存在しないことに注意してください (がない「(テスト)」または「(開発)」、H1 見出し、ことを示しています、 *Web.config*環境のインジケーターの変換が成功しました。

![ステージングのホーム ページ](deploying-to-production/_static/image12.png)

実行、**受講者**ページに配置されるデータベースに受講者がいないことを確認します。

実行、**講習においてインストラクター**ページを Code First シード インストラクター データとデータベースを確認します。

選択**受講者を追加**から、**受講者** メニューの 、受講者を追加し、新しい学生を表示、**受講者**データベースに正常に記述できることを確認する ページ.

**コース**] ページで [**更新クレジット**です。 **更新クレジット**ページには、管理者のアクセス許可が必要がありますので、**ログで**ページが表示されます。 以前 ("admin"および"prodpwd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示されたら、前のチュートリアルで作成した管理者アカウントが、テスト環境に正しく展開されたことを確認します。

ELMAH が追跡、および ELMAH エラー レポートを次に、要求は、エラーが発生する無効な URL を要求します。 サード パーティのホスティング プロバイダーに配置する場合、レポートが前のチュートリアルでは空か、同じ理由から空である可能性がありますわかります。 ログ フォルダーに書き込む ELMAH を有効にするフォルダーのアクセス許可を構成するホスティング プロバイダーのアカウントの管理ツールを使用する必要があります。

作成したアプリケーションは、実稼働に使用するものと同じように、web アプリで、クラウドで実行されています。 すべてが正しく動作しているために、次の手順は、実稼働環境に展開するは。

## <a name="deploy-to-production"></a>実稼働環境に展開します。

運用 web アプリを作成して実稼働環境に展開するプロセスについては、同じステージング、除外する必要がある場合を除き、 *robots.txt*展開からします。 これを行うには、発行プロファイル ファイルを編集します。

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>実稼働環境を作成し、発行プロファイルの運用環境

1. Azure では、同じ手順に従って、ステージングのために使用するには、運用 web アプリとデータベースを作成します。

    データベースを作成するときに前に作成した同じサーバーに配置を選択したり、新しいサーバーを作成できます。
2. ダウンロード、 *.publishsettings*ファイル。
3. 運用環境にインポートすることによって、発行プロファイルを作成*.publishsettings*ファイル、ステージングのために使用する同じ手順を実行します。

    忘れずに データの配置スクリプトを構成する**DefaultConnection**で、**データベース**のセクションで、**設定**タブです。
4. 発行プロファイルの名前を変更*運用*です。
5. ステージングを使用している同じ手順に従って、環境のインジケーターの発行プロファイルの変換を構成するには.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Robots.txt を除外する .pubxml ファイルを編集します。

発行プロファイル ファイルの名前は&lt;profilename&gt;*.pubxml*内にあると、 *PublishProfiles*フォルダーです。 *PublishProfiles*フォルダーは、*プロパティ*フォルダー c# web アプリケーションでプロジェクトの下にある、 *My Project*フォルダーまたは VB web アプリケーション プロジェクトで、*アプリ\_データ*web アプリケーション プロジェクト内のフォルダーです。 各*.pubxml*ファイルには、いずれかに適用される設定が含まれています。 発行プロファイル。 Web の発行ウィザードで入力した値は、これらのファイルに格納および編集することを作成または Visual Studio UI で利用可能になっていない設定を変更することができます。

既定では、 *.pubxml*発行プロファイルを作成する、プロジェクトから除外することができますが、および Visual Studio は引き続き使用すると、プロジェクトのファイルが表示されます。 Visual Studio の検索、 *PublishProfiles*用のフォルダー *.pubxml*プロジェクトに含まれているかどうかに関係なく、ファイルです。

各*.pubxml*ファイルがある、 *. pubxml.user*ファイル。 *. Pubxml.user*ファイルには、暗号化されたパスワードが含まれる選択した場合、**保存パスワード**オプション、および既定では、プロジェクトから除外されます。

A *.pubxml*ファイルには、特定の発行プロファイルに関連する設定が含まれています。 すべてのプロファイルに適用される設定を構成する場合は、作成、 *. wpp.targets*ファイル。 ビルド プロセスにこれらのファイルのインポート、 *.csproj*または*.vbproj*プロジェクト ファイル、プロジェクト ファイルで構成できる設定のほとんどは、これらのファイルで構成できます。 詳細については*.pubxml*ファイルと*. wpp.targets*ファイルを参照してください[する方法: 発行プロファイル (.pubxml) ファイルでの展開設定の編集、および wpp.targets Visual Studio でのファイル。Web プロジェクト](https://msdn.microsoft.com/en-us/library/ff398069.aspx)です。

1. **ソリューション エクスプ ローラー**、展開**プロパティ**展開**PublishProfiles**です。
2. 右クリック*Production.pubxml*  をクリック**開く**です。

    ![.Pubxml ファイルを開く](deploying-to-production/_static/image13.png)
3. 右クリック*Production.pubxml*  をクリック**開く**です。
4. 終了する直前に次の行を追加`PropertyGroup`要素。

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    .Pubxml ファイルは、次の例のようになります。

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    ファイルとフォルダーを除外する方法の詳細については、次を参照してください。[できますものを除外する特定のファイルまたはフォルダーの展開から?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)で、 **Visual Studio と ASP.NET の Web 配置の FAQ** msdn です。

### <a name="deploy-to-production"></a>実稼働環境に展開します。

1. 開く、 **Web の発行**ウィザードことを確認して、**実稼働**発行プロファイルを選択して、をクリックして**開始プレビュー**上、**プレビュー**ことを確認するには、タブ、 *robots.txt*ファイルは、運用アプリにコピーされません。

    ![実稼働環境に発行されるファイルのプレビュー](deploying-to-production/_static/image14.png)

    コピーされるファイルの一覧を確認します。 表示されるすべての*.cs*など、ファイル*. aspx.cs*、 *. aspx.designer.cs*、 *Master.cs*、および*Master.designer.cs*ファイルは省略されます。 このすべてのコードにコンパイルした、 *ContosoUniversity.dll*と*ContosUniversity.pdb*にファイル、 *bin*フォルダーです。 のみ、 *.dll*に必要な実行して、アプリケーションでは、前に指定、アプリケーションの実行に必要なファイルのみを展開する必要がある、no *.cs*ファイルは、先にコピーされました環境。 *Obj*フォルダーおよび*ContosoUniversity.csproj*と*. csproj.user*ファイルは、同じ理由から省略されます。

    をクリックして**発行**を運用環境に展開します。
2. 同じ手順に従って、ステージングを使用して、実稼働環境でテストします。

    同じ URL を除くステージングおよびがない場合にすべて、 *robots.txt*ファイル。

## <a name="summary"></a>概要

これで正常にデプロイして、web アプリをテストしが使用可能なパブリック インターネット経由でします。

![実稼働のホーム ページ](deploying-to-production/_static/image15.png)

次のチュートリアルでをアプリケーション コードを更新し、テスト、ステージング、および実稼働環境に変更を展開します。

> [!NOTE]
> アプリケーションが、実稼働環境で使用するときは、復旧計画を実装する必要があります。 つまり、する必要がある定期的にデータベースのバックアップを運用アプリから、セキュリティで保護された記憶域の場所に、このようなバックアップのいくつかの世代を保存する必要があります。 データベースを更新するときに、変更の直前からバックアップ コピーを作成する必要があります。 次に、更新を間違えたして実稼働環境に配置した後まで、検出されない場合もことができますが破損する前に、の状態にデータベースを回復します。 詳細については、次を参照してください。 [Azure SQL データベースのバックアップと復元](https://msdn.microsoft.com/en-us/library/windowsazure/jj650016.aspx)です。


> [!NOTE]
> このチュートリアルは、SQL Server を展開しているエディションは、Azure SQL データベースがします。 展開プロセスは、SQL Server の他のエディションに似ていますが、実際の運用アプリケーション必要があります特別なコードを Azure SQL Database の一部のシナリオで。 詳細については、次を参照してください。 [Azure SQL データベースで作業](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)と[SQL Server と Azure SQL Database の使い分け](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)です。

>[!div class="step-by-step"]
[前へ](setting-folder-permissions.md)
[次へ](deploying-a-code-update.md)
