---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: カスタム ルート (VB) を作成する |Microsoft Docs
author: microsoft
description: ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 このチュートリアルでは、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d6125f93d99acc695be319bd000ff65ab4a1159
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829701"
---
<a name="creating-custom-routes-vb"></a>カスタム ルート (VB) を作成します。
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 このチュートリアルでは、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。


このチュートリアルでは、ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 カスタム ルートを使用して、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。

ASP.NET MVC アプリケーションでは既定のルーティング テーブルが正しく機能します。 ただし、ルーティングのニーズが特殊化されたことを見つけることがあります。 その場合は、カスタム ルートを作成することができます。

たとえば、ブログ アプリケーションを作成することは想像してください。 次のように受信要求を処理する可能性があります。

/アーカイブ/12-25-2009 年

日付に対応するブログ エントリを取得するユーザーには、この要求が入ると、2009 年 12 月 25 日。 この種の要求を処理するためには、カスタム ルートを作成する必要があります。

リスト 1 で、Global.asax ファイルには、ブログ、/Archive/のような要求を処理するという名前の新しいカスタム ルートが含まれています。*エントリの日付*します。

**1 - Global.asax (カスタム ルーティングあり) を一覧表示します。**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

ルート テーブルに追加するルートの順序が重要です。 既存の既定のルートの前に、新しいカスタムのブログのルートが追加されます。 順序を反転する場合、既定のルート常に呼び出されるカスタム ルートではなく。

カスタムのブログのルートでは、/アーカイブ/で始まるすべての要求と一致します。 そのため、次の Url はすべて一致します。

- /アーカイブ/12-25-2009 年

- /アーカイブ/10-6-2004

- /アーカイブ/apple

カスタム ルートでは、アーカイブをという名前のコント ローラーに、受信要求をマップし、Entry() アクションを呼び出します。 Entry() メソッドが呼び出されたときに、エントリの日付が entryDate という名前のパラメーターとして渡されます。

リスト 2 でコント ローラーでは、ブログのカスタム ルートを使用できます。

**2 - ArchiveController.vb を一覧表示します。**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

リスト 2 で Entry() メソッドが DateTime 型のパラメーターを受け入れることに注意してください。 MVC フレームワークには、URL からエントリの日付を自動的に DateTime 値に変換します。 URL からエントリの日付のパラメーターは、DateTime に変換できない、エラーが発生します (図 1 参照)。

**図 1 - パラメーターの変換からのエラー**


[![[新しいプロジェクト] ダイアログ ボックス](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**図 01**: パラメーターの変換からのエラー ([フルサイズの画像を表示する をクリックします](creating-custom-routes-vb/_static/image2.png))。


## <a name="summary"></a>まとめ

このチュートリアルの目的は、カスタム ルートを作成する方法を説明することでした。 ブログ エントリを表す、Global.asax ファイルのルート テーブルにカスタム ルートを追加する方法を学習しました。 ブログのエントリに対する要求を ArchiveController という名前のコント ローラーと Entry() という名前のコント ローラー アクションにマップする方法を説明しました。

> [!div class="step-by-step"]
> [前へ](asp-net-mvc-controller-overview-vb.md)
> [次へ](creating-a-route-constraint-vb.md)
