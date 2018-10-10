---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'ハンズ オン ラボ: 保守性の高い Azure の web サイト: 変更とスケールの管理 |Microsoft Docs'
author: rick-anderson
description: このラボでは、どの Microsoft Azure 簡単にビルドして、web サイトを運用環境にデプロイについて説明します。
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: bc6de2f0c8b2cd958c198abb90fc4ad97613e973
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913308"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>ハンズ オン ラボ: 保守性の高い Azure の web サイト: 変更とスケールの管理
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> Microsoft Azure では、簡単にビルドし、web サイトを運用環境にデプロイできます。 アプリケーションのライブが完了して、始めたばかりという方します。 変化する要件、データベースの更新、スケール、および詳細を処理する必要があります。 さいわい、Azure App Service では、多数の機能をスムーズに実行されている、サイトを保護するために、対応していますが。
>
> Azure では、セキュリティで保護された柔軟性の高い開発、展開、および拡張オプションをあらゆるサイズの web アプリケーションを提供します。 インフラストラクチャの管理の負担をかけずアプリケーションの展開を作成し、既存のツールを活用します。
>
> 運用環境の web アプリケーションを自分で数分でプロビジョニング、好みの開発ツールを使用して作成されたコンテンツを簡単にデプロイすることで。 対応のソース管理から直接既存のサイトを展開する**Git**、 **GitHub**、 **Bitbucket**、 **TFS**とでも**DropBox**します。 お気に入りの IDE から直接、またはを使用してスクリプトから展開**PowerShell**で Windows または**CLI**あらゆる OS で実行されるツールです。 展開した後、サイトを継続的なデプロイのサポートを常に最新の状態のままにします。
>
> Azure にはスケーラブルで持続性のあるクラウド ストレージ、バックアップ、および回復ソリューションのすべてのデータでは、小規模または大規模な。 実稼働環境では、テーブル、Blob、SQL データベースなどのストレージ サービスにアプリケーションをデプロイするときに、クラウドでアプリケーションがスケール アップできます。
>
> SQL データベースが新しいバージョンのアプリケーションをデプロイするときに、生産性の高いデータベースを最新に重要です。 方々 に感謝**Entity Framework Code First Migrations**、分単位でお使いの環境を更新する、開発と、データ モデルの展開が簡素化されています。 このハンズオン ラボでは、web アプリを Microsoft Azure で運用環境にデプロイするときに発生する可能性があります、さまざまなトピックを示します。
>
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。
>
> このトピックの詳しい内容について詳細を参照してください、 [Azure 電子書籍の実際のクラウド アプリの構築](building-real-world-cloud-apps-with-windows-azure/introduction.md)します。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- 既存のモデルで Entity Framework の移行を有効にします。
- オブジェクト モデルと Entity Framework の移行を適宜使用してデータベースを更新します。
- Git を使用して Azure App Service にデプロイします。
- Azure Management portal を使用して以前の展開にロールバックします。
- Azure Storage を使用して web アプリをスケーリング
- Azure 管理ポータルを使用して web アプリの自動スケーリングの構成します。
- 作成し、Visual Studio でロード テストのプロジェクトを構成します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上
- [Azure SDK for .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT バージョン管理システム](http://git-scm.com/download)
- Microsoft Azure サブスクリプション

    - サインアップ、[無料試用版](http://aka.ms/watk-freetrial)
    - アクティブ Visual Studio Professional、Test Professional、Premium または Ultimate with MSDN または MSDN Platforms のサブスクライバーに、 [MSDN の特典](http://aka.ms/watk-msdn)Azure で開発とテストを開始するようになりました
    - [BizSpark](http://aka.ms/watk-bizspark)メンバーは、Azure に自動的に受信、Visual Studio Ultimate with MSDN サブスクリプション特典
    - メンバー、 [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials プログラムが無料で毎月の Azure クレジットを受け取る

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。

1. Windows Explorer をラボの [参照] を開いて**ソース**フォルダー。
2. 右クリックして**Setup.cmd**選択と**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。
3. ユーザー アカウント制御 ダイアログが表示されている場合は、操作を続行を確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットの使用

ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。 演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [Entity Framework の移行を使用します。](#Exercise1)
2. [ステージング Web アプリを展開します。](#Exercise2)
3. [運用環境でデプロイのロールバックを実行します。](#Exercise3)
4. [Azure Storage を使用したスケーリング](#Exercise4)
5. [Web アプリの自動スケールを使用して](#Exercise5)(Visual Studio 2013 Ultimate エディションの省略可能)

この演習の所要時間を推定: **75 分**

> [!NOTE]
> Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。 このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。 開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>手順 1: Entity Framework の移行を使用します。

アプリケーションを開発しているときに、データ モデルが時間の経過と共に変更可能性があります。 これらの変更に影響する可能性、データベース内の既存のモデル (新しいバージョンを作成する) 場合と、データベースのエラーを防ぐために最新の状態を維持することが重要です。

モデルのこれらの変更の追跡を簡略化する**Entity Framework Code First Migrations**自動的にデータベース スキーマを使用してモデルを比較する変更を検出し、データベースを更新する特定のコードを生成します新規作成*バージョン*データベースの。

この演習は、有効にする方法を示します**移行**アプリケーションと簡単に検出しする方法、データベースを更新する変更を生成します。

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>タスク 1 – Migrations の有効化

このタスクで有効にすると次の手順を参照してください**Entity Framework Code First Migrations**を**ギーク Quiz**データベース、モデルを変更してでこれらの変更を反映する方法を理解すること、データベース。

1. Visual Studio を開き、開く、 **GeekQuiz.sln**ソリューション ファイルから**Source\Ex1 UsingEntityFrameworkMigrations\Begin**します。
2. ダウンロードしてインストールするためにソリューションをビルド、 **NuGet**依存関係をパッケージ化します。 これを行うには、ソリューションを右クリックし、をクリックして**ソリューションのビルド**またはキーを押します**Ctrl + Shift + B**します。
3. **ツール** メニューの選択 Visual Studio で**NuGet パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。
4. **パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。 既存のモデルに基づく最初の移行が作成されます。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![移行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Migrations を有効にします。")

    *移行を有効にします。*

    > [!NOTE]
    > このコマンドを追加、**移行**というフォルダーにファイルを含むギーク Quiz プロジェクト**Configuration.cs**します。 **構成**クラスでは、コンテキストの移行の動作を構成できます。
5. 移行が有効になっている、更新する必要があります、**構成**初期データと共にデータベースを設定するクラスを**ギーク Quiz**が必要です。 **移行**、置換、 **Configuration.cs**に 1 つのファイルがある、 **Source\Assets**このラボのフォルダー。

    > [!NOTE]
    > **移行**が呼び出す、**シード**メソッドすべてのデータベース更新をデータベース内のレコードが重複していないことを確認する必要があります。 **AddOrUpdate**方法は、データの重複を防ぐために役立ちます。
6. 最初の移行を追加するに次のコマンドを入力し、キーを押します**Enter**します。

    > [!NOTE]
    > という名前のデータベースがないことを確認&quot;GeekQuizProd&quot; LocalDB インスタンスにします。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![ベース スキーマの移行を追加する](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "追加ベース スキーマの移行")

    *ベース スキーマの移行を追加します。*

    > [!NOTE]
    > **Add-migration**最後の移行が作成されたため、モデルに加えられた変更内容に基づいて次の移行をスキャフォールディングします。 ここでは、プロジェクトの最初の移行は、これはスクリプトを追加で定義されているすべてのテーブルを作成する、 **TriviaContext**クラス。
7. 次のコマンドを実行して、データベースの更新への移行を実行します。 このコマンドを指定します、 **Verbose**先データベースに適用される SQL ステートメントを表示するフラグ。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![最初のデータベースを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "初期データベースを作成します。")

    *初期データベースを作成します。*

    > [!NOTE]
    > **Update Database**保留中の移行をデータベースに適用されます。 この場合、web.config ファイルで定義されている接続文字列を使用してデータベースを作成、されます。
8. 移動して**ビュー**メニューを開く**SQL Server オブジェクト エクスプ ローラー**します。

    ![SQL Server オブジェクト エクスプ ローラーで開く](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server オブジェクト エクスプ ローラーで開く")

    *SQL Server オブジェクト エクスプ ローラーで開く*
9. **SQL Server オブジェクト エクスプ ローラー**ウィンドウを右クリックして、LocalDB インスタンスに接続、 **SQL Server**ノードと選択**SQL Server の追加.** オプション。

    ![SQL Server のインスタンスを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "SQL Server のインスタンスを追加します。")

    *SQL Server オブジェクト エクスプ ローラーへの SQL Server インスタンスの追加*
10. 設定、**サーバー名**に *(localdb) \v11.0*して**Windows 認証**認証モードとして。 クリックして**Connect**を続行します。

    ![LocalDB への接続](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "LocalDB への接続")

    *LocalDB への接続*
11. 開く、 **GeekQuizProd**データベースし、展開、**テーブル**ノード。 ご覧のとおり、 **Update-database**コマンドで定義されているすべてのテーブルを生成する、 **TriviaContext**クラス。 検索、 **dbo します。TriviaQuestions**テーブルし、列のノードを開きます。 次のタスクでは、このテーブルに新しい列を追加し、データベースを使用して、更新**移行**します。

    ![トリビアは、列を質問](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "トリビアの列の質問")

    *トリビアの列を質問します。*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>タスク 2 – Migrations を使用してデータベース スキーマの更新

このタスクで使用する**Entity Framework Code First Migrations**モデルの変更を検出し、データベースを更新するために必要なコードを生成します。 更新は、 **TriviaQuestions**エンティティに新しいプロパティを追加しています。 テーブルに新しい列を含めるには新しい移行を作成するためのコマンドを実行します。

1. **ソリューション エクスプ ローラー**、ダブルクリックして、 **TriviaQuestion.cs**ファイル内にある、**モデル**フォルダー。
2. という名前の新しいプロパティを追加**ヒント**の次のコード スニペットに示すようにします。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. **パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。 私たちのモデルの変更を反映した新しい移行が作成されます。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-migration")

    *追加の移行*

    > [!NOTE]
    > 移行ファイルが 2 つの方法で構成される**を**と**ダウン**します。
    >
    > - **を**どのような変更をデータベースに適用する、アプリケーションのニーズの現在のバージョンの指定に使用されるメソッド。
    > - **ダウン**に追加した変更を取り消すために使用、**を**メソッド。
    >
    > データベースの移行では、データベースを更新と、は、タイムスタンプの順序と前回の更新以降に使用されていないものだけですべての移行が実行されます (、 \_MigrationHistory テーブルの追跡どの移行が適用されています)。 **を**すべての移行のメソッドが呼び出され、データベースを指定して、変更すると、します。 以前の移行に戻るになった場合、**ダウン**逆の順序の変更を元に戻すメソッドが呼び出されます。
4. **パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. 出力、 **Update-database**生成されたコマンド、 **Alter Table**に新しい列を追加する SQL ステートメント、 **TriviaQuestions**テーブル、次の図に示すようにします。

    ![生成される SQL ステートメントの列を追加](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "生成される SQL ステートメントの列の追加")

    *生成される SQL ステートメントの列を追加します。*
6. **SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブル、いることを確認し、新しい**ヒント**列が表示されます。

    ![新しいヒント列を示す](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "新しいヒント列の表示")

    *新しいヒント列の表示*
7. 戻り、 **TriviaQuestion.cs**エディター、追加、 **StringLength**に制約、*ヒント*プロパティは、次のコード スニペットに示すようにします。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. **パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. **パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. 出力、 **Update-database**生成されたコマンド、 **Alter Table**を更新する SQL ステートメント、*ヒント*の列の型、 **TriviaQuestions**テーブル、次の図に示すようにします。

    ![生成される SQL ステートメントの列を alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL ステートメントの生成")

    *Alter column SQL ステートメントの生成*
11. **SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブルが表示されことを確認、**ヒント**列の型が**nvarchar(150)** します。

    ![新しい制約を示す](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "新しい制約を示す")

    *新しい制約を示す*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>手順 2: ステージング Web アプリを展開します。

**Web アプリを Azure App Service で**ステージングされた発行を実行することができます。 ステージングされた発行では、既定の運用サイトごとにステージング サイト スロットを作成し、ダウンタイムなしでこれらのスロットをスワップすることができます。 これは、パブリックにリリースする前に変更を検証、段階的サイトのコンテンツを統合し、変更が期待どおりに動作していない場合はロールバックを本当に便利です。

この演習ではデプロイ、**ギーク Quiz** Git ソース管理を使用して web アプリのステージング環境へのアプリケーション。 これを行うが web アプリを作成し、管理ポータルで必要なコンポーネントのプロビジョニング、構成、 **Git**リポジトリとプッシュ アプリケーション ソース コードをローカル コンピューターから、ステージング スロットにします。 実稼働データベースを更新することもが、 **Code First Migrations**前の演習で作成しました。 その操作を確認するには、このテスト環境でアプリケーションを実行します。 問題がなければ、期待どおりに動作しているそのこと、運用環境にアプリケーションが昇格されます。

> [!NOTE]
> ステージングされた発行を有効にするのには、web アプリである必要があります**標準モード**します。 Web アプリを標準モードに変更する場合は、追加料金が発生することに注意してください。 価格の詳細については、次を参照してください。 [App Service の価格](https://azure.microsoft.com/pricing/details/app-service/)します。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>タスク 1 – Azure App Service で Web アプリの作成

このタスクでの web アプリを作成します**Azure App Service**管理ポータルから。 構成することもが、 **SQL Database**アプリケーションのデータを保持し、ソース コントロールのローカル Git リポジトリを構成します。

1. 移動して、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられた Microsoft アカウントを使用してサインインします。

    ![Azure 管理ポータルにサインインするには](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Azure 管理ポータルにサインインするには*
2. クリックして**新規**ページの下部にあるコマンド バーにします。

    ![新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "新しい web アプリを作成します。")

    *新しい web アプリを作成します。*
3. クリックして**コンピューティング**、 **web サイト**し**カスタム作成**です。

    ![カスタム作成を使用して新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "カスタム作成を使用して新しい web アプリを作成します。")

    *カスタム作成を使用して新しい web アプリを作成します。*
4. **新規 web サイト - カスタム作成** ダイアログ ボックスで、使用可能な入力**URL** (例:*おたくクイズ*)、場所を選択、**リージョン**ドロップダウン リスト、および選択**新しい SQL データベースの作成**で、**データベース**ドロップダウン リスト。 最後に、選択、**ソース管理から発行** チェック ボックスをクリックします**次**します。

    ![新しい web アプリをカスタマイズします。](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *新しい web アプリをカスタマイズします。*
5. データベースの設定の次の情報を指定します。

   - **名前**テキスト ボックスに、データベース名を入力します (例: *geekquiz\_db*)
   - サーバーで**ドロップダウン**一覧で、**新しい SQL database server**します。 または、既存のサーバーを選択できます。
   - **データベース ユーザー名**と**データベース パスワード**ボックスに、SQL database のサーバー管理者のユーザー名とパスワードを入力します。 サーバーを選択する場合は、既に作成して、パスワードを求められます。

     ![データベースの設定を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *データベースの設定を指定します。*
6. **[次へ]** をクリックして、続行します。
7. 選択**ローカル Git リポジトリ**してをクリックしてソース管理の**次**。

    > [!NOTE]
    > 展開資格情報 (ユーザー名とパスワード) が求められるあります。

    ![Git リポジトリを作成します。](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Git リポジトリを作成します。*
8. 新しい web アプリが作成されるまで待機します。

    > [!NOTE]
    > 既定では、Azure には、ドメインに*azurewebsites.net*が、Azure 管理ポータルを使用してカスタム ドメインを設定することができます。 ただし、特定の Azure App Service のモードを使用している場合、のみカスタム ドメインを管理することができます。
    >
    > Azure App Service は、Free、Shared、Basic、Standard、および Premium エディションで使用できます。 Free および Shared のモードでは、すべての web アプリはマルチ テナント環境で実行され、CPU、メモリ、およびネットワークの使用量クォータがあります。 プランの無料のアプリの最大数が異なる場合があります。 標準モードでは、標準の Azure への対応する専用の仮想マシンで実行されるアプリはコンピューティング リソースを選択します。 Web アプリ モードの構成を見つけることができます、**スケール**web アプリのメニュー。
    >
    > ![Azure App Service モード](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service のモード")
    >
    > 使用する場合**Shared**または**標準**モードができます、アプリに移動して、web アプリ用のカスタム ドメインを管理する**構成**をクリックしてメニュー**ドメインの管理***ドメイン名*します。
    >
    > ![ドメインの管理](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "ドメインの管理")
    >
    > ![カスタム ドメインを管理](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "カスタム ドメインの管理")
9. Web アプリを作成した後は、下のリンクをクリックして、 **URL**新しい web アプリが実行されていることを確認する列。

    ![新しい web アプリを参照](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *新しい web アプリを参照*

    ![実行されている web アプリ](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *実行されている web アプリ*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>タスク 2 – 運用環境の SQL データベースを作成します。

このタスクでは、使用、 **Entity Framework Code First Migrations**対象とするデータベースを作成する、 **Azure SQL Database**前のタスクで作成したインスタンス。

1. 管理ポータルで、前のタスクで作成した web アプリに移動しに移動その**ダッシュ ボード**します。
2. **ダッシュ ボード**] ページで [**接続文字列を表示**下のリンク、**概要**セクション。

    ![接続文字列を表示](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "接続文字列を表示")

    *接続文字列の表示*
3. コピー、**接続文字列**値し、ダイアログ ボックスを閉じます。

    ![Azure 管理ポータルで接続文字列](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure 管理ポータルで接続文字列")

    *Azure 管理ポータルで接続文字列*
4. クリックして**SQL データベース**で、Azure SQL データベースの一覧を表示するには

    ![SQL Database メニューの ](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database のメニュー")

    *SQL Database のメニュー*
5. 前のタスクで作成したデータベースを検索し、[サーバー] をクリックします。

    ![SQL Database サーバー](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database サーバー")

    *SQL Database サーバー*
6. **クイック スタート**ページ サーバーのクリックして**構成**します。

    ![構成メニュー](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "構成メニュー")

    *メニューを構成します。*
7. **使用できる IP アドレス**セクションで、をクリックして**許可された IP アドレスを追加**ip アドレスを SQL Database サーバーへの接続を有効にするリンク。

    ![使用できる IP アドレス](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "使用できる IP アドレス")

    *許可された IP アドレス*
8. クリックして**保存**手順を実行するページの下部にあります。
9. Visual Studio に切り替えます。
10. **パッケージ マネージャー コンソール**、次のコマンドの置換を実行する *[YOUR CONNECTION STRING]* プレース ホルダーを Azure からコピーした接続文字列

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Windows Azure SQL データベースを対象とするデータベースの更新](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Windows Azure SQL データベースを対象とするデータベースの更新")

    *Azure SQL データベースを対象とするデータベースの更新*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>タスク 3 – Git を使用してステージング環境に展開するコンピューターおたくクイズ

このタスクでは、web アプリのステージングされた発行を有効にします。 次に、ギーク Quiz アプリケーションをローカル コンピューターから直接 web アプリのステージング環境に発行を Git を使用します。

1. ポータルに戻り、下で web アプリの名前をクリックして、**名前**管理ページを表示する列。

    ![Web アプリの管理ページを開く](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Web アプリの管理ページを開く*
2. 移動し、**スケール**ページ。 下、**全般**セクションで、**標準**構成をクリックします**保存**コマンド バーでします。

    > [!NOTE]
    > 現在の地域とサブスクリプションのすべての web アプリを実行する**標準**モードのままにしてください、**すべて選択**チェック ボックスをオン、**サイトの選択**構成。 それ以外の場合、オフ、**すべて選択**チェック ボックスをオンします。

    ![Web アプリを標準モードにアップグレードする](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "標準モードに web アプリをアップグレードします。")

    *Web アプリを標準モードにアップグレードします。*
3. クリックして**はい**変更を確認します。

    ![標準モードに変更を確認する](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web アプリのモードの変更を続行")

    *標準モードに変更を確認します。*
4. 移動して、**ダッシュ ボード**ページし、をクリックして**ステージングされた発行を有効にする**下、**概要**セクション。

    ![ステージングされた発行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "ステージングされた発行を有効にします。")

    *ステージングされた発行を有効にします。*
5. クリックして**はい**ステージングされた発行を有効にします。

    ![発行を段階的な確認](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "ステージングされた発行を有効にするには、[はい] をクリックして")

    *ステージングされた発行を確認します。*
6. Web アプリの一覧では、ステージング サイト スロットを表示する web アプリ名の左側に、マークを展開します。 続けて、web アプリの名前を持つ ***(ステージング)*** します。 管理ページに移動するステージング サイトをクリックします。

    ![ステージング web アプリに移動する](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "ステージング web アプリに移動します。")

    *ステージングのアプリに移動します。*
7. その彼の管理 ページの管理ページの他の web アプリのように注意してください。 移動し、**展開**ページとコピー、 **Git URL**値。 この演習の後半で使用します。

    ![Git URL 値をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Git URL 値をコピー*
8. 新しく開きます**Git Bash**コンソールし、次のコマンドを実行します。 更新プログラム、 *[YOUR APPLICATION PATH]* へのパスのプレース ホルダー、 **GeekQuiz**内にあるソリューション、 **Source\Ex1 DeployingWebSiteToStaging\Begin**のフォルダーこのラボでは。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git の初期化と最初のコミット](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git の初期化と最初のコミット*
9. Web アプリをリモートにプッシュするには、次のコマンドを実行**Git**リポジトリ。 管理ポータルから取得した URL では、プレース ホルダーを置き換えます。 デプロイ パスワードを求めるメッセージが表示されます。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure へのプッシュ](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Azure へのプッシュ*

    > [!NOTE]
    > FTP ホストや GIT リポジトリの web アプリにコンテンツを展開する場合を使用して認証する必要があります、**デプロイ資格情報**から web アプリの作成した**クイック スタート**または**ダッシュ ボード**管理ページ。 デプロイ資格情報がわからない場合、管理ポータルを使用して簡単にリセットできます。 Web アプリを開いて**ダッシュ ボード**ページ、をクリックし、**デプロイ資格情報をリセット**リンク。 新しいパスワードを入力し、クリックして**OK**します。 デプロイ資格情報は、サブスクリプションに関連付けられているすべての web apps の使用に対して有効です。
10. Web アプリは、Azure に正常にプッシュされたことを確認するために、管理ポータルに戻り をクリックして**Websites**します。
11. Web アプリを検索し、ステージング サイト スロットを表示するエントリを展開します。 をクリックして、**名前**管理ページに移動します。
12. クリックして**展開**を参照してください、**デプロイ履歴**します。 あることを確認、**アクティブなデプロイ**で、 *&quot;初期コミット&quot;* します。

    ![アクティブなデプロイ](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *アクティブなデプロイ*
13. 最後に、クリックして**参照**web アプリに移動して、コマンド バーにします。

    ![Web アプリを参照します。](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Web アプリを参照します。*
14. アプリケーションが正常にデプロイされている場合は、ギーク Quiz ログイン ページが表示されます。

    > [!NOTE]
    > デプロイされたアプリケーションのアドレスの URL に続けて、web アプリの名前が含まれています *-ステージング*します。

    ![ステージング環境で実行されているアプリケーション](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *ステージング環境で実行されているアプリケーション*
15. アプリケーションを探索する場合は、クリックして**登録**新しいユーザーを登録します。 ユーザー名、電子メール アドレスとパスワードを入力して、アカウントの詳細を完了します。 次に、アプリケーションは、クイズの最初の質問を示します。 想定どおりに動作を確認するいくつかの質問に回答します。

    ![アプリケーションをすぐに使用できます。](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *アプリケーションをすぐに使用できます。*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>タスク 4 – Web アプリを運用環境に昇格します。

Web アプリがステージング環境で正しく動作していることを確認できた運用環境に昇格させることする準備が整いました。 このタスクでは、運用サイト スロットとステージング サイト スロットをスワップします。

1. 管理ポータルに戻るし、ステージング サイト スロットを選択します。 クリックして**スワップ**コマンド バーにします。

    ![運用環境にスワップします。](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *運用環境にスワップします。*
2. クリックして**はい**スワップ操作を続行する確認のダイアログ ボックスでします。 Azure は、ステージング サイトのコンテンツを含む実稼働サイトのコンテンツをすぐにスワップされます。

    > [!NOTE]
    > (例: 接続文字列の上書き、ハンドラー マッピングなど) の production バージョンには、ステージングされたバージョンからのいくつかの設定が自動的にコピーされますが、(例: DNS エンドポイント、SSL のバインドなど)、その他の設定は変更されません。

    ![スワップ操作を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *スワップ操作を確認します。*
3. スワップが完了すると、運用スロットを選択し、をクリックして**参照**コマンド バーを実稼働サイトを開きます。 アドレス バーの URL に注意してください。

    > [!NOTE]
    > キャッシュをクリアするブラウザーを更新する必要があります。 Internet Explorer で、これを行うキーを押して**CTRL + R**します。

    ![運用環境で実行されている web アプリ](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. **GitBash**コンソールで、運用スロットを対象に、ローカルの Git リポジトリのリモート URL を更新します。 これを行うには、プレース ホルダーをデプロイ ユーザー名と、web アプリの名前に置き換えて、次のコマンドを実行します。

    > [!NOTE]
    > 次の演習では、ステージング ラボの簡潔さのためだけではなく、運用サイトへ変更をプッシュします。 実際のシナリオで、運用環境に昇格する前にステージング環境の変更を確認することをお勧めします。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>手順 3: 運用環境でデプロイのロールバックを実行します。

場所がないステージングや運用などの間でのホット スワップを実行するステージング スロットを使用する場合のシナリオがある**Free**または**Shared**モード。 そのようなシナリオでする必要がありますアプリケーションをテストするテスト環境で – ローカルまたはリモート サイトで – 運用環境にデプロイする前にします。 ただし、実稼働サイトに、テスト フェーズ中に検出されない問題が発生することが可能です。 この場合、可能な限り早く前とより安定したバージョンのアプリケーションに切り替える簡単にするためのメカニズムを理解しておくことができます。

**Azure App Service**、ソース管理からの継続的なデプロイは、考えられるこのおかげ、**再デプロイ**管理ポータルで使用できるアクション。 Azure では、リポジトリにプッシュされたコミットに関連付けられている展開を追跡し、いつでも、以前の展開のいずれかを使用してアプリケーションを再デプロイするためのオプションを提供します。

この演習では、コードに変更を実行して、**ギーク Quiz**意図的に挿入するアプリケーション、*バグ*します。 ときに、運用環境にアプリケーションを展開するし、以前の状態に戻るに再デプロイ機能がかかります。

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>タスク 1 – おたくクイズ アプリケーションの更新

このタスクでは、小型のコードのリファクタリングが、 **TriviaController**クラスの新しいメソッドに、データベースから選択したクイズ オプションを取得するロジックの一部を抽出します。

1. Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の演習からソリューション。
2. **ソリューション エクスプ ローラー**、オープン、 **TriviaController.cs**ファイル内で、**コント ローラー**フォルダー。
3. 検索、 **StoreAsync**メソッドと、コードは次の図で強調表示を選択します。

    ![コードを選択します。](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *コードを選択します。*
4. 選択したコードを右クリックし、展開、**リファクタリング**メニュー選択し、**メソッドの抽出しています.**.

    ![新しいメソッドとして、コードを抽出](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *メソッドの抽出を選択します。*
5. **メソッドの抽出**ダイアログ ボックスで、新しいメソッドの名前*MatchesOption*  をクリック**OK**します。

    ![メソッド名を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *抽出されたメソッドの名前を指定します。*
6. 選択したコードに抽出し、 **MatchesOption**メソッド。 結果として得られるコードは、次のスニペットに示します。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. キーを押して**CTRL + S**変更を保存します。

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>タスク 2 – おたくクイズのアプリケーションを再デプロイします。

運用環境への新しい配置をトリガーすると、リポジトリに、前のタスクで加えた変更をプッシュします。 次を使用して、問題のトラブルシューティングは、 **F12 開発ツール**、Internet Explorer によって提供され、Azure 管理ポータルから、以前のデプロイにロールバックを実行します。

1. 新しく開きます**Git Bash**更新されたアプリケーションを Azure App Service をデプロイするコンソール。
2. Azure に変更をプッシュするには、次のコマンドを実行します。 更新プログラム、 *[YOUR APPLICATION PATH]* へのパスのプレース ホルダー、 **GeekQuiz**ソリューション。 デプロイ パスワードを求めるメッセージが表示されます。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![リファクタリング後のコードを Azure にプッシュ](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *リファクタリング後のコードを Azure にプッシュ*
3. Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。 以前に作成した資格情報を使用してをログインします。
4. キーを押して**F12**開発ツールを起動するには、次のように選択します。、**ネットワーク** タブで  をクリックし、**再生**記録を開始するボタンをクリックします。

    ![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ネットワークの記録を開始")

    *ネットワークの記録を開始*
5. クイズの任意のオプションを選択します。 何も起こらないことが表示されます。
6. **F12**ウィンドウで、POST HTTP 要求に対応するエントリを示しています、HTTP **500**結果。

    ![HTTP 500 エラー](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 エラー*
7. 選択、**コンソール**タブ。エラーが原因の詳細を記録します。

    ![ログに記録されたエラー](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *ログに記録されたエラー*
8. エラーの詳細部分を見つけます。 明らかに、リファクタリング前の手順でコミットするコードによってこのエラーが発生します。

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`。
9. ブラウザーを閉じないでください。
10. 新しいブラウザー インスタンスでは、に移動、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられた Microsoft アカウントを使用してサインインします。
11. 選択**Websites**手順 2 で作成した web アプリをクリックします。
12. 移動し、**展開**ページ。 デプロイ履歴で、実行されるすべてのコミットが表示されていることに注意してください。

    ![既存の展開の一覧](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *既存の展開の一覧*
13. 以前のコミットを選択し、クリックして**再デプロイ**コマンド バーにします。

    ![以前のコミットを再デプロイします。](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *以前のコミットを再デプロイします。*
14. 確認するメッセージが表示されたら、クリックして**はい**します。

    ![再展開を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. デプロイが完了したらは、web アプリとキーを押して、ブラウザー インスタンスに切り替える**ctrl キーを押しながら f5 キーを押して**します。
16. 任意のオプションをクリックします。 フリップ アニメーション処理を実行し、結果 (*修正または正しくない*) が表示されます。
17. (省略可能)切り替えて、 **Git Bash**コンソールし、以前のコミットに戻すには、次のコマンドを実行します。

    > [!NOTE]
    > これらのコマンドは、Git リポジトリに正しくないコミットに加えられたすべての変更を元に戻すための新しいコミットを作成します。 Azure は、新しいコミットを使用して、アプリケーションとし、再デプロイします。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>手順 4: スケーリング、Azure Storage の使用

**Blob**は大量の非構造化テキストまたはバイナリ データなど、ビデオ、オーディオ、イメージを格納する最も簡単な方法です。 記憶域をアプリケーションの静的コンテンツを移動するは、画像またはドキュメントをブラウザーに直接配信することによって、アプリケーションをスケールするのに役立ちます。

この演習では、Blob コンテナーに、アプリケーションの静的コンテンツを移動します。 アプリケーションを追加するを構成し、 **ASP.NET URL の書き換え規則**で、 **Web.config**コンテンツを Blob コンテナーにリダイレクトします。

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>タスク 1 – Azure ストレージ アカウントの作成

このタスクでは、管理ポータルを使用して、新しいストレージ アカウントを作成する方法を学びます。

1. 移動し、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられた Microsoft アカウントを使用してサインインします。
2. 選択**新しい |Data Services |記憶域 |簡易作成**新しいストレージ アカウントの作成を開始します。 クリックし、アカウントの一意の名前を入力、**リージョン**一覧から。 クリックして**ストレージ アカウントの作成**を続行します。

    ![新しいストレージ アカウントを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "新しいストレージ アカウントの作成")

    *新しいストレージ アカウントの作成*
3. **ストレージ**セクションで、新しいストレージ アカウントの状態に変わるまで待機*オンライン*次の手順を続行するためです。

    ![作成したストレージ アカウント](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "ストレージ アカウントの作成")

    *ストレージ アカウントの作成*
4. ストレージ アカウントの名前をクリックし、**ダッシュ ボード**ページの上部にあるリンクです。 **ダッシュ ボード**ページによって、アカウントと、アプリケーション内で使用できるサービス エンドポイントの状態に関する情報を提供します。

    ![ストレージ アカウント ダッシュ ボードを表示する](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "ストレージ アカウント ダッシュ ボードを表示します。")

    *ストレージ アカウント ダッシュ ボードを表示します。*
5. をクリックして、**アクセス キーの管理**ナビゲーション バーのボタンをクリックします。

    ![アクセス キーの管理 ボタン](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "アクセス キーの管理 ボタン")

    *[アクセス キー] ボタンを管理します。*
6. **アクセス キーの管理**ダイアログ ボックスで、コピー、**ストレージ アカウント名**と**プライマリ アクセス キー**に次の手順で必要になります。 次に、ダイアログ ボックスを閉じます。

    ![アクセス キーの管理 ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "アクセス キーの管理 ダイアログ ボックス")

    *管理アクセス キー ダイアログ ボックス*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>タスク 2 – Azure Blob Storage への資産のアップロード

このタスクでは、ストレージ アカウントに接続する Visual Studio から、サーバー エクスプ ローラー ウィンドウを使用します。 Blob コンテナーを作成し、ギーク Quiz ロゴのファイルをコンテナーにアップロードされます。

1. Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の演習からソリューション。
2. メニュー バーから選択**ビュー**  をクリックし、**サーバー エクスプ ローラー**します。
3. **サーバー エクスプ ローラー**を右クリックし、 **Azure**ノード**を Azure に接続しています.**.サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。

    ![Windows Azure への接続します。](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Azure への接続します。*
4. 展開、 **Azure**ノードを右クリックし**ストレージ**選択と**外部ストレージのアタッチしています.**.
5. **新しいストレージ アカウントの追加** ダイアログ ボックスに、入力、**アカウント名**と**アカウント キー** 、前のタスクとクリックで取得した**OK**.

    ![新しいストレージ アカウント ダイアログ ボックスを追加します。](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *新しいストレージ アカウント ダイアログ ボックスを追加します。*
6. ストレージ アカウントは、下に表示する必要があります、**ストレージ**ノード。 ストレージ アカウントを展開し、右クリックして**Blob**選択**Blob コンテナーの作成.**.

    ![Blob コンテナーを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob コンテナーを作成します。")

    *Blob コンテナーを作成します。*
7. **Blob コンテナーの作成** ダイアログ ボックスで、blob コンテナーの名前を入力し、をクリックして**OK**します。

    ![Blob コンテナーの作成 ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob コンテナーの作成 ダイアログ ボックス")

    *Blob コンテナー ダイアログ ボックスを作成します。*
8. 新しい blob コンテナーに追加する必要があります、 **Blob**ノード。 コンテナーを公開して、コンテナーのアクセス許可を変更します。 これを行うを右クリックし、**イメージ**コンテナーと選択**プロパティ**します。

    ![コンテナーのプロパティをイメージ](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "イメージのコンテナーのプロパティ")

    *コンテナー イメージのプロパティ*
9. **プロパティ**ウィンドウで、設定、**パブリック読み取りアクセス**に**コンテナー**します。

    ![パブリック読み取りアクセスのプロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "パブリック読み取りアクセスのプロパティを変更します。")

    *パブリック読み取りアクセスのプロパティを変更します。*
10. パブリック アクセス プロパティを変更するには、をクリックする場合は、確認を求められたら**はい**します。

    ![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")

    *Microsoft Visual Studio 警告*
11. **サーバー エクスプ ローラー**でを右クリックし、**イメージ**blob コンテナーと選択**Blob コンテナーの表示**します。

    ![Blob コンテナーを表示](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob コンテナーの表示")

    *Blob コンテナーの表示*
12. イメージ コンテナーが新しいウィンドウで開く必要があり、エントリがないの凡例を表示する必要があります。 をクリックして、**アップロード**アイコン ファイルを blob コンテナーにアップロードします。

    ![エントリがないイメージ コンテナー](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "エントリがないイメージ コンテナー")

    *エントリがないイメージ コンテナー*
13. **Blob のアップロード** ダイアログ ボックスに移動、**資産**ラボのフォルダー。 選択、**ロゴ big.png**ファイルし、クリックして**オープン**。
14. ファイルがアップロードされるまで待ちます。 アップロードが完了したら、ファイルがイメージのコンテナーに表示されます。 ファイルのエントリを右クリックして**URL のコピー**します。

    ![Blob の URL をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "blob ファイルの URL をコピー")

    *Blob の URL をコピーします。*
15. Internet Explorer を開き、URL を貼り付けます。 次の図は、ブラウザーに表示する必要があります。

    ![Windows の Blob ストレージからイメージをロゴ big.png](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "ストレージからロゴ big.png イメージ")

    *Azure Blob Storage からロゴ big.png イメージ*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>タスク 3 – Azure Blob Storage から静的コンテンツを使用するソリューションを更新しています

このタスクでは、構成、 **GeekQuiz**ソリューション、イメージを使用する Azure Blob Storage にアップロード (web アプリにあるイメージ) ではなくで ASP.NET URL 書き換え規則を追加することで、 **web.config**ファイル。

1. Visual Studio で開く、 **Web.config**ファイル内で、 **GeekQuiz**プロジェクトし、検索、 **&lt;system.webServer&gt;** 要素。
2. URL 書き換え規則が、プレース ホルダーをストレージ アカウント名で更新を追加するには、次のコードを追加します。

    (コード スニペット - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL リライトは、受信 Web 要求をインターセプトし、別のリソースへの要求のリダイレクトのプロセスです。 URL 書き換えルールでは、書き換えエンジンを要求をリダイレクトする必要がある場合と、リダイレクト先にするように指示します。 書き換えルールは 2 つの文字列で構成されます: 要求された URL で検索するパターン (通常は、正規表現を使用して) 場合に使用すると、パターンを置換する文字列が見つかったとします。 詳細については、次を参照してください。 [ASP.NET における URL リライト](https://msdn.microsoft.com/library/ms972974.aspx)します。
3. キーを押して**CTRL + S**変更を保存します。
4. 新しく開きます**Git Bash**更新されたアプリケーションを Azure App Service をデプロイするコンソール。
5. Azure に変更をプッシュするには、次のコマンドを実行します。 更新プログラム、 *[YOUR APPLICATION PATH]* へのパスのプレース ホルダー、 **GeekQuiz**ソリューション。 デプロイ パスワードを求めるメッセージが表示されます。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Azure への更新プログラムの展開](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Azure への更新プログラムの展開*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>タスク 4 – 検証

このタスクでは、使用**Internet Explorer**を参照する、**ギーク Quiz**でホストされているイメージにアプリケーションと、URL の書き換え規則がのイメージが機能し、チェックをリダイレクト**Azure Blob記憶域**します。

1. Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。 以前に作成した資格情報を使用してをログインします。

    ![イメージとギーク Quiz web アプリを表示](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "イメージ ギーク Quiz web アプリを表示")

    *イメージとギーク Quiz web アプリを表示*
2. キーを押して**F12**開発ツールを起動するには、選択、**ネットワーク**タブし、記録を開始します。

    ![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ネットワークの記録を開始")

    *ネットワークの記録を開始*
3. キーを押して**ctrl キーを押しながら f5 キーを押して**web ページを更新します。
4. HTTP 要求が表示、ページの読み込みが完了すると、 **/img/logo-big.png** http URL **301**結果 (リダイレクト) と別の要求を`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL と HTTP **200**結果。

    ![URL リダイレクトの確認](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開発ツールでリダイレクトを示す")

    *URL リダイレクトの確認*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>手順 5: Web アプリの自動スケールの使用

> [!NOTE]
> Web ロードのサポートを必要があるために、この手順は省略可能で&amp;パフォーマンス テストで使用可能なだけ**Visual Studio 2013 Ultimate Edition**します。 Visual Studio 2013 の特定の機能の詳細については、のバージョンを比較[ここ](https://www.microsoft.com/visualstudio/eng/products/compare)します。


**Azure App Service Web Apps**実行されている web アプリの自動スケール機能を提供して**標準モード**します。 自動スケールは、Azure の負荷に応じて、web アプリのインスタンス数の自動スケールを使用できます。 自動スケールを有効にすると、Azure は、web アプリの CPU が 5 分ごとに 1 回チェックし、その時点で必要に応じてインスタンスが追加されます。 CPU 使用率が低い場合インスタンスが削除されます 2 時間おきに web アプリのパフォーマンスが低下しないことを確認します。

この演習では構成に必要な手順を参照してください、**自動スケール**の特徴として、**ギーク Quiz** web アプリ。 インスタンスのアップグレードをトリガーするアプリケーションで十分な CPU の負荷を生成する Visual Studio ロード テストを実行して、この機能を確認します。

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>タスク 1 –、CPU のメトリックに基づく自動スケールを構成します。

このタスクでは、手順 2 で作成した web アプリの自動スケール機能を有効にするのに、Azure 管理ポータルを使用します。

1. [Azure 管理ポータル](https://manage.windowsazure.com/)を選択します**Websites**手順 2 で作成した web アプリをクリックします。
2. 移動し、**スケール**ページ。 で、**容量**セクションで、 **CPU**の**メトリックによるスケール**構成します。

    > [!NOTE]
    > CPU によってスケーリング、Azure は、CPU 使用率が変更された場合に、アプリが使用するインスタンスの数を動的に調整されます。

    ![CPU によるスケールを選択する](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "自動スケーリングの CPU のメトリックを選択します。")

    *CPU によるスケールを選択します。*
3. 変更、**ターゲット CPU**構成**20**-**40** %。

    > [!NOTE]
    > この範囲は、web アプリの平均 CPU 使用率を表します。 Azure では、追加または、この範囲内で web アプリを保持するインスタンスを削除します。 拡張するために使用されるインスタンスの最小値と最大数がで指定された、**インスタンス数**構成します。 上またはその制限を超える azure になることはありません。
    >
    > 既定の**ターゲット CPU**値は、このラボの目的でのみ変更されます。 中程度の負荷をアプリケーションに配置すると、CPU の範囲を小さい値に構成すると、自動スケールをトリガーする機会は増加します。

    ![CPU の 20 ~ 40% がターゲットの変更](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "に 20 ~ 40% に CPU ターゲットの変更")

    *ターゲット CPU の 20 ~ 40% に変更します。*
4. クリックして**保存**変更を保存するコマンド バーにします。

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>タスク 2 – Visual Studio によるロード テスト

できた**自動スケール**された作成は、構成されている、 **Web パフォーマンスとロード テストのプロジェクト**で Visual Studio で web アプリに対する CPU 負荷を生成します。

1. 開いている**Visual Studio Ultimate 2013**選択**ファイル |新しい |プロジェクトの追加.** 新しいソリューションを開始します。

    ![新しいプロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "新しいプロジェクトを作成します。")

    *新しいプロジェクトを作成します。*
2. **新しいプロジェクト**ダイアログ ボックスで、 **Web パフォーマンスとロード テストのプロジェクト**下、 **(Visual C#) |テスト**タブ。確認します **.NET Framework 4.5**がプロジェクトに名前を選択すると、 *WebAndLoadTestProject*、選択、**場所** をクリック**OK**。

    ![新しい Web およびロード テスト プロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Web およびロード テスト プロジェクトを新規作成")

    *新しい Web およびロード テスト プロジェクトを作成します。*
3. **WebTest1.webtest**を右クリックし、 **WebTest1**ノードをクリックします**要求の追加**します。

    ![[Webtest1] への要求の追加](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "[webtest1] への要求の追加")

    *[Webtest1] への要求の追加*
4. **プロパティ**ウィンドウ、新しい要求のノードの更新、 **Url** web アプリの URL を指すプロパティ (例: *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Url プロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url プロパティを変更します。")

    *Url プロパティを変更します。*
5. **WebTest1.webtest**ウィンドウで、右クリック**WebTest1**  をクリック**ループの追加.**.

    ![[Webtest1] へのループの追加](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1 にループの追加")

    *[Webtest1] へのループの追加*
6. **条件付き規則の追加と項目をループ**ダイアログ ボックスで、 **For ループ**ルールし、次のプロパティを変更します。

   1. **終了値:** 1000
   2. **コンテキスト パラメーター名:** 反復子
   3. **増分の値:** 1

      ![For ループのルールを選択し、プロパティを更新](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For ループのルールを選択し、プロパティの更新")

      *For ループのルールを選択し、プロパティの更新*
7. で、**ループ内の項目**セクションで、ループの最初と最後の項目にする前に作成した要求を選択します。 **[OK]** をクリックして続行します。

    ![ループの最初と最後の項目を選択する](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "ループの最初と最後の項目を選択します。")

    *ループの最初と最後の項目を選択します。*
8. **ソリューション エクスプ ローラー**を右クリックし、 **WebAndLoadTestProject**プロジェクトで、展開、**追加**メニュー選択し、**ロード テストしています.**.

    ![WebAndLoadTestProject プロジェクトへのロード テストの追加](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject プロジェクトへのロード テストの追加")

    *WebAndLoadTestProject プロジェクトへのロード テストの追加*
9. **新しいロード テスト ウィザード**ダイアログ ボックスで、をクリックして**次**します。

    ![新しいロード テスト ウィザード](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新しいロード テスト ウィザード")

    *新しいロード テスト ウィザード*
10. **シナリオ** ページで、**待ち時間を使用しないでください** をクリック**次**します。

    ![待ち時間を使用しないように選択](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "待ち時間を使用しないように選択します。")

    *待ち時間を使用しないように選択します。*
11. **ロード パターン**ことを確認します ページで、**持続ロード**オプションを選択します。 変更、**ユーザー カウント**に設定**250**ユーザーとクリック **[次へ]** します。

    ![ユーザー カウントを 250 に変更する](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "を 250 ユーザー数を変更します。")

    *ユーザー カウントを 250 に変更します。*
12. **テスト ミックス モデル** ページで、**時系列順に基づいて** をクリック**次**します。

    ![テスト ミックス モデルを選択する](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "テスト ミックス モデルの選択")

    *テスト ミックス モデルの選択*
13. **テスト ミックス モデル**] ページで [**追加しています.** テストをミックスに追加します。

    ![テスト ミックスにテストを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "テスト ミックスにテストを追加します。")

    *テスト ミックスにテストを追加します。*
14. **テストの追加**ダイアログ ボックスをダブルクリック**WebTest1**にテストを追加する、**選択されたテスト**一覧。 **[OK]** をクリックして続行します。

    ![[Webtest1] テストを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "[webtest1] テストを追加します。")

    *[Webtest1] テストを追加します。*
15. 戻り、**テスト ミックス**] ページで [**次**します。

    ![テスト ミックスのページを完了](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "テスト ミックスのページの完了")

    *テスト ミックスのページの完了*
16. **ネットワーク ミックス**] ページで [**次**します。

    ![クリックして、[ネットワーク ミックス] ページで次に](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "ネットワーク ミックス ページで次をクリックすると")

    *ネットワーク ミックス ページで 次へ をクリックします。*
17. **ブラウザー ミックス**] ページで、[ **Internet Explorer 10.0**ブラウザーの種類をクリックします**次**します。

    ![ブラウザーの種類を選択する](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "ブラウザーの種類を選択します。")

    *ブラウザーの種類を選択します。*
18. **カウンター セット**] ページで [**次**します。

    ![カウンター セット ページで 次へ をクリックすると](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "カウンター セット ページでは、次をクリックすると")

    *カウンター セット ページで 次へ をクリックして*
19. **実行設定** ページで、設定、**ロード テストの継続時間**に**5 分** をクリック**完了**します。

    ![ロード テストの継続時間を 5 分に設定](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "ロード テストの継続時間を 5 分に設定します。")

    *ロード テストの継続時間を 5 分に設定します。*
20. **ソリューション エクスプ ローラー**、ダブルクリックして、 **Local.settings**テストの設定を表示するファイル。 既定では、Visual Studio は、テストを実行するのにローカル コンピューターを使用します。

    > [!NOTE]
    > 使用して、クラウドでロード テストを実行するテスト プロジェクトを構成する代わりに、 **Azure テスト計画**します。 Azure のテスト計画には、クラウド ベースのロード テスト サービスをより現実的な負荷をシミュレートする、CPU 容量、使用可能なメモリ、ネットワーク帯域幅などのローカル環境の制約を回避が提供します。 Azure のテスト計画を使用して、ロード テストを実行する方法の詳細については、次を参照してください。[ロード テスト シナリオ](/azure/devops/test/load-test/overview?view=vsts)します。

    ![テストの設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>タスク 3 – 自動スケール検証

前のタスクで作成したロード テストを実行するようになりましたし負荷の下で web アプリの動作を参照してください。

1. **ソリューション エクスプ ローラー**、ダブルクリックして**LoadTest1.loadtest**をロード テストを開きます。

    ![LoadTest1.loadtest を開く](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest を開く")

    *LoadTest1.loadtest を開く*
2. **LoadTest1.loadtest**ウィンドウで、ロード テストを実行するツールボックス内の最初のボタンをクリックします。

    ![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "ロード テストの実行")

    *ロード テストの実行*
3. ロード テストが完了するまで待機します。

    > [!NOTE]
    > ロード テストでは、web アプリを同時に要求を送信する複数のユーザーをシミュレートします。 テストが実行されている場合は、すべてのエラー、警告、またはロード テストの実行に関連するその他の情報を検出するために使用可能なカウンターを監視できます。

    ![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "ロード テストが完了するまで待機しています")

    *ロード テストの実行*
4. テストが完了すると、管理ポータルに戻るしに移動します、**スケール**web アプリのページ。 で、**容量**セクションに表示されます、グラフの新しいインスタンスが自動的にデプロイされたことです。

    ![新しいインスタンスを自動的にデプロイ](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *新しいインスタンスを自動的にデプロイ*

    > [!NOTE]
    > 変更、グラフに表示されるまで数分かかる場合があります (キーを押して**ctrl キーを押しながら f5 キーを押して**ページを更新するには、定期的に)。 すべての変更が表示されない場合、次の操作を行うことができます。
    >
    > - ロード テストの期間は長くなります (例: に**10 分**)
    > - 最大値と最小値を減らして、**ターゲット CPU**範囲は、web アプリの自動スケールの構成
    > - 使用してクラウドでロード テストの実行**Azure テスト計画**します。 詳細については[ここ](/azure/devops/test/load-test/index?view=vsts)

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを設定して、Azure で web アプリを運用環境にアプリケーションを配置する方法について説明しました。 使用してデータベースの更新を検出して開始した**Entity Framework Code First Migrations**、しを使用して、サイトの新しいバージョンを展開することで続き**Git**へのロールバックを実行して、サイトの最新の安定バージョン。 さらに、記憶域を使用して静的コンテンツを Blob コンテナーに移動するアプリをスケールする方法を学習しました。
