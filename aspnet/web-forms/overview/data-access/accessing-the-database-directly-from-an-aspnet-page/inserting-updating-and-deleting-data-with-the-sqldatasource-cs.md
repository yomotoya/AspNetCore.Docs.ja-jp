---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: 挿入、更新、および (c#)、SqlDataSource によるデータの削除 |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルでは、ObjectDataSource コントロールを挿入、更新、およびデータの削除の許可する方法について説明しました。 SqlDataSource コントロールでは、t がサポートしています.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c52fcf746d80899d7ea568c8110c4dfa610224c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835358"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>挿入、更新、および (c#)、SqlDataSource によるデータの削除
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe)または[PDF のダウンロード](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> 前のチュートリアルでは、ObjectDataSource コントロールを挿入、更新、およびデータの削除の許可する方法について説明しました。 SqlDataSource コントロールが、同じ操作をサポートしていますが、アプローチは、さまざまなと、このチュートリアルは、挿入、更新、およびデータを削除する SqlDataSource を構成する方法を示します。


## <a name="introduction"></a>はじめに

説明したよう[、概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)と共に、DetailsView コントロールと FormView コントロールは、挿入、削除の機能をサポート、GridView コントロールは、組み込みの更新を提供します。編集、および機能を削除しています。 これらのデータ変更機能を記述する必要があるコードを記述しなくてもデータ ソース コントロールに直接接続することができます。 [概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)を挿入、更新、および GridView、DetailsView、FormView コントロールを使用した削除を容易にする、ObjectDataSource を使用して確認します。 または、SqlDataSource、ObjectDataSource の代わりに使用できます。

挿入、更新、および削除、ObjectDataSource 必要な挿入を実行するために呼び出すオブジェクト層のメソッドを指定する更新、または削除操作をサポートすることを思い出してください。 提供する、SqlDataSource による`INSERT`、 `UPDATE`、および`DELETE`SQL ステートメント (またはストアド プロシージャ) を実行します。 このチュートリアルでは表示されるよう、これらのステートメントは手動で作成することができますか、SqlDataSource s のデータ ソース構成ウィザードによって自動的に生成できます。

> [!NOTE]
> 以降既に説明した、挿入、編集、および GridView、DetailsView の機能を削除してコントロールと FormView コントロール、このチュートリアルはこれらの操作をサポートするために、SqlDataSource コントロールを構成するのに注意してください。 以降で、編集、挿入、および削除するデータのチュートリアルを GridView、DetailsView、FormView、戻り値内でこれらの機能を実装する方法を復習する場合は、 [、概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)します。


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>手順 1: 指定`INSERT`、`UPDATE`、および`DELETE`ステートメント

私たちは、する必要があります、SqlDataSource コントロールからデータを取得する過去の 2 つのチュートリアルで確認したところは、2 つのプロパティを設定します。

1. `ConnectionString`、、クエリを送信するデータベースの内容を指定して
2. `SelectCommand`、アドホック SQL ステートメントまたは実行結果を返すストアド プロシージャの名前を指定します。

`SelectCommand` SqlDataSource s を使用して値を指定するパラメーター、パラメーターを持つ値`SelectParameters`コレクション ハード コーディングされた値、共通パラメーターのソースの値を含めることができます (クエリ文字列フィールド、セッション変数、Web コントロールの値となどの)、またはプログラムによって割り当てられることができます。 SqlDataSource コントロール s`Select()`メソッドが呼び出されたプログラムまたは自動的にデータ Web コントロールからデータベースへの接続を確立するには、パラメーターの値は、クエリに割り当てられているおよびにコマンドが渡さオフ、データベース。 結果がデータセットか、DataReader のいずれかとして s コントロールの値に応じて、返される`DataSourceMode`プロパティ。

データを選択すると、と共に、SqlDataSource コントロールを使用して挿入、更新、および指定することによってデータを削除する`INSERT`、 `UPDATE`、および`DELETE`ほぼ同じ方法で SQL ステートメント。 割り当てるだけで、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ、 `INSERT`、`UPDATE`と`DELETE`SQL ステートメントを実行します。 (が常に最も) と、ステートメントがパラメーターを持つかどうかに含める、 `InsertParameters`、 `UpdateParameters`、および`DeleteParameters`コレクション。

1 回、 `InsertCommand`、 `UpdateCommand`、または`DeleteCommand`値が指定されている、対応するデータ Web コントロールのスマート タグの挿入を有効にする、編集の有効化または削除を有効にするオプションは使用可能になります。 これを示すためには、let s がから例を見て、 `Querying.aspx`  ページで作成した、 [SqlDataSource コントロールでデータのクエリ](querying-data-with-the-sqldatasource-control-cs.md)チュートリアルと強化機能の削除が含まれるようにします。

開いて開始、`InsertUpdateDelete.aspx`と`Querying.aspx`ページから、`SqlDataSource`フォルダー。 上のデザイナーから、 `Querying.aspx`  ページで、最初の例から、SqlDataSource や GridView を選択します (、`ProductsDataSource`と`GridView1`コントロール)。 2 つのコントロールを選択すると、[編集] メニューに移動およびコピーを選択 (または ctrl キーを押しながら C キーを押すだけ)。 デザイナーに移動して次に、`InsertUpdateDelete.aspx`コントロールに貼り付けます。 に 2 つのコントロールを移動したら`InsertUpdateDelete.aspx`ブラウザーでページをテストします。 値を表示する必要があります、 `ProductID`、 `ProductName`、および`UnitPrice`内のレコードのすべての列、`Products`データベース テーブル。


[![すべての製品が一覧表示されます ProductID によって順序付け](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**図 1**: すべての製品が一覧表示されます並べ`ProductID`([フルサイズの画像を表示する をクリックします](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))。


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>SqlDataSource s を追加する`DeleteCommand`と`DeleteParameters`プロパティ

この時点であるすべてからレコードを返すだけ SqlDataSource、`Products`テーブルと、このデータを表示する GridView。 目標は、GridView を使用して製品を削除するユーザーに対して許可するには、この例を拡張することです。 SqlDataSource コントロール秒の値を指定する必要があります。 これを実現する`DeleteCommand`と`DeleteParameters`プロパティし、削除をサポートするために、GridView を構成します。

`DeleteCommand`と`DeleteParameters`プロパティは、さまざまな方法で指定することができます。

- 宣言型構文を使用
- デザイナーの [プロパティ] ウィンドウから
- カスタム SQL ステートメントまたはストアド プロシージャがデータ ソースの構成ウィザードでの画面に指定
- ビューの画面で、データ ソースの構成ウィザードでのテーブルから列を指定の詳細設定 ボタンを使用してこれが実際に自動的に生成、`DELETE`で使用される SQL ステートメントやパラメーターのコレクション、`DeleteCommand`と`DeleteParameters`プロパティ

自動的にする方法について説明します、`DELETE`手順 2. で作成したステートメント。 ここでは、データ ソース構成ウィザードまたは宣言型構文オプションは機能と同様ですが、デザイナーの [プロパティ] ウィンドウを使用して、s を使用できます。

デザイナーから`InsertUpdateDelete.aspx`、 をクリックして、 `ProductsDataSource` SqlDataSource プロパティ ウィンドウを表示し (表示 メニューから、プロパティ ウィンドウを選択または f4 キーを押すだけで)。 省略記号のセットが表示されます、DeleteQuery プロパティを選択します。


![[プロパティ] ウィンドウから DeleteQuery プロパティを選択します。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**図 2**: DeleteQuery プロパティ [プロパティ] ウィンドウから選択します


> [!NOTE]
> SqlDataSource されないでは、DeleteQuery プロパティがあります。 DeleteQuery の組み合わせは、代わりに、`DeleteCommand`と`DeleteParameters`プロパティと、デザイナーでウィンドウを表示するときに、[プロパティ] ウィンドウでのみ表示されます。 ソース ビューのプロパティ ウィンドウを見る場合、`DeleteCommand`プロパティ代わりにします。


コマンドおよびパラメーターのエディター ダイアログ ボックスを表示、DeleteQuery プロパティで省略記号ボックス (図 3 参照) をクリックします。 このダイアログ ボックスから指定できます、 `DELETE` SQL ステートメントのパラメーターを指定します。 次のクエリを入力、`DELETE`コマンド テキスト ボックス (か、手動でまたはしたい場合に、クエリ ビルダーを使用して)。

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

次に、追加するパラメーターの更新ボタンをクリックして、`@ProductID`パラメーター以下のパラメーターの一覧にします。


[![[プロパティ] ウィンドウから DeleteQuery プロパティを選択します。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**図 3**: DeleteQuery プロパティ [プロパティ] ウィンドウから選択します ([フルサイズの画像を表示する をクリックします](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))。


*いない*(そのパラメーターのソース: None のままに) このパラメーターの値を指定します。 GridView に削除のサポートを追加すると、GridView は自動的にこのパラメーター値を入力の値を使用してその`DataKeys`の Delete ボタンがクリックされた行のコレクション。

> [!NOTE]
> 使用されるパラメーター名、`DELETE`クエリ*する必要があります*の名前と同じである、 `DataKeyNames` GridView、DetailsView、またはフォーム ビュー内の値。 内のパラメーターは、`DELETE`ステートメントの名前は意図的`@ProductID`(の代わりに、たとえば、 `@ID`)、Products テーブル (とそのため、gridview DataKeyNames 値) の主キー列名があるため、`ProductID`します。


場合、パラメーター名と`DataKeyNames`値は t の一致、GridView ことはできませんを自動的に割り当てるパラメーターの値、`DataKeys`コレクション。

コマンドおよびパラメーターのエディター ダイアログ ボックスに、削除に関連する情報を入力した後に [ok] をクリックし、結果として得られる宣言型マークアップを確認する、ソース ビューに移動します。

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

メモの追加、`DeleteCommand`プロパティだけでなく`<DeleteParameters>`セクションとという名前のパラメーター オブジェクト`productID`します。

## <a name="configuring-the-gridview-for-deleting"></a>GridView を削除するための構成

`DeleteCommand`プロパティの追加、GridView s のスマート タグには、削除を有効にするオプションが含まれています。 このチェック ボックスを確認してください。 説明したよう[、概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)、これが原因で、GridView で [commandfield] を追加するその`ShowDeleteButton`プロパティに設定`true`します。 ページがブラウザーからアクセスしたときに、4 に示すを図には、[削除] ボタンが含まれます。 このページをテストするには、一部の製品を削除します。


[![GridView の行ごとに Delete ボタンが含まれています](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**図 4**: GridView 行ごとに、[削除] ボタンが含まれています ([フルサイズの画像を表示する をクリックします](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))。


[削除] ボタンをクリックすると、ポストバックが発生する、GridView を割り当てます、`ProductID`パラメーター値の`DataKeys`が [削除] ボタンがクリックしてされ、SqlDataSource s を呼び出す行のコレクション値`Delete()`メソッド。 SqlDataSource コントロール: データベースに接続し、実行、`DELETE`ステートメント。 GridView が SqlDataSource、戻す (を不要になっただけ削除されたレコードを含む) の製品の現在のセットを表示する、再バインドします。

> [!NOTE]
> GridView を使用しているため、 `DataKeys` 、SqlDataSource パラメーターを設定するコレクションが s の重要なを GridView s`DataKeyNames`プロパティと主キーを構成する列に設定する SqlDataSource の`SelectCommand`を返しますこれらの列。 さらに、その SqlDataSource s でパラメーターの名前、重要な`DeleteCommand`に設定されている`@ProductID`します。 場合、`DataKeyNames`プロパティが設定されていないか、パラメーターの名前が付いていません`@ProductsID`ポストバックを発生させるが、[削除] ボタンをクリックすると、受注の t が実際には任意のレコードを削除します。


図 5 は、この相互作用をグラフィカルに示しています。 参照、 [、イベントに関連付けられている挿入、更新、および削除の確認](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)挿入、更新、およびデータ Web コントロールからの削除に関連付けられているイベントのチェーンについては詳細なチュートリアルです。


![SqlDataSource の Delete() メソッドを呼び出し、GridView で [削除] ボタンをクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**図 5**: SqlDataSource s を呼び出す、GridView で [削除] ボタンをクリックすると`Delete()`メソッド


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>手順 2: が自動的に生成する、`INSERT`、`UPDATE`、および`DELETE`ステートメント

検査すると、ステップ 1 として`INSERT`、 `UPDATE`、および`DELETE`プロパティ ウィンドウまたはコントロールの宣言型構文によって SQL ステートメントを指定できます。 ただし、このアプローチでは、こと手動で SQL ステートメントを手動で記述単調なりエラーが発生しやすい可能性がありますが必要です。 幸いにも、データ ソース構成ウィザードはさせるオプションを提供します。、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントの表示画面のテーブルから列を指定を使用する場合に自動的に生成されます。

この自動生成オプションを探索して使用できます。 デザイナーに、DetailsView を追加`InsertUpdateDelete.aspx`設定とその`ID`プロパティを`ManageProducts`します。 次に、DetailsView s のスマート タグから、SqlDataSource という名前を作成し、新しいデータ ソースを作成する選択`ManageProductsDataSource`します。


[![ManageProductsDataSource という名前の新しい SqlDataSource を作成します。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**図 6**: 名前付き新しい SqlDataSource 作成`ManageProductsDataSource`([フルサイズの画像を表示する をクリックします](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))。


使用することを選択、データ ソースの構成ウィザードから、`NORTHWINDConnectionString`接続文字列を [次へ] をクリックします。 Select ステートメントの画面の構成 から選択したテーブルまたはビューのオプション ボタンから列を指定のままにし、選択、`Products`ドロップダウン リストからテーブル。 選択、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`チェック ボックスの一覧から列。


[![ProductID、ProductName、UnitPrice、および提供が中止された列を返す、Products テーブルを使用して、](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**図 7**: を使用して、`Products`テーブルを返す、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`列 ([フルサイズの画像を表示する をクリックします](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))。


自動的に生成する`INSERT`、`UPDATE`と`DELETE`選択したテーブルと列に基づいて、ステートメントは高度なボタンをクリックし、確認の生成`INSERT`、`UPDATE`と`DELETE`ステートメントのチェック ボックスをオンします。


![チェック ボックスをオンの生成を挿入、更新、および DELETE ステートメントを確認してください。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**図 8**: 確認の生成`INSERT`、 `UPDATE`、および`DELETE`ステートメント チェック ボックス


生成`INSERT`、 `UPDATE`、および`DELETE`選択されているテーブルが主キーと主キー列 (または列) が返される列の一覧に含まれている場合に、ステートメントのチェック ボックスをチェック可能なだけになります。 選択可能なオプティミスティック同時実行制御チェック ボックスを使用生成 1 回`INSERT`、 `UPDATE`、および`DELETE`拡張は、ステートメントのチェック ボックスをオンされましたが、 `WHERE` 、最終的な句`UPDATE`と`DELETE`オプティミスティック同時実行制御を提供するステートメント。 ここでは、オフのままにこのチェック ボックスをオンします。次のチュートリアルでは、SqlDataSource コントロールでオプティミスティック同時実行をについて説明します。

確認の生成後`INSERT`、 `UPDATE`、および`DELETE`ステートメントのチェック ボックスをオン、Select ステートメントの構成画面に戻り、次に、をクリックして ok をクリックし、データ ソースの構成ウィザードを完了します。 ウィザードを完了すると、Visual Studio は、の DetailsView に BoundFields を追加は、 `ProductID`、 `ProductName`、および`UnitPrice`列との CheckBoxField、`Discontinued`列。 DetailsView s のスマート タグからページングを有効にするオプションを確認するため、このページにアクセスするユーザーは、製品をたどることができます。 DetailsView s もクリア`Width`と`Height`プロパティ。

スマート タグが挿入を有効にする、編集の有効化、および削除を有効にするオプションを持つことに注意してください。 これは、SqlDataSource の値を格納するため、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`のように、次の宣言型構文。

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

SqlDataSource コントロールが自動的に設定された値があった方法に注意してください。 その`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ。 参照される列のセット、`InsertCommand`と`UpdateCommand`プロパティがあるプログラムに基づきます、`SELECT`ステートメント。 ではなく*すべて*製品列で、`InsertCommand`と`UpdateCommand`で指定された列のみがある、 `SelectCommand` (小さい`ProductID`、ため、これが省略されて、s、 [`IDENTITY`列](http://www.sqlteam.com/item.asp?ItemID=102)を挿入するときに自動的に割り当て、編集する際に、値を変更ことはできません)。 さらの各パラメーターに、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティに対応するパラメーターがある、 `InsertParameters`、 `UpdateParameters`、および`DeleteParameters`コレクション。

DetailsView のデータの変更機能を有効にする、挿入を有効にする、有効にする編集、およびそのスマート タグの削除を有効にするオプションを確認してください。 [Commandfield] が追加されます、 `ShowInsertButton`、 `ShowEditButton`、および`ShowDeleteButton`プロパティに設定`true`します。

ブラウザーでページにアクセスし、編集、削除、および DetailsView に含まれる新しいボタンに注意してください。 各 BoundField を表示する編集モードにある DetailsView をオンに編集ボタンをクリックを`ReadOnly`プロパティに設定されて`false`(既定)、テキスト ボックスとチェック ボックスとして CheckBoxField として。


[![DetailsView s 既定の編集インターフェイス](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**図 9**:、DetailsView s 編集インターフェイスの既定の ([フルサイズの画像を表示する をクリックします](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))。


同様に、現在選択されている製品を削除したり、システムに新しい製品を追加できます。 以降、`InsertCommand`ステートメントでのみ動作、 `ProductName`、 `UnitPrice`、および`Discontinued`列、他の列がいずれか`NULL`または挿入時にデータベースによって割り当てられた既定値。 場合、ObjectDataSource と同様、`InsertCommand`ない列で使用できる任意のデータベース テーブルがありません`NULL`s としない既定の値を持つ SQL エラーが発生を実行する際、`INSERT`ステートメント。

> [!NOTE]
> DetailsView s を挿入して、インターフェイスの編集には、あらゆる種類のカスタマイズや検証が不足しています。 検証コントロールを追加する、またはインターフェイスをカスタマイズするには、TemplateFields、BoundFields に変換する必要があります。 参照してください、[編集および挿入インターフェイスに検証コントロールを追加](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)と[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)詳細についてはチュートリアル。


また、留意更新および削除、DetailsView を使用する、現在の製品 s`DataKey`場合にのみ存在する値、`DataKeyNames`プロパティが構成されています。 場合は、編集または削除は、影響を与えないが表示されたら、確認、`DataKeyNames`プロパティを設定します。

## <a name="limitations-of-automatically-generating-sql-statements"></a>SQL ステートメントを自動的に生成の制限事項

生成後`INSERT`、 `UPDATE`、および`DELETE`ステートメント オプションは、独自に作成する必要がありますより複雑なクエリのテーブルから列を選択するときにのみ使用可能な`INSERT`、 `UPDATE`、および`DELETE`手順 1 で行ったようにステートメントです。 一般的には、SQL`SELECT`ステートメントを使用して、`JOIN`の 1 つまたは複数のルックアップ テーブルからデータを表示目的で元に戻しますに (元に戻す方法など、`Categories`テーブルの`CategoryName`製品情報を表示するときにフィールド)。 同時に、編集、更新、または、中核となるテーブルにデータを挿入するユーザーをできるようにする場合があります (`Products`、この場合)。

中に、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントは、手動で入力することができます、次の時間節約のヒントを検討してください。 最初に、SqlDataSource をセットアップ データだけを取得戻すように、`Products`テーブル。 テーブルまたはビューの画面からデータ ソースの構成ウィザードの列と s を指定を使用して、自動的に生成することができるように、 `INSERT`、 `UPDATE`、および`DELETE`ステートメント。 ウィザードの完了後に選択のプロパティ ウィンドウから SelectQuery を構成する (または、代わりに、使用するカスタム SQL ステートメントを指定しますが、データ ソース構成ウィザードまたはストアド プロシージャのオプションに戻る)。 更新し、`SELECT`ステートメントに含める、`JOIN`構文。 この手法が自動的に生成された SQL ステートメントの時間節約の利点し、さらにカスタマイズできます`SELECT`ステートメント。

自動的に生成する別の制限、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントは、列、`INSERT`と`UPDATE`ステートメントはによって返される列に基づいて、`SELECT`ステートメント。 私たちは、更新またはただし、またはより少ないフィールドを挿入する必要があります。 たとえば、手順 2 の例ですることができますが、 `UnitPrice` BoundField が読み取り専用にします。 その場合は、これで表示されるべきで t、`UpdateCommand`します。 または、GridView に表示されていない、テーブルのフィールドの値を設定することがあります。 たとえば、新しい追加するときにレコード可能性がある、`QuantityPerUnit`値 TODO に設定します。

このようなカスタマイズが必要な場合は、手動でできるように、[プロパティ] ウィンドウ、カスタム SQL ステートメントを指定してくださいまたはウィザードで、または宣言の構文を使用してストアド プロシージャのオプションを使用する必要があります。

> [!NOTE]
> データに対応するフィールドがないパラメーターの追加は、コントロールを Web、ときに、これらのパラメーターの値がなんらかの方法で値を割り当てる必要があることに留意してください。 これらの値を指定できます。 ハード コーディングされたで直接、`InsertCommand`または`UpdateCommand`(クエリ文字列、セッション状態、ページで、上の Web コントロール) は、いくつかの事前定義されたソースから取得できます。 または、前のチュートリアルで説明したようにプログラムで、割り当てることができます。


## <a name="summary"></a>まとめ

Web コントロールの組み込みの挿入、編集、および削除機能を利用するデータの順序でにバインドされているデータ ソース コントロールは、このような機能を提供する必要があります。 SqlDataSource、つまり`INSERT`、 `UPDATE`、および`DELETE`SQL ステートメントを割り当てる必要があります、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ。 これらのプロパティと、対応するパラメーターのコレクションを手動で追加またはデータ ソース構成ウィザードを使用して自動的に生成します。 このチュートリアルでは、両方の手法を調査します。

調べるで ObjectDataSource でオプティミスティック同時実行制御を使用して、[オプティミスティック同時実行を実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)チュートリアル。 SqlDataSource コントロールには、オプティミスティック同時実行制御のサポートも提供します。 自動的に生成するときに、手順 2. で説明したように、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントでは、ウィザードを使用してオプティミスティック同時実行制御オプションを使用しています。 次のチュートリアルで後ほど、SqlDataSource でオプティミスティック同時実行制御を使用して変更、`WHERE`内の句、`UPDATE`と`DELETE`の他の列の値にデータが最後に変更されていないことを確認するステートメントページに表示されます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [次へ](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
