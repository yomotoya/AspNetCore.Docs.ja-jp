---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 8 - Code-Only 更新の展開 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: a07ac968482866ba9a4eca25722d99fd641080e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 8 - Code-Only 更新の展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

初期展開後の維持と、web サイトの開発作業を続行、および時間の長い前にする更新プログラムを展開します。 このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセスです。 この更新プログラムがデータベースの変更を含まない次のチュートリアルではデータベースの変更の展開の違いは何が表示されます。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="making-a-code-change"></a>コードの変更

単純な例として、更新プログラムをアプリケーションに、追加、**講習においてインストラクター**  ページで選択した講師コースの一覧です。

実行する場合、**講習においてインストラクター**  ページで、かがわかりますがある**選択**グリッド内のリンクを行わない make 以外の何か、行の背景が灰色に変わりますが、します。

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

これで、ときに実行されるコードを追加、**選択**リンクがクリックされ、選択した講師でコースの一覧を表示します。

*Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

ページを実行し、インストラクターを選択します。 そのインストラクターでコースの一覧を参照してください。

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>コードの更新をテスト環境に展開します。

テスト環境に展開すると、実行中の 1 回のクリックの単純な処理をもう一度発行です。 このプロセスを短縮するためには、使用することができます、 **Web 1 つをクリックして 発行**ツールバー。

**ビュー**  メニューの 選択**ツールバー**し、 **Web 1 つをクリックして 発行**です。

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

**ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。

**Web 1 つをクリックして 発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印のアイコン)。

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio 更新済みのアプリケーションを配置して、ブラウザーは、ホーム ページに自動的に開きます。 講習においてインストラクター ページを実行し、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

通常どおりも回帰テストを行う (つまり、新しい変更で、既存の機能が損なわれていないことを確認するサイトの残りの部分をテストする)。 このチュートリアルでをその手順をスキップし、実稼働環境に、更新プログラムの展開を続行します。

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>実稼働環境に初期データベースの状態の再展開を防止

実際のアプリケーションでユーザーが、最初の展開後に、実稼働サイトと対話およびデータベースには、ライブ データが挿入されます。 そのため、すべてのライブ データをワイプすると、初期状態で、メンバーシップ データベースを再配置したくないです。 SQL Server Compact データベースが内のファイルであるため、*アプリ\_データ*フォルダー内のファイルを展開の設定を変更することによってこれを防ぐにする必要がある、*アプリ\_データ*フォルダー展開されません。

開く、**プロジェクト プロパティ**を選択して ContosoUniversity プロジェクト ウィンドウ、**パッケージ化/発行 Web**  タブ。確認して、**構成**ドロップ ダウン ボックスにはいずれかの**アクティブ (リリース)**または**リリース**選択すると、選択**アプリからファイルを除外する\_データ フォルダー**です。

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

デバッグ ビルド構成に対して同じ変更を行うことをお勧めは、後で、デバッグ ビルドを配置する場合に備えて: 変更**構成**に**デバッグ**し、**を除外します。アプリからファイル\_データ フォルダー**です。

保存して閉じます、**パッケージ化/発行 Web**タブです。

> [!NOTE] 
> 
> [!IMPORTANT]
> いないことを確認してください**先で追加ファイルを削除する**発行プロファイルで選択します。 アプリ内にあるデータベース オプションを選択した場合、展開プロセスは削除されます\_アプリケーションの展開済みのサイトとのデータが削除されます\_データ フォルダー自体です。


## <a name="preventing-user-access-to-the-production-site-during-update"></a>更新中に、実稼働サイトへのユーザー アクセスを防止

配置する変更は、単純な変更を単一のページを今すぐです。 規模の大きな変更を展開することもありますその場合は、サイトことができますと動作と異常展開が完了する前に、ユーザーがページを要求する場合。 これを防ぐためには、使用することができます、*アプリ\_offline.htm*ファイル。 という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーに IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。 アクセスを防ぐため、配置時に、配置するように*アプリ\_offline.htm*ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*です。

**ソリューション エクスプ ローラー**(いないプロジェクトの 1 つ)、ソリューションを右クリックし、選択、**新しいソリューション フォルダー**です。

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

名前のフォルダー *SolutionFiles*です。

名前付き HTML ページを作成、新しいフォルダーに*アプリ\_offline.htm*です。 既存の内容を次のマークアップに置き換えます。

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

コピーすることができます、*アプリ\_offline.htm*ファイルを FTP 接続を使用して、サイト、または**ファイル マネージャー**ホスティング プロバイダーのコントロール パネルの ユーティリティです。 このチュートリアルでは、使用するので、**ファイル マネージャー**です。

コントロール パネルを開き、選択**ファイル マネージャー**で行ったよう、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。 選択**contosouniversity.com**し**wwwroot** 、アプリケーションのルート フォルダーを取得し、をクリックする**アップロード**です。

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

**ファイルのアップロード**ダイアログ ボックスで、*アプリ\_offline.htm*ファイルし、をクリックして**アップロード**です。

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

サイトの URL を参照します。 参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されます。

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

実稼働環境に展開する準備が整いました。

## <a name="deploying-the-code-update-to-the-production-environment"></a>コードの更新を実稼働環境に展開します。

**Web 1 つをクリックして 発行**ツールバーで、選択、**運用**発行プロファイルをクリックして**Web の発行**です。

Visual Studio では、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーが開きます。 *アプリ\_offline.htm*ファイルが表示されます。 展開の成功を確認するテストすることができます、前に削除する必要があります、*アプリ\_offline.htm*ファイル。

戻り、**ファイル マネージャー**コントロール パネルの アプリケーションです。 選択**contosouniversity.com**と**wwwroot**を選択**アプリ\_offline.htm**、クリックして**削除**です。

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

ブラウザーで、パブリックのサイトで講習においてインストラクター ページを開き、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

データベースの変更は含まれませんが、アプリケーションの更新プログラムを展開したようになりました。 次のチュートリアルでは、データベースの変更を配置する方法を示します。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[次へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
