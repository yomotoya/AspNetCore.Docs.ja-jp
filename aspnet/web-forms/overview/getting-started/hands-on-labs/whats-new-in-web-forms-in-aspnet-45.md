---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: ASP.NET 4.5 の新機能については Web フォームの |Microsoft Docs
author: rick-anderson
description: ASP.NET Web フォームの新しいバージョンでは、さまざまなデータを扱うときにユーザー エクスペリエンスの向上に重点を置いた機能強化について説明します。 以前のバージョンにしています.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830413"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Asp.net 4.5 Web フォームの新機能新機能
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> ASP.NET Web フォームの新しいバージョンでは、さまざまなデータを扱うときにユーザー エクスペリエンスの向上に重点を置いた機能強化について説明します。
> 
> オブジェクトのメンバーの値を出力するデータ バインドを使用する場合は、Web フォームの以前のバージョンでは、Bind() または"eval()"のデータ バインディング式を使用します。 新しいバージョンの ASP.NET、データの種類のコントロールに新しい ItemType プロパティを使用してバインドすることを宣言することができます。 このプロパティを設定すると、その Visual Studio 開発環境の IntelliSense、ナビゲーションのメンバー、およびコンパイル時チェックなどの特典をフルに厳密に型指定された変数を使用することが有効になります。
> 
> データ バインド コントロールを使用ようになりましたも指定できます、独自のカスタム メソッドを選択すると、更新、削除、および、データの挿入のページ コントロールと、アプリケーションのロジック間の対話を簡略化します。 さらに、ASP.NET で、メソッド型パラメーターに直接、ページのデータをマップすることを意味するモデル バインド機能が追加されました。
> 
> ユーザー入力の検証は、最新の Web フォームで簡単にあります。 モデル クラスからの検証属性で注釈を表示できます、 **System.ComponentModel.DataAnnotations**名前空間と、すべてのサイトを制御する要求は、その情報を使用してユーザー入力を検証します。 Web フォームでのクライアント側検証は、jQuery では、クライアント側コード クリーナーと控えめな JavaScript 機能を提供する統合ようになりました。
> 
> 要求検証領域で、選択的に要求の検証、アプリケーションの特定の部分をオフにするか、無効化された要求データを読み取りやすく改良が加えられてがいます。
> 
> 一部の機能強化が HTML5 の新機能を活用するためにサーバー コントロールを Web フォームに加えられました。
> 
> - TextBox コントロールの TextMode プロパティは、電子メール、datetime、およびなどのような新しい HTML5 入力タイプをサポートするために更新されました。
> - FileUpload コントロールでは、この HTML5 機能をサポートするブラウザーからの複数のファイル アップロードできるようになりました。
> - 検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。
> - これで、URL を表す属性を持つ新しい HTML5 の要素をサポートして runat =&quot;server&quot;します。 その結果、規則を使用できます ASP.NET URL のパスのような ~ を表すアプリケーション ルート演算子 (たとえば、&lt;ビデオ runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/ビデオ&gt;)。
> - UpdatePanel コントロールは、入力フィールドの転記 HTML5 をサポートするために修正されています。
> 
> 公式の ASP.NET ポータルでは、ASP.NET WebForms 4.5 での新機能の例についてを確認できます: [ASP.NET 4.5 と Visual Studio 2012 では新機能](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。


<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- 厳密に型指定されたデータ バインディング式を使用します。
- 新しいモデル バインド機能を使用して、Web フォーム
- コード分離メソッドにページ データをマッピングするための値プロバイダーを使用します。
- データ注釈を使用して、ユーザー入力の検証
- Web フォームでの jquery クライアント側検証を unobstrusive advange がかかる
- 詳細な要求の検証を実装します。
- 非同期ページの Web フォームでの処理の実装します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。 実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;します。

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [手順 1: モデルの ASP.NET Web フォームでのバインド](#Exercise1)
2. [手順 2: データの検証](#Exercise2)
3. [手順 3: フォームの非同期ページの ASP.NET web 処理](#Exercise3)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **60 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>手順 1: モデルの ASP.NET Web フォームでのバインド

ASP.NET Web フォームの新しいバージョンには、多数のデータを使用する場合、エクスペリエンスの向上に重点を置いた機能が導入されています。 この演習では、厳密に型指定されたデータ コントロールについて説明し、モデル バインドします。

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>タスク 1 - 厳密に型指定のデータ バインディングの使用

このタスクでは、新しい厳密に型指定されたバインド ASP.NET 4.5 で使用可能なを検出します。

1. 開く、**開始**ソリューションがある**ソース/Ex1-ModelBinding/開始/** フォルダー。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、 **Customers.aspx**ページ。 メインのコントロールに番号なしのリストを配置し、各顧客の一覧を表示するための内部 repeater コントロールが含まれます。 リピータの名前に設定**customersRepeater**次のコードに示すようにします。

    以前のバージョンの Web フォーム、データ バインディングをしているオブジェクトのメンバーの値を出力するデータ バインドを使用する場合は式を使用するデータ バインド、Eval メソッドの呼び出しと共に渡して、メンバーの名前を文字列として。

    、実行時に、Eval をこれらの呼び出しは現在バインドされているオブジェクトに対してリフレクションを使用して、指定した名前を持つメンバーの値を読み取るし、html 形式で結果を表示します。 このアプローチでは、任意、unshaped データに対してデータをバインドする非常に簡単です。

    残念ながら、メンバー名、(定義へ移動) などのナビゲーションやコンパイル時チェックのサポート用の IntelliSense を含む、Visual Studio での開発時の優れたエクスペリエンス機能の多くが失われます。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. 開く、 **Customers.aspx.cs**ファイル。
4. 次の追加ステートメントを使用します。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. **ページ\_ロード**メソッドでは、顧客のリストと repeater を設定するコードを追加します。

    (コード スニペット -*フォーム ラボ - Ex01 - バインドのお客様のデータ ソースを Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    ソリューションでは、CodeFirst と共に EntityFramework を使用して作成し、データベースへのアクセスします。 次のコードで、customersRepeater は、データベースからすべての顧客を返す具体化されたクエリにバインドされます。
6. キーを押して**F5**にソリューションを実行しに移動、**顧客**アクションで repeater を表示するページ。 CodeFirst、ソリューションを使用するいると、データベースを作成し、アプリケーションを実行するときに、ローカル SQL Express インスタンスに設定されます。

    ![Repeater で顧客を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "repeater で顧客を一覧表示します。")

    *Repeater で顧客を一覧表示します。*

    > [!NOTE]
    > Visual Studio 2012、IIS Express は、既定の Web 開発サーバーです。
7. ブラウザーを閉じて、Visual Studio に戻ります。
8. 厳密に型指定されたバインドを使用する実装を置き換えるようになりました。 開く、 **Customers.aspx**ページし、新しい**ItemType**属性を設定する中継ぎ局で、**顧客**バインディングの種類の型。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    ItemType プロパティでは、データの種類、コントロールにバインドすると厳密に型指定、データ バインド コントロール内でバインドを使用することができますを宣言することができます。
9. ItemTemplate のコンテンツを次のコードに置き換えます。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    上記のアプローチの 1 つの欠点は、"eval()"を Bind() 呼び出しでは遅延バインディングのプロパティ名を表す文字列を渡すことを意味します。 これは、Intellisense は、メンバー名、コード ナビゲーション (定義へ移動) などのサポートもコンパイル時チェックのサポートを得られないことを意味します。

    ItemType プロパティを設定すると、データ バインディング式のスコープに、生成する 2 つの新しい型指定された変数:**項目**と**BindItem**します。 データ バインディング式でこれらの厳密に型指定された変数を使用し、Visual Studio の開発エクスペリエンスの完全なメリットを享受できます。

    &quot; **:** &quot;式で使用が自動的に HTML エンコード (クロスサイト スクリプティング攻撃など) のセキュリティの問題を回避するために出力します。 この表記法では、使用可能な .NET 4 以降の書き込み、応答しますが、ようになりましたはデータ バインド式で使用できるも。

    > [!NOTE]
    > 項目のメンバーでは、一方向のバインドは機能します。 双方向のバインドを実行する場合、 **BindItem**メンバー。

    ![厳密に型指定されたバインディングでの IntelliSense サポート](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "厳密に型指定されたバインディングでの IntelliSense のサポート")

    *厳密に型指定されたバインディングでの IntelliSense のサポート*
10. キーを押して**F5**にソリューションを実行し、期待どおりに変更を確認する [顧客] ページに移動します。

    ![顧客の詳細を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "顧客の詳細を一覧表示します。")

    *顧客の詳細を一覧表示します。*
11. ブラウザーを閉じて、Visual Studio に戻ります。

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>タスク 2 - 概要のモデルを Web フォームでのバインド

ASP.NET Web フォームの以前のバージョンで取得して、データの更新の両方に双方向データ バインディングを実行する必要な場合にするために必要なデータ ソース オブジェクトを使用しています。 これは、オブジェクト データ ソースを SQL データ ソース、LINQ のデータ ソースなど。 ただし、シナリオでは、カスタム コードを必要に応じて、データを処理するため、オブジェクトのデータ ソースを使用する必要があるし、これにより、いくつかの欠点です。 たとえば、複合型を回避するために必要なし、検証ロジックを実行するときに例外を処理する必要があります。

ASP.NET Web フォームの新しいバージョンでは、データ バインド コントロールは、モデル バインディングをサポートします。 つまり、できるし、指定することを選択、更新、挿入、分離コード ファイルまたは別のクラスからは、ロジックを呼び出すにはデータ バインド コントロールで直接メソッドを削除します。

これについては、新しい製品カテゴリを一覧表示する GridView を使用するが**SelectMethod**属性。 この属性では、GridView のデータを取得するための方法を指定することができます。

1. 開く、**ディスク上のファイル**ページし、が含まれて、 **GridView**します。 厳密に型指定されたバインディングを使用し、並べ替えとページングを有効にする次のように、GridView を構成します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. 新たに使用**SelectMethod**を呼び出す GridView を構成する属性、 **GetCategories**メソッド データを選択します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. オープン、 **Products.aspx.cs**分離コード ファイルを開き、次を追加するステートメントを使用します。

    (コード スニペット - *Web フォームのラボ - Ex01 - 名前空間*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. プライベート メンバーを追加、**製品**クラスし、の新しいインスタンスを割り当てる**ProductsContext**します。 このプロパティでは、データベースに接続することができます、Entity Framework データ コンテキストを格納します。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. 作成、 **GetCategories** LINQ を使用してカテゴリの一覧を取得します。 クエリが含まれます、**製品**プロパティ GridView は各カテゴリの製品の量を表示できるようにします。 メソッドが後でページのライフ サイクルを実行するクエリを表す生の IQueryable オブジェクトを返すことに注意してください。

    (コード スニペット - *Web フォームのラボ - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > ASP.NET Web フォームの以前のバージョンで有効にする並べ替えとページング、データ ソースのオブジェクト コンテキスト内で独自のリポジトリ ロジックを使用してに必要なカスタム コードを記述し、必要なすべてのパラメーターを受け取る。 ここで、データ バインディングのメソッドが IQueryable を返すことができ、これは、クエリを表します。 適切な並べ替えを追加するクエリとページングのパラメーターを変更することもを実行する ASP.NET を処理できます。
6. キーを押して**F5**サイトのデバッグを開始し、[製品] ページに移動します。 GridView には、メソッドによって返されるカテゴリが表示されていることがわかります。

    ![モデル バインドを使用して GridView を設定](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "モデル バインドを使用して GridView を設定")

    *モデル バインドを使用して GridView を設定*
7. キーを押して**SHIFT**+**F5**デバッグを停止します。

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>タスク 3 - モデル バインドの値プロバイダー

モデル バインドだけでなく、データ バインド コントロールで直接データを操作するカスタム メソッドを指定することができますが、ページからデータをこれらのメソッドからのパラメーターにマップすることもできます。 メソッドのパラメーターの値のデータ ソースを指定するのに値プロバイダー属性を使用できます。 例えば:

- ページ上のコントロール
- クエリ文字列の値
- データの表示
- セッション状態
- クッキー
- ポストされたフォーム データ
- ビュー ステート
- カスタム値プロバイダーがもサポートされています

ASP.NET MVC 4 を使用している場合、モデルのバインディング サポートは似ていますが表示されます。 実際には、これらの機能が ASP.NET MVC から取得されに移動された、 **System.Web**も Web フォームで使用できるアセンブリ。

このタスクでにはモデル バインドで filter パラメーターを受け取る、各カテゴリの製品の量によって、結果をフィルター処理する GridView 更新されます。

1. 戻り、**繰り返し**ページ。
2. GridView の上部にある追加、**ラベル**と**ComboBox**を次に示すように各カテゴリの製品の数を選択します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. 追加、**後**製品の選択した数のカテゴリがない場合にメッセージを表示する GridView にします。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. 開く、 **Products.aspx.cs**分離コードと、次を追加するステートメントを使用します。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. 変更、 **GetCategories**を整数値を受け取るメソッド**minProductsCount**引数と、返された結果をフィルター処理します。 これを行うには、次のコードでメソッドを置き換えます。

    (コード スニペット - *Web フォーム ラボ - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    新しい **[Control]** 属性を**minProductsCount**引数は ASP.NET ページにコントロールを使用してその値を設定する必要がありますを知ることができます。 ASP.NET の引数 (minProductsCount) の名前と一致する任意のコントロールを検索および必要なマッピングとコントロールの値をパラメーターに入力への変換を実行します。

    また、属性は、コントロール値を取得する場所からを指定することができます、オーバー ロードされたコンス トラクターを提供します。

    > [!NOTE]
    > データ バインド機能の 1 つの目的は、ページの相互作用を記述する必要があるコードの量を削減します。 別に、[コントロール] の値プロバイダーには、メソッドのパラメーターでその他のモデル バインド プロバイダーを使用できます。 その一部は、タスクの概要に表示されます。
6. キーを押して**F5**サイトのデバッグを開始し、[製品] ページに移動します。 ドロップダウン リストで製品の数を選択し、GridView を今すぐ更新方法に注意してください。

    ![ドロップダウン リストの値を持つ GridView をフィルタ リング](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView で、ドロップダウン リストの値をフィルター処理")

    *ドロップダウン リストの値を持つ GridView をフィルター処理*
7. デバッグを停止します。

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>タスク 4 - を使用してモデルをフィルター処理するためのバインド

このタスクでは、秒、選択したカテゴリに含まれる製品を表示する GridView の子を追加します。

1. 開く、**ディスク上のファイル**ページし、更新プログラムのカテゴリの選択ボタンを自動生成する GridView。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. 1 秒あたりの追加**GridView**という**productsGrid**下部にあります。 設定、 **ItemType**に**WebFormsLab.Model.Product**、 **DataKeyNames**に**ProductId**と**SelectMethod**に**GetProducts**します。 設定**AutoGenerateColumns**に**false** ProductId、ProductName、説明、UnitPrice の列を追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. 開く、 **Products.aspx.cs**分離コード ファイル。 実装、 **GetProducts**カテゴリ GridView からカテゴリ ID を受信して、製品をフィルター処理するメソッド。 モデル バインドで選択した行を使用してパラメーター値が設定されます、 **categoriesGrid**します。 コントロールの名前と引数の名前が一致しないために、コントロールの値プロバイダー内コントロールの名前を指定する必要があります。

    (コード スニペット - *Web フォームのラボ - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > このアプローチが容易に単位これらのメソッドをテストします。 Web フォームが実行されていない、単体テストのコンテキストで、[Control] 属性では、特定のアクションは実行しません。
4. 開く、**ディスク上のファイル**ページおよび GridView の製品を検索します。 選択した製品を編集するためのリンクを表示する GridView の製品を更新します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. 開く、 **ProductDetails.aspx**分離コード ページし、置換、 **SelectProduct**メソッドを次のコード。

    (コード スニペット - *Web フォーム ラボ - Ex01 - SelectProduct メソッド*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > 注意、 **[QueryString]** 属性を使用して、クエリ文字列で productId パラメーターからメソッド パラメーターを入力します。
6. キーを押して**F5**サイトのデバッグを開始し、[製品] ページに移動します。 GridView のカテゴリから任意のカテゴリを選択し、GridView の製品が更新されたことに注意してください。

    ![選択したカテゴリの製品を示す](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "選択したカテゴリからの製品を表示")

    *選択したカテゴリの製品を表示*
7. をクリックして、**ビュー** ProductDetails.aspx ページを開くの製品にリンクします。

    ページが、クエリ文字列から productId パラメーターを使用して、SelectMethod で製品を取得することに注意してください。

    ![製品の詳細を表示する](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "製品の詳細を表示します。")

    *製品の詳細を表示します。*

    > [!NOTE]
    > HTML の説明を入力する機能は、次の手順で実装されます。

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>タスク 5 - 更新操作のバインディングを使用してモデル

前のタスクでは、データを選択するには、主にモデル バインドを使用した、このタスクでは、モデル バインドの更新操作で使用する方法について説明します。

カテゴリの更新プログラムのカテゴリのユーザーに GridView が更新されます。

1. 開く、**繰り返し**ページし、更新プログラムのカテゴリを編集 ボタンを自動生成し、新しい GridView **UpdateMethod**属性を指定する、 **UpdateCategory**メソッドを選択した項目を更新します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Gridview DataKeyNames 属性を定義、モデル バインド オブジェクトを一意に識別されるメンバーであるし、update メソッドを受け取るには少なくともパラメーターであるため、します。
2. 開く、 **Products.aspx.cs**分離コード ファイルと実装、 **UpdateCategory**メソッド。 メソッドは、現在のカテゴリを読み込む、GridView から値を設定し、カテゴリを更新するカテゴリ ID を受信する必要があります。

    (コード スニペット - *Web フォームのラボ - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    新しい**tryupdatemodel に渡します**設定 ページでコントロールから値を使用して、モデル オブジェクトのページ クラスのメソッドが担当します。 この場合に編集されている現在の GridView 行から更新された値に置き換わります、**カテゴリ**オブジェクト。

    > [!NOTE]
    > 次の手順では、オブジェクトを編集するときに、ユーザーが入力したデータの検証、ModelState.IsValid の使用方法を説明します。
3. サイトを実行し、[製品] ページに移動します。 カテゴリを編集します。 新しい名前を入力し、クリックして**Update**変更を確定します。

    ![カテゴリを編集](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "カテゴリの編集")

    *カテゴリの編集*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>手順 2: データの検証

この演習では、ASP.NET 4.5 でのデータ検証の新機能について学びます。 Web フォームで新しい控え目な検証機能を確認します。 ユーザー入力の検証をアプリケーションのモデル クラスでデータ注釈を使用して、最後に、オンまたはオフを個別のコントロール ページの要求の検証を有効にする方法を学習します。

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>タスク 1 - 控え目な検証

フォーム検証コントロールを含む複雑なデータは、コードの約 60% を表すことができます ページで JavaScript コードが多すぎるを生成する傾向があります。 控え目な検証が有効になっている、HTML コードは、見た目と若干見やすくしています。

このセクションでは、両方の構成によって生成された HTML コードを比較する ASP.NET での控えめな検証を有効になります。

1. 開く**Visual Studio 2012**を開くと、**開始**ソリューション、 **Source\Ex2 Validation\Begin**このラボのフォルダー。 または、前の演習から既存のソリューションで作業を続けることができます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 ソリューション エクスプ ローラーで、をクリックして、 **WebFormsLab**プロジェクト**NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. キーを押して**F5** web アプリケーションを起動します。 ページに、顧客 をクリックし、**新しい顧客を追加**リンク。
3. ブラウザーのページを右クリックし、選択**ソースの表示**アプリケーションによって生成された HTML コードを開くにはオプションです。

    ![ページの HTML コードを示す](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "ページの HTML コードの表示")

    *ページの HTML コードの表示*
4. ページのソース コードをスクロールし、ASP.NET の JavaScript コードとデータの検証コントロールがページで、検証を実行して、エラー一覧を表示が挿入されたことに注意してください。

    ![CustomerDetails ページの JavaScript コードを検証](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails ページの JavaScript の検証コード")

    *CustomerDetails ページで JavaScript コードの検証*
5. ブラウザーを閉じて、Visual Studio に戻ります。
6. 今すぐ控え目な検証を有効になります。 開いている**Web.Config**探し**ValidationSettings:UnobtrusiveValidationMode**キー、 **AppSettings**セクション**します。** キーの値を設定**WebForms**します。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > このプロパティを設定することも、 &quot;**ページ\_ロード**&quot;控え目な検証をいくつかのページに対してのみ有効にする場合のイベント。
7. 開いている**CustomerDetails.aspx**キーを押します**F5** Web アプリケーションを起動します。
8. IE の開発者ツールを開く F12 キーを押します。 開発者ツールが開いたら、スクリプト タブを選択します。選択**CustomerDetails.aspx**ローカル サイトからブラウザーに読み込まれたページに jQuery を実行するスクリプトが必要なことに注意してください メニューおよび take から。

    ![ローカル IIS サーバーから直接ファイルを読み込み、jQuery JavaScript](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "ローカル IIS サーバーから直接ファイル jQuery JavaScript の読み込み")

    *ローカル IIS サーバーから直接、jQuery JavaScript ファイルの読み込み*
9. Visual Studio に戻るにブラウザーを閉じます。 開く、 **Site.Master**ファイルを再び探し、 **ScriptManager**します。 属性を追加**EnableCdn**プロパティ値を持つ**True**します。 これで、ローカル サイトの URL ではなく、オンラインの URL から読み込まれる jQuery です。
10. 開いている**CustomerDetails.aspx** Visual Studio でします。 サイトを実行する F5 キーを押します。 Internet Explorer が開いたら、開発者ツールを開く F12 キーを押します。 選択、**スクリプト**タブをクリックし、ドロップダウン リストを参照してください。 JQuery JavaScript ファイルが不要になった読み込まれているローカルのサイトからではなくオンライン jQuery CDN に注意してください。

    ![ファイルを CDN から jQuery JavaScript の読み込み](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "ファイル、CDN から jQuery JavaScript の読み込み")

    *CDN から jQuery JavaScript ファイルの読み込み*
11. もう一度表示ソース オプションを使用して、ブラウザーで HTML ページのソース コードを開きます。 通知を控え目な検証を有効にすると ASP.NET が挿入された JavaScript コードに置き換えデータ-\*属性。

    ![控え目な検証コード](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "控え目な検証コード")

    *控え目な検証コード*

    > [!NOTE]
    > この例で HTML および JavaScript 数行のみに簡略化された概要データ注釈検証する方法を説明しました。 以前は、控え目な検証が、追加すると、複数の検証コントロールのサイズが大きく、JavaScript の検証コードが増えます。

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>タスク 2 - データ注釈を使用したモデルの検証

ASP.NET 4.5 では、Web フォームのデータ注釈検証について説明します。 各入力の検証コントロールはなく、ここで、モデル クラスで制約を定義し、すべての web アプリケーション全体で使用できます。 このセクションでは、顧客の新規作成/編集フォームを検証するためのデータ注釈を使用する方法を学びます。

1. 開いている**CustomerDetail.aspx**ページ。 顧客名し、2 つ目の名前が、**後**と**後**のセクションでは、RequiredFieldValidator コントロールを使用して検証されます。 チェックする条件として、多くの検証コントロールを含める必要があるために、個々 の検証は、特定の条件に関連付けられています。
2. 顧客のモデル クラスを検証するデータの注釈を追加します。 開いている**Customer.cs**クラス、**モデル**フォルダーと*装飾*データ注釈属性を使用して各プロパティ。

    (コード スニペット - *Web フォーム ラボ - Ex02 - データ注釈*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 では、既存のデータ注釈のコレクションを拡張しました。 これらは、一部のデータ注釈を使用することができます: [CreditCard] [Phone] [EmailAddress]、[範囲] [比較]、[Url] [FileExtensions]、[Required]、[Key]、[正規表現]。
    > 
    > いくつかの使用例:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > 各属性内の独自のエラー メッセージを定義することもできます。
3. 開いている**CustomerDetails.aspx**と FormView コントロールの後と後のセクションで、最初と最後の名前フィールドのすべての RequiredFieldvalidators を削除します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > データ注釈を使用する利点の 1 つは、アプリケーション ページに検証ロジックが重複していないことです。 モデルでは、一度定義し、そのデータを操作するすべてのアプリケーション ページを使用します。
4. 開いている**CustomerDetails.aspx**分離コードと SaveCustomer メソッドを検索します。 このメソッドは、新しい顧客を挿入するときに呼び出されるし、FormView コントロールの値から顧客のパラメーターを受け取ります。 ページ コントロールとパラメーターのオブジェクトも間のマッピング、ASP.NET は実行時に、モデルのすべてのデータ注釈検証は属性し、存在する場合に発生したエラーのある ModelState ディクショナリを入力します。

    ModelState.IsValid は、検証を実行した後、モデルのすべてのフィールドが有効な場合だけです。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. 追加、 **ValidationSummary**モデル エラーの一覧を表示する CustomerDetails ページの末尾にあるコントロール。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors**に設定すると新しいプロパティを ValidationSummary コントロール**true**、ModelState ディクショナリからのエラーをコントロールが表示されます。 これらのエラーは、データ注釈検証から取得されます。
6. キーを押して**F5** Web アプリケーションを実行します。 エラーのあるいくつかの値をフォームに入力し、をクリックして**保存**検証を実行します。 エラーの下部にある概要を確認します。

    ![データ注釈を使用した検証](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "データ注釈を使用した検証")

    *データ注釈を使用した検証*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>タスク 3 - ModelState とカスタム データベース エラーの処理

以前のバージョンの Web フォームでは、長すぎる文字列または一意キー違反などのデータベース エラーの処理が係わりますリポジトリ コードにおける例外のスローとエラーを表示する分離コードで例外を処理します。 優れたコード量が比較的単純な処理を行う必要です。

Web フォームの 4.5 では、一貫した方法で、ページで、モデルとは、データベースから、エラーを表示する ModelState オブジェクトを使用できます。

このタスクでは、適切にデータベースの例外を処理し、ModelState オブジェクトを使用して、ユーザーに適切なメッセージを表示するためのコードを追加します。

1. アプリケーションが実行中でも、重複した値を使用してカテゴリの名前を更新してみてください。

    ![重複した名前とカテゴリを更新](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "重複した名前とカテゴリを更新しています")

    *重複した名前とカテゴリを更新しています*

    ために、例外がスローされますが、&quot;一意&quot;の制約、 **CategoryName**列。

    ![重複しているカテゴリの名前は、例外を](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "重複しているカテゴリの名前は、例外")

    *重複しているカテゴリの名前は、例外*
2. デバッグを停止します。 **Products.aspx.cs**分離コード ファイル、更新、 **UpdateCategory** db によってスローされた例外を処理するメソッド。SaveChanges() メソッドの呼び出しを追加するとエラー、 **ModelState**オブジェクト。

    新しい**tryupdatemodel に渡します**メソッドは、ユーザーによって指定されたフォーム データを使用して、データベースから取得したカテゴリ オブジェクトを更新します。

    (コード スニペット - *Web フォーム ラボ - Ex02 - UpdateCategory ハンドル エラー*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > 理想的には、DbUpdateException の原因を特定し、根本原因が一意キー制約の違反を確認する必要があります。
3. 開いている**ディスク上のファイル**を追加し、 **ValidationSummary**モデル エラーの一覧を表示する GridView のカテゴリの下のコントロール。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. サイトを実行し、[製品] ページに移動します。 重複した値を使用してカテゴリの名前を更新しようとしてください。

    例外の処理し、エラー メッセージに表示されますが、 **ValidationSummary**コントロール。

    ![カテゴリのエラーが重複して](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "カテゴリのエラーが重複しています")

    *重複しているカテゴリのエラー*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>タスク 4 - 要求の ASP.NET Web フォーム 4.5 での検証

Asp.net 要求の検証機能では、特定のレベルのクロス サイト スクリプティング (XSS) 攻撃に対する既定の保護を提供します。 以前のバージョンの ASP.NET では、要求の検証が既定で有効になっているし、ページ全体を無効にするのみできます。 ASP.NET Web フォームの新しいバージョンで今すぐ 1 つのコントロール用要求の検証を無効にする、遅延要求の検証を実行したり、(これを行う場合は慎重に!) が検証されていない要求のデータにアクセスできます。

1. キーを押して**Ctrl + F5**にデバッグなしでサイトを開始し、[製品] ページに移動します。 カテゴリを選択し、をクリックし、**編集**製品のいずれかでリンクします。
2. 危険性のあるコンテンツを含む、HTML タグを含むインスタンスの説明を入力します。 要求の検証のためにスローされる例外の通知を実行します。

    ![製品の危険性のあるコンテンツを編集](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "危険性のあるコンテンツを含む製品の編集")

    *製品の危険性のあるコンテンツの編集*

    ![要求の検証のためにスローされる例外](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "要求の検証のためにスローされる例外")

    *要求の検証のためにスローされる例外*
3. ページを閉じるし、Visual Studio で、キーを押して**SHIFT + F5**デバッグを停止します。
4. 開く、 **ProductDetails.aspx**ページし、検索、**説明**テキスト ボックス。
5. 新しい追加**ValidateRequestMode**テキスト ボックスにプロパティに値を設定し、**無効になっている**。

    新しい**ValidateRequestMode**属性は、各コントロールで細かく、要求の検証を無効にすることができます。 これは、HTML コードが表示される可能性がありますが、残りのページの検証を保持するための入力を使用する場合に便利です。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. キーを押して**F5** web アプリケーションを実行します。 製品の編集 ページを再度開くし、HTML タグを含む製品の説明を完了します。 説明に HTML コンテンツを追加できるようになりましたことに注意してください。

    ![要求の検証、製品の説明を無効になっている](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "要求の検証、製品の説明を無効になっています")

    *製品の説明を無効になっている要求の検証*

    > [!NOTE]
    > 実稼働アプリケーションでは、唯一の安全な HTML タグが入力したかどうかを確認するユーザーが入力した HTML コードをサニタイズする必要があります (たとえば、あるありません&lt;スクリプト&gt;タグ)。 これを行うには、使用することができます[Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS)します。
7. 製品をもう一度編集します。 [名前] フィールドに HTML コードを入力し、をクリックして**保存**します。 要求の検証は、説明フィールドののみ無効にし、re フィールドの残りの部分はまだ危険性のあるコンテンツに対する検証に注意してください。

    ![要求の検証の残りのフィールドで有効になっている](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "要求の検証の残りのフィールドで有効になっています。")

    *残りのフィールドで有効になっている要求の検証*

    ASP.NET Web フォーム 4.5 には、遅延要求の検証を実行する新しい要求の検証モードが含まれています。 設定する要求の検証モード**4.5**、コードへのアクセスの場合は、 *Request.Form [&quot;キー&quot;]*、ASP.NET 4.5 の要求の検証はのみトリガー要求の検証フォームのコレクションでその要素を特定します。

    さらに、ASP.NET 4.5 には、Microsoft ANTI-XSS ライブラリ v4.0 からコア エンコーディング ルーチンが含まれています。 ANTI-XSS エンコーディング ルーチンは、新しいによって実装される*AntiXssEncoder* 、新しい型が見つかった**System.Web.Security.AntiXss**名前空間。 **EncoderType**パラメーターを使用するように構成*AntiXssEncoder*、すべての出力を新しいエンコード ルーチンを使用して ASP.NET 内で自動的にエンコードします。
8. ASP.NET 4.5 の要求の検証には、データを要求されていない検証済みのアクセスもサポートしています。 ASP.NET 4.5 で追加、新しいコレクションのプロパティを**HttpRequest**と呼ばれるオブジェクト**Unvalidated**します。 移動すると**HttpRequest.Unvalidated**のすべての要求のデータ、フォーム、クエリ文字列、Cookie、Url などの一般的な情報にアクセスします。

    ![Request.Unvalidated オブジェクト](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated オブジェクト")

    *Request.Unvalidated オブジェクト*

    > [!NOTE]
    > **注意して HttpRequest.Unvalidated プロパティを使用してください。** 危険なテキストのないをラウンドト リップし、無防備な顧客に表示にする未加工の要求データを慎重にカスタム検証を実行することを確認してください。

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>手順 3: フォームの非同期ページの ASP.NET web 処理

この演習では、新しい非同期ページの ASP.NET Web フォームで機能を処理する導入する予定です。

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>タスク 1 - 製品の更新の詳細をアップロードし、イメージを表示するページ

このタスクで、製品の画像の URL を指定し、読み取り専用ビューで表示できるようにする製品の詳細ページが更新されます。 同期的にダウンロードして、指定されたイメージのローカル コピーを作成するされます。 次のタスクでは、非同期的に動作させるのには、この実装を更新します。

1. 開いている**Visual Studio 2012**を読み込むと、**開始**ソリューション**Source\Ex3 Async\Begin**このラボのフォルダーから。 または、前の演習から既存のソリューションで作業を続けることができます。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 ソリューション エクスプ ローラーで、をクリックして、 **WebFormsLab**順に選択して**NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、 **ProductDetails.aspx**ソース ページおよび製品イメージを表示するフォーム ビューの ItemTemplate でフィールドを追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. FormView の EditTemplate で画像の URL を指定するフィールドを追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. 開く、 **ProductDetails.aspx.cs**分離コード ファイルを開き、次の名前空間ディレクティブを追加します。

    (コード スニペット - *Web フォームのラボ - Ex03 - 名前空間*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. 作成、 **UpdateProductImage**メソッドをリモートのイメージをローカルに格納**イメージ**フォルダーと新しいイメージの場所の値を持つ製品エンティティを更新します。

    (コード スニペット - *Web フォームのラボ - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. 更新プログラム、 **UpdateProduct**メソッドを呼び出す、 **UpdateProductImage**メソッド。

    (コード スニペット - *Web フォーム ラボ - Ex03 - UpdateProductImage 呼び出し*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. アプリケーションを実行し、製品の画像をアップロードしようとしてください。 たとえば、Office クリップ Arts から次の画像の URL を使用できます。 [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![製品の画像を設定](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "製品の画像を設定")

    *製品の画像を設定*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>タスク 2 - 非同期処理の製品詳細ページに追加します。

このタスクで非同期的に連携できるようにする製品の詳細ページが更新されます。 実行時間の長いタスク - イメージのダウンロード プロセスでは、ASP.NET 4.5 の非同期ページ処理を使用して強化されます。

Web アプリケーションでの非同期メソッドは、ASP.NET スレッド プールの使用方法を最適化するために使用できます。 Asp.net がありますが在籍しているのためにスレッド プールのスレッド数が制限を要求するそのため、すべてのスレッドがビジー状態で ASP.NET 新しい要求を拒否、アプリケーションのエラー メッセージを送信します始まり、サイトを使用できなくなります。

Web サイトで時間のかかる操作では、長時間割り当てられているスレッドを占有するために、非同期プログラミングの有力候補が。 これには、実行時間の長い要求、さまざまな要素の多くのページとオフライン操作、そのようなデータベース クエリを実行または外部 web サーバーへのアクセスを必要とするページが含まれます。 利点は、こと、ページの処理中にこれらの操作を非同期メソッドを使用する場合、スレッドが解放され、スレッドに返されるプールし、新しいページ要求に使用できます。 この場合は、ページで、スレッド プールから 1 つのスレッドで処理を開始し、非同期処理が完了したら、別の処理を完了することがあります。

1. 開く、 **ProductDetails.aspx**ページ。 追加、 **Async**属性、**ページ**要素に設定し、 **true**します。 この属性は、IHttpAsyncHandler インターフェイスを実装するために ASP.NET に指示します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. ページを実行しているスレッドの詳細を表示するページの下部にラベルを追加します。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. 開いて**ProductDetails.aspx.cs**し、次の名前空間ディレクティブを追加します。

    (コード スニペット - *Web フォーム ラボ - Ex03 - 名前空間 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. 変更、 **UpdateProductImage**メソッドの非同期タスクでイメージをダウンロードします。 置換は、 **WebClient** **DownloadFile**メソッドを**DownloadFileTaskAsync**メソッドを含めると、 **await**キーワード。

    (コード スニペット - *Web フォーム ラボ - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    別のスレッドで実行される新しいページの非同期タスクを RegisterAsyncTask に登録します。 タスク (t) を実行すると、ラムダ式を受け取ります。 **Await**キーワード、 **DownloadFileTaskAsync**メソッド、メソッドの残りの部分は後で非同期的に呼び出されるコールバックに変換、 **DownloadFileTaskAsync**メソッドが完了しました。 ASP.NET では、HTTP 要求元のすべての値を自動的に維持することによって、メソッドの実行が再開されます。 .NET 4.5 では、新しい非同期プログラミング モデルでは、次の同期コードの場合とほぼ同じな非同期コードを記述し、コンパイラは、コールバック機能または継続コードの複雑さを処理できるようにできます。
    > [!NOTE]
    > RegisterAsyncTask と PageAsyncTask いた使用可能な .NET 2.0 以降。 Await キーワードは、.NET 4.5 の非同期プログラミング モデルから新しい .NET WebClient オブジェクトから新しい TaskAsync メソッドと共に使用できます。
5. これでコードが開始され、実行が終了したスレッドを表示するコードを追加します。 これを行うには、更新、 **UpdateProductImage**メソッドを次のコード。

    (コード スニペット - *Web フォーム ラボ - Ex03 - Show スレッド*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. 開く web サイトの**Web.config**ファイル。 次の appSetting の変数を追加します。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. キーを押して**F5**にアプリケーションを実行し、製品の画像をアップロードします。 スレッド ID、開始および終了コードが異なる場合がありますに注意してください。 ASP.NET スレッド プールから別のスレッドで非同期タスクを実行するためです。 タスクが完了したら、ASP.NET は、キューに戻し、タスクを配置し、利用可能なスレッドのいずれかを割り当てます。

    ![イメージの非同期的にダウンロード](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "非同期的にイメージのダウンロード")

    *非同期のイメージのダウンロード*

> [!NOTE]
> また、Azure の次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。


* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボで、次の概念が対処して示されています。

- 厳密に型指定されたデータ バインディング式を使用します。
- 新しいモデル バインド機能を使用して、Web フォーム
- コード分離メソッドにページ データをマッピングするための値プロバイダーを使用します。
- データ注釈を使用して、ユーザー入力の検証
- Web フォームでの jquery クライアント側検証を unobstrusive advange がかかる
- 詳細な要求の検証を実装します。
- 非同期ページの Web フォームでの処理の実装します。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express for Web のタイル*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

この付録では、Azure Portal から新しい web サイトを作成して Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>タスク 1 - Azure Portal から新しい Web サイトの作成

1. 移動して、 [Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。

    ![Windows Azure ポータルにログオン](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Windows Azure ポータルにログオン")

    *ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションを使用すると、ポータルの外部から Azure に完成した web アプリケーションをデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*有効になっているパブリケーションの各メソッドには、Azure の web アプリケーションを発行するために必要な情報がすべて含まれます。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはAzure に web アプリケーションを公開します。

    ![発行プロファイルのダウンロード web サイト](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Azure への web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 SQL Database サーバーを表示するには、サブスクリプションで Azure 管理ポータルで**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**. 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *変更を確認します。*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - 新しいデータベース名を入力します。

     ![ターゲットの接続文字列を構成する](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "データベース文字列を作成します。")

    *データベースの作成*
7. Azure SQL データベースへの接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>付録 c: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。 コード スニペットを挿入するを右クリックします。

1. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
2. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*
