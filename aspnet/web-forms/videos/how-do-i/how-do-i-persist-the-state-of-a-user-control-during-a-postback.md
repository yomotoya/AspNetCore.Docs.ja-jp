---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: "[操作方法]: ポストバック時にユーザー コントロールの状態を永続化 |Microsoft ドキュメント"
author: rick-anderson
description: "このビデオ Chris Pels では、ユーザー コントロールに 1 つまたは複数のオブジェクトの状態を維持する方法を示します。 最初に、ユーザー コントロールを表す、abilit が作成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="49eb9-104">[操作方法]: ポストバック時にユーザー コントロールの状態を維持</span><span class="sxs-lookup"><span data-stu-id="49eb9-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="49eb9-105">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="49eb9-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="49eb9-106">このビデオ Chris Pels では、ユーザー コントロールに 1 つまたは複数のオブジェクトの状態を維持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="49eb9-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="49eb9-107">まず、検索のフィルター条件を指定するユーザーの機能を表し、ユーザー コントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="49eb9-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="49eb9-108">さらに、フィルター情報を格納するコンパニオン フィルター クラスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="49eb9-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="49eb9-109">いくつかのユーザー インターフェイス要素は、いくつかのメソッドと、フィルター クラスのインスタンスに現在のフィルター情報を格納するプロパティと共にフィルター コントロールに追加されます。</span><span class="sxs-lookup"><span data-stu-id="49eb9-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="49eb9-110">次に、ユーザー コントロールの永続化は RegisterRequiresControlState メソッドおよび関連する保存/復元メソッドを使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="49eb9-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="49eb9-111">これらのメソッドは、ページのポストバック時に、フィルター クラスとそのデータのインスタンスを格納します。</span><span class="sxs-lookup"><span data-stu-id="49eb9-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="49eb9-112">さらに、コントロールの状態の実装に複数のオブジェクトを格納する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="49eb9-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="49eb9-113">&#9654;です。ビデオでは (23 分)</span><span class="sxs-lookup"><span data-stu-id="49eb9-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
