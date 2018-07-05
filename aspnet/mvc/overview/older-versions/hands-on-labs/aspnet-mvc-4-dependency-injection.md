---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 依存関係の挿入 |Microsoft Docs
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC と ASP.NET MVC 4 のフィルターの基本的な知識がある前提としています。 場合 rec する前に、ASP.NET MVC 4 のフィルターに使用していない.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 715444a6fbf491d7b99918294cfd2d0d0216cd09
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388045"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 依存関係の挿入

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**と**ASP.NET MVC 4 をフィルター処理**します。 使用していない場合**ASP.NET MVC 4 をフィルター処理**前を経由することを勧めします。 **ASP.NET MVC のカスタム アクション フィルター**ハンズオン ラボ。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 依存関係の注入](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)します。

**オブジェクト指向プログラミング**パラダイムでは、各オブジェクトが連携コラボレーション モデルでの共同作成者とコンシューマーがある場合。 当然ながら、この通信モデルは、オブジェクトと複雑さが増すと管理が困難になるコンポーネント間の依存関係を生成します。

![クラスの依存関係と、モデルの複雑さ](aspnet-mvc-4-dependency-injection/_static/image1.png "クラスの依存関係と、モデルの複雑さ")

*クラスの依存関係とモデルの複雑さ*

おそらく耳にしたが、**ファクトリ パターン**およびインターフェイスとクライアント オブジェクトはサービスの場所を担当多くの場合、サービスを使用する実装の間の分離。

依存関係の注入パターンは、制御の反転の特定の実装です。 **反転 (IoC) コントロールの**オブジェクトが依存しているが作業を行う他のオブジェクトを作成しないことを意味します。 代わりに、これらは、外部ソース (たとえば、xml 構成ファイル) から必要なオブジェクトを取得します。

**依存関係挿入 (DI)** つまりこれは、オブジェクトの介入なしは、通常、コンポーネントによって実行フレームワークをコンス トラクターのパラメーターを渡すし、プロパティを設定します。

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>依存関係の注入 (DI) デザイン パターン

大まかに言えば、依存関係の注入の目的は、するクライアント クラス (例: *、ゴルファー*) ものをインターフェイスを満たす必要があります (例: *IClub*)。 具象型では関係ありません (例: *WoodClub、IronClub、WedgeClub*または*PutterClub*)、それを処理する他のユーザーが必要 (例: 適切な*キャディ*)。 ASP.NET MVC では、依存関係競合回避モジュールは依存関係のロジックを別の場所を登録することを許可することができます (例: コンテナーまたは*クラブのバッグ*)。

![依存関係の注入ダイアグラム](aspnet-mvc-4-dependency-injection/_static/image2.png "依存関係の挿入の図")

*依存関係の挿入 - ゴルフとの類似性*

依存関係の注入パターンと制御の反転を使用する利点は次のとおりです。

- クラス結合度を削減します。
- コードの再利用が増加します。
- コードの保守性を向上します。
- アプリケーションのテストが向上します。

> [!NOTE]
> 依存関係の挿入は抽象ファクトリ デザイン パターンと比較場合がありますが、両方のアプローチのわずかな違いがあります。 DI には、フレームワーク、ファクトリと、登録済みサービスを呼び出すことによって依存関係を解決するために、背後にある作業があります。


依存関係の注入パターンを理解したところで ASP.NET MVC 4 を適用する方法をこの演習全体を通じて学習がします。 依存関係挿入を使用して開始、**コント ローラー**にデータベース アクセス サービスが含まれます。 次に、依存関係の挿入が適用されますが、**ビュー**サービスを使用して、情報を表示します。 最後に、ソリューション内のカスタム アクション フィルターを挿入する ASP.NET MVC 4 のフィルターに、DI を拡張します。

このハンズオン ラボでは、学習する方法。

- NuGet パッケージを使用して依存関係の注入に Unity を使用した ASP.NET MVC 4 を統合します。
- ASP.NET MVC のコント ローラー内の依存関係の挿入を使用して、
- ASP.NET MVC ビュー内の依存関係の挿入を使用して、
- ASP.NET MVC のアクション フィルター内の依存関係の挿入を使用して、

> [!NOTE]
> このラボは、依存関係の解決の Unity.Mvc3 NuGet パッケージを使用して、ASP.NET MVC 4 で動作する任意の依存関係挿入フレームワークを調整することができます。


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

次の演習では、このハンズオン ラボから成ります。

1. [手順 1: コント ローラーを挿入します。](#Exercise1)
2. [手順 2: ビューを挿入します。](#Exercise2)
3. [手順 3: フィルターを挿入します。](#Exercise3)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **30 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>手順 1: コント ローラーを挿入します。

この演習では、Unity は NuGet パッケージを使用して統合することにより、ASP.NET MVC のコント ローラーで依存関係の挿入を使用する方法を学びます。 そのため、サービスをデータ アクセス ロジックを分離する、MvcMusicStore コント ローラーに含まれます。 サービスはコント ローラーのコンス トラクターは、のヘルプで依存関係の挿入を使用して、解決に新しい依存関係を作成**Unity**します。

このアプローチでは、小さい結合された、アプリケーションはより柔軟で簡単に管理およびテストを生成する方法を示します。 Unity を使用した ASP.NET MVC を統合する方法も学習します。

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>StoreManager サービスについて

今すぐ begin ソリューションで提供されている MVC Music Store にはという名前のデータ ストアのコント ローラーを管理するサービスが含まれています**StoreService**します。 次のとおり、ストア サービスの実装が表示されます。 すべてのメソッドがモデルのエンティティを返すことに注意してください。

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController**ソリューションのようになりましたの使用を開始から**StoreService**します。 すべてのデータの参照が削除された**StoreController**、および使用する任意のメソッドを変更することがなく、現在のデータ アクセス プロバイダーを変更する実行可能なようになりました**StoreService**します。

その下に表示されます、 **StoreController**実装との依存関係を持つ**StoreService**クラスのコンス トラクター内。

> [!NOTE]
> この演習で導入された依存関係に関連する**Inversion of Control** (IoC)。
> 
> **StoreController**クラスのコンス トラクターは、 **IStoreService**型のパラメーターは、クラス内からのサービス呼び出しを実行するために不可欠です。 ただし、 **StoreController**を ASP.NET MVC を使用する任意のコント ローラーを持つ必要があります (パラメーターなし) での既定コンス トラクターを実装していません。
> 
> 依存関係を解決するには、コント ローラーを抽象ファクトリ (指定した型の任意のオブジェクトを返すクラス) で作成する必要があります。


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> クラスは、パラメーターなしのコンス トラクターが宣言されていないため、サービス オブジェクトを送信することがなく、StoreController を作成するとき、エラーが発生します。


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>タスク 1 - アプリケーションの実行

このタスクで、データ アクセスでは、アプリケーション ロジックから分離されるストア コント ローラーに、サービスを含む開始アプリケーションを実行します。

アプリケーションを実行するときにコント ローラー サービスが既定でのパラメーターとして渡されないと、例外が表示されます。

1. 開く、**開始**ソリューション**Source\Ex01 挿入 Controller\Begin**します。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. キーを押して**ctrl キーを押しながら f5 キーを押して**デバッグなしでアプリケーションを実行します。 エラー メッセージが表示されます&quot;**パラメーターなしのコンス トラクターはこのオブジェクトに対して定義されている**&quot;:

    ![ASP.NET MVC の開始のアプリケーションの実行中にエラー](aspnet-mvc-4-dependency-injection/_static/image3.png "ASP.NET MVC の開始のアプリケーションの実行中にエラー")

    *ASP.NET MVC の開始のアプリケーションの実行中にエラー*
3. ブラウザーを閉じます。

次の手順では、このコント ローラーが必要な依存関係を挿入する音楽ストア ソリューションの機能します。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>タスク 2 - MvcMusicStore ソリューションに Unity を含む

このタスクで含める**Unity.Mvc3** NuGet パッケージをソリューションにします。

> [!NOTE]
> Unity.Mvc3 パッケージは、ASP.NET MVC 3 では、用に設計されましたが、ASP.NET MVC 4 と完全な互換性です。
> 
> Unity では、インスタンス オプション サポートで軽量で拡張可能な依存関係挿入コンテナーにし、型のインターセプトします。 あらゆる種類の .NET アプリケーションで使用するための汎用コンテナーになります。 などの依存関係の注入メカニズムで見つかったすべての一般的な機能を提供します。 オブジェクトの作成、コンテナーにコンポーネントの構成を遅らせることで、ランタイムと柔軟性の依存関係を指定することによって要件を抽象化します。


1. インストール**Unity.Mvc3**で NuGet パッケージ、 **MvcMusicStore**プロジェクト。 これを行うには、開く、**パッケージ マネージャー コンソール**から**ビュー** | **その他の Windows**します。
2. 次のコマンドを実行します。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Unity.Mvc3 NuGet パッケージをインストールする](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet パッケージをインストールします。")

    *Unity.Mvc3 NuGet パッケージをインストールします。*
3. 1 回、 **Unity.Mvc3**パッケージがインストールされている、探索ファイルとフォルダーの Unity の構成を簡略化するために自動的に追加します。

    ![Unity.Mvc3 パッケージがインストールされている](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 パッケージがインストールされています。")

    *Unity.Mvc3 パッケージがインストールされています。*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>タスク 3 - Global.asax.cs のアプリケーションを登録する Unity\_開始

このタスクでは更新、**アプリケーション\_開始**メソッドにある**Global.asax.cs** Unity ブートス トラップ初期化子を呼び出すし、次に、ブートス トラップ ファイルの登録を更新するにはサービスと依存関係の挿入に使用するコント ローラー。

1. ここで、これは、Unity コンテナーを初期化するファイル ブートス トラップと依存関係競合回避モジュールをフックするされます。 これを行うには、開く**Global.asax.cs**内で強調表示されている次のコードを追加し、**アプリケーション\_開始**メソッド。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex01 - 初期化 Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. 開いている**Bootstrapper.cs**ファイル。
3. 次の名前空間を含める: **MvcMusicStore.Services**と**MusicStore.Controllers**します。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex01 - ブートス トラップ名前空間を追加する*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. 置換**BuildUnityContainer**メソッドの内容をストアのコント ローラーとストア サービスを登録する次のコードにします。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex01 - 登録ストア コント ローラーとサービス*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションの実行

このタスクで Unity を含めた後にロードするようになりましたことを確認するアプリケーションを実行します。

1. キーを押して**F5**アプリケーションを実行する、アプリケーションはすべてのエラー メッセージを表示することがなく読み込むようになりました必要があります。

    ![アプリケーションを実行して、依存関係の挿入](aspnet-mvc-4-dependency-injection/_static/image6.png "アプリケーションを実行して、依存関係の挿入")

    *実行中のアプリケーション依存関係の挿入*
2. 参照する **/格納**します。 これを呼び出す**StoreController**を使用して今すぐ作成されます**Unity**します。

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC Music Store*
3. ブラウザーを閉じます。

次の演習では、ASP.NET MVC ビューとアクション フィルター内で使用する依存関係の挿入のスコープを拡張する方法を学びます。

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>手順 2: ビューを挿入します。

この演習では、Unity の統合を ASP.NET MVC 4 の新機能を含むビューの依存関係の挿入を使用する方法を学びます。 このため、ストア ビューの参照、これは、メッセージと、次の図に記載内にカスタム サービスを呼び出します。

次に、プロジェクトを Unity に統合し、依存関係を挿入するカスタム依存関係競合回避モジュールを作成します。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>タスク 1 - サービスを使用するビューを作成します。

このタスクでは、新しい依存関係を生成するサービスの呼び出しを実行するビューを作成します。 このソリューションに含まれる単純なメッセージング サービスで、サービスが構成されます。

1. 開く、**開始**ソリューション、 **Source\Ex02 挿入 View\Begin**フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
      > 
      > 詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。
2. 含める、 **MessageService.cs**と**IMessageService.cs**クラスに格納、 **\Assets をソース**フォルダー **/サービス**。 これを行うには、右クリックして**サービス**フォルダーと選択**既存項目の追加**します。 ファイルの場所を参照し、それらが含まれます。

    ![メッセージのサービスとサービスのインターフェイスを追加する](aspnet-mvc-4-dependency-injection/_static/image8.png "メッセージ サービスとサービスのインターフェイスを追加します。")

    *サービス インターフェイスと追加のメッセージ サービス*

    > [!NOTE]
    > **IMessageService**インターフェイスによって実装される 2 つのプロパティを定義する、 **MessageService**クラス。 これらのプロパティ -**メッセージ**と**ImageUrl**-表示するには、メッセージと、イメージの URL を格納します。
3. フォルダーを作成**ページ/** プロジェクトのルート フォルダー、および既存のクラスを追加し、 **MyBasePage.cs**から**Source\Assets**します。 継承する基本ページには、次の構造があります。

    ![Pages フォルダー](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages フォルダー")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. 開いている**Browse.cshtml**から表示 **/ビュー/ストア**フォルダーから継承し**MyBasePage.cs**します。

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. **参照**ビューで、呼び出しを追加して**MessageService**イメージと、サービスによって取得したメッセージを表示します。
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>タスク 2 - カスタム依存関係競合回避モジュールと、カスタム ビュー ページ アクティベーターを含む

前のタスクでは、その中のサービス呼び出しを実行して、ビュー内の新しい依存関係を挿入します。 ここで、ASP.NET MVC 依存関係の注入インターフェイスを実装することでその依存関係を解決する**IViewPageActivator**と**IDependencyResolver**します。 実装、ソリューションに含めるは**IDependencyResolver** Unity を使用してサービスの取得を処理するされます。 別のカスタム実装が含まれますが、 **IViewPageActivator**インターフェイス ビューの作成を解決するためです。

> [!NOTE]
> ASP.NET MVC 3 では、以降の依存関係の挿入の実装にサービスを登録するインターフェイスが簡略化されます。 **IDependencyResolver**と**IViewPageActivator**依存関係の挿入の ASP.NET MVC 3 の機能の一部であります。
> 
> **-IDependencyResolver**インターフェイスには、前の IMvcServiceLocator が置き換えられます。 IDependencyResolver を実装には、サービスまたはサービスのコレクションのインスタンスを返す必要があります。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator**インターフェイスには、ページの表示に依存関係の挿入を使用してインスタンス化きめの細かい制御が用意されています。 実装するクラス**IViewPageActivator**インターフェイスは、コンテキスト情報を使用してビューのインスタンスを作成できます。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. 作成、/**ファクトリ**プロジェクトのルート フォルダー内のフォルダー。
2. 含める**CustomViewPageActivator.cs**からソリューションに**ソース/資産/** に**ファクトリ**フォルダー。 そのためには右クリックし、 **/Factories**フォルダーで、**追加 |既存項目の**選び**CustomViewPageActivator.cs**します。 このクラスは、実装、 **IViewPageActivator** Unity コンテナーを保持するインターフェイス。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** Unity コンテナーを使用して、ビューの作成の管理を担当します。
3. 含める**UnityDependencyResolver.cs**ファイルから **/ソース/資産**に **/Factories**フォルダー。 そのためには右クリックし、 **/Factories**フォルダーで、**追加 |既存項目の**選び**UnityDependencyResolver.cs**ファイル。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver**クラスは、Unity のカスタム DependencyResolver します。 Unity コンテナー内でサービスが見つからない場合は、ベースの競合回避モジュールが invocated します。

次の実習で両方の実装は、サービスと、ビューの場所を知っているモデルに登録されます。

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>タスク 3 - Unity コンテナー内の依存関係の挿入に登録します。

このタスクでは、依存関係の挿入操作を作成する前、すべての操作を格納します。

ここまで、ソリューションは、次の要素があります。

- A**参照**ビューから継承する**MyBaseClass**消費**MessageService**します。
- 中間クラス -**MyBaseClass**-依存関係の挿入サービス インターフェイスの宣言を持ちます。
- サービス - **MessageService** - とそのインターフェイス**IMessageService**します。
- Unity - 使用するカスタム依存関係リゾルバー **UnityDependencyResolver** -サービスの取得を処理します。
- ビュー ページ アクティベーター - **CustomViewPageActivator** -ページを作成します。

挿入する**参照**ビュー、今すぐ登録するカスタム依存関係競合回避モジュール Unity コンテナーにします。

1. 開いている**Bootstrapper.cs**ファイル。
2. インスタンスを登録**MessageService**サービスを初期化するために Unity コンテナーに。

    (コード スニペット - *ASP.NET 依存関係注入ラボ - Ex02 - 登録メッセージ サービス*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. 参照を追加**MvcMusicStore.Factories**名前空間。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex02 - ファクトリ Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. 登録**CustomViewPageActivator**として Unity コンテナーにビュー ページ アクティベーター。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex02 - 登録 CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. インスタンスでの ASP.NET MVC 4 の既定の依存関係競合回避モジュールを置き換えます**UnityDependencyResolver**します。 これを行うには、置き換える**Initialise**メソッドの内容を次のコードに。

    (コード スニペット - *ASP.NET 依存関係の挿入ラボ - Ex02 - 更新プログラムの依存関係リゾルバー*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC には、既定の依存関係競合回避モジュールのクラスが用意されています。 Unity に対して作成した 1 つとして、カスタム依存関係競合回避モジュールを操作するには、この競合回避モジュールを交換する必要があります。

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションの実行

このタスクでストア ブラウザーが、サービスを利用して、取得したメッセージのイメージを示していますを確認するアプリケーションを実行します。

1. **F5** キーを押してアプリケーションを実行します。
2. をクリックして**Rock**を参照し、[ジャンル] メニュー内方法、 **MessageService**ビューに挿入され、ウェルカム メッセージと、イメージが読み込まれます。 この例で入力は&quot; **Rock**&quot;:

    ![MVC Music Store - ビュー挿入](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - ビューの挿入")

    *MVC Music Store - ビューの挿入*
3. ブラウザーを閉じます。

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>手順 3: アクション フィルターを挿入します。

前のハンズオン ラボで**カスタム アクション フィルター**フィルターのカスタマイズと挿入を使用してきた。 この演習では、依存関係の注入に Unity コンテナーを使用してフィルターを挿入する方法を学習します。 そのためにするソリューションに追加されます、ミュージック ストア サイトのアクティビティをトレースするカスタム アクション フィルター。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>タスク 1 - ソリューションで追跡フィルターを含む

このタスクでに含める音楽ストア イベントをトレースするカスタム アクション フィルター。 概念を既に前の実習で扱うカスタム アクション フィルターとして&quot;カスタム アクション フィルター&quot;、このラボでの Assets フォルダーからフィルター クラスを含めるだけですし、し、Unity のフィルター プロバイダーを作成します。

1. 開く、**開始**ソリューション、 **Source\Ex03 - 挿入アクション Filter\Begin**フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
      > 
      > 詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。
2. 含める**TraceActionFilter.cs**ファイルから **/ソース/資産**に**フィルター/** フォルダー。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > このカスタム アクション フィルターは、ASP.NET のトレースを実行します。 チェックすることができます&quot;ASP.NET MVC 4 ローカルおよびアクション フィルターが動的&quot;ラボ他の参照。
3. 空のクラスを追加**FilterProvider.cs**フォルダー内のプロジェクトに  **/フィルター処理します。**
4. 追加、 **System.Web.Mvc**と**Microsoft.Practices.Unity**内の名前空間**FilterProvider.cs**します。

    (コード スニペット - *ASP.NET 依存関係のインジェクション ラボ - Ex03 - フィルター プロバイダーが名前空間を追加する*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. クラスから継承するように**IFilterProvider**インターフェイス。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. 追加、 **IUnityContainer**プロパティ、 **FilterProvider**クラス、し、コンテナーを割り当てるクラスのコンス トラクターを作成します。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - フィルター プロバイダー コンス トラクター*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > フィルター プロバイダーのクラス コンス トラクターを作成しない、**新しい**内のオブジェクトします。 コンテナーは、パラメーターとして渡され、依存関係は、Unity によって解決されます。
7. **FilterProvider**クラス、メソッドを実装**GetFilters**から**IFilterProvider**インターフェイス。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - フィルター プロバイダー GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>タスク 2 - 登録して、フィルターを有効にします。

このタスクでは、サイトの追跡を有効になります。 フィルターを登録するには、 **Bootstrapper.cs BuildUnityContainer**トレースを開始するメソッド。

1. 開いている**Web.config** System.Web グループで有効にするトレースの追跡とプロジェクトのルートにあります。

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. 開いている**Bootstrapper.cs**プロジェクト ルートにあります。
3. 参照を追加、 **MvcMusicStore.Filters**名前空間。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - ブートス トラップ名前空間を追加する*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. 選択、 **BuildUnityContainer**メソッドと Unity コンテナーでのフィルターを登録します。 アクション フィルターとフィルター プロバイダーを登録する必要があります。

    (コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - 登録 FilterProvider と ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>タスク 3 - アプリケーションの実行

このタスクでは、アプリケーションを実行し、カスタム アクション フィルターが、アクティビティをトレースしているテストします。

1. **F5** キーを押してアプリケーションを実行します。
2. クリックして**Rock**ジャンル メニュー内で。 する場合は、ジャンルの詳細を参照できます。

    ![ミュージック ストア](aspnet-mvc-4-dependency-injection/_static/image11.png "ミュージック ストア")

    *Music Store*
3. 参照する **/Trace.axd** ] ページの [クリックして、アプリケーション トレースを表示する**詳細を表示する**します。

    ![アプリケーション トレース ログ](aspnet-mvc-4-dependency-injection/_static/image12.png "アプリケーション トレース ログ")

    *アプリケーション トレース ログ*

    ![アプリケーション トレース - 要求の詳細](aspnet-mvc-4-dependency-injection/_static/image13.png "アプリケーション トレース - 要求の詳細")

    *アプリケーション トレース - 要求の詳細*
4. ブラウザーを閉じます。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボは、Unity は NuGet パッケージを使用して統合することにより、ASP.NET MVC 4 で依存関係の挿入を使用する方法を学習しました。 これを実現するには、コント ローラー、ビュー、およびアクション フィルター内で依存関係の挿入を使用しています。

次の概念がについて説明します。

- ASP.NET MVC 4 依存関係の注入機能
- Unity.Mvc3 NuGet パッケージを使用して unity の統合
- コント ローラーで依存関係の挿入
- ビューの依存関係挿入
- アクション フィルターの依存関係の挿入

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express for Web のタイル*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>付録 b: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-dependency-injection/_static/image19.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](aspnet-mvc-4-dependency-injection/_static/image20.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-dependency-injection/_static/image21.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-dependency-injection/_static/image22.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-dependency-injection/_static/image23.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-dependency-injection/_static/image24.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
