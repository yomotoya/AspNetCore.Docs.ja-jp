---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "ASP.NET MVC 4 のカスタム アクション フィルター |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET MVC には、事前にまたはアクション メソッドが呼び出された後にフィルタ リング ロジックを実行するためのアクション フィルターが用意されています。 アクション フィルターは、カスタム属性 tha しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 103cd68c576463d87d0077cc149f9b89c6e028e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 のカスタム アクション フィルター
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> ASP.NET MVC には、事前にまたはアクション メソッドが呼び出された後にフィルタ リング ロジックを実行するためのアクション フィルターが用意されています。 アクション フィルターは、コント ローラーのアクション メソッドにアクション前とアクション後の動作を追加する宣言型の手段を提供するカスタム属性です。
> 
> このハンズオン ラボでは、コント ローラーの要求をキャッチし、データベース テーブルに、サイトの操作をログ記録を MvcMusicStore ソリューションにカスタム アクション フィルター属性を作成します。 任意のコント ローラーまたはアクションをインジェクションして、ログ記録フィルターを追加することができます。 最後に、ユーザーの一覧を示すログ ビューが表示されます。
> 
> > [!NOTE]
> > このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。 使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC 4 基礎**ハンズオン ラボ。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- フィルター処理機能を拡張するカスタム アクション フィルター属性を作成します。
- 特定のレベルにインジェクションでカスタム フィルター属性を適用します。
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

便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。 実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

次の演習では、このハンズオン ラボから成ります。

1. [手順 1: アクションのログ記録](#Exercise1)
2. [手順 2: 複数のアクション フィルターの管理](#Exercise2)

この演習を完了する時間を推定: **30 分**です。

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>手順 1: アクションのログ記録

この演習では、ASP.NET MVC 4 のフィルター プロバイダーを使用して、カスタム アクション ログ フィルターを作成する方法を学習します。 目的とするログ記録フィルターを選択したコント ローラーのすべてのアクティビティが記録 MusicStore サイトに適用されます。

フィルターが拡張する**ActionFilterAttributeClass**オーバーライドと**OnActionExecuting**メソッドを各要求をキャッチして、ログ記録のアクションを実行します。 HTTP 要求に関するコンテキスト情報は ASP.NET MVC によって提供されるメソッド、結果、およびパラメーターの実行は**ActionExecutingContext**クラス**です。**

> [!NOTE]
> ASP.NET MVC 4 では、カスタム フィルターを作成せずに使用できる既定のフィルター プロバイダーもあります。 ASP.NET MVC 4 には、次の種類のフィルターが用意されています。
> 
> - **承認**認証の実行や要求のプロパティを検証するなど、アクション メソッドを実行するかどうかに関するセキュリティ上の決定は、これをフィルター処理します。
> - **アクション**フィルター、アクション メソッドの実行をラップします。 このフィルターは、アクション メソッドに余分なデータを提供して、戻り値の検査、アクション メソッドの実行の取り消しなど、追加の処理を実行できます。
> - **結果**ActionResult オブジェクトの実行をラップするフィルター。 このフィルターは、HTTP 応答の変更など、結果の追加の処理を実行できます。
> - **例外**フィルターで、結果の実行で承認フィルターを使用して開始および終了、アクション メソッドでスローどこかにハンドルされない例外がある場合に実行します。 例外フィルターは、ログ記録やエラー ページの表示などのタスクで使用できます。
> 
> フィルター プロバイダーの詳細については、この MSDN リンクを参照してください: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx))。


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>MVC 音楽ストア アプリケーションのログ記録機能について

この Music Store ソリューションが、サイトのログ記録用に新しいデータ モデル テーブル**ActionLog**、次のフィールドを持つ: 要求、呼び出されるアクション、クライアントの ip アドレス、およびタイムスタンプを受信するコント ローラーの名前。

![データ モデル。ActionLog テーブルです。](aspnet-mvc-4-custom-action-filters/_static/image1.png "データ モデル。ActionLog テーブルです。")

*データ モデルの ActionLog テーブル*

ソリューションに含まれているアクション ログの ASP.NET MVC ビューを提供する**MvcMusicStores/ビュー/ActionLog**:

![アクション ログ ビュー](aspnet-mvc-4-custom-action-filters/_static/image2.png "アクション ログの表示")

*アクション ログの表示*

この構造体を指定するには、すべての作業はコント ローラーの要求を中断することと、カスタム フィルターを使用して、ログ記録の実行にフォーカスします。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>タスク 1 - コント ローラーの要求をキャッチするカスタム フィルターを作成します。

このタスクでは、ログのロジックを格納するカスタム フィルター属性クラスを作成します。 ASP.NET MVC を拡張する目的に**ActionFilterAttribute**クラスとインターフェイスを実装する**IActionFilter**です。

> [!NOTE]
> **ActionFilterAttribute**すべての属性フィルターの基本クラスです。 後、コント ローラー アクションの実行前に、特定のロジックを実行する次のメソッドを提供します。
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): 操作の直前にメソッドが呼び出されます。
> - **OnActionExecuted**(ActionExecutedContext filterContext): (ビューのレンダリング) の前に、結果が実行される前に、アクション メソッドが呼び出された後にします。
> - **OnResultExecuting**(ResultExecutingContext filterContext): (ビューのレンダリング) の前に、結果が実行される前に、だけです。
> - **OnResultExecuted**(ResultExecutedContext filterContext): (ビューが表示される) 後に結果が実行された後にします。
> 
> 派生クラスには、これらのメソッドをオーバーライドすることでは、独自のフィルター処理コードを実行できます。


1. 開く、**開始**ソリューションにある**\Source\Ex01-LoggingActions\Begin**フォルダーです。

    1. 続行する前に、いくつか不足している NuGet パッケージをダウンロードする必要があります。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

    > [!NOTE]
    > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
    > 
    > 詳細については、この記事を参照してください: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。
2. 新しい c# クラスを追加、**フィルター**フォルダーし名前を付けます*CustomActionFilter.cs*です。 このフォルダーでは、すべてのカスタム フィルターを格納します。
3. 開いている**CustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。

    (コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. 継承、 **CustomActionFilter**クラス**ActionFilterAttribute**し**CustomActionFilter**クラス実装**IActionFilter**インターフェイスです。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. ください**CustomActionFilter**クラス、メソッドをオーバーライド**OnActionExecuting**フィルターの実行ログを記録するために必要なロジックを追加します。 これを行うには、次の強調表示されたコード内での追加**CustomActionFilter**クラスです。

    (コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting**メソッドを使用して**Entity Framework**新しい ActionLog レジスタを追加します。 作成してからコンテキスト情報を含むエンティティの新しいインスタンスを塗りつぶします**filterContext**です。
    > 
    > に関する詳細を読み取ることができます**ControllerContext**でクラス[msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)です。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>タスク 2 - ストアのコント ローラー クラスにコード インターセプターを挿入します。

このタスクでは、すべてのコント ローラー クラスおよび記録されるコント ローラーのアクションを挿入し、カスタム フィルターを追加します。 この演習では、ログが、ストアのコント ローラー クラスになります。

メソッド**OnActionExecuting**から**ActionLogFilterAttribute**挿入された要素が呼び出されたときに、カスタム フィルターが実行されます。

特定のコント ローラーのメソッドをインターセプトすることもできます。

1. 開く、 **StoreController**で**MvcMusicStore\Controllers**への参照を追加し、**フィルター**名前空間。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. カスタム フィルターを挿入**CustomActionFilter**に**StoreController**クラスを追加して**[CustomActionFilter]**クラス宣言の前に、の属性です。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > フィルターは、コント ローラー クラスに組み込まれてと、そのすべてのアクションは挿入もします。 一連のアクションに対してのみフィルターを適用するには、挿入する必要があります**[CustomActionFilter]**それらのいずれか。
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションを実行します。

このタスクでは、ログ記録フィルターが機能しているかをテストします。 アプリケーションを起動して、ストアにアクセスしようし、ログに記録されたアクティビティはチェックします。

1. **F5** キーを押してアプリケーションを実行します。
2. 参照**/ActionLog**ログ ビューの初期状態を確認します。

    ![ログの追跡ツールの状態 ページのアクティビティの前に](aspnet-mvc-4-custom-action-filters/_static/image3.png "ログの追跡ツールの状態 ページのアクティビティの前に")

    *ページのアクティビティの前にログの追跡ツールの状態*

    > [!NOTE]
    > 既定では、1 つの項目、メニューの既存のジャンルを取得するときに生成された常に表示されます。
    > 
    > クリーンアップおわかりやすくするための目的で、 **ActionLog**たびに、アプリケーションが実行されるは、それぞれ特定のタスクの検証のログのみが表示するためのテーブルです。
    > 
    > 次のコードを削除する必要がありますが、**セッション\_開始**メソッド (で、 **Global.asax**クラス)、ストア内で実行されるすべての操作の履歴ログを保存するのにはコント ローラー。
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。
4. 参照**/ActionLog**され、ログが空のキーを押して**f5 キーを押して**ページを更新します。 訪問が追跡されなかったことを確認します。

    ![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image4.png "ログに記録するアクティビティのアクション ログ")

    *ログ記録アクティビティのアクション ログ*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>手順 2: 複数のアクション フィルターの管理

この演習では、StoreController クラスに 2 番目のカスタム操作フィルターを追加し、両方のフィルターを実行する特定の順序を定義します。 フィルターをグローバルに登録するコードが更新されます。

さまざまなオプションを考慮に入れるフィルターの実行順序を定義するときにします。 たとえば、Order プロパティと、フィルターのスコープ:

定義することができます、**スコープ**フィルターごとに、たとえば、でしたスコープを設定する内で実行するすべてのアクション フィルター、**コント ローラーのスコープ**とで実行するすべての承認フィルター**グローバル スコープ**. スコープでは、定義済みの実行順序があります。

さらに、各アクション フィルターでは、フィルターのスコープ内の実行順序を決定するために使用する順序プロパティがいます。

カスタム アクション フィルターの実行順序の詳細については、この MSDN の記事をご覧ください: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)) です。

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>タスク 1: 新しいカスタム アクション フィルターの作成

このタスクは作成 StoreController クラスに挿入する新しいカスタム アクション フィルターのフィルターの実行順序を管理する方法を学習します。

1. 開く、**開始**ソリューションにある**\Source\Ex02-ManagingMultipleActionFilters\Begin**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

    1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

        > [!NOTE]
        > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
        > 
        > 詳細については、この記事を参照してください: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。
2. 新しい c# クラスを追加、**フィルター**フォルダーし名前を付けます*MyNewCustomActionFilter.cs*
3. 開いている**MyNewCustomActionFilter.cs**への参照を追加および**System.Web.Mvc**と**MvcMusicStore.Models**名前空間。

    (コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. 既定のクラス宣言を次のコードに置き換えます。

    (コード スニペットの*ASP.NET MVC 4 のカスタム アクション フィルター - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > このカスタム アクション フィルターは、前述の手順で作成されたものよりもほぼ同じです。 主な違いがある、 *&quot;によってログに記録&quot;*状態フィルターを識別するこの新しいクラスの名前で更新された属性には、ログが登録されています。

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>タスク 2: StoreController クラスに新しいコード インターセプターを挿入します。

このタスクでは、StoreController クラスに新しいカスタム フィルターを追加し、どのように両方のフィルターが共同作業を確認するようにソリューションを実行します。

1. 開く、 **StoreController**クラスにある**MvcMusicStore\Controllers**し、新しいカスタム フィルターを挿入**MyNewCustomActionFilter**に**StoreController**ようなクラスが、次のコードに示すようにします。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. ここで、これら 2 つのカスタム アクション フィルターの動作を確認するためにアプリケーションを実行します。 これを行うには、キーを押して**f5 キーを押して**あり、アプリケーションが開始されるまで待機します。
3. 参照**/ActionLog**ログ ビューの初期状態を確認します。

    ![ログの追跡ツールの状態 ページのアクティビティの前に](aspnet-mvc-4-custom-action-filters/_static/image5.png "ログの追跡ツールの状態 ページのアクティビティの前に")

    *ページのアクティビティの前にログの追跡ツールの状態*
4. いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。
5. です。 この時間を確認します。訪問履歴の管理が 2 回されていた: 内の各カスタム アクション フィルターを追加した後、 **StorageController**クラスです。

    ![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image6.png "ログに記録するアクティビティのアクション ログ")

    *ログ記録アクティビティのアクション ログ*
6. ブラウザーを閉じます。

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>タスク 3: フィルターの順序を管理します。

このタスクでは、これらの順序を使用してフィルターの実行順序を管理する方法を学習します。

1. 開く、 **StoreController**クラスにある**MvcMusicStore\Controllers**を指定し、**順序**などのプロパティのフィルターの両方で次に示すようにします。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. ここで、その Order プロパティの値に応じて、フィルターを実行する方法を確認します。 最小の注文の値を含むフィルターと考えることが (**CustomActionFilter**) が実行される最初の 1 つです。 キーを押して**f5 キーを押して**あり、アプリケーションが開始されるまで待機します。
3. 参照**/ActionLog**ログ ビューの初期状態を確認します。

    ![ログの追跡ツールの状態 ページのアクティビティの前に](aspnet-mvc-4-custom-action-filters/_static/image7.png "ログの追跡ツールの状態 ページのアクティビティの前に")

    *ページのアクティビティの前にログの追跡ツールの状態*
4. いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。
5. フィルターの順序の値順にチェックを現時点では、訪問が追跡されなかった: **CustomActionFilter**ログの最初。

    ![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image8.png "ログに記録するアクティビティのアクション ログ")

    *ログ記録アクティビティのアクション ログ*
6. ここで、フィルターの順序の値を更新し、ログの順序がどのように変化するかを確認します。 **StoreController**クラス、次に示すように、フィルターの順序の値を更新します。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. キーを押してアプリケーションを再実行**f5 キーを押して**です。
8. いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。
9. によって作成されたログを現時点では、ことを確認して**MyNewCustomActionFilter**フィルターが最初に表示します。

    ![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image9.png "ログに記録するアクティビティのアクション ログ")

    *ログ記録アクティビティのアクション ログ*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>タスク 4: をグローバル フィルターの登録

このタスクでは、新しいフィルターを登録するようにソリューションを更新します (**MyNewCustomActionFilter**) グローバル フィルターとして。 これにより、これはすべての操作実行前のタスクと同様に StoreController ものだけでなくと、アプリケーションでによってトリガーされます。

1. **StoreController**クラス、削除**[MyNewCustomActionFilter]**属性と元の order プロパティ**[CustomActionFilter]**です。 次のようになります。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. 開いている**Global.asax**ファイルし、検索、**アプリケーション\_開始**メソッドです。 呼び出して各 thime アプリケーションが開始されることに注意してくださいがグローバル フィルターを登録する**RegisterGlobalFilters**メソッド内で**FilterConfig**クラスです。

    ![Global.asax でグローバル フィルターを登録する](aspnet-mvc-4-custom-action-filters/_static/image10.png "Global.asax でグローバル フィルターを登録します。")

    *Global.asax でグローバル フィルターを登録します。*
3. 開いている**FilterConfig.cs**内でファイル**アプリ\_開始**フォルダーです。
4. System.Web.Mvc; を使用してへの参照を追加します。MvcMusicStore.Filters; を使用します。名前空間です。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. 更新**RegisterGlobalFilters**メソッドは、カスタム フィルターを追加します。 これを行うには、強調表示されたコードを追加します。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. キーを押してアプリケーションを実行**f5 キーを押して**です。
7. いずれかをクリックして、**ジャンル** メニューから使用可能なアルバムを参照するように、操作を実行したりします。
8. チェックされている**[MyNewCustomActionFilter]**すぎる HomeController および ActionLogController で挿入するがします。

    ![ログ記録アクティビティのアクション ログ](aspnet-mvc-4-custom-action-filters/_static/image11.png "ログに記録するアクティビティのアクション ログ")

    *ログに記録するグローバルのアクティビティのアクション ログ*

> [!NOTE]
> Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを完了して、カスタム アクションを実行するアクション フィルターを拡張する方法を学習しました。 任意のページ コント ローラーのフィルターを挿入する方法も学習しました。 次の概念が使われていました。

- ASP.NET MVC ActionFilterAttribute クラスを使用したカスタム アクション フィルターを作成する方法
- ASP.NET MVC のコント ローラーにフィルターを挿入する方法
- フィルターの順序のプロパティを使用して順序を管理する方法
- フィルターをグローバルに登録する方法

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Windows Azure ポータルにログオンする](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure ポータルにログオンします。")

    *Windows Azure 管理ポータルにログオンします。*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image18.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-custom-action-filters/_static/image19.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](aspnet-mvc-4-custom-action-filters/_static/image20.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](aspnet-mvc-4-custom-action-filters/_static/image21.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](aspnet-mvc-4-custom-action-filters/_static/image22.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-custom-action-filters/_static/image23.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](aspnet-mvc-4-custom-action-filters/_static/image24.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image29.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](aspnet-mvc-4-custom-action-filters/_static/image30.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](aspnet-mvc-4-custom-action-filters/_static/image31.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

    - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。
    - **ユーザー名**サーバー管理者のログイン名を入力します。
    - **パスワード**サーバー管理者のログイン パスワードを入力します。
    - 新しいデータベース名を入力します。

    ![対象の接続文字列を構成する](aspnet-mvc-4-custom-action-filters/_static/image33.png "対象の接続文字列を構成します。")

    *対象の接続文字列を構成します。*
6. 次に、 **[OK]**をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](aspnet-mvc-4-custom-action-filters/_static/image34.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]**をクリックします。

    ![SQL データベースを指す接続文字列](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](aspnet-mvc-4-custom-action-filters/_static/image36.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-custom-action-filters/_static/image37.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](aspnet-mvc-4-custom-action-filters/_static/image38.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-custom-action-filters/_static/image39.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-custom-action-filters/_static/image40.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-custom-action-filters/_static/image41.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-custom-action-filters/_static/image42.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
