---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: "データベース (VB) で CascadingDropDown を使用して |Microsoft ドキュメント"
author: wenz
description: "CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 65b9a499dd9b500338ccdb90e56b742ff50a1024
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>データベース (VB) で CascadingDropDown の使用
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 これを行うためには、特別な web サービスを作成する必要があります。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。 (たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。これを行うためには、特別な web サービスを作成する必要があります。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?表示 = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。

このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。 セットアップが異なっている場合は、データベースの接続情報を調整する必要です。

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、 &lt; `form` &gt;要素)。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

次の手順では、2 つの DropDownList コントロールが必要です。 このサンプルでは、AdventureWorks から仕入先と連絡先情報は、使用、したがって利用可能なベンダーと使用可能なアドレス帳のいずれか 1 つのリストを作成します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

次に、2 つの CascadingDropDown エクステンダーは、ページに追加する必要があります。 最初の (ベンダー) のリストを塗りつぶしますいずれかと 2 番目の (連絡先) のリストを塗りつぶします、もう 1 つです。 次の属性を設定する必要があります。

- `ServicePath`: 一覧のエントリを提供する web サービスの URL
- `ServiceMethod`: Web メソッドの一覧のエントリを提供します。
- `TargetControlID`。 ドロップダウン リストの ID
- `Category`: カテゴリについては、web メソッドが呼び出されたときに送信されます。
- `PromptText`: サーバーから非同期的にリストのデータを読み込むときに表示されるテキスト
- `ParentControlID`: (省略可能) 親ボックスの一覧で現在のリストの場合は、そのトリガー読み込み

使用するプログラミング言語に応じて、問題のある web サービスの名前を変更が他のすべての属性値が同じ。 最初のドロップダウン リストの CascadingDropDown 要素を次に示します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

2 番目のリストのコントロール エクステンダーを設定する必要があります、`ParentControlID`属性の関連付けられている連絡先リストに要素を読み込み仕入先一覧トリガー内のエントリを選択するようにします。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

実績作業時間は、次のように設定された web サービスで行われます。 なお、`[ScriptService]`属性を使用すると、それ以外の場合 ASP.NET AJAX でクライアント側のスクリプト コードから web メソッドにアクセスする JavaScript プロキシを作成できません。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

CascadingDropDown によって呼び出される web メソッドのシグネチャは次のとおりです。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

戻り値の型の配列でなければなりませんように`CascadingDropDownNameValue`コントロール ツールキットによって定義されています。 `GetVendors()`メソッドは非常に簡単に実装します。 コードは、AdventureWorks データベースに接続し、最初の 25 個のベンダーのクエリを実行します。 最初のパラメーター、`CascadingDropDownNameValue`コンス トラクターは一覧のエントリ、2 つ目のキャプション、その値 (html の value 属性&lt; `option` &gt;要素)。 コードを次に示します。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

仕入先の関連付けられている連絡先の取得 (メソッド名: `GetContactsForVendor()`) は、少し複雑になります。 最初に、最初のドロップダウン リストから選択されている仕入先を決定する必要があります。 コントロール Toolkit は、そのタスクのヘルパー メソッドを定義します。、`ParseKnownCategoryValuesString()`メソッドを返します、`StringDictionary`ドロップダウン リストのデータを持つ要素。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

セキュリティ上の理由から、このデータを最初に検証する必要があります。 仕入先のエントリがある場合は (ため、 `Category` CascadingDropDown の最初の要素のプロパティに設定されて`"Vendor"`)、選択したベンダの ID を取得することがあります。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

メソッドの残りの部分がかなり簡単、なります。 仕入先の ID は、そのベンダーのすべての関連付けられている連絡先を取得する SQL クエリのパラメーターとして使用されます。 型の配列を返します、もう一度`CascadingDropDownNameValue`です。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

ASP.NET ページを読み込むされ、しばらくすると、後にベンダーの一覧が 25 エントリが入力されます。 1 つのエントリを取得し、データが 2 番目のドロップダウン リストを格納する方法に注意してください。


[![最初のリストが自動的に入力します。](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

最初のリストが自動的に入力 ([フルサイズのイメージを表示するをクリックして](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![最初のリストの選択に応じて、2 番目の一覧を入力します。](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

最初のリストの選択に応じて、2 番目の一覧を入力 ([フルサイズのイメージを表示するをクリックして](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

>[!div class="step-by-step"]
[前へ](filling-a-list-using-cascadingdropdown-vb.md)
[次へ](presetting-list-entries-with-cascadingdropdown-vb.md)
