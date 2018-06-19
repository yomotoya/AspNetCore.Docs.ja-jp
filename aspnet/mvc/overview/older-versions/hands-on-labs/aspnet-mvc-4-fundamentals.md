---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 基礎 |Microsoft ドキュメント
author: rick-anderson
description: このハンズオン ラボは、MVC (モデル ビュー コント ローラー) Music Store、チュートリアルのアプリケーションを紹介し、ASP.NET MV を使用する方法の詳細な手順について説明しますに基づきます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 225dff4663e0e556cfb8966f1078848b4c2b47a5
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306768"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 の基礎

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

このハンズオン ラボは、MVC (モデル ビュー コント ローラー) Music Store、紹介し、ASP.NET MVC と Visual Studio を使用する方法の詳細な手順について説明しますチュートリアル アプリケーションに基づいています。 ラボでは、わかりやすくするためを学習しますをこれらのテクノロジを同時に使用する、まだ power です。 単純なアプリケーションを開始し、完全に機能 ASP.NET MVC 4 Web アプリケーションになるまで、ビルドします。

このラボは、ASP.NET MVC 4 で動作します。

ASP.NET MVC 3 のバージョンのチュートリアル アプリケーションを調査したい場合でを検索[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。

このハンズオン ラボでは、HTML および JavaScript などの Web 開発テクノロジで、開発者の経験があることを前提としています。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。 このラボに固有のプロジェクトは[ASP.NET MVC 4 基礎](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)です。

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>音楽ストア アプリケーション

この実習でビルドされる音楽ストアの web アプリケーションに 3 つの主要な部分は構成されています: ショッピング、チェック アウト、および管理します。 閲覧者は、ジャンル アルバムを参照、カートにアルバムを追加、選択内容を確認および最後にログインし、注文を完了にチェック アウトを続行することができます。 さらに、ストア管理者は、使用可能なアルバムとその主要なプロパティを管理することができます。

![Music Store 画面](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store 画面")

*Music Store 画面*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

使用して音楽ストア アプリケーションがビルドされます**モデル ビュー コント ローラー (MVC)** アプリケーションを次の 3 つの主なコンポーネントを分離するアーキテクチャ パターン。

- **モデル**: モデル オブジェクトは、ドメイン ロジックを実装するアプリケーションの部分です。 多くの場合、モデル オブジェクトも取得し、モデルの状態をデータベースに格納します。
- **ビュー:** ビューは、アプリケーションのユーザー インターフェイス (UI) を表示するコンポーネントです。 通常、この UI は、モデル データから作成されます。 例には、テキスト ボックスやアルバム オブジェクトの現在の状態に基づいて、ボックスの一覧を表示するアルバムの編集ビューがあります。
- **コント ローラー:** コント ローラーは、コンポーネントのユーザーとの対話を処理して、モデルを操作し、最終的には、UI をレンダリングするビューを選択します。 MVC アプリケーションでは、ビューは情報のみを表示し、コントローラーがユーザーの入力と操作を処理して応答します。

MVC パターンでは、これらの要素間の疎結合を提供しつつ、アプリケーション (入力ロジック、ビジネス ロジック、および UI ロジック) のさまざまな側面を分離するアプリケーションを作成するのに役立ちます。 この分離では、一度に実装の 1 つの側面に注目することができます、アプリケーションをビルドするときに、複雑さを管理できます。 さらに、MVC パターンしやすいアプリケーションを作成するためのテスト駆動開発 (TDD) の使用を促進もアプリケーションをテストします。

**ASP.NET MVC** framework では、ASP.NET MVC ベースの Web アプリケーションを作成するための代わりに、ASP.NET Web フォーム パターンが用意されています。 **ASP.NET MVC**フレームワークは、軽量で高テスト可能なプレゼンテーション フレームワークを (Web フォーム ベースのアプリケーションと同様) と統合されたマスター ページなど、メンバーシップに基づく既存の ASP.NET 機能認証できるため、中核となる .NET framework のすべての機能を取得します。 これは、既に使用しているすべてのライブラリがも ASP.NET MVC 4 で使用できるため ASP.NET Web フォームに慣れている既に場合に便利です。

さらに、MVC アプリケーションは、次の 3 つの主要なコンポーネント間の疎結合では、並行開発も促進されます。 たとえば、1 人の開発者は、ビューで作業できる、2 番目の開発者、コント ローラー ロジックで作業でき、3 番目の開発者は、モデル内のビジネス ロジックに集中できます。

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- 音楽ストア アプリケーションのチュートリアルに基づいて最初から ASP.NET MVC アプリケーションを作成します。
- その主な機能を参照するため、サイトのホーム ページに Url を処理するコント ローラーを追加します。
- スタイルと共に表示される内容をカスタマイズするビューを追加します。
- 含めるし、データとドメインのロジックを管理するモデル クラスを追加します。
- ビュー モデルのパターンを使用して、コント ローラーのアクションからテンプレートを表示する情報を渡す
- インターネット アプリケーションの ASP.NET MVC 4 の新しいテンプレートを調査します。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (読み取り[付録 A](#AppendixA)をインストールする方法について)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。 実行のコード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の演習では、このハンズオン ラボから成ります。

1. [手順 1: MusicStore ASP.NET MVC Web アプリケーション プロジェクトの作成](#Exercise1)
2. [手順 2: コント ローラーの作成](#Exercise2)
3. [手順 3: コント ローラーにパラメーターを渡す](#Exercise3)
4. [手順 4: ビューを作成します。](#Exercise4)
5. [手順 5: ビュー モデルを作成します。](#Exercise5)
6. [手順 6: ビュー内のパラメーターの使用](#Exercise6)
7. [手順 7: ASP.NET MVC 4 の新しいテンプレートの周囲膝](#Exercise7)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **60 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>手順 1: MusicStore ASP.NET MVC Web アプリケーション プロジェクトの作成

この演習では、Visual Studio 2012 Express でのメイン フォルダーの組織と、Web、ASP.NET MVC アプリケーションを作成する方法を学習します。 さらに、新しいコント ローラーを追加し、アプリケーションのホーム ページで、単純な文字列を表示する方法を学習します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>タスク 1 - ASP.NET MVC Web アプリケーション プロジェクトを作成します。

1. このタスクでは、Visual Studio の MVC テンプレートを使用して、空の ASP.NET MVC アプリケーション プロジェクトを作成します。 開始**VS Express for Web**です。
2. **[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。
3. **新しいプロジェクト**ダイアログ ボックスの 、 **ASP.NET MVC 4 Web Application** 、プロジェクトの種類の下にある**Visual C# の場合、** **Web**テンプレート一覧です。
4. 変更、**名前**に*MvcMusicStore*です。
5. 新しいソリューションの場所を設定**開始**例については、この手順のソース フォルダー内のフォルダー **[、HOL PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**です。 **[OK]** をクリックします。

    ![[新しいプロジェクト] ダイアログ ボックスを作成する](aspnet-mvc-4-fundamentals/_static/image2.png "新しいプロジェクト ダイアログ ボックスを作成します。")

    *[新しいプロジェクト] ダイアログ ボックスを作成します。*
6. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**基本**テンプレートことを確認し、**ビュー エンジン**が選択されている**Razor**です。 **[OK]** をクリックします。

    ![新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス](aspnet-mvc-4-fundamentals/_static/image3.png "新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス")

    *新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>タスク 2 - ソリューションの構造を調べる

ASP.NET MVC フレームワークには、MVC パターンをサポートする Web アプリケーションを作成するための Visual Studio プロジェクト テンプレートが含まれています。 このテンプレートは、必要なフォルダー、項目テンプレート、および構成ファイル エントリを新しい ASP.NET MVC Web アプリケーションを作成します。

このタスクでは、関連する要素を理解するソリューションの構造とそのリレーションシップを調べます。 既定では、ASP.NET MVC フレームワークを使用するため、すべての ASP.NET MVC アプリケーションで、次のフォルダーが含まれて、&quot;設定よりも規約&quot;アプローチでは、し、いくつかの既定の条件がフォルダーの名前に基づく規則。

1. プロジェクトを作成した後は、右側にあるソリューション エクスプ ローラーで作成されているフォルダー構造を確認します。

    ![ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造](aspnet-mvc-4-fundamentals/_static/image4.png "ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造")

    *ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造*

   1. **コント ローラー**です。 このフォルダーは、コント ローラー クラスに格納されます。 MVC ベース アプリケーションでコント ローラーは、エンド ユーザーとの対話を処理、モデルを操作および最終的には、UI をレンダリングするビューを選択するためです。

       > [!NOTE]
       > MVC フレームワークには、末尾にはすべてのコント ローラーの名前が必要です。&quot;コント ローラー&quot;-たとえば、の HomeController、LoginController、productcontroller です。
   2. **モデル**です。 MVC Web アプリケーションのアプリケーション モデルを表すクラスのこのフォルダーが提供されます。 これにより、オブジェクトとデータ ストアと対話するためのロジックを定義するコードが通常含まれます。 通常、実際のモデル オブジェクトは、別のクラス ライブラリになります。 新しいアプリケーションを作成するときにクラスを含めるし、開発サイクルの後で別のクラス ライブラリに移動可能性があります。
   3. **ビュー**です。 このフォルダーは、ビュー、アプリケーションのユーザー インターフェイスを表示するために担当のコンポーネントの推奨される場所です。 ビューは、ビューのレンダリングに関連するその他のファイルだけでなく、.aspx、.ascx、.cshtml および .master ファイルを使用します。 Views フォルダーには、各コント ローラー用のフォルダーが含まれています。フォルダーには、コント ローラー名のプレフィックスが付いたです。 たとえば、という名前のコント ローラーがある場合**HomeController**、Views フォルダーには、Home という名前のフォルダーにが含まれます。 既定では、ASP.NET MVC フレームワークは、ビューを読み込むときに、.aspx ファイルが検索 Views\controllerName フォルダーに要求されたビューの名前を持つ (**ビュー [ControllerName] [アクション] .aspx**) または (**ビュー [ControllerName][アクション] .cshtml**) Razor ビュー。

      > [!NOTE]
      > フォルダーに加え、前の表に、MVC Web アプリケーションを使用して、 **Global.asax**グローバル URL ルーティングを設定するファイルの既定値 を使用して、 **Web.config**アプリケーションを構成するファイル。

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>タスク 3 -、HomeController の追加

MVC フレームワークを使用して ASP.NET アプリケーションでは、ユーザーとの対話はページとを発生させると、それらのページからのイベント処理の周囲に編成されます。 これに対し、ASP.NET MVC アプリケーションでのユーザー操作は、コント ローラーとそのアクション メソッドの周囲に編成されます。

その一方で、ASP.NET MVC フレームワークは、Url をコント ローラーと呼ばれるクラスにマップします。 コント ローラーが入力方向の要求を処理、ユーザー入力との相互作用を処理、適切なアプリケーション ロジックを実行およびクライアントに返送する応答を決定 (HTML を表示、ファイルをダウンロードするには、別の URL をリダイレクトします。)。 HTML を表示するには、場合、コント ローラー クラスは、通常、要求の HTML マークアップを生成する別個のビュー コンポーネントを呼び出します。 MVC アプリケーションでは、ビューは情報のみを表示し、コントローラーがユーザーの入力と操作を処理して応答します。

このタスクでは、音楽ストア サイトのホーム ページの Url を処理するコント ローラー クラスを追加します。

1. 右クリック**コント ローラー** [ソリューション エクスプ ローラー] を選択内のフォルダー**追加**し**コント ローラー**コマンド。

    ![コント ローラー コマンドを追加する](aspnet-mvc-4-fundamentals/_static/image5.png "コント ローラー コマンドを追加します。")

    *コント ローラーのコマンドを追加します。*
2. **コント ローラーの追加**ダイアログが表示されます。 コント ローラーの名前を付けます*HomeController*とキーを押します**追加**です。

    ![[追加] ダイアログのコント ローラー](aspnet-mvc-4-fundamentals/_static/image6.png "コント ローラー ダイアログの追加")

    *[追加] ダイアログのコント ローラー*
3. ファイル**HomeController.cs**で作成された、**コント ローラー**フォルダーです。 ために、 **HomeController**そのインデックスの操作で文字列を返す、置換、**インデックス**メソッドを次のコード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex1 HomeController インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションを実行します。

このタスクでは、web ブラウザーでアプリケーションを試みます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。 プロジェクトがコンパイルされ、ローカル IIS Web サーバーを起動します。 ローカル IIS Web サーバーが自動的に開きます web ブラウザーが Web サーバーの URL をポイントします。

    ![Web ブラウザーで実行されているアプリケーション](aspnet-mvc-4-fundamentals/_static/image7.png "web ブラウザーで実行されているアプリケーション")

    *Web ブラウザーで実行されているアプリケーション*

    > [!NOTE]
    > ローカル IIS Web サーバーは、ランダムな空いているポート番号で、web サイトが実行されます。 上記の図に、サイトが実行されている`http://localhost:50103/`ポート 50103 使用されているため、します。 使用するポート番号が異なる場合があります。
2. ブラウザーを閉じます。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>手順 2: コント ローラーの作成

この演習では、音楽ストア アプリケーションの簡単な機能を実装するコント ローラーを更新する方法を学習します。 そのコント ローラーには、それぞれ次の特定の要求を処理するアクション メソッドを定義します。

- ミュージック ストアで音楽のジャンルの一覧ページ
- すべての特定のジャンルの音楽のアルバムを一覧表示する [参照] ページ
- 特定の音楽のアルバムに関する情報が表示される詳細ページ

この演習では、スコープにそれらのアクションが返されるだけ文字列ここまででします。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>タスク 1 - 新しい StoreController クラスの追加

このタスクでは、新しいコント ローラーを追加します。

1. まだ開いていない場合は開始**VS が Express for Web 2012**です。
2. **ファイル**] メニューの [選択**プロジェクトを開く**です。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex02 CreatingAController\Begin****[Begin.sln]** をクリック**開く**です。 代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. 新しいコント ローラーを追加します。 これを行うを右クリックし、**コント ローラー** [ソリューション エクスプ ローラー] を選択内のフォルダー**追加**し、**コント ローラー**コマンド。 変更、**コント ローラー名**に*StoreController*、 をクリック**追加**です。

    ![[追加] ダイアログのコント ローラー](aspnet-mvc-4-fundamentals/_static/image8.png "コント ローラー ダイアログの追加")

    *[追加] ダイアログのコント ローラー*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>タスク 2 - StoreController のアクションを変更します。

このタスクでは、呼び出されるコント ローラーのメソッドを変更して**アクション**です。 アクションは、URL 要求を処理して、ブラウザーや URL を呼び出したユーザーに送信されるコンテンツを判断します。

1. **StoreController**クラスには既に、**インデックス**メソッドです。 このラボに後で、音楽ストアのすべてのジャンルを一覧するページを実装するのに使用するされます。 ここでは、置き換えるだけです、**インデックス**メソッドを次のコード文字列を返す&quot;Store.Index() から Hello&quot;:。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex2 StoreController インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. 追加**参照**と**詳細**メソッドです。 これを行うには、次のコードを追加、 **StoreController**:

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションを実行します。

このタスクでは、web ブラウザーでアプリケーションを試みます。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. プロジェクトを起動、**ホーム**ページ。 各アクションの実装を確認する URL を変更します。

    1. **/Store**です。 表示されます **&quot;Store.Index() からこんにちは&quot;** です。
    2. **/ストア/参照**です。 表示されます **&quot;Store.Browse() からこんにちは&quot;** です。
    3. **/ストア/詳細**です。 表示されます **&quot;Store.Details() からこんにちは&quot;** です。

        ![参照 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse をブラウズ")

        */Store/Browse をブラウズ*
3. ブラウザーを閉じます。

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>手順 3: コント ローラーにパラメーターを渡す

これまでにはコント ローラーから定数文字列が返すことされています。 この演習では、URL と、クエリ文字列を使用し、ブラウザーにテキストを含む応答メソッドのアクション、コント ローラーにパラメーターを渡す方法を学習します。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>タスク 1 - StoreController をジャンル パラメーターを追加します。

このタスクでは使用して、 **querystring**へのパラメーターを送信する、**参照**でアクション メソッド、 **StoreController**です。

1. まだ開いていない場合は開始**VS Express for Web**です。
2. **ファイル**] メニューの [選択**プロジェクトを開く**です。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex03 PassingParametersToAController\Begin****[Begin.sln]** をクリック**開く**です。 代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. 開いている**StoreController**クラスです。 これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。
4. 変更、**参照**特定ジャンルを要求する文字列パラメーターを追加するメソッド。 ASP.NET MVC の任意のクエリ文字列を渡すまたはフォーム ポスト パラメーターの名前が自動的に**ジャンル**このアクション メソッドが呼び出されたときにします。 これを行うには、置換、**参照**メソッドを次のコード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> 使用している、 **HttpUtility.HtmlEncode**するユーティリティ メソッドがのようなリンクを使用して、ビューに Javascript を挿入できないように**ストア/参照しますか?ジャンル =&lt;スクリプト&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** です。
> 
> 詳細についてを参照してください[この msdn 記事](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)です。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションを実行します。

このタスクでは、web ブラウザーでアプリケーションを試してみるしを使用して、**ジャンル**パラメーター。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. プロジェクトを起動、**ホーム**ページ。 URL を変更して  */格納/参照しますか?ジャンル Disco を =* アクションがジャンル パラメーターを受け取ることを確認します。

    ![参照 StoreBrowseGenre Disco を =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre をブラウズ Disco を =")

    */Store/Browse をブラウズしますか。ジャンル Disco を =*
3. ブラウザーを閉じます。

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>タスク 3 - URL に埋め込まれた Id パラメーターを追加します。

このタスクでは使用して、 **URL**に渡す、 **Id**パラメーターを**詳細**のアクション メソッド、 **StoreController**です。 ASP.NET MVC の既定のルーティング規則がという名前のパラメーターとしてアクション メソッド名の後に、URL のセグメントを処理するには**Id**です。アクション メソッドに Id をという名前のパラメーターがある場合は、し、ASP.NET MVC は自動的に URL セグメントをパラメーターとして渡します。 URL で**ストア/詳細/5**、 **Id**として解釈されます**5**です。

1. 変更、**詳細**のメソッド、 **StoreController**を追加する、 **int**という名前のパラメーター **id**です。これを行うには、置換、**詳細**メソッドを次のコード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションを実行します。

このタスクでは、web ブラウザーでアプリケーションを試してみるしを使用して、 **Id**パラメーター。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. プロジェクトを起動、**ホーム**ページ。 URL を変更して */Store/Details/5*アクションが、id パラメーターを受け取っていることを確認します。

    ![参照 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 をブラウズ")

    */Store/Details/5 をブラウズ*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>手順 4: ビューを作成します。

これまでにするがされた文字列から返すコント ローラーのアクション。 コント ローラーが機能するしくみを理解するための便利な方法はどのように、実際の Web アプリケーションは構築されません。 ビューは、テンプレート ファイルの使用にブラウザーに HTML を生成するためのより適切な方法を提供するコンポーネントです。

この演習では、一般的な HTML コンテンツをサイトと、最後に HTML を返す HomeController を有効にするビュー テンプレートのルック アンド フィールを強化するために、スタイル シートのテンプレートをセットアップするレイアウトのマスター ページを追加する方法を学習します。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>タスク 1 - ファイルを変更する\_layout.cshtml

ファイル **~/Views/Shared/\_layout.cshtml**一般的な HTML web サイト全体にわたって使用するためのテンプレートをセットアップすることができます。 このタスクでは、ホーム ページおよびストアの領域にリンクに共通のヘッダーでレイアウトのマスター ページを追加します。

1. まだ開いていない場合は開始**VS Express for Web**です。
2. **ファイル**] メニューの [選択**プロジェクトを開く**です。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex04 CreatingAView\Begin****[Begin.sln]** をクリック**開く**です。 代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. ファイル <strong>\_layout.cshtml</strong>コンテナーの HTML レイアウト、サイトのすべてのページが含まれています。 含まれている、 <strong>&lt;html&gt;</strong> HTML 応答の要素だけでなく<strong>&lt;ヘッド&gt;</strong>と<strong>&lt;本文&gt;</strong>要素。 <strong>@RenderBody()</strong> HTML 内の本文を識別地域そのビューのテンプレートは、動的なコンテンツをコピーすることができます。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. サイト内のすべてのページで、ホーム ページおよびストアの領域へのリンクに共通のヘッダーを追加します。 そのためには、下の次のコードを追加&lt;本文&gt;ステートメントです。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. 各ページの body セクションに表示するために div が含まれます。 置き換える <strong>@RenderBody()</strong>を次の強調表示されているコード: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > ご存知でしたか。 Visual Studio 2012 では、HTML やコード ファイルで一般的に使用されるコードを追加しやすいスニペットが。 」と入力して試して**&lt;div&gt;** キーを押して**タブ**完全な挿入を 2 回**div**タグ。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>作業 2: 追加の CSS スタイル シート

空のプロジェクト テンプレートには、基本的なフォームや検証メッセージを表示するために使用するスタイルだけを含む非常に簡素化された CSS ファイルが含まれています。 追加の CSS および (デザイナーによって提供される可能性があります) イメージは、サイトのルック アンド フィールを強化するために使用されます。

このタスクでは、サイトのスタイルを定義する CSS スタイル シートを追加します。

1. CSS ファイルとイメージに含まれる、 **Source\Assets\Content**このラボのフォルダーです。 それらをアプリケーションに追加するためにからコンテンツをドラッグ、 **Windows エクスプ ローラー**ウィンドウに、**ソリューション エクスプ ローラー** Visual Studio Express for Web、次に示すようにします。

    ![スタイルの内容をドラッグする](aspnet-mvc-4-fundamentals/_static/image12.png "スタイルの内容をドラッグします。")

    *スタイルの内容をドラッグします。*
2. 警告ダイアログが表示されます、置換するのには確認を求める**Site.css**ファイルといくつかの既存のイメージです。 確認**すべての項目に適用する** をクリック**はい**です。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>タスク 3 - ビュー テンプレートを追加します。

このタスクでは、レイアウトのマスター ページを使用して HTML 応答を生成するテンプレートの表示を追加し、CSS は、この手順で追加します。

1. サイトのホーム ページを参照するときに、ビュー テンプレートを使用して、まず必要があることを示す文字列を返す代わりに、 **HomeController インデックス**メソッドは、**ビュー**です。 開いている**HomeController**クラスし、変更、**インデックス**を返すメソッドを**ActionResult**、してそれを返す**View()** です。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex4 HomeController インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. ここで、適切なビュー テンプレートを追加する必要があります。 これを行う**を右クリックして**内、**インデックス**アクション メソッドを選択**ビューの追加**です。 これが表示されます、**ビューの追加**ダイアログ。

    ![インデックス メソッド内からビューを追加する](aspnet-mvc-4-fundamentals/_static/image13.png "インデックス メソッド内からビューを追加します。")

    *インデックス メソッド内からビューを追加します。*
3. **ビューの追加**ビュー テンプレート ファイルを生成するダイアログが表示されます。 既定では、このダイアログ ボックスでは、それを使用するアクション メソッドに一致するビュー テンプレートの名前が事前設定します。 使用したため、**ビューの追加**内でコンテキスト メニュー、**インデックス**HomeController、内のアクション メソッド、**ビューの追加**ダイアログでは、既定の表示名としてのインデックス。 **[追加]** をクリックします。

    ![[追加] ダイアログの表示](aspnet-mvc-4-fundamentals/_static/image14.png "追加ビュー ダイアログ ボックス")

    *[追加] ダイアログの表示*
4. Visual Studio によって生成、 **Index.cshtml**内部テンプレートの表示、 **views \home**フォルダーし、それを開きます。

    ![作成されたインデックス ビューをホーム](aspnet-mvc-4-fundamentals/_static/image15.png "ホーム インデックス ビューの作成")

    *作成されたホームのインデックス ビュー*

    > [!NOTE]
    > 名前と場所、 **Index.cshtml**ファイルは関連し、既定の ASP.NET MVC の名前付け規則に従います。
    > 
    > フォルダー \Views\**ホーム** コント ローラー名と一致する (**ホーム**コント ローラー)。 ビューのテンプレート名 (**インデックス**)、ビューを表示するコント ローラー アクション メソッドに一致します。
    > 
    > これにより、ASP.NET MVC は、この名前付け規則を使用して、ビューを取得するときに、名前またはビュー テンプレートの場所を明示的に指定することで回避できます。
5. 生成されたビュー テンプレートがに基づいて、  **\_layout.cshtml**以前に定義されているテンプレートです。 ViewBag.Title するプロパティを更新**ホーム**、主な内容の変更と**これは、ホーム ページ**次のコードに示すように。

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. 選択**MvcMusicStore**キーを押して、ソリューション エクスプ ローラーでプロジェクト**f5 キーを押して**アプリケーションを実行します。

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>タスク 4: 検証

前述の手順で、すべての手順を正しく実行したことを確認するために次の手順します。

ブラウザーで開かれているアプリケーションを注意してください。

1. HomeController の Index アクション メソッドが検出され、表示、 **\Views\Home\Index.cshtml**コードが呼び出されていなくても、テンプレートを表示**View() を返す**でビュー テンプレートの後にあるため、標準の名前付け規則です。
2. ホーム ページ内で定義されたへようこそ メッセージを表示、 **\Views\Home\Index.cshtml**ビュー テンプレート。
3. ホーム ページを使用して、  **\_layout.cshtml**テンプレート、およびウェルカム メッセージが標準サイト HTML レイアウト内に含まれるようにします。

    ![定義済みの LayoutPage とスタイルを使用して、インデックス ビューをホーム](aspnet-mvc-4-fundamentals/_static/image16.png "定義済みの LayoutPage とスタイルを使用して、インデックス ビュー ホーム")

    *定義済みの LayoutPage とスタイルを使用してホームのインデックス ビュー*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>手順 5: ビュー モデルを作成します。

これまでにハードコードされた、HTML を表示してビューを作成したが、動的な web アプリケーションを作成するために、ビュー テンプレートは、コント ローラーから情報を受信する必要があります。 そのような目的に使用する 1 つの一般的な手法は、 **ViewModel**パターンでは、これにより、コント ローラーを適切な HTML 応答を生成するために必要なすべての情報をパッケージ化します。

この演習では、ViewModel クラスを作成し、必要なプロパティを追加する方法を学習します。 ストアに格納され、ジャンルの一覧のジャンルの数。 作成された ViewModel を使用する StoreController を更新することも、および最後に、ページで説明したようにプロパティを表示する新しいビュー テンプレートを作成します。

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>タスク 1 - ViewModel クラスを作成します。

このタスクでは、ストア ジャンルの一覧のシナリオを実装する ViewModel クラスを作成します。

1. まだ開いていない場合は開始**VS Express for Web**です。
2. **ファイル**] メニューの [選択**プロジェクトを開く**です。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex05 CreatingAViewModel\Begin****[Begin.sln]** をクリック**開く**です。 代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. 作成、 **ViewModels** ViewModel を保持するフォルダーです。 これを行うには、最上位レベルを右クリックし**MvcMusicStore**プロジェクトで、**追加**し**新しいフォルダー**です。

    ![新しいフォルダーを追加する](aspnet-mvc-4-fundamentals/_static/image17.png "新しいフォルダーを追加します。")

    *新しいフォルダーを追加します。*
4. 名前のフォルダー *ViewModels*です。

    ![ソリューション エクスプ ローラーでフォルダーを ViewModels](aspnet-mvc-4-fundamentals/_static/image18.png "ソリューション エクスプ ローラー ViewModels フォルダー")

    *ソリューション エクスプ ローラー ViewModels フォルダー*
5. 作成、 **ViewModel**クラスです。 これを行うを右クリックし、 **ViewModels**最近作成、フォルダーを選択**追加**し**新しい項目の**します。 **コード**、選択、**クラス**項目し、ファイルの名前*StoreIndexViewModel.cs*、順にクリックして**追加**です。

    ![新しいクラスの追加](aspnet-mvc-4-fundamentals/_static/image19.png "新しいクラスの追加")

    *新しいクラスの追加*

    ![StoreIndexViewModel クラスを作成する](aspnet-mvc-4-fundamentals/_static/image20.png "作成 StoreIndexViewModel クラス")

    *StoreIndexViewModel クラスを作成します。*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>タスク 2 - ViewModel クラスにプロパティを追加します。

予期される HTML 応答を生成するために、StoreController からビュー テンプレートに渡される 2 つのパラメーターがある: ストアに格納され、ジャンルの一覧のジャンルの数。

このタスクでは、これら 2 つのプロパティを追加する、 **StoreIndexViewModel**クラス: **NumberOfGenres** (整数) と**ジャンル**(文字列の一覧)。

1. 追加**NumberOfGenres**と**ジャンル**プロパティを**StoreIndexViewModel**クラスです。 これを行うには、クラス定義に次の 2 行を追加します。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex5 StoreIndexViewModel プロパティ*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> **{Get; に設定}** 表記法では、# の使用により、自動実装プロパティの機能です。 バッキング フィールドを宣言することを必要とせず、プロパティの利点を提供します。

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>タスク 3 - 更新 StoreController、StoreIndexViewModel を使用するには

**StoreIndexViewModel**クラスからを渡すために必要な情報をカプセル化**StoreController**の**インデックス**メソッドを応答を生成するためにテンプレートの表示.

このタスクを更新、 **StoreController**を使用する、 **StoreIndexViewModel**です。

1. 開いている**StoreController**クラスです。

    ![StoreController クラスを開いて](aspnet-mvc-4-fundamentals/_static/image21.png "開く StoreController クラス")

    *開く StoreController クラス*
2. 使用するために、 **StoreIndexViewModel**クラス、 **StoreController**の上部にある次の名前空間を追加、 **StoreController**コード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: ViewModels を使用して Ex5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. 変更、 **StoreController**の**インデックス**アクション メソッドを作成して設定ように、 **StoreIndexViewModel**オブジェクトし、をビュー テンプレートに渡します関連付けを HTML 応答を生成します。

    > [!NOTE]
    > ラボで&quot;ASP.NET MVC モデルおよびデータ アクセス&quot;は、データベースからストア ジャンルの一覧を取得するコードを記述します。 次のコードでは、作成、**リスト**設定はダミー データ ジャンルの**StoreIndexViewModel**です。
    > 
    > 作成し、設定した後、 **StoreIndexViewModel**オブジェクトを引数として渡されます、**ビュー**メソッドです。 これは、ビュー テンプレートがそのオブジェクトを使用して関連付けを HTML 応答を生成することを示します。
4. 置換、**インデックス**メソッドを次のコード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex5 StoreController インデックス メソッド*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> 使用するを想定する可能性があります (C#) を使い慣れていない場合は、 **var**ことを意味、 **viewModel**変数は遅延バインディング。 正しくありません - c# コンパイラに使用されている型推論、変数に代入する新機能に基づくことがわかった**viewModel**の種類は**StoreIndexViewModel**です。 ローカルをコンパイルしても、 **viewModel**変数として、 **StoreIndexViewModel** get コンパイル時のチェックと Visual Studio コード エディターのサポートを入力します。

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>タスク 4 - StoreIndexViewModel を使用する表示テンプレートを作成します。

このタスクでは、コント ローラーから渡された StoreIndexViewModel オブジェクトを使用してジャンルの一覧を表示するビュー テンプレートを作成します。

1. 新しいテンプレートの表示を作成する前に、プロジェクトを構築してみましょうできるように、**ビューの追加 ダイアログ**知って、 **StoreIndexViewModel**クラスです。 選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。

    ![プロジェクトのビルド](aspnet-mvc-4-fundamentals/_static/image22.png "プロジェクトのビルド")

    *プロジェクトのビルド*
2. 新しいテンプレートの表示を作成します。 実行するには、内部を右クリックして、**インデックス**メソッドと選択**ビューの追加**です。

    ![ビューを追加する](aspnet-mvc-4-fundamentals/_static/image23.png "ビューを追加します。")

    *ビューの追加*
3. **ビューの追加 ダイアログ**から呼び出された、 **StoreController**、既定では、ビュー テンプレートが追加、 **\Views\Store\Index.cshtml**ファイル。 チェック、**ビューを作成する厳密に型指定された -** チェック ボックスをオンし、 **StoreIndexViewModel**として、**モデル クラス**です。 また、必ず選択されているビュー エンジンが**Razor**です。 **[追加]** をクリックします。

    ![[追加] ダイアログの表示](aspnet-mvc-4-fundamentals/_static/image24.png "追加ビュー ダイアログ ボックス")

    *[追加] ダイアログの表示*

    **\Views\Store\Index.cshtml**ビュー テンプレート ファイルが作成され、表示します。 提供された情報に基づいて、**ビューの追加**最後の手順では、テンプレートを必要とするビューでダイアログ、 **StoreIndexViewModel**を HTML 応答の生成に使用するデータとしてのインスタンス。 テンプレートを継承することがわかります、 `ViewPage<musicstore.viewmodels.storeindexviewmodel>` C# の場合。

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>タスク 5 - ビュー テンプレートの更新

このタスクでは、ジャンルおよびページ内でその名前の数を取得する最後のタスクで作成したビュー テンプレートが更新されます。

> [!NOTE]
> @ 構文を使用する (と呼ばれます&quot;コード ナゲット&quot;) ビュー テンプレート内でコードを実行します。

1. **Index.cshtml**ファイル内で、**ストア**フォルダー、そのコードを次に置き換えます。

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
2. ジャンルの一覧をループ**StoreIndexViewModel** HTML を作成および**&lt;ul&gt;** を使用して、 **foreach**ループします。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. キーを押して**f5 キーを押して**アプリケーションを実行し、[参照] を **/格納**です。 渡されたジャンルの一覧が表示されます、 **StoreIndexViewModel**オブジェクトから、 **StoreController**ビュー テンプレートにします。

    ![ジャンルの一覧を表示するビュー](aspnet-mvc-4-fundamentals/_static/image26.png "ジャンルの一覧を表示するビュー")

    *ジャンルの一覧を表示するビュー*
4. ブラウザーを閉じます。

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>手順 6: ビュー内のパラメーターの使用

手順 3 では、コント ローラーにパラメーターを渡す方法について学習しました。 この演習では、テンプレートの表示でそれらのパラメーターを使用する方法を学習します。 ため、するは最初に導入モデル クラスをデータとドメインのロジックを管理できるようにします。 さらに、心配するエンコードの URL パスなどの ASP.NET MVC アプリケーション内のページへのリンクを作成する方法を学習します。

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>タスク 1 - モデル クラスを追加します。

ViewModels、単に情報を渡すコント ローラーからビューに作成されるとは異なり、モデル クラスは、包含してデータとドメイン ロジックを管理する構築されます。 このタスクでは、これらの概念を表す 2 つのモデル クラスを追加します:**ジャンル**と**アルバム**です。

1. まだ開いていない場合は開始**VS Express for Web**
2. **ファイル**] メニューの [選択**プロジェクトを開く**です。 プロジェクトを開くダイアログ ボックスを参照**Source\Ex06 UsingParametersInView\Begin****[Begin.sln]** をクリック**開く**です。 代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. 追加、**ジャンル**モデル クラス。 これを行うを右クリックし、**モデル**フォルダーに、**ソリューション エクスプ ローラー****追加**し、**新しい項目の**オプション。 **コード**、選択、**クラス**項目し、ファイルの名前*Genre.cs*、順にクリックして**追加**です。

    ![クラスの追加](aspnet-mvc-4-fundamentals/_static/image27.png "クラスの追加")

    *新しい項目の追加*

    ![ジャンル モデル クラスを追加](aspnet-mvc-4-fundamentals/_static/image28.png "ジャンル モデル クラスを追加")

    *ジャンル モデル クラスを追加します。*
4. 追加、**名前**ジャンル クラスにプロパティです。 これを行うには、次のコードを追加します。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 ジャンル*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. 次の前と同じ手順を追加、**アルバム**クラスです。 これを行うを右クリックし、**モデル**フォルダーに、**ソリューション エクスプ ローラー****追加**し、**新しい項目の**オプション。 **コード**、選択、**クラス**項目し、ファイルの名前*Album.cs*、順にクリックして**追加**です。
6. アルバム クラスに次の 2 つのプロパティを追加:**ジャンル**と**タイトル**です。 これを行うには、次のコードを追加します。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 アルバム*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>タスク 2 -、StoreBrowseViewModel を追加します。

A **StoreBrowseViewModel**は選択したジャンルに一致するアルバムを表示するこのタスクで使用します。 このタスクでは、このクラスを作成し、処理する 2 つのプロパティを追加、**ジャンル**とその**アルバム**のリストします。

1. 追加、 **StoreBrowseViewModel**クラスです。 これを行うを右クリックし、 **ViewModels**フォルダーに、**ソリューション エクスプ ローラー****追加**し、**新しい項目の**オプション。 **コード**、選択、**クラス**項目し、ファイルの名前*StoreBrowseViewModel.cs*、順にクリックして**追加**です。
2. モデルへの参照を追加**StoreBrowseViewModel**クラスです。 これを行うには、次の追加名前空間を使用します。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. 2 つのプロパティを追加**StoreBrowseViewModel**クラス:**ジャンル**と**アルバム**です。 これを行うには、次のコードを追加します。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> 新機能**リスト&lt;アルバム&gt;** ?: この定義を使用して、**リスト&lt;T&gt;** 型、場所**T**制約、この要素を型**リスト**ここに属している**アルバム**(またはその子孫のいずれか)。
> 
> このクラスまたはメソッドが宣言されているし、c# 言語の機能は、クライアント コードでインスタンス化されるまでに 1 つまたは複数の種類の仕様を延期するクラスとメソッドをデザインする機能と呼ばれる**ジェネリック**です。
> 
> **リスト&lt;T&gt;** ジェネリックと同等は、 **ArrayList**を入力しで使用できるは、 **System.Collections.Generic**名前空間。 使用する利点の 1 つ**ジェネリック**いるため、型を指定すると、する必要がない型に要素をキャストなどの操作のチェックを処理する**アルバム**で行うよう**ArrayList**です。

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>タスク 3 -、StoreController で新しい ViewModel の使用

このタスクでは、変更、 **StoreController**の**参照**と**詳細**のアクション メソッドを使用して、新しい**StoreBrowseViewModel**.

1. モデル フォルダーへの参照を追加**StoreController**クラスです。 これを行うには、展開、**コント ローラー**内のフォルダー、**ソリューション エクスプ ローラー**を開くと、 **StoreController**クラスです。 次のコードを追加し、します。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. 置換、**参照**アクション メソッドを使用する、 **StoreViewBrowseController**クラスです。 ダミーのデータをジャンル、および新しいアルバムの 2 つのオブジェクトを作成します (次のハンズオン ラボでデータベースの実際のデータで使用します)。 これを行うには、置換、**参照**メソッドを次のコード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. 置換、**詳細**アクション メソッドを使用する、 **StoreViewBrowseController**クラスです。 新規に作成する、**アルバム**に返されるオブジェクト、**ビュー**です。 これを行うには、置換、**詳細**メソッドを次のコード。

    (コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>タスク 4 - 参照ビュー テンプレートを追加します。

このタスクでは、追加、**参照**特定ジャンルのアルバムを表示するビュー。

1. 新しいテンプレートの表示を作成する前に、プロジェクトをビルドする必要がありますできるように、**ビューの追加**ダイアログを知って、 **ViewModel**クラスを使用します。 選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。
2. 追加、**参照**ビュー。 これを行うには、右クリックし、**参照**のアクション メソッド、 **StoreController**  をクリック**ビューの追加**です。
3. **ビューの追加** ダイアログ ボックスで、ビュー名があることを確認**参照**です。 チェック、**厳密に型指定されたビューを作成する** チェック ボックスを選択し、 **StoreBrowseViewModel**から、**モデル クラス**ドロップダウンします。 既定値は、他のフィールドのままにします。 次に、 **[追加]** をクリックします。

    ![[参照] ビューを追加する](aspnet-mvc-4-fundamentals/_static/image29.png "参照ビューの追加")

    *[参照] ビューの追加*
4. 変更、 **Browse.cshtml**ジャンルの情報を表示するへのアクセス、 **StoreBrowseViewModel**ビュー テンプレートに渡されるオブジェクト。 これを行うには、コンテンツを次に置き換えます: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションを実行します。

このタスクは、テストを**参照**メソッドからアルバムの取得、**参照**メソッドのアクション。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して  **/格納/参照しますか?ジャンル Disco を =** アクションが 2 つのアルバムを返すことを確認します。

    ![ストア Disco アルバムを閲覧](aspnet-mvc-4-fundamentals/_static/image30.png "ストア Disco アルバムを参照")

    *ストア Disco アルバムを参照*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>タスク 6: を表示する情報は、特定のアルバム

このタスクでは、実装、**ストア/詳細**特定のアルバムに関する情報を表示するビュー。 このハンズオン ラボでは、そのアルバムに関する表示すべてに既に含まれて、**ビュー**テンプレート。 作成する代わりに、 **StoreDetailsViewModel**クラスは現在使用して**StoreBrowseViewModel**アルバムを渡すテンプレート。

1. Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。 新しい**詳細**の表示、 **StoreController**の**詳細**アクション メソッド。 これを行うを右クリックし、**詳細**メソッドで、 **StoreController**クラスし、をクリックして**ビューの追加**です。
2. **ビューの追加**ダイアログ ボックスで、いることを確認、**ビュー名**は**詳細**です。 チェック、**厳密に型指定されたビューを作成する** チェック ボックスを選択し、**アルバム**から、**モデル クラス**ドロップダウンします。 既定値は、他のフィールドのままにします。 次に、 **[追加]** をクリックします。 これが作成され、 **\Views\Store\Details.cshtml**ファイル。

    ![詳細ビューを追加する](aspnet-mvc-4-fundamentals/_static/image31.png "詳細ビューを追加します。")

    *詳細ビューを追加します。*
3. 変更、 **Details.cshtml**アルバムの情報を表示するファイルへのアクセス、**アルバム**ビュー テンプレートに渡されるオブジェクト。 これを行うには、コンテンツを次に置き換えます: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>タスク 7 - アプリケーションを実行します。

このタスクは、テストを**詳細**ビューからアルバムの情報を取得する、**アクションの詳細を**メソッドです。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. プロジェクトを起動、**ホーム**ページ。 URL を変更して **/Store/Details/5**アルバムの情報を確認します。

    ![アルバムの詳細を参照](aspnet-mvc-4-fundamentals/_static/image32.png "アルバムの詳細を参照")

    *アルバムの詳細を参照*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>タスク 8 - ページ間のリンクの追加

このタスクでは、適切な各ジャンル名にリンクがあるストア ビューでリンクを追加します**ストア/参照**URL。 これにより、たとえば、ジャンルをクリックすると**Disco**に移動**ストア/参照? ジャンル Disco を =** URL。

1. Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。 更新プログラム、**インデックス**へのリンクを追加するページ、**参照**ページ。 これを行うで、**ソリューション エクスプ ローラー**展開、**ビュー**フォルダー、**ストア**フォルダーをダブルクリック、 **Index.cshtml**ページ。
2. 選択したジャンルを示す参照ビューへのリンクを追加します。 これを行うには、次の強調表示されたコード内を置き換える、 **&lt;li&gt;** タグ: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > 別の方法としては ページで、次のようなコードに直接リンクは。
   > 
   > &lt;href =&quot;ストア/参照? ジャンル =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > このアプローチでも動作しますが、ハードコーディングされた文字列に依存します。 コント ローラーを後で変更した場合は、この命令を手動で変更する必要があります。 優れた代替手段は、使用する、 **HTML ヘルパー**メソッドです。 ASP.NET MVC には、このようなタスクを使用できる HTML ヘルパー メソッドが含まれています。 **Html.ActionLink()** ヘルパー メソッドでは、容易に HTML を構築する**&lt;、&gt;** リンク、URL パスは、正しく URL エンコードされていることを確認します。
   > 
   > Htlm.ActionLink には、いくつかのオーバー ロードがあります。 この演習では、次の 3 つのパラメーターを受け取るいずれかを使用します。
   > 
   > 1. リンク テキストは、ジャンル名前が表示されます。
   > 2. コント ローラー アクションの名前 (**参照**)
   > 3. ルート名の両方を指定する、パラメーター値 (**ジャンル**) と値 (**ジャンル名**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>タスク 9 - アプリケーションを実行します。

このタスクで各ジャンルに適切なへのリンクが表示されることをテストは**ストア/参照**URL。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して **/格納**を適切な各ジャンルにリンクしていることを確認する**ストア/参照**URL。

    ![[参照] ページへのリンクをジャンルをブラウズ](aspnet-mvc-4-fundamentals/_static/image33.png "と参照ページへのリンクのジャンルの参照")

    *[参照] ページへのリンクをジャンルをブラウズ*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>タスク 10 - の値を渡すを使用して動的 ViewModel コレクション

このタスクでは、モデルの変更を加えずに、コント ローラーとビューの間の値を渡すためのシンプルかつ強力なメソッドを学習します。 ASP.NET MVC 4 は、コレクションを提供します。 &quot;ViewModel&quot;、動的な値に割り当てられコント ローラーとビューも体内でアクセスするには、をします。

一覧を渡す ViewBag 動的コレクションを使用する、今すぐ&quot;**ジャンルの星型**&quot;ビューに、コント ローラーからです。 アクセスは、ストアのインデックス ビュー **ViewModel**し、情報を表示します。

1. Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。 開いている**StoreController.cs**および変更**インデックス**の一覧を作成するメソッドでは、ジャンルを放映 ViewModel コレクションに。

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > 構文を使用することも**ViewBag [&quot;Starred&quot;]** プロパティにアクセスします。
2. 星アイコン**&quot;starred.png&quot;** に含まれる、 **Source\Assets\Images**このラボのフォルダーです。 これをアプリケーションに追加するためにからコンテンツをドラッグして、 **Windows エクスプ ローラー**ウィンドウに、**ソリューション エクスプ ローラー** Visual Web Developer Express、次に示すように。

    ![ソリューションに追加するスター イメージ](aspnet-mvc-4-fundamentals/_static/image34.png "をソリューションに追加するスター イメージ")

    *星の画像をソリューションに追加します。*
3. ビューを開く**Store/Index.cshtml**およびコンテンツを変更します。 読み取りが、&quot;放映&quot;プロパティに、 **ViewBag**コレクション、ジャンルの現在の名前が一覧にかどうかを依頼してください。 その場合は、ジャンル リンクを右星形のアイコンが表示されます。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>タスク 11 - アプリケーションを実行します。

このタスクでは、星印が付いてジャンルが星形のアイコンを表示することをテストします。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. プロジェクトを起動、**ホーム**ページ。 URL を変更して **/格納**おすすめ各ジャンルに重視のラベルがあることを確認します。

    ![星印が付いて要素を持つジャンルをブラウズ](aspnet-mvc-4-fundamentals/_static/image35.png "星印が付いて要素を持つジャンルの参照")

    *星印が付いて要素を持つジャンルをブラウズ*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>手順 7: ASP.NET MVC 4 の新しいテンプレートの周囲膝

この演習では調査、ASP.NET MVC 4 プロジェクト テンプレートの拡張機能を参照して、新しいテンプレートに最も関連する機能を取得します。

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>タスク 1: ASP.NET MVC 4 のインターネット アプリケーション テンプレートの表示

1. まだ開いていない場合は開始**VS Express for Web**
2. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択、 **ASP.NET MVC 4 Web Application**です。 **名前**プロジェクト*MusicStore*し、更新、**ソリューション名**に*開始*、し、場所を選択 (または既定値) をクリック**ok**.

    ![新しい ASP.NET MVC 4 プロジェクトを作成する](aspnet-mvc-4-fundamentals/_static/image36.png "新しい ASP.NET MVC 4 プロジェクトを作成します。")

    *新しい ASP.NET MVC 4 プロジェクトを作成します。*
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログで、選択、**インターネット アプリケーション**プロジェクト テンプレートをクリック**OK**です。 ビュー エンジンとして Razor または ASPX のいずれかを選択できますに注意してください。

    ![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](aspnet-mvc-4-fundamentals/_static/image37.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")

    *新しい ASP.NET MVC 4 インターネット アプリケーションの作成*

    > [!NOTE]
    > ASP.NET MVC 3 razor 構文が導入されました。 その目的は、文字の数および fast およびワークフローをコーディングする流体を有効にすると、ファイルに必要なキーストロークを最小限に抑えるです。 Razor 既存 C #/vb (またはその他) を活用して言語スキルと優れた HTML 構築ワークフローを使用できるテンプレートのマークアップ構文を配信します。
4. キーを押して**f5 キーを押して**にソリューションを実行し、更新されたテンプレートを表示します。 次の機能を確認することができます。

    1. **モダン スタイル テンプレート**

        テンプレートが更新されました、モダン外見の他のスタイルを提供することです。

        ![ASP.NET MVC 4 のスタイルを変更テンプレート](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 のテンプレートのスタイルを変更")

        *ASP.NET MVC 4 のスタイルを変更テンプレート*
    2. **アダプティブ レンダリング**

        チェック アウト、ブラウザー ウィンドウのサイズを変更し、どのページ レイアウトに動的に適応新しいウィンドウのサイズに注意してください。 これらのテンプレートは、アダプティブ レンダリング手法をカスタマイズすることがなく、デスクトップおよびモバイルの両方のプラットフォームで正しく表示するために使用します。

        ![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](aspnet-mvc-4-fundamentals/_static/image39.png "別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート")

        *別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート*
5. デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。
6. これで、ソリューションを探索し、いくつかのプロジェクト テンプレートの ASP.NET MVC 4 で導入された新しい機能を確認することができます。

    ![ASP.NET MVC4 のインターネット-アプリケーション-プロジェクトのテンプレート](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")

    *ASP.NET MVC 4 インターネット アプリケーション プロジェクト テンプレート*

   1. **HTML5 マークアップ**

       テンプレート ビューを開くなど、新しいテーマ マークアップを調べるには参照**About.cshtml**内で表示**ホーム**フォルダーです。

       ![Razor、HTML5 のマークアップを使用して、新しいテンプレート](aspnet-mvc-4-fundamentals/_static/image41.png "Razor、HTML5 のマークアップを使用して、新しいテンプレート")

       *Razor、HTML5 のマークアップを使用して、新しいテンプレート*
   2. **JavaScript ライブラリが含まれています。**

      1. **jQuery**: jQuery HTML ドキュメントを通過する、イベント処理、アニメーション、および Ajax の相互作用を簡略化します。
      2. **jQuery UI**: このライブラリには、低レベルの相互作用、アニメーション、影響の詳細およびテーマを指定ウィジェット、jQuery JavaScript ライブラリ上に構築される抽象クラスが用意されています。

         > [!NOTE]
         > JQuery と jQuery UI について学習できますで[ [ http://docs.jquery.com/ ](http://docs.jquery.com/)](http://docs.jquery.com/)です。
      3. **KnockoutJS**:: ASP.NET MVC 4 の既定のテンプレートが含まれています**KnockoutJS**JavaScript と HTML を使用してリッチと応答性の高い web アプリケーションを作成できる JavaScript MVVM フレームワークです。 ように ASP.NET MVC 3、jQuery、jQuery UI ライブラリも含まれます ASP.NET MVC 4。

          > [!NOTE]
          > このリンクに KnockOutJS ライブラリに関する詳しい情報を入手できます: [ http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)です。
      4. **Modernizr**: HTML5 および css3 用のテクノロジを使用するときに、サイトの古いブラウザーとの互換性を行うこのライブラリが自動的に実行されます。

          > [!NOTE]
          > このリンクに Modernizr ライブラリに関する詳しい情報を入手できます: [ http://www.modernizr.com/](http://www.modernizr.com/)です。
   3. **ソリューションに含まれる SimpleMembership**

       SimpleMembership は、以前の ASP.NET ロールおよびメンバーシップ プロバイダーのシステムの代わりとして設計されています。 容易にするセキュリティで保護された web ページに、開発者の柔軟な方法で多くの新機能があります。

       など、AccountController する準備ができた OAuthWebSecurity (OAuth アカウントの登録、ログイン、管理など) 用と Web セキュリティを使用して、インターネットのテンプレートは、SimpleMembership を統合する点を設定が既に。

       ![ソリューションに含まれる SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership がソリューションに含まれる")

       *ソリューションに含まれる SimpleMembership*

       > [!NOTE]
       > に関する詳細を検索[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) msdn に掲載されています。

> [!NOTE]
> Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを完了して、ASP.NET MVC の基本を学習しました。

- MVC アプリケーションとやり取りする方法の主要な要素
- ASP.NET MVC アプリケーションを作成する方法
- URL とクエリ文字列を通じて渡される追加し、パラメーターを処理するコント ローラーを構成する方法
- 一般的な HTML コンテンツ、外観、および HTML コンテンツを表示するビュー テンプレートを拡張するスタイル シートのテンプレートをセットアップするレイアウトのマスター ページを追加する方法
- ビュー テンプレートのプロパティに渡すため ViewModel パターンを使用して、動的な情報を表示する方法
- ビューのテンプレートでコント ローラーに渡されるパラメーターを使用する方法
- ASP.NET MVC アプリケーション内のページへのリンクを追加する方法
- 追加し、ビューで動的なプロパティを使用する方法
- ASP.NET MVC 4 プロジェクト テンプレートでの機能強化

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-fundamentals/_static/image44.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-fundamentals/_static/image45.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](aspnet-mvc-4-fundamentals/_static/image46.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express Web タイルを*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。

この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル

1. 移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。

    ![Windows Azure ポータルにログオンする](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure ポータルにログオンします。")

    *Windows Azure 管理ポータルにログオンします。*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](aspnet-mvc-4-fundamentals/_static/image49.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-fundamentals/_static/image50.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](aspnet-mvc-4-fundamentals/_static/image51.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](aspnet-mvc-4-fundamentals/_static/image52.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](aspnet-mvc-4-fundamentals/_static/image53.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-fundamentals/_static/image54.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](aspnet-mvc-4-fundamentals/_static/image55.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-fundamentals/_static/image56.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-fundamentals/_static/image58.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](aspnet-mvc-4-fundamentals/_static/image59.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](aspnet-mvc-4-fundamentals/_static/image60.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](aspnet-mvc-4-fundamentals/_static/image61.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](aspnet-mvc-4-fundamentals/_static/image62.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](aspnet-mvc-4-fundamentals/_static/image63.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

   - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者のログイン名を入力します。
   - **パスワード**サーバー管理者のログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。

     ![対象の接続文字列を構成する](aspnet-mvc-4-fundamentals/_static/image64.png "対象の接続文字列を構成します。")

     *対象の接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](aspnet-mvc-4-fundamentals/_static/image65.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](aspnet-mvc-4-fundamentals/_static/image66.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](aspnet-mvc-4-fundamentals/_static/image67.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

    ![アプリケーションが Windows Azure に発行](aspnet-mvc-4-fundamentals/_static/image68.png "アプリケーションが Windows Azure に発行")

    *Windows Azure に発行されるアプリケーション*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-fundamentals/_static/image69.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](aspnet-mvc-4-fundamentals/_static/image70.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-fundamentals/_static/image71.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-fundamentals/_static/image72.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-fundamentals/_static/image73.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-fundamentals/_static/image74.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
