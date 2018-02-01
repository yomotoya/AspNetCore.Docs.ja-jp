---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "ハンズオン ラボ: Azure の web サイトの保守が容易な: 変更とスケールを管理する |Microsoft ドキュメント"
author: rick-anderson
description: "このラボでは、Microsoft Azure 使用して簡単方法をビルドし、web サイトを実稼働環境にデプロイについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 4bce02b2c592ff04e0dbce78d18004c69268e4fd
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>ハンズオン ラボ: Azure の web サイトの保守が容易な: 変更とスケールを管理します。
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> Microsoft Azure 簡単をビルドし、web サイトを実稼働環境にデプロイできます。 アプリケーションが完了していません、だけを開始します。 変化する要件、データベースの更新、スケール、および詳細を処理する必要があります。 さいわい、Azure App Service には、サイトの円滑な動作を確保するための機能の多くでは、カバーします。
> 
> Azure では、セキュリティで保護された柔軟な開発、展開、およびスケーリングの任意のサイズの web アプリケーションのオプションを提供します。 インフラストラクチャを管理する煩わしさのないアプリケーションの展開を作成し、既存のツールを活用します。
> 
> 実稼働 web アプリケーションをプロビジョニング自分で分単位で、使い慣れた開発ツールを使用して作成されたコンテンツを簡単に展開しています。 ソース管理のサポートから直接既存のサイトを展開する**Git**、 **GitHub**、 **Bitbucket**、 **TFS**、およびでも**DropBox**です。 好きな IDE から直接、またはを使用してスクリプトから展開**PowerShell** windows または**CLI**すべての OS で実行されるツールです。 展開した後は、サイトを常に最新の状態の継続的な展開のサポートのままにします。
> 
> Azure では、拡張性、持続性のあるクラウドの記憶域、バックアップ、および復旧のソリューション、データを小規模または大規模なです。 実稼働環境、テーブル、Blob、SQL データベースなどの記憶域サービスのアプリケーションを展開するときに、クラウドでアプリケーションを拡張できます。
> 
> SQL データベースを使用するには、新しいバージョンのアプリケーションを展開するときに生産性の高いデータベースを最新に保つ必要があります。 感謝**Entity Framework Code First Migrations**、分単位で、環境を更新する、開発と、データ モデルの配置が簡素化されています。 このハンズオン ラボでは、Microsoft Azure で実稼働環境に web アプリを配置するときに発生する可能性がありますのさまざまなトピックを示します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。
> 
> このトピックの詳しい内容について詳細を参照してください、 [Azure 電子書籍の実際のクラウド アプリのビルド](building-real-world-cloud-apps-with-windows-azure/introduction.md)です。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- 既存のモデルとエンティティ フレームワークの移行を有効にします。
- オブジェクト モデルとエンティティ フレームワークの移行を適宜使用してデータベースを更新します。
- Git を使用して Azure App Service に展開します。
- Azure 管理ポータルを使用して以前の展開にロールバック
- Azure ストレージを使用して web アプリをスケールするには
- Azure 管理ポータルを使用して、web アプリの自動スケーリングを構成します。
- 作成し、Visual Studio でのロード テストのプロジェクトを構成します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上
- [Azure SDK for .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT バージョン管理システム](http://git-scm.com/download)
- Microsoft Azure サブスクリプション 

    - サインアップ、[無料試用版](http://aka.ms/watk-freetrial)
    - 場合は、Visual Studio Professional、Test Professional、Premium または Ultimate の MSDN または MSDN のプラットフォームのサブスクライバーと、アクティブ化、 [MSDN 特典付き](http://aka.ms/watk-msdn)の開発と Azure でテストを開始するようになりました
    - [BizSpark](http://aka.ms/watk-bizspark)メンバーは、Azure を自動的に受信を通じて、Visual Studio Ultimate with MSDN サブスクリプション特典
    - メンバー、 [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials プログラム毎月 Azure クレジットが無料の受信

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。

1. 開いている Windows エクスプ ローラーおよびテスト環境の参照**ソース**フォルダーです。
2. 右クリックして**Setup.cmd**選択と**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。
3. ユーザー アカウント制御 ダイアログが表示されている場合は、アクションを続行を確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットを使用します。

ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> 各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。 実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [Entity Framework Migrations を使用してください。](#Exercise1)
2. [ステージングに Web アプリを展開します。](#Exercise2)
3. [実稼働環境で展開のロールバックを実行します。](#Exercise3)
4. [Azure ストレージを使用して scaling](#Exercise4)
5. [Web アプリの自動スケールを使用して](#Exercise5)(Visual Studio 2013 Ultimate edition の省略可能)

この演習を完了する時間を推定: **75 分**

> [!NOTE]
> Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。 このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。 開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>手順 1: Entity Framework の移行を使用します。

アプリケーションを開発している際に、データ モデルが時間の経過と共に変更します。 これらの変更に影響する可能性、データベース内の既存のモデル (作成する場合は、新しいバージョン) と、データベースのエラーを防ぐために最新の状態を保持することが重要です。

モデル内のこれらの変更の追跡を簡略化する**Entity Framework Code First Migrations**自動的にデータベース スキーマと、モデルを比較する変更を検出し、データベースを更新する特定のコードが生成されます新規に作成する*バージョン*データベースのです。

この演習は、有効にする方法を示します**移行**アプリケーションと簡単に検出しする方法、データベースを更新する変更を生成します。

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>作業 1: 移行を有効にします。

このタスクで有効にする手順を参照してください**Entity Framework Code First Migrations**を**マニア Quiz**データベース、モデルを変更してにそれらの変更を反映する方法を理解すること、データベースです。

1. Visual Studio を開き、開き、 **GeekQuiz.sln**からソリューション ファイル**Source\Ex1 UsingEntityFrameworkMigrations\Begin**です。
2. ダウンロードしてインストールするためにソリューションをビルド、 **NuGet**の依存関係をパッケージ化します。 これを行うには、ソリューションを右クリックし、をクリックして**ソリューションのビルド**またはキーを押して**Ctrl + Shift + B**です。
3. **ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**、クリックして**パッケージ マネージャー コンソール**です。
4. **Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。 既存のモデルに基づいて最初の移行が作成されます。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![移行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "移行を有効にします。")

    *移行を有効にします。*

    > [!NOTE]
    > このコマンドを追加、**移行**マニア クイズを含むプロジェクト ファイルをフォルダーと呼ばれる**される Configuration.cs**です。 **構成**クラスでは、コンテキストの移行の動作を構成することができます。
5. 移行が有効になっているで更新する必要があります、**構成**初期データと共にデータベースを設定するクラスを**マニア Quiz**が必要です。 **移行**、置換、**される Configuration.cs**で 1 つ持つファイルが格納されて、 **Source\Assets**このラボのフォルダーです。

    > [!NOTE]
    > **移行**が呼び出す、**シード**メソッドすべてのデータベース更新プログラムをデータベース内のレコードが重複していないことを確認する必要があります。 **AddOrUpdate**方法は、データの重複を防ぐために役立ちます。
6. 追加する最初の移行、次のコマンドを入力し、キーを押します**Enter**です。

    > [!NOTE]
    > という名前のデータベースがないことを確認してください&quot;GeekQuizProd&quot; LocalDB インスタンスにします。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![ベース スキーマの移行を追加する](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "追加ベース スキーマの移行")

    *ベース スキーマの移行を追加します。*

    > [!NOTE]
    > **追加の移行**前回の移行が作成された後に、モデルに行われた変更に基づき、次の移行をスキャフォールディングできます。 この場合、プロジェクトの最初の移行が追加で定義されているすべてのテーブルを作成するスクリプト、 **TriviaContext**クラスです。
7. 次のコマンドを実行して、データベースを更新する移行を実行します。 このコマンドを指定します、 **Verbose**先データベースに適用されている SQL ステートメントを表示するフラグ。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![最初にデータベースの作成](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "最初にデータベースの作成")

    *初期データベースを作成します。*

    > [!NOTE]
    > **データベースを更新**保留中の移行をデータベースに適用されます。 ここでは、これは、web.config ファイルで定義されている接続文字列を使用してデータベースを作成します。
8. 移動して**ビュー**メニューと開いている**SQL Server オブジェクト エクスプ ローラー**です。

    ![SQL Server オブジェクト エクスプ ローラーで開く](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server オブジェクト エクスプ ローラーで開く")

    *SQL Server オブジェクト エクスプ ローラーで開く*
9. **SQL Server オブジェクト エクスプ ローラー**  ウィンドウを右クリックして、LocalDB インスタンスに接続、 **SQL Server**ノードを選択して**SQL サーバーを追加しています.**オプション。

    ![SQL Server インスタンスを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "SQL Server インスタンスを追加します。")

    *SQL Server オブジェクト エクスプ ローラーに SQL Server インスタンスを追加します。*
10. 設定、**サーバー名**に*(localdb) \v11.0*のままにして**Windows 認証**認証モードとして。 をクリックして**接続**を続行します。

    ![LocalDB への接続](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "LocalDB への接続")

    *LocalDB への接続*
11. 開く、 **GeekQuizProd**データベースを展開し、**テーブル**ノード。 わかります、 **Update-database**コマンドで定義されているすべてのテーブルを生成する、 **TriviaContext**クラスです。 検索、 **dbo します。TriviaQuestions**テーブルし、列のノードを開きます。 次のタスクでは、このテーブルに新しい列を追加し、データベースを使用して、更新**移行**です。

    ![列の質問にトリビア](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "トリビア質問列")

    *トリビア質問列*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>タスク 2: 移行を使用してデータベース スキーマを更新

このタスクでは、使用**Entity Framework Code First Migrations**モデルの変更を検出し、データベースを更新するために必要なコードを生成します。 更新が表示される、 **TriviaQuestions**エンティティを新しいプロパティを追加します。 テーブルに新しい列を含める新しい移行を作成するコマンドを実行します。

1. **ソリューション エクスプ ローラー**をダブルクリックして、 **TriviaQuestion.cs**ファイル内にある、**モデル**フォルダーです。
2. という名前の新しいプロパティを追加**ヒント**の次のコード スニペットに示すようにします。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. **Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。 新しい移行は、モデルの変更を反映して作成されます。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")

    *Add-Migration*

    > [!NOTE]
    > 移行ファイルが 2 つの方法で構成される**を**と**ダウン**です。
    > 
    > - **を**メソッドは、どのような変更をデータベースに適用する、アプリケーションのニーズの現在のバージョンを指定する使用されます。
    > - **ダウン**に追加した変更を取り消すために使用、**を**メソッドです。
    > 
    > 更新すると、データベースの移行、データベース、すべての移行、タイムスタンプ順、および前回の更新以降使用されていないもののみで実行されます (、 \_MigrationHistory テーブルは、移行の種類が適用されているを追跡し続けます)。 **を**すべての移行のメソッドが呼び出され、データベースへ指定した変更を加えます。 前の移行に戻る場合、**ダウン**逆の順序の変更を再実行するメソッドが呼び出されます。
4. **Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. 出力、 **Update-database**生成されたコマンド、 **Alter Table**に新しい列を追加する SQL ステートメント、 **TriviaQuestions**表に、次の図に示すようにします。

    ![生成される SQL ステートメントの列を追加](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "生成される SQL ステートメントの列を追加")

    *生成される SQL ステートメントの列を追加します。*
6. **SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブルが表示されことを確認、新しい**ヒント**列が表示されます。

    ![新しい [ヒント] 列を示す](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "新しいヒント列を表示")

    *新しい [ヒント] 列の表示*
7. 戻り、 **TriviaQuestion.cs**エディターを追加、 **StringLength**制約を*ヒント*プロパティ、次のコード スニペットで示すようにします。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. **Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. **Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. 出力、 **Update-database**生成されたコマンド、 **Alter Table**を更新する SQL ステートメント、*ヒント*の列の型、 **TriviaQuestions**表に、次の図に示すようにします。

    ![生成される SQL ステートメントの列を alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL ステートメントの生成")

    *生成された列の SQL ステートメントを変更します。*
11. **SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブルが表示されことを確認、**ヒント**列の型が**nvarchar(150)**です。

    ![新しい制約を示す](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "新しい制約を示す")

    *新しい制約を示す*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>手順 2: ステージングに Web アプリを展開します。

**Web アプリを Azure App service の**ステージングされた発行を実行することができます。 ステージングされた発行では、既定の運用サイトごとのステージング サイト スロットを作成し、ダウンタイムなしでこれらのスロットをスワップすることができます。 これは一般に解放する前に変更を検証、増分を統合するサイトのコンテンツ変更が期待どおりに動作していない場合をロールバック本当に役立ちます。

この演習では展開、**マニア Quiz** Git ソース管理を使用して、web アプリのステージング環境へのアプリケーションです。 これを行うが web アプリを作成し、管理ポータルで必要なコンポーネントのプロビジョニング、構成、 **Git**リポジトリとプッシュ アプリケーション ソース コードをローカル コンピューターから、ステージング スロットにします。 使用して実稼働データベースを更新することも、 **Code First Migrations**前述の手順で作成します。 その操作を確認するには、このテスト環境でアプリケーションを実行します。 入力が完了したらことが期待どおりに動作して、実稼働環境にアプリケーションが昇格されます。

> [!NOTE]
> ステージングされた発行を有効にするには、web アプリにあります**標準モード**です。 Web アプリを標準モードに変更する場合に追加料金が発生することに注意してください。 料金の詳細については、次を参照してください。 [App Service の料金](https://azure.microsoft.com/pricing/details/app-service/)です。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>作業 1: Azure App Service で Web アプリの作成

このタスクでは、web アプリを作成します**Azure App Service**管理ポータルからです。 また、構成、 **SQL データベース**アプリケーション データを保持して、ローカル Git リポジトリをソース管理を構成します。

1. 移動して、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。

    ![Azure 管理ポータルにサインインするには](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Azure 管理ポータルにサインインするには*
2. をクリックして**新規**ページの下部にあるコマンド バーにします。

    ![新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "新しい web アプリを作成します。")

    *新しい web アプリを作成します。*
3. をクリックして**コンピューティング**、 **web サイト**し**カスタム作成**です。

    ![カスタム作成を使用して新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "カスタム作成を使用して新しい web アプリを作成します。")

    *カスタム作成を使用して新しい web アプリを作成します。*
4. **新しい web サイト - カスタム作成** ダイアログ ボックスで、使用可能な指定**URL** (例:*マニア クイズ*)、場所を選択、**地域**ドロップダウン リスト、および選択**新しい SQL データベースの作成**で、**データベース**ドロップダウン リスト。 最後に、選択、**ソース管理から発行** チェック ボックスをクリック**次**です。

    ![新しい web アプリのカスタマイズ](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *新しい web アプリのカスタマイズ*
5. データベースの設定の次の情報を指定します。

    - **名前**テキスト ボックスに、データベースの名前を入力 (例: *geekquiz\_db*)
    - サーバーで**ドロップダウン**一覧で、**新しい SQL データベース サーバー**です。 または、既存のサーバーを選択することができます。
    - **データベース ユーザー名**と**データベース パスワード**ボックスに、SQL データベース サーバーの管理者のユーザー名とパスワードを入力します。 サーバーを選択する場合は、既に作成して、パスワードを求められます。

    ![データベースの設定を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    *データベースの設定を指定します。*
6. **[次へ]** をクリックして、続行します。
7. 選択**ローカル Git リポジトリ**してをクリックして、ソース コントロールの**次**です。

    > [!NOTE]
    > デプロイ資格情報 (ユーザー名とパスワード) の要求があります。

    ![Git レポジトリの作成](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Git レポジトリの作成*
8. 新しい web アプリが作成されるまで待機します。

    > [!NOTE]
    > 既定では、Azure 提供のドメイン*azurewebsites.net* Azure 管理ポータルを使用してカスタム ドメインを設定することも付与します。 ただし、特定の Azure App Service モードを使用している場合、のみカスタム ドメインを管理することができます。
    > 
    > Azure App Service は、空き、Shared、Basic、Standard、および Premium エディションで使用できます。 無料モードおよび共有モードでは、すべての web アプリは、マルチ テナント環境で実行し、CPU、メモリ、およびネットワークの使用量クォータがあります。 無料のアプリの最大数は、プランで異なる場合があります。 標準モードでは、標準の Azure に対応する専用の仮想マシンで実行するアプリのコンピューティング リソースを選択します。 Web アプリ モードの構成を見つけることができます、**スケール**web アプリのメニュー。
    > 
    > ![Azure App Service モード](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service のモード")
    > 
    > 使用している場合**Shared**または**標準**モードでは、ことができます、アプリに移動して、web アプリのカスタム ドメインを管理する**構成**をクリックしてメニュー**ドメインの管理***ドメイン名*です。
    > 
    > ![ドメインを管理する](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "ドメインを管理します。")
    > 
    > ![カスタム ドメインを管理する](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "カスタム ドメインを管理します。")
9. Web アプリを作成した後は、下のリンクをクリックして、 **URL**新しい web アプリが実行されていることを確認する列。

    ![新しい web アプリへの参照](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *新しい web アプリへの参照*

    ![web アプリが実行されています。](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *web アプリが実行されています。*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>タスク 2: 実稼働 SQL データベースの作成

このタスクでは使用して、 **Entity Framework Code First Migrations**を対象とするデータベースを作成する、 **Azure SQL Database**前のタスクで作成したインスタンス。

1. 管理ポータルで、前のタスクで作成した web アプリに移動しに移動その**ダッシュ ボード**です。
2. **ダッシュ ボード**] ページで [**接続文字列を表示**下にあるリンク、**クイック グランス**セクションです。

    ![接続文字列を表示](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "接続文字列を表示")

    *接続文字列を表示します。*
3. コピー、**接続文字列**値し、ダイアログ ボックスを閉じます。

    ![Azure 管理ポータルでの接続文字列](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure 管理ポータルでの接続文字列")

    *Azure 管理ポータルでの接続文字列*
4. をクリックして**SQL データベース**azure SQL データベースの一覧を表示するには

    ![SQL データベース] メニューの [](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL データベース メニュー")

    *SQL データベース メニュー*
5. 前のタスクで作成したデータベースを見つけて、[サーバー] をクリックします。

    ![SQL データベース サーバー](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL データベース サーバー")

    *SQL データベース サーバー*
6. **クイック スタート**ページ上をクリックして、サーバーの**構成**です。

    ![構成 メニュー](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "構成 メニュー")

    *メニューを構成します。*
7. **許可されている IP アドレス**セクションで、をクリックして**許可される IP アドレスを追加**リンクされた SQL データベース サーバーに接続する、IP を有効にします。

    ![使用できる IP アドレス](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "許可されている IP アドレス")

    *許可される IP アドレス*
8. をクリックして**保存**手順を実行するページの下部にあります。
9. Visual Studio に切り替えます。
10. **Package Manager Console**、次のコマンドの置換を実行*[、接続文字列]*プレース ホルダーを Azure からコピーした接続文字列

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Windows Azure SQL データベースを対象とするデータベースの更新](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Windows Azure SQL データベースを対象とするデータベースの更新")

    *Azure SQL データベースを対象とするデータベースの更新*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>タスク 3: Git を使用してステージング展開マニア クイズ

このタスクでは、web アプリのステージングされた発行を有効にします。 次に、web アプリのステージング環境に、ローカル コンピューターから直接マニア Quiz アプリケーションを発行するのに Git を使用します。

1. ポータルに戻るしの下で web アプリの名前をクリックして、**名前**管理ページを表示する列。

    ![Web アプリの管理ページを開く](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Web アプリの管理ページを開く*
2. 移動し、**スケール**ページ。 下にある、**全般**セクションで、**標準**の構成と をクリック**保存**コマンド バーでします。

    > [!NOTE]
    > 現在の地域とサブスクリプションのすべての web アプリを実行する**標準**モードのままにして、**すべて選択** チェック ボックスをオンに、**サイトの選択**構成します。 それ以外の場合、オフ、**すべて選択**チェック ボックスをオンします。

    ![Web アプリを標準モードにアップグレードする](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "web アプリを標準モードにアップグレードします。")

    *Web アプリを標準モードにアップグレードします。*
3. をクリックして**はい**変更を確認します。

    ![標準モードに変更を確認する](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web アプリのモードの変更を続行")

    *標準モードに変更を確認します。*
4. 移動して、**ダッシュ ボード**ページし、をクリックして**ステージングされた発行を有効にする**下にある、**クイック グランス**セクションです。

    ![ステージングされた発行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "ステージングされた発行を有効にします。")

    *ステージングされた発行を有効にします。*
5. をクリックして**はい**ステージングされた発行を有効にします。

    ![ステージングされた発行を確認する](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "ステージングされた発行を有効にするには、[はい] をクリックすると")

    *ステージングされた発行を確認します。*
6. Web アプリの一覧で、ステージング サイト スロットを表示する web アプリ名の左側に、マークを展開します。 続けて、web アプリの名前を持つ***(ステージング)***です。 管理ページに移動するステージング サイトをクリックします。

    ![ステージングの web アプリに移動する](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "ステージングの web アプリに移動します。")

    *ステージングのアプリに移動します。*
7. 彼管理 ページの他の web アプリの管理 ページのように注意してください。 移動し、**展開**ページのコピーと、 **Git URL**値。 この演習で後で使用されます。

    ![Git の URL の値をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Git の URL の値をコピー*
8. 新しく開きます**Git Bash**コンソールし、次のコマンドを実行します。 更新プログラム、 *[、アプリケーション パス]*へのパスのプレース ホルダー、 **GeekQuiz**ソリューションにある、 **Source\Ex1 DeployingWebSiteToStaging\Begin**のフォルダーこの演習です。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git の初期化と最初のコミット](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git の初期化と最初のコミット*
9. Web アプリをリモートにプッシュする次のコマンド実行**Git**リポジトリです。 プレース ホルダーを管理ポータルから取得した URL に置き換えます。 展開パスワードを求められます。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure へのプッシュ](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Azure へのプッシュ*

    > [!NOTE]
    > コンテンツを FTP ホストや GIT リポジトリ web アプリを展開するときに使用して認証する必要があります、**デプロイ資格情報**から web アプリの作成した**クイック スタート**または**ダッシュ ボード**管理ページ。 デプロイ資格情報がわからない場合、管理ポータルを使用して簡単にリセットできます。 Web アプリを開き、**ダッシュ ボード**ページ、をクリックし、**デプロイ資格情報をリセット**リンクします。 新しいパスワードを指定して、をクリックして**OK**です。 デプロイ資格情報は、お客様のサブスクリプションに関連付けられているすべての web アプリを使用して使用に対して有効です。
10. Azure に web アプリを正常にプッシュすることを確認するために、管理ポータルに戻り、をクリックして**Websites**です。
11. Web アプリを見つけて、ステージング サイト スロットを表示するエントリを展開します。 クリックしてその**名前**管理ページに移動します。
12. をクリックして**展開**を表示する、**デプロイ履歴**です。 あることを確認してください、**アクティブなデプロイ**で、 *&quot;初期コミット&quot;*です。

    ![アクティブな展開](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *アクティブな展開*
13. 最後に、をクリックして**参照**web アプリに移動して、コマンド バーにします。

    ![Web アプリを参照します。](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Web アプリを参照します。*
14. アプリケーションが正常に展開されている場合は、マニア Quiz ログイン ページが表示されます。

    > [!NOTE]
    > 展開済みアプリケーションのアドレスの URL には、続けて、web アプリの名前が含まれています。 *-ステージング*です。

    ![ステージング環境で実行されるアプリケーション](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *ステージング環境で実行されるアプリケーション*
15. アプリケーションを調査する場合は、クリックして**登録**新しいユーザーを登録します。 ユーザー名、電子メール アドレスとパスワードを入力して、アカウントの詳細を完了します。 次に、アプリケーションは、クイズの最初の質問を示します。 期待どおりに機能を確認するいくつかの質問に回答します。

    ![使用できるようにアプリケーション](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *使用できるようにアプリケーション*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>タスク 4-Web アプリを実稼働に昇格します。

これで、ステージング環境で web アプリが正しく動作していることを確認している実稼働環境に昇格する準備ができます。 このタスクでは、運用サイト スロットとステージング サイト スロットをスワップします。

1. 管理ポータルに戻り、ステージング サイト スロットを選択します。 をクリックして**スワップ**コマンド バーでします。

    ![実稼働環境にスワップします。](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *実稼働環境にスワップします。*
2. をクリックして**はい**確認ダイアログ ボックスでのスワップ操作を続行します。 Azure は、ステージング サイトのコンテンツを持つ運用サイトのコンテンツをすぐに交換されます。

    > [!NOTE]
    > (例: 接続文字列の上書き、ハンドラー マッピングなど) の実稼働バージョンには、ステージングされたバージョンからのいくつかの設定が自動的にコピーされますが、(例: DNS エンドポイント、SSL のバインドなど)、その他の設定は変更されません。

    ![スワップ操作を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *スワップ操作を確認します。*
3. スワップの完了後、運用スロットを選択し、クリックして**参照**コマンド バーを開くには、実稼働サイトでします。 アドレス バーの URL に注意してください。

    > [!NOTE]
    > キャッシュをクリアするブラウザーを更新する必要があります。 Internet Explorer で、これを行うキーを押して**CTRL + R**です。

    ![実稼働環境で実行されている web アプリ](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. **GitBash**コンソールで、運用スロットを対象とするローカルの Git リポジトリ用のリモート URL を更新します。 これを行うには、展開ユーザー名と web アプリの名前のプレース ホルダーに置き換えて、次のコマンドを実行します。

    > [!NOTE]
    > 次の演習では、ステージング、ラボのわかりやすくするためだけではなく運用サイトに変更をプッシュします。 実際のシナリオで、実稼働環境に昇格する前にステージング環境の変更を確認することをお勧めします。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>手順 3: 実稼働環境で展開のロールバックを実行します。

場所がないステージングと運用などの間でホット スワップを実行するステージング スロットで作業している場合のシナリオがある**空き**または**Shared**モード。 そのようなシナリオではアプリケーションをテストするテスト環境で – ローカルまたはリモート サイトに – 実稼働環境に展開する前にします。 ただし、可能であればテスト フェーズ中に検出されない問題が実稼働サイトで発生する可能性があります。 この場合、可能な限り早く前およびより安定したバージョンのアプリケーションに切り替える簡単にするためのメカニズムを理解しておくことができます。

**Azure App Service**、ソース管理からの継続的なデプロイが可能なこのおかげ、**再展開**管理ポータルで使用できるアクション。 Azure では、リポジトリにプッシュされたコミットに関連付けられている展開を追跡し、いつでも、以前の展開のいずれかを使用してアプリケーションを再展開するためのオプションを提供します。

この演習では、コードに変更を実行します、**マニア Quiz**意図的に挿入するアプリケーション、*バグ*です。 実稼働環境にアプリケーションが、エラーを展開して、し、前の状態に戻るに再展開機能を利用するためです。

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>作業 1: マニア クイズ アプリケーションの更新

このタスクで小規模のコードをリファクタリングが、 **TriviaController**データベースから新しいメソッドに、選択したクイズ オプションを取得するロジックの一部を抽出するクラス。

1. Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の手順からソリューションです。
2. **ソリューション エクスプ ローラー**を開き、 **TriviaController.cs**内のファイル、**コント ローラー**フォルダーです。
3. 検索、 **StoreAsync**メソッドと、コードが次の図で強調表示を選択します。

    ![コードを選択します。](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *コードを選択します。*
4. 選択したコードを右クリックし、展開、**リファクター**メニュー**メソッドの抽出しています.**.

    ![新しいメソッドとして、コードを抽出します。](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Extract メソッドを選択します。*
5. **メソッドの抽出** ダイアログ ボックスに、名前は新しいメソッド*MatchesOption*  をクリック**OK**です。

    ![メソッド名を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *抽出されたメソッドの名前を指定します。*
6. 選択したコードに抽出し、 **MatchesOption**メソッドです。 結果のコードは、次のスニペットに表示されます。

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. キーを押して**ctrl キーを押しながら S**変更を保存します。

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>タスク 2-マニア クイズ アプリケーションを再配置

運用環境に新しい展開をトリガーする、リポジトリに前のタスクで加えた変更をプッシュするようになりました。 次を使用して、問題のトラブルシューティングは、 **F12 開発ツール**Internet Explorer によって提供され、Azure 管理ポータルから、以前のデプロイをロールバックを実行します。

1. 新しく開きます**Git Bash** Azure App Service に更新されたアプリケーションを配置するコンソール。
2. 変更を Azure にプッシュするには、次のコマンドを実行します。 更新プログラム、 *[、アプリケーション パス]*へのパスのプレース ホルダー、 **GeekQuiz**ソリューションです。 展開パスワードを求められます。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![リファクタリングされたコードを Azure にプッシュします。](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *リファクタリングされたコードを Azure にプッシュします。*
3. Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。 以前に作成した資格情報を使用してをログインします。
4. キーを押して**F12**開発ツールを起動するには、次のように選択します。、**ネットワーク** タブで  をクリックし、**再生**記録を開始するボタンをクリックします。

    ![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ネットワークの記録を開始")

    *ネットワークの記録を開始*
5. クイズの任意のオプションを選択します。 何も起こらないことが表示されます。
6. **F12**ウィンドウで、POST HTTP 要求に対応するエントリを示しています、HTTP **500**結果。

    ![HTTP 500 エラー](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 エラー*
7. 選択、**コンソール**タブです。エラーが原因の詳細を記録します。

    ![ログに記録されたエラー](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *ログに記録されたエラー*
8. エラーの詳細部分を探します。 明確に、このエラーはリファクタリング前の手順でコミットするコードによって発生します。

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`。
9. ブラウザーを閉じないでください。
10. 新しいブラウザー インスタンスでは、に移動、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。
11. 選択**web サイト**手順 2 で作成した web アプリケーション をクリックします。
12. 移動し、**展開**ページ。 実行されるすべてのコミットが、デプロイ履歴に表示されていることに注意してください。

    ![既存の展開の一覧](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *既存の展開の一覧*
13. 前回のコミットを選択し、クリックして**再展開**コマンド バーでします。

    ![前回のコミットを再配置](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *前回のコミットを再配置*
14. 確認するメッセージが表示されたら、クリックして**はい**です。

    ![再展開を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. 展開が完了したら、web アプリとキーを押してブラウザー インスタンスに切り替える**ctrl キーを押しながら f5 キーを押して**です。
16. 任意のオプションをクリックします。 フリップ アニメーション処理を実行し、結果 (*解決と不適切な*) が表示されます。
17. (省略可能)切り替えて、 **Git Bash**コンソールし、前回のコミットに戻すには、次のコマンドを実行します。

    > [!NOTE]
    > これらのコマンドは、Git リポジトリに不適切なコミットで行われたすべての変更を元に戻す新しい、コミットを作成します。 Azure は、新しい、コミットを使用して、アプリケーションとし、再展開します。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>手順 4: スケールが Azure ストレージを使用します。

**Blob**は、大量の非構造化テキストまたはビデオ、オーディオ、画像などのバイナリ データを格納する最も簡単な方法です。 記憶域にアプリの静的コンテンツを移動するにはのイメージまたはブラウザーに直接ドキュメントを使用して、アプリケーションの規模に役立ちます。

この練習では、Blob コンテナーに、アプリケーションの静的なコンテンツを移動します。 追加するアプリケーションを構成するは、 **ASP.NET URL 書き換えルール**で、 **Web.config** Blob コンテナーにコンテンツをリダイレクトします。

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>作業 1: Azure ストレージ アカウントを作成します。

このタスクでは、管理ポータルを使用して新しいストレージ アカウントを作成する方法を学習します。

1. 移動し、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。
2. 選択**新しい |データ サービス |記憶域 |簡易作成**新しいストレージ アカウントの作成を開始します。 クリックし、アカウントの一意の名前を入力、**地域**一覧からです。 をクリックして**ストレージ アカウントの作成**を続行します。

    ![新しいストレージ アカウントを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "新しいストレージ アカウントを作成します。")

    *新しいストレージ アカウントを作成します。*
3. **ストレージ**セクションで、新しいストレージ アカウントの状態に変更されるまで待機*オンライン*次の手順を続行するためにします。

    ![作成されたストレージ アカウント](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "ストレージ アカウントの作成")

    *ストレージ アカウントの作成*
4. ストレージ アカウント名をクリックし、をクリックして、**ダッシュ ボード**ページの上部にリンクします。 **ダッシュ ボード** ページでは、アカウントと、アプリケーション内で使用できるサービスのエンドポイントの状態に関する情報。

    ![ストレージ アカウント ダッシュ ボードを表示する](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "ストレージ アカウント ダッシュ ボードを表示します。")

    *ストレージ アカウント ダッシュ ボードを表示します。*
5. クリックして、**アクセス キーの管理**ナビゲーション バーで、ボタンをクリックします。

    ![管理アクセス キー ボタン](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "アクセス キーの管理 ボタン")

    *管理アクセス キー ボタン*
6. **アクセス キーの管理**ダイアログ ボックスで、コピー、**ストレージ アカウント名**と**プライマリ アクセス キー**ように次の手順で必要となります。 次に、ダイアログ ボックスを閉じます。

    ![アクセス キーの管理 ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "アクセス キーの管理 ダイアログ ボックス")

    *管理アクセス キー ダイアログ ボックス*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>タスク 2: Azure Blob ストレージに資産をアップロードします。

このタスクでは、Visual Studio からサーバー エクスプ ローラー ウィンドウを使用して、ストレージ アカウントへの接続にします。 Blob コンテナーを作成し、マニア Quiz ロゴ ファイルをコンテナーにアップロードされます。

1. Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の手順からソリューションです。
2. メニュー バーから、選択**ビュー**  をクリックし、**サーバー エクスプ ローラー**です。
3. **サーバー エクスプ ローラー**を右クリックし、 **Azure**ノード**を Azure に接続しています.**.お客様のサブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。

    ![Windows Azure への接続します。](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Azure への接続します。*
4. 展開、 **Azure**  ノードを右クリックして**ストレージ**を選択し、**外部の記憶域をアタッチしています.**.
5. **新しいストレージ アカウントの追加** ダイアログ ボックスで、入力、**アカウント名**と**アカウント キー**をクリックして、前のタスクで取得した**OK**.

    ![新しいストレージ アカウント ダイアログ ボックスを追加します。](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *新しいストレージ アカウント ダイアログ ボックスを追加します。*
6. ストレージ アカウントが表示されるはずの**ストレージ**ノード。 ストレージ アカウントを展開しを右クリックして**Blob**選択**Blob コンテナーの作成しています.**.

    ![Blob コンテナーを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob コンテナーの作成")

    *Blob コンテナーを作成します。*
7. **Blob コンテナーの作成** ダイアログ ボックスで、blob コンテナーの名前を入力し、をクリックして**OK**です。

    ![作成する Blob コンテナー ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob コンテナーの作成 ダイアログ ボックス")

    *Blob コンテナー ダイアログ ボックスを作成します。*
8. 新しい blob コンテナーを追加する必要があります、 **Blob**ノード。 コンテナーを公開して、コンテナーのアクセス許可を変更します。 これを行うを右クリックし、**イメージ**コンテナーと選択**プロパティ**です。

    ![コンテナーのプロパティをイメージ](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "イメージのコンテナーのプロパティ")

    *イメージ コンテナーのプロパティ*
9. **プロパティ**ウィンドウで、設定、**パブリック読み取りアクセス**に**コンテナー**です。

    ![パブリック読み取りアクセスのプロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "読み取りパブリック アクセス プロパティを変更します。")

    *パブリック読み取りアクセスのプロパティを変更します。*
10. パブリック アクセス プロパティを変更するには、をクリックすることを確認する場合は入力を求められたら**はい**です。

    ![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio の警告")

    *Microsoft Visual Studio の警告*
11. **サーバー エクスプ ローラー**、右クリックし、**イメージ**blob コンテナーを選択**Blob コンテナーの**します。

    ![Blob コンテナーを表示](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob コンテナーを表示")

    *Blob コンテナーの表示*
12. イメージ コンテナーが新しいウィンドウで開く必要があり、エントリがない凡例を表示する必要があります。 クリックして、**アップロード**blob コンテナーにファイルをアップロードするにはアイコン。

    ![エントリがないイメージ コンテナー](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "エントリがないイメージ コンテナー")

    *エントリがないイメージ コンテナー*
13. **Blob のアップロード** ダイアログ ボックスに移動、**資産**ラボのフォルダーです。 選択、**ロゴ big.png**ファイルし、をクリックして**開く**です。
14. ファイルがアップロードされるまで待ちます。 アップロードが完了したら、ファイルをイメージのコンテナーに表示されるはずです。 ファイルのエントリを右クリックし  **URL のコピー**です。

    ![Blob の URL をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "blob ファイルの URL をコピー")

    *Blob の URL をコピーします。*
15. Internet Explorer を開き、URL を貼り付けます。 次の図は、ブラウザーに表示する必要があります。

    ![Windows の Blob ストレージからイメージをロゴ big.png](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "ストレージからロゴ big.png イメージ")

    *Azure Blob ストレージからロゴ big.png イメージ*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>タスク 3: Azure Blob ストレージからの静的なコンテンツを利用するソリューションの更新

このタスクでは、構成、 **GeekQuiz**イメージを使用するソリューションにアップロードされた Azure Blob ストレージではなく web アプリにあるイメージ) で ASP.NET URL 書き換えルールを追加することによって、 **web.config**ファイル。

1. Visual Studio で開く、 **Web.config**内のファイル、 **GeekQuiz**プロジェクトし、検索、  **&lt;system.webServer&gt;** 要素。
2. URL 書き換えルールをストレージ アカウント名のプレース ホルダーの更新を追加するには、次のコードを追加します。

    (コード スニペットの*WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL の書き換えは、受信 Web 要求を受け取り、別のリソースへの要求のリダイレクトのプロセスです。 URL 書き換えルールは、要求をリダイレクトする必要がある場合と、リダイレクト先にする、書き換えエンジンを指示します。 書き換え規則が 2 つの文字列で構成されます: 要求された URL 内で検索するパターン (通常は、正規表現を使用して) 場合は、そのパターンを置換する文字列が検出されたとします。 詳細については、次を参照してください。 [ASP.NET の URL の書き換え](https://msdn.microsoft.com/library/ms972974.aspx)です。
3. キーを押して**ctrl キーを押しながら S**変更を保存します。
4. 新しく開きます**Git Bash** Azure App Service に更新されたアプリケーションを配置するコンソール。
5. 変更を Azure にプッシュするには、次のコマンドを実行します。 更新プログラム、 *[、アプリケーション パス]*へのパスのプレース ホルダー、 **GeekQuiz**ソリューションです。 展開パスワードを求められます。

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Azure への更新の展開](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Azure への更新の展開*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>タスク 4: 検証

このタスクでは、使用**Internet Explorer**を参照する、**マニア Quiz**アプリケーションと URL がイメージの動作とするルールを書き直すことのチェックはでホストされているイメージにリダイレクト**Azure Blob記憶域**です。

1. Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。 以前に作成した資格情報を使用してをログインします。

    ![イメージとマニア Quiz web アプリケーションを表示](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "マニア Quiz で web アプリ イメージを表示")

    *イメージとマニア Quiz web アプリケーションを表示*
2. キーを押して**F12**開発ツールを起動するには、選択、**ネットワーク**タブし、記録を開始します。

    ![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ネットワークの記録を開始")

    *ネットワークの記録を開始*
3. キーを押して**ctrl キーを押しながら f5 キーを押して**web ページを更新します。
4. に対する HTTP 要求が表示されます、ページが読み込みを完了した後、 **/img/logo-big.png** 、http URL **301**結果 (リダイレクト) と別の要求を`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL と HTTP **200**結果。

    ![URL リダイレクトを確認する](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開発ツールでのリダイレクトを示す")

    *URL リダイレクトを確認しています*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>手順 5: Web アプリの自動スケールを使用します。

> [!NOTE]
> Web の負荷のサポートを必要となるために、この手順は省略可能で&amp;パフォーマンス テストで使用可能なのみ**Visual Studio 2013 Ultimate Edition**です。 Visual Studio 2013 の特定の機能の詳細については、バージョンを比較[ここ](https://www.microsoft.com/visualstudio/eng/products/compare)です。


**Azure App Service Web Apps**で実行されている web アプリの自動スケール機能を提供して**標準モード**です。 自動スケールには、Azure の負荷に応じて、web アプリのインスタンス数を自動的にスケール設定ことができます。 自動スケールを有効にすると、Azure web アプリの CPU が 5 分ごとに 1 回をチェックされ、その時点で必要に応じてインスタンスが追加されます。 CPU 使用率が低い場合は、インスタンスが削除されます 2 時間ごとに web アプリのパフォーマンスが低下していないことを確認してください。

この手順で構成に必要な手順を参照してください、**自動スケール**の機能に関する、**マニア Quiz** web アプリです。 インスタンスのアップグレードをトリガーするアプリケーションで十分な CPU の負荷を生成する Visual Studio ロード テストを実行して、この機能を確認します。

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>作業 1: CPU メトリックに基づいて自動スケールを構成します。

このタスクでは、Azure 管理ポータルを使用して手順 2. で作成した web アプリケーションの自動スケール機能を有効にします。

1. [Azure 管理ポータル](https://manage.windowsazure.com/)[ **web サイト**手順 2 で作成した web アプリケーション] をクリックします。
2. 移動し、**スケール**ページ。 下にある、**容量**セクションで、 **CPU**の**メトリックによるスケール**構成します。

    > [!NOTE]
    > CPU によるスケール、Azure は、CPU 使用率が変更された場合、アプリが使用するインスタンスの数を動的に調整されます。

    ![CPU によるスケールを選択する](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "自動スケーリングの CPU のメトリックを選択します。")

    *CPU によるスケールを選択します。*
3. 変更、**ターゲット CPU**構成**20**-**40** % です。

    > [!NOTE]
    > この範囲は、web アプリの平均 CPU 使用率を表します。 Azure では、追加または web アプリをこの範囲内で保持するインスタンスを削除します。 スケーリングに使用されたインスタンスの最小値と最大数が指定された、**インスタンス カウント**構成します。 Azure は、上またはその制限を超えるは接続されません。
    > 
    > 既定値**ターゲット CPU**だけのため、このラボの値が変更されます。 CPU の範囲を小さい値に構成するは、中程度の負荷がアプリケーションに配置された自動スケールを発生させる可能性が大きくしています。

    ![20 ~ 40% を指定する CPU ターゲットを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "20 ~ 40% を指定する CPU ターゲットを変更します。")

    *20 ~ 40% を指定するターゲット CPU を変更します。*
4. をクリックして**保存**変更を保存するコマンド バーにします。

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>タスク 2-Visual Studio でロード テスト

これで**自動スケール**された構成を作成、 **Web パフォーマンスとロード テストのプロジェクト**web アプリである程度の CPU 負荷を生成する Visual Studio でします。

1. 開いている**Visual Studio Ultimate 2013**選択**ファイル |新しい |プロジェクトの追加.**を新しいソリューションを開始します。

    ![新しいプロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "新規プロジェクトの作成")

    *新しいプロジェクトを作成します。*
2. **新しいプロジェクト**ダイアログ ボックスで、 **Web パフォーマンスとロード テストのプロジェクト**下にある、 **Visual c# |テスト**タブです。確認してください**.NET Framework 4.5**がプロジェクトに名前を選択すると、 *WebAndLoadTestProject*を選択、**場所** をクリック**OK**です。

    ![新しい Web およびロード テスト プロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Web とロード テスト プロジェクトを新規作成")

    *新しい Web およびロード テスト プロジェクトを作成します。*
3. **WebTest1.webtest**を右クリックし、 **WebTest1**ノードとクリック**要求の追加**です。

    ![WebTest1 への要求の追加](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1 への要求の追加")

    *WebTest1 への要求の追加*
4. **プロパティ**ウィンドウ、新しい要求のノードの更新、 **Url** web アプリの URL を参照するプロパティ (例:  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).

    ![Url プロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url プロパティを変更します。")

    *Url プロパティを変更します。*
5. **WebTest1.webtest**  ウィンドウを右クリックして**WebTest1**  をクリック**ループを追加しています.**.

    ![WebTest1 にループを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1 にループを追加します。")

    *WebTest1 にループを追加します。*
6. **条件付き規則の追加と項目をループ**ダイアログ ボックスで、 **For ループ**ルールし、次のプロパティを変更します。

    1. **終了値:** 1000
    2. **コンテキスト パラメーター名:**反復子
    3. **増分値:** 1

    ![For ループ ルールを選択し、プロパティを更新](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For ループ ルールを選択し、プロパティの更新")

    *For ループ ルールを選択し、プロパティの更新*
7. 下にある、**ループ内の項目**セクションで、ループの最初と最後の項目にする、以前に作成する要求を選択します。 **[OK]** をクリックして続行します。

    ![ループの最初と最後の項目を選択する](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "ループの最初と最後の項目を選択します。")

    *ループの最初と最後の項目を選択します。*
8. **ソリューション エクスプ ローラー**を右クリックし、 **WebAndLoadTestProject**プロジェクトで、展開、**追加**メニュー**ロード テストしています.**.

    ![WebAndLoadTestProject プロジェクトへのロード テストの追加](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject プロジェクトへのロード テストの追加")

    *WebAndLoadTestProject プロジェクトへのロード テストの追加*
9. **新しいロード テスト ウィザード**ダイアログ ボックスで、をクリックして**次**です。

    ![新しいロード テスト ウィザード](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新しいロード テスト ウィザード")

    *新しいロード テスト ウィザード*
10. **シナリオ** ページで、**待ち時間を使用しないでください** をクリック**次**です。

    ![待ち時間を使用しないことを選択すると](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "待ち時間を使用しないように選択します。")

    *待ち時間を使用しないことを選択します。*
11. **ロード パターン** ページで、ことを確認して、**持続ロード**オプションを選択します。 変更、**ユーザー カウント**設定を**250**ユーザーとクリック**[次へ]**です。

    ![ユーザー カウントを 250 に変更する](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "250 に、ユーザーの数を変更します。")

    *ユーザー カウントを 250 に変更します。*
12. **テスト ミックス モデル** ページで、**テストの時系列順に基づいて** をクリック**次**です。

    ![テスト ミックス モデルを選択すると](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "テスト ミックス モデルを選択します。")

    *テスト ミックス モデルを選択します。*
13. **テスト ミックス モデル**] ページで [**追加しています.**テストをミックスに追加します。

    ![テストをテスト ミックスに追加する](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "テストをテスト ミックスに追加します。")

    *テストをテスト ミックスに追加します。*
14. **テストの追加**ダイアログ ボックスをダブルクリックして**WebTest1**にテストを追加する、**選択したテストの** ボックスの一覧です。 **[OK]** をクリックして続行します。

    ![WebTest1 テストを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1 テストを追加します。")

    *WebTest1 テストを追加します。*
15. 戻り、**テスト ミックス** ページで、をクリックして**次**です。

    ![テスト ミックスのページを完了](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "テスト ミックスのページを完了します。")

    *テスト ミックスのページを完了します。*
16. **ネットワーク ミックス**] ページで [**次**です。

    ![クリックすると、ネットワーク ミックス ページで次に](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "ネットワーク ミックスのページで次をクリックします。")

    *ネットワーク ミックスのページの [次へ] をクリックして*
17. **ブラウザー ミックス**] ページで、[**インターネット エクスプ ローラー 10.0**をクリックして、ブラウザーの種類として**次**です。

    ![ブラウザーの種類を選択すると](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "ブラウザーの種類を選択します。")

    *ブラウザーの種類を選択します。*
18. **カウンター セット**] ページで [**次**です。

    ![カウンター セット ページで [次へ] をクリックすると](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "カウンター セット] ページでは、[次をクリックすると")

    *カウンター セット ページで 次へ をクリックして*
19. **実行設定** ページで、設定、**ロード テストの継続**に**5 分** をクリック**完了**です。

    ![ロード テストの継続を 5 分に設定](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "ロード テストの継続を 5 分に設定します。")

    *ロード テストの継続を 5 分に設定します。*
20. **ソリューション エクスプ ローラー**をダブルクリックして、 **Local.settings**テストの設定を表示するファイル。 既定では、Visual Studio は、ローカル コンピューターを使用してテストを実行します。

    > [!NOTE]
    > 使用して、クラウドでロード テストを実行するテスト プロジェクトを構成する代わりに、 **Visual Studio Online (VSO)**です。 VSO より現実的なロードをシミュレートするサービスのテスト、CPU の使用率、使用可能なメモリ、ネットワーク帯域幅などのローカル環境の制約を回避するクラウド ベースのロードを提供します。 VSO を使用してロード テストを実行する方法の詳細については、次を参照してください。[この資料](https://www.visualstudio.com/get-started/load-test-your-app-vs)です。

    ![テストの設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>タスク 3-自動スケールの検証

今すぐ、前のタスクで作成したロード テストを実行し、負荷の下で web アプリの動作を確認します。

1. **ソリューション エクスプ ローラー**をダブルクリックして**LoadTest1.loadtest**をロード テストを開きます。

    ![LoadTest1.loadtest を開く](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest を開く")

    *開く LoadTest1.loadtest*
2. **LoadTest1.loadtest**ウィンドウで、ロード テストを実行するツールボックス内の最初のボタンをクリックします。

    ![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "ロード テストの実行")

    *ロード テストの実行*
3. ロード テストが完了するまで待機します。

    > [!NOTE]
    > ロード テストでは、web アプリに同時に要求を送信する複数のユーザーをシミュレートします。 テストが実行されている場合は、すべてのエラー、警告、またはロード テストの実行に関連するその他の情報を検出するために使用可能なカウンターを監視できます。

    ![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "ロード テストが完了するまで待機しています")

    *ロード テストの実行*
4. テストが完了すると、管理ポータルに戻りに移動、**スケール**web アプリのページです。 下にある、**容量** セクションで、表示されます、グラフの新しいインスタンスが自動的に展開されたことです。

    ![新しいインスタンスを自動的に展開](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *新しいインスタンスを自動的に展開*

    > [!NOTE]
    > グラフに表示を変更するには数分かかる場合があります (キーを押して**ctrl キーを押しながら f5 キーを押して**ページを更新するには、定期的に)。 すべての変更が表示されない場合、次を実行できます。
    > 
    > - ロード テストの期間を延長 (たとえば**10 分**)
    > - 最大値と最小値を減らして、**ターゲット CPU** web アプリの自動スケールの構成内の範囲
    > - クラウド内のロード テストの実行**Visual Studio Online**です。 詳細については[ここ](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを設定し、Azure で web アプリを実稼働環境にアプリケーションを配置する方法を学習しました。 使用して、データベースの更新を検出して開始した**Entity Framework Code First Migrations**、しを使用して、サイトの新しいバージョンを展開することで続き**Git**へのロールバックを実行して、サイトの最新の安定バージョン。 さらに、記憶域を使用して、静的なコンテンツを Blob コンテナーに移動するアプリをスケールする方法について学習しました。
