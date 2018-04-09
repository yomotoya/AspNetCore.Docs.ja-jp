---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 ヘルパー、フォーム、および検証 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET MVC 4 モデルおよびデータ アクセスのハンズオン ラボでされて読み込みしてデータベースからデータを表示します。 このハンズオン ラボに追加します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 ヘルパー、フォーム、および検証

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

**ASP.NET MVC 4 モデルおよびデータ アクセス**ハンズオン ラボされている必要が読み込みと、データベースからデータを表示します。 このハンズオン ラボに追加、 **Music Store**アプリケーション データを編集する機能。

その目的を念頭にするとには、アルバムの作成、読み取り、更新、削除 (CRUD) 操作をサポートするコント ローラーを作成します。 HTML テーブルのアルバムのプロパティを表示する ASP.NET MVC のスキャフォールディング機能の活用インデックス ビュー テンプレートが生成されます。 そのビューを強化するためには、カスタム HTML ヘルパーの長い説明を切り捨てることを追加します。

その後、編集しているビューを作成するために使用する alter のドロップダウン リストのようなフォーム要素を利用して、データベースのアルバムを追加します。

最後に、するは、ユーザーがアルバムを削除してもするにその入力を検証することによって正しくないデータを入力します。

このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。 使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC の基本事項**ハンズオン ラボ。

このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。 このラボに固有のプロジェクトは[ASP.NET MVC 4 ヘルパー、フォーム、および検証](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)です。

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- CRUD 操作をサポートするためにコント ローラーを作成します。
- HTML テーブルにエンティティのプロパティを表示する、インデックス ビューを生成します。
- カスタム HTML ヘルパーを追加します。
- 作成し、編集ビューをカスタマイズします。
- HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッドの違い
- 追加し、ビューの作成のカスタマイズ
- エンティティの削除を処理します。
- ユーザー入力を検証します。

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

次の演習では、このハンズオン ラボを構成します。

1. [ストア マネージャー コント ローラーとそのインデックス ビューを作成します。](#Exercise1)
2. [HTML ヘルパーを追加します。](#Exercise2)
3. [編集ビューを作成します。](#Exercise3)
4. [作成ビューを追加します。](#Exercise4)
5. [削除の処理](#Exercise5)
6. [検証の追加](#Exercise6)
7. [クライアント側での控えめな jQuery の使用](#Exercise7)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **60 分**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>手順 1: ストア マネージャー コント ローラーとそのインデックス ビューを作成します。

この演習では、CRUD 操作をサポートするには、そのインデックスは、データベースと ASP.NET MVC のスキャフォールディングを活用インデックス ビュー テンプレートの最後に生成からアルバムの一覧を返すメソッドをアクションのカスタマイズを新しいコント ローラーを作成する方法を学習しますHTML テーブルのアルバムのプロパティを表示する機能です。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>タスク 1 -、StoreManagerController を作成します。

このタスクと呼ばれる新しいコント ローラーを作成する**StoreManagerController** CRUD 操作をサポートします。

1. 開く、**開始**ソリューションにある**ソース/Ex1-CreatingTheStoreManagerController/開始/**フォルダーです。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 新しいコント ローラーを追加します。 これを行うを右クリックし、**コント ローラー** [ソリューション エクスプ ローラー] を選択内のフォルダー**追加**し、**コント ローラー**コマンド。 変更、**コント ローラー** **名前**に**StoreManagerController**オプションの確認と**の空の読み取り/書き込みアクションがあるMVCコントローラー**が選択されています。 **[追加]** をクリックします。

    ![コント ローラーの追加ダイアログ](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "コント ローラーの追加ダイアログ")

    *[追加] ダイアログのコント ローラー*

    新しいコント ローラー クラスが生成されます。 示されているアクションの読み取り/書き込み、スタブ メソッドを追加するので、一般的な CRUD ƒaƒnƒvƒ ‡ ƒ はアプリケーション固有のロジックを含むメッセージを表示、入力 TODO コメントで作成されます。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>タスク 2 - StoreManager インデックスをカスタマイズします。

このタスクでは、データベースからアルバムのリストとビューを返す StoreManager Index アクション メソッドをカスタマイズします。

1. StoreManagerController クラスで、次のコードを追加*を使用して*ディレクティブです。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex1 MvcMusicStore を使用して*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. フィールドを追加、 **StoreManagerController**のインスタンスを保持するために**MusicStoreEntities です。**

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. アルバムのリストとビューを返す StoreManagerController インデックスの操作を実装します。

    コント ローラー アクション ロジックは、以前に書き込まれる StoreController のインデックスの操作によく似ていますされます。 LINQ を使用すると、すべてのアルバム、ジャンル、アーティストの情報を表示するためなどを取得できます。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex1 StoreManagerController インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>タスク 3 - インデックス ビューを作成します。

このタスクでは、によって返されるアルバムの一覧を表示するインデックス ビュー テンプレートを作成します、 **StoreManager**コント ローラー。

1. 新しいテンプレートの表示を作成する前に、プロジェクトをビルドする必要がありますできるように、**ビューの追加 ダイアログ**知って、**アルバム**を使用するクラス。 選択**ビルド |ビルド MvcMusicStore**プロジェクトをビルドします。
2. 内部を右クリックし、**インデックス**アクション メソッドと選択**ビューの追加**です。 これが表示されます、**ビューの追加**ダイアログ。

    ![ビューを追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ビューを追加します。")

    *インデックス メソッド内からビューを追加します。*
3. ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**インデックス**です。 選択、**厳密に型指定されたビューを作成する**オプション、および選択**アルバム (MvcMusicStore.Models)**から、**モデル クラス**ドロップダウンします。 選択**リスト**から、 **Scaffold テンプレート**ドロップダウンします。 ままにして、**ビュー エンジン**に**Razor**クリックし、その既定値はその他のフィールド値**追加**です。

    ![インデックス ビューの追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "インデックス ビューの追加")

    *インデックス ビューの追加*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>タスク 4 - インデックス ビューのスキャフォールディングをカスタマイズします。

このタスクでは、ASP.NET MVC のスキャフォールディング機能に必要なフィールドを表示することで作成された単純なビュー テンプレートを調整します。

> [!NOTE]
> **スキャフォールディング**ASP.NET MVC でのサポートには、アルバムのモデルのすべてのフィールドの一覧を表示する単純なビュー テンプレートが生成されます。 **スキャフォールディング**厳密に型指定されたビューで作業を開始する簡単な方法を提供します。 ビュー テンプレートを手動で記述するのではなくすばやくスキャフォールディング テンプレートが生成される既定値とし、生成されたコードを変更することができます。


1. 作成したコードを確認します。 生成されたフィールドの一覧を次の一部となる HTML テーブルを**スキャフォールディング**表形式のデータを表示するために使用されています。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. 置換、 **&lt;テーブル&gt;**コードのみを表示する次のコードと、**ジャンル**、**アーティスト**、**アルバム タイトル**、および**価格**フィールドです。 これにより、削除、 **AlbumId**と**アルバム アート URL**列です。 GenreId と ArtistId を表示する列の場合は、そのリンクされたクラス プロパティを変更ことも、 **Artist.Name**と**Genre.Name**、し、削除、**詳細**リンクします。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. 次の説明を変更します。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションを実行します。

このタスクは、テストを**StoreManager** **インデックス**ビュー テンプレートは、前の手順のデザインに従ってアルバムの一覧を表示します。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager**アルバムの一覧が表示されることを示すことを確認する、**タイトル**、**アーティスト**と**ジャンル**です。

    ![アルバムの一覧を参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "アルバムの一覧を参照")

    *アルバムの一覧を参照*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>手順 2: HTML ヘルパーを追加します。

StoreManager インデックス ページが潜在的な問題の 1 つ。 タイトル、アーティスト名のプロパティは両方とも、テーブルの書式設定をスローするのに十分な長さです。 この演習では、そのテキストの切り捨てを行うためのカスタム HTML ヘルパーを追加する方法を学習します。

次の図では、サイズの小さなブラウザーを使用するときに、テキストの長さのために変更が、形式の方法がわかります。

![テキストは切り捨てられませんされたアルバムの一覧を参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "テキストは切り捨てられませんされたアルバムの一覧を参照")

*テキストは切り捨てられませんされたアルバムの一覧を参照*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>タスク 1 - された HTML ヘルパーを拡張します。

このタスクでは、新しいメソッドを追加します**Truncate**を**HTML** ASP.NET MVC ビューの中で公開されたオブジェクト。 これを行うにを実装する、**拡張メソッド**組み込み**System.Web.Mvc.HtmlHelper** ASP.NET MVC によって提供されるクラスです。

> [!NOTE]
> 詳細を確認する**拡張メソッド**、この msdn の記事を参照してください。 [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. 開く、**開始**ソリューションにある**ソース/Ex2-AddingAnHTMLHelper/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. StoreManager のインデックス ビューを開きます。 これを行うには、ソリューション エクスプ ローラーで、展開、**ビュー** 、フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。
3. 下の次のコードを追加、 <strong>@model</strong>を定義するディレクティブ、 <strong>Truncate</strong>ヘルパー メソッドです。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>作業 2: ページのテキストが切り捨て

このタスクでは使用して、 **Truncate**ビュー テンプレート内のテキストを短くします。

1. StoreManager のインデックス ビューを開きます。 これを行うには、ソリューション エクスプ ローラーで、展開、**ビュー** 、フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。
2. 表示する行を置き換える、**アーティスト名**およびアルバムの**タイトル**です。 これを行うには、次の行を置き換えます。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションを実行します。

このタスクは、テストを**StoreManager** **インデックス**アルバムのタイトル、アーティスト名、ビュー テンプレートが切り捨てられます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager**その時間の長いことを確認するテキストで、**タイトル**と**アーティスト**列は切り捨てられます。

    ![切り捨てのタイトルやアーティスト名](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "切り捨てタイトルやアーティスト名")

    *切り捨てられたタイトル、アーティスト名*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>手順 3: 編集ビューを作成します。

この演習では、ストア マネージャー アルバムを編集するためのフォームを作成する方法を学習します。 参照する、 **/StoreManager/Edit/id** URL (**id**を編集するアルバムの一意の id をされている)、サーバーへの HTTP GET 呼び出しになります。

アクション メソッドのコント ローラーの編集は、データベースから適切なアルバムを取得、作成、 **StoreManagerViewModel** (アーティスト、ジャンルの一覧)、およびカプセル化およびにビュー テンプレートに渡すオブジェクトユーザーに、HTML ページを表示します。 このページには、 **&lt;フォーム&gt;**テキスト ボックスとアルバムのプロパティを編集するためのドロップダウン リストを持つ要素。

ユーザーは、アルバムのフォーム値を更新しをクリックした後、**保存** ボタンを使用して、変更を送信する HTTP POST にコールバックする**/StoreManager/Edit/id**です。URL は、最後の呼び出しの場合と同様、ASP.NET MVC を識別するこの時間は、HTTP POST し、そのため、別の編集操作方法を実行 (で修飾された 1 つ**[HttpPost]**)。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>タスク 1 - HTTP GET 編集アクション メソッドの実装

このタスクでは、データベースから適切なアルバムを取得する編集のアクション メソッドの HTTP GET バージョンだけでなくすべてのジャンル、アーティストの一覧を実装します。 これはパッケージ化このデータ**StoreManagerViewModel**で応答を表示するためにテンプレートの表示にし、渡される、最後の手順で定義されているオブジェクト。

1. 開く、**開始**ソリューションにある**ソース/Ex3-CreatingTheEditView/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **StoreManagerController**クラスです。 これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。
3. 置換、 **HTTP-GET 編集**アクション メソッドを適切な取得は、次のコードを**アルバム**だけでなく**ジャンル**と**アーティスト**を一覧表示します。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォームと検証のアクションの Ex3 StoreManagerController HTTP-GET 編集*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > 使用している**System.Web.Mvc** **SelectList**アーティスト、ジャンルの代わりに、 **System.Collections.Generic**  ボックスの一覧です。
    > 
    > **SelectList**クリーナーの方法を HTML ドロップダウン リストを作成し、現在の選択などを管理します。 インスタンス化して、コント ローラーのアクションでこれらの ViewModel オブジェクトの後で設定すると、編集フォーム シナリオ クリーナーです。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>タスク 2 - 編集ビューを作成します。

このタスクでは、後で、アルバムのプロパティを表示するビューの編集 テンプレートを作成します。

1. 編集ビューを作成します。 これを行うには、内部を右クリックして、**編集**アクション メソッドと選択**ビューの追加**です。
2. ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**編集**です。 チェック、**厳密に型指定されたビューを作成する** チェック ボックスを選択し、**アルバム (MvcMusicStore.Models)**から、**データ クラスを表示**ドロップダウンします。 選択**編集**から、 **Scaffold テンプレート**ドロップダウンします。 既定値は、他のフィールドのままにし、をクリックして**追加**です。

    ![編集ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "編集ビューを追加します。")

    *編集ビューを追加します。*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションを実行します。

このタスクは、テストを**StoreManager** **編集**ビュー ページには、パラメーターとして渡されるアルバムのプロパティの値が表示されます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager/Edit/1**を渡されるアルバムのプロパティの値が表示されることを確認します。

    ![アルバムの編集ビューを参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "アルバムの編集ビューを参照")

    *アルバムの編集ビューを参照*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>タスク 4 - アルバム エディター テンプレートをドロップダウン リストを実装します。

このタスクでは、ユーザーは、アーティスト、ジャンルの一覧から選択できるように、前回の作業で作成したビュー テンプレートをドロップダウン リストを追加します。

1. [すべて置換]、**アルバム**を次のフィールド セット コード。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList**アーティスト、ジャンルを選択するためのドロップダウン リストを表示するためにヘルパーが追加されました。 渡されるパラメーター **Html.DropDownList**は。
    > 
    > 1. フォーム フィールドの名前 (**&quot;ArtistId&quot;**)。
    > 2. **SelectList**ドロップダウン リストの値。

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションを実行します。

このタスクは、テストを**StoreManager** **編集**ビュー ページには、アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager/Edit/1**アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されることを確認します。

    ![アルバムのドロップダウン リストでビューの編集をブラウズ](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "参照アルバムのドロップダウン リストでビューの編集")

    *アルバムの編集ビューのこの時点でのドロップダウン リストを参照*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>タスク 6 - HTTP POST Edit アクション メソッドを実装します。

ビューの編集は、期待どおりに表示されます、アルバムへの変更を保存するアクションの編集 HTTP POST メソッドを実装する必要があります。

1. Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。 開いている**StoreManagerController**から、**コント ローラー**フォルダーです。
2. 置き換える**HTTP POST 編集**を次のアクション メソッドのコード (置き換える必要があるメソッドを 2 つのパラメーターを受け取るオーバー ロードされたバージョンであることに注意してください)。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォームと検証のアクションの Ex3 StoreManagerController HTTP POST の編集*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > このメソッドは、ユーザーがクリックしたときに、実行は、**保存**ビューのボタンをクリックし、HTTP POST サーバーへのフォーム値のデータベース内に保持するを実行します。 デコレータ**[HttpPost]** HTTP POST シナリオのメソッドを使用することを示します。 メソッドは、**アルバム**オブジェクト。 ASP.NET MVC が、ポストされたからアルバム オブジェクトを自動的に作成します。&lt;フォーム&gt;値。
    > 
    > メソッドは、これらの手順が実行されます。
    > 
    > 1. モデルが有効な場合。
    > 
    >     1. 変更されたオブジェクトをマークするコンテキストでアルバム エントリを更新します。
    >     2. 変更を保存し、インデックス ビューにリダイレクトします。
    > 2. ViewBag が表示されます、モデルが有効でない場合、 **GenreId**と**ArtistId**ユーザーを許可する受信したアルバム オブジェクトでビューが返されますし、任意の必要な更新プログラムを実行します。

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>タスク 7 - アプリケーションを実行します。

このタスクは、テストを**StoreManager 編集**ビュー ページ実際には、更新されたアルバム データ、データベースに保存します。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager/Edit/1**です。 アルバムのタイトルを変更**ロード** をクリック**保存**です。 アルバムの一覧で、アルバムのタイトルが実際に変更されたことを確認します。

    ![アルバムを更新](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "アルバムの更新")

    *アルバムの更新*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>手順 4: 追加のビューを作成します。

これで、 **StoreManagerController**をサポートしている、**編集**機能は、ここでは、格納できるようにするビューの作成テンプレートを追加する方法を学習するにマネージャーが、アプリケーションに新しいアルバムを追加します。

内の 2 つの異なるメソッドを使用して作成するシナリオを実装のように編集機能を持つ、 **StoreManagerController**クラス。

1. 1 つのアクション メソッドは、ストア マネージャーを閲覧すると空のフォームに表示されます、 **StoreManager/作成**URL。
2. 2 番目のアクション メソッドは、シナリオを処理するストア マネージャーがクリックした、**保存**フォーム内でボタンをクリックし、値にバックアップを送信する、 **StoreManager/作成**URL、HTTP POST として。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>タスク 1 - HTTP-GET 作成アクション メソッドの実装

このタスクでは、HTTP GET 以外のバージョンにこのデータをパッケージ化、すべてのジャンル、アーティストの一覧を取得する作成アクション メソッドを実装します、 **StoreManagerViewModel**オブジェクトで、ビュー テンプレートに渡されます。

1. 開く、**開始**ソリューションにある**ソース/Ex4-AddingACreateView/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開いている**StoreManagerController**クラスです。 これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。
3. 置換、**作成**を次のアクション メソッドのコード。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex4 StoreManagerController HTTP-GET 作成アクション*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>タスク 2 - 作成ビューの追加

このタスクを新しい (空) のアルバム フォームを表示するビューの作成テンプレートを追加します。

1. 内部を右クリックし、**作成**アクション メソッドを選択**ビューの追加**です。 ビューの追加 ダイアログが表示されます。
2. ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**作成**です。 選択、**厳密に型指定されたビューを作成する**をクリックして**アルバム (MvcMusicStore.Models)**から、**モデル クラス**ドロップダウンおよび**の作成**から、 **Scaffold テンプレート**ドロップダウンします。 既定値は、他のフィールドのままにし、をクリックして**追加**です。

    ![作成ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "の追加-a の作成-view.png")

    *作成ビューの追加*
3. 更新プログラム、 **GenreId**と**ArtistId**次に示すように、ドロップダウン リストを使用するフィールド。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションを実行します。

このタスクは、テストを**StoreManager** **作成**ビュー ページには、アルバムの空のフォームが表示されます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**StoreManager/作成**です。 新しいアルバム プロパティを補正するための空のフォームが表示されることを確認します。

    ![空のフォームでビューを作成](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "の空のフォームでのビューの作成")

    *空のフォームでビューを作成します。*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>タスク 4 - HTTP POST を実装するアクション メソッドを作成します。

このタスクでは、HTTP POST のバージョンのユーザーがクリックしたときに呼び出される作成アクション メソッドを実装します、**保存**ボタンをクリックします。 メソッドは、データベースに新しいアルバムを保存する必要があります。

1. Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。 開いている**StoreManagerController**クラスです。 これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。
2. 置き換える**HTTP POST 作成**を次のアクション メソッドのコード。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex4 StoreManagerController HTTP POST 作成アクション*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > 作成アクションは前の編集操作メソッドに非常に似ていますが、オブジェクトを設定するには、変更済みとして代わりに追加されることをコンテキストにします。

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションを実行します。

このタスクは、テストを**StoreManager 作成**ビュー ページが新しいアルバムを作成することができ、StoreManager インデックス ビューをリダイレクトします。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**StoreManager/作成**です。 次の図のように、新しいアルバムのデータにすべてのフォーム フィールドを入力します。

    ![アルバムを作成する](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "アルバムを作成します。")

    *アルバムを作成します。*
3. StoreManager インデックスを含むビューを作成した新しいアルバムにリダイレクトすることを確認します。

    ![作成された新しいアルバム](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新しいアルバムの作成")

    *新しいアルバムの作成*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>手順 5: 削除の処理

アルバムを削除する機能はまだ実装されていません。 これは、この手順がどうなるのです。 内の 2 つの異なるメソッドを使用して、削除のシナリオを実装する前と同じように、 **StoreManagerController**クラス。

1. 1 つのアクション メソッドが確認フォームに表示されます。
2. 2 番目のアクション メソッドでは、フォームの送信を処理します。

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>タスク 1 - HTTP GET Delete アクション メソッドの実装

このタスクでは、HTTP GET バージョンのアルバムの情報を取得する Delete アクション メソッドを実装します。

1. 開く、**開始**ソリューションにある**ソース/Ex5-HandlingDeletion/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開いている**StoreManagerController**クラスです。 これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。
3. Delete コント ローラーのアクションには正確には、前のストアの詳細コント ローラー アクションと同じ: クエリして、**アルバム**オブジェクトを使用して、データベースから、 **id** URL を返しますで提供される、適切な**ビュー**です。 これを行うには、HTTP GET を置き換える**削除**を次のアクション メソッドのコード。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex5 処理削除 HTTP-GET 削除アクション*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. 内部を右クリックし、**削除**アクション メソッドを選択**ビューの追加**です。 ビューの追加 ダイアログが表示されます。
5. ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**削除**です。 選択、**厳密に型指定されたビューを作成する**をクリックして**アルバム (MvcMusicStore.Models)**から、**モデル クラス**ドロップダウンします。 選択**削除**から、 **Scaffold テンプレート**ドロップダウンします。 既定値は、他のフィールドのままにし、をクリックして**追加**です。

    ![削除ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "削除ビューを追加します。")

    *削除ビューを追加します。*
6. Delete テンプレートは、モデルからのすべてのフィールドを示しています。 アルバムのタイトルのみが表示されます。 これを行うには、ビューの内容を次のコードに置き換えます。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションを実行します。

このタスクは、テストを**StoreManager** **削除**ビュー ページには、確認削除フォームが表示されます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager**です。 クリックして削除する 1 つのアルバムを選択して**削除**新しいビューがアップロードされたことを確認してください。

    ![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "アルバムを削除します。")

    *アルバムを削除します。*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>タスク 3-HTTP POST Delete アクション メソッドの実装

このタスクでは、HTTP POST のバージョンのユーザーがクリックしたときに呼び出される Delete アクション メソッドを実装します、**削除**ボタンをクリックします。 メソッドは、データベースのアルバムを削除する必要があります。

1. Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。 開いている**StoreManagerController**クラスです。 これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。
2. 置き換える**HTTP POST 削除**を次のアクション メソッドのコード。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex5 処理削除 HTTP POST 削除アクション*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションを実行します。

このタスクは、テストを**StoreManager Delete**ビュー ページがアルバムを削除することができ、StoreManager インデックス ビューをリダイレクトします。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/StoreManager**です。 クリックして削除する 1 つのアルバムを選択して**を削除します。** クリックして、削除の確認**削除**ボタンをクリックします。

    ![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "アルバムを削除します。")

    *アルバムを削除します。*
3. 表示されないので、アルバムが削除されたことを確認してください、**インデックス**ページ。

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>手順 6: 検証の追加

現時点では、インプレースが作成および編集フォームには、任意の種類の検証は行いません。 場合は、ユーザーは、[価格] フィールドに必要なフィールドを空白または型の文字が、最初のエラーが表示されますが、データベースからなります。

アプリケーションに検証を追加するには、モデル クラスへのデータの注釈を追加します。 データ注釈が、モデルのプロパティに適用するルールを記述するを使用して、ASP.NET MVC くれますを適用し、ユーザーに適切なメッセージを表示します。

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>タスク 1 - データ注釈を追加します。

このタスクでは、データ注釈を作成および編集するページを構成するアルバム モデルに該当する場合、検証メッセージの表示を追加します。

シンプルなモデル クラスのデータ注釈を追加することはだけ処理を追加して、**を使用して**ステートメント**System.ComponentModel.DataAnnotation**、配置、 **[必須]**、適切なプロパティの属性です。 次の例のように、**名前**プロパティ ビューに必要なフィールドです。

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

これは、エンティティ データ モデルが生成されるこのアプリケーションのような場合、もう少し複雑です。 モデルのクラスに直接データ注釈を追加した場合が上書きされますデータベースからモデルを更新する場合。 代わりに、行うことができます、注釈を保持するためには存在し、モデルに関連付けられたメタデータ部分クラスを使用してクラスを使用して、 **[MetadataType]**属性。

1. 開く、**開始**ソリューションにある**ソース/Ex6-AddingValidation/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **Album.cs**から、**モデル**フォルダーです。
3. 置き換える**Album.cs**コンテンツの強調表示されたコードでは、次のように見えるようにします。

    > [!NOTE]
    > 行**[DisplayFormat(ConvertEmptyStringToNull=false)]**データ ソースのデータ フィールドが更新されたときに、モデルから空の文字列を null に変換されないことを示します。 この設定には、Entity Framework は、データ注釈は、フィールドを検証する前に、モデルに null 値を割り当てを行うときに例外を回避できます。

    (コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex6 アルバムのメタデータ部分クラス*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > これは、**アルバム**部分クラスには、 **MetadataType**属性を指す、 **AlbumMetaData**データ注釈のクラスです。 これらは、注釈を付ける、アルバムのモデルを使用しているデータの注釈属性の一部を示します。
    > 
    > - 必須 - プロパティが必須フィールドであることを示します
    > - DisplayName - フォーム フィールドと検証メッセージを使用するテキストを定義します。
    > - DisplayFormat - には、データ フィールドの表示方法および書式設定方法を指定します。
    > - StringLength - 文字列フィールドの最大長を定義します。
    > - 範囲 - 数値フィールドの最大値と最小値を設定
    > - ScaffoldColumn - により、エディターのフォームのフィールドを非表示にします。

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションを実行します。

このタスクではテストを作成および編集ページが、フィールドを検証する最後のタスクで選択された表示名を使用しています。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**StoreManager/作成**です。 表示名が、部分クラスにあるものと一致することを確認 (と同様に**アルバム アート URL**の代わりに**AlbumArtUrl**)
3. をクリックして**作成**フォームを入力しなくてもします。 対応する検証メッセージを取得することを確認します。

    ![[作成] ページでフィールドを検証](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "の作成 ページでフィールドの検証")

    *[作成] ページで検証済みのフィールド*
4. 同じでも発生することを確認することができます、**編集**ページ。 URL を変更して**/StoreManager/Edit/1**表示名が、部分クラスにあるものと一致することを確認し、(など**アルバム アート URL**の代わりに**AlbumArtUrl**)。 空、**タイトル**と**価格**フィールドとクリック**保存**です。 対応する検証メッセージを取得することを確認します。

    ![検証済みのフィールドの編集 ページ](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *検証済みのフィールドの編集 ページ*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>手順 7: クライアント側での控えめな jQuery の使用

この演習では、クライアント側で MVC 4 の控えめな jQuery 検証を有効にする方法を学習します。

> [!NOTE]
> 控えめな jQuery では、データ ajax プレフィックス JavaScript を使用して、インライン クライアント スクリプトをそのままの状態の出力ではなく、サーバー上のアクション メソッドを呼び出します。


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>タスク 1 - 有効にすると控えめな jQuery 前に、アプリケーションを実行しています。

このタスクでは、両方の検証モデルを比較するために jQuery をインクルードする前にアプリケーションを実行します。

1. 開く、**開始**ソリューションにある**ソース/Ex7-UnobtrusivejQueryValidation/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. **F5** キーを押してアプリケーションを実行します。
3. ホーム ページで、プロジェクトを開始します。 参照 **StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するフォームの入力なし。

    ![クライアント検証を無効に](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "クライアント検証を無効になっています")

    *クライアント検証を無効になっています*
4. ブラウザーで HTML ソース コードを開きます。

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>タスク 2 - 控えめなクライアント検証を有効にします。

このタスクでは、jQuery を有効に**控えめなクライアント検証**から**Web.config**ファイル。 これは既定ですべての新しい ASP.NET MVC 4 プロジェクトで false に設定します。 さらに、必要なスクリプト jQuery 控えめなクライアント検証作業をするため、参照を追加します。

1. 開いている**Web.Config**プロジェクトのルートにファイルを確認して、 **ClientValidationEnabled**と**UnobtrusiveJavaScriptEnabled** にキー値が設定**true**です。

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > また、同じ結果を得るための Global.asax.cs に対してコードでクライアント検証を有効にすることができます。
    > 
    > **HtmlHelper.ClientValidationEnabled = true**
    > 
    > さらに、カスタム動作のコント ローラーに ClientValidationEnabled 属性を割り当てることができます。
2. 開いている**Create.cshtml**で**Views\StoreManager**です。
3. 次のスクリプト ファイルを確認してください**jquery.validate**と**jquery.validate.unobtrusive**、ビューを介してで参照される、 &quot; **~/bundles/jqueryval**&quot;バンドルします。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > MVC 4 の新しいプロジェクトでは、これらすべての jQuery ライブラリが含まれています。 複数のライブラリを見つけることができます、 **スクリプト/**するプロジェクトのフォルダーです。
    > 
    > この検証を行うためには、ライブラリの機能は、jQuery フレームワーク ライブラリへの参照を追加する必要があります。 この参照が既に追加されているため、  **\_Layout.cshtml**ファイル、する必要はありませんをこの特定のビューに追加します。

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>タスク 3 - アプリケーションを使用して控えめな jQuery 検証を実行しています。

このタスクは、テストを**StoreManager**テンプレートは、ユーザーが新しいアルバムを作成するときに jQuery ライブラリを使用してクライアント側の検証を実行するビューを作成します。

1. **F5** キーを押してアプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 参照 **StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するフォームの入力なし。

    ![クライアント検証を有効になっている jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "有効になっている jQuery を使用したクライアントの検証")

    *有効になっている jQuery を使用したクライアントの検証*
3. ブラウザーで、ビューを作成するためのソース コードを開きます。

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > 控えめな jQuery がデータを使用して属性を追加する各クライアント検証規則の-val -*rulename*=&quot;*メッセージ*&quot;です。 その Unobtrusive タグの一覧を以下は、jQuery クライアント検証を実行するには、html 入力フィールドに挿入します。
   > 
   > - データ val
   > - データ val 番号
   > - データ val 範囲
   > - データ val 範囲 min/データ val range max
   > - データ val 必要
   > - Val 長さがデータに
   > - データ val 長さ最大/データ val 長さ分
   > 
   > モデルはすべてのデータ値が格納**データ注釈**です。 次に、サーバー側で動作するすべてのロジックは、クライアント側で実行できます。 たとえば、Price 属性に次の注釈をデータ モデル。
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > 控えめな jQuery を使用すると、生成されたコードです。
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを完了して、次のデータベースに格納されているデータを変更するユーザーを有効にする方法を学習しました。

- コント ローラーのアクションなどのインデックスを作成、編集、削除
- HTML テーブルのプロパティを表示するための ASP.NET MVC のスキャフォールディング機能
- ユーザーを向上させるためにカスタムの HTML ヘルパーが発生します。
- HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッド
- 同様のビュー テンプレートの作成や編集などの共有エディター テンプレート
- ドロップダウン リストなどの form 要素
- モデル検証のためデータ注釈
- JQuery 控えめなライブラリを使用してクライアント側の検証

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express Web タイルを*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>付録 b: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
