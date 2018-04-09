---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: (C#) のカスタム ルートを作成 |Microsoft ドキュメント
author: microsoft
description: ASP.NET MVC アプリケーションにカスタム ルートを追加する方法を説明します。 このチュートリアルでは、Global.asax ファイルで既定のルート テーブルを変更する方法を学習します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-c"></a>カスタム ルートを作成する (c#)
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC アプリケーションにカスタム ルートを追加する方法を説明します。 このチュートリアルでは、Global.asax ファイルで既定のルート テーブルを変更する方法を学習します。


このチュートリアルでは、ASP.NET MVC アプリケーションにカスタム ルートを追加する方法を学習します。 カスタム ルートを Global.asax ファイルで既定のルート テーブルを変更する方法を学びます。

多くの単純な ASP.NET MVC アプリケーションは既定のルート テーブルが正しく機能します。 ただし、ルーティングのニーズが専門的なことを検出できます。 その場合は、カスタム ルートを作成することができます。

たとえば、ブログ アプリケーションを構築することに想像してください。 次のように入力方向の要求を処理する可能性があります。

/アーカイブ/12-25-2009

日付に対応するブログ エントリを返すユーザーは、この要求が入力、2009 年 12 月 25 日。 この種類の要求を処理するためには、カスタム ルートを作成する必要があります。

リスト 1 の Global.asax ファイルには、ブログ、/Archive/のようなどのハンドル要求をという名前の新しいカスタム ルートが含まれている*エントリ日付*です。

**1 - (カスタム ルーティング) を持つ Global.asax を一覧表示します。**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

ルート テーブルに追加するルートの順序が重要です。 既存の既定のルートの前に、新しいカスタム ブログ ルートが追加されます。 順序を反転する場合、既定のルート常に呼び出されるカスタム ルートの代わりにします。

カスタムのブログ ルートでは、/アーカイブ/で始まるすべての要求と一致します。 そのため、すべて次の Url の一致します。

- /アーカイブ/12-25-2009

- /アーカイブ 2004/10-6-

- /Archive/apple

カスタム ルートは、Archive というコント ローラーに、受信要求をマップし、Entry() アクションを呼び出します。 Entry() メソッドが呼び出されると、エントリの日付は entryDate をという名前のパラメーターとして渡されます。

一覧表示する 2 でのコント ローラーでは、ブログのカスタム ルートを使用できます。

**2 - ArchiveController.cs を一覧表示します。**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

リスト 2 で Entry() メソッドが DateTime 型のパラメーターを受け入れることに注意してください。 MVC フレームワークには、URL からのエントリの日付を自動的に DateTime 値に変換します。 エラーが発生する場合は、URL からのエントリの日付のパラメーターは、DateTime に変換できない、(図 1 を参照してください)。

**図 1 - パラメーターの変換からのエラー**


[![[新しいプロジェクト] ダイアログ ボックス](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**図 01**: パラメーターの変換からのエラー ([フルサイズのイメージを表示するをクリックして](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>まとめ

このチュートリアルの目的は、カスタム ルートを作成する方法を示すためにでした。 ブログ エントリを表す Global.asax ファイルでルート テーブルに対するカスタム ルートを追加する方法を学習しました。 ブログ エントリの要求を ArchiveController をという名前、コント ローラーと Entry() をという名前のコント ローラー アクションにマップする方法を説明したとします。

> [!div class="step-by-step"]
> [前へ](aspnet-mvc-controllers-overview-cs.md)
> [次へ](creating-a-route-constraint-cs.md)
