---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 モデルとデータ アクセス |Microsoft Docs
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC の基本的な知識がある前提としています。 前に ASP.NET MVC を使用していない場合をお勧めすると、ASP.NET MVC 4 を経由しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: afc03d87431632bbb3ab59241de0edb4bb7af12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371130"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 モデルとデータ アクセス

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**します。 使用していない場合**ASP.NET MVC**前を経由することを勧めします。 **ASP.NET MVC 4 の基礎**ハンズオン ラボ。

このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 モデルとデータ アクセス](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)します。

**ASP.NET MVC の基礎**のハンズオン ラボを渡そうとしてがされているハード コードされたデータ、コント ローラーからテンプレートの表示にします。 しかし、実際の Web アプリケーションを構築するために実際のデータベースを使用したい場合があります。

このハンズオン ラボでは、ミュージック ストア アプリケーションに必要なデータ格納および取得するためにデータベース エンジンを使用する方法を示します。 これを実現がするには、既存のデータベースで開始し、そこから、Entity Data Model を作成します。 この演習では、全体で変化する、 **Database First**方法だけでなく**Code First**アプローチします。

ただし、使用することできますも、 **Model First**アプローチや、ツールを使用して、同じモデルを作成し、そこから、データベースを生成します。

![Vs を最初にデータベース。モデル ファースト](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First とします。まずモデルします。")

*Vs を最初にデータベース。まずモデルします。*

モデルを生成した後にハード コーディングされたデータを使用する代わりに、データベースから取得したデータ ストアのビューを提供する StoreController で適切な調整が作成されます。 今度は、データは、データベースから取得されますが、ビュー テンプレートに同じビューモデルが戻される、StoreController のため、テンプレートの表示に変更を加えることは必要はありません。

**Code First 手法**

Code First アプローチでは、フレームワークを組み合わせて、一般的にクラスを生成せず、コードからモデルを定義できます。

コードで最初に、モデル オブジェクトが定義されて、Poco で&quot;Plain Old CLR Object&quot;します。 Poco は、単純なのプレーンなクラスを継承がないインターフェイスを実装していません。 これらのファイルからデータベースを自動的に生成することができます。 または既存のデータベースを使用して、コードからクラスのマッピングを生成します。

このアプローチを使用する利点は、マッピング フレームワークでは、Poco クラスは結合されていないと、モデルは (この例では Entity Framework) では、永続化フレームワークから独立してです。

> [!NOTE]
> このラボは ASP.NET MVC 4 に基づくし、ミュージック ストア サンプル アプリケーションのバージョンに合わせてこのハンズオン ラボで表示されている機能のみを最小限に抑えます。
> 
> 全体を検証する場合**ミュージック ストア**で見つかりますチュートリアル アプリケーション[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。


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

1. [手順 1: データベースの追加](#Exercise1)
2. [手順 2: Code First を使用してデータベースを作成します。](#Exercise2)
3. [手順 3: パラメーターを持つデータベースの照会](#Exercise3)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **35 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>手順 1: データベースの追加

この演習では、そのデータを使用するために、ソリューションに MusicStore アプリケーションのテーブルを持つデータベースを追加する方法を学びます。 データベースがモデルでは、生成され、ソリューションに追加、ハード コーディングされた値を使用する代わりに、データベースから取得されたデータにビュー テンプレートを提供する StoreController クラスを変更します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>タスク 1 - データベースの追加

このタスクでは、ソリューションに MusicStore アプリケーションのメインのテーブルでは、既に作成したデータベースを追加します。

1. 開く、**開始**ソリューションがある**ソース/Ex1-AddingADatabaseDBFirst/開始/** フォルダー。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 追加**MvcMusicStore**データベース ファイル。 このハンズオン ラボと呼ばれる作成済みのデータベースを使用して**MvcMusicStore.mdf**します。 そのためには、右クリック**アプリ\_データ**フォルダーをポイントして**追加**順にクリックします**既存項目の**します。 参照する**\Source\Assets**を選択し、 **MvcMusicStore.mdf**ファイル。

    ![既存の項目を追加する](aspnet-mvc-4-models-and-data-access/_static/image2.png "既存の項目の追加")

    *既存の項目を追加します。*

    ![データベース ファイルの MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf データベース ファイル")

    *MvcMusicStore.mdf データベース ファイル*

    データベースがプロジェクトに追加されました。 データベースが、ソリューション内にある場合でもは、クエリを実行し、別のデータベース サーバーでホストされているように更新します。

    ![ソリューション エクスプ ローラーでデータベースを MvcMusicStore](aspnet-mvc-4-models-and-data-access/_static/image4.png "ソリューション エクスプ ローラーで MvcMusicStore データベース")

    *ソリューション エクスプ ローラーで MvcMusicStore データベース*
3. データベースへの接続を確認します。 これを行うには、ダブルクリック**MvcMusicStore.mdf**接続を確立します。

    ![MvcMusicStore.mdf への接続](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf への接続")

    *MvcMusicStore.mdf への接続*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>タスク 2 - データ モデルを作成します。

このタスクでは、前のタスクで追加されたデータベースと対話するデータ モデルを作成します。

1. データベースを表すデータ モデルを作成します。 ソリューション エクスプ ローラーを右クリックで、**モデル**フォルダーを指す**追加**順にクリックします**新しい項目の**します。 **新しい項目の追加**ダイアログ ボックスで、**データ**テンプレートをクリックし、 **ADO.NET Entity Data Model**項目。 データ モデル名を変更して**StoreDB.edmx**クリック**追加**します。

    ![StoreDB ADO.NET Entity Data Model を追加する](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET エンティティ データ モデルの追加")

    *StoreDB ADO.NET エンティティ データ モデルの追加*
2. **Entity Data Model ウィザード**が表示されます。 このウィザードでモデル レイヤーの作成手順を説明します。 追加、既存のデータベース recentyl に基づいてモデルを作成する必要があります、ために選択**データベースから生成** をクリック**次**します。

    ![モデル コンテンツを選択する](aspnet-mvc-4-models-and-data-access/_static/image7.png "モデル コンテンツを選択します。")

    *モデル コンテンツを選択します。*
3. データベースからモデルを生成するため使用する接続を指定する必要があります。 クリックして**新しい接続**します。
4. 選択**Microsoft SQL Server データベース ファイル**クリック**続行**します。

    ![データ ソース](aspnet-mvc-4-models-and-data-access/_static/image8.png "データ ソースの選択")

    *データ ソース ダイアログを選択します。*
5. をクリックして**参照**、データベースを選択します**MvcMusicStore.mdf**内にある、**アプリ\_データ**フォルダーをクリックします**OK**します。

    ![接続プロパティ](aspnet-mvc-4-models-and-data-access/_static/image9.png "接続のプロパティ")

    *接続のプロパティ*
6. 生成されたクラスする必要がありますエンティティ接続文字列と同じ名前を指定しているため、その名前を**MusicStoreEntities**  をクリック**次**します。

    ![データ接続を選択する](aspnet-mvc-4-models-and-data-access/_static/image10.png "データ接続を選択します。")

    *データ接続を選択します。*
7. 使用するデータベース オブジェクトを選択します。 エンティティ モデルは、データベースのテーブルだけを使用して、選択、**テーブル**オプションを確認して、**モデルに外部キー列を含める**と**化または単数生成オブジェクト名**もオプションが選択されます。 変更をモデル Namespace **MvcMusicStore.Model**  をクリック**完了**します。

    ![データベース オブジェクトを選択する](aspnet-mvc-4-models-and-data-access/_static/image11.png "データベース オブジェクトを選択します。")

    *データベース オブジェクトを選択します。*

    > [!NOTE]
    > セキュリティ警告ダイアログが表示されている場合にクリックします**OK**をテンプレートを実行し、モデルのエンティティ クラスを生成します。
8. 各テーブルをデータベースにマップする別のクラスが作成するときに、データベースのエンティティの図が表示されます。 たとえば、**アルバム**テーブルによって表されます、**アルバム**クラスは、テーブル内の各列がクラス プロパティにマップされます。 これは、使用すると、クエリを実行し、データベース内の行を表すオブジェクトを使用できます。

    ![エンティティの図](aspnet-mvc-4-models-and-data-access/_static/image12.png "エンティティ図")

    *エンティティの図*

    > [!NOTE]
    > T4 テンプレート (.tt) は、エンティティ クラスを生成するコードを実行し、同じ名前の既存のクラスが上書きされます。 この例では、クラスで&quot;アルバム&quot;、&quot;ジャンル&quot;と&quot;アーティスト&quot;が生成されたコードで上書きされます。


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>タスク 3 - アプリケーションのビルド

このタスクでは、確認が、モデルの生成が削除、**アルバム**、**ジャンル**と**アーティスト**モデル クラスでは、プロジェクトが正常にビルドを使用して新しいデータ モデル クラス。

1. 選択してプロジェクトをビルド、**ビルド**メニュー項目をクリックし**ビルド MvcMusicStore**します。

    ![プロジェクトのビルド](aspnet-mvc-4-models-and-data-access/_static/image13.png "プロジェクトのビルド")

    *プロジェクトのビルド*
2. プロジェクトが正常にビルドします。 理由は、引き続き動作しますか。 データベース テーブルが削除されたクラスに使用していたプロパティを含むフィールドをうまく**アルバム**と**ジャンル**します。

    ![ビルドが成功した](aspnet-mvc-4-models-and-data-access/_static/image14.png "ビルドに成功しました")

    *ビルドに成功しました*
3. デザイナーがダイアグラム形式のエンティティを表示する、c# のクラスでは本当に。 展開、 **StoreDB.edmx**ソリューション エクスプ ローラーでノードをクリックし**StoreDB.tt**、新規生成されたエンティティが表示されます。

    ![生成されたファイル](aspnet-mvc-4-models-and-data-access/_static/image15.png "によって生成されたファイル")

    *生成されたファイル*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>タスク 4 - データベースのクエリを実行します。

このタスクでは、ハードコーディングされたデータを使用する代わりには、データベース情報を取得するクエリ実行できるように StoreController クラスを更新します。

1. 開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:

    (コード スニペット -*モデルとデータへのアクセス - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. **MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。 Update**参照**のすべてのジャンルを取得するアクション メソッド、**アルバム**します。

    (コード スニペット -*モデルとデータ アクセス - Ex1 ストア参照*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > .NET と呼ばれる機能を使用している**LINQ** (統合言語クエリ) に対しては、データベースに対してコードを実行し、返すこのコレクションは、厳密に型指定されたクエリ式を記述するオブジェクトをプログラミングできますに対して。
    > 
    > LINQ の詳細についてを参照してください、 [msdn サイト](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx)します。
3. Update**インデックス**アクション メソッドは、すべてのジャンルを取得します。

    (コード スニペット -*モデルとデータ アクセス - Ex1 ストア インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Update**インデックス**アクション メソッドをすべてのジャンルを取得し、一覧のコレクションを変換します。

    (コード スニペット -*モデルとデータ アクセス - Ex1 ストア GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションの実行

このタスクでは、ストアのインデックス ページをハードコードされたものではなく、データベースに格納されているジャンルようになりました表示するかを確認します。 にビュー テンプレートを変更する必要はありません、 **StoreController**はエンティティを取得する同じと同様が、今度は、データは、データベースから取得されます。

1. ソリューションとキーを押してリビルド**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 いることを確認のメニュー**ジャンル**一覧をハード コーディングをして、データは、データベースから直接取得します。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *データベースからジャンルを参照*
3. これで、ジャンルを参照し、アルバムがデータベースから設定されますを確認します。

    ![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image17.png "データベースからアルバムを参照")

    *データベースからアルバムを参照*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>手順 2: 最初のコードを使用してデータベースを作成します。

この演習では、Code First アプローチを使用して、MusicStore アプリケーションのテーブルを含むデータベースを作成する方法とそのデータにアクセスする方法を説明します。

モデルが生成されると、ハードコードされた値を使用する代わりに、データベースから取得されたデータにビュー テンプレートを提供する StoreController を変更します。

> [!NOTE]
> 演習 1 を完了したし、Database First アプローチでは以前、今すぐに別のプロセスで同じ結果を取得する方法を学習します。 手順 1 と共通のタスクは、読みやすくマークされています。 手順 1 を完了していないコードの最初の方法を学習したい場合は、この演習からを起動し、トピックの完全なカバレッジを取得できます。


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>タスク 1 - サンプル データを追加

このタスクで Code First を使用して最初作成時に、データベースにサンプル データを読み込みます。

1. 開く、**開始**ソリューションがある**ソース/Ex2-CreatingADatabaseCodeFirst/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 追加、 **SampleData.cs**ファイルを**モデル**フォルダー。 そのためには、右クリック**モデル**フォルダーをポイントして**追加**順にクリックします**既存項目の**します。 参照する**\Source\Assets**を選択し、 **SampleData.cs**ファイル。

    ![サンプル データは、コードを入力](aspnet-mvc-4-models-and-data-access/_static/image18.png "サンプル データは、コードを設定")

    *データのサンプル コードを設定します。*
3. 開く、 **Global.asax.cs**ファイルを開き、次の追加*を使用して*ステートメント。

    (コード スニペット -*モデルとデータ アクセス - Ex2 グローバル Asax Using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. **アプリケーション\_Start()** メソッドは、データベース初期化子を設定するのには、次の行を追加します。

    (コード スニペット -*モデルとデータ アクセス - Ex2 グローバル Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>タスク 2 - データベースへの接続を構成します。

作成できたので、プロジェクトに既にデータベースを追加した、 **Web.config**ファイルの接続文字列。

1. 接続文字列に追加**Web.config**します。そのために開く**Web.config**プロジェクトのルートと置換の接続文字列で示される行で DefaultConnection をという名前で、 **&lt;connectionStrings&gt;** セクション。

    ![Web.config ファイルの場所](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config ファイルの場所")

    *Web.config ファイルの場所*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>タスク 3: モデルの使用

データベースのテーブル モデルをリンクする、データベースへの接続を構成しておくことようになりました。 このタスクでは、Code First とデータベースにリンクするクラスを作成します。 変更する必要があります、既存モデルの POCO クラスがあることに注意してください。

> [!NOTE]
> 演習 1 を完了すると場合、は、ウィザードによってこの手順が実行されたことがわかります。 Code First により、データ エンティティにリンクするクラスを手動で作成されます。

1. POCO のモデル クラスを開きます**ジャンル**から**モデル**プロジェクト フォルダーと ID を含める Int 型のプロパティを使用して、名前を持つ**GenreId**します。

    (コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初のジャンル*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Code First 規約を使用するには、クラスのジャンルに主キー プロパティが自動的に検出することが必要です。
    > 
    > 詳細については、この最初の規則をコード[msdn の記事](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx)します。
2. これで、POCO のモデル クラスを開く**アルバム**から**モデル**プロジェクト フォルダー、外部キーを含めると、名前のプロパティを作成**GenreId**と**ArtistId**します。 このクラスが既にある、 **GenreId**の主キー。

    (コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初のアルバム*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. POCO のモデル クラスを開きます**アーティスト**を含めると、 **ArtistId**プロパティ。

    (コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初のアーティスト*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. 右クリックし、**モデル**プロジェクト フォルダーと選択**追加 |クラス**します。 ファイルに名前を**MusicStoreEntities.cs**します。 をクリックし、**追加します。**

    ![クラスの追加](aspnet-mvc-4-models-and-data-access/_static/image20.png "クラスの追加")

    *新しい項目の追加*

    ![追加、class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 を追加します。")

    *クラスの追加*
5. 先ほど作成したクラスを開きます**MusicStoreEntities.cs**、名前空間を含めると**System.Data.Entity**と**System.Data.Entity.Infrastructure**します。

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. 拡張するクラスの宣言を置き換える、 **DbContext**クラス: パブリック宣言**DBSet**オーバーライドと**OnModelCreating**メソッド。 この手順の後に、Entity Framework を使用したモデルにリンクされるドメイン クラスが表示されます。 そのためには、次のようにクラス コードに置き換えます。

    (コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初の MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Entity Framework で**DbContext**と**DBSet**ジャンル POCO クラスを照会することができます。 拡張することによって**OnModelCreating**で指定するメソッド、**コード**ジャンルがデータベース テーブルにマップする方法。 この msdn の記事で DBContext、DBSet の詳細が見つかります:[リンク](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>タスク 4 - データベースのクエリを実行します。

このタスクでは、ハードコーディングされたデータを使用する代わりがから取得するために、データベース StoreController クラスを更新します。

> [!NOTE]
> このタスクは、演習 1 と共通です。
> 
> 演習 1 を完了する場合は、次の手順は、両方の方法で同じを記録しては (データベースの最初の Code first または)。 これらは同じで、モデルでは、データをリンクする方法が、データ エンティティへのアクセスは、コント ローラーから透過的ながまだします。


1. 開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:

    (コード スニペット -*モデルとデータへのアクセス - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. **MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。 Update**参照**のすべてのジャンルを取得するアクション メソッド、**アルバム**します。

    (コード スニペット -*モデルとデータ アクセス - Ex2 ストア参照*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > .NET と呼ばれる機能を使用している**LINQ** (統合言語クエリ) に対しては、データベースに対してコードを実行し、返すこのコレクションは、厳密に型指定されたクエリ式を記述するオブジェクトをプログラミングできますに対して。
    > 
    > LINQ の詳細についてを参照してください、 [msdn サイト](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx)します。
3. Update**インデックス**アクション メソッドは、すべてのジャンルを取得します。

    (コード スニペット -*モデルとデータ アクセス - Ex2 ストア インデックス*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Update**インデックス**アクション メソッドをすべてのジャンルを取得し、一覧のコレクションを変換します。

    (コード スニペット -*モデルとデータ アクセス - Ex2 ストア GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションの実行

このタスクでは、ストアのインデックス ページをハードコードされたものではなく、データベースに格納されているジャンルようになりました表示するかを確認します。 にビュー テンプレートを変更する必要はありません、 **StoreController**同じを返して**StoreIndexViewModel**と同様に、今回はデータベースからデータが取得されます。

1. ソリューションとキーを押してリビルド**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 いることを確認のメニュー**ジャンル**一覧をハード コーディングをして、データは、データベースから直接取得します。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *データベースからジャンルを参照*
3. これで、ジャンルを参照し、アルバムがデータベースから設定されますを確認します。

    ![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image23.png "データベースからアルバムを参照")

    *データベースからアルバムを参照*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>手順 3: パラメーターを持つデータベースの照会

この演習では、パラメーターを使用してデータベースを照会する方法とクエリ結果の整形を使用する方法について説明しますが、データベース数を削減する機能がより効率的な方法でデータの取得にアクセスします。

> [!NOTE]
> クエリ結果の形成について詳しくは、次を参照してください。 [msdn の記事](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)します。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>タスク 1 - 変更 StoreController アルバムをデータベースから取得するには

このタスクでは、変更、 **StoreController**特定のジャンルからのアルバムを取得するデータベースにアクセスするクラス。

1. 開く、**開始**ソリューションがある、 **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** Code First アプローチを使用する場合はフォルダーまたは**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**データベース優先のアプローチを使用する場合はフォルダーです。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、 **StoreController**クラスを変更する、**参照**アクション メソッド。 この場合に、**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリックします**StoreController.cs**。
3. 変更、**参照**アクション メソッドを特定のジャンルのアルバムを取得します。 これを行うには、次のコードに置き換えます。

    (コード スニペット -*モデルとデータ アクセス - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> エンティティのコレクションを設定するには使用する必要があります、 **Include**すぎる、アルバムを取得するを指定します。 使用することができます、します。**Single()** LINQ の拡張機能、アルバムの 1 つだけのジャンルがここで予想されるためです。 **Single()** メソッドは、ここでその名前が定義されている値と一致するように 1 つのジャンル オブジェクトを指定するパラメーターとしてラムダ式を受け取ります。
> 
> ジャンル オブジェクトを取得するときにも読み込まれたたいその他の関連エンティティを示すために使用できる機能の利点になります。 この機能は呼**クエリ結果の整形**情報を取得するデータベースにアクセスするために必要な回数を削減することができます。 このシナリオでは、アルバム、ジャンルを取得するをプリフェッチするでしょう。
> 
> クエリが含まれる**Genres.Include (&quot;アルバム&quot;)** 関連アルバムもすることを指定します。 これが 1 つのデータベース要求のジャンルとアルバムの両方のデータを取得するためより効率的なアプリケーションで発生します。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションの実行

このタスクでは、アプリケーションを実行し、特定のジャンルのアルバムをデータベースから取得されます。

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/ストア/参照? ジャンル = Pop**結果をデータベースから取得することを確認します。

    ![ジャンルによる参照](aspnet-mvc-4-models-and-data-access/_static/image24.png "ジャンルによる参照")

    *参照/ストア/参照? ジャンル Pop を =*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>タスク 3 - Id でのアルバムへのアクセス

このタスクでは、アルバムの id を取得するには、前の手順を繰り返します

1. Visual Studio に戻りますが、必要な場合は、ブラウザーを閉じます。 開く、 **StoreController**クラスを変更する、**詳細**アクション メソッド。 この場合に、**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリックします**StoreController.cs**。
2. 変更、**詳細**アルバムの詳細を取得するアクション メソッドがに基づいて、 **Id**します。これを行うには、次のコードに置き換えます。

    (コード スニペット -*モデルとデータ アクセス - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションの実行

このタスクでは、web ブラウザーでアプリケーションを実行し、その id でアルバムの詳細を取得

1. キーを押して**F5**アプリケーションを実行します。
2. ホーム ページで、プロジェクトが開始されます。 URL を変更して **/Store/Details/51**ジャンルを参照して、結果をデータベースから取得することを確認するアルバムを選択します。

    ![詳細を閲覧](aspnet-mvc-4-models-and-data-access/_static/image25.png "の詳細を参照")

    *閲覧/Store/Details/51*

> [!NOTE]
> また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

ASP.NET MVC のモデルとデータ アクセスの基礎を学習するこのハンズオン ラボを使用して、によって、 **Database First**方法だけでなく**Code First**アプローチ。

- そのデータを使用するために、ソリューションにデータベースを追加する方法
- 1 つのハード コーディングされたのではなく、データベースから取得されたデータにビュー テンプレートを提供するコント ローラーを更新する方法
- パラメーターを使用してデータベースを照会する方法
- クエリ結果の整形、データベースへのアクセスの数を削減する機能をより効率的な方法でデータの取得を使用する方法
- Microsoft Entity Framework の Code First と Database First の両方の方法を使用して、モデルとデータベースをリンクする方法

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Windows Azure ポータルにログオン](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure ポータルにログオン")

    *Windows Azure 管理ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image32.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。 簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image33.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](aspnet-mvc-4-models-and-data-access/_static/image34.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](aspnet-mvc-4-models-and-data-access/_static/image35.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](aspnet-mvc-4-models-and-data-access/_static/image36.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。

    ![発行プロファイルのダウンロード web サイト](aspnet-mvc-4-models-and-data-access/_static/image37.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](aspnet-mvc-4-models-and-data-access/_static/image38.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image43.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](aspnet-mvc-4-models-and-data-access/_static/image44.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](aspnet-mvc-4-models-and-data-access/_static/image45.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - 新しいデータベース名を入力します。

     ![ターゲットの接続文字列を構成する](aspnet-mvc-4-models-and-data-access/_static/image47.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](aspnet-mvc-4-models-and-data-access/_static/image48.png "データベース文字列を作成します。")

    *データベースの作成*
7. Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image50.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-models-and-data-access/_static/image51.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](aspnet-mvc-4-models-and-data-access/_static/image52.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-models-and-data-access/_static/image53.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-models-and-data-access/_static/image54.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-models-and-data-access/_static/image55.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-models-and-data-access/_static/image56.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
