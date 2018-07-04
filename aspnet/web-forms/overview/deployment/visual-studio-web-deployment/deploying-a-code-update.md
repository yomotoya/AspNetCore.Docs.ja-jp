---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio を使用して ASP.NET Web 展開: コードの更新の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368215"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Visual Studio を使用して ASP.NET Web 展開: コードの更新を展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

初期のデプロイ後の保守および web サイトの開発作業が引き続き発生してまで長い間は、更新プログラムを展開します。 このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセス説明。 データベースの変更を伴わない更新プログラムを実装し、このチュートリアルでデプロイします。次のチュートリアルでデータベースの変更の展開に関する相違点を確認します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="make-a-code-change"></a>コードの変更

アプリケーションへの更新プログラムの簡単な例とに追加します、 **Instructors**ページ、選択したインストラクターが指導するコースのリスト。

実行する場合、 **Instructors**ページが表示されます**選択**グリッド内のリンクが何もしないこと以外、行の背景が灰色に変わりますが、します。

![Instructors ページを選択した場合](deploying-a-code-update/_static/image1.png)

追加するときに実行するコード、**選択**リンクがクリックされ、選択したインストラクター指導するコースの一覧を表示します。

1. *Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. ページを実行し、インストラクターを選択します。 そのインストラクターが指導するコースのリストを参照してください。

    ![Instructors ページに指導するコース](deploying-a-code-update/_static/image2.png)
3. ブラウザーを閉じます。

## <a name="deploy-the-code-update-to-the-test-environment"></a>テスト環境にコードの更新を展開します。

発行プロファイルを使用して、テスト、ステージング、および運用環境にデプロイすることができます、前に、データベースの公開オプションを変更する必要があります。 メンバーシップ データベースの grant およびデータの配置スクリプトを実行する必要がなくなります。

1. 開く、 **Web の発行**を ContosoUniversity プロジェクトを右クリックしてウィザード**発行**します。
2. をクリックして、**テスト**のプロファイルの**プロファイル**ドロップダウン リスト。
3. をクリックして、**設定**タブ。
4. [ **DefaultConnection**で、**データベース**] セクションで、クリア、 **Update database**チェック ボックスをオンします。
5. をクリックして、**プロファイル**タブをクリックし、をクリックし、**ステージング**のプロファイルの**プロファイル**ドロップダウン リスト。
6. 加えられた変更を保存するように求め、**テスト**プロファイルで、**はい**します。
7. ステージングのプロファイルに同じ変更を加えます。
8. 運用プロファイルで同じ変更を行うプロセスを繰り返します。
9. 閉じる、 **Web の発行**ウィザード。

テスト環境へのデプロイが実行 1 回のクリックの簡単な処理をもう一度発行します。 このプロセスを短縮するためには、使用することができます、 **Web の 1 クリックして発行**ツールバー。

1. **ビュー** ] メニューの [選択**ツールバー**選び**Web の 1 クリックして発行**します。

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. **ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。
3. **Web の 1 クリックして発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印の付いたアイコン)。

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio が更新されたアプリケーションを配置し、ブラウザーはホーム ページに自動的に開きます。
5. Instructors ページを実行し、更新プログラムが正常にデプロイされたことを確認するインストラクターを選択します。

通常どおり回帰テストを行うことも (つまり、新しい変更が、既存の機能を中断していないことを確認するには、サイトの残りの部分をテストする)。 このチュートリアルでをその手順をスキップし、ステージングと運用環境に更新を展開に進みます。

を再デプロイするときに Web デプロイ、変更されたファイルを自動的に決定し、コピーのみがサーバーにファイルを変更します。 既定では、Web Deploy は、最終変更日をファイルに変更されたものを判断使用します。 いくつかのソース管理システムでは、ファイルの内容を変更しないと、ファイルが偶数の日付に変更します。 その場合は、Web 配置ファイルのチェックサムを使用して、変更されたファイルを確認するを構成する場合があります。 詳細については、次を参照してください。[理由はすべてのファイル再デプロイが、それらを変更していないか。](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 配置のよく寄せられる質問。

## <a name="take-the-application-offline-during-deployment"></a>デプロイ時にアプリケーションをオフライン

デプロイする変更は、1 つのページを簡単に変更を今すぐです。 大規模な変更を展開することがありますまたはコードとデータベースの変更を配置して、サイトが正しく動作しない場合、デプロイが完了する前に、ユーザーがページを要求します。 ユーザーが、展開が進行中はサイトへのアクセスをするを防ぐために使用することができます、*アプリ\_offline.htm*ファイル。 という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーで IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。 アクセスを防ぐため、デプロイ時に、配置するように*アプリ\_offline.htm* 、ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*後成功展開します。

Web は、既定値を自動的に展開を構成する*アプリ\_offline.htm*展開を開始した場合に、サーバー上のファイルし、完了すると、それを削除します。 行う必要があることを行うには、発行プロファイル (.pubxml) ファイルに次の XML 要素を追加します。

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

このチュートリアルで作成し、カスタムを使用する方法が紹介*アプリ\_offline.htm*ファイル。

使用して*アプリ\_offline.htm*ステージング サイトでは必要ありません、ステージング サイトにアクセスする必要がないためです。 すべての実稼働環境で展開する方法をテストするステージングを使用することをお勧めします。

### <a name="create-appofflinehtm"></a>アプリの作成\_offline.htm

1. **ソリューション エクスプ ローラー**ソリューションを右クリックし、クリックして**追加**、 をクリックし、**新しい項目の**します。
2. 作成、 **HTML ページ**という*アプリ\_offline.htm* (で最後の"l"を削除、 *.html* Visual Studio は既定で作成する拡張機能)。
3. テンプレート マークアップを次のマークアップに置き換えます。

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. ファイルを保存して閉じます。

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>アプリのコピー\_offline.htm web サイトのルート フォルダーに

任意の FTP ツールを使用して、web サイトにファイルをコピーすることができます。 [FileZilla](http://filezilla-project.org/)一般的な FTP ツールであり、スクリーン ショットに表示されます。

FTP ツールを使用するには、するには、次の 3 つが必要です。: FTP の URL、ユーザー名、およびパスワード。

URL は、Azure 管理ポータルでの web サイトのダッシュ ボード ページに表示され、ユーザー名と FTP のパスワードで見つかんだことができます、 *.publishsettings*以前ダウンロードしたファイル。 次の手順では、これらの値を取得する方法を示します。

1. Azure 管理ポータルで次のようにクリックします。 **Websites**タブと、ステージング web サイトをクリックします。
2. **ダッシュ ボード** ページで、FTP ホスト名が見つかるまでスクロール、**概要**セクション。

    ![FTP ホスト名](deploying-a-code-update/_static/image5.png)
3. ステージングを開く *.publishsettings*メモ帳または別のテキスト エディターでファイル。
4. 検索、 `publishProfile` FTP プロファイルの要素。
5. コピー、`userName`と`userPWD`値。

    ![FTP の資格情報](deploying-a-code-update/_static/image6.png)
6. FTP ツールと FTP の URL にログオンを開きます。
7. コピー*アプリ\_offline.htm*をソリューション フォルダーから、 *wwwroot/サイト/* ステージング サイト内のフォルダー。

    ![App_offline をコピーします。](deploying-a-code-update/_static/image7.png)
8. ステージング サイトの URL に移動します。 参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されるようになりました。

    ![ブラウザーのウィンドウで app_offline.htm](deploying-a-code-update/_static/image8.png)

ステージング環境にデプロイする準備が整いました。

## <a name="deploy-the-code-update-to-staging-and-production"></a>ステージングと運用環境にコード更新を展開します。

1. **Web の 1 クリックして発行**ツールバーで、選択、**ステージング**発行プロファイルをクリックして**Web の発行**します。

    Visual Studio は、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーで開きます。 *アプリ\_offline.htm*ファイルが表示されます。 削除する必要がありますを正常にデプロイを確認するテストする前に、*アプリ\_offline.htm*ファイル。
2. FTP ツールに戻るし、削除**アプリ\_offline.htm**ステージング サイトからします。
3. ブラウザーで、ステージング サイトで、Instructors ページを開くし、更新プログラムが正常にデプロイされたことを確認する、インストラクターを選択します。
4. ステージングしたとき、運用環境と同じ手順に従います。

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>変更内容を確認し、特定のファイルを展開します。

Visual Studio 2012 は個々 のファイルを展開する機能も提供します。 選択したファイルには、ローカル バージョンと、展開されたバージョンの違いを表示し、ファイルを展開先の環境に展開または移行先の環境からローカルのプロジェクトにファイルをコピーすることができます。 チュートリアルのこのセクションでこれらの機能を使用する方法を参照してください。

### <a name="make-a-change-to-deploy"></a>変更をデプロイするには

1. 開いている*Content/Site.css*のブロックを見つけて、`body`タグ。
2. 値を変更`background-color`から`#fff`に`darkblue`します。

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>発行のプレビュー ウィンドウで、変更を表示します。

使用すると、 **Web の発行**、プロジェクトを発行するウィザードでファイルをダブルクリックして発行すると、変更を参照してください、**プレビュー**ウィンドウ。

1. ContosoUniversity プロジェクトを右クリックし、をクリックして**発行**します。
2. **プロファイル**ドロップダウン リストで、**テスト**プロファイルを公開します。
3. クリックして**プレビュー**、 をクリックし、**プレビューの開始**します。
4. **プレビュー**ウィンドウで、ダブルクリックして**Site.css**します。

    ![Site.css をダブルクリックします。](deploying-a-code-update/_static/image9.png)

    **変更のプレビュー**ダイアログが展開される変更のプレビューが表示されます。

    ![Site.css に変更のプレビュー](deploying-a-code-update/_static/image10.png)

    ダブルクリックする場合、 *Web.config*ファイル、**変更のプレビュー**ダイアログは、構成の変換、ビルドの効果を示していて、発行プロファイルの変換。 この時点で完了していないことになるもの、 *Web.config*その変更が表示されないことを想定して、変更するサーバー上のファイル。 ただし、**変更のプレビュー**ウィンドウで 2 つの変更が正しく表示されます。 2 つの XML 要素が削除されるように見えます。 選択すると、発行プロセスでこれらの要素が追加された**実行 Code First Migrations アプリケーションの起動時に**Code First のコンテキスト クラス。 発行プロセスが削除されませんが削除されているように表示されます、それらの要素を追加する前に、比較が行われます。 このエラーは、将来のリリースで修正される予定です。
5. **[閉じる]** をクリックします。
6. **[発行]** をクリックします。
7. テスト サイトのホーム ページを開くと、ブラウザー、CSS の変更の影響を確認するためにハードの更新が発生する CTRL + f5 キーを押します。

    ![CSS の変更の影響](deploying-a-code-update/_static/image11.png)
8. ブラウザーを閉じます。

### <a name="publish-specific-files-from-solution-explorer"></a>ソリューション エクスプ ローラーの特定のファイルを発行します。

背景色が青し元の色に戻す対象にしないとします。 このセクションでは、特定のファイルから直接発行することによって、元の設定を復元します**ソリューション エクスプ ローラー**します。

1. 開いている*Content/Site.css*と復元、`background-color`設定`#fff`します。
2. **ソリューション エクスプ ローラー**を右クリックし、 *Content/Site.css*ファイル。

    コンテキスト メニューでは、3 つの発行オプションを示しています。

    ![ソリューション エクスプ ローラーからオプションのパブリッシュ](deploying-a-code-update/_static/image12.png)
3. クリックして**Site.css に加えた変更のプレビュー**します。

    移行先の環境でローカル ファイルとそのバージョンの違いを表示するウィンドウが開きます。

    ![差分-コンテンツ/Site.css](deploying-a-code-update/_static/image13.png)
4. **ソリューション エクスプ ローラー**を右クリックして**Site.css**もう一度クリックします**発行 Site.css**。

    **Web 発行アクティビティ**ファイルが公開されているウィンドウに表示されます。

    ![Web 発行アクティビティ ウィンドウ](deploying-a-code-update/_static/image14.png)
5. ブラウザーを開いて、 `http://localhost/contosouniversity` URL とキーを押しますがハードに CTRL + f5 キーを変更、CSS の効果を確認するために更新します。

    ![通常の CSS を使用してホーム ページ](deploying-a-code-update/_static/image15.png)
6. ブラウザーを閉じます。

## <a name="summary"></a>まとめ

データベースの変更が関係しないアプリケーションの更新プログラムを展開するいくつかの方法が表示されました、ことを確認するどのような更新が期待どおりの変更をプレビューする方法を説明しました。 Instructors ページのようになりましたが、**指導するコース**セクション。

![Instructors ページに指導するコース](deploying-a-code-update/_static/image16.png)

次のチュートリアルでは、データベースの変更をデプロイする方法を示します。 データベースと、Instructors ページには、birthdate フィールドを追加します。

> [!div class="step-by-step"]
> [前へ](deploying-to-production.md)
> [次へ](deploying-a-database-update.md)
