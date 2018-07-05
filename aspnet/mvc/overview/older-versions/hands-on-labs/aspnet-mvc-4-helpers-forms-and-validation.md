---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 ヘルパー、フォーム、および検証 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 モデルとデータ アクセスのハンズオン ラボでされて読み込みし、データベースからデータを表示します。 このハンズオン ラボに追加します.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: a84e35695fa08ac1bd4834d2803d2be76f863e5b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815922"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 ヘルパー、フォーム、および検証

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

**ASP.NET MVC 4 モデルとデータ アクセス**ハンズオン ラボでは、読み込みと、データベースからデータを表示するをされています。 このハンズオン ラボを追加、**ミュージック ストア**アプリケーション、そのデータを編集する機能。

その目的を念頭には、使用には、アルバムの作成、読み取り、更新、削除 (CRUD) 操作をサポートするコント ローラーを作成します。 HTML テーブルのアルバムのプロパティを表示する ASP.NET MVC のスキャフォールディング機能の活用、インデックス ビュー テンプレートが生成されます。 そのビューを強化するためには、長い説明が切り捨てられますが、カスタム HTML ヘルパーを追加します。

その後、フォームのドロップダウン リストの要素のヘルプで、データベースのアルバムを変更できるようにするビューの作成と編集を追加します。

最後に、アルバムを削除するユーザーができるようにし、もすることができなくなりますが入力を検証することで無効なデータを入力します。

このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**します。 使用していない場合**ASP.NET MVC**前を経由することを勧めします。 **ASP.NET MVC の基礎**ハンズオン ラボ。

このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 ヘルパー、フォーム、検証](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)です。

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- CRUD 操作をサポートするためにコント ローラーを作成します。
- HTML テーブルにエンティティのプロパティを表示する、インデックス ビューの生成します。
- カスタム HTML ヘルパーを追加します。
- 作成し、編集ビューをカスタマイズします。
- HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッドの区別します。
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

便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。 実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;します。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の演習では、このハンズオン ラボを構成します。

1. [ストア マネージャー コント ローラーとそのインデックス ビューを作成します。](#Exercise1)
2. [HTML ヘルパーを追加します。](#Exercise2)
3. [編集ビューを作成します。](#Exercise3)
4. [作成ビューの追加](#Exercise4)
5. [削除の処理](#Exercise5)
6. [検証の追加](#Exercise6)
7. [クライアント側での控えめな jQuery の使用](#Exercise7)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **60 分**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>手順 1: ストア マネージャー コント ローラーとそのインデックス ビューを作成します。

この演習では、CRUD 操作をサポートするには、データベースおよび最後に、インデックス ビュー テンプレートを活用 ASP.NET MVC のスキャフォールディングを生成からアルバムのリストを返すため、インデックス アクション メソッドをカスタマイズする新しいコント ローラーを作成する方法について説明しますHTML テーブルのアルバムのプロパティを表示する機能。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>タスク 1 -、StoreManagerController の作成

このタスクと呼ばれる新しいコント ローラーを作成する**StoreManagerController** CRUD 操作をサポートします。

1. 開く、**開始**ソリューションがある**ソース/Ex1-CreatingTheStoreManagerController/開始/** フォルダー。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 新しいコント ローラーを追加します。 これを行うを右クリックし、**コント ローラー**フォルダー内、ソリューション エクスプ ローラー、選択**追加**をクリックし、**コント ローラー**コマンド。 変更、**コント ローラー** **名前**に**StoreManagerController**オプションを確認して**の空の読み取り/書き込みアクションがあるMVCコントローラー**が選択されています。 **[追加]** をクリックします。

    ![コント ローラーの追加ダイアログ](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "コント ローラーの追加ダイアログ")

    *[追加] ダイアログのコント ローラー*

    新しいコント ローラー クラスが生成されます。 読み取り/書き込み、それらをスタブ メソッドのアクションを追加する指定したため、一般的な CRUD 操作は、アプリケーション固有のロジックを含めるに確認入力されている、TODO コメントで作成されます。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>タスク 2 - StoreManager インデックスをカスタマイズします。

このタスクをデータベースからアルバムの一覧では、ビューを返す StoreManager インデックス アクション メソッドをカスタマイズします。

1. 次のコードを追加、StoreManagerController クラスで*を使用して*ディレクティブ。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証 - Ex1 MvcMusicStore を使用して*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. フィールドを追加、 **StoreManagerController**のインスタンスを保持する**MusicStoreEntities します。**

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証 - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. アルバムのリストでビューを返す StoreManagerController Index アクションを実装します。

    コント ローラーのアクション ロジックは、StoreController の Index アクションの前に記述された非常にようになります。 LINQ を使用すると、すべてのアルバム、ジャンル、アーティストの情報の表示などを取得できます。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証 - Ex1 StoreManagerController インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>タスク 3 - インデックス ビューを作成します。

によって返されるアルバムの一覧を表示する Index ビュー テンプレートを作成するこのタスクで、 **StoreManager**コント ローラー。

1. 新しいテンプレートの表示を作成する前にプロジェクトをビルドする必要がありますように、**ビューの追加 ダイアログ**認識、**アルバム**クラスを使用します。 選択**ビルド |ビルド MvcMusicStore**プロジェクトをビルドします。
2. 内側を右クリックし、**インデックス**アクション メソッドと select**ビューの追加**します。 これが表示されます、**ビューの追加**ダイアログ。

    ![ビューの追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ビューの追加")

    *Index メソッド内からビューを追加します。*
3. ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**インデックス**します。 選択、**厳密に型指定されたビューを作成する**オプション、および選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンします。 選択**一覧**から、**スキャフォールディング テンプレート**ドロップダウンします。 ままに、**ビュー エンジン**に**Razor** 、既定値は、その他のフィールドの値をクリックして**追加**します。

    ![インデックス、ビューの追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "インデックス ビューの追加")

    *インデックス、ビューの追加*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>タスク 4 - Index ビューのスキャフォールディングをカスタマイズします。

このタスクでは、ASP.NET MVC のスキャフォールディング機能を表示するフィールドで作成された単純なビュー テンプレートを調整します。

> [!NOTE]
> **スキャフォールディング**ASP.NET MVC でのサポートは、アルバムのモデルのすべてのフィールドの一覧を表示する単純なビュー テンプレートを生成します。 **スキャフォールディング**厳密に型指定されたビューで開始する簡単な方法を提供します。 ビュー テンプレートを手動で記述するのではなくすばやくスキャフォールディング既定のテンプレートを生成し、し、生成されたコードを変更することができます。


1. 作成したコードを確認します。 生成されたフィールドの一覧を次の一部となる HTML テーブルを**スキャフォールディング**が表形式のデータを表示するために使用しています。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. 置換、 **&lt;テーブル&gt;** のみを表示する次のコードを使用したコード、**ジャンル**、**アーティスト**、 **アルバムのタイトル**、および**価格**フィールド。 これにより、削除、 **AlbumId**と**アルバム アート URL**列。 また、そのリンクされたクラス プロパティを表示する GenreId、ArtistId の列を変更**Artist.Name**と**Genre.Name**、し、削除、**詳細**リンク。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. 次の説明を変更します。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションの実行

このタスクではテストを**StoreManager** **インデックス**ビュー テンプレートには、前の手順のデザインに従ってアルバムの一覧が表示されます。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager**アルバムの一覧が表示されますかを示すことを確認する、**タイトル**、**アーティスト**と**ジャンル**します。

    ![アルバムの一覧を参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "アルバムの一覧を参照")

    *アルバムの一覧を参照*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>手順 2: HTML ヘルパーを追加します。

StoreManager インデックス ページが 1 つの潜在的な問題: タイトル、アーティスト名のプロパティ両方できる、テーブルを書式設定をスローするのに十分な長さ。 この演習では、そのテキストを切り捨てるカスタム HTML ヘルパーを追加する方法を学びます。

次の図では、小さなブラウザーのサイズを使用するときに、テキストの長さのために変更が形式の方法がわかります。

![切り捨てられたテキストとアルバムの一覧を参照しない](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "切り捨てられたテキストとアルバムの一覧を参照できません")

*切り捨てられたテキストとアルバムの一覧を参照できません。*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>タスク 1 - HTML ヘルパーを拡張します。

このタスクでは、新しいメソッドを追加する**Truncate**を**HTML** ASP.NET MVC ビュー内で公開されているオブジェクト。 これを行うには、実装、**拡張メソッド**組み込み**System.Web.Mvc.HtmlHelper** ASP.NET MVC によって提供されるクラス。

> [!NOTE]
> 詳細について**拡張メソッド**、こちらの msdn 記事をご覧ください。 [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. 開く、**開始**ソリューションがある**ソース/Ex2-AddingAnHTMLHelper/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. StoreManager のインデックス ビューを開きます。 これを行うには、ソリューション エクスプ ローラーで展開、**ビュー**フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。
3. 下の次のコードを追加、 <strong>@model</strong>を定義するディレクティブ、 <strong>Truncate</strong>ヘルパー メソッド。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>タスク 2 - ページのテキストの切り捨て

このタスクでは、使用、 **Truncate**メソッドをテンプレートの表示のテキストを切り捨てます。

1. StoreManager のインデックス ビューを開きます。 これを行うには、ソリューション エクスプ ローラーで展開、**ビュー**フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。
2. 表示する行を置き換える、**アーティスト名**とアルバムの**タイトル**します。 これを行うには、次の行を置き換えます。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションの実行

このタスクではテストを**StoreManager** **インデックス**アルバムのタイトル、アーティスト名、テンプレートの表示が切り捨てられます。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager**ことを確認するテキストで、**タイトル**と**アーティスト**列は切り捨てられます。

    ![切り捨てのタイトルやアーティスト名](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "切り捨てタイトルやアーティスト名")

    *切り捨てられたタイトル、アーティスト名*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>手順 3: 編集ビューを作成します。

この演習では、ストア管理者は、アルバムの編集を許可するフォームを作成する方法を学習します。 参照する、 **/StoreManager/Edit/id** URL (**id**を編集するアルバムの一意の id)、サーバーへの HTTP GET 呼び出しになります。

コント ローラーの Edit アクション メソッドは、データベースから適切なアルバムを取得、作成、 **StoreManagerViewModel** (およびアーティスト、ジャンルのリスト) をカプセル化をにビュー テンプレートに渡すオブジェクトユーザーには、HTML ページをレンダリングします。 このページが表示されます、 **&lt;フォーム&gt;** テキスト ボックスとアルバムのプロパティを編集するためのドロップダウン リストを持つ要素。

ユーザーは、アルバムのフォーム値を更新しをクリックすると、**保存** ボタンを使用して、変更を送信、HTTP POST にコールバックする **/StoreManager/Edit/id**します。URL は、最後の呼び出しの場合と同様、ASP.NET MVC を識別するそのため、別の Edit アクション メソッドを実行し、HTTP POST は、この時間 (で修飾された 1 つ **[HttpPost]**)。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>タスク 1 - HTTP GET の Edit アクション メソッドを実装します。

このタスクでは、データベースから適切なアルバムを取得する Edit アクション メソッドの HTTP GET のバージョンとのすべてのジャンル、アーティスト一覧を実装します。 このデータをパッケージ化、 **StoreManagerViewModel**で応答をレンダリングするビュー テンプレートに渡されるし、最後の手順で定義されているオブジェクト。

1. 開く、**開始**ソリューションがある**ソース/Ex3-CreatingTheEditView/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、 **StoreManagerController**クラス。 これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。
3. 置換、 **HTTP-GET 編集**アクション メソッドを次のコードを適切な取得**アルバム**だけでなく**ジャンル**と**アーティスト**を一覧表示します。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証のアクションの Ex3 StoreManagerController HTTP-GET 編集*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > 使用している**System.Web.Mvc** **SelectList**のアーティスト、ジャンルの代わりに、 **System.Collections.Generic**一覧。
    > 
    > **SelectList**クリーナー HTML ドロップダウン リストからの入力および現在の選択などを管理する方法です。 インスタンス化し、コント ローラー アクションでこれらの ViewModel オブジェクトを後で設定すると、編集フォーム シナリオ クリーナーします。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>タスク 2 - 編集ビューを作成します。

このタスクでは、後で、アルバムのプロパティを表示する編集ビュー テンプレートを作成します。

1. 編集ビューを作成します。 これを行うには、内を右クリックして、**編集**アクション メソッドと select**ビューの追加**します。
2. ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**編集**します。 チェック、**厳密に型指定されたビューを作成**チェック ボックスを選択し、**アルバム (MvcMusicStore.Models)** から、**データ クラスを表示**ドロップダウンします。 選択**編集**から、**スキャフォールディング テンプレート**ドロップダウンします。 その他のフィールド値が既定値のままにし、**追加**します。

    ![編集ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "編集ビューを追加します。")

    *編集ビューを追加します。*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションの実行

このタスクではテストを**StoreManager** **編集**ビュー ページには、パラメーターとして渡されるアルバムのプロパティの値が表示されます。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager/Edit/1**を渡されたアルバムのプロパティの値が表示されることを確認します。

    ![アルバムの編集ビューを閲覧](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "アルバムの編集ビューの参照")

    *アルバムの編集ビューの参照*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>タスク 4 - アルバム エディター テンプレートをドロップダウン リストを実装します。

このタスクで、ユーザーは、アーティスト、ジャンルの一覧から選択できるように、前回の作業で作成したビュー テンプレートをドロップダウン リストを追加します。

1. [すべて置換]、**アルバム**コードを次のフィールド セット。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList**アーティスト、ジャンルを選択するためのドロップダウン リストを表示するためにヘルパーが追加されました。 渡されるパラメーター **Html.DropDownList**は。
    > 
    > 1. フォーム フィールドの名前 (**&quot;ArtistId&quot;**)。
    > 2. **SelectList**のドロップダウン リストの値。

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションの実行

このタスクではテストを**StoreManager** **編集**ビュー ページには、アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されます。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager/Edit/1**アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されることを確認します。

    ![アルバムのドロップダウン リストでビューの編集をブラウズ](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "参照アルバムのドロップダウン リストでビューの編集")

    *アルバムの編集ビューのこの時点で、ドロップダウン リストからの参照*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>タスク 6 - HTTP POST の Edit アクション メソッドを実装します。

ビューの編集は、期待どおりに表示されます、アルバムに加えた変更を保存する HTTP POST アクションの編集メソッドを実装する必要があります。

1. Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。 開いている**StoreManagerController**から、**コント ローラー**フォルダー。
2. 置換**HTTP POST 編集**を次のアクション メソッドのコード (2 つのパラメーターを受け取るオーバー ロードされたバージョンに置き換える必要があるメソッドに注意してください)。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォームの検証 - アクションの Ex3 StoreManagerController HTTP POST の編集および*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > ユーザーがクリックしたときに、このメソッドが実行される、**保存**ビューのボタンをクリックし、データベースに保持へのサーバーにフォームの値の HTTP の POST を実行します。 デコレーター **[HttpPost]** メソッドが HTTP POST のこれらのシナリオに使用することを示します。 メソッドには、**アルバム**オブジェクト。 ASP.NET MVC が、ポストされたからアルバム オブジェクトを自動的に作成します。&lt;フォーム&gt;値。
    > 
    > メソッドは、これらの手順が実行されます。
    > 
    > 1. モデルが有効な場合。
    > 
    >     1. 変更されたオブジェクトをマークするコンテキストでアルバム エントリを更新します。
    >     2. 変更を保存し、インデックス ビューにリダイレクトします。
    > 2. ViewBag を設定は、モデルが有効でない場合、 **GenreId**と**ArtistId**、ユーザーが受信したアルバム オブジェクトにビューが返されますし、任意の必要な更新プログラムを実行します。

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>タスク 7 のアプリケーションを実行します。

このタスクではテストを**StoreManager 編集**ビュー ページでは、データベースで、アルバムの更新されたデータを実際に保存します。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager/Edit/1**します。 アルバム タイトルに変更**ロード** をクリック**保存**します。 アルバムのタイトルがアルバムの一覧で、実際に変更されたことを確認します。

    ![アルバムを更新](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "アルバムを更新しています")

    *アルバムを更新しています*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>手順 4: 追加のビューを作成します。

これで、 **StoreManagerController**をサポートしています、**編集**格納できるようにするビューの作成テンプレートを追加する方法について説明しますが、この演習では、機能マネージャーは、アプリケーションに新しいアルバムを追加します。

内の 2 つの個別のメソッドを使用して作成シナリオを実装は編集機能と同様、 **StoreManagerController**クラス。

1. 1 つのアクション メソッドは、ストア マネージャーは初回アクセス時に空のフォームに表示されます、 **/StoreManager/作成**URL。
2. 2 つ目のアクション メソッドは、シナリオを処理するストア マネージャーがクリックした、**保存**フォーム内でボタンをクリックし、値を送信する、 **/StoreManager/作成**として、HTTP POST の URL。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>タスク 1 - アクションの作成の HTTP GET メソッドを実装します。

すべてのジャンル、アーティストの一覧を取得するにこのデータをパッケージ化、Create アクション メソッドの HTTP GET のバージョンを実装するこのタスクで、 **StoreManagerViewModel**テンプレートの表示にし、渡されるオブジェクト。

1. 開く、**開始**ソリューションがある**ソース/Ex4-AddingACreateView/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開いている**StoreManagerController**クラス。 これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。
3. 置換、**作成**を次のアクション メソッドのコード。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex4 StoreManagerController HTTP GET の作成操作の検証 -*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>タスク 2 - 作成ビューの追加

このタスクでは、新しい (空) のアルバムのフォームを表示するビューの作成テンプレートを追加します。

1. 内側を右クリックし、**作成**アクション メソッドと select**ビューの追加**します。 ビューの追加 ダイアログが表示されます。
2. ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**作成**です。 選択、**厳密に型指定されたビューを作成する**オプションし、選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンと**を作成します。** から、**スキャフォールディング テンプレート**ドロップダウンします。 その他のフィールド値が既定値のままにし、**追加**します。

    ![作成ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "の追加-、-作成-view.png")

    *作成ビューの追加*
3. 更新プログラム、 **GenreId**と**ArtistId**次に示すように、ドロップダウン リストを使用するフィールド。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションの実行

このタスクではテストを**StoreManager** **作成**ビュー ページには、アルバムの空のフォームが表示されます。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して**StoreManager/作成**です。 新しいアルバム プロパティを入力するための空のフォームが表示されることを確認します。

    ![空のフォームでビュー作成](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "の空のフォームとビューの作成")

    *空のフォームとビューを作成します。*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>タスク 4 - HTTP POST を実装するアクション メソッドを作成します。

このタスクでは、ユーザーがクリックしたときに呼び出される Create アクション メソッドの HTTP POST のバージョンを実装します、**保存**ボタンをクリックします。 メソッドは、データベースに新しいアルバムを保存する必要があります。

1. Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。 開いている**StoreManagerController**クラス。 これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。
2. 置換**HTTP POST 作成**を次のアクション メソッドのコード。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex4 StoreManagerController HTTP-投稿を作成するアクションの検証 -*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > 作成アクションは、前の Edit アクション メソッドとよく似ていますが、変更されたオブジェクトを設定する代わりに追加されているコンテキストにします。

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションの実行

このタスクでは、テストを**StoreManager 作成**ビュー ページが新しいアルバムを作成することができ、StoreManager インデックス ビューにリダイレクトします。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して**StoreManager/作成**です。 次の図のように、新しいアルバムのデータをすべてのフォーム フィールドを入力します。

    ![アルバムを作成する](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "アルバムを作成します。")

    *アルバムを作成します。*
3. 先ほど作成した新しいアルバムを含む StoreManager インデックス ビューにリダイレクトすることを確認します。

    ![作成された新しいアルバム](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新しいアルバムの作成")

    *新しいアルバムの作成*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>手順 5: 削除を処理します。

アルバムを削除する機能はまだ実装されていません。 これは、この演習の詳細になります。 内の 2 つの個別のメソッドを使用して、削除のシナリオを実装する前に、ように、 **StoreManagerController**クラス。

1. 1 つのアクション メソッドは確認フォームを表示します。
2. 2 つ目のアクション メソッドは、フォームの送信を処理します。

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>タスク 1 - Delete アクション メソッドを HTTP GET を実装します。

このタスクでは、HTTP GET のバージョンのアルバムの情報を取得する Delete アクション メソッドを実装します。

1. 開く、**開始**ソリューションがある**ソース/Ex5-HandlingDeletion/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開いている**StoreManagerController**クラス。 これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。
3. 削除のコント ローラー アクションは、前のストアの詳細コント ローラー アクションと同じで、まさに: クエリ、**アルバム**オブジェクトを使用して、データベースから、 **id** URL を返しますで提供される、適切な**ビュー**します。 これを行うには、HTTP GET を置き換える**削除**を次のアクション メソッドのコード。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex5 処理削除 HTTP-GET 削除アクションの検証 -*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. 内側を右クリックし、**削除**アクション メソッドと select**ビューの追加**します。 ビューの追加 ダイアログが表示されます。
5. ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**削除**します。 選択、**厳密に型指定されたビューを作成する**オプションし、選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンします。 選択**削除**から、**スキャフォールディング テンプレート**ドロップダウンします。 その他のフィールド値が既定値のままにし、**追加**します。

    ![削除ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Delete ビューの追加")

    *削除ビューの追加*
6. Delete テンプレートには、モデルからのすべてのフィールドが表示されます。 アルバムのタイトルのみが表示されます。 これを行うには、ビューの内容を次のコードに置き換えます。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションの実行

このタスクではテストを**StoreManager** **削除**ビュー ページ確認削除フォームを表示します。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager**します。 1 つのアルバムをクリックして、削除を選択します。**削除**し、新しいビューがアップロードされたことを確認します。

    ![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "アルバムを削除します。")

    *アルバムを削除します。*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>タスク 3 - Delete アクション メソッドを HTTP POST を実装します。

このタスクでは、ユーザーがクリックしたときに呼び出される Delete アクション メソッドの HTTP POST のバージョンを実装します、**削除**ボタンをクリックします。 メソッドは、データベースのアルバムを削除する必要があります。

1. Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。 開いている**StoreManagerController**クラス。 これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。
2. 置換**HTTP POST Delete**を次のアクション メソッドのコード。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex5 処理削除 HTTP POST 削除アクションの検証 -*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションの実行

このタスクでは、テストを**StoreManager Delete**ビュー ページ アルバムを削除することができ、StoreManager インデックス ビューにリダイレクトします。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/StoreManager**します。 1 つのアルバムをクリックして、削除を選択します。**を削除します。** クリックして、削除を確定します**削除**ボタンをクリックします。

    ![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "アルバムを削除します。")

    *アルバムを削除します。*
3. 表示されないので、アルバムが削除されたことを確認、**インデックス**ページ。

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>手順 6: 検証の追加

現時点では、場所にあるを作成および編集フォームには、任意の種類の検証は行いません。 ユーザーの価格のフィールドに必要なフィールドを空白または型の文字が離れた場合、最初のエラーが表示されますが、データベースからになります。

アプリケーションに検証を追加するには、データの注釈モデル クラスを追加します。 データ注釈は、モデルのプロパティに適用するルールを記述するを許可して、ASP.NET MVC が適用して、ユーザーに適切なメッセージを表示するが取得されます。

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>タスク 1 - データ注釈の追加

このタスクでは、Create と Edit ページと、アルバムのモデルにデータ注釈は、該当する場合に、検証メッセージを表示を追加します。

単純なモデル クラスをだけを追加して処理されますデータ注釈を追加する、**を使用して**ステートメントの**System.ComponentModel.DataAnnotation**、配置、 **[必須]** 適切なプロパティの属性。 次の例のように、**名前**プロパティ ビューでの必須フィールドです。

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

エンティティ データ モデルが生成されますこのアプリケーションのような場合、少し複雑です。 モデル クラスに直接データ注釈を追加した場合、データベースからモデルを更新する場合、上書きが。 代わりに、行うことができますが、注釈を保持するために存在し、モデルに関連付けられているメタデータ部分クラスを使用してクラスを使用して、 **[MetadataType]** 属性。

1. 開く、**開始**ソリューションがある**ソース/Ex6-AddingValidation/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、 **Album.cs**から、**モデル**フォルダー。
3. 置換**Album.cs**次のように見えるように、強調表示されたコードを使用するコンテンツします。

    > [!NOTE]
    > 行 **[DisplayFormat(ConvertEmptyStringToNull=false)]** データ ソースのデータ フィールドが更新されたときに、モデルから空の文字列を null に変換されないことを示します。 Entity Framework は、データ注釈は、フィールドを検証する前にモデルに null 値を割り当てるときに、この設定は例外を回避します。

    (コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォームの検証 - Ex6 アルバム メタデータ部分クラスおよび*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > これは、**アルバム**部分クラスには、 **MetadataType**属性を指す、 **AlbumMetaData**データ注釈クラス。 これらは、アルバムのモデルの注釈を使用しているデータ注釈属性の一部を示します。
    > 
    > - 必須 - プロパティが必須のフィールドであることを示します。
    > - DisplayName - は、フォーム フィールドと検証メッセージで使用するテキストを定義します。
    > - DisplayFormat には、データ フィールドの表示し、書式設定する方法を指定します。
    > - StringLength - は、文字列フィールドの最大長を定義します。
    > - 範囲 - 数値フィールドの最大値と最小値は、
    > - ScaffoldColumn - により、エディターのフォームのフィールドを非表示

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションの実行

このタスクではテスト Create と Edit ページが、フィールドを検証することで最後のタスクを選択して表示名を使用します。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して**StoreManager/作成**です。 表示名が部分クラスにあるものと一致することを確認 (など**アルバム アート URL**の代わりに**AlbumArtUrl**)
3. をクリックして**作成**フォームの入力なし。 対応する検証メッセージを取得することを確認します。

    ![[作成] ページでフィールドを検証](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "の作成 ページでフィールドの検証")

    *[作成] ページで検証済みのフィールド*
4. 発生する、同じことを確認することができます、**編集**ページ。 URL を変更して **/StoreManager/Edit/1**表示名が部分クラスにあるものと一致することを確認し、(など**アルバム アート URL**の代わりに**AlbumArtUrl**)。 空の**タイトル**と**価格**フィールドをクリックします**保存**します。 対応する検証メッセージを取得することを確認します。

    ![[編集] ページで検証済みのフィールド](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *[編集] ページで検証済みのフィールド*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>手順 7: クライアント側での控えめな jQuery の使用

この演習では、クライアント側での MVC 4 の控えめな jQuery 検証を有効にする方法を学びます。

> [!NOTE]
> 控えめな jQuery では、データ、ajax プレフィックス JavaScript を使用して、インライン クライアント スクリプトの出力をそのままの状態ではなく、サーバー上のアクション メソッドを呼び出します。


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>タスク 1 - jQuery Unobtrusive を有効にする前に、アプリケーションを実行しています。

このタスクで検証の両方のモデルを比較するために jQuery を含める前にアプリケーションを実行します。

1. 開く、**開始**ソリューションがある**ソース/Ex7-UnobtrusivejQueryValidation/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. **F5** キーを押してアプリケーションを実行します。
3. ホーム ページで、プロジェクトが開始されます。 参照 **/StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するためのフォームの入力なし。

    ![クライアント検証を無効になっている](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "クライアント検証を無効になっています")

    *クライアント検証を無効になっています*
4. ブラウザーで HTML のソース コードを開きます。

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>タスク 2 - 控えめなクライアント検証を有効にします。

このタスクでは、jQuery を有効に**控えめなクライアント検証**から**Web.config**ファイル。 これは既定ではすべての新しい ASP.NET MVC 4 プロジェクトで false に設定します。 さらに、必要なスクリプト jQuery Unobtrusive Validation にクライアントの作業をするため、参照を追加します。

1. 開いている**Web.Config** 、プロジェクトのルートにファイルを確認して、 **ClientValidationEnabled**と**UnobtrusiveJavaScriptEnabled** にキー値が設定されて**true**します。

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > クライアント検証を有効に同じ結果を得るための Global.asax.cs でコードによってもできます。
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > さらに、カスタム動作を任意のコント ローラーに ClientValidationEnabled 属性を割り当てることができます。
2. 開いている**Create.cshtml**で**Views\StoreManager**します。
3. 次のスクリプト ファイルを必ず**jquery.validate**と**jquery.validate.unobtrusive**、システム ビューのレジストリ設定で参照される、 &quot; **~/bundles/jqueryval**&quot;バンドルします。

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > MVC 4 の新しいプロジェクトでは、これらすべての jQuery ライブラリが含まれます。 より多くのライブラリを見つけることができます、 **スクリプト/** するプロジェクトのフォルダー。
    > 
    > この検証を行うためにライブラリの機能は、jQuery ライブラリへの参照を追加する必要があります。 この参照が既に追加されているため、  **\_Layout.cshtml**ファイル必要はありません、このビューで追加します。

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>タスク 3 - アプリケーションを使用した控えめな jQuery 検証を実行しています。

このタスクではテストを**StoreManager**テンプレートは、ユーザーが新しいアルバムを作成するときに、jQuery ライブラリを使用してクライアント側の検証を実行します。 ビューを作成します。

1. **F5** キーを押してアプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 参照 **/StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するためのフォームの入力なし。

    ![クライアント検証を有効になっている jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "有効になっている jQuery を使用したクライアントの検証")

    *有効になっている jQuery を使用したクライアントの検証*
3. ブラウザーで、作成ビューのソース コードを開きます。

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > 各クライアント検証規則の控えめな jQuery はデータを使用して属性を追加します-val -*rulename*=&quot;*メッセージ*&quot;します。 以下は、タグのリストをその Unobtrusive jQuery クライアント検証を実行する html 入力フィールドに挿入されます。
   > 
   > - データ値
   > - データ val-番号
   > - データ値の範囲
   > - データ val 範囲 min/データ val range max
   > - Val-必要なデータ
   > - データの長さ val
   > - データ値の長さ最大/データ val 長さ分
   > 
   > すべてのデータ値がモデルで塗りつぶされます**データ注釈**します。 次に、サーバー側で動作するすべてのロジックは、クライアント側で実行できます。 たとえば、Price 属性では、モデルに、次のデータ注釈がいます。
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > 控えめな jQuery を使用すると、生成されたコードは次のとおりです。
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボは、次の使用量、データベースに格納されているデータを変更するユーザーを有効にする方法について説明しました。

- コント ローラー アクションなどのインデックスを作成、編集、削除
- HTML テーブルのプロパティを表示するための ASP.NET MVC のスキャフォールディング機能
- ユーザーを向上させるためにカスタム HTML ヘルパーをエクスペリエンスします。
- HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッド
- Create と Edit などの同様のビュー テンプレートの共有エディター テンプレート
- ドロップダウン リスト フォームの要素
- モデル検証のためのデータ注釈
- JQuery Unobtrusive ライブラリを使用してクライアント側の検証

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express for Web のタイル*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>付録 b: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
