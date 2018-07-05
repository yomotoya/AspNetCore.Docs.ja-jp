---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Visual Studio を使用して ASP.NET Web 展開: データベース更新の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817624"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Visual Studio を使用して ASP.NET Web 展開: データベース更新の展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルで行ったデータベースの変更と関連するコードの変更、Visual Studio での変更をテストし、テスト、ステージング、実稼働環境に更新を展開します。

DbDacFx プロバイダーを使用してデータベースを更新する方法を示します後で、およびチュートリアルが最初に Code First Migrations で管理されているデータベースを更新する方法を示します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Code First Migrations を使用してデータベースの更新を展開します。

このセクションに生年月日の列を追加、`Person`の基本クラス、`Student`と`Instructor`エンティティ。 新しい列を表示するように、インストラクター データを表示するページを更新します。 最後に、テスト、ステージング、および運用環境に変更をデプロイします。

### <a name="add-a-column-to-a-table-in-the-application-database"></a>アプリケーション データベース内のテーブルに列を追加します。

1. *ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (あります終わり波かっこの後に 2 つ)。

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    次に、更新、`Seed`メソッドを新しい列の値を提供するようにします。 開いている*migrations \configuration.cs*と置換を開始するコード ブロック`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. ソリューションをビルドしを開き、**パッケージ マネージャー コンソール**ウィンドウ。 として ContosoUniversity.DAL がまだ選択されていることを確認、**既定のプロジェクト**します。
3. **パッケージ マネージャー コンソール**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`メソッド、新しい列を作成するコードを表示できます。 `Up`メソッドは、変更を実装するときに、列を作成し、`Down`メソッドは、変更をロールバックするときに列を削除します。

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. ソリューションをビルドおよびでは、次のコマンドを入力し、**パッケージ マネージャー コンソール**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework の実行、`Up`メソッドとし、実行、`Seed`メソッド。

### <a name="display-the-new-column-in-the-instructors-page"></a>Instructors ページに新しい列を表示します。

1. ContosoUniversity プロジェクトで開きます*Instructors.aspx*し、誕生日を表示する新しいテンプレート フィールドを追加します。 雇用日とオフィスの割り当てのためのものの間に追加します。

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (コードのインデントとれていない場合は、することができますキーを押して CTRL + K し CTRL-D ファイルを自動的に再フォーマットします。)
2. アプリケーションを実行し、をクリックして、 **Instructors**リンク。

    ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールド。

    ![Instructors ページには birthdate で](deploying-a-database-update/_static/image2.png)
3. ブラウザーを閉じます。

### <a name="deploy-the-database-update"></a>データベースの更新を展開します。

1. **ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。
2. **Web の 1 クリックして発行**ツールバーで、をクリックして、**テスト**発行プロファイルをクリックして**Web の発行**します。 (ツールバーが無効になっている場合で ContosoUniversity プロジェクトを選択します**ソリューション エクスプ ローラー**。)。

    Visual Studio が更新されたアプリケーションを配置し、ホーム ページにブラウザーを開きます。
3. 実行、 **Instructors**ページを更新プログラムが正常にデプロイされたことを確認します。

    Code First アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッド。 予期される確認ページが表示されたら、**生年月日**で日付を含む列。
4. **Web の 1 クリックして発行**ツールバーで、をクリックして、**ステージング**発行プロファイルをクリックして**Web の発行**します。
5. 実行、 **Instructors**  ページで、ステージング、更新プログラムが正常にデプロイされたことを確認します。
6. **Web の 1 クリックして発行**ツールバーで、をクリックして、**運用**発行プロファイルをクリックして**Web の発行**します。
7. 実行、 **Instructors**更新プログラムが正常にデプロイされたことを確認するには、実稼働環境でのページ。

    データベースの変更を含む実際の運用アプリケーションの更新プログラムを使用してデプロイ時にアプリケーションをオフラインをも通常取得するが*アプリ\_offline.htm*、前のチュートリアルで学習したようにします。

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>DbDacFx プロバイダーを使用してデータベースの更新を展開します。

このセクションでは追加、*コメント*列を*ユーザー*メンバーシップ データベース内のテーブルし、表示し、各ユーザーのコメントを編集することができます ページを作成します。 変更は、テスト、ステージング、および運用環境に配置します。

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>メンバーシップ データベース内のテーブルに列を追加します。

1. Visual Studio で開く**SQL Server オブジェクト エクスプ ローラー**します。
2. 展開 **(localdb) \v11.0**、展開**データベース**、展開**aspnet ContosoUniversity** (いない**aspnet-ContosoUniversity-運用**)順に展開**テーブル**します。

    表示されない場合 **(localdb) \v11.0**下、 **SQL Server**ノードを右クリックし、 **SQL Server**ノードをクリックします**SQL Server の追加**します。 **サーバーへの接続**ダイアログ ボックスの入力 *(localdb) \v11.0*として、**サーバー名**、 をクリックし、 **Connect**。

    表示されない場合**aspnet ContosoUniversity**、プロジェクトを実行しを使用してログイン、*管理者*資格情報 (パスワードが*devpwd*)、し、更新、 **SQL Server オブジェクト エクスプ ローラー**ウィンドウ。
3. 右クリックし、**ユーザー**テーブル、およびクリックして**ビュー デザイナー**します。

    ![SSOX のビュー デザイナー](deploying-a-database-update/_static/image3.png)
4. デザイナーで、追加、*コメント*列、それを*nvarchar (128)* と null を許容すると、順にクリックします**Update**します。

    ![Comments 列を追加します。](deploying-a-database-update/_static/image4.png)
5. **データベース更新のプレビュー**ボックスで、 **Update Database**します。

    ![データベース更新のプレビュー](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>表示し、新しい列を編集するページを作成します。

1. **ソリューション エクスプ ローラー**を右クリックし、**アカウント**ContosoUniversity プロジェクトのフォルダーをクリックして**追加**、 をクリックし、**新しい項目の**.
2. 新規作成**Web フォームを使用してマスター ページ**名前を付けます*UserInfo.aspx*します。 既定値を受け入れる*Site.Master*のマスター ページとしてファイル。
3. 次のマークアップをコピー、 `MainContent` `Content`要素 (最後に、3`Content`要素)。

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. 右クリックし、 *UserInfo.aspx*ページし、クリックして**ブラウザーで表示**します。
5. ログイン、*管理者*ユーザーの資格情報 (パスワードが*devpwd*) し、ユーザーは、ページが正しく動作することを確認するいくつかのコメントを追加します。

    ![UserInfo ページ](deploying-a-database-update/_static/image6.png)
6. ブラウザーを閉じます。

## <a name="deploy-the-database-update"></a>データベースの更新を展開します。

を dbDacFx プロバイダーを使用して展開するには、だけを選択する必要が、 **Update database** 、発行プロファイルのオプション。 ただし、初期のデプロイにこのオプションを使用したときにも構成したいくつか追加の SQL スクリプトを実行する: プロファイルにまだいるし、再び実行されないようにする必要があります。

1. 開く、 **Web の発行**を ContosoUniversity プロジェクトを右クリックしてウィザード**発行**します。
2. 選択、**テスト**プロファイル。
3. をクリックして、**設定**タブ。
4. **DefaultConnection**、 **Update database**。
5. 初期デプロイを実行するように構成する追加のスクリプトを無効にします。

    1. クリックして**データベース更新を構成する**します。
    2. **データベース更新を構成する**ダイアログ ボックスで、クリア、チェック ボックスを横に*Grant.sql*と*aspnet-データ-dev.sql*します。
    3. **[閉じる]** をクリックします。
6. をクリックして、**プレビュー**タブ。
7. **データベース**の右側に**DefaultConnection**、クリックして、**プレビュー データベース**リンク。

    ![データベースのプレビュー](deploying-a-database-update/_static/image7.png)

    プレビュー ウィンドウには、ソース データベースのスキーマに一致するデータベース スキーマを転送先データベースで実行されるスクリプトが表示されます。 スクリプトには、新しい列を追加する ALTER TABLE コマンドが含まれています。
8. 閉じる、**データベース プレビュー**クリックしてダイアログ ボックスで、**発行**します。

    Visual Studio が更新されたアプリケーションを配置し、ホーム ページにブラウザーを開きます。
9. UserInfo ページの実行 (追加*Account/UserInfo.aspx*ホーム ページの URL に)、更新プログラムが正常にデプロイされたことを確認します。 入力してログインする必要があります*管理者*と*devpwd*します。

    既定では、テーブル内のデータを展開していないと、開発で追加したコメントを検索しないように、実行するにはデータ デプロイ スクリプトを構成しませんでした。 新しいコメントを今すぐ追加するには、変更がデータベースに展開されたことと、ページが正常に機能を確認するステージングします。
10. ステージングと運用環境に展開するのと同じ手順に従います。

    忘れずに余分なスクリプトを無効にします。 テスト プロファイルに対する唯一の違いは、ステージング環境で 1 つだけのスクリプトを無効にするには、実稼働プロファイルのみを実行するように構成されたため、 *aspnet-prod-data.sql*します。

    ステージングと運用環境の資格情報は、管理者と prodpwd です。

    データベースの変更を含む実際の運用アプリケーションの更新プログラムをアップロードしてデプロイ時にアプリケーションをオフラインをも通常実行するは*アプリ\_offline.htm*発行を削除する前にその後で見たよう[、前のチュートリアル](deploying-a-code-update.md)します。

## <a name="summary"></a>まとめ

Code First Migrations と dbDacFx プロバイダーの両方を使用してデータベースの変更を含むアプリケーションの更新プログラムをデプロイしたようになりました。

![Instructors ページには birthdate で](deploying-a-database-update/_static/image8.png)

![UserInfo ページ](deploying-a-database-update/_static/image9.png)

次のチュートリアルでは、コマンドラインを使用してデプロイを実行する方法を示します。

> [!div class="step-by-step"]
> [前へ](deploying-a-code-update.md)
> [次へ](command-line-deployment.md)
