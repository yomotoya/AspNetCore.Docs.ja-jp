---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開する |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881335"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

初期展開後の維持と、web サイトの開発作業を続行、および時間の長い前にする更新プログラムを展開します。 このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセスです。 実装し、このチュートリアルで展開する更新プログラムがデータベースの変更を含まない次のチュートリアルではデータベースの変更の展開の違いは何が表示されます。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。

## <a name="make-a-code-change"></a>コードの変更

単純な例として、更新プログラムをアプリケーションに、追加、**講習においてインストラクター**  ページで選択した講師コースの一覧です。

実行する場合、**講習においてインストラクター**  ページで、かがわかりますがある**選択**グリッド内のリンクを行わない make 以外の何か、行の背景が灰色に変わりますが、します。

![講習においてインストラクターの選択 ページ](deploying-a-code-update/_static/image1.png)

これで、ときに実行されるコードを追加、**選択**リンクがクリックされ、選択した講師でコースの一覧を表示します。

1. *Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. ページを実行し、インストラクターを選択します。 そのインストラクターでコースの一覧を参照してください。

    ![講習においてインストラクター学期コース ページ](deploying-a-code-update/_static/image2.png)
3. ブラウザーを閉じます。

## <a name="deploy-the-code-update-to-the-test-environment"></a>コードの更新をテスト環境に展開します。

テスト、ステージング、および実稼働環境に展開する、発行プロファイルを使用することができます、前に、データベースの公開オプションを変更する必要があります。 メンバーシップ データベースの grant およびデータ配置スクリプトを実行する必要がなくなります。

1. 開く、 **Web の発行**ContosoUniversity プロジェクトを右クリックしをクリックしてウィザード**発行**です。
2. クリックして、**テスト**のプロファイルの**プロファイル**ドロップダウン リスト。
3. クリックして、**設定**タブです。
4. [ **DefaultConnection**で、**データベース**] セクションで、クリア、**データベースの更新**チェック ボックスをオンします。
5. クリックして、**プロファイル** タブをクリックして、**ステージング**のプロファイルの**プロファイル**ドロップダウン リスト。
6. 変更を保存するように求めは、**テスト**プロファイルで、**はい**です。
7. ステージングのプロファイルに同じ変更を加えます。
8. 実稼働プロファイルで、同じ変更を行う手順を繰り返します。
9. 閉じる、 **Web の発行**ウィザード。

これで、実行中の 1 回のクリックの単純な処理をもう一度発行は、テスト環境に展開します。 このプロセスを短縮するためには、使用することができます、 **Web 1 つをクリックして 発行**ツールバー。

1. **ビュー**  メニューの 選択**ツールバー**し、 **Web 1 つをクリックして 発行**です。

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. **ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。
3. **Web 1 つをクリックして 発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印のアイコン)。

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio 更新済みのアプリケーションを配置して、ブラウザーは、ホーム ページに自動的に開きます。
5. 講習においてインストラクター ページを実行し、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。

通常どおりも回帰テストを行う (つまり、新しい変更で、既存の機能が損なわれていないことを確認するサイトの残りの部分をテストする)。 このチュートリアルでをその手順をスキップし、ステージング環境と運用環境の更新プログラムの展開に進みます。

再展開するときにどのファイルが変更を自動的に決定 Web 配置し、コピーのみがサーバーにファイルを変更します。 既定では、Web Deploy で最終変更日に基づいてファイル判別どれが変更されました。 いくつかのソース管理システムでは、ファイルの内容を変更しない場合、ファイルがあっても日付に変更します。 その場合は、Web デプロイを決定するファイルが変更されたファイルのチェックサムを使用して構成することができます。 詳細については、次を参照してください。[理由はすべてのファイルを取得を再展開が、それらを変更していないか。](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 展開 FAQ にします。

## <a name="take-the-application-offline-during-deployment"></a>配置時にオフライン アプリケーションします。

配置する変更は、単純な変更を単一のページを今すぐです。 規模の大きな変更を展開することがありますが、またはコードとデータベースの両方の変更を配置するし、サイトが正しく動作しない場合は、ユーザーが展開を完了する前に、ページを要求します。 ユーザーが、展開が進行中はサイトへのアクセスをするを防ぐために使用することができます、*アプリ\_offline.htm*ファイル。 という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーに IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。 アクセスを防ぐため、配置時に、配置するように*アプリ\_offline.htm*ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*後に正常に実行展開します。

Web は、既定値を自動的に展開を構成する*アプリ\_offline.htm*の展開開始時に、サーバー上のファイルし、完了時に削除します。 すべてあることを行うには、発行プロファイル (.pubxml) ファイルに次の XML 要素を追加します。

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

このチュートリアルで作成して、ユーザー設定を使用する方法が表示されます*アプリ\_offline.htm*ファイル。

使用して*アプリ\_offline.htm*ステージング サイトでは必要ありません、ステージング サイトにアクセスする必要があるためです。 すべての実稼働環境で展開する方法をテストするステージングを使用することをお勧めします。

### <a name="create-appofflinehtm"></a>アプリを作成する\_offline.htm

1. **ソリューション エクスプ ローラー**ソリューションを右クリックしをクリックして、**追加**、順にクリック**新しい項目の**します。
2. 作成、 **HTML ページ**という*アプリ\_offline.htm* (で最後の"l"を削除、 *.html* Visual Studio を既定で作成する拡張機能)。
3. テンプレートのマークアップを次のマークアップに置き換えます。

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. ファイルを保存して閉じます。

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>コピー アプリ\_web サイトのルート フォルダーに offline.htm

任意の FTP ツールを使用すると、web サイトにファイルをコピーします。 [FileZilla](http://filezilla-project.org/)は人気のある FTP およびツールのスクリーン ショットに表示されます。

FTP ツールを使用する次の 3 つ必要があります: FTP の URL、ユーザー名とパスワード。

URL は、Azure 管理ポータルで、web サイトのダッシュ ボード ページに表示され、ユーザー名と FTP のパスワードは含まれて、 *.publishsettings*以前ダウンロードしたファイルです。 次の手順では、これらの値を取得する方法を示します。

1. Azure 管理ポータルでをクリックして**Web サイト**タブし、[ステージングの web サイト] をクリックします。
2. **ダッシュ ボード** ページで、検索での FTP ホストの名前まで下にスクロール、 **Quick Glance**セクションです。

    ![FTP ホスト名](deploying-a-code-update/_static/image5.png)
3. ステージングを開く *.publishsettings*メモ帳または別のテキスト エディターでファイル。
4. 検索、 `publishProfile` FTP プロファイルの要素。
5. コピー、`userName`と`userPWD`値。

    ![FTP の資格情報](deploying-a-code-update/_static/image6.png)
6. FTP の URL にログインして、FTP ツールを開きます。
7. コピー*アプリ\_offline.htm*をソリューション フォルダーから、 */サイト/wwwroot*ステージング サイト内のフォルダーです。

    ![コピー app_offline](deploying-a-code-update/_static/image7.png)
8. ステージング サイトの URL を参照します。 参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されます。

    ![ブラウザーのウィンドウで app_offline.htm](deploying-a-code-update/_static/image8.png)

ステージングを展開する準備が整いました。

## <a name="deploy-the-code-update-to-staging-and-production"></a>ステージングと運用環境にコードの更新を展開します。

1. **Web 1 つをクリックして 発行**ツールバーで、選択、**ステージング**発行プロファイルをクリックして**Web の発行**です。

    Visual Studio では、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーが開きます。 *アプリ\_offline.htm*ファイルが表示されます。 展開の成功を確認するテストすることができます、前に削除する必要があります、*アプリ\_offline.htm*ファイル。
2. FTP ツールに戻るし、削除**アプリ\_offline.htm**ステージング サイトからです。
3. ブラウザーには、ステージング サイトで講習においてインストラクター ページを開き、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。
4. 場合と同様ステージング、実稼働の同じ手順を実行します。

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>変更を確認し、特定のファイルを展開します。

Visual Studio 2012 は個々 のファイルを展開することも提供します。 選択したファイルのローカル バージョンと、展開されたバージョンの違いを表示、ファイルを送信先の環境に展開したり、送信先の環境からローカルのプロジェクトにファイルをコピーします。 チュートリアルのこのセクションには、これらの機能を使用する方法が表示されます。

### <a name="make-a-change-to-deploy"></a>展開する変更を行う

1. 開いている*Content/Site.css*のブロックを見つけて、`body`タグ。
2. 値を変更`background-color`から`#fff`に`darkblue`です。

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>公開プレビュー ウィンドウで、変更を表示します。

使用すると、 **Web の発行**、プロジェクトを発行するウィザードでファイルをダブルクリックして公開されることは、変更を参照してください、**プレビュー**ウィンドウです。

1. ContosoUniversity プロジェクトを右クリックし、をクリックして**発行**です。
2. **プロファイル**ドロップダウン リストで、**テスト**プロファイルを発行します。
3. をクリックして**プレビュー**、順にクリック**開始プレビュー**です。
4. **プレビュー**  ウィンドウをダブルクリックして**Site.css**です。

    ![Site.css をダブルクリックします。](deploying-a-code-update/_static/image9.png)

    **変更のプレビュー**ダイアログが展開される変更のプレビューを表示します。

    ![Site.css に加えた変更のプレビュー](deploying-a-code-update/_static/image10.png)

    ダブルクリックする場合、 *Web.config*ファイル、**変更のプレビュー**ダイアログ変換の構成、ビルドの効果を示していて、発行プロファイルの変換。 この時点で実行していない原因は、 *Web.config*を変更すると、変更が表示されないため、サーバー上のファイルです。 ただし、**変更のプレビュー**ウィンドウが 2 つの変更を正しく表示します。 2 つの XML 要素が削除するようです。 選択すると、発行プロセスでこれらの要素が追加された**実行 Code First Migrations アプリケーション開始時に**Code First コンテキスト クラスです。 発行プロセスが削除されませんが、取り消されるように見えるために、それらの要素を追加する前に、比較が行われます。 このエラーは、将来のリリースで修正される予定です。
5. **[閉じる]** をクリックします。
6. **[発行]** をクリックします。
7. テスト サイトのホーム ページを開くと、ブラウザー、CTRL + f5 キーを押して CSS の変更の結果を確認するためにハード更新が発生します。

    ![CSS の変更の結果](deploying-a-code-update/_static/image11.png)
8. ブラウザーを閉じます。

### <a name="publish-specific-files-from-solution-explorer"></a>ソリューション エクスプ ローラーからの特定のファイルを発行します。

などの背景色が青おらず、元の色に戻したいとします。 このセクションの内容から直接特定のファイルを発行することによって、元の設定を復元**ソリューション エクスプ ローラー**です。

1. 開いている*Content/Site.css*と復元、`background-color`設定`#fff`です。
2. **ソリューション エクスプ ローラー**を右クリックし、 *Content/Site.css*ファイル。

    コンテキスト メニューでは、3 つのパブリッシュ オプションを示します。

    ![ソリューション エクスプ ローラーからのオプションのパブリッシュ](deploying-a-code-update/_static/image12.png)
3. をクリックして**Site.css に加えた変更のプレビュー**です。

    送信先の環境に、ローカル ファイルとそのバージョンの違いを表示するウィンドウが開きます。

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. **ソリューション エクスプ ローラー**を右クリックして**Site.css**再度 をクリック**発行 Site.css**です。

    **Web 公開アクティビティ**ファイルが公開されているウィンドウを示しています。

    ![Web 公開活動ウィンドウ](deploying-a-code-update/_static/image14.png)
5. ブラウザーを開いて、 `http://localhost/contosouniversity` URL、およびし、CTRL + f5 キーを押してハードが発生する変更、CSS の効果を確認するために更新します。

    ![通常の CSS のホーム ページ](deploying-a-code-update/_static/image15.png)
6. ブラウザーを閉じます。

## <a name="summary"></a>まとめ

データベースの変更を含まないアプリケーションの更新プログラムを展開するいくつかの方法を確認したようになりましたし、新機能が更新されますが期待どおりに表示を確認する変更をプレビューする方法を説明しました。 講習においてインストラクター ページのようになりましたが、**コース学期**セクションです。

![講習においてインストラクター学期コース ページ](deploying-a-code-update/_static/image16.png)

次のチュートリアルは、データベースの変更を展開する方法を示します: データベースと講習においてインストラクター ページには、生年月日フィールドを追加します。

> [!div class="step-by-step"]
> [前へ](deploying-to-production.md)
> [次へ](deploying-a-database-update.md)
