---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: このビデオの Chris Pels では、ユーザー コントロール内の 1 つまたは複数のオブジェクトの状態を維持する方法を示します。 まず、abilit を表すユーザー コントロールを作成しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: fde95af4f639d778a108a0267fb738ac2e0a2d46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372612"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[操作方法]: ポストバック時にユーザー コントロールの状態を永続化
====================
によって[Chris Pels](https://twitter.com/chrispels)

このビデオの Chris Pels では、ユーザー コントロール内の 1 つまたは複数のオブジェクトの状態を維持する方法を示します。 最初に、ユーザーが検索のフィルター条件を指定するための機能を表すユーザー コントロールが作成されます。 さらに、フィルター情報を格納するコンパニオン フィルター クラスが作成されます。 いくつかのユーザー インターフェイス要素は、いくつかのメソッドとプロパティ フィルター クラスのインスタンスに現在のフィルター情報を格納すると共に、フィルター コントロールに追加されます。 次に、ユーザー コントロールの永続化では、RegisterRequiresControlState メソッドと保存/復元の関連するメソッドを使用して実装されます。 これらのメソッドは、ページのポストバック時にフィルター クラスとそのデータのインスタンスを格納します。 最後に、状態のコントロール実装で複数のオブジェクトを格納する方法の詳細については。

[&#9654;ビデオ (23 分)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
