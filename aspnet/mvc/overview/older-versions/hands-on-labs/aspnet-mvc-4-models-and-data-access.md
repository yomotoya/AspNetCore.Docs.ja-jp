---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 モデルおよびデータ アクセス |Microsoft ドキュメント
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC の基本的な知識がある前提としています。 前に ASP.NET MVC を使用していない場合をお勧めすると、ASP.NET MVC 4 経由しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 モデルおよびデータ アクセス

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。 使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC 4 基礎**ハンズオン ラボ。

このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。 このラボに固有のプロジェクトは[ASP.NET MVC 4 モデルおよびデータ アクセス](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)です。

**ASP.NET MVC の基本事項**ハンズオン ラボがされて渡そうとしてハード コードされたデータのコント ローラーからテンプレートの表示にします。 しかし、実際の Web アプリケーションを構築するために実際のデータベースを使用する場合があります。

このハンズオン ラボでは、格納および音楽ストア アプリケーションに必要なデータを取得するためにデータベース エンジンを使用する方法を示します。 そのため、既存のデータベースで開始してから、Entity Data Model を作成します。 この演習では、全体で満たすことになります、 **Database First**方法だけでなく**Code First**アプローチです。

ただし、使用することできますも、 **Model First**アプローチ、ツールを使用して、同じモデルの作成、およびそこから、データベースを生成します。

![データベースの最初とします。最初のモデル](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs です。最初のモデルします。")

*データベースの最初とします。最初のモデルします。*

モデルを生成した後にハード コードされたデータを使用する代わりに、データベースから取得されたデータ ストアのビューを提供する StoreController で適切な調整が作成されます。 変更するビュー テンプレート ビュー テンプレートに同じ ViewModels が戻される、StoreController のためこの時点のデータは、データベースから取得されますが、する必要はありません。

**コードの最初の方法**

コードの最初の方法では、フレームワークと組み合わせると、通常のクラスを生成せず、コードからモデルを定義できます。

コードで最初に、モデル オブジェクトで定義した poco から、 &quot;Plain Old CLR Object&quot;です。 した poco からは、単純なの plain クラスを継承がないインターフェイスを実装していないです。 これらのファイルからデータベースを自動的に生成することができます、または既存のデータベースを使用して、コードから、クラスへのマッピングを生成します。

この方法を使用する利点は、すること、モデルを引き続き (ここでは、Entity Framework)、永続化フレームワークから独立した poco からクラスは、マッピング framework と関連していないようです。

> [!NOTE]
> このラボは ASP.NET MVC 4 に基づいており、音楽ストア サンプル アプリケーションのバージョンがカスタマイズされ、このハンズオン ラボで示されている機能のみに合わせて最小限に抑えられます。
> 
> 全体を調査したい場合**Music Store**で検索できるチュートリアル アプリケーション[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。


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

1. [手順 1: データベースに追加します。](#Exercise1)
2. [手順 2: Code First を使用してデータベースを作成します。](#Exercise2)
3. [手順 3: パラメーターを持つデータベースの照会](#Exercise3)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **35 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>手順 1: データベースに追加します。

この演習では、そのデータを使用するために、ソリューションに MusicStore アプリケーションのテーブルを持つデータベースを追加する方法を学習します。 データベースは、モデルを使用して生成され、ソリューションに追加、ハード コーディングされた値を使用する代わりに、データベースから取得したデータをビュー テンプレートを提供する StoreController クラスを変更します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>タスク 1 - データベースに追加します。

このタスクでは、ソリューションに MusicStore アプリケーションのメイン テーブルを作成済みのデータベースを追加します。

1. 開く、**開始**ソリューションにある**ソース/Ex1-AddingADatabaseDBFirst/開始/**フォルダーです。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 追加**MvcMusicStore**データベース ファイル。 このハンズオン ラボと呼ばれる作成済みのデータベースを使用して**MvcMusicStore.mdf**です。 を右クリックして**アプリ\_データ**フォルダーを指す**追加** をクリックし、**既存項目の**です。 参照**\Source\Assets**を選択し、 **MvcMusicStore.mdf**ファイル。

    ![既存の項目を追加する](aspnet-mvc-4-models-and-data-access/_static/image2.png "既存の項目を追加します。")

    *既存の項目を追加します。*

    ![データベース ファイルの MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf データベース ファイル")

    *MvcMusicStore.mdf データベース ファイル*

    データベースがプロジェクトに追加されました。 ソリューション内のデータベースが配置されている場合でもクエリを実行し、別のデータベース サーバーでホストされているように更新できます。

    ![ソリューション エクスプ ローラーでデータベースを MvcMusicStore](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore ソリューション エクスプ ローラーでデータベース")

    *ソリューション エクスプ ローラーで MvcMusicStore データベース*
3. データベースへの接続を確認します。 これを行うをダブルクリックして**MvcMusicStore.mdf**の接続を確立します。

    ![MvcMusicStore.mdf に接続する](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf への接続")

    *MvcMusicStore.mdf への接続*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>タスク 2 - データ モデルを作成します。

このタスクでは、前のタスクに追加されたデータベースとやり取りするデータ モデルを作成します。

1. データベースを表すデータ モデルを作成します。 ソリューション エクスプ ローラーを右クリックで、**モデル**フォルダーを指す**追加**順にクリック**新しい項目の**します。 **新しい項目の追加**ダイアログで、選択、**データ**テンプレートし、 **ADO.NET エンティティ データ モデル**項目。 データ モデル名に変更**StoreDB.edmx**  をクリック**追加**です。

    ![StoreDB ADO.NET エンティティ データ モデルを追加する](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET エンティティ データ モデルを追加します。")

    *StoreDB ADO.NET エンティティ データ モデルを追加します。*
2. **Entity Data Model ウィザード**が表示されます。 このウィザードをモデル層の作成に使用します。 追加された既存のデータベース recentyl に基づいてモデルを作成する必要があります、ために選択**データベースから生成** をクリック**次**です。

    ![モデル コンテンツを選択する](aspnet-mvc-4-models-and-data-access/_static/image7.png "モデル コンテンツを選択します。")

    *モデル コンテンツを選択します。*
3. データベースからモデルを生成するためを使用する接続を指定する必要があります。 をクリックして**新しい接続**です。
4. 選択**Microsoft SQL Server データベース ファイル** をクリック**続行**です。

    ![データ ソースの選択](aspnet-mvc-4-models-and-data-access/_static/image8.png "データ ソースの選択")

    *データ ソース ダイアログを選択します。*
5. をクリックして**参照**にデータベースを選択**MvcMusicStore.mdf**内にある、**アプリ\_データ**フォルダーをクリック**OK**です。

    ![接続プロパティ](aspnet-mvc-4-models-and-data-access/_static/image9.png "接続のプロパティ")

    *接続のプロパティ*
6. 生成されたクラスはエンティティ接続文字列と同じ名前を付ける、名前を変更する必要があります**MusicStoreEntities**  をクリック**次**です。

    ![データ接続を選択する](aspnet-mvc-4-models-and-data-access/_static/image10.png "データ接続を選択します。")

    *データ接続を選択します。*
7. 使用するデータベース オブジェクトを選択します。 エンティティ モデルには、データベースのテーブルだけで使用する、必要に応じて、選択、**テーブル**オプション、およびことを確認して、**モデルに外部キー列を含める**と**複数化または単数化する生成オブジェクト名**オプションも選択します。 モデルの Namespace を変更する**MvcMusicStore.Model**  をクリック**完了**です。

    ![データベース オブジェクトを選択する](aspnet-mvc-4-models-and-data-access/_static/image11.png "データベース オブジェクトを選択します。")

    *データベース オブジェクトを選択します。*

    > [!NOTE]
    > セキュリティ警告ダイアログ ボックスが表示されている場合にをクリックして**OK**をテンプレートを実行し、モデルのエンティティ クラスを生成します。
8. データベースを各テーブルにマップする別のクラスが作成するときに、データベースのエンティティの図が表示されます。 たとえば、**アルバム**テーブルはで表されます、**アルバム**クラス、テーブル内の各列がクラス プロパティにマップされます。 こうとをクエリして、データベース内の行を表すオブジェクトを使用します。

    ![エンティティの図](aspnet-mvc-4-models-and-data-access/_static/image12.png "エンティティの図")

    *エンティティの図*

    > [!NOTE]
    > T4 テンプレート (.tt) は、エンティティ クラスを生成するコードを実行し、同じ名前の既存のクラスが上書きされます。 クラスは、この例で&quot;アルバム&quot;、&quot;ジャンル&quot;と&quot;アーティスト&quot;生成されたコードで上書きされています。


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>タスク 3 - アプリケーションのビルド

このタスクでは、確認をモデルの生成が削除されますが、**アルバム**、**ジャンル**と**アーティスト**モデル クラス プロジェクトが正常にビルドを使用して新しいデータ モデル クラス。

1. 選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。

    ![プロジェクトのビルド](aspnet-mvc-4-models-and-data-access/_static/image13.png "プロジェクトのビルド")

    *プロジェクトのビルド*
2. プロジェクトが正常にビルドします。 なぜ引き続き動作しますか。 データベースのテーブルが削除されたクラスに使用していたプロパティが含まれるフィールドを持つために、それと**アルバム**と**ジャンル**です。

    ![ビルドが成功した](aspnet-mvc-4-models-and-data-access/_static/image14.png "が成功したビルド")

    *ビルドが成功しました*
3. デザイナーはダイアグラム形式でのエンティティが表示されますが、これらは実際に c# クラスです。 展開して、 **StoreDB.edmx**ソリューション エクスプ ローラーでノードし**StoreDB.tt**、新規生成されたエンティティが表示されます。

    ![生成されたファイル](aspnet-mvc-4-models-and-data-access/_static/image15.png "によって生成されたファイル")

    *生成されたファイル*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>タスク 4 - データベースの照会

このタスクでハードコーディングされたデータを使用する代わりにこれはデータベースにクエリ情報を取得するように、StoreController クラスが更新されます。

1. 開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:

    (コード スニペットの*モデルおよびデータ アクセス-Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. **MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。 更新**参照**アクションを取得する方法ですべてのジャンル、**アルバム**です。

    (コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア参照*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. 更新**インデックス**アクション メソッドは、すべてのジャンルを取得します。

    (コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア インデックス*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. 更新**インデックス**アクション メソッドのすべてのジャンルを取得し、一覧にコレクションを変換します。

    (コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションを実行します。

このタスクでは、ストア インデックスのページをジャンルのハードコードされたものではなく、データベースに格納されているようになりました表示するかを確認します。 に、ビューのテンプレートを変更する必要はありません、 **StoreController**はエンティティを取得する同じ以前と同様が、この時点のデータは、データベースから取得されます。

1. キーを押して、ソリューションを再構築**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 いることを確認のメニュー**ジャンル**ハードコードされた一覧で、できなくなり、データがデータベースから直接取得します。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *データベースからジャンルをブラウズ*
3. ここで、ジャンルを参照し、アルバムがデータベースから生成されますを確認します。

    ![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image17.png "データベースからアルバムを参照")

    *データベースからアルバムを参照*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>手順 2: 最初にコードを使用してデータベースを作成します。

この演習では、コードの最初の方法を使用して、MusicStore アプリケーションのテーブルを含むデータベースを作成する方法とそのデータにアクセスする方法を学習します。

モデルが生成されると、ハードコードされた値を使用する代わりに、データベースから取得したデータをビュー テンプレートを提供する StoreController を変更します。

> [!NOTE]
> 手順 1 を完了し、データベースの最初の方法を使用してきた既にする場合は、ここで、別のプロセスで同じ結果を取得する方法を学習します。 演習 1 と同じようなタスクは、読み取りが容易に設定されています。 手順 1 を完了していないコードの最初の方法を学習したい場合は、ここから開始し、このトピックの完全なカバレッジを取得できます。


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>タスク 1 - サンプル データを取り込み

このタスクの作成時に初期状態で 1 コード優先を使用してデータベースにサンプル データを読み込みます。

1. 開く、**開始**ソリューションにある**ソース/Ex2-CreatingADatabaseCodeFirst/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 追加、 **SampleData.cs**ファイルの名前を**モデル**フォルダーです。 を右クリックして**モデル**フォルダーを指す**追加** をクリックし、**既存項目の**です。 参照**\Source\Assets**を選択し、 **SampleData.cs**ファイル。

    ![サンプル データへの追加コード](aspnet-mvc-4-models-and-data-access/_static/image18.png "サンプル データへのコードの追加")

    *サンプル データへのコードを追加します。*
3. 開く、 **Global.asax.cs**ファイルし、次の追加*を使用して*ステートメントです。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 グローバル Asax Using*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. **アプリケーション\_Start()**メソッドは、データベース初期化子を設定する次の行を追加します。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 グローバル Asax SetInitializer*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>タスク 2 - データベースへの接続の構成

プロジェクトにデータベースが既に追加されたに記述、 **Web.config**ファイルの接続文字列。

1. 接続文字列を追加する**Web.config**です。実行するには、開く**Web.config**プロジェクトのルートと置換、接続文字列を次の行で DefaultConnection をという名前で、 **&lt;connectionStrings&gt;**セクション。

    ![Web.config ファイルの場所](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config ファイルの場所")

    *web.config ファイルの場所*


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>タスク 3 - モデルを操作

データベースへの接続を構成しておくことは、データベース テーブルを使用してモデルをリンクします。 このタスクでは、Code First でデータベースにリンクするクラスを作成します。 変更する必要がある、既存モデルの POCO クラスがあることに注意してください。

   > [!NOTE]
> 演習 1 を完了する場合は、ウィザードによって、この手順が行われたことがわかります。 Code First によりデータ エンティティにリンクするクラスを手動で作成されます。


1. POCO モデル クラスを開く**ジャンル**から**モデル**フォルダーをプロジェクトし、ID を含める Int 型のプロパティを使用して、名前を持つ**GenreId**です。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のジャンル*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. これで、POCO のモデル クラスを開く**アルバム**から**モデル**プロジェクト フォルダーと外部キーが含まれて、名前のプロパティを作成する**GenreId**と**ArtistId**です。 このクラスが既にある、 **GenreId**の主キー。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のアルバム*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. POCO モデル クラスを開く**アーティスト**を含めると、 **ArtistId**プロパティです。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のアーティスト*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. 右クリックし、**モデル**プロジェクト フォルダーを選択**追加 |クラス**です。 ファイルの名前を付けます**MusicStoreEntities.cs**です。 をクリックし、**追加します。**

    ![クラスの追加](aspnet-mvc-4-models-and-data-access/_static/image20.png "クラスの追加")

    *新しい項目の追加*

    ![Class2 を追加する](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 を追加します。")

    *クラスの追加*
5. 先ほど作成したクラスを開く**MusicStoreEntities.cs**、名前空間を含めると**System.Data.Entity**と**System.Data.Entity.Infrastructure**です。


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. 拡張するクラスの宣言を置き換える、 **DbContext**クラス: パブリックに宣言**DBSet**オーバーライドと**OnModelCreating**メソッドです。 この手順の後に、Entity Framework でモデルをリンクしているドメイン クラスが表示されます。 そのためには、次のようにクラス コードを置き換えます。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 コード最初 MusicStoreEntities*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>タスク 4 - データベースの照会

これにより、ハードコードされたデータを使用する代わりに、そのデータベースから取得する、このタスクで StoreController クラスが更新されます。

> [!NOTE]
> このタスクでは、手順 1 と共通します。
> 
> 演習 1 が完了した場合これらの手順は、同じ両方の方法で注意してくださいされます (データベースの最初または Code first)。 モデルでは、データをリンクする方法では異なるが、データ エンティティへのアクセスは、コント ローラーから透過的まだ。


1. 開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:

    (コード スニペットの*モデルおよびデータ アクセス-Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. **MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。 更新**参照**アクションを取得する方法ですべてのジャンル、**アルバム**です。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア参照*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. 更新**インデックス**アクション メソッドは、すべてのジャンルを取得します。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア インデックス*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. 更新**インデックス**アクション メソッドのすべてのジャンルを取得し、一覧にコレクションを変換します。

    (コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>タスク 5 - アプリケーションを実行します。

このタスクでは、ストア インデックスのページをジャンルのハードコードされたものではなく、データベースに格納されているようになりました表示するかを確認します。 に、ビューのテンプレートを変更する必要はありません、 **StoreController**同じを返して**StoreIndexViewModel**以前と同様、今度は、データは、データベースから取得されます。

1. キーを押して、ソリューションを再構築**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 いることを確認のメニュー**ジャンル**ハードコードされた一覧で、できなくなり、データがデータベースから直接取得します。

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *データベースからジャンルをブラウズ*
3. ここで、ジャンルを参照し、アルバムがデータベースから生成されますを確認します。

    ![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image23.png "データベースからアルバムを参照")

    *データベースからアルバムを参照*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>手順 3: パラメーターを持つデータベースの照会

この演習では、パラメーターを使用してデータベースを照会する方法およびクエリ結果の整形を使用する方法を学習、番号のデータベースを小さく機能がより効率的な方法でのデータの取得をアクセスします。

> [!NOTE]
> クエリ結果のシェイプの詳細については、次を参照してください。 [msdn の記事](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)です。


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>タスク 1 - 変更 StoreController データベースからアルバムを取得するには

このタスクでは、変更、 **StoreController**特定ジャンルからアルバムを取得するデータベースにアクセスするクラス。

1. 開く、**開始**ソリューションがある、 **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin**コード優先のアプローチを使用する場合はフォルダーまたは**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**データベース優先のアプローチを使用する場合はフォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **StoreController**を変更するクラス、**参照**アクション メソッド。 これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。
3. 変更、**参照**アクション メソッドを特定のジャンルのアルバムを取得します。 これを行うには、次のコードに置き換えます。

    (コード スニペットの*モデルおよびデータ アクセス - Ex3 StoreController BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>タスク 2 - アプリケーションを実行します。

このタスクでは、アプリケーションを実行し、特定のジャンルのアルバムをデータベースから取得します。

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**ストア/参照? ジャンル = Pop**結果は、データベースから取得されていることを確認します。

    ![ジャンル参照](aspnet-mvc-4-models-and-data-access/_static/image24.png "ジャンルの閲覧")

    *Browsing /Store/Browse?genre=Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>タスク 3 - Id でのアルバムへのアクセス

このタスクでは、アルバムの id を取得する前の手順を繰り返します

1. Visual Studio に戻るには、必要な場合は、ブラウザーを閉じます。 開く、 **StoreController**を変更するクラス、**詳細**アクション メソッド。 これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。
2. 変更、**詳細**アルバムの詳細を取得するアクション メソッドがに基づいて、 **Id**です。これを行うには、次のコードに置き換えます。

    (コード スニペットの*モデルおよびデータ アクセス - Ex3 StoreController DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>タスク 4 - アプリケーションを実行します。

このタスクでは、web ブラウザーでアプリケーションを実行し、その id でアルバムの詳細の取得

1. キーを押して**f5 キーを押して**アプリケーションを実行します。
2. ホーム ページで、プロジェクトを開始します。 URL を変更して**/Store/Details/51**か、ジャンルを参照し、結果をデータベースから取得することを確認するアルバムを選択します。

    ![詳細情報を閲覧](aspnet-mvc-4-models-and-data-access/_static/image25.png "の詳細を参照")

    */Store/Details/51 をブラウズ*

> [!NOTE]
> Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

ASP.NET MVC モデルおよびデータ アクセスの基礎を学習したこのハンズオン ラボを完了するを使用して、 **Database First**方法だけでなく**Code First**方法。

- そのデータを使用するために、ソリューションにデータベースを追加する方法
- テンプレートの表示をハード コーディングされた 1 つではなく、データベースから取得したデータを提供するコント ローラーを更新する方法
- パラメーターを使用してデータベースを照会する方法
- クエリ結果の整形、データベースへのアクセスの数を削減する機能より効率的な方法でデータの取得を使用する方法
- Microsoft Entity Framework での Database First と Code First の両方のアプローチを使用して、モデルとデータベースをリンクする方法

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Windows Azure ポータルにログオンする](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure ポータルにログオンします。")

    *Windows Azure 管理ポータルにログオンします。*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image32.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image33.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](aspnet-mvc-4-models-and-data-access/_static/image34.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](aspnet-mvc-4-models-and-data-access/_static/image35.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](aspnet-mvc-4-models-and-data-access/_static/image36.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-models-and-data-access/_static/image37.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](aspnet-mvc-4-models-and-data-access/_static/image38.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image43.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](aspnet-mvc-4-models-and-data-access/_static/image44.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](aspnet-mvc-4-models-and-data-access/_static/image45.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

   - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。
   - **ユーザー名**サーバー管理者のログイン名を入力します。
   - **パスワード**サーバー管理者のログイン パスワードを入力します。
   - 新しいデータベース名を入力します。

     ![対象の接続文字列を構成する](aspnet-mvc-4-models-and-data-access/_static/image47.png "対象の接続文字列を構成します。")

     *対象の接続文字列を構成します。*
6. 次に、 **[OK]**をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](aspnet-mvc-4-models-and-data-access/_static/image48.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]**をクリックします。

    ![SQL データベースを指す接続文字列](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image50.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-models-and-data-access/_static/image51.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](aspnet-mvc-4-models-and-data-access/_static/image52.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-models-and-data-access/_static/image53.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-models-and-data-access/_static/image54.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-models-and-data-access/_static/image55.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-models-and-data-access/_static/image56.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
