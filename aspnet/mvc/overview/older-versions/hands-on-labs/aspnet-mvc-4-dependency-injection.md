---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 の依存関係の挿入 |Microsoft ドキュメント
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC と ASP.NET MVC 4 のフィルターの基本的な知識がある前提としています。 場合は使用していないする前に、ASP.NET MVC 4 フィルター推奨しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 の依存関係の挿入

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**と**ASP.NET MVC 4 フィルター**です。 使用していない場合**ASP.NET MVC 4 フィルター**経由で移動する前をお勧め**ASP.NET MVC のカスタム アクション フィルター**ハンズオン ラボ。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。 このラボに固有のプロジェクトは[ASP.NET MVC 4 依存性の注入](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)です。

**オブジェクト指向プログラミング**パラダイム、オブジェクトが連携コラボレーション モデルでの寄稿者およびコンシューマーがある場所です。 必然的に、この通信モデルは、オブジェクトおよび複雑さが増すときに、管理するが困難になるコンポーネント間の依存関係を生成します。

![クラスの依存関係と複雑さをモデル化](aspnet-mvc-4-dependency-injection/_static/image1.png "クラスの依存関係と複雑さをモデル化")

*クラスの依存関係とモデルの複雑さ*

知らされた可能性があります、**ファクトリ パターン**とインターフェイスと、クライアント オブジェクトがある多くの場合、サービスの場所を担当するサービスを使用する実装の間の分離。

依存関係の挿入パターンは、制御の反転の特定の実装です。 **反転 (IoC) のコントロールの**オブジェクトが依存している仕事をするその他のオブジェクトを作成しないことを意味します。 代わりに、外部ソース (たとえば、xml 構成ファイル) に必要なオブジェクトを取得します。

**依存関係の挿入 (DI)**つまりこれは通常はコンス トラクターのパラメーターを渡す framework コンポーネントによって、オブジェクトの介入なし行われます、プロパティを設定します。

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>依存関係挿入 (DI) デザイン パターン

大まかに言えば、依存関係の挿入の目的は、するクライアント クラス (例: *、ゴルファー*) インターフェイスを満たしているものが必要 (例: *IClub*)。 具象型を気に (例: *WoodClub、IronClub、WedgeClub*または*PutterClub*)、他のユーザーを処理する必要がある (適切な例:*キャディ*)。 ASP.NET MVC で依存関係競合回避モジュールは依存関係のロジックを別の場所を登録することを許可することができます (例: コンテナーまたは*クラブのバッグ*)。

![依存関係の挿入ダイアグラム](aspnet-mvc-4-dependency-injection/_static/image2.png "依存関係の挿入の図")

*依存関係の挿入のゴルフとの類似性*

次に示します依存性の注入パターンと制御の反転を使用する利点があります。

- クラス結合度が減少します。
- コードの再利用を向上します。
- コードの保守性を向上します。
- アプリケーションのテストが向上します。

> [!NOTE]
> 依存関係の挿入が抽象のファクトリ デザイン パターンと比較場合もありますが、両方のアプローチのわずかな違いがあります。 DI では、背後にあるが、ファクトリと、登録済みサービスを呼び出すことによって依存関係の解決に取り組んでフレームワークがあります。


依存関係挿入パターンを理解する方法について取り上げますこの実習で ASP.NET MVC 4 で適用します。 依存関係の挿入での使用を開始するが、**コント ローラー**データベース アクセス サービスを含めることです。 依存関係の挿入を適用する次に、**ビュー**サービスを使用すると、情報を表示します。 最後に、ソリューション内のカスタム アクション フィルターを提供する ASP.NET MVC 4 フィルターに、DI が拡張されます。

このハンズオン ラボでは学習する方法。

- 依存関係の挿入が NuGet パッケージを使用するために Unity と ASP.NET MVC 4 を統合します。
- ASP.NET MVC のコント ローラー内の依存関係の挿入を使用します。
- ASP.NET MVC ビュー内の依存関係の挿入を使用します。
- ASP.NET MVC アクション フィルター内の依存関係の挿入を使用します。

> [!NOTE]
> このラボが Unity.Mvc3 NuGet パッケージを使用して、依存関係の解決ですが、ASP.NET MVC 4 を使用する任意の依存関係挿入フレームワークを適用すること。


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

次の演習では、このハンズオン ラボから成ります。

1. [手順 1: コント ローラーの挿入](#Exercise1)
2. [手順 2: ビューを挿入します。](#Exercise2)
3. [手順 3: フィルターを提供します。](#Exercise3)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **30 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>手順 1: コント ローラーの挿入

この演習では、NuGet パッケージを使用する Unity の統合により、ASP.NET MVC のコント ローラーで依存関係の挿入を使用する方法を学習します。 そのため、サービスをデータ アクセス ロジックを区別するための MvcMusicStore コント ローラーに含まれます。 サービスは、コント ローラーのコンス トラクターでのヘルプで依存関係の挿入を使用して決定される新しい依存関係を作成**Unity**です。

この方法では、小さい結合であるアプリケーションより柔軟かつ容易に維持し、テストを生成する方法を示します。 ASP.NET MVC を Unity に統合する方法も学習します。

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>StoreManager サービスについて

今すぐ開始ソリューションで提供されている MVC 音楽ストアには、という名前のストアのコント ローラー データを管理するサービスが含まれています。 **StoreService**です。 次に示す、ストア サービスの実装が表示されます。 すべてのメソッドがモデルのエンティティを返すことに注意してください。

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController**ソリューション、begin から消費今すぐ**StoreService**です。 すべてのデータの参照が削除された**StoreController**を使用する任意の方法を変更することがなく、現在のデータ アクセス プロバイダーを変更するようになりましたことも、 **StoreService**です。

次に表示されます、 **StoreController**実装との依存関係を持つ**StoreService**クラスのコンス トラクター内です。

> [!NOTE]
> この演習で導入された依存関係に関連する**制御の反転**(IoC)。
> 
> **StoreController**クラスのコンス トラクターを受け取る、 **IStoreService**型のパラメーターは、クラス内からサービスの呼び出しを実行することは不可欠です。 ただし、 **StoreController**は任意のコント ローラーが必要な ASP.NET MVC を使用する (パラメーターなし) での既定コンス トラクターを実装していません。
> 
> 依存関係を解決するには、コント ローラーは、抽象ファクトリ (を指定した型の任意のオブジェクトを返すクラス) によって作成されるがします。


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> クラスが宣言されているパラメーターなしのコンス トラクターが存在しないために、サービス オブジェクトを送信せず、StoreController を作成しようとするときにエラーが表示されます。


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>タスク 1 - アプリケーションを実行します。

このタスクには、サービスを含む、アプリケーション ロジックからデータ アクセスを分離するストア コント ローラーに Begin アプリケーションが実行されます。

アプリケーションを実行するときに、コント ローラー サービスが既定でのパラメーターとして渡されないと、例外が表示されます。

1. 開く、**開始**にソリューションがある**Source\Ex01 送り込んで Controller\Begin**です。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. キーを押して**ctrl キーを押しながら f5 キーを押して**デバッグを使わないアプリケーションを実行します。 エラー メッセージが表示されます&quot;**パラメーターなしのコンス トラクターはこのオブジェクトに対して定義されている**&quot;:

    ![ASP.NET MVC 開始アプリケーションの実行中にエラー](aspnet-mvc-4-dependency-injection/_static/image3.png "ASP.NET MVC 開始アプリケーションの実行中にエラー")

    *ASP.NET MVC 開始アプリケーションの実行中にエラー*
3. ブラウザーを閉じます。

次の手順では、このコント ローラーが必要な依存関係を挿入する音楽ストア ソリューションで動作します。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>タスク 2 - MvcMusicStore ソリューションに Unity を含む

このタスクでに含めます**Unity.Mvc3** NuGet パッケージをソリューションにします。

> [!NOTE]
> ASP.NET MVC 3 の Unity.Mvc3 パッケージの設計ですが、ASP.NET MVC 4 と完全な互換性。
> 
> Unity では、インスタンスのサポートが省略可能な軽量で拡張性のある依存性の注入コンテナーは、し、インターセプションを入力します。 これは、あらゆる種類の .NET アプリケーションで使用する汎用的なコンテナーです。 などの依存関係の挿入メカニズムで見つかったすべての一般的な機能を提供します。 オブジェクトの作成、コンテナーにコンポーネントの構成を延期すると、ランタイムと、柔軟性の依存関係を指定することによって要件の抽象化します。


1. インストール**Unity.Mvc3**での NuGet パッケージ、 **MvcMusicStore**プロジェクト。 これを行うには、開く、 **Package Manager Console**から**ビュー** | **その他のウィンドウ**します。
2. 次のコマンドを実行します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Unity.Mvc3 NuGet パッケージをインストールする](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet パッケージをインストールします。")

    *Unity.Mvc3 NuGet パッケージをインストールします。*
3. 1 回、 **Unity.Mvc3**パッケージがインストールされている、ファイルおよび Unity の構成を簡略化するために自動的に追加フォルダーを表示します。

    ![インストールされている Unity.Mvc3 パッケージ](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 パッケージがインストールされています。")

    *Unity.Mvc3 パッケージがインストールされています。*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>タスク 3 - Global.asax.cs アプリケーションで登録する Unity\_開始

このタスクを更新、**アプリケーション\_開始**にメソッドがある**Global.asax.cs** Unity ブートス トラップ初期化子を呼び出すし、次に、ブートス トラップ ファイルの登録を更新するにはサービスとコント ローラーは、依存関係の挿入に使用されます。

1. ここで、これは、Unity のコンテナーを初期化するファイル ブートス トラップと依存関係競合回避モジュールをフックするされます。 これを行うには、開く**Global.asax.cs**内で強調表示されている次のコードを追加し、**アプリケーション\_開始**メソッドです。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex01 - 初期化 Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. 開いている**Bootstrapper.cs**ファイル。
3. 次の名前空間を含める: **MvcMusicStore.Services**と**MusicStore.Controllers**です。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex01 - ブートス トラップの名前空間を追加する*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. 置き換える**BuildUnityContainer**メソッドのコンテンツ ストア コント ローラーとストア サービスを登録する次のコードにします。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex01 - 登録ストア コント ローラーとサービス*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションを実行します。

このタスクを確認するようになりました読み込むことができます Unity を含めた後でアプリケーションを実行します。

1. キーを押して**f5 キーを押して**アプリケーションを実行する、アプリケーションは、エラー メッセージを表示することがなく読み込むようになりました必要があります。

    ![依存関係の挿入でアプリケーションを実行している](aspnet-mvc-4-dependency-injection/_static/image6.png "アプリケーション依存関係の挿入を実行")

    *依存関係の挿入を実行中のアプリケーション*
2. 参照**/格納**です。 これを呼び出す**StoreController**を使用してこれは今すぐ作成**Unity**です。

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC 音楽ストア")

    *MVC 音楽ストア*
3. ブラウザーを閉じます。

次の演習では、ASP.NET MVC ビューとアクション フィルター内で使用する依存関係の挿入のスコープを拡張する方法を学習します。

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>手順 2: ビューを挿入します。

この演習では、Unity の統合を ASP.NET MVC 4 の新機能を含むビューの依存関係の挿入を使用する方法を学習します。 これを実行するためには、内部、ストア ビューの参照をメッセージと下の画像を表示するカスタム サービスを呼び出します。

次に、unity プロジェクトを統合し、依存関係を挿入するカスタムの依存関係リゾルバーを作成します。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>タスク 1 - サービスを使用するビューを作成します。

このタスクでは、新しい依存関係を生成するサービスの呼び出しを実行するビューを作成します。 このソリューションに含まれる単純なメッセージング サービスで、サービスが構成されます。

1. 開く、**開始**にソリューションがある、 **Source\Ex02 送り込んで View\Begin**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
      > 
      > 詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。
2. 含める、 **MessageService.cs**と**IMessageService.cs**クラスに格納、 **\Assets をソース**フォルダーに**/サービス**です。 これを行うを右クリックして**Services**フォルダーと選択**既存項目の追加**です。 ファイルの場所を参照し、それらを含めます。

    ![メッセージのサービスとサービスのインターフェイスを追加する](aspnet-mvc-4-dependency-injection/_static/image8.png "メッセージ サービスとサービスのインターフェイスを追加します。")

    *サービス インターフェイスと追加のメッセージ サービス*

    > [!NOTE]
    > **IMessageService**インターフェイスによって実装される 2 つのプロパティを定義する、 **MessageService**クラスです。 これらのプロパティ -**メッセージ**と**ImageUrl**-を表示するには、メッセージとイメージの URL を格納します。
3. フォルダーを作成して**ページ/**プロジェクトのルート フォルダー、および既存のクラスを追加**MyBasePage.cs**から**Source\Assets**です。 継承する基本ページには、次のような構造があります。

    ![ページ フォルダー](aspnet-mvc-4-dependency-injection/_static/image9.png "ページ フォルダー")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. 開いている**Browse.cshtml**から表示**/ビュー/ストア**フォルダーから継承して**MyBasePage.cs**です。

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. **参照**ビューで、呼び出しを追加して**MessageService**イメージと、サービスによって取得したメッセージを表示します。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>タスク 2 - カスタム依存関係競合回避モジュールとカスタム ビュー ページ アクティベーターを含む

前のタスクでは、その中のサービス呼び出しを実行するためのビュー内の新しい依存関係を挿入します。 ASP.NET MVC 依存性の注入インターフェイスを実装する依存関係を解決するようになりました、 **IViewPageActivator**と**IDependencyResolver**です。 実装ソリューションに含めるは**IDependencyResolver** Unity を使用して、サービスの取得を扱うことができます。 次の別のカスタム実装を含めますが**IViewPageActivator**インターフェイス ビューの作成を解決するためです。

> [!NOTE]
> ASP.NET MVC 3 から依存関係の挿入の実装にサービスを登録するインターフェイスが簡略化されます。 **IDependencyResolver**と**IViewPageActivator**依存関係の挿入の ASP.NET MVC 3 の機能の一部であります。
> 
> **-IDependencyResolver**インターフェイスには、以前の IMvcServiceLocator が置き換えられます。 IDependencyResolver の実装では、サービスまたはサービスのコレクションのインスタンスを返す必要があります。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator**インターフェイスには、依存関係の挿入を使用してページの表示がインスタンス化する方法より詳細に制御が用意されています。 実装するクラス**IViewPageActivator**インターフェイスは、コンテキスト情報を使用してビューのインスタンスを作成できます。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. 作成、/**ファクトリ**プロジェクトのルート フォルダー内のフォルダーです。
2. 含める**CustomViewPageActivator.cs**からソリューションに**ソース資産//**に**ファクトリ**フォルダーです。 右クリックし、 **/Factories**フォルダーを選択**追加 |既存の項目**し、 **CustomViewPageActivator.cs**です。 このクラスは、実装、 **IViewPageActivator** Unity コンテナーを保持するインターフェイスです。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** Unity コンテナーを使用して、ビューの作成の管理を担当します。
3. 含める**UnityDependencyResolver.cs**ファイルから**ソース/資産**に**/Factories**フォルダーです。 右クリックし、 **/Factories**フォルダーを選択**追加 |既存の項目**し、 **UnityDependencyResolver.cs**ファイル。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver**クラスは、Unity のカスタム DependencyResolver です。 Unity コンテナー内のサービスが見つからない場合は、ベースの競合回避モジュールが invocated です。

次の実習で両方の実装は、サービスと、ビューの場所を知っているモデルに登録されます。

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>タスク 3 - Unity コンテナー内の依存関係の挿入の登録

このタスクでは、依存関係の挿入操作にまとめる前、すべての処理を格納します。

これまでは、ソリューションは、次の要素に。

- A**参照**ビューから継承する**MyBaseClass**消費**MessageService**です。
- 中間クラス -**MyBaseClass**-依存関係の挿入、サービス インターフェイスの宣言を含むです。
- サービス - **MessageService** - とそのインターフェイス**IMessageService**です。
- Unity でのカスタムの依存関係リゾルバー **UnityDependencyResolver** -サービスの取得を処理します。
- ビュー ページ アクティベーター - **CustomViewPageActivator**のページを作成します。

挿入する**参照**ビュー、今すぐ登録する、カスタムの依存関係競合回避モジュール Unity コンテナーにします。

1. 開いている**Bootstrapper.cs**ファイル。
2. インスタンスを登録**MessageService**サービスを初期化するために Unity コンテナーに。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex02 - 登録メッセージ サービス*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. 参照を追加**MvcMusicStore.Factories**名前空間。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex02 - ファクトリ Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. 登録**CustomViewPageActivator**として Unity コンテナーにビュー ページ アクティベーター。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex02 - 登録 CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. インスタンスと ASP.NET MVC 4 の既定の依存関係競合回避モジュールを交換して**UnityDependencyResolver**です。 これを行うには、置換**Initialise**メソッドを次のコード コンテンツ。

    (コード スニペットの*ASP.NET の依存関係インジェクション ラボ - Ex02 - 更新プログラムの依存関係競合回避モジュール*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC には、既定の依存関係競合回避モジュールのクラスが用意されています。 Unity に対して作成したものと、カスタムの依存関係競合回避モジュールを操作するには、この競合回避モジュールは、置換するがします。

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションを実行します。

このタスクでは、ことを確認ストア ブラウザー サービスを使用し、イメージと取得したメッセージを示しています。 アプリケーションを実行します。

1. **F5** キーを押してアプリケーションを実行します。
2. をクリックして**ロック**を参照し、ジャンル メニュー内でどのように**MessageService**ビューに挿入された、ようこそメッセージと、イメージを読み込みます。 この例では入力する&quot;**ロック**&quot;:

    ![MVC 音楽ストア - ビュー インジェクション](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC 音楽ストア - ビュー インジェクション")

    *MVC 音楽ストア - ビュー インジェクション*
3. ブラウザーを閉じます。

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>手順 3: 挿入アクション フィルター

前の実習で**カスタム アクション フィルター**フィルター カスタマイズおよびインジェクションで作業しました。 この演習では、依存関係の挿入と Unity のコンテナーを使用してフィルターを挿入する方法を学習します。 実行するにはソリューションに追加する、音楽ストア、サイトの利用状況を追跡するカスタム アクション フィルター。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>タスク 1: ソリューション内の追跡フィルターを含む

このタスクでは含めます音楽ストアにイベントをトレースするカスタム アクション フィルター。 カスタム アクション フィルターと概念は、既に前の実習で扱わ&quot;カスタム アクション フィルター&quot;、だけこのラボの Assets フォルダーからフィルター クラスを含めるされ、Unity のフィルター プロバイダーを作成します。

1. 開く、**開始**にソリューションがある、 **Source\Ex03 - 挿入アクション Filter\Begin**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
      > 
      > 詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。
2. 含める**TraceActionFilter.cs**ファイルから**ソース/資産**に**フィルター/**フォルダーです。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > このカスタム アクション フィルターでは、ASP.NET のトレースを実行します。 チェックすることができます&quot;ASP.NET MVC 4 ローカルおよび動的アクション フィルター&quot;詳細の参照用のラボ環境。
3. 空のクラスを追加**FilterProvider.cs**フォルダー内のプロジェクトに  **/フィルター処理します。**
4. 追加、 **System.Web.Mvc**と**Microsoft.Practices.Unity**内の名前空間**FilterProvider.cs**です。

    (コード スニペットの*ASP.NET 依存関係インジェクション ラボ - Ex03 のフィルター プロバイダーの名前空間を追加する*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. クラスから継承するように**IFilterProvider**インターフェイスです。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. 追加、 **IUnityContainer**プロパティに、 **FilterProvider**クラス、およびコンテナーを割り当てるクラスのコンス トラクターを作成します。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 のフィルター プロバイダー コンス トラクター*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > フィルター プロバイダーのクラス コンス トラクターを作成しない、**新しい**の内部オブジェクトです。 コンテナーは、パラメーターとして渡され、Unity で依存関係を解決します。
7. **FilterProvider**クラス、メソッドを実装して**GetFilters**から**IFilterProvider**インターフェイスです。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 のフィルター プロバイダー GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>タスク 2 - 登録して、フィルターを有効にします。

このタスクでは、サイトの追跡を有効にします。 内のフィルターを登録するには、 **Bootstrapper.cs BuildUnityContainer**トレースを開始するメソッド。

1. 開いている**Web.config** System.Web グループで有効にするトレースの追跡およびプロジェクトのルートに存在します。

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. 開いている**Bootstrapper.cs**プロジェクトのルートにします。
3. 参照を追加、 **MvcMusicStore.Filters**名前空間。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 - ブートス トラップの名前空間を追加する*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. 選択、 **BuildUnityContainer**メソッドと、Unity コンテナー内のフィルターを登録します。 アクション フィルターと同様に、フィルター プロバイダーを登録する必要があります。

    (コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 - 登録 FilterProvider と ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションを実行します。

このタスクでは、アプリケーションを実行し、カスタム アクション フィルターが、アクティビティをトレースしているテストします。

1. **F5** キーを押してアプリケーションを実行します。
2. をクリックして**ロック**ジャンル メニュー内で。 する場合、ジャンルの詳細を参照することができます。

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "音楽ストア")

    *Music Store*
3. 参照**/Trace.axd**  ページで、クリックして、アプリケーション トレースを表示する**詳細を表示する**です。

    ![アプリケーションのトレース ログ](aspnet-mvc-4-dependency-injection/_static/image12.png "アプリケーションのトレース ログ")

    *アプリケーションのトレース ログ*

    ![アプリケーション トレース - 要求の詳細](aspnet-mvc-4-dependency-injection/_static/image13.png "アプリケーションのトレースの要求の詳細")

    *アプリケーション トレース - 要求の詳細*
4. ブラウザーを閉じます。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボでは、NuGet パッケージを使用する Unity の統合により、ASP.NET MVC 4 で依存関係の挿入を使用する方法を学習しました。 実現するためには、コント ローラー、ビュー、およびアクション フィルターの内部依存関係の挿入を使用しています。

次の概念がカバーされました。

- ASP.NET MVC 4 の依存関係の挿入機能
- Unity.Mvc3 NuGet パッケージを使用する unity の統合
- コント ローラーで依存関係の挿入
- ビューの依存関係の挿入
- アクション フィルターの依存関係の挿入

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express Web タイルを*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>付録 b: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-dependency-injection/_static/image19.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](aspnet-mvc-4-dependency-injection/_static/image20.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-dependency-injection/_static/image21.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-dependency-injection/_static/image22.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-dependency-injection/_static/image23.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-dependency-injection/_static/image24.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
