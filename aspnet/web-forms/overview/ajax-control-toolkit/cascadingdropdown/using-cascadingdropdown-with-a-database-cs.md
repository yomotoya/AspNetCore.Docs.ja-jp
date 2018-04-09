---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: データベース (c#) で CascadingDropDown の使用 |Microsoft ドキュメント
author: wenz
description: CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-cascadingdropdown-with-a-database-c"></a>データベース (c#) で CascadingDropDown の使用
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 これを行うためには、特別な web サービスを作成する必要があります。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。これを行うためには、特別な web サービスを作成する必要があります。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。

このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。 セットアップが異なっている場合は、データベースの接続情報を調整する必要です。

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、 &lt; `form` &gt;要素)。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

次の手順では、2 つの DropDownList コントロールが必要です。 このサンプルでは、AdventureWorks から仕入先と連絡先情報は、使用、したがって利用可能なベンダーと使用可能なアドレス帳のいずれか 1 つのリストを作成します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

次に、2 つの CascadingDropDown エクステンダーは、ページに追加する必要があります。 最初の (ベンダー) のリストを塗りつぶしますいずれかと 2 番目の (連絡先) のリストを塗りつぶします、もう 1 つです。 次の属性を設定する必要があります。

- `ServicePath`: 一覧のエントリを提供する web サービスの URL
- `ServiceMethod`: Web メソッドの一覧のエントリを提供します。
- `TargetControlID`。 ドロップダウン リストの ID
- `Category`: カテゴリについては、web メソッドが呼び出されたときに送信されます。
- `PromptText`: サーバーから非同期的にリストのデータを読み込むときに表示されるテキスト
- `ParentControlID`: (省略可能) 親ボックスの一覧で現在のリストの場合は、そのトリガー読み込み

使用するプログラミング言語に応じて、問題のある web サービスの名前を変更が他のすべての属性値が同じ。 最初のドロップダウン リストの CascadingDropDown 要素を次に示します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

2 番目のリストのコントロール エクステンダーを設定する必要があります、`ParentControlID`属性の関連付けられている連絡先リストに要素を読み込み仕入先一覧トリガー内のエントリを選択するようにします。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

実績作業時間は、次のように設定された web サービスで行われます。 なお、`[ScriptService]`属性を使用すると、それ以外の場合 ASP.NET AJAX でクライアント側のスクリプト コードから web メソッドにアクセスする JavaScript プロキシを作成できません。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

CascadingDropDown によって呼び出される web メソッドのシグネチャは次のとおりです。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

戻り値の型の配列でなければなりませんように`CascadingDropDownNameValue`コントロール ツールキットによって定義されています。 `GetVendors()`メソッドは非常に簡単に実装します。 コードは、AdventureWorks データベースに接続し、最初の 25 個のベンダーのクエリを実行します。 最初のパラメーター、`CascadingDropDownNameValue`コンス トラクターは一覧のエントリ、2 つ目のキャプション、その値 (html の value 属性&lt; `option` &gt;要素)。 コードを次に示します。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

仕入先の関連付けられている連絡先の取得 (メソッド名: `GetContactsForVendor()`) は、少し複雑になります。 最初に、最初のドロップダウン リストから選択されている仕入先を決定する必要があります。 コントロール Toolkit は、そのタスクのヘルパー メソッドを定義します。、`ParseKnownCategoryValuesString()`メソッドを返します、`StringDictionary`ドロップダウン リストのデータを持つ要素。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

セキュリティ上の理由から、このデータを最初に検証する必要があります。 仕入先のエントリがある場合は (ため、 `Category` CascadingDropDown の最初の要素のプロパティに設定されて`"Vendor"`)、選択したベンダの ID を取得することがあります。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

メソッドの残りの部分がかなり簡単、なります。 仕入先の ID は、そのベンダーのすべての関連付けられている連絡先を取得する SQL クエリのパラメーターとして使用されます。 型の配列を返します、もう一度`CascadingDropDownNameValue`です。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

ASP.NET ページを読み込むされ、しばらくすると、後にベンダーの一覧が 25 エントリが入力されます。 1 つのエントリを取得し、データが 2 番目のドロップダウン リストを格納する方法に注意してください。


[![最初のリストが自動的に入力します。](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

最初のリストが自動的に入力 ([フルサイズのイメージを表示するをクリックして](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![最初のリストの選択に応じて、2 番目の一覧を入力します。](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

最初のリストの選択に応じて、2 番目の一覧を入力 ([フルサイズのイメージを表示するをクリックして](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [前へ](filling-a-list-using-cascadingdropdown-cs.md)
> [次へ](presetting-list-entries-with-cascadingdropdown-cs.md)
