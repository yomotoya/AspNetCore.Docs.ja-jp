---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: Code-Only 更新 - 12 の 8 の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16a9e48034536964d526717ecde2a9b813818963
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816085"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: Code-Only 更新 - 12 の 8 の展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

初期のデプロイ後の保守および web サイトの開発作業が引き続き発生してまで長い間は、更新プログラムを展開します。 このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセス説明。 この更新プログラムがデータベースの変更が含まれない次のチュートリアルでデータベースの変更の展開に関する相違点を確認します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="making-a-code-change"></a>コードの変更

アプリケーションへの更新プログラムの簡単な例とに追加します、 **Instructors**ページ、選択したインストラクターが指導するコースのリスト。

実行する場合、 **Instructors**ページが表示されます**選択**グリッド内のリンクが何もしないこと以外、行の背景が灰色に変わりますが、します。

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

追加するときに実行するコード、**選択**リンクがクリックされ、選択したインストラクター指導するコースの一覧を表示します。

*Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

ページを実行し、インストラクターを選択します。 そのインストラクターが指導するコースのリストを参照してください。

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>コードの更新をテスト環境に展開します。

テスト環境に展開するが、実行 1 回のクリックの簡単な処理をもう一度発行します。 このプロセスを短縮するためには、使用することができます、 **Web の 1 クリックして発行**ツールバー。

**ビュー** ] メニューの [選択**ツールバー**選び**Web の 1 クリックして発行**します。

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

**ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。

**Web の 1 クリックして発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印の付いたアイコン)。

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio が更新されたアプリケーションを配置し、ブラウザーはホーム ページに自動的に開きます。 Instructors ページを実行し、更新プログラムが正常にデプロイされたことを確認するインストラクターを選択します。

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

通常どおり回帰テストを行うことも (つまり、新しい変更が、既存の機能を中断していないことを確認するには、サイトの残りの部分をテストする)。 ただし、このチュートリアルではその手順をスキップし、更新プログラムを運用環境に配置を続行します。

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>運用環境への初期データベースの状態の再デプロイを防止

実際のアプリケーションで、初期のデプロイ後、ユーザーが、実稼働サイトと対話し、データベースには、ライブ データが設定されます。 そのため、すべてのライブ データをワイプすると、初期の状態のメンバーシップ データベースを再デプロイしたくないです。 SQL Server Compact データベースはファイルであるため、*アプリ\_データ*フォルダー内のファイルを展開の設定を変更することでこれを回避する必要がある、*アプリ\_データ*フォルダーデプロイはありません。

開く、**プロジェクトのプロパティ**ContosoUniversity プロジェクト、および選択のウィンドウ、**パッケージ化/発行 Web**タブ。確認、**構成**ドロップダウン ボックスに**アクティブ (リリース)** または**リリース**選択選択すると、 **アプリからファイルを除外する\_データ フォルダー**します。

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

デバッグ ビルドを今後の展開を決定する場合は、デバッグ ビルド構成の同じ変更を加えることをお勧め: 変更**構成**に**デバッグ**選び**を除外します。アプリからファイル\_データ フォルダー**します。

保存して閉じます、**パッケージ化/発行 Web**タブ。

> [!NOTE] 
> 
> [!IMPORTANT]
> ないことを確認**転送先に追加のファイルを削除**発行プロファイルで選択されています。 展開プロセスがアプリ内にあるデータベースを削除するオプションを選択した場合\_でデプロイされたサイトとそのデータがアプリを削除\_データ フォルダー自体。


## <a name="preventing-user-access-to-the-production-site-during-update"></a>更新中に、実稼働サイトへのユーザー アクセスを防止

デプロイする変更は、1 つのページを簡単に変更を今すぐです。 大規模な変更を展開することがあります、その場合は、サイトことができます動作妙デプロイが完了する前に、ユーザーがページを要求する場合。 これを防ぐためには、使用することができます、*アプリ\_offline.htm*ファイル。 という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーで IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。 アクセスを防ぐため、デプロイ時に、配置するように*アプリ\_offline.htm* 、ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*します。

**ソリューション エクスプ ローラー**(いない、プロジェクトの 1 つ) ソリューションを右クリックし、選択、**新しいソリューション フォルダー**します。

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

フォルダーの名前*SolutionFiles*します。

という名前の HTML ページを作成、新しいフォルダーに*アプリ\_offline.htm*します。 既存の内容を次のマークアップに置き換えます。

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

コピーすることができます、*アプリ\_offline.htm*ファイルを FTP 接続を使用して、サイト、または**ファイル マネージャー**ホスティング プロバイダーのコントロール パネルの ユーティリティ。 このチュートリアルで使用する、**ファイル マネージャー**します。

コントロール パネルを開き、選択**ファイル マネージャー**で行ったように、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。 選択**contosouniversity.com**し**wwwroot**をクリックして、アプリケーションのルート フォルダーに**アップロード**します。

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

**ファイルのアップロード**ダイアログ ボックスで、*アプリ\_offline.htm*ファイルをクリックして**アップロード**します。

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

サイトの URL に移動します。 参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されるようになりました。

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

運用環境にデプロイする準備が整いました。

## <a name="deploying-the-code-update-to-the-production-environment"></a>運用環境にコードの更新を展開します。

**Web の 1 クリックして発行**ツールバーで、選択、**運用**発行プロファイルをクリックして**Web の発行**します。

Visual Studio は、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーで開きます。 *アプリ\_offline.htm*ファイルが表示されます。 削除する必要がありますを正常にデプロイを確認するテストする前に、*アプリ\_offline.htm*ファイル。

戻り、**ファイル マネージャー**コントロール パネルの アプリケーション。 選択**contosouniversity.com**と**wwwroot**を選択します**アプリ\_offline.htm**、 をクリックし、**削除**します。

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

ブラウザーで、パブリックのサイトで、Instructors ページを開くし、更新プログラムが正常にデプロイされたことを確認する、インストラクターを選択します。

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

これで、データベースの変更を使用してアプリケーションの更新プログラムをデプロイしました。 次のチュートリアルでは、データベースの変更をデプロイする方法を示します。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
