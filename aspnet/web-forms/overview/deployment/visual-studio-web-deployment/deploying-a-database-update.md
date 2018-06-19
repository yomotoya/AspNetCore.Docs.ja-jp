---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Visual Studio を使用した ASP.NET Web 展開: データベース更新の展開 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892622"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Visual Studio を使用した ASP.NET Web 展開: データベース更新の展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルで行うデータベースの変更と関連するコードの変更、Visual Studio で、変更をテストし、テスト、ステージング、および運用環境を更新プログラムを展開します。

DbDacFx プロバイダーを使用してデータベースを更新する方法を示します後で、およびチュートリアルが最初に Code First Migrations で管理されているデータベースを更新する方法を示します。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Code First Migrations を使用して、データベースの更新を展開します。

このセクションで、生年月日に列を追加する、`Person`の基本クラス、`Student`と`Instructor`エンティティです。 新しい列を表示するように、インストラクター データを表示するページを更新します。 最後に、テスト、ステージング、および実稼働環境に変更を展開します。

### <a name="add-a-column-to-a-table-in-the-application-database"></a>アプリケーション データベース内のテーブルに列を追加します。

1. *ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (存在する必要があります終わり波かっこの後に続く 2)。

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    次に、更新、`Seed`メソッドが新しい列の値を提供するようにします。 開いている*Migrations\Configuration.cs*が開始されるコード ブロックを置き換える`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. ソリューションをビルドし、開きます、 **Package Manager Console**ウィンドウです。 ContosoUniversity.DAL として選択されていることを確認してください、**既定のプロジェクト**です。
3. **Package Manager Console**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`方法、新しい列を作成するコードを確認することができます。 `Up`メソッドは、変更を実装しているときに、列を作成し、`Down`メソッドは、変更をロールバックするときに、列を削除します。

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. ソリューションをビルドしに次のコマンドを入力、 **Package Manager Console**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework が実行される、`Up`メソッドとし、実行、`Seed`メソッドです。

### <a name="display-the-new-column-in-the-instructors-page"></a>講習においてインストラクター ページで、新しい列を表示します。

1. ContosoUniversity プロジェクトで開きます*Instructors.aspx*生年月日を表示する新しいテンプレート フィールドを追加します。 雇用日とオフィス割り当てのためのものの間に追加します。

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (場合はコードのインデントがとれていない状態を取得することができますキーを押して CTRL K し CTRL-D ファイルを自動的に書式設定を変更します。)
2. アプリケーションを実行し、をクリックして、**講習においてインストラクター**リンクします。

    ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールドです。

    ![講習においてインストラクター birthdate ページ](deploying-a-database-update/_static/image2.png)
3. ブラウザーを閉じます。

### <a name="deploy-the-database-update"></a>データベースの更新を展開します。

1. **ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。
2. **Web 1 つをクリックして 発行**ツールバーで、をクリックして、**テスト**発行プロファイルをクリックして**Web の発行**です。 (ツールバーを無効にした場合で ContosoUniversity プロジェクトを選択**ソリューション エクスプ ローラー**)。

    Visual Studio は、更新済みのアプリケーションを配置し、ホーム ページにブラウザーを開きます。
3. 実行、**講習においてインストラクター**ページを更新プログラムが正常に展開されたことを確認します。

    Code First、アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッドです。 予期される表示ページが表示されたら、**生年月日**で日付を含む列。
4. **Web 1 つをクリックして 発行**ツールバーで、をクリックして、**ステージング**発行プロファイルをクリックして**Web の発行**です。
5. 実行、**講習においてインストラクター**ステージング環境の更新プログラムが正常に展開されたことを確認するページ。
6. **Web 1 つをクリックして 発行**ツールバーで、をクリックして、**運用**発行プロファイルをクリックして**Web の発行**です。
7. 実行、**講習においてインストラクター**更新プログラムが正常に展開されたことを確認するには、実稼働環境でのページです。

    データベースの変更を含む実際の運用アプリケーションの更新プログラムの通常実行する配置時にアプリケーションをオフラインを使用して*アプリ\_offline.htm*前のチュートリアルで説明したとおり、します。

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>DbDacFx プロバイダーを使用して、データベースの更新を展開します。

このセクションでは追加、*コメント*列を*ユーザー*メンバーシップ データベースにテーブルが表示され、ページを表示し、各ユーザーのコメントを編集することができますを作成します。 その変更は、テスト、ステージング、および実稼働環境を展開します。

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>メンバーシップ データベース内のテーブルに列を追加します。

1. Visual Studio で開く**SQL Server オブジェクト エクスプ ローラー**です。
2. 展開 **(localdb) \v11.0**、展開**データベース**、展開**aspnet ContosoUniversity** (いない**aspnet ContosoUniversity 本番**)展開し、**テーブル**です。

    表示されない場合 **(localdb) \v11.0**下にある、 **SQL Server**  ノードを右クリックし、 **SQL Server**ノードをクリック**SQL Server の追加**です。 **サーバーへの接続** ダイアログ ボックスに「 *(localdb) \v11.0*として、**サーバー名**、クリックして**接続**です。

    表示されない場合**aspnet ContosoUniversity**、プロジェクトを実行しを使用してログイン、 *admin*資格情報 (パスワードが*devpwd*)、し、更新、 **SQL Server オブジェクト エクスプ ローラー**ウィンドウです。
3. 右クリックし、**ユーザー**テーブル、およびクリックして**ビュー デザイナー**です。

    ![SSOX ビュー デザイナー](deploying-a-database-update/_static/image3.png)
4. デザイナーで、追加、*コメント*列、それを*nvarchar (128)* null を許可すると、をクリックし、**更新**です。

    ![Comments 列を追加します。](deploying-a-database-update/_static/image4.png)
5. **データベース更新のプレビュー**ボックスで、クリックして**更新データベース**です。

    ![データベース更新のプレビュー](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>表示および新しい列を編集するページを作成します。

1. **ソリューション エクスプ ローラー**を右クリックし、**アカウント**ContosoUniversity プロジェクト内のフォルダーをクリックして**追加**、クリックして**新しい項目の**.
2. 新しい**Web フォームを使用してマスター ページ**し名前を付けます*UserInfo.aspx*です。 既定値を受け入れる*Site.Master*マスター ページとしてファイル。
3. 次のマークアップをコピー、 `MainContent` `Content`要素 (3 の最後の`Content`要素)。

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. 右クリックし、 *UserInfo.aspx*ページし、をクリックして**ブラウザーで表示**です。
5. ログインに使用、 *admin*ユーザーの資格情報 (パスワードが*devpwd*) し、ページが正しく動作することを確認するユーザーにいくつかのコメントを追加します。

    ![ユーザー情報 ページ](deploying-a-database-update/_static/image6.png)
6. ブラウザーを閉じます。

## <a name="deploy-the-database-update"></a>データベースの更新を展開します。

DbDacFx プロバイダーを使用して、展開するだけ選択する必要が、**更新データベース**発行プロファイルでオプションです。 ただし、最初の展開は、このオプションを使用したときにも構成したいくつか追加の SQL スクリプトを実行する: まだプロファイルであるものと、もう一度実行されないようにする必要があります。

1. 開く、 **Web の発行**ContosoUniversity プロジェクトを右クリックしをクリックしてウィザード**発行**です。
2. 選択、**テスト**プロファイル。
3. クリックして、**設定**タブです。
4. **DefaultConnection****更新データベース**です。
5. 最初の展開を実行するように構成する追加のスクリプトを無効にします。

    1. をクリックして**データベース更新を構成する**です。
    2. **データベース更新を構成する**ダイアログ ボックスで、チェック ボックスをオフのチェック ボックス の横に*Grant.sql*と*aspnet データ-dev.sql*です。
    3. **[閉じる]** をクリックします。
6. クリックして、**プレビュー**タブです。
7. [**データベース**の右側および**DefaultConnection**、] をクリックして、**プレビューのデータベース**リンクします。

    ![データベースのプレビュー](deploying-a-database-update/_static/image7.png)

    プレビュー ウィンドウでは、ソース データベースのスキーマに一致するデータベース スキーマを作成する対象データベースで実行されるスクリプトを示します。 スクリプトには、新しい列を追加する ALTER TABLE コマンドが含まれています。
8. 閉じる、**データベース プレビュー**クリックしてダイアログ ボックスで、**発行**です。

    Visual Studio は、更新済みのアプリケーションを配置し、ホーム ページにブラウザーを開きます。
9. ユーザー情報 ページの実行 (追加*Account/UserInfo.aspx*ホーム ページの URL を)、更新プログラムが正常に展開されたことを確認します。 入力してログインする必要があります*admin*と*devpwd*です。

    既定では、テーブル内のデータが展開されていないと、開発で追加したコメントを検索しないように、実行するには、データの配置スクリプトを構成しませんでした。 新しいコメントを今すぐ追加するにはステージング環境の変更がデータベースに配置され、ページが正しく動作することを確認します。
10. ステージングと運用環境に展開する同じ手順に従います。

    余分なスクリプトを無効にしてください。 テストのプロファイルと比較して唯一の違いは、ステージング環境の 1 つだけのスクリプトを無効にするには、実稼働プロファイルのみを実行するように構成されたため*aspnet prod-data.sql*です。

    ステージングと運用環境の資格情報は、管理者、prodpwd がします。

    データベースの変更を含む実際の運用アプリケーションの更新プログラムのも通常実行する配置時にアプリケーションをオフラインをアップロードして*アプリ\_offline.htm*発行を削除する前にその後で学習したよう[前のチュートリアル](deploying-a-code-update.md)です。

## <a name="summary"></a>まとめ

Code First Migrations および dbDacFx プロバイダーの両方を使用してデータベースの変更が含まれているアプリケーションの更新プログラムを展開したようになりました。

![講習においてインストラクター birthdate ページ](deploying-a-database-update/_static/image8.png)

![ユーザー情報 ページ](deploying-a-database-update/_static/image9.png)

次のチュートリアルでは、コマンドラインを使用して、展開を実行する方法を示します。

> [!div class="step-by-step"]
> [前へ](deploying-a-code-update.md)
> [次へ](command-line-deployment.md)
