---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: "ASP.NET 4.5 のフォームの Web の新機能 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET Web フォームの新しいバージョンには、多数のデータを操作するときに、ユーザー エクスペリエンスを向上させることに重点を置いた機能強化が導入されています。 以前のバージョンのしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 23e38416dc294a1a07cb320cf5ab328fa036d1e8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Asp.net 4.5 Web フォームの新機能
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> ASP.NET Web フォームの新しいバージョンには、多数のデータを操作するときに、ユーザー エクスペリエンスを向上させることに重点を置いた機能強化が導入されています。
> 
> Web フォーム、データ バインディングを使用してオブジェクトのメンバーの値を出力する場合の以前のバージョンでは、Bind() または Eval() データ バインディング式を使用します。 ASP.NET の新しいバージョンでは、データの種類、コントロールがしようとする新しい ItemType プロパティを使用してにバインドを宣言できます。 このプロパティを設定すると、その厳密に型指定された変数を使用して、IntelliSense、メンバー ナビゲーション、およびコンパイル時のチェックなど、Visual Studio 開発環境のすべてのメリットを受信することが有効になります。
> 
> データ バインド コントロールを今すぐも指定できます、独自のカスタム メソッドを選択すると、更新、削除して、データの挿入のページ コントロールと、アプリケーション ロジックの間の相互作用を簡略化すること。 さらに、メソッドの型パラメーターに直接ページからデータをマップすることを意味する ASP.NET には、モデル バインディング機能が追加されました。
> 
> ユーザー入力を検証する必要がありますも Web フォームの最新バージョンを簡単にします。 検証属性を持つモデル クラスを今すぐ注釈を付けることができます、 **System.ComponentModel.DataAnnotations**名前空間と、すべてのサイトを制御する要求は、その情報を使用してユーザー入力を検証します。 Web フォームでのクライアント側の検証はクライアント側コード クリーナーと控えめな JavaScript 機能を提供する、jQuery と統合されています。
> 
> 要求の検証 領域での機能強化に加えられたを選択して、アプリケーションの特定の部分で要求の検証をオフにするまたは無効化された要求データを読みやすきます。
> 
> 一部の機能強化に加えられた Web フォーム サーバー コントロールの HTML5 の新機能を利用します。
> 
> - TextBox コントロールの TextMode プロパティは、電子メール、datetime、およびなどのように、新しい HTML5 入力型をサポートするために更新されました。
> - ファイルアップロード コントロールは、この機能は HTML5 のブラウザーから複数のファイルのアップロードをサポートします。
> - 検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。
> - ここで URL を表す属性を持つ新しい HTML5 要素をサポートして runat =&quot;サーバー&quot;です。 その結果、規則を使用できます ASP.NET URL のパスと同様に、~、アプリケーション ルートを表す演算子 (たとえば、&lt;ビデオ runat =&quot;サーバー&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/ビデオ&gt;)。
> - UpdatePanel コントロールは、HTML5 ポストされた内容は、入力フィールドをサポートするために修正されています。
> 
> 公式の ASP.NET ポータルで ASP.NET WebForms 4.5 での新機能の例についてを見つけることができます: [ASP.NET 4.5 と Visual Studio 2012 の新](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。


<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- 厳密に型指定されたデータ バインディング式を使用します。
- Web フォームの新しいモデル バインド機能を使用してください。
- ページのデータを分離コードのメソッドにマップするための値プロバイダーを使用します。
- データ注釈を使用して、ユーザー入力の検証の
- Web フォームでの jQuery を使用したクライアント側検証を unobstrusive advange になります
- 詳細な要求の検証を実装します。
- 非同期のページが Web フォームでの処理を実装します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。 実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [手順 1: モデルの ASP.NET Web フォームでのバインディング](#Exercise1)
2. [手順 2: データの検証](#Exercise2)
3. [手順 3: 非同期ページの処理に ASP.NET Web フォームします。](#Exercise3)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **60 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>手順 1: モデルの ASP.NET Web フォームでのバインディング

ASP.NET Web フォームの新しいバージョンでは、さまざまなデータを操作するときに、エクスペリエンスを向上させることに重点を置いた機能強化について説明します。 この演習では、厳密に型指定のデータ コントロールの概要について説明し、モデル バインディングします。

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>タスク 1 の厳密に型指定のデータ バインディングの使用

このタスクでは、新しい厳密に型指定されたバインド ASP.NET 4.5 で利用可能なことがわかります。

1. 開く、**開始**ソリューションにある**ソース/Ex1-ModelBinding/開始/**フォルダーです。

    1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

    > [!NOTE]
    > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **Customers.aspx**ページ。 メイン コントロールに番号なしのリストを置くし、各顧客の一覧を表示するための内部リピータ コントロールなどがあります。 リピータ名を設定します**customersRepeater**次のコードに示すようにします。

    以前のバージョンの Web フォーム、データ バインディングをしているオブジェクトのメンバーの値を出力するデータ バインディングを使用するは式を使用するデータ バインド、Eval メソッドの呼び出しと共に渡すメンバーの名前を文字列として。

    実行時に、Eval にこれらの呼び出しは現在バインドされているオブジェクトに対してリフレクションを使用して、指定した名前を持つメンバーの値を読み取るし、結果を HTML で表示します。 この方法では、任意、unshaped データに対してデータ バインドするには非常に簡単です。

    残念ながら、コンパイル時のチェック (定義へ移動) などのナビゲーションのサポート、メンバー名の IntelliSense をなど、Visual Studio で効果的に開発時の機能の多くが失われます。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. 開く、 **Customers.aspx.cs**ファイル。
4. 次の追加ステートメントを使用します。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. **ページ\_ロード**メソッド、顧客のリストを持つリピータを設定するコードを追加します。

    (コード スニペットの*フォーム ラボ - Ex01 - バインド顧客のデータ ソースの Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    ソリューションでは、CodeFirst と共に EntityFramework を使用して作成し、データベースにアクセスします。 次のコードで、customersRepeater をデータベースからすべての顧客を返す具体化されたクエリにバインドされています。
6. キーを押して**f5 キーを押して**にソリューションを実行しに移動、**顧客**リピータの動作を表示するページ。 ソリューションは CodeFirst を使用して、データベースが作成され、アプリケーションを実行するときに、ローカル SQL Express インスタンスに設定されます。

    ![リピータの顧客を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "リピータと顧客の一覧を表示します。")

    *リピータの顧客を一覧表示します。*

    > [!NOTE]
    > Visual Studio 2012、IIS Express は、既定 Web 開発サーバーです。
7. ブラウザーを終了し、Visual Studio に戻ります。
8. 厳密に型指定のバインディングを使用する実装を置き換えるようになりました。 開く、 **Customers.aspx**ページ使用して、新しい**ItemType**属性を設定する中継ぎ局で、**顧客**バインディングの種類として。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    ItemType プロパティでは、データの種類コントロールにバインドするには、使用できるように厳密に型指定されたデータ バインド コントロール内のバインドを宣言することができます。
9. 次のコードでコンテンツの ItemTemplate を置き換えます。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    これらのアプローチの 1 つの欠点は、呼び出し Eval() および Bind() の遅延バインディングのプロパティの名前を表す文字列を渡す意味です。 つまり、メンバー名、(定義へ移動) のようなコード ナビゲーションのサポートもコンパイル時チェックのサポートの Intellisense を取得しません。

    ItemType プロパティを設定すると、データ バインディング式のスコープに、生成する 2 つの新しい型指定された変数:**項目**と**BindItem**です。 データ バインディング式でこれらの厳密に型指定された変数を使用し、Visual Studio 開発環境のすべてのメリットを取得できます。

    &quot; **:** &quot;式で使用が自動的に HTML エンコード (クロスサイト スクリプティング攻撃など) のセキュリティの問題を避けるために出力します。 この表記できた .NET 4 以降の書き込み、応答が現在もデータ バインディング式で使用可能なです。

    > [!NOTE]
    > 項目のメンバーは、一方向のバインドに対して機能します。 双方向のバインドを実行する場合、 **BindItem**メンバー。

    ![厳密に型指定されたバインディングでの IntelliSense サポート](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "厳密に型指定されたバインディングでの IntelliSense のサポート")

    *厳密に型指定されたバインディングでの IntelliSense のサポート*
10. キーを押して**f5 キーを押して**にソリューションを実行し、期待どおりに変更を確認する [顧客] ページに移動します。

    ![顧客の詳細を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "顧客の詳細を一覧表示します。")

    *顧客の詳細を一覧表示します。*
11. ブラウザーを終了し、Visual Studio に戻ります。

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>作業 2: Web フォームでのバインディング紹介モデル

以前のバージョンの ASP.NET Web フォーム、取得して、データの更新の両方に双方向データ バインディングの実行に必要な場合にするに必要なデータ ソース オブジェクトを使用します。 これは、オブジェクト データ ソース、SQL データ ソース、LINQ のデータ ソースなど。 ただし場合、シナリオでは、カスタム コードを必要なデータを処理するため、オブジェクト データ ソースを使用するために必要ないくつかの欠点になったこの たとえば、複合型を避けるために必要なし、検証ロジックを実行するときに例外を処理する必要があります。

ASP.NET Web フォームの新しいバージョンでは、データ バインド コントロールは、モデル バインディングをサポートします。 つまり、できるし、指定することを選択、更新、挿入、分離コード ファイルまたは別のクラスからロジックを呼び出すにはデータ バインド コントロールで直接メソッドを削除します。

これについては、使用する、GridView、new を使用して、製品カテゴリを一覧表示する**SelectMethod**属性。 この属性では、GridView のデータを取得する方法を指定することができます。

1. 開く、**繰り返し**ページし、が含まれて、 **GridView**です。 GridView は、厳密に型指定されたバインディングを使用し、並べ替えとページングを有効にする次のように構成します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. 使用して、新しい**SelectMethod**を呼び出す GridView を構成する属性、 **GetCategories**データを選択するメソッド。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. 開く、 **Products.aspx.cs**分離コード ファイルし、次の追加ステートメントを使用します。

    (コード スニペットの*フォーム ラボ - Ex01 - 名前空間を Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. プライベート メンバーを追加、**製品**クラスし、の新しいインスタンスを割り当てる**ProductsContext**です。 このプロパティでは、データベースに接続することができます、Entity Framework データ コンテキストを格納します。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. 作成、 **GetCategories** LINQ を使用してカテゴリの一覧を取得します。 クエリが含まれます、**製品**プロパティ GridView は、各カテゴリの製品の金額を表示できるようにします。 メソッドが後でページのライフ サイクルに実行されるにクエリを表す生の IQueryable オブジェクトを返すことに注意してください。

    (コード スニペットの*Web フォーム ラボ - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > 以前のバージョンの ASP.NET Web フォームを有効にする並べ替えとページング、オブジェクト データ ソースのコンテキスト内で、自分のリポジトリ ロジックを使用して必要なを独自のカスタム コードを記述し、必要なすべてのパラメーターを受信します。 ここで、データ バインディングのメソッドは、IQueryable を返すことができ、これは、クエリを表します、実行するには、まだ ASP.NET を守ることが適切な並べ替えを追加するクエリとページングのパラメーターを変更します。
6. キーを押して**f5 キーを押して**サイトのデバッグを開始し、[製品] ページに移動します。 GridView に、メソッドによって返されるカテゴリが表示されているはずです。

    ![モデル バインディングを使用して GridView を設定する](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "モデル バインディングを使用して GridView を設定します。")

    *モデル バインディングを使用して GridView を設定します。*
7. キーを押して**SHIFT**+**f5 キーを押して**デバッグを停止します。

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>タスク 3 - モデル バインディング内の値プロバイダー

モデル バインドは、データ バインド コントロールで直接データを操作するカスタム メソッドを指定することができますだけでなく、これらのメソッドのパラメーターに、ページからデータをマップすることもできます。 メソッド パラメーターに値のデータ ソースを指定するのに値プロバイダー属性を使用することができます。 例:

- ページ上のコントロール
- クエリ文字列の値
- データの表示
- セッションの状態
- クッキー
- ポストされたフォーム データ
- ビューの状態
- カスタム値プロバイダーはもサポートされます。

ASP.NET MVC 4 を使用している場合は、モデル バインディングのサポートは似ていますがわかります。 実際には、これらの機能が ASP.NET MVC から取得されに移動された、 **System.Web**も Web フォームで使用することであるアセンブリ。

このタスクではモデル バインディングで filter パラメーターを受け取る各カテゴリの製品の量によって、結果をフィルター処理する GridView 更新されます。

1. 戻り、**繰り返し**ページ。
2. GridView の上部に追加、**ラベル**と**ComboBox**を次に示すように各カテゴリの製品の数を選択します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. 追加、 **EmptyDataTemplate**製品数を選択したカテゴリが存在しない場合は、メッセージを表示する GridView にします。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. 開く、 **Products.aspx.cs**分離コードと、次の追加ステートメントを使用します。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. 変更、 **GetCategories**を整数値を受け取るメソッド**minProductsCount**引数と、返される結果をフィルター処理します。 これを行うには、次のコードでメソッドを置き換えます。

    (コード スニペットの*Web フォーム ラボ - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    新しい**[コントロール]**属性を**minProductsCount**引数が ASP.NET ページにコントロールを使用して、その値を設定する必要が次のトピックを使用できます。 ASP.NET の引数 (minProductsCount) の名前に一致する任意のコントロールを検索および、必要なマッピングとコントロールの値を持つパラメーターを入力への変換を実行します。

    また、属性は、値を取得する場所からコントロールを指定することができますをオーバー ロードされたコンス トラクターを提供します。

    > [!NOTE]
    > データ バインディング機能の 1 つの目的は、ページの対話用に作成する必要があるコードの量を削減します。 別に、[管理] の値プロバイダーには、メソッドのパラメーターでその他のモデル バインド プロバイダーを使用することができます。 それらの一部は、タスクの概要に表示されます。
6. キーを押して**f5 キーを押して**サイトのデバッグを開始し、[製品] ページに移動します。 ドロップダウン リストで製品の数を選択し、GridView の現在の更新方法に注意してください。

    ![ドロップダウン リストの値を持つ GridView をフィルタ リング](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView、ドロップダウン リストの値をフィルター処理")

    *ドロップダウン リストの値を持つ GridView をフィルター処理*
7. デバッグを停止します。

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>タスク 4 - フィルターのバインドを使用してモデル

このタスクは、2 番目の子を選択したカテゴリ内の製品を表示する GridView を追加します。

1. 開く、**繰り返し**ページおよび更新のカテゴリを選択 ボタンを自動生成する GridView。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. 1 秒あたりの追加**GridView**という**productsGrid**下部にあります。 設定、 **ItemType**に**WebFormsLab.Model.Product**、 **DataKeyNames**に**ProductId**と**SelectMethod**に**GetProducts**です。 設定**AutoGenerateColumns**に**false** ProductId、ProductName、説明、および UnitPrice の列を追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. 開く、 **Products.aspx.cs**分離コード ファイル。 実装、 **GetProducts** GridView のカテゴリから、カテゴリの ID を受信し、製品をフィルター処理するメソッド。 モデル バインディングで選択した行を使用してパラメーター値が設定されます、 **categoriesGrid**です。 コントロールの名前と引数名が一致しないために、コントロール値プロバイダーでコントロールの名前を指定する必要があります。

    (コード スニペットの*Web フォーム ラボ - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > この方法では簡単に単位にこれらのメソッドをテストします。 Web フォームがない実行されている場合、単体テスト コンテキストで、[コントロール] 属性では、特定のアクションは実行しません。
4. 開く、**繰り返し**ページおよび GridView の製品を検索します。 選択した製品を編集するためのリンクを表示する GridView の製品を更新します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. 開く、 **ProductDetails.aspx**分離コード ページし、置換、 **SelectProduct**メソッドを次のコード。

    (コード スニペットの*Web フォーム ラボ - Ex01 - SelectProduct メソッド*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > 注意して、 **[QueryString]**属性を使用して、クエリ文字列内の productId パラメーターからメソッド パラメーターを入力します。
6. キーを押して**f5 キーを押して**サイトのデバッグを開始し、[製品] ページに移動します。 GridView のカテゴリから任意のカテゴリを選択し、GridView の製品が更新されることに注意してください。

    ![選択したカテゴリの製品を示す](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "選択したカテゴリの製品を表示")

    *選択したカテゴリの製品を表示*
7. クリックして、**ビュー** ProductDetails.aspx ページを開くに製品をリンクします。

    ページが、クエリ文字列から productId パラメーターを使用して、SelectMethod で製品を取得することに注意してください。

    ![製品の詳細を表示する](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "製品詳細を表示します。")

    *製品の詳細を表示します。*

    > [!NOTE]
    > HTML の説明を入力する機能は、次の手順で実装されます。

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>タスク 5 - 更新操作のためのバインドを使用してモデル

前のタスクでは、主にデータを選択するモデル バインドを使用した、このタスクでは、更新操作で、モデル バインディングを使用する方法を学習します。

ユーザーがカテゴリを更新できるようにする GridView のカテゴリが更新されます。

1. 開く、**繰り返し**ページおよび更新のカテゴリの [編集] ボタンを自動生成して、新しい GridView **UpdateMethod**を指定する属性、 **UpdateCategory**メソッドを選択した項目を更新します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    GridView で DataKeyNames 属性を定義するモデル バインド オブジェクトを一意に識別するメンバーと、update メソッドを受け取るには、少なくともパラメーターであるため、します。
2. 開く、 **Products.aspx.cs**分離コード ファイルと実装、 **UpdateCategory**メソッドです。 メソッドは、現在のカテゴリを読み込む、GridView から値を設定し、カテゴリを更新するカテゴリ ID を受け取るはずです。

    (コード スニペットの*Web フォーム ラボ - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    新しい**TryUpdateModel**ページ クラスのメソッドは、ページにコントロールから値を使用して、モデル オブジェクトを配置します。 この場合に編集されている現在の GridView 行から更新された値に置き換え、**カテゴリ**オブジェクト。

    > [!NOTE]
    > 次の手順では、オブジェクトを編集するときに、ユーザーが入力したデータの検証 ModelState.IsValid の使用方法を説明します。
3. サイトを実行し、[製品] ページに進みます。 カテゴリを編集します。 新しい名前を入力し、をクリックして**更新**変更を保持します。

    ![編集するカテゴリ](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "カテゴリを編集")

    *編集するカテゴリ*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>手順 2: データの検証

この演習では、ASP.NET 4.5 で新しいデータの検証機能について学習します。 Web フォームで新しい控えめな検証機能を確認します。 ユーザー入力の検証に、アプリケーション モデルのクラスにデータ注釈を使用して、最後に、オンまたはオフを個別のコントロール ページの要求の検証を有効にする方法を学習します。

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>タスク 1 - 控えめな検証

複雑なデータの検証コントロールを含むフォームは、ページで、コードの約 60% を表すことができますが多すぎる JavaScript コードを生成する傾向があります。 控えめな検証が有効になっている、状態では、HTML コードは、クリーナーと整備に探します。

このセクションでは、両方の構成によって生成された HTML コードを比較する ASP.NET での控えめな検証を有効にします。

1. 開く**Visual Studio 2012**を開くと、**開始**にソリューションがある、 **Source\Ex2 Validation\Begin**このラボのフォルダーです。 また、前述の手順から既存のソリューションで作業を続行できます。

    1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 ソリューション エクスプ ローラーで、これをクリックして、 **WebFormsLab**プロジェクト**NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

    > [!NOTE]
    > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. キーを押して**f5 キーを押して**web アプリケーションを起動します。 [ページの顧客にしてください] をクリックして、**新しい顧客を追加**リンクします。
3. ブラウザー ページで、右クリックし **ソースの表示**オプションを開くには、アプリケーションによって生成された HTML コード。

    ![ページの HTML コードを示す](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "ページの HTML コードの表示")

    *ページの HTML コードの表示*
4. ページのソース コードをスクロールし、ASP.NET が JavaScript コードとデータの検証コントロールがページで、検証を行い、エラー一覧を表示、挿入ことに注意してください。

    ![CustomerDetails ページでの JavaScript コードの検証](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails ページの検証の JavaScript コード")

    *CustomerDetails ページでの JavaScript コードの検証*
5. ブラウザーを終了し、Visual Studio に戻ります。
6. 今すぐ控えめな検証を有効になります。 開いている**Web.Config**を見つけて選択**ValidationSettings:UnobtrusiveValidationMode**キー、 **AppSettings**セクション**です。** キーの値を設定**WebForms**です。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > このプロパティを設定することもできます、 &quot;**ページ\_ロード**&quot;場合もいくつかのページに対してのみ控えめな検証を有効にするイベントです。
7. 開いている**CustomerDetails.aspx**とキーを押します**f5 キーを押して**Web アプリケーションを起動します。
8. 開くには、IE developer tools F12 キーを押します。 開発者用ツールが表示されたら、[スクリプト] タブを選択します。選択**CustomerDetails.aspx** take し、メニューからは、スクリプトがページでの jQuery の実行に必要なことに注意してくださいをローカル サイトからブラウザーに読み込まれています。

    ![ローカル IIS サーバーから直接ファイルを読み込み、jQuery JavaScript](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "ローカル IIS サーバーから直接ファイル jQuery JavaScript の読み込み")

    *ローカル IIS サーバーから直接、jQuery JavaScript ファイルの読み込み*
9. Visual Studio に戻るには、ブラウザーを閉じます。 開く、 **Site.Master**ファイルを再びと検索、 **ScriptManager**です。 属性を追加**EnableCdn** 、値を持つプロパティ**True**です。 これで、ローカル サイトの URL ではなく、オンラインの URL から読み込まれる jQuery です。
10. 開いている**CustomerDetails.aspx** Visual Studio でします。 サイトを実行する F5 キーを押します。 Internet Explorer を開くと後、は、開発者ツールを開く F12 キーを押します。 選択、**スクリプト**タブをクリックし、ドロップダウン リストを見て、します。 JQuery JavaScript ファイルが読み込まれなくなりますされている、ローカル サイトからからではなくオンライン jQuery CDN に注意してください。

    ![CDN からファイルを読み込み、jQuery JavaScript](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "CDN からファイルを読み込み、jQuery JavaScript")

    *CDN からの jQuery JavaScript ファイルの読み込み*
11. ブラウザーで表示ソース オプションを使用して HTML ページのソース コードを開きます。 控えめな検証を有効にすると ASP.NET に置き換えること、挿入されたコードを JavaScript データ通知\*属性。

    ![控えめな検証コード](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "控えめな検証コード")

    *控えめな検証コード*

    > [!NOTE]
    > この例では、HTML および JavaScript 数行だけに概要データの注釈で検証がどのように簡略化されたかを確認しました。 以前は、控えめな検証なしで追加すると、複数の検証コントロールのサイズが大きく、JavaScript の検証コードが増えます。

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>タスク 2 - データ注釈を使用してモデルを検証します。

ASP.NET 4.5 には、Web フォームのデータ注釈検証が導入されています。 各入力の検証コントロールはなく、ようになりましたモデル クラスの制約を定義し、すべての web アプリケーションでして使用できます。 このセクションでは、顧客の新規作成/編集フォームを検証するためデータ注釈を使用する方法を学習します。

1. 開いている**CustomerDetail.aspx**ページ。 顧客名で 2 つ目の名前に注意してください、 **EditItemTemplate**と**後**のセクションでは、RequiredFieldValidator コントロールを使用して検証されます。 個々 の検証は、特定の条件に関連付けられている多くの検証コントロールをチェックする条件として含める必要があります。
2. Customer モデル クラスを検証するデータ注釈を追加します。 開いている**Customer.cs**クラス内で、**モデル**フォルダーと*装飾*データ注釈の属性を使用して各プロパティ。

    (コード スニペットの*フォーム ラボ - Ex02 - データ注釈を Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 は、既存のデータの注釈のコレクションを拡張します。 使用できるデータ注釈をいくつか: [CreditCard]、[電話]、[EmailAddress] [範囲] [比較]、[Url] [FileExtensions]、[Required]、[キー]、[正規表現] です。
    > 
    > いくつかの使用例:
    > 
    > [キー]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > 各属性内の独自のエラー メッセージを定義することもできます。
3. 開いている**CustomerDetails.aspx**およびで EditItemTemplate と後のセクションで、フォーム ビュー コントロールの最初と最後の名前のフィールドのすべての RequiredFieldvalidators を削除します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > データ注釈を使用する利点の 1 つは、アプリケーション ページで、検証ロジックが重複していないことです。 モデルでは、一度定義し、データを操作するすべてのアプリケーション ページで使用するとします。
4. 開いている**CustomerDetails.aspx**分離コードと SaveCustomer メソッドを検索します。 このメソッドは、新しい顧客を挿入するときに呼び出されるし、フォーム ビュー コントロールの値から顧客パラメーターを受け取ります。 ページ コントロールとパラメーター オブジェクトが発生間のマッピング、ASP.NET は実行時に、データ注釈をすべてに対してモデルの検証は属性を存在する場合に、検出されると、エラーが発生 ModelState ディクショナリを入力します。

    ModelState.IsValid には、検証を実行した後、モデルのすべてのフィールドが有効な場合は true を返しますのみです。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. 追加、 **ValidationSummary**モデル エラーの一覧を表示する CustomerDetails ページの最後に制御します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** 、ValidationSummary に新しいプロパティをコントロールに設定した場合によっては、 **true**コントロールが ModelState ディクショナリからのエラーを表示します。 これらのエラーは、データ注釈検証から取得されます。
6. キーを押して**f5 キーを押して**Web アプリケーションを実行します。 いくつかの不適切な値をフォームに入力し、をクリックして**保存**検証を実行します。 エラーの下部にある概要を確認します。

    ![データ注釈を使用した検証](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "データ注釈を使用した検証")

    *データ注釈を使用した検証*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>タスク 3 - ModelState でカスタム データベース エラーの処理

以前のバージョンの Web フォームでは、長すぎる文字列または一意キー違反などのデータベース エラーの処理が係わりますリポジトリ コードで例外をスローし、分離コード エラーとして表示する上で、例外を処理します。 比較的単純な処理を行うには、大量のコードが必要です。

4.5 では Web フォーム、一貫した方法で、ページで、モデルとは、データベースからのエラーを表示する ModelState オブジェクトを使用できます。

このタスクは正しくデータベース例外の処理および ModelState オブジェクトを使用して、ユーザーに適切なメッセージを表示するコードを追加します。

1. アプリケーションがまだ実行されているときに重複する値を使用してカテゴリの名前を更新してください。

    ![重複する名前のカテゴリを更新](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "重複する名前を持つ、カテゴリの更新")

    *重複する名前を持つ、カテゴリの更新*

    ためにスローされる例外に注意してください、&quot;一意&quot;の制約、 **CategoryName**列です。

    ![重複するカテゴリ名については、例外](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "重複するカテゴリ名の例外")

    *重複するカテゴリ名の例外*
2. デバッグを停止します。 **Products.aspx.cs**分離コード ファイル、更新、 **UpdateCategory** db によってスローされる例外を処理するメソッド。SaveChanges() メソッドの呼び出しを追加すると、エラー、 **ModelState**オブジェクト。

    新しい**TryUpdateModel**メソッドは、ユーザーによって指定されたフォーム データを使用して、データベースから取得したカテゴリ オブジェクトを更新します。

    (コード スニペットの*Web フォーム ラボ - Ex02 - UpdateCategory ハンドル エラー*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > 理想的には、DbUpdateException の原因を特定し、根本原因が一意キー制約の違反を確認する必要があります。
3. 開いている**繰り返し**を追加し、 **ValidationSummary**モデル エラーの一覧を表示する GridView のカテゴリの下のコントロールです。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. サイトを実行し、[製品] ページに進みます。 重複する値を使用してカテゴリの名前を更新しようとしてください。

    通知を例外が処理しで、エラー メッセージが表示されます、 **ValidationSummary**コントロール。

    ![カテゴリのエラーが重複して](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "カテゴリのエラーを重複しています")

    *重複するカテゴリのエラー*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>タスク 4 - 要求の ASP.NET Web フォーム 4.5 での検証

ASP.NET の要求の検証機能は、特定のレベルのクロス サイト スクリプト (XSS) 攻撃に対する既定の保護を提供します。 ASP.NET の以前のバージョンで要求の検証が既定で有効になっているし、ページ全体に対してのみ無効にする可能性があります。 ASP.NET Web フォームの新しいバージョンで今すぐ 1 つのコントロールの要求の検証を無効にする、限定的な要求の検証またはを実行できます (これを行う場合は慎重に!) が検証されていない要求のデータにアクセスします。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**にデバッグを行わず、サイトを起動し、[製品] ページに移動します。 カテゴリを選択し、クリックして、**編集**製品のいずれかのリンク。
2. 危険性のあるコンテンツを含む、HTML タグを含むインスタンスの説明を入力します。 要求の検証のためにスローされる例外の通知を実行します。

    ![危険性のあるコンテンツを持つ製品を編集](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "危険性のあるコンテンツを持つ製品の編集")

    *製品は危険性のあるコンテンツの編集*

    ![要求の検証のためにスローされる例外](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "要求の検証のためにスローされる例外")

    *要求の検証のためにスローされる例外*
3. ページを閉じるし、Visual Studio で、キーを押します**shift キーを押しながら f5 キーを押して**デバッグを停止します。
4. 開く、 **ProductDetails.aspx**ページし、検索、**説明** テキスト ボックス。
5. 新しい追加**ValidateRequestMode**プロパティ テキスト ボックスをその値に設定し、**無効になっている**です。

    新しい**ValidateRequestMode**属性では、各コントロールに細かく要求の検証を無効にすることができます。 これは、HTML コードが表示される可能性がありますが、残りのページの検証を維持する入力を使用する場合に便利です。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. キーを押して**f5 キーを押して**web アプリケーションを実行します。 製品の編集 ページを再度開くし、HTML タグを含む製品の説明を完了します。 説明への HTML コンテンツを追加できるようになりましたことに注意してください。

    ![要求の検証、製品の説明を無効になっている](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "要求の検証、製品の説明を無効になっています")

    *製品の説明を無効になっている要求の検証*

    > [!NOTE]
    > 実稼働アプリケーションでは、唯一の安全な HTML タグが入力したかどうかを確認するユーザーが入力した HTML コードを校正する必要があります (たとえば、あるありません&lt;スクリプト&gt;タグ)。 これを行うには、使用することができます[Microsoft Web Protection ライブラリ](https://www.nuget.org/packages/AntiXSS)です。
7. 製品を再度編集します。 [名前] フィールドでの HTML コードを入力し、をクリックして**保存**です。 [説明] フィールドの要求の検証を無効にのみ、re フィールドの残りの部分は、危険性のあるコンテンツに対してまだ検証に注意してください。

    ![要求の残りのフィールドで有効になっている検証](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "要求の検証の残りのフィールドで有効になっています。")

    *残りのフィールドで有効になっている要求の検証*

    ASP.NET Web フォーム 4.5 には、遅延要求の検証を実行する新しい要求の検証モードが含まれています。 要求の検証モードを設定**4.5**コードへのアクセスの場合は、 *Request.Form [&quot;キー&quot;]*、ASP.NET 4.5 の要求の検証はのみトリガー要求の検証フォーム コレクションでその要素を特定します。

    さらに、ASP.NET 4.5 には Microsoft アンチ XSS ライブラリ v4.0 からコア エンコード ルーチンが含まれています。 エンコードのルーチンは、新しいによって実装されるアンチ XSS *AntiXssEncoder*型については、新しい**System.Web.Security.AntiXss**名前空間。 **EncoderType**パラメーターを使用するように構成*AntiXssEncoder*、すべての出力を新しいエンコード ルーチンを使用して ASP.NET 内で自動的にエンコードします。
8. ASP.NET 4.5 の要求の検証には、検証されていないデータにアクセスする要求もサポートしています。 ASP.NET 4.5 に新しいコレクション プロパティの追加、 **HttpRequest**と呼ばれるオブジェクト**Unvalidated**です。 移動したとき**HttpRequest.Unvalidated**すべての要求のデータ、フォーム、クエリー文字列、Cookie、Url などの共通部分にアクセス権があります。

    ![Request.Unvalidated オブジェクト](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated オブジェクト")

    *Request.Unvalidated オブジェクト*

    > [!NOTE]
    > **注意して HttpRequest.Unvalidated プロパティを使用してください。** 危険なテキストがないラウンドト リップと無防備な顧客に表示されることを確認要求の生データを慎重にカスタム検証を実行することを確認してください。

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>手順 3: 非同期ページの処理に ASP.NET Web フォームします。

この演習では、ASP.NET Web フォームの機能を処理する新しい非同期ページに導入される予定です。

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>タスク 1: 製品の更新の詳細をアップロードし、イメージを表示する ページ

このタスクでは、ユーザーが製品の画像の URL を指定し、読み取り専用ビューで表示できるようにする製品の詳細ページが更新されます。 同期的にダウンロードして、指定されたイメージのローカル コピーを作成します。 次のタスクでは、非同期的に動作させるのには、この実装を更新します。

1. 開いている**Visual Studio 2012**と読み込み、**開始**にソリューションがある**Source\Ex3 Async\Begin**このラボのフォルダーからです。 代わりに、前の演習から既存のソリューションで作業を続行できます。

    1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 ソリューション エクスプ ローラーで、これをクリックして、 **WebFormsLab**プロジェクトし、選択**NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

    > [!NOTE]
    > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **ProductDetails.aspx**ソース ページし、製品の画像を表示するフォーム ビューの ItemTemplate でフィールドを追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. FormView の EditTemplate で画像の URL を指定するフィールドを追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. 開く、 **ProductDetails.aspx.cs**分離コード ファイルし、次の名前空間ディレクティブを追加します。

    (コード スニペットの*フォーム ラボ - Ex03 - 名前空間を Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. 作成、 **UpdateProductImage**イメージを格納するリモート ローカル メソッド**イメージ**フォルダーと、新しいイメージの場所の値を持つ製品エンティティを更新します。

    (コード スニペットの*Web フォーム ラボ - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. 更新プログラム、 **UpdateProduct**メソッドを呼び出す、 **UpdateProductImage**メソッドです。

    (コード スニペットの*Web フォーム ラボ - Ex03 - UpdateProductImage 呼び出し*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. アプリケーションを実行し、製品の画像をアップロードしようとしてください。 たとえば、Office クリップ アートから次の画像の URL を使用することができます: [ [http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/en-us/images/MB900437099.jpg)

    ![製品のイメージを設定](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "製品用のイメージの設定")

    *製品のイメージの設定*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>作業 2: 非同期製品の詳細 ページに処理を追加します。

このタスクで非同期的に動作させるのに製品の詳細ページが更新されます。 イメージのダウンロード プロセスの-実行時間の長いタスクは、ASP.NET 4.5 ページの非同期処理を使用して強化されます。

Web アプリケーションで非同期のメソッドは、ASP.NET スレッド プールの使用方法を最適化するために使用できます。 Asp.net がありますを推進のスレッド プール内のスレッドの限られた数を要求するためとすべてのスレッドがビジー状態、、ASP.NET が起動新しい要求を拒否する、アプリケーションのエラー メッセージを送信され、サイトを使用できなくなります。

Web サイトで時間のかかる操作が長時間割り当てられているスレッドを占有するために、非同期プログラミングの優れた候補です。 これには、実行時間の長い要求、多数のさまざまな要素を含むページおよびオフライン操作、そのようなデータベースに対するクエリまたは外部 web サーバーへのアクセスを必要とするページが含まれます。 利点は、ページの処理中に、これらの操作では、非同期メソッドを使用する場合、スレッドは解放され、スレッドに返されるプールし、新しいページ要求への参加に使用できます。 つまり、ページで、スレッド プールから 1 つのスレッドで処理を開始し、非同期処理が完了した後に、別の処理が完了することがあります。

1. 開く、 **ProductDetails.aspx**ページ。 追加、 **Async**属性、**ページ**要素に設定し、 **true**です。 この属性では、ASP.NET IHttpAsyncHandler インターフェイスを実装するように指示します。


    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. ページを実行しているスレッドの詳細を表示するページの下部にラベルを追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. 開いている**ProductDetails.aspx.cs**し、次の名前空間ディレクティブを追加します。

    (コード スニペットの*Web フォーム ラボ - Ex03 - 名前空間 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. 変更、 **UpdateProductImage**非同期のタスクでイメージをダウンロードする方法です。 置換は、 **WebClient** **DownloadFile**メソッドを**DownloadFileTaskAsync**メソッドが含まれて、 **await**キーワード。

    (コード スニペットの*Web フォーム ラボ - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask は、新しいページを別のスレッドで実行する非同期タスクを登録します。 Task (t) を実行すると、ラムダ式を受け取ります。 **Await**キーワード、 **DownloadFileTaskAsync**メソッドでは、メソッドの残りの部分を変換した後に非同期的に呼び出されるコールバックに、 **DownloadFileTaskAsync**メソッドが完了します。 ASP.NET は、HTTP 要求元のすべての値を自動的に管理して、メソッドの実行が再開されます。 .NET 4.5 で、新しい非同期プログラミング モデルを使用すると、似て同期コードは、非同期コードを記述して、コンパイラがコールバック関数や継続コードの複雑さの一部を処理できます。
    > [!NOTE]
    > RegisterAsyncTask と PageAsyncTask いた使用可能な .NET 2.0 以降。 Await キーワードは、.NET 4.5 の非同期プログラミング モデルから新しい .NET WebClient オブジェクトから新しい TaskAsync メソッドと一緒に使用することができます。
5. コードが開始され、実行が完了スレッドを表示するコードを追加します。 これを行うには、更新、 **UpdateProductImage**メソッドを次のコード。

    (コード スニペットの*Web フォーム ラボ - Ex03 - 表示スレッド*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. 開く web サイトの**Web.config**ファイル。 次の appSetting 変数を追加します。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. キーを押して**f5 キーを押して**にアプリケーションを実行し、製品の画像をアップロードします。 ここで開始および終了コードが異なる場合があります、スレッド ID に注意してください。 これは、ASP.NET スレッドのプールから別のスレッドで非同期タスクが実行されるためです。 タスクが完了したら、ASP.NET は、キューに戻し、タスクを配置し、使用可能なスレッドのいずれかを割り当てます。

    ![イメージを非同期的にダウンロード](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "イメージを非同期的にダウンロードします。")

    *イメージを非同期的にダウンロードします。*

> [!NOTE]
> Azure の次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。


* * *

<a id="Summary"></a>
## <a name="summary"></a>概要

このハンズオン ラボでは、次の概念が対処して示されています。

- 厳密に型指定されたデータ バインディング式を使用します。
- Web フォームの新しいモデル バインド機能を使用してください。
- ページのデータを分離コードのメソッドにマップするための値プロバイダーを使用します。
- データ注釈を使用して、ユーザー入力の検証の
- Web フォームでの jQuery を使用したクライアント側検証を unobstrusive advange になります
- 詳細な要求の検証を実装します。
- 非同期のページが Web フォームでの処理を実装します。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Azure SDK*&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express Web タイルを*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。

この付録では、Azure ポータルから新しい web サイトを作成し、Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>タスク 1 - Azure ポータルから新しい Web サイトを作成します。

1. 移動して、 [Azure Management Portal](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。

    ![Windows Azure ポータルにログオンする](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Windows Azure ポータルにログオンします。")

    *ポータルにログインします*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、ポータルの外部から Azure に完了した web アプリケーションを展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべての有効なパブリケーション メソッドごとに Azure に web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Azure に web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から Azure に web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**. 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

    - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。
    - **ユーザー名**サーバー管理者のログイン名を入力します。
    - **パスワード**サーバー管理者のログイン パスワードを入力します。
    - 新しいデータベース名を入力します。

    ![対象の接続文字列を構成する](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "対象の接続文字列を構成します。")

    *対象の接続文字列を構成します。*
6. 次に、 **[OK]**をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Azure の SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]**をクリックします。

    ![SQL データベースを指す接続文字列](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。 コード スニペットを挿入する場所を右クリックします。

1. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
2. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*
