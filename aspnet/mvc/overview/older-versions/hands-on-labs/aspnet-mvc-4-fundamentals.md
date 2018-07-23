---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 の基礎 |Microsoft Docs
author: rick-anderson
description: このハンズオン ラボは、MVC (Model View Controller) ミュージック ストアが導入されています、ASP.NET の MV を使用する方法が順を追って説明されたチュートリアル アプリケーションに基づきます.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 2b3f8916bdca1df0dd2855f02ae46f5e5d13311a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819972"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 の基礎

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

このハンズオン ラボは、MVC (Model View Controller) ミュージック ストアが導入されています、ASP.NET MVC と Visual Studio を使用する方法が順を追って説明されたチュートリアル アプリケーションに基づいています。 ラボでは、わかりやすくするためを学習します。 これらのテクノロジを組み合わせて使用の電源がまだ。 単純なアプリケーションから開始し、完全に機能の ASP.NET MVC 4 Web アプリケーションを設定するまでにビルドします。

このラボでは、ASP.NET MVC 4 で動作します。

チュートリアルのアプリケーションの ASP.NET MVC 3 のバージョンを探索する場合でを見つける[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。

このハンズオン ラボでは、開発者に、HTML や JavaScript などの Web 開発テクノロジの経験があると仮定します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 の基礎](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)します。

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>ミュージック ストア アプリケーション

次の 3 つの主要な部分で構成されますが、この演習全体で構築される、ミュージック ストア web アプリケーション: ショッピング、チェック アウト、および管理します。 訪問者はジャンルでアルバムを参照、アルバムをカートに追加、その選択の確認、および最後にログインし、注文の完了をチェック アウトを続行することになります。 さらに、ストアの管理者が使用可能なアルバムとその主要なプロパティを管理できるようになります。

![ミュージック ストア画面](aspnet-mvc-4-fundamentals/_static/image1.png "ミュージック ストアの画面")

*ミュージック ストアの画面*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

ミュージック ストア アプリケーションを使用して構築されます**Model View Controller (MVC)**、3 つの主要なコンポーネントにアプリケーションを分離するアーキテクチャのパターン。

- **モデル**: モデル オブジェクトは、アプリケーションのドメイン ロジックを実装する部分。 多くの場合、モデル オブジェクトも取得し、モデルの状態をデータベースに格納します。
- **ビュー:** ビューは、アプリケーションのユーザー インターフェイス (UI) を表示するコンポーネント。 通常、この UI は、モデル データから作成されます。 例は、テキスト ボックスとアルバム オブジェクトの現在の状態に基づいてドロップダウン リストを表示するアルバムの編集ビューになります。
- **コント ローラー:** コント ローラーはコンポーネント ユーザーの操作を処理し、モデルを操作し、最終的には、UI をレンダリングするビューを選択します。 MVC アプリケーションでは、ビューは情報のみを表示し、コントローラーがユーザーの入力と操作を処理して応答します。

MVC パターンでは、これらの要素間の疎結合を提供しながら、アプリケーション (入力ロジック、ビジネス ロジック、および UI ロジック) のさまざまな側面を分離するアプリケーションを作成するのに役立ちます。 この分離を使用して、一度に実装の 1 つの側面に集中することができ、アプリケーションをビルドすると、複雑さを管理できます。 さらに、MVC パターンは、簡単にもアプリケーションを作成するためのテスト駆動開発 (TDD) の使用を促進しているアプリケーションをテストします。

**ASP.NET MVC**フレームワークでは、ASP.NET MVC ベースの Web アプリケーションを作成するため、ASP.NET Web フォーム パターンの代替を提供します。 **ASP.NET MVC**フレームワークは、軽量で高度にテスト可能なプレゼンテーション フレームワークです (Web フォーム ベースのアプリケーションと同様)、マスター ページやメンバーシップ ベースの既存の ASP.NET 機能と統合されてcore の .NET framework のすべての機能を取得するために認証します。 これは、既に慣れて ASP.NET Web フォームで既に使用しているすべてのライブラリがも ASP.NET MVC 4 で使用できるため、場合に便利です。

さらに、MVC アプリケーションの 3 つの主要なコンポーネント間の疎結合では、並行開発も促進されます。 たとえば、1 人の開発者は、ビューで作業できる 2 つ目の開発者、コント ローラー ロジックで作業でき、3 番目の開発者は、モデル内のビジネス ロジックに集中できます。

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- ミュージック ストア アプリケーション チュートリアルに基づいて最初から ASP.NET MVC アプリケーションを作成します。
- ホーム ページのサイトとその主な機能を参照するために Url を処理するコント ローラーを追加します。
- そのスタイルと共に表示される内容をカスタマイズするビューを追加します。
- 格納し、管理のデータとドメイン ロジックにモデル クラスを追加します。
- ビュー モデル パターンを使用して、テンプレートの表示をコント ローラー アクションからの情報を渡します
- インターネット アプリケーションの ASP.NET MVC 4 の新しいテンプレートを詳細します。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (読み取り[付録 A](#AppendixA)をインストールする方法について)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。 実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;します。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の演習では、このハンズオン ラボから成ります。

1. [手順 1: MusicStore ASP.NET MVC Web アプリケーション プロジェクトを作成します。](#Exercise1)
2. [手順 2: コント ローラーの作成](#Exercise2)
3. [手順 3: コント ローラーにパラメーターを渡す](#Exercise3)
4. [手順 4: ビューを作成します。](#Exercise4)
5. [手順 5: ビュー モデルを作成します。](#Exercise5)
6. [手順 6: ビュー内のパラメーターの使用](#Exercise6)
7. [手順 7: の簡単な ASP.NET MVC 4 の新しいテンプレート](#Exercise7)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **60 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>手順 1: MusicStore ASP.NET MVC Web アプリケーション プロジェクトを作成します。

この演習では、Visual Studio 2012 Express でのメイン フォルダーの組織と、Web、ASP.NET MVC アプリケーションを作成する方法を学びます。 さらに、新しいコント ローラーを追加し、アプリケーションのホーム ページに単純な文字列を表示する方法を学習します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>タスク 1 - ASP.NET MVC Web アプリケーション プロジェクトの作成

1. このタスクでは、Visual Studio の MVC テンプレートを使用して、空の ASP.NET MVC アプリケーション プロジェクトを作成します。 開始**VS Express for Web**します。
2. **[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。
3. **新しいプロジェクト**ダイアログ ボックスの 、 **ASP.NET MVC 4 Web アプリケーション**プロジェクトの種類の下にある**Visual c#、** **Web**テンプレートリスト。
4. 変更、**名前**に*MvcMusicStore*します。
5. 新しいソリューションの場所を設定**開始**例については、この演習のソース フォルダー内のフォルダー **[、ハンズオン ラボ PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**します。 **[OK]** をクリックします。

    ![新しいプロジェクト ダイアログ ボックスを作成する](aspnet-mvc-4-fundamentals/_static/image2.png "新しいプロジェクト ダイアログ ボックスの作成")

    *新しいプロジェクト ダイアログ ボックスを作成します。*
6. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**基本的な**テンプレートを確認して、**ビュー エンジン**が選択されている**Razor**します。 **[OK]** をクリックします。

    ![新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス](aspnet-mvc-4-fundamentals/_static/image3.png "新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス")

    *新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>タスク 2 - ソリューション構造を調べる

ASP.NET MVC フレームワークには、MVC パターンをサポートする Web アプリケーションを作成するのに役立つ、Visual Studio プロジェクト テンプレートが含まれています。 このテンプレートでは、必要なフォルダー、項目テンプレート、構成ファイルのエントリと、新しい ASP.NET MVC Web アプリケーションを作成します。

このタスクでは、関連する要素を理解するソリューションの構造とその関係を調べます。 既定では、ASP.NET MVC framework を使用するため、すべての ASP.NET MVC アプリケーションで、次のフォルダーが含まれる、&quot;設定より規約&quot;アプローチ、およびいくつかの既定の解釈がフォルダーの名前に基づいてにより規則。

1. プロジェクトが作成されると、右側にある ソリューション エクスプ ローラーで作成されたフォルダー構造を確認してください。

    ![ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造](aspnet-mvc-4-fundamentals/_static/image4.png "ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造")

    *ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造*

   1. **コント ローラー**します。 このフォルダーには、コント ローラー クラスが含まれます。 MVC ベースのアプリケーションでは、コント ローラーはエンドユーザーの対話を処理、モデルを操作および最終的には、UI をレンダリングするビューを選択する責任を負います。

       > [!NOTE]
       > 終わるすべてのコント ローラーの名前に、MVC フレームワークが必要です。&quot;コント ローラー&quot;-たとえば、の HomeController、LoginController、productcontroller します。
   2. **モデル**します。 MVC Web アプリケーションのアプリケーション モデルを表すクラスのこのフォルダーが提供されます。 通常、オブジェクトとデータ ストアと対話するためのロジックを定義するコードが含まれます。 通常、実際のモデル オブジェクトは、別のクラス ライブラリになります。 新しいアプリケーションを作成するときにクラスを含めるし、開発サイクルで、後で別のクラス ライブラリに移動可能性があります。
   3. **ビュー**します。 このフォルダーは、ビュー、アプリケーションのユーザー インターフェイスを表示するために行うそれぞれのコンポーネントの推奨される場所です。 ビューは、ビューのレンダリングに関連するその他のファイルだけでなく、.aspx、.ascx、.cshtml および .master ファイルを使用します。 Views フォルダーには、各コント ローラーのフォルダーが含まれています。フォルダーは、コント ローラー名のプレフィックスを持つという名前です。 たとえば、という名前のコント ローラーがある場合**HomeController**、Views フォルダーには、Home という名前のフォルダーにが含まれます。 既定では、ASP.NET MVC フレームワークでビューの読み込み時に見えます Views\controllerName フォルダーで要求されたビューの名前を持つ .aspx ファイルを (**ビュー [ControllerName] [アクション] .aspx**) または (**ビュー [ControllerName][アクション] .cshtml**) Razor ビュー。

      > [!NOTE]
      > 前の表に、フォルダーに加え、MVC Web アプリケーションを使用して、 **Global.asax**グローバル URL ルーティングを設定するファイルの既定値を使用して、 **Web.config**アプリケーションを構成するファイル。

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>タスク 3 - を HomeController の追加

MVC framework を使用して ASP.NET アプリケーションでは、ユーザーの操作は、ページの周囲および発生して、それらのページからのイベント処理の周囲に編成されます。 これに対し、による ASP.NET MVC アプリケーションのユーザー操作は、コント ローラーおよびアクション メソッドの周囲に編成されます。

その一方で、ASP.NET MVC フレームワークは、Url をコント ローラーとして参照されるクラスにマップします。 コント ローラーが受信要求を処理、ユーザーの入力との対話を処理、適切なアプリケーション ロジックを実行およびクライアントに返送する対応を判断する (HTML を表示、ファイルをダウンロードするなど、別の URL をリダイレクトします。)。 HTML を表示する場合、コント ローラー クラスは、通常、要求の HTML マークアップを生成する別のビュー コンポーネントを呼び出します。 MVC アプリケーションでは、ビューは情報のみを表示し、コントローラーがユーザーの入力と操作を処理して応答します。

このタスクでは、ミュージック ストアのサイトのホーム ページに Url を処理するコント ローラー クラスを追加します。

1. 右クリックして**コント ローラー**フォルダー内、ソリューション エクスプ ローラー、選択**追加**し**コント ローラー**コマンド。

    ![コント ローラーのコマンドを追加](aspnet-mvc-4-fundamentals/_static/image5.png "コント ローラーのコマンドを追加します。")

    *コント ローラーのコマンドを追加します。*
2. **コント ローラーの追加**ダイアログが表示されます。 コント ローラー *HomeController*キーを押します**追加**します。

    ![[追加] ダイアログのコント ローラー](aspnet-mvc-4-fundamentals/_static/image6.png "コント ローラーのダイアログの追加")

    *[追加] ダイアログのコント ローラー*
3. ファイル**HomeController.cs**で作成、**コント ローラー**フォルダー。 確保するために、 **HomeController** 、インデックス アクションの文字列を返す、置換、**インデックス**メソッドを次のコード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex1 HomeController の Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションの実行

このタスクでは、web ブラウザーでアプリケーションを試みます。

1. キーを押して**F5**アプリケーションを実行します。 プロジェクトがコンパイルされ、ローカル IIS Web サーバーを起動します。 ローカル IIS Web サーバーでは、Web サーバーの URL を指す web ブラウザーを開きは自動的にします。

    ![Web ブラウザーで実行されているアプリケーション](aspnet-mvc-4-fundamentals/_static/image7.png "web ブラウザーで実行されているアプリケーション")

    *Web ブラウザーで実行されているアプリケーション*

    > [!NOTE]
    > ローカル IIS Web サーバーは、ランダムなポートの番号で、web サイトが実行されます。 上記の図に、サイトが実行されている`http://localhost:50103/`50103 ポートを使用しているため、します。 実際のポート番号が異なる場合があります。
2. ブラウザーを閉じます。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>手順 2: コント ローラーの作成

この演習では、ミュージック ストア アプリケーションの単純な機能を実装するコント ローラーを更新する方法を学びます。 そのコント ローラーには、次の特定の要求の各に対応するアクション メソッドを定義します。

- ミュージック ストアで音楽のジャンルのリスト ページ
- すべての特定のジャンルの音楽のアルバムを一覧表示する [参照] ページ
- 特定の音楽のアルバムに関する情報を表示するための詳細ページ

この演習のスコープをそれらのアクションによって単純に文字列がここまでで返します。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>タスク 1 - 新しい StoreController クラスの追加

このタスクでは、新しいコント ローラーを追加します。

1. 既に開かれていない場合は開始**VS Express for Web 2012**します。
2. **ファイル**] メニューの [選択**プロジェクトを開く**します。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex02 CreatingAController\Begin**を選択します**Begin.sln**クリック**オープン**。 または、前の演習を完了後に取得したソリューションを引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. 新しいコント ローラーを追加します。 これを行うを右クリックし、**コント ローラー**フォルダー内、ソリューション エクスプ ローラー、選択**追加**をクリックし、**コント ローラー**コマンド。 変更、**コント ローラー名**に*StoreController*、 をクリック**追加**します。

    ![[追加] ダイアログのコント ローラー](aspnet-mvc-4-fundamentals/_static/image8.png "コント ローラーのダイアログの追加")

    *[追加] ダイアログのコント ローラー*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>タスク 2 - StoreController のアクションを変更します。

このタスクでは、呼び出されるコント ローラー メソッドを変更します**アクション**します。 アクションは、URL 要求を処理し、ブラウザーまたは URL を呼び出すユーザーに送信されるコンテンツを判断する責任を負います。

1. **StoreController**クラスには既に、**インデックス**メソッド。 このラボに後でミュージック ストアのすべてのジャンルを一覧表示されたページを実装するために使用するされます。 ここでは、置き換えるだけです、**インデックス**文字列を返す次のコードでメソッド&quot;Store.Index() からこんにちは&quot;:

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex2 StoreController インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. 追加**参照**と**詳細**メソッド。 これを行うには、次のコードを追加、 **StoreController**:

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションの実行

このタスクでは、web ブラウザーでアプリケーションを試みます。

1. キーを押して**F5**アプリケーションを実行します。
2. プロジェクトの開始、**ホーム**ページ。 各アクションの実装を検証する URL を変更します。

    1. **/Store**します。 表示されます **&quot;Store.Index() からこんにちは&quot;** します。
    2. **/ストア/参照**します。 表示されます **&quot;Store.Browse() からこんにちは&quot;** します。
    3. **/保存/詳細**します。 表示されます **&quot;Store.Details() からこんにちは&quot;** します。

        ![参照 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse の参照")

        *閲覧/Store/Browse*
3. ブラウザーを閉じます。

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>手順 3: コント ローラーにパラメーターを渡す

これまでは、コント ローラーから定数文字列を返していましたが。 この演習では、URL と、クエリ文字列を使用し、ブラウザーにテキストを含む応答メソッドのアクションのコント ローラーにパラメーターを渡す方法を学習します。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>タスク 1 - StoreController をジャンル パラメーターを追加します。

このタスクで使用する、 **querystring**へのパラメーターを送信する、**参照**内のアクション メソッド、 **StoreController**します。

1. 既に開かれていない場合は開始**VS Express for Web**します。
2. **ファイル**] メニューの [選択**プロジェクトを開く**します。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex03 PassingParametersToAController\Begin**を選択します**Begin.sln**クリック**オープン**。 または、前の演習を完了後に取得したソリューションを引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. 開いている**StoreController**クラス。 この場合に、**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリックします**StoreController.cs**。
4. 変更、**参照**メソッド、特定のジャンルを要求する文字列パラメーターを追加します。 ASP.NET MVC の任意のクエリ文字列を渡すまたはフォーム ポスト パラメーターという名前が自動的に**ジャンル**呼び出されたときにこのアクション メソッドにします。 これを行うには、置換、**参照**メソッドを次のコード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> 使用している、 **HttpUtility.HtmlEncode**するユーティリティ メソッドのようなリンクを含むビューに Javascript を挿入することからように  **/ストア/参照でしょうか。ジャンル =&lt;スクリプト&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** します。
> 
> 詳細については、次を参照してください[こちらの msdn 記事](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)します。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションの実行

このタスクでは、web ブラウザーでアプリケーションを試してみるし、使用して、、**ジャンル**パラメーター。

1. キーを押して**F5**アプリケーションを実行します。
2. プロジェクトの開始、**ホーム**ページ。 URL を変更して  */保存/を参照しますか?ジャンル Disco を =* アクションがジャンルのパラメーターを受け取ることを確認します。

    ![参照 StoreBrowseGenre Disco を =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre をブラウズ Disco を =")

    */Store/Browse を参照しますか。ジャンル Disco を =*
3. ブラウザーを閉じます。

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>タスク 3 - URL に埋め込まれた Id パラメーターを追加します。

このタスクでは、使用、 **URL**を渡す、 **Id**パラメーターを**詳細**のアクション メソッド、 **StoreController**します。 ASP.NET MVC の既定のルーティング規則がという名前のパラメーターとしてアクション メソッド名の後、URL のセグメントを処理するには**Id**します。アクション メソッドに Id をという名前のパラメーターがある場合は、し、ASP.NET MVC は自動的に URL セグメントにパラメーターとして渡します。 URL で**ストア/詳細/5**、 **Id**として解釈されます**5**します。

1. 変更、**詳細**のメソッド、 **StoreController**を追加する、 **int**という名前のパラメーター **id**します。これを行うには、置換、**詳細**メソッドを次のコード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションの実行

このタスクでは、web ブラウザーでアプリケーションを試してみるし、使用して、、 **Id**パラメーター。

1. キーを押して**F5**アプリケーションを実行します。
2. プロジェクトの開始、**ホーム**ページ。 URL を変更して */Store/Details/5*アクションが、id パラメーターを受け取ることを確認します。

    ![参照 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 の参照")

    *閲覧/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>手順 4: ビューを作成します。

これまでにするがされてを返す文字列コント ローラー アクションから。 コント ローラーのしくみを理解するのに便利ですが、実際の Web アプリケーションの構築方法いないをお勧めします。 ビューは、テンプレート ファイルを使用して、ブラウザーに HTML を生成するためのより優れたアプローチを提供するコンポーネントです。

この演習では、一般的な HTML コンテンツをサイトと、最後に HTML を返す HomeController を有効にするテンプレートの表示の外観を強化するために、スタイル シートのテンプレートをセットアップするマスター ページをレイアウトを追加する方法を学習します。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>タスク 1 - ファイルを変更する\_layout.cshtml

ファイル **~/Views/Shared/\_layout.cshtml** web サイト全体にわたって使用する共通の HTML のテンプレートをセットアップすることができます。 このタスクでは、ホーム ページとストアの領域へのリンクに共通のヘッダーとレイアウトのマスター ページを追加します。

1. 既に開かれていない場合は開始**VS Express for Web**します。
2. **ファイル**] メニューの [選択**プロジェクトを開く**します。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex04 CreatingAView\Begin**を選択します**Begin.sln**クリック**オープン**。 または、前の演習を完了後に取得したソリューションを引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. ファイル <strong>\_layout.cshtml</strong>サイト上のすべてのページの HTML コンテナー レイアウトが含まれています。 含まれています、 <strong>&lt;html&gt;</strong> 、HTML 応答の要素だけでなく<strong>&lt;ヘッド&gt;</strong>と<strong>&lt;本文&gt;</strong>要素。 <strong>@RenderBody()</strong> HTML 内で本文地域の特定のテンプレートが動的なコンテンツに入力できるビュー。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. サイトのすべてのページのホーム ページとストアの領域へのリンクに共通のヘッダーを追加します。 このためには、以下の次のコードを追加&lt;本文&gt;ステートメント。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. 各ページの本文のセクションを表示するために、div が含まれます。 置換 <strong>@RenderBody()</strong>を次の強調表示されているコード: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > ご存じですか。 Visual Studio 2012 は、スニペットを HTML、コード ファイルでよく使用されるコードを追加するが簡単にします。 」と入力して試して**&lt;div&gt;** キーを押すと**タブ**挿入を完了するには、2 回**div**タグ。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>タスク 2 - 追加の CSS スタイル シート

空のプロジェクト テンプレートには、基本的なフォームや検証メッセージを表示するために使用するスタイルが含まれている非常に簡素化された CSS ファイルが含まれています。 サイトの外観を強化するために、追加の CSS と (デザイナーによって提供される可能性がある) イメージを使用します。

このタスクでは、サイトのスタイルを定義する CSS スタイル シートを追加します。

1. CSS ファイルとイメージに含める、 **Source\Assets\Content**このラボのフォルダー。 それらをアプリケーションに追加するからコンテンツをドラッグして、 **Windows エクスプ ローラー**ウィンドウに、**ソリューション エクスプ ローラー** Visual Studio Express for Web には、次に示すように。

    ![スタイルの内容をドラッグ](aspnet-mvc-4-fundamentals/_static/image12.png "スタイルの内容をドラッグします。")

    *スタイルの内容をドラッグします。*
2. 警告ダイアログが表示されます、交換されることを確認するかを確認する**Site.css**ファイルと一部の既存のイメージ。 確認**すべての項目に適用する** をクリック**はい**します。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>タスク 3 - ビュー テンプレートを追加します。

このタスクでレイアウトのマスター ページを使用して HTML 応答を生成するビュー テンプレートを追加し、CSS は、この手順で追加します。

1. サイトのホーム ページを参照するときにビュー テンプレートを使用する必要があります、文字列を返す代わりに示すために、 **HomeController インデックス**メソッドは、**ビュー**します。 開いている**HomeController**クラスし、変更、**インデックス**を返すメソッドを**ActionResult**が返される**View()** します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex4 HomeController の Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. ここで、適切なテンプレートの表示を追加する必要があります。 これを行う**を右クリックして**内で、**インデックス**アクション メソッドと select**ビューの追加**。 これが表示されます、**ビューの追加**ダイアログ。

    ![Index メソッド内からビューを追加する](aspnet-mvc-4-fundamentals/_static/image13.png "Index メソッド内からビューを追加します。")

    *Index メソッド内からビューを追加します。*
3. **ビューの追加**ビュー テンプレート ファイルを生成するダイアログが表示されます。 既定では、このダイアログ ボックスでは、それを使用するアクション メソッドに一致するビュー テンプレートの名前が事前設定します。 使用しているため、**ビューの追加**内のコンテキスト メニュー、**インデックス**、HomeController 内のアクション メソッド、**ビューの追加**ダイアログに既定のビュー名としてインデックスがあります。 **[追加]** をクリックします。

    ![[追加] ダイアログの表示](aspnet-mvc-4-fundamentals/_static/image14.png "追加ビュー ダイアログ ボックス")

    *[追加] ダイアログの表示*
4. Visual Studio によって生成、 **Index.cshtml**内でテンプレートの表示、 **views \home**フォルダーし、それを開きます。

    ![作成されたインデックス ビューをホーム](aspnet-mvc-4-fundamentals/_static/image15.png "ホーム インデックス ビューの作成")

    *作成したホームのインデックス ビュー*

    > [!NOTE]
    > 名前と場所、 **Index.cshtml**ファイルが関連して、既定の ASP.NET MVC の名前付け規則に従います。
    > 
    > フォルダー \Views\**ホーム** コント ローラー名と一致する (**ホーム**コント ローラー)。 ビュー テンプレートの名前 (**インデックス**)、このビューを表示するコント ローラー アクション メソッドと一致します。
    > 
    > これにより、ASP.NET MVC は、この名前付け規則を使用して、ビューを返すときに、名前またはビュー テンプレートの場所を明示的に指定することを回避します。
5. 生成されたテンプレートの表示がに基づいて、  **\_layout.cshtml**以前に定義されているテンプレートです。 ViewBag.Title プロパティを更新**ホーム**、主な内容の変更と**これは、ホーム ページ**の次のコードに示しますように。

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. 選択**MvcMusicStore**キーを押して、ソリューション エクスプ ローラーでプロジェクト**F5**アプリケーションを実行します。

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>タスク 4: 検証

前の演習では、すべての手順を正しく実行したことを確認するには、次のように進んでください。

ブラウザーで開かれているアプリケーションを注意してください。

1. HomeController の Index アクション メソッドを見つけて表示、 **\Views\Home\Index.cshtml**テンプレートを表示、コードが呼び出されていなくて**View() を返す**ビュー テンプレートであるため、標準的な名前付け規則。
2. ホーム ページ内で定義されたようこそメッセージを表示、 **\Views\Home\Index.cshtml**テンプレートの表示。
3. ホーム ページを使用して、  **\_layout.cshtml**テンプレート、ようこそメッセージは、標準のサイトの HTML レイアウト内に含まれるためです。

    ![定義済みの LayoutPage とスタイルを使用してインデックス ビューをホーム](aspnet-mvc-4-fundamentals/_static/image16.png "定義 LayoutPage とスタイルを使用してインデックス ビュー ホーム")

    *定義済みの LayoutPage とスタイルを使用してホームのインデックス ビュー*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>手順 5: ビュー モデルを作成します。

ここまでは、ハードコーディングされた HTML を表示するビューを作成したが、動的な web アプリケーションを作成するには、テンプレートの表示は、コント ローラーから情報を受信する必要があります。 目的のために使用する 1 つの一般的な手法は、 **ViewModel**パターンで、適切な HTML 応答を生成するために必要なすべての情報をパッケージ化するコント ローラーを使用します。

この演習では、ViewModel クラスを作成し、必要なプロパティを追加する方法について説明します。 ストアとそのジャンルのリストでジャンルの数。 作成の ViewModel を使用する StoreController も更新し、ページで説明したようにプロパティを表示する新しいビュー テンプレートを作成する最後に、します。

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>タスク 1 - ViewModel クラスを作成します。

このタスクでは、ストアのジャンル一覧のシナリオを実装するビューモデル クラスを作成します。

1. 既に開かれていない場合は開始**VS Express for Web**します。
2. **ファイル**] メニューの [選択**プロジェクトを開く**します。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex05 CreatingAViewModel\Begin**を選択します**Begin.sln**クリック**オープン**。 または、前の演習を完了後に取得したソリューションを引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. 作成、 **ViewModels**ビューモデルを格納するフォルダー。 これを行うには、最上位レベルを右クリックし**MvcMusicStore**プロジェクトで、**追加**し**新しいフォルダー**します。

    ![新しいフォルダーを追加する](aspnet-mvc-4-fundamentals/_static/image17.png "新しいフォルダーを追加します。")

    *新しいフォルダーを追加します。*
4. フォルダーの名前*ViewModels*します。

    ![ソリューション エクスプ ローラー、ビューモデル フォルダー](aspnet-mvc-4-fundamentals/_static/image18.png "ソリューション エクスプ ローラー、ビューモデル フォルダー")

    *ソリューション エクスプ ローラー、ビューモデル フォルダー*
5. 作成、 **ViewModel**クラス。 これを行うを右クリックし、 **ViewModels**フォルダーが最近作成した、選択**追加**し**新しい項目の**します。 [**コード**、選択、**クラス**項目し、ファイルの名前*StoreIndexViewModel.cs*、] をクリックし、**追加**します。

    ![新しいクラスの追加](aspnet-mvc-4-fundamentals/_static/image19.png "新しいクラスの追加")

    *新しいクラスの追加*

    ![StoreIndexViewModel クラスを作成する](aspnet-mvc-4-fundamentals/_static/image20.png "作成 StoreIndexViewModel クラス")

    *StoreIndexViewModel クラスを作成します。*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>タスク 2 - ViewModel クラスにプロパティを追加します。

予期される HTML 応答を生成するためにビュー テンプレートに、StoreController から渡される 2 つのパラメーターがある: ストアに格納され、ジャンルの一覧のジャンルの数。

このタスクでは、これら 2 つのプロパティを追加、 **StoreIndexViewModel**クラス: **NumberOfGenres** (整数) と**ジャンル**(文字列のリスト)。

1. 追加**NumberOfGenres**と**ジャンル**プロパティを**StoreIndexViewModel**クラス。 これを行うには、クラス定義に次の 2 行を追加します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex5 StoreIndexViewModel プロパティ*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> **{Get; 設定}。** 表記法では、# の使用により、自動実装プロパティの機能です。 バッキング フィールドを宣言することを必要とせず、プロパティの利点を提供します。

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>タスク 3 - 更新 StoreController、StoreIndexViewModel を使用するには

**StoreIndexViewModel**クラスからを渡すために必要な情報をカプセル化**StoreController**の**インデックス**応答を生成するためにビュー テンプレートにメソッド.

このタスクでは更新して、 **StoreController**を使用する、 **StoreIndexViewModel**します。

1. 開いている**StoreController**クラス。

    ![StoreController クラスを開いて](aspnet-mvc-4-fundamentals/_static/image21.png "開く StoreController クラス")

    *開く StoreController クラス*
2. 使用するには、 **StoreIndexViewModel**クラスから、 **StoreController**、上部にある次の名前空間を追加、 **StoreController**コード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - ビューモデルを使用して Ex5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. 変更、 **StoreController**の**インデックス**アクション メソッドを作成および設定しますので、 **StoreIndexViewModel**オブジェクトし、をビュー テンプレートに渡すこれで、HTML 応答を生成します。

    > [!NOTE]
    > ラボで&quot;ASP.NET MVC のモデルとデータ アクセス&quot;ストア ジャンルのリストをデータベースから取得するコードを記述します。 次のコードでは、作成、**一覧**設定がダミー データ ジャンルの**StoreIndexViewModel**します。
    > 
    > 作成し、設定した後、 **StoreIndexViewModel**オブジェクトへの引数として渡されます、**ビュー**メソッド。 これは、ビュー テンプレートがそのオブジェクトを使用して、HTML 応答を生成することを示します。
4. 置換、**インデックス**メソッドを次のコード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex5 StoreController Index メソッド*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> 使用するとする可能性があります (C#) を使い慣れていない場合は、 **var** 。 つまり、 **viewModel**変数は遅延バインディング。 C# コンパイラが判断するために変数に代入するに基づいて型推論を使用して、正しくないを**viewModel**の種類は**StoreIndexViewModel**します。 ローカルをコンパイルしても、 **viewModel**変数として、 **StoreIndexViewModel** get コンパイル時チェックと Visual Studio コード エディターでサポートする型します。

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>タスク 4 - StoreIndexViewModel を使用するビュー テンプレートを作成します。

この作業はジャンルの一覧を表示するコント ローラーから渡された StoreIndexViewModel オブジェクトを使用するテンプレートの表示を作成します。

1. 新しいテンプレートの表示を作成する前に、プロジェクトをビルドしてみましょうように、**ビューの追加 ダイアログ**認識、 **StoreIndexViewModel**クラス。 選択してプロジェクトをビルド、**ビルド**メニュー項目をクリックし**ビルド MvcMusicStore**します。

    ![プロジェクトのビルド](aspnet-mvc-4-fundamentals/_static/image22.png "プロジェクトのビルド")

    *プロジェクトのビルド*
2. 新しいテンプレートの表示を作成します。 内部を右クリックするには、**インデックス**メソッドと select**ビューの追加**します。

    ![ビューの追加](aspnet-mvc-4-fundamentals/_static/image23.png "ビューの追加")

    *ビューの追加*
3. **ビューの追加 ダイアログ**から呼び出された、 **StoreController**、ビュー テンプレートに既定で追加されます、 **\Views\Store\Index.cshtml**ファイル。 チェック、**を厳密に型指定された、ビューを作成する**チェック ボックスをオンし、 **StoreIndexViewModel**として、**モデル クラス**します。 また、選択されているビュー エンジンがであることを確認**Razor**します。 **[追加]** をクリックします。

    ![[追加] ダイアログの表示](aspnet-mvc-4-fundamentals/_static/image24.png "追加ビュー ダイアログ ボックス")

    *[追加] ダイアログの表示*

    **\Views\Store\Index.cshtml**ビュー テンプレート ファイルが作成され開かれます。 提供された情報に基づいて、**ビューの追加**最後の手順では、テンプレートに期待されるビューでダイアログを**StoreIndexViewModel**インスタンスとして使用して、HTML 応答を生成するデータ。 テンプレートを継承することがわかります、 `ViewPage<musicstore.viewmodels.storeindexviewmodel>` (C#)。

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>タスク 5 - テンプレートの表示を更新しています

このタスクで [ジャンル]、およびページ内には、その名前の数を取得する最後のタスクで作成したテンプレートの表示が更新されます。

> [!NOTE]
> @ 構文に使用されます (呼ば&quot;コード ナゲット&quot;) ビュー テンプレート内のコードを実行します。

1. **Index.cshtml**ファイル内で、**ストア**フォルダーを次のコードに置き換えます。

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. ジャンルのリストをループ**StoreIndexViewModel** HTML を作成および**&lt;ul&gt;** を使用して、 **foreach**ループします。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. キーを押して**F5**アプリケーションを実行し、[参照] を **/格納**します。 渡されたジャンルの一覧が表示されます、 **StoreIndexViewModel**オブジェクトから、 **StoreController**テンプレートの表示にします。

    ![ジャンルの一覧を表示するビュー](aspnet-mvc-4-fundamentals/_static/image26.png "ジャンルの一覧を表示するビュー")

    *ジャンルの一覧を表示するビュー*
4. ブラウザーを閉じます。

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>手順 6: ビュー内のパラメーターの使用

手順 3 では、コント ローラーにパラメーターを渡す方法について説明しました。 この演習では、ビュー テンプレートでこれらのパラメーターを使用する方法を学びます。 そのため、紹介最初に、データとドメイン ロジックを管理する際に役立つモデル クラス。 さらに、エンコードする URL パスなどの心配することがなく、ASP.NET MVC アプリケーション内のページへのリンクを作成する方法を学習します。

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>タスク 1 - Model クラスの追加

コント ローラーからビューに情報を渡すためだけに作成される、ビューモデルとは異なり、モデル クラスは、格納し、管理のデータとドメイン ロジックに構築されます。 このタスクでは、これらの概念を表す 2 つのモデル クラスを追加します:**ジャンル**と**アルバム**します。

1. 既に開かれていない場合は開始**VS Express for Web**
2. **ファイル**] メニューの [選択**プロジェクトを開く**します。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex06 UsingParametersInView\Begin**を選択します**Begin.sln**クリック**オープン**。 または、前の演習を完了後に取得したソリューションを引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. 追加、**ジャンル**モデル クラス。 これを行うを右クリックし、**モデル**フォルダーで、**ソリューション エクスプ ローラー**を選択します**追加**をクリックし、**新しい項目の**オプション。 [**コード**、選択、**クラス**項目し、ファイルの名前*Genre.cs*、] をクリックし、**追加**します。

    ![クラスの追加](aspnet-mvc-4-fundamentals/_static/image27.png "クラスの追加")

    *新しい項目の追加*

    ![ジャンルのモデル クラスを追加](aspnet-mvc-4-fundamentals/_static/image28.png "ジャンル モデル クラスを追加")

    *ジャンルのモデル クラスを追加します。*
4. 追加、**名前**プロパティをジャンル クラスにします。 これを行うには、次のコードを追加します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 ジャンル*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. 追加前と同じ手順に従って、**アルバム**クラス。 これを行うを右クリックし、**モデル**フォルダーで、**ソリューション エクスプ ローラー**を選択します**追加**をクリックし、**新しい項目の**オプション。 [**コード**、選択、**クラス**項目し、ファイルの名前*Album.cs*、] をクリックし、**追加**します。
6. アルバム クラスに 2 つのプロパティを追加:**ジャンル**と**タイトル**します。 これを行うには、次のコードを追加します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 アルバム*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>タスク 2 -、StoreBrowseViewModel を追加します。

A **StoreBrowseViewModel**が選択されているジャンルと一致するアルバムを表示するこのタスクで使用します。 このタスクでは、このクラスを作成し、処理するために 2 つのプロパティを追加し、**ジャンル**とその**アルバム**のリストします。

1. 追加、 **StoreBrowseViewModel**クラス。 これを行うを右クリックし、 **ViewModels**フォルダーで、**ソリューション エクスプ ローラー**を選択します**追加**をクリックし、**新しい項目の**オプション。 [**コード**、選択、**クラス**項目し、ファイルの名前*StoreBrowseViewModel.cs*、] をクリックし、**追加**します。
2. モデルへの参照を追加**StoreBrowseViewModel**クラス。 これを行うには、次の追加名前空間を使用します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. 2 つのプロパティを追加**StoreBrowseViewModel**クラス:**ジャンル**と**アルバム**します。 これを行うには、次のコードを追加します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> 新機能**一覧&lt;アルバム&gt;** でしょうか: この定義を使用して、**一覧&lt;T&gt;** 型、場所**T**制約、。この要素を型**一覧**ここに属している**アルバム**(またはその子孫のいずれか)。
> 
> クラスとクラスまたはメソッドが宣言されているし、c# 言語の機能は、クライアント コードによってインスタンス化されるまでに 1 つまたは複数の種類の指定を遅延させるメソッドを設計するには、この機能と呼ばれる**ジェネリック**します。
> 
> **リスト&lt;T&gt;** はジェネリックと同等の**ArrayList**を入力しで使用できるは、 **System.Collections.Generic**名前空間。 使用する利点の 1 つ**ジェネリック**型が指定されているため、型チェックに要素をキャストなどの操作を処理する必要はありませんが**アルバム**で行う場合、**ArrayList**します。

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>タスク 3 - 新しい ViewModel を使用して、StoreController

このタスクでは、変更、 **StoreController**の**参照**と**詳細**のアクション メソッドを使用して、新しい**StoreBrowseViewModel**.

1. Models フォルダーへの参照を追加**StoreController**クラス。 これを行うには、展開、**コント ローラー**フォルダーで、**ソリューション エクスプ ローラー**を開くと、 **StoreController**クラス。 次のコードを追加します。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. 置換、**参照**アクション メソッドを使用して、 **StoreViewBrowseController**クラス。 ダミー データでジャンル、および新しいアルバムの 2 つのオブジェクトを作成します (次のハンズオン ラボで、データベースから実際のデータで使用します)。 これを行うには、置換、**参照**メソッドを次のコード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. 置換、**詳細**アクション メソッドを使用して、 **StoreViewBrowseController**クラス。 新規に作成する、**アルバム**に返されるオブジェクト、**ビュー**します。 これを行うには、置換、**詳細**メソッドを次のコード。

    (コード スニペット - *ASP.NET MVC 4 の基礎 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>タスク 4 - 参照ビュー テンプレートを追加します。

このタスクでは、追加、**参照**特定のジャンルのアルバムを表示するビュー。

1. 新しいテンプレートの表示を作成する前にプロジェクトをビルドする必要がありますように、**ビューの追加**ダイアログを知って、 **ViewModel**クラスを使用します。 選択してプロジェクトをビルド、**ビルド**メニュー項目をクリックし**ビルド MvcMusicStore**します。
2. 追加、**参照**ビュー。 これを行うで右クリックし、**参照**のアクション メソッド、 **StoreController**  をクリック**ビューの追加**します。
3. **ビューの追加** ダイアログ ボックスで、ビュー名があることを確認**参照**します。 チェック、**厳密に型指定されたビューを作成**チェック ボックスを選択し、 **StoreBrowseViewModel**から、**モデル クラス**ドロップダウンします。 既定値は、他のフィールドのままにします。 次に、 **[追加]** をクリックします。

    ![[参照] ビューを追加する](aspnet-mvc-4-fundamentals/_static/image29.png "参照ビューの追加")

    *[参照] ビューを追加します。*
4. 変更、 **Browse.cshtml**ジャンルの情報を表示するへのアクセス、 **StoreBrowseViewModel**ビュー テンプレートに渡されるオブジェクト。 これを行うには、次の内容が置換: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションの実行

このタスクでは、テストを**参照**メソッドからアルバムの取得、**参照**メソッドのアクション。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して  **/保存/を参照しますか?ジャンル Disco を =** アクションが 2 つのアルバムを返すことを確認します。

    ![ストアのディスコ アルバムを閲覧](aspnet-mvc-4-fundamentals/_static/image30.png "ストア ディスコ アルバムを参照")

    *ストアのディスコ アルバムを参照*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>タスク 6 - について、特定のアルバム情報の表示

このタスクでは、実装、**ストア/詳細**については、特定のアルバムを表示するビュー。 このハンズオン ラボでは、そのアルバムに関する表示すべてに既に含まれて、**ビュー**テンプレート。 作成する代わりに、 **StoreDetailsViewModel**クラスは現在使用して**StoreBrowseViewModel**アルバムを渡すテンプレート。

1. Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。 新しい追加**詳細**の表示、 **StoreController**の**詳細**アクション メソッド。 これを行うを右クリックし、**詳細**メソッドで、 **StoreController**クラスし、をクリックして**ビューの追加**します。
2. **ビューの追加**ダイアログ ボックスで、いることを確認、**ビュー名**は**詳細**します。 チェック、**厳密に型指定されたビューを作成**チェック ボックスを選択し、**アルバム**から、**モデル クラス**ドロップダウンします。 既定値は、他のフィールドのままにします。 次に、 **[追加]** をクリックします。 これを作成し、開き、 **\Views\Store\Details.cshtml**ファイル。

    ![詳細ビューを追加する](aspnet-mvc-4-fundamentals/_static/image31.png "詳細ビューの追加")

    *詳細ビューの追加*
3. 変更、 **Details.cshtml**アルバムの情報を表示するファイルへのアクセス、**アルバム**ビュー テンプレートに渡されるオブジェクト。 これを行うには、次の内容が置換: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>タスク 7 のアプリケーションを実行します。

このタスクでは、テストを**詳細**ビューからアルバムの情報の取得、**アクションの詳細を**メソッド。

1. キーを押して**F5**アプリケーションを実行します。
2. プロジェクトの開始、**ホーム**ページ。 URL を変更して **/Store/Details/5**アルバムの情報を確認します。

    ![アルバムの詳細を閲覧](aspnet-mvc-4-fundamentals/_static/image32.png "アルバムの詳細を参照")

    *アルバムの詳細を参照*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>タスク 8 - ページ間のリンクの追加

このタスクで、該当するすべてのジャンル名にリンクがあるストア ビューのリンクを追加するは **/ストア/参照**URL。 たとえば、ジャンルをクリックすると、このように**Disco**に移動します **/ストア/参照でしょうか。 ジャンル Disco を =** URL。

1. Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。 更新プログラム、**インデックス**ページへのリンクを追加する、**参照**ページ。 これを実行する、**ソリューション エクスプ ローラー**展開、**ビュー**フォルダー、**ストア**フォルダーをダブルクリックします、 **Index.cshtml**ページ。
2. 選択されたジャンルを示す参照ビューへのリンクを追加します。 これを行うには、内の次の強調表示されたコードを置き換える、 **&lt;li&gt;** タグ: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > 別の方法は、次のようなコードで、ページに直接リンクは。
   > 
   > &lt;a =&quot;/ストア/参照? ジャンル =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > このアプローチは、その文字列をハードコードに依存します。 後で、コント ローラーを変更した場合はこの命令を手動で変更する必要があります。 優れた代替を使用して、 **HTML ヘルパー**メソッド。 ASP.NET MVC には、これは、このようなタスクに使用できる HTML ヘルパー メソッドが含まれています。 **Html.ActionLink()** ヘルパー メソッドでは、簡単に構築 HTML **&lt;、&gt;** リンクについては、URL パスが正しく URL エンコードすることを確認します。
   > 
   > Htlm.ActionLink では、いくつかのオーバー ロードがあります。 この演習では、次の 3 つのパラメーターを受け取るいずれかを使用します。
   > 
   > 1. リンク テキストは、ジャンル名が表示されます。
   > 2. コント ローラー アクションの名前 (**参照**)
   > 3. ルート名の両方を指定する、パラメーターの値 (**ジャンル**) と値 (**ジャンル名**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>タスク 9 - アプリケーションの実行

このタスクで、ジャンルごとに、適切なへのリンクが表示されることをテストは **/ストア/参照**URL。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/格納**各ジャンルに適切なリンクしていることを確認する **/ストア/参照**URL。

    ![[参照] ページへのリンクをジャンルを閲覧](aspnet-mvc-4-fundamentals/_static/image33.png "[ジャンル] を [参照] ページへのリンクを参照")

    *[参照] ページへのリンクをジャンルの参照*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>タスク 10 - 値を渡すを使用して動的な ViewModel コレクション

このタスクでは、モデルの変更を加えずに、コント ローラーとビューの間の値を渡すシンプルかつ強力な方法を説明します。 ASP.NET MVC 4 は、コレクションを提供します。 &quot;ViewModel&quot;、任意の値を動的に割り当てられ内部でコント ローラーとビューにもアクセスするには、をします。

一覧を渡す ViewBag 動的コレクションを使用するは今すぐ&quot;**ジャンルを星付き**&quot;ビュー コント ローラーから。 ストアのインデックス ビューへのアクセスは**ViewModel**し、情報を表示します。

1. Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。 開いている**StoreController.cs**および変更**インデックス**星付きジャンルでビューモデルのコレクションにメソッドの一覧を作成します。

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > 構文を使用することも**ViewBag [&quot;Starred&quot;]** プロパティにアクセスします。
2. 星のアイコン**&quot;starred.png&quot;** に含まれている、 **Source\Assets\Images**このラボのフォルダー。 アプリケーションに追加するからコンテンツをドラッグして、 **Windows エクスプ ローラー**ウィンドウに、**ソリューション エクスプ ローラー** Visual Web Developer Express を次に示すように。

    ![星のイメージをソリューションに追加](aspnet-mvc-4-fundamentals/_static/image34.png "星のイメージをソリューションに追加")

    *星のイメージをソリューションに追加します。*
3. ビューを開く**Store/Index.cshtml**およびコンテンツを変更します。 読み取りが、&quot;星付き&quot;プロパティ、 **ViewBag**コレクション、および現在のジャンル名がリストかどうか。 その場合はジャンルのリンクを右の星のアイコンが表示されます。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>タスク 11 - アプリケーションの実行

このタスクでは、星印が付いたジャンルが星のアイコンを表示することをテストします。

1. キーを押して**F5**アプリケーションを実行します。
2. プロジェクトの開始、**ホーム**ページ。 URL を変更して **/格納**おすすめ、ジャンルごとに重視のラベルがあることを確認します。

    ![ジャンルの星印が付いた要素を持つ参照](aspnet-mvc-4-fundamentals/_static/image35.png "星印が付いた要素を持つジャンルの参照")

    *ジャンルの星印が付いた要素を持つ参照*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>新しいテンプレートの ASP.NET MVC 4 簡単な手順 7:

この演習では、新しいテンプレートの最も関連する機能を見て、ASP.NET MVC 4 プロジェクト テンプレートでの機能強化探索は。

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>タスク 1: ASP.NET MVC 4 のインターネット アプリケーション テンプレートの表示

1. 既に開かれていない場合は開始**VS Express for Web**
2. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリーを選び、 **ASP.NET MVC 4 Web アプリケーション**します。 **名前**プロジェクト*MusicStore*し、更新、**ソリューション名**に*開始*、し、場所を選択します (または既定値のままに) をクリック **[ok]**.

    ![新しい ASP.NET MVC 4 プロジェクトを作成する](aspnet-mvc-4-fundamentals/_static/image36.png "新しい ASP.NET MVC 4 プロジェクトを作成します。")

    *新しい ASP.NET MVC 4 プロジェクトを作成します。*
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリックします**OK**。 ビュー エンジンとして Razor または ASPX のいずれかを選択することができますに注意してください。

    ![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](aspnet-mvc-4-fundamentals/_static/image37.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")

    *新しい ASP.NET MVC 4 インターネット アプリケーションの作成*

    > [!NOTE]
    > Razor 構文は、ASP.NET MVC 3 で導入されました。 その目的は、文字と、高速とワークフローをコーディング流体を有効にするファイルでは、必要なキーストロークの数を最小限に抑えるです。 既存 C #/vb (またはその他) を活用する razor 言語スキルと最高の HTML の構築ワークフローを有効にするテンプレート マークアップ構文を提供します。
4. キーを押して**F5**ソリューションを実行し、更新されたテンプレートを参照してください。 次の機能を確認することができます。

    1. **モダン スタイルのテンプレート**

        テンプレートが更新されました、現代的な外観の他のスタイルを提供します。

        ![ASP.NET MVC 4 のスタイル テンプレート](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 のテンプレートのスタイル")

        *ASP.NET MVC 4 のスタイルのテンプレート*
    2. **アダプティブ レンダリング**

        ブラウザー ウィンドウのサイズを確認し、どのページ レイアウトが動的に、新しいウィンドウ サイズに変化に注目してください。 これらのテンプレートでは、アダプティブ レンダリング手法を使用して、カスタマイズすることがなく、デスクトップとモバイルの両方のプラットフォームで正しく表示します。

        ![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](aspnet-mvc-4-fundamentals/_static/image39.png "別のブラウザーのサイズを使用した ASP.NET MVC 4 プロジェクト テンプレート")

        *別のブラウザーのサイズを使用した ASP.NET MVC 4 プロジェクト テンプレート*
5. デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。
6. ソリューションおよびプロジェクト テンプレートの ASP.NET MVC 4 で導入された新機能の一部をチェック_アウト整いました。

    ![ASP.NET MVC4 のインターネット-アプリケーション-プロジェクトのテンプレート](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")

    *ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート*

   1. **HTML5 マークアップ**

       テンプレート ビューを開くなどの新しいテーマ マークアップを確認する参照**About.cshtml**内で表示**ホーム**フォルダー。

       ![Razor、HTML5 マークアップを使用して、新しいテンプレート](aspnet-mvc-4-fundamentals/_static/image41.png "Razor、HTML5 マークアップを使用して、新しいテンプレート")

       *Razor、HTML5 マークアップを使用して、新しいテンプレート*
   2. **含まれる JavaScript ライブラリ**

      1. **jQuery**: HTML ドキュメントの走査、イベント処理、アニメーション化、および Ajax の相互作用を簡素化します。
      2. **jQuery UI**: このライブラリは低レベルの相互作用と効果を高度なアニメーションと jQuery JavaScript ライブラリの上に構築された、テーマを反映できますウィジェットの抽象化を提供します。

         > [!NOTE]
         > JQuery と jQuery UI について学習できますで[ [ http://docs.jquery.com/ ](http://docs.jquery.com/)](http://docs.jquery.com/)します。
      3. **KnockoutJS**: が含まれています: ASP.NET MVC 4 の既定のテンプレートには**KnockoutJS**JavaScript の MVVM フレームワーク JavaScript と HTML を使ったリッチで応答性に優れた web アプリケーションを作成することができます。 ように ASP.NET MVC 3 では、jQuery と jQuery UI ライブラリも含まれます ASP.NET MVC 4 で。

          > [!NOTE]
          > このリンクに KnockOutJS ライブラリの詳細を表示できます: [ http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)します。
      4. **Modernizr**: HTML5 および CSS3 のテクノロジを使用する場合、サイトの古いブラウザーとの互換性を行うこのライブラリが自動的に実行されます。

          > [!NOTE]
          > このリンクに Modernizr ライブラリの詳細を表示できます: [ http://www.modernizr.com/](http://www.modernizr.com/)します。
   3. **ソリューションに含まれる SimpleMembership**

       SimpleMembership は、以前の ASP.NET ロールおよびメンバーシップ プロバイダーのシステムの代わりとして設計されています。 簡単、開発者はセキュリティで保護された web ページをより柔軟な方法で多数の新機能があります。

       インターネット テンプレートは、SimpleMembership を統合する、いくつかの点を設定が既に、OAuthWebSecurity (OAuth アカウントの登録、ログイン、管理など) 用と Web のセキュリティを使用する、AccountController の準備など。

       ![ソリューションに含めた SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership がソリューションに含まれる")

       *ソリューションに含めた SimpleMembership*

       > [!NOTE]
       > 詳細情報の検索[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN にします。

> [!NOTE]
> また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボは、ASP.NET MVC の基礎について説明しました。

- MVC アプリケーションとやり取りする方法の主要な要素
- ASP.NET MVC アプリケーションを作成する方法
- URL とクエリ文字列を通じて渡される追加して、パラメーターを処理するコント ローラーを構成する方法
- 一般的な HTML コンテンツ、外観と HTML コンテンツを表示するビュー テンプレートを強化するスタイル シートのテンプレートをセットアップするレイアウトのマスター ページを追加する方法
- ビュー テンプレートのプロパティに渡すため ViewModel パターンを使用して、動的な情報を表示する方法
- ビュー テンプレートでコント ローラーに渡されるパラメーターを使用する方法
- ASP.NET MVC アプリケーション内のページへのリンクを追加する方法
- 追加して、ビューでの動的プロパティを使用する方法
- ASP.NET MVC 4 プロジェクト テンプレートでの機能強化

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-fundamentals/_static/image44.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-fundamentals/_static/image45.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](aspnet-mvc-4-fundamentals/_static/image46.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express for Web のタイル*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal

1. 移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。

    ![Windows Azure ポータルにログオン](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure ポータルにログオン")

    *Windows Azure 管理ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](aspnet-mvc-4-fundamentals/_static/image49.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。 簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-fundamentals/_static/image50.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](aspnet-mvc-4-fundamentals/_static/image51.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](aspnet-mvc-4-fundamentals/_static/image52.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](aspnet-mvc-4-fundamentals/_static/image53.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。

    ![発行プロファイルのダウンロード web サイト](aspnet-mvc-4-fundamentals/_static/image54.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](aspnet-mvc-4-fundamentals/_static/image55.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-fundamentals/_static/image56.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](aspnet-mvc-4-fundamentals/_static/image58.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](aspnet-mvc-4-fundamentals/_static/image59.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](aspnet-mvc-4-fundamentals/_static/image60.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](aspnet-mvc-4-fundamentals/_static/image61.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](aspnet-mvc-4-fundamentals/_static/image62.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](aspnet-mvc-4-fundamentals/_static/image63.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。

     ![ターゲットの接続文字列を構成する](aspnet-mvc-4-fundamentals/_static/image64.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](aspnet-mvc-4-fundamentals/_static/image65.png "データベース文字列を作成します。")

    *データベースの作成*
7. Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](aspnet-mvc-4-fundamentals/_static/image66.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](aspnet-mvc-4-fundamentals/_static/image67.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

    ![Windows Azure にアプリケーションが発行される](aspnet-mvc-4-fundamentals/_static/image68.png "アプリケーションは Windows Azure に発行")

    *Windows Azure に発行されたアプリケーション*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-fundamentals/_static/image69.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](aspnet-mvc-4-fundamentals/_static/image70.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-fundamentals/_static/image71.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-fundamentals/_static/image72.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-fundamentals/_static/image73.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-fundamentals/_static/image74.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
