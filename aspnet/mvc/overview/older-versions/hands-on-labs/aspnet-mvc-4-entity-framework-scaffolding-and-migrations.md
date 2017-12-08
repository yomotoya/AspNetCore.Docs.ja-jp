---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "ASP.NET MVC 4 エンティティ フレームワークのスキャフォールディングと移行 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET MVC 4 コント ローラーのメソッドに慣れているか、完了したかどうか、&quot;ヘルパー、フォーム、および検証&quot;ハンズオン ラボは、注意してください."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 エンティティ フレームワークのスキャフォールディングと移行
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> ASP.NET MVC 4 コント ローラーのメソッドに慣れているか、完了したかどうか、&quot;ヘルパー、フォーム、および検証&quot;ハンズオン ラボは、注意すべきことを作成するためのロジックの多くの更新、ボックスの一覧およびが繰り返される任意のデータ エンティティを削除します。間でのアプリケーションです。 モデルにいくつかのクラスを操作する場合はもちろんのことの各エンティティ操作だけでなく、ビューの各アクション メソッド POST および GET を書き込み、かなりの時間を費やす可能性がありますができます。
> 
> このラボでは、ASP.NET MVC 4 のスキャフォールディングを使用して、アプリケーションの CRUD (作成、読み取り、更新、削除) のベースラインを自動的に生成する方法を学習します。 以降は、単純なモデル クラス、および、1 行のコードを記述することがなく、すべての CRUD 操作だけでなく、すべての必要なビューを格納するコント ローラーを作成します。 構築し、単純なソリューションを実行すると、生成されると、MVC ロジックとデータ操作にビューと共に、アプリケーション データベースがあります。
> 
> さらに、Entity Framework の移行を使用して、アプリケーション全体のモデルの更新を実行するは簡単な方法を学習します。 Entity Framework の移行を使用すると、簡単な手順と、モデルが変更された後に、データベースを変更できます。 これらすべての点に注意、ことができますをビルドして、web アプリケーションをより効率的に管理する ASP.NET MVC 4 の最新の機能を活用します。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- コント ローラーでの CRUD 操作のためには、ASP.NET のスキャフォールディングを使用します。
- Entity Framework Migrations を使用して、データベース モデルを変更します。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。 実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;です。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の手順は、このハンズオン ラボを構成します。

1. [Entity Framework の移行で ASP.NET MVC 4 のスキャフォールディングを使用します。](#Exercise1)

> [!NOTE]
> この練習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習によって、作業のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **30 分**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>手順 1: Entity Framework の移行で ASP.NET MVC 4 のスキャフォールディングを使用します。

ASP.NET MVC のスキャフォールディングでは、により、アプリケーションは、データベース層と対話するために必要なロジックを作成する標準化された方法での CRUD 操作を生成する簡単な方法を提供します。

この演習では、CRUD メソッドを作成する最初のコードで ASP.NET MVC 4 のスキャフォールディングを使用する方法を学習します。 次に、Entity Framework の移行を使用して、データベース内の変更を適用するモデルを更新する方法を学習します。

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>スキャフォールディングを使用してタスク 1 で作成する新しい ASP.NET MVC 4 プロジェクト

1. まだ開いていない場合は開始**Visual Studio 2012**です。
2. 選択**ファイル |新しいプロジェクト**です。 [新規プロジェクト] ダイアログで、 **Visual c# |Web**セクションで、 **ASP.NET MVC 4 Web Application**です。 プロジェクトに名前を**MVC4andEFMigrations**の場所を設定および**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**このラボのフォルダーです。 設定、**ソリューション名**に**開始**を確認してください**ソリューションのディレクトリを作成**がオンになっています。 **[OK]** をクリックします。

    ![新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス")

    *新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス*
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**インターネット アプリケーション**テンプレート、ことを確認および**Razor**は、選択した**ビュー エンジン**. をクリックして**OK**プロジェクトを作成します。

    ![新しい ASP.NET MVC 4 のインターネット アプリケーション](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新しい ASP.NET MVC 4 のインターネット アプリケーション")

    *新しい ASP.NET MVC 4 のインターネット アプリケーション*
4. ソリューション エクスプ ローラーで右クリック**モデル**選択**追加 |クラス**を単純なクラス人 (POCO) を作成します。 名前を付けます**Person**  をクリック**OK**です。
5. ユーザー クラスを開き、次のプロパティを挿入します。

    (コード スニペットの*ASP.NET MVC 4 および Entity Framework の移行 - Ex1 Person プロパティ*)


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. をクリックして**ビルド |ソリューションをビルド**変更を保存し、プロジェクトをビルドします。

    ![アプリケーションのビルド](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "アプリケーションのビルド")

    *アプリケーションのビルド*
7. ソリューション エクスプ ローラーで、controllers フォルダーを右クリックして**追加 |コント ローラー**です。
8. コント ローラーの名前を付けます*PersonController*を完了し、**スキャフォールディング オプション**次の値。

    1. **テンプレート**ドロップダウン リストで、**読み取り/書き込みアクションと Entity Framework を使用して、ビューの MVC コント ローラー**オプション。
    2. **モデル クラス**ドロップダウン リストで、 **Person**クラスです。
    3. **データ コンテキスト クラス**一覧で、 **&lt;新しいデータ コンテキストしています.&gt;**. 任意の名前を選択し、クリックして**OK**です。
    4. **ビュー**ドロップダウン リストで、ことを確認して**Razor**が選択されています。

    ![スキャフォールディングがある人コント ローラーを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Person コント ローラーのスキャフォールディングを追加します。")

    *スキャフォールディングがある人コント ローラーを追加します。*
9. をクリックして**追加**スキャフォールディングとユーザーを新しいコント ローラーを作成します。 コント ローラーのアクションだけでなく、ビューを生成したようになりました。

    ![スキャフォールディングを含む Person コント ローラーを作成したら](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "スキャフォールディングで人コント ローラーを作成した後")

    *スキャフォールディングで人コント ローラーを作成した後*
10. 開いている**PersonController**クラスです。 完全 CRUD アクション メソッドが自動的に生成されたことに注意してください。

    ![Person コント ローラー内](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "ユーザーの内部コント ローラー")

    *Person コント ローラー内*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>タスク 2-実行中のアプリケーション

この時点では、データベースはまだは作成されません。 このタスクでは、最初にアプリケーションを実行し、CRUD 操作をテストします。 データベースは、Code First で即座に作成されます。

1. **F5** キーを押してアプリケーションを実行します。
2. ブラウザーで、追加**/Person**ユーザー ページを開くための URL にします。

    ![アプリケーションの初回実行](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "アプリケーションの初回実行")

    *アプリケーション: 最初の実行します。*
3. ようになりましたが、ユーザーのページの調査および、CRUD 操作をテストします。

    1. をクリックして**新規作成**を新しいユーザーを追加します。 名前、姓と名を入力し、クリックして**作成**です。

        ![新しいユーザーを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "新しいユーザーを追加します。")

        *新しいユーザーを追加します。*
    2. 人の一覧で、削除、編集または項目を追加することができます。

        ![人物リスト](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "人物リスト")

        *人物リスト*
    3. をクリックして**詳細**人の詳細を表示します。

        ![ユーザーの詳細](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人の詳細")

        *ユーザーの詳細*
4. ブラウザーを閉じて、Visual Studio に戻ります。 Person エンティティに対して CRUD 全体を 1 行のコードを記述しなくても、モデル、ビューから、アプリケーション全体で作成したことを確認します。

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>タスク 3-更新 Entity Framework Migrations を使用して、データベース

このタスクでは、Entity Framework Migrations を使用してデータベースが更新されます。 どのように簡単にモデルを変更し、Entity Framework 移行機能を使用して、データベースの変更が反映することがわかります。

1. パッケージ マネージャー コンソールを開きます。 選択**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。
2. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![移行を有効にする](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "移行を有効にします。")

    *移行を有効にします。*

    移行を有効にするコマンドは、作成、**移行**フォルダーで、データベースを初期化するためのスクリプトが含まれています。

    ![Migrations フォルダー](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations フォルダー")

    *Migrations フォルダー*
3. 開く、**される Configuration.cs** Migrations フォルダー内のファイルです。 クラスのコンス トラクターを検索し、変更、 **AutomaticMigrationsEnabled**値*true*です。


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. ユーザー クラスを開き、個人のミドル ネームの属性を追加します。 この新しい属性を持つモデルも変更されます。


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. 選択**ビルド |ソリューションをビルド**メニューにアプリケーションをビルドします。

    ![アプリケーションのビルド](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "アプリケーションのビルド")

    *アプリケーションのビルド*
6. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    このコマンドはデータ オブジェクトの変更を検索し、その後、それに応じてデータベースを変更するために必要なコマンドが追加します。

    ![ミドル ネームを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ミドル ネームを追加します。")

    *ミドル ネームを追加します。*
7. (省略可能)差分更新を含む SQL スクリプトを生成するには、次のコマンドを実行することができます。 これにより、データベースを手動で更新する (この場合は必要はありません)、またはその他のデータベースの変更を適用します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![SQL スクリプトを生成して](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL スクリプトの生成")

    *SQL スクリプトの生成*

    ![SQL スクリプトの update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL スクリプトの更新")

    *SQL スクリプトの更新*
8. パッケージ マネージャー コンソールで、データベースを更新するには、次のコマンドを入力します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![データベースの更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "データベースの更新")

    *データベースの更新*

    これが追加されます、 **MiddleName**内の列、**人**の現在の定義と一致するテーブル、 **Person**クラスです。
9. データベースが更新されると、コント ローラーのフォルダーを右クリックし **追加 |コント ローラー** Person コント ローラー再度 (同じ値を持つ完了) を追加します。 これは、既存のメソッドと、新しい属性を追加するビューで更新されます。

    ![コント ローラーの更新を追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "コント ローラーの更新を追加します。")

    *コント ローラーの更新*
10. **[追加]**をクリックします。 値を選択して、**上書き PersonController.cs**と**上書きするには、ビューが関連付けられている** をクリック**ok**です。

    ![コント ローラーの上書きを追加します。](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    *コント ローラーの更新*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - アプリケーションの実行

1. **F5** キーを押してアプリケーションを実行します。
2. 開いている**/Person**です。 データが保持されて、ミドル ネーム列が追加されたときに注意してください。

    ![追加のミドル ネーム](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ミドル ネームの追加")

    *ミドル ネームの追加*
3. クリックすると**編集**、ミドル ネームを現在のユーザーに追加することができます。

    ![ミドル ネーム エディション](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ミドル ネームのエディション")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>概要

このハンズオン ラボでは、どのモデル クラスを使用して ASP.NET MVC 4 スキャフォールディングでの CRUD 操作を作成する簡単な手順を学習しました。 次に、Entity Framework の移行を使用して、ビューへのデータベースから、アプリケーションでエンド ツー エンドの更新プログラムを実行する方法を学習しました。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express Web タイルを*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>付録 b: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
