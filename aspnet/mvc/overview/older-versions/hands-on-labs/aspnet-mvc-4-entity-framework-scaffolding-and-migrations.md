---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework スキャフォールディングと移行 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 コント ローラーのメソッドに慣れてまたは、完了したかどうか、&quot;ヘルパー、フォーム、検証&quot;ハンズオン ラボは、注意する必要が.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 10a61b70ef52aa9f5bb9004df3dba9e323d021db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833926"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework スキャフォールディングと移行

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 コント ローラーのメソッドに慣れてまたは、完了したかどうか、&quot;ヘルパー、フォーム、検証&quot;ハンズオン ラボは、注意すべきことを作成するためのロジックの多くを更新、一覧表示およびこれが繰り返される任意のデータ エンティティの削除間でのアプリケーションです。 モデルにいくつかのクラスを操作する場合はもちろんのことを各エンティティ操作だけでなく各ビューの POST および GET アクション メソッドの記述、かなりの時間を費やす可能性ができます。

このラボでは、ASP.NET MVC 4 のスキャフォールディングを使用して、アプリケーションの CRUD (作成、読み取り、更新、削除) のベースラインを自動的に生成する方法を学びます。 以降は、単純なモデル クラス、および、1 行のコードを記述することがなく、すべての CRUD 操作だけでなく、すべての必要なビューを含むコント ローラーを作成します。 構築し、単純なソリューションを実行すると、生成されると、MVC のロジックとデータ操作にビューと共に、アプリケーション データベースがあります。

さらに、Entity Framework の移行を使用して、アプリケーション全体のモデルの更新プログラムを実行するがいかに簡単かを学習します。 Entity Framework の移行を使用すると、簡単な手順で、モデルが変更された後に、データベースを変更できます。 これらすべての点で、できなくをビルドして、web アプリケーションをより効率的に管理する ASP.NET MVC 4 の最新の機能を活用します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 Entity Framework スキャフォールディングと移行](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations)します。

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- コント ローラーでの CRUD 操作には、ASP.NET のスキャフォールディングを使用します。
- Entity Framework の移行を使用して、データベース モデルを変更します。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。 実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;します。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の演習では、このハンズオン ラボを構成します。

1. [Entity Framework の移行と ASP.NET MVC 4 のスキャフォールディングを使用します。](#Exercise1)

> [!NOTE]
> この演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 演習を通して追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **30 分**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>演習 1: Entity Framework の移行での ASP.NET MVC 4 のスキャフォールディングを使用します。

ASP.NET MVC のスキャフォールディングにより、アプリケーションで、データベース層と対話するために必要なロジックを作成する、標準化された方法で CRUD 操作を生成する簡単な方法を提供します。

この演習では、CRUD メソッドを作成する最初のコードを ASP.NET MVC 4 のスキャフォールディングを使用する方法を学びます。 次に、Entity Framework の移行を使用してデータベースの変更を適用してモデルを更新する方法を学習します。

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>タスク 1-作成する新しい ASP.NET MVC 4 プロジェクトのスキャフォールディングを使用

1. 既に開かれていない場合は開始**Visual Studio 2012**します。
2. 選択**ファイル |新しいプロジェクト**します。 [プロジェクト] ダイアログ、new、 **(Visual C#) |Web**セクションで、 **ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトに名前を**MVC4andEFMigrations**に場所を設定および**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**このラボのフォルダー。 設定、**ソリューション名**に**開始**を確認して**ソリューションのディレクトリを作成**がチェックされます。 **[OK]** をクリックします。

    ![新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス")

    *新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス*
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**インターネット アプリケーション**テンプレート、ことを確認および**Razor**が、選択されている**ビュー エンジン**. **[OK]** をクリックして、プロジェクトを作成します。

    ![新しい ASP.NET MVC 4 のインターネット アプリケーション](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新しい ASP.NET MVC 4 のインターネット アプリケーション")

    *新しい ASP.NET MVC 4 のインターネット アプリケーション*
4. ソリューション エクスプ ローラーで右クリックして**モデル**選択**追加 |クラス**単純なクラス person (POCO) を作成します。 名前を付けます**人** をクリック**OK**します。
5. Person クラスを開き、次のプロパティを挿入します。

    (コード スニペット - *ASP.NET MVC 4 と Entity Framework の移行 - Ex1 Person プロパティ*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. クリックして**ビルド |ソリューションをビルド**変更を保存し、プロジェクトをビルドします。

    ![アプリケーションのビルド](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "アプリケーションを構築します。")

    *アプリケーションのビルド*
7. ソリューション エクスプ ローラーでは、controllers フォルダーを右クリックして**追加 |コント ローラー**します。
8. コント ローラーに名前*PersonController*を完了して、**スキャフォールディングのオプション**次の値。

   1. **テンプレート**ドロップダウン リストで、**読み取り/書き込みアクションと Entity Framework を使用して、ビューがある MVC コント ローラー**オプション。
   2. **モデル クラス**ドロップダウン リストで、 **Person**クラス。
   3. **データ コンテキスト クラス**一覧で、 **&lt;新しいデータ コンテキスト.&gt;**. 任意の名前を選択し、クリックして**OK**します。
   4. **ビュー**ドロップダウン リストで、ことを確認します**Razor**が選択されています。

      ![スキャフォールディングと Person のコント ローラーを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "スキャフォールディングと Person のコント ローラーを追加します。")

      *スキャフォールディングと Person のコント ローラーを追加します。*
9. クリックして**追加**スキャフォールディングとユーザーを新しいコント ローラーを作成します。 コント ローラー アクションとビューを生成したようになりました。

    ![Person コント ローラーをスキャフォールディングを作成したら](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Person コント ローラーをスキャフォールディングで作成した後")

    *Person コント ローラーをスキャフォールディングで作成した後*
10. 開いている**PersonController**クラス。 完全な CRUD アクション メソッドが自動的に生成されたことに注意してください。

   ![Person コント ローラーの内部](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "内で、Person のコント ローラー")

   *Person コント ローラーの内部*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>タスク 2-実行中のアプリケーション

この時点では、データベースがまだ作成されていません。 このタスクでは、最初にアプリケーションを実行し、CRUD 操作をテストします。 データベースは、Code First ですぐに作成されます。

1. **F5** キーを押してアプリケーションを実行します。
2. ブラウザーで、追加 **/Person**人 ページを開くための URL にします。

    ![アプリケーションの初回実行](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "アプリケーションの初回実行")

    *アプリケーションの場合: 最初の実行します。*
3. ユーザー ページを調べ、CRUD 操作のテストはようになりました。

    1. クリックして**新規作成**新しいメンバーを追加します。 名と姓を入力し、クリックして**作成**です。

        ![新しいユーザーを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "新しいユーザーを追加します。")

        *新しいユーザーを追加します。*
    2. ユーザーの一覧で、削除、編集または項目を追加することができます。

        ![人物リスト](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "人物リスト")

        *人物リスト*
    3. クリックして**詳細**人の詳細を開きます。

        ![ユーザーの詳細](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人の詳細")

        *ユーザーの詳細*
4. ブラウザーを閉じて、Visual Studio に戻ります。 Person エンティティの全体の CRUD を 1 行のコードを記述しなくても、ビューに、モデルからアプリケーション全体で作成したことを確認します。

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>タスク 3-更新の Entity Framework の移行を使用して、データベース

このタスクでは、Entity Framework の移行を使用してデータベースを更新します。 モデルを変更し、Entity Framework の移行機能を使用して、データベースの変更が反映がいかに簡単かが検出されます。

1. パッケージ マネージャー コンソールを開きます。 選択**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**します。
2. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![移行を有効にする](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Migrations を有効にします。")

    *移行を有効にします。*

    移行の有効化コマンドは、作成、**移行**フォルダーで、データベースを初期化するためのスクリプトが含まれています。

    ![Migrations フォルダー](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations フォルダー")

    *Migrations フォルダー*
3. 開く、 **Configuration.cs** Migrations フォルダーにファイル。 クラスのコンス トラクターを検索し、変更、 **AutomaticMigrationsEnabled**値を*true*します。

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Person クラスを開き、個人のミドル ネームの属性を追加します。 この新しい属性では、モデルを変更します。

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. 選択**ビルド |ソリューションをビルド**メニュー アプリケーションをビルドします。

    ![アプリケーションのビルド](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "アプリケーションを構築します。")

    *アプリケーションのビルド*
6. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    このコマンドは、データ オブジェクトの変更を検索し、それに応じて、データベースを変更するために必要なコマンドを追加し、します。

    ![ミドル ネームを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ミドル ネームを追加します。")

    *ミドル ネームを追加します。*
7. (省略可能)差分の更新プログラムでの SQL スクリプトを生成するには、次のコマンドを実行することができます。 これにより、データベースを手動で更新する (この場合必要はありません)、または他のデータベースで変更を適用します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![SQL スクリプトを生成して](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL スクリプトの生成")

    *SQL スクリプトの生成*

    ![SQL スクリプトの update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL スクリプトの更新")

    *SQL スクリプトの更新*
8. パッケージ マネージャー コンソールでは、データベースを更新する次のコマンドを入力します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![データベースの更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "データベースの更新")

    *データベースの更新*

    これは追加、 **MiddleName**内の列、**人**の現在の定義と一致するテーブル、**人**クラス。
9. データベースを更新すると、コント ローラーのフォルダーを右クリックし **追加 |コント ローラー** Person コントをもう一度 (同じ値で完了) を追加します。 これは、既存のメソッドとビューの新しい属性を追加することで更新されます。

    ![コント ローラーの更新を追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "コント ローラーの更新を追加します。")

    *コント ローラーを更新しています*
10. **[追加]** をクリックします。 値を選択し、**上書き PersonController.cs**と**上書きするには、ビューが関連付けられている** をクリック**ok**します。

   ![コント ローラーの上書きを追加します。](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *コント ローラーを更新しています*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - アプリケーションの実行

1. **F5** キーを押してアプリケーションを実行します。
2. 開いている **/Person**します。 データが保持されて、ミドル ネームの列が追加されたときに注意してください。

    ![追加のミドル ネーム](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ミドル ネームの追加")

    *ミドル ネームの追加*
3. クリックすると**編集**、ミドル ネームを現在のユーザーに追加することができます。

    ![ミドル ネーム edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ミドル ネームのエディション")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボでは、どのモデル クラスを使用して ASP.NET MVC 4 スキャフォールディングで CRUD 操作を作成する簡単な手順を説明しました。 次に、元のデータベースのビューに、アプリケーションで Entity Framework の移行を使用してエンド ツー エンドの更新プログラムを実行する方法を説明しました。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express for Web のタイル*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>付録 b: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
