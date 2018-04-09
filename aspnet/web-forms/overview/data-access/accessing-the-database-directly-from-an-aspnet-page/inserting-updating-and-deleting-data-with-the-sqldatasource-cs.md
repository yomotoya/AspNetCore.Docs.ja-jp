---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: 挿入、更新、および (c#)、SqlDataSource によるデータの削除 |Microsoft ドキュメント
author: rick-anderson
description: 前のチュートリアルでの挿入、更新、およびデータの削除、ObjectDataSource コントロールが許可されている方法がわかりました。 SqlDataSource コントロールは、t をサポートしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 25dab0292aefa183a1abc2615a7ba8e7a512346d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>挿入、更新、および (c#)、SqlDataSource によるデータの削除
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe)または[PDF のダウンロード](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> 前のチュートリアルでの挿入、更新、およびデータの削除、ObjectDataSource コントロールが許可されている方法がわかりました。 SqlDataSource コントロールをサポートしますが同じ操作が、アプローチが異なるため、このチュートリアルは、挿入、更新、およびデータを削除する SqlDataSource を構成する方法を示します。


## <a name="introduction"></a>はじめに

説明したよう[、概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)GridView コントロールは、組み込みの更新、およびと共に DetailsView およびフォームのコントロールには、挿入するときに、削除の機能をサポート編集および機能を削除します。 これらのデータ変更機能は、書き込まれる必要があるコードの行がないデータ ソース コントロールに直接接続できます。 [概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)ObjectDataSource を使用して挿入、更新、および GridView、DetailsView、フォーム ビューのコントロールを使用した削除が容易にするかを確認します。 また、SqlDataSource、ObjectDataSource の代わりに使用できます。

挿入、更新、および削除、ObjectDataSource おために必要なメソッドを指定する、オブジェクト層の挿入を実行するために呼び出す更新、または削除操作をサポートすることに注意してください。 指定しなければ、SqlDataSource による`INSERT`、 `UPDATE`、および`DELETE`SQL ステートメント (またはストアド プロシージャ) を実行します。 このチュートリアルで後ほどお見せ、これらのステートメントを手動で作成または SqlDataSource のデータ ソース構成ウィザードによって自動的に生成できます。

> [!NOTE]
> 以降既に説明した、挿入、編集、および GridView DetailsView の機能を削除して FormView コントロール、このチュートリアルをこれらの操作をサポートする SqlDataSource コントロールの構成に焦点を当てます。 編集、挿入、および削除するデータのチュートリアルへ GridView、DetailsView、および FormView、戻り値内でこれらの機能の実装を復習する必要がある場合で始まる[、概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)です。


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>手順 1: 指定`INSERT`、`UPDATE`、および`DELETE`ステートメント

おとしては、過去の 2 つのチュートリアルでは、データを取得する必要があります、SqlDataSource コントロールから見てきましたは、2 つのプロパティを設定します。

1. `ConnectionString`、、クエリを送信するデータベースの内容を指定し、
2. `SelectCommand`、アドホック SQL ステートメントまたはを実行結果を返すストアド プロシージャの名前を指定します。

`SelectCommand`パラメーターの値、値が SqlDataSource s を介して指定されたパラメーター`SelectParameters`コレクション ハード コーディングされた値、共通パラメーターのソース値を含めることができます (クエリ文字列フィールド、セッション変数、Web コントロールの値とよびな)、またはプログラムによって割り当てられたことができます。 SqlDataSource が s を制御するときに`Select()`メソッドが呼び出されたプログラムでも自動的に Web コントロールのデータから、データベースへの接続が確立される、パラメーターの値は、クエリに割り当てられたおよびにコマンドが渡さオフ、データベースです。 結果がデータセットか、DataReader のいずれかとしてコントロール秒の値に応じて、返される`DataSourceMode`プロパティです。

データを選択すると、と共に、SqlDataSource コントロールを使用して挿入、更新、および指定することによってデータを削除する`INSERT`、 `UPDATE`、および`DELETE`ほぼ同じ方法で SQL ステートメント。 割り当てるだけで、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ、 `INSERT`、 `UPDATE`、および`DELETE`SQL ステートメントを実行します。 (ようほとんど常に) ステートメントがパラメーターを持つかどうかに含める、 `InsertParameters`、 `UpdateParameters`、および`DeleteParameters`コレクション。

1 回、 `InsertCommand`、 `UpdateCommand`、または`DeleteCommand`値が指定されて、Web コントロールのスマート タグの対応するデータの挿入を有効にする、編集を有効にする、または削除を有効にするオプションを使用可能になる予定です。 Let s はこれを示すためから例を見て、 `Querying.aspx`  ページで作成した、 [SqlDataSource コントロールでのデータのクエリを実行する](querying-data-with-the-sqldatasource-control-cs.md)チュートリアルおよびを追加するための削除機能を強化します。

開いて開始、`InsertUpdateDelete.aspx`と`Querying.aspx`ページから、`SqlDataSource`フォルダーです。 上のデザイナーから、 `Querying.aspx`  ページで、最初の例から SqlDataSource GridView を選択します (、`ProductsDataSource`と`GridView1`コントロール)。 2 つのコントロールを選択すると、[編集] メニューに移動およびコピーを選択 (または Ctrl + C キーを押すだけ)。 次のデザイナーへ移動`InsertUpdateDelete.aspx`コントロールに貼り付けます。 に 2 つのコントロールを移動した後に`InsertUpdateDelete.aspx`ブラウザーでページをテストします。 値を確認する必要があります、 `ProductID`、 `ProductName`、および`UnitPrice`レコード内のすべての列、`Products`データベース テーブルです。


[![すべての製品が一覧表示されます ProductID 順](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**図 1**: すべての製品の一覧表示されますの順で`ProductID`([フルサイズのイメージを表示するをクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>SqlDataSource s を追加する`DeleteCommand`と`DeleteParameters`プロパティ

この時点であるだけで、すべてのレコードから返す SqlDataSource、`Products`テーブルとデータを表示する GridView。 目標は GridView 経由で製品を削除するユーザーに対して許可するには、この例を拡張することです。 SqlDataSource コントロール秒の値を指定する必要があります。 これを実現する`DeleteCommand`と`DeleteParameters`プロパティおよび削除をサポートする GridView を構成します。

`DeleteCommand`と`DeleteParameters`さまざまな方法でプロパティを指定することができます。

- 宣言の構文を使用
- デザイナーで [プロパティ] ウィンドウ
- カスタム SQL ステートメントまたはストアド プロシージャがデータ ソースの構成ウィザードでの画面を指定してから
- データ ソースの構成ウィザードでの表示画面のテーブルから列を指定の詳細設定 ボタンを使用してこれが実際に自動的に生成、`DELETE`で使用される SQL ステートメントやパラメーターのコレクション、`DeleteCommand`と`DeleteParameters`プロパティ

自動的に実施する方法について確認、`DELETE`手順 2. で作成されたステートメント。 ここでは、により、s、デザイナーの [プロパティ] ウィンドウを使用して、データ ソース構成ウィザードまたは宣言構文オプションは機能と同様。

デザイナーから`InsertUpdateDelete.aspx`、をクリックして、 `ProductsDataSource` SqlDataSource し、[プロパティ] ウィンドウを開き ([表示] メニューから [プロパティ] ウィンドウを選択または、f4 キーを押すだけで)。 省略記号のセットが表示されます、DeleteQuery プロパティを選択します。


![[プロパティ] ウィンドウから DeleteQuery プロパティを選択します。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**図 2**: [プロパティ] ウィンドウから DeleteQuery プロパティを選択


> [!NOTE]
> SqlDataSource されないでは、DeleteQuery プロパティがあります。 DeleteQuery の組み合わせは、代わりに、`DeleteCommand`と`DeleteParameters`プロパティと、デザイナーでウィンドウを表示するときにのみ、[プロパティ] ウィンドウに表示します。 場合は、ソース ビューの [プロパティ] ウィンドウを見ることがわかります、`DeleteCommand`プロパティ代わりにします。


コマンドおよびパラメーターのエディター ダイアログを表示する DeleteQuery プロパティ内の省略記号ボックス (図 3 を参照) をクリックします。 このダイアログ ボックスで指定することができます、 `DELETE` SQL ステートメントのパラメーターを指定します。 次のクエリを入力、 `DELETE`  ボックスにコマンド (か、手動でまたはしたい場合は、クエリ ビルダーを使用して)。

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

次に、追加パラメーターの更新ボタンをクリックして、`@ProductID`パラメーター以下のパラメーターの一覧にします。


[![[プロパティ] ウィンドウから DeleteQuery プロパティを選択します。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**図 3**: [プロパティ] ウィンドウから DeleteQuery プロパティを選択 ([フルサイズのイメージを表示するをクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


*いない*(そのパラメーターのソースなし のままにして) このパラメーターの値を指定します。 GridView に、削除のサポートを追加した GridView は自動的にこの値を入力パラメーターの値を使用してその`DataKeys`の削除 ボタンがクリックしてされた行のコレクション。

> [!NOTE]
> 使用されるパラメーター名、`DELETE`クエリ*必要があります*の名前と同じである、 `DataKeyNames` GridView、DetailsView、またはフォーム ビュー内の値。 内のパラメーターは、`DELETE`ステートメントの名前は意図的に`@ProductID`(の代わりに、たとえば、 `@ID`)、Products テーブル (およびそのため、GridView にされている DataKeyNames 値) の主キー列名があるため、`ProductID`です。


場合、パラメーター名と`DataKeyNames`値が一致していない GridView に自動的に値を代入できません、パラメーターから、`DataKeys`コレクション。

コマンドおよびパラメーターのエディター ダイアログ ボックスに、削除の関連情報を入力した後に ok をクリックし、結果として得られるの宣言型マークアップをソース ビューに移動します。

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

追加に注意してください、`DeleteCommand`プロパティだけでなく`<DeleteParameters>`セクションとという名前のパラメーター オブジェクト`productID`です。

## <a name="configuring-the-gridview-for-deleting"></a>削除する GridView の構成

`DeleteCommand`プロパティの追加、GridView s のスマート タグが削除を有効にするオプションを含むようになりました。 このチェック ボックスを確認してください。 説明したよう[、概要の挿入、更新、および削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)、これが原因で CommandField を追加する GridView の`ShowDeleteButton`プロパティに設定`true`です。 図に 4 に示すページがブラウザーからアクセスしたときに、[削除] ボタンが含まれます。 このページをテストするには、いくつかの製品を削除します。


[![GridView の行ごとに [削除] ボタンが含まれています](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**図 4**: GridView 行ごとに、[削除] ボタンが含まれています ([フルサイズのイメージを表示するをクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


削除 ボタンをクリックすると、ポストバックが発生した、GridView 割り当てます、`ProductID`パラメーター値の`DataKeys`コレクションの値の行の削除 ボタンがクリックされ、SqlDataSource s を呼び出す`Delete()`メソッドです。 SqlDataSource コントロールは、データベースに接続し、実行、`DELETE`ステートメントです。 SqlDataSource を取得して、(を不要になったばかり削除されたレコードを含む) の製品の現在のセットを表示する GridView を再バインドします。

> [!NOTE]
> GridView が使用されるため、 `DataKeys` SqlDataSource パラメーターを設定するコレクション、s の重要なを GridView s`DataKeyNames`プロパティと主キーを構成する列に設定する SqlDataSource の`SelectCommand`を返しますこれらの列です。 さらに、その SqlDataSource s でパラメーターの名前、重要な`DeleteCommand`に設定されている`@ProductID`です。 場合、`DataKeyNames`プロパティが設定されていないか、パラメーターの名前が付いていない`@ProductsID`ポストバックを [削除] ボタンをクリックすると生成されますが、受注 t が実際には任意のレコードを削除します。


図 5 では、この操作を視覚的に示しています。 戻って、 [、イベントに関連付けられている挿入、更新、および削除を確認する](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)挿入、更新、およびデータ Web コントロールからの削除に関連するイベントのチェーンについては詳細なチュートリアルです。


![GridView で [削除] ボタンをクリックするとメソッドを呼び出し、SqlDataSource の Delete()](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**図 5**: SqlDataSource の GridView で [削除] ボタンをクリックすると呼び出す`Delete()`メソッド


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>手順 2: が自動的に生成する、`INSERT`、`UPDATE`、および`DELETE`ステートメント

調べてから、手順 1. として`INSERT`、 `UPDATE`、および`DELETE`プロパティ ウィンドウまたはコントロール s 宣言の構文によって SQL ステートメントを指定できます。 ただし、この方法を手動で書き込みます SQL ステートメントを手動で単調とエラーが発生しやすいできる必要があります。 幸いにも、データ ソース構成ウィザードする機能が、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントの表示画面のテーブルから列を指定を使用すると自動的に生成します。

この自動生成オプションを探索 s を使用できます。 DetailsView にデザイナーでは、追加`InsertUpdateDelete.aspx`設定とその`ID`プロパティを`ManageProducts`です。 次に、DetailsView s のスマート タグから新しいデータ ソースを作成し、名前付き SqlDataSource を作成する選択`ManageProductsDataSource`です。


[![ManageProductsDataSource をという名前の新しい SqlDataSource を作成します。](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**図 6**: 名前付き新しい SqlDataSource 作成`ManageProductsDataSource`([フルサイズのイメージを表示するをクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


データ ソースの構成の作成ウィザードを使用してよう、`NORTHWINDConnectionString`接続文字列を [次へ] をクリックします。 Select ステートメントの画面の構成 から選択したテーブルまたはビューのオプション ボタンから列を指定のままにし、選択、`Products`ドロップダウン リストからテーブル。 選択、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued` チェック ボックスの一覧から列です。


[![ProductID、ProductName、単価、および提供が中止された列を返す、Products テーブルを使用して、](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**図 7**: を使用して、`Products`テーブルで、返す、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`列 ([フルサイズのイメージを表示するをクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


自動的に生成`INSERT`、 `UPDATE`、および`DELETE`、選択したテーブルと列に基づいて、ステートメントが高度なボタンをクリックし、確認の生成`INSERT`、`UPDATE`と`DELETE`ステートメントのチェック ボックスをオンします。


![生成 INSERT、UPDATE、および DELETE ステートメントのチェック ボックス](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**図 8**: 確認生成`INSERT`、 `UPDATE`、および`DELETE`ステートメント チェック ボックス


生成`INSERT`、 `UPDATE`、および`DELETE`選択されたテーブルが主キーと主キー列 (または列) が返される列の一覧に含まれている場合、ステートメントのチェック ボックスはチェック可能必ずです。 選択可能になりますオプティミスティック同時実行制御チェック ボックスを使用して 1 回生成`INSERT`、 `UPDATE`、および`DELETE`ステートメントのチェック ボックスがチェックされている、強化は、`WHERE`結果内の句`UPDATE`と`DELETE`オプティミスティック同時実行制御を提供するステートメント。 ここでは、このチェック ボックスをオンにせずです。次のチュートリアルでは、SqlDataSource コントロールでオプティミスティック同時実行制御について確認します。

生成をチェックした後`INSERT`、 `UPDATE`、および`DELETE`ステートメント、Select ステートメントの構成画面に戻り、次に、をクリックし [ok] をクリックしてボックスとし、[完了] で、データ ソースの構成ウィザードを完了します。 ウィザードを完了すると、Visual Studio が追加 BoundFields の DetailsView、 `ProductID`、 `ProductName`、および`UnitPrice`列との CheckBoxField、`Discontinued`列です。 DetailsView s のスマート タグからこのページにアクセスしたユーザーを製品ステップスルーようにページングを有効にするオプションを確認します。 DetailsView s もクリア`Width`と`Height`プロパティです。

スマート タグに挿入を有効にする、編集を有効にするには、および削除を有効にするオプションがありますに注意してください。 これは、SqlDataSource に値が含まれているため、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`次の宣言の構文に示すように、します。

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

値が自動的に設定する SqlDataSource コントロールがどのようにに注意してください。 その`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティです。 参照される列のセット、`InsertCommand`と`UpdateCommand`プロパティに基づいて内、`SELECT`ステートメントです。 つまりではなく*すべて*製品列で、`InsertCommand`と`UpdateCommand`で指定された列のみがある、 `SelectCommand` (小さい`ProductID`、ためこれを省略すると、s、 [`IDENTITY`列](http://www.sqlteam.com/item.asp?ItemID=102)編集する際に、値が変更されたことはできません、および挿入するときに自動的に割り当てられる)。 さらの各パラメーターに、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティに対応するパラメーターがある、 `InsertParameters`、 `UpdateParameters`、および`DeleteParameters`コレクション。

DetailsView のデータ変更機能を有効にするには、挿入を有効にする、編集を有効にして、スマート タグの削除を有効にするオプションを確認します。 CommandField が追加されます、 `ShowInsertButton`、 `ShowEditButton`、および`ShowDeleteButton`プロパティに設定`true`です。

ブラウザーでページを参照してください、編集、削除、および DetailsView に含まれる新しいボタンに注意してください。 各 BoundField を表示する編集モードに DetailsView をオンに [編集] ボタンをクリックするとが`ReadOnly`プロパティに設定されている`false`(既定)、テキスト ボックスとチェック ボックスとして CheckBoxField として。


[![DetailsView の既定の編集用のインターフェイス](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**図 9**:「DetailsView s 編集インターフェイス既定の ([フルサイズ イメージを表示するに、をクリックして](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


同様に、現在選択されている製品を削除したり、新しい製品をシステムに追加できます。 以降、`InsertCommand`ステートメントに対してのみ適用、 `ProductName`、 `UnitPrice`、および`Discontinued`列、他の列であるか、`NULL`または挿入時にデータベースによって割り当てられる既定値です。 同じように、ObjectDataSource の場合、`InsertCommand`ない列で使用できる任意のデータベース テーブルがありません`NULL`s としない既定の値を持つ、SQL エラーが発生実行しようとするとき、`INSERT`ステートメントです。

> [!NOTE]
> DetailsView の s を挿入して、インターフェイスを編集するには、あらゆる種類のカスタマイズや検証が不足しています。 検証コントロールを追加する、またはインターフェイスをカスタマイズするには、TemplateFields、BoundFields に変換する必要があります。 参照してください、[検証コントロールを編集および挿入するインターフェイスを追加する](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)と[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)詳細については、チュートリアルです。


またの更新と削除を DetailsView で使用される、現在の製品の注意してください`DataKey`場合にのみ存在する値、`DataKeyNames`プロパティを構成します。 編集または削除に見える場合がある影響しない、いることを確認、`DataKeyNames`プロパティを設定します。

## <a name="limitations-of-automatically-generating-sql-statements"></a>SQL ステートメントを自動的に生成の制限事項

生成後`INSERT`、 `UPDATE`、および`DELETE`ステートメント オプションは、独自に作成する必要があるより複雑なため、テーブルの列を選択するときにのみ使用可能な`INSERT`、 `UPDATE`、および`DELETE`手順 1. で同じようにステートメントです。 一般的には、SQL`SELECT`ステートメントを使用して`JOIN`の 1 つまたは複数のルックアップ テーブルからデータを表示目的に戻すには (元に戻す方法など、`Categories`表の`CategoryName`製品情報を表示するときにフィールド)。 同時に、編集、更新、または、中核となるテーブルにデータを挿入するユーザーを許可するか可能性があります (`Products`、ここで)。

中に、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントが手動で入力することができますし、次の時間を節約のヒントを検討してください。 最初に、バックアップからデータをプルしたように、SqlDataSource をセットアップ、`Products`テーブル。 自動的に生成できるように、テーブルまたはビューの画面からデータ ソースの構成ウィザードの列と s を指定を使用して、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントです。 次に、ウィザードを完了すると、[プロパティ] ウィンドウから SelectQuery を構成する (か、または、使用するカスタム SQL ステートメントの指定が、データ ソース構成ウィザードまたはストアド プロシージャのオプションに戻る)。 更新して、`SELECT`を含めるようにステートメント、`JOIN`構文です。 この手法は、時間節約の利点が自動的に生成された SQL ステートメントのでき、さらにカスタマイズしたことができます`SELECT`ステートメントです。

自動的に生成する別の制限、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントは、内の列を提供する、`INSERT`と`UPDATE`ステートメントは、によって返される列に基づいて、`SELECT`ステートメントです。 更新またはただし、またはより少ないフィールドを挿入する必要があります。 たとえば、手順 2 の例にすることができますが、 `UnitPrice` BoundField 読み取り専用になります。 その場合は、それに表示されるべきで t、`UpdateCommand`です。 またはに GridView で表示されていない、テーブルのフィールドの値を設定することがあります。 たとえば、新しいを追加するときにレコードお場合、`QuantityPerUnit`値 TODO に設定します。

このようなカスタマイズが必要な場合は、ように手動で、[プロパティ] ウィンドウ、カスタム SQL ステートメントの指定またはウィザードで、または宣言の構文を使用してストアド プロシージャのオプションから必要があります。

> [!NOTE]
> データの対応するフィールドがないパラメーターの追加は、コントロールを Web、ときに、これらのパラメーターの値は、なんらかの方法で値を割り当てる必要があることに注意してください。 これらの値を指定できます。 ハード コーディングされたで直接、`InsertCommand`または`UpdateCommand`; は、(クエリ文字列、セッション状態、ページ上の Web コントロール); いくつかの事前定義されたソースから取得できますか、またはプログラムでは、前のチュートリアルで示したように割り当てることができます。


## <a name="summary"></a>まとめ

Web コントロールを組み込みの挿入、編集、および削除機能を利用して、データの順序でにバインドされているデータ ソース コントロールは、このような機能を提供する必要があります。 SqlDataSource、これは意味を`INSERT`、 `UPDATE`、および`DELETE`SQL ステートメントを割り当てる必要があります、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティです。 これらのプロパティとは対応するパラメーターのコレクションを手動で追加またはデータ ソースの構成ウィザードを自動的に生成します。 このチュートリアルでは、両方の方法を確認します。

ObjectDataSource でオプティミスティック同時実行制御の使用について説明しました、[オプティミスティック同時実行制御で実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)チュートリアルです。 SqlDataSource コントロールでは、オプティミスティック同時実行制御のサポートも提供します。 自動的に生成するときに、手順 2. で説明したとおり、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントを使用するオプティミスティック同時実行制御オプションに、ウィザードで提供しています。 次のチュートリアルで後ほどお見せ、として、SqlDataSource によるオプティミスティック同時実行制御を使用して変更、`WHERE`内の句、`UPDATE`と`DELETE`ステートメントを他の列の値が t の後、データが最後に変更をまだことを確認してください。ページに表示されます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [次へ](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
