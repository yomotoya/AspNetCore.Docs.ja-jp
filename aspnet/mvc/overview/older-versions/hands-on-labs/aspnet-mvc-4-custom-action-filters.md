---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 カスタム アクション フィルター |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC には前に、または後、アクション メソッドが呼び出されるフィルター処理のロジックを実行するためのアクション フィルターが用意されています。 アクション フィルターは、カスタム属性 tha.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826369"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 カスタム アクション フィルター

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

ASP.NET MVC には前に、または後、アクション メソッドが呼び出されるフィルター処理のロジックを実行するためのアクション フィルターが用意されています。 アクション フィルターは、コント ローラーのアクション メソッドへの事前アクションと事後アクションの動作を追加する宣言型の手段を提供するカスタム属性です。

このハンズオン ラボでは、コント ローラーの要求をキャッチし、データベース テーブルに、サイトのアクティビティ ログに記録する MvcMusicStore ソリューションにカスタム アクション フィルター属性を作成します。 挿入によって、コント ローラーまたはアクションに、ログ記録フィルターを追加することができます。 最後に、訪問者の一覧を示すログ ビューが表示されます。

このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**します。 使用していない場合**ASP.NET MVC**前を経由することを勧めします。 **ASP.NET MVC 4 の基礎**ハンズオン ラボ。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 カスタム アクション フィルター](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)します。

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- フィルター処理機能を拡張するカスタム アクション フィルター属性を作成します。
- 特定のレベルの挿入によって、カスタム フィルター属性を適用します。
- カスタム アクション フィルターをグローバルに登録します。

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

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;します。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の演習では、このハンズオン ラボから成ります。

1. [手順 1: アクションのログ記録](#Exercise1)
2. [手順 2: 複数のアクション フィルターの管理](#Exercise2)

この演習の所要時間を推定: **30 分**します。

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>手順 1: アクションのログ記録

この演習では、ASP.NET MVC 4 のフィルター プロバイダーを使用してカスタム アクションのログ フィルターを作成する方法を学習します。 目的のためには、選択したコント ローラーで、すべてのアクティビティを記録する MusicStore サイトにログ記録フィルターが適用されます。

フィルターを拡張**ActionFilterAttributeClass**オーバーライドと**OnActionExecuting**メソッドを各要求をキャッチし、ログ記録アクションを実行します。 HTTP 要求に関するコンテキスト情報、メソッド、結果、およびパラメーターを実行することが ASP.NET MVC によって**ActionExecutingContext**クラス**します。**

> [!NOTE]
> ASP.NET MVC 4 では、カスタム フィルターを作成せずに使用できる既定のフィルター プロバイダーもあります。 ASP.NET MVC 4 では、次の種類のフィルターを提供します。
> 
> - **承認**認証の実行や要求のプロパティの検証などのアクション メソッドを実行するかどうかに関するセキュリティ上の決定は、これをフィルター処理します。
> - **アクション**フィルターで、アクション メソッドの実行をラップします。 このフィルターは、アクション メソッドに追加のデータの提供、戻り値の検査、アクション メソッドの実行の取り消しなど、追加の処理を実行できます。
> - **結果**フィルターで、ActionResult オブジェクトの実行をラップします。 このフィルターは、HTTP 応答の変更など、結果の追加の処理を実行できます。
> - **例外**フィルターで、結果の実行で始まり、承認フィルターを使用して、アクション メソッドでどこかにスローされた未処理の例外がある場合に実行されます。 例外フィルターは、ログ記録やエラー ページの表示などのタスクで使用できます。
> 
> フィルター プロバイダーの詳細についてはこの MSDN リンクを参照してください: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx))。


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>MVC のミュージック ストア アプリケーションのログ記録機能について

このミュージック ストア ソリューションが、サイトのログ記録を新しいデータ モデル テーブル**ActionLog**、以下のフィールド。 要求、呼び出されるアクション、クライアントの ip アドレス、およびタイムスタンプを受信したコント ローラーの名前。

![データ モデル。ActionLog テーブルです。](aspnet-mvc-4-custom-action-filters/_static/image1.png "データ モデル。ActionLog テーブルです。")

*データ モデル - ActionLog テーブル*

ソリューションにあることができますが、操作ログ用の ASP.NET MVC ビューを提供します**MvcMusicStores/ビュー/ActionLog**:

![アクション ログ ビュー](aspnet-mvc-4-custom-action-filters/_static/image2.png "アクション ログの表示")

*アクション ログの表示*

この構造は、すべての作業が注目コント ローラーの要求を中断することと、カスタム フィルター処理を使用して、ログ記録を実行します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>タスク 1 - コント ローラーの要求をキャッチするカスタム フィルターを作成します。

このタスクは、ログのロジックを含むカスタム フィルター属性クラスを作成します。 目的のためには、ASP.NET MVC を拡張する**ActionFilterAttribute**クラスとそのインターフェイスを実装**IActionFilter**します。

> [!NOTE]
> **ActionFilterAttribute**属性のすべてのフィルターの基本クラスです。 後、コント ローラー アクションの実行前に特定のロジックを実行する次のメソッドを提供します。
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): 直前のアクション メソッドが呼び出されます。
> - **OnActionExecuted**(ActionExecutedContext filterContext): (ビューのレンダリング) の前に、結果が実行される前に、アクション メソッドが呼び出されるとします。
> - **OnResultExecuting**(ResultExecutingContext filterContext): (ビューのレンダリング) の前に、結果が実行される前にだけです。
> - **OnResultExecuted**(ResultExecutedContext filterContext): (後のビューが表示される)、結果の実行後にします。
> 
> これらのメソッドをオーバーライドする派生クラスに、独自のフィルター処理のコードを実行できます。


1. 開く、**開始**ソリューションがある**\Source\Ex01-LoggingActions\Begin**フォルダー。

   1. 続行する前に、いくつか不足している NuGet パッケージをダウンロードする必要があります。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
      > 
      > 詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。
2. 新しい c# クラスを追加、**フィルター**フォルダーと名前を付けます*CustomActionFilter.cs*します。 このフォルダーでは、すべてのカスタム フィルターを格納します。
3. 開いている**CustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。

    (コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. 継承、 **CustomActionFilter**クラス**ActionFilterAttribute**行い**CustomActionFilter**クラス実装**IActionFilter**インターフェイス。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. ように**CustomActionFilter**クラス、メソッドをオーバーライド**OnActionExecuting**フィルターの実行ログを記録するために必要なロジックを追加します。 これを行うには、内の次の強調表示されたコードを追加**CustomActionFilter**クラス。

    (コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting**メソッドを使用して**Entity Framework**新しい ActionLog レジスタを追加します。 作成し、新しいエンティティ インスタンスからコンテキスト情報を設定**filterContext**します。
    > 
    > 詳細をご覧ください**ControllerContext**でクラス[msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)します。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>タスク 2 - ストアのコント ローラー クラスにコードのインターセプターを挿入します。

このタスクでは、すべてのログに記録するコント ローラー アクションとコント ローラー クラスに挿入することによってカスタム フィルターを追加します。 この演習では、ストアのコント ローラー クラスは、ログがあります。

メソッド**OnActionExecuting**から**ActionLogFilterAttribute**挿入された要素が呼び出されたときに、カスタム フィルターが実行されます。

特定のコント ローラー メソッドをインターセプトすることもできます。

1. 開く、 **StoreController**で**MvcMusicStore\Controllers**への参照を追加し、**フィルター**名前空間。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. カスタム フィルターを挿入**CustomActionFilter**に**StoreController**クラスを追加して **[CustomActionFilter]** クラス宣言の前に、の属性。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > フィルターは、コント ローラー クラスに挿入されますが、ときに、すべてのアクションも挿入されます。 一連のアクションに対してのみフィルターを適用するには、挿入する必要があります **[CustomActionFilter]** それらのいずれか。
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションの実行

このタスクでは、ログ記録フィルターが動作しているかをテストします。 アプリケーションを起動して、ストアにアクセスして、ログに記録されたアクティビティを確認します。

1. **F5** キーを押してアプリケーションを実行します。
2. 参照する **/ActionLog**ログ ビューの初期状態を確認します。

    ![ページのアクティビティの前にトラッカーの状態をログ記録](aspnet-mvc-4-custom-action-filters/_static/image3.png "ページ アクティビティの前にトラッカーの状態をログ記録")

    *ページのアクティビティの前にログの追跡ツールの状態*

   > [!NOTE]
   > 既定では、1 つの項目 メニューの既存のジャンルを取得するときに生成された常に表示されます。
   > 
   > 簡略化のためをクリーンアップいたします、 **ActionLog**たびに、アプリケーションが実行されるは、それぞれ特定のタスクの検証のログのみが表示するためのテーブルします。
   > 
   > 次のコードから削除する必要があります、**セッション\_開始**メソッド (で、 **Global.asax**クラス)、ストア内で実行されるすべてのアクションの履歴ログを保存するために、コント ローラー。
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。
4. 参照 **/ActionLog**ログが空のキーを押して**F5**ページを更新します。 訪問者が追跡されなかったことを確認します。

    ![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image4.png "ログに記録するアクティビティを含むアクション ログ")

    *アクティビティ ログに記録を含むアクション ログ*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>手順 2: 複数のアクション フィルターの管理

この演習では、StoreController クラスに 2 つ目のカスタム操作フィルターを追加し、両方のフィルターを実行する特定の順序を定義します。 フィルターをグローバルに登録するコードが更新されます。

フィルターの実行順序を定義するときに考慮するさまざまなオプションがあります。 たとえば、注文プロパティとフィルターのスコープ:

定義することができます、**スコープ**フィルターごとに、たとえば、する可能性がありますスコープ内で実行するすべてのアクション フィルター、**コント ローラーのスコープ**とで実行するすべての承認フィルター**グローバル スコープ**. スコープは定義されている実行順序があります。

また、各アクション フィルターでは、フィルターのスコープ内での実行順序を決定するために使用される順序プロパティがあります。

カスタム アクション フィルターの実行順序の詳細については、この MSDN の記事をご覧ください。 ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx))。

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>タスク 1: 新しいカスタム アクション フィルターの作成

このタスクでには、フィルターの実行順序を管理する方法を学習、StoreController クラスに挿入する新しいカスタム アクション フィルター作成されます。

1. 開く、**開始**ソリューションがある**\Source\Ex02-ManagingMultipleActionFilters\Begin**フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

    1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
    3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

        > [!NOTE]
        > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
        > 
        > 詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。
2. 新しい c# クラスを追加、**フィルター**フォルダーと名前を付けます*MyNewCustomActionFilter.cs*
3. 開いている**MyNewCustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。

    (コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. 既定のクラス宣言を次のコードに置き換えます。

    (コード スニペット - *ASP.NET MVC 4 カスタム アクション フィルター - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > このカスタム アクション フィルターは、前の演習で作成したものよりもほぼ同じです。 主な違いは、持っていること、 *&quot;ログに記録して&quot;* 送信先のフィルターを識別するためにこの新しいクラスの名前で更新された属性には、ログが登録されています。

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>タスク 2: StoreController クラスに新しいコード インターセプターを挿入します。

このタスクでは、StoreController クラスに新しいカスタム フィルターを追加し、どのように両方のフィルターが連携することを確認するソリューションを実行します。

1. 開く、 **StoreController**クラスがある**MvcMusicStore\Controllers**し、新しいカスタム フィルターを注入**MyNewCustomActionFilter**に**StoreController**のようなクラスは、次のコードを示します。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. 次に、これら 2 つのカスタム アクション フィルターの動作を確認するためにアプリケーションを実行します。 これを行うには、キーを押して**F5**と、アプリケーションが開始されるまで待ちます。
3. 参照する **/ActionLog**ログ ビューの初期状態を確認します。

    ![ページのアクティビティの前にトラッカーの状態をログ記録](aspnet-mvc-4-custom-action-filters/_static/image5.png "ページ アクティビティの前にトラッカーの状態をログ記録")

    *ページのアクティビティの前にログの追跡ツールの状態*
4. いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。
5. この時間を確認します。2 回、訪問者が追跡されていた: 後で追加した各カスタム アクション フィルター、 **StorageController**クラス。

    ![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image6.png "ログに記録するアクティビティを含むアクション ログ")

    *アクティビティ ログに記録を含むアクション ログ*
6. ブラウザーを閉じます。

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>タスク 3: フィルターの順序を管理します。

このタスクでは、順序のプロパティを使用して、フィルターの実行順序を管理する方法を学習します。

1. オープン、 **StoreController**クラスがある**MvcMusicStore\Controllers**を指定し、**順序**などの両方のフィルター プロパティ下図のようにします。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. ここで、その順序のプロパティの値に応じて、フィルターを実行する方法を確認します。 表示されますが、最小の順序の値でフィルター (**CustomActionFilter**) が実行される最初の 1 つ。 キーを押して**F5**と、アプリケーションが開始されるまで待ちます。
3. 参照する **/ActionLog**ログ ビューの初期状態を確認します。

    ![ページのアクティビティの前にトラッカーの状態をログ記録](aspnet-mvc-4-custom-action-filters/_static/image7.png "ページ アクティビティの前にトラッカーの状態をログ記録")

    *ページのアクティビティの前にログの追跡ツールの状態*
4. いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。
5. フィルターの順序の値順に、今回は、訪問者が追跡されなかったチェック: **CustomActionFilter**ログの最初。

    ![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image8.png "ログに記録するアクティビティを含むアクション ログ")

    *アクティビティ ログに記録を含むアクション ログ*
6. ここで、フィルターの順序の値を更新し、ログ記録の順序を変更する方法を確認します。 **StoreController**クラスを次に示すようにフィルターの順序の値を更新します。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. キーを押してアプリケーションをもう一度実行**F5**します。
8. いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。
9. によって作成されたログを現時点では、ことを確認**MyNewCustomActionFilter**最初にフィルターが表示されます。

    ![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image9.png "ログに記録するアクティビティを含むアクション ログ")

    *アクティビティ ログに記録を含むアクション ログ*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>タスク 4: をグローバルにフィルターを登録します。

このタスクでは、新しいフィルターを登録するソリューションを更新します (**MyNewCustomActionFilter**) グローバル フィルターとして。 これにより、すべてのアクション実行、アプリケーションと、前のタスクのように StoreController ものだけでなくによってトリガーされます。

1. **StoreController**クラスを削除 **[MyNewCustomActionFilter]** 属性と順序プロパティから **[CustomActionFilter]** します。 次のようになります。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. 開いている**Global.asax**ファイルし、検索、**アプリケーション\_開始**メソッド。 呼び出すことによってグローバル フィルターを登録するたびに、アプリケーションが開始されることに注意してください**RegisterGlobalFilters**メソッド内で**FilterConfig**クラス。

    ![Global.asax でグローバル フィルターを登録する](aspnet-mvc-4-custom-action-filters/_static/image10.png "Global.asax でグローバル フィルターを登録します。")

    *Global.asax でグローバル フィルターを登録します。*
3. 開いている**FilterConfig.cs**内ファイル**アプリ\_開始**フォルダー。
4. System.Web.Mvc; を使用してへの参照を追加します。MvcMusicStore.Filters; を使用します。名前空間。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Update **RegisterGlobalFilters**メソッドは、カスタム フィルターを追加します。 これを行うには、強調表示されたコードを追加します。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. キーを押してアプリケーションを実行**F5**します。
7. いずれかを**ジャンル** メニューから使用可能なアルバムを閲覧など、操作を実行したりします。
8. チェックなった **[MyNewCustomActionFilter]** すぎる HomeController と ActionLogController で挿入されるは。

    ![アクティビティ ログに記録を含むアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image11.png "ログに記録するアクティビティを含むアクション ログ")

    *グローバル アクティビティをログに記録を含むアクション ログ*

> [!NOTE]
> また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボは、カスタム アクションを実行するアクション フィルターを拡張する方法を学習しました。 任意のフィルター、ページのコント ローラーを挿入する方法についても説明しました。 次の概念を使用しました。

- ASP.NET MVC ActionFilterAttribute クラスを使用してカスタム アクション フィルターを作成する方法
- ASP.NET MVC コント ローラーにフィルターを挿入する方法
- フィルターの順序のプロパティを使用して順序を管理する方法
- フィルターをグローバルに登録する方法

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Windows Azure ポータルにログオン](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure ポータルにログオン")

    *Windows Azure 管理ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image18.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。 簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image19.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](aspnet-mvc-4-custom-action-filters/_static/image20.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](aspnet-mvc-4-custom-action-filters/_static/image21.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](aspnet-mvc-4-custom-action-filters/_static/image22.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。

    ![発行プロファイルのダウンロード web サイト](aspnet-mvc-4-custom-action-filters/_static/image23.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](aspnet-mvc-4-custom-action-filters/_static/image24.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image29.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](aspnet-mvc-4-custom-action-filters/_static/image30.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](aspnet-mvc-4-custom-action-filters/_static/image31.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - 新しいデータベース名を入力します。

     ![ターゲットの接続文字列を構成する](aspnet-mvc-4-custom-action-filters/_static/image33.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](aspnet-mvc-4-custom-action-filters/_static/image34.png "データベース文字列を作成します。")

    *データベースの作成*
7. Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image36.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-custom-action-filters/_static/image37.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](aspnet-mvc-4-custom-action-filters/_static/image38.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-custom-action-filters/_static/image39.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-custom-action-filters/_static/image40.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-custom-action-filters/_static/image41.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-custom-action-filters/_static/image42.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
