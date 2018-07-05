---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: カスタム ルート (c#) を作成する |Microsoft Docs
author: microsoft
description: ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 このチュートリアルでは、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bb694b08d595b2ce75123c3da0e9b8e8d60a652
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399958"
---
<a name="creating-custom-routes-c"></a>カスタム ルートを作成する (c#)
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 このチュートリアルでは、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。


このチュートリアルでは、ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 カスタム ルートを使用して、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。

多くの単純な ASP.NET MVC アプリケーションの既定のルーティング テーブルは問題なく動作します。 ただし、ルーティングのニーズが特殊化されたことを見つけることがあります。 その場合は、カスタム ルートを作成することができます。

たとえば、ブログ アプリケーションを作成することは想像してください。 次のように受信要求を処理する可能性があります。

/アーカイブ/12-25-2009 年

日付に対応するブログ エントリを取得するユーザーには、この要求が入ると、2009 年 12 月 25 日。 この種の要求を処理するためには、カスタム ルートを作成する必要があります。

リスト 1 で、Global.asax ファイルには、ブログ、/Archive/のような要求を処理するという名前の新しいカスタム ルートが含まれています。*エントリの日付*します。

**1 - Global.asax (カスタム ルーティングあり) を一覧表示します。**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

ルート テーブルに追加するルートの順序が重要です。 既存の既定のルートの前に、新しいカスタムのブログのルートが追加されます。 順序を反転する場合、既定のルート常に呼び出されるカスタム ルートではなく。

カスタムのブログのルートでは、/アーカイブ/で始まるすべての要求と一致します。 そのため、次の Url はすべて一致します。

- /アーカイブ/12-25-2009 年

- /アーカイブ/10-6-2004

- /アーカイブ/apple

カスタム ルートでは、アーカイブをという名前のコント ローラーに、受信要求をマップし、Entry() アクションを呼び出します。 Entry() メソッドが呼び出されたときに、エントリの日付が entryDate という名前のパラメーターとして渡されます。

リスト 2 でコント ローラーでは、ブログのカスタム ルートを使用できます。

**2 - ArchiveController.cs を一覧表示します。**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

リスト 2 で Entry() メソッドが DateTime 型のパラメーターを受け入れることに注意してください。 MVC フレームワークには、URL からエントリの日付を自動的に DateTime 値に変換します。 URL からエントリの日付のパラメーターは、DateTime に変換できない、エラーが発生します (図 1 参照)。

**図 1 - パラメーターの変換からのエラー**


[![[新しいプロジェクト] ダイアログ ボックス](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**図 01**: パラメーターの変換からのエラー ([フルサイズの画像を表示する をクリックします](creating-custom-routes-cs/_static/image2.png))。


## <a name="summary"></a>まとめ

このチュートリアルの目的は、カスタム ルートを作成する方法を説明することでした。 ブログ エントリを表す、Global.asax ファイルのルート テーブルにカスタム ルートを追加する方法を学習しました。 ブログのエントリに対する要求を ArchiveController という名前のコント ローラーと Entry() という名前のコント ローラー アクションにマップする方法を説明しました。

> [!div class="step-by-step"]
> [前へ](aspnet-mvc-controllers-overview-cs.md)
> [次へ](creating-a-route-constraint-cs.md)
