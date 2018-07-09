---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: このビデオの Chris Pels では、ユーザー コントロール内の 1 つまたは複数のオブジェクトの状態を維持する方法を示します。 まず、abilit を表すユーザー コントロールを作成しています.
ms.author: aspnetcontent
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 7862d83d3df3ca5407b7d8fd465cf42da8e7228a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805179"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[操作方法]: ポストバック時にユーザー コントロールの状態を永続化
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="90eb7-104">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="90eb7-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="90eb7-105">このビデオの Chris Pels では、ユーザー コントロール内の 1 つまたは複数のオブジェクトの状態を維持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="90eb7-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="90eb7-106">最初に、ユーザーが検索のフィルター条件を指定するための機能を表すユーザー コントロールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="90eb7-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="90eb7-107">さらに、フィルター情報を格納するコンパニオン フィルター クラスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="90eb7-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="90eb7-108">いくつかのユーザー インターフェイス要素は、いくつかのメソッドとプロパティ フィルター クラスのインスタンスに現在のフィルター情報を格納すると共に、フィルター コントロールに追加されます。</span><span class="sxs-lookup"><span data-stu-id="90eb7-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="90eb7-109">次に、ユーザー コントロールの永続化では、RegisterRequiresControlState メソッドと保存/復元の関連するメソッドを使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="90eb7-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="90eb7-110">これらのメソッドは、ページのポストバック時にフィルター クラスとそのデータのインスタンスを格納します。</span><span class="sxs-lookup"><span data-stu-id="90eb7-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="90eb7-111">最後に、状態のコントロール実装で複数のオブジェクトを格納する方法の詳細については。</span><span class="sxs-lookup"><span data-stu-id="90eb7-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="90eb7-112">&#9654;ビデオ (23 分)</span><span class="sxs-lookup"><span data-stu-id="90eb7-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
