---
uid: web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
title: '[How Do i:]Excel のようなアプリケーションのデータをコンマ区切り (CSV) ファイルにエクスポート |Microsoft Docs'
author: rick-anderson
description: このビデオでは、Chris Pels は、データベースまたはその他のソースからデータを取得し、アプリケーション、li で使用できるコンマ区切りファイルにエクスポートする方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/22/2009
ms.topic: article
ms.assetid: c9df86ad-aec2-43d5-bb8a-413ebb666673
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
msc.type: video
ms.openlocfilehash: 7c1f94118c64fee7f4198cd096ae2ef200981ba8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402604"
---
<a name="how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel"></a><span data-ttu-id="fbac6-103">[How Do i:]Excel のようなアプリケーションのデータをコンマ区切り (CSV) ファイルにエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="fbac6-103">[How Do I:] Export Data to a Comma Delimited (CSV) File for an Application Like Excel</span></span>
====================
<span data-ttu-id="fbac6-104">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="fbac6-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="fbac6-105">このビデオでは、Chris Pels は、データベースまたはその他のソースからデータを取得し、Excel などのアプリケーションで使用できるコンマ区切りファイルにエクスポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fbac6-105">In this video Chris Pels shows how to take data from a database or other source and export it to a comma delimited file that can be used in an application like Excel.</span></span> <span data-ttu-id="fbac6-106">最初に、一連のデータは、DataTable オブジェクトとして作成されます。</span><span class="sxs-lookup"><span data-stu-id="fbac6-106">First, a set of data is created as a DataTable object.</span></span> <span data-ttu-id="fbac6-107">次に、現在の web ページ要求に対する応答がオフになってされ、ヘッダーとコンテンツの種類は、csv ファイルに構成されます。</span><span class="sxs-lookup"><span data-stu-id="fbac6-107">Next, the Response for the current web page request is cleared and the header and content type are configured to be a csv file.</span></span> <span data-ttu-id="fbac6-108">実際のデータは、最初にデータ値の後に、csv ファイルの列ヘッダーを記述することで、応答ストリームに追加されます。</span><span class="sxs-lookup"><span data-stu-id="fbac6-108">Then the actual data is added to the response stream by first writing the column headers for the csv file followed by the data values.</span></span> <span data-ttu-id="fbac6-109">このアプローチは、ユーザーは、Excel などのプログラムでローカルで操作できるように、データのエクスポートを必要とするときに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="fbac6-109">This approach can be useful when users require an export of data so it can be manipulated locally in a program like Excel.</span></span>

[<span data-ttu-id="fbac6-110">&#9654;(19 分) のビデオを見る</span><span class="sxs-lookup"><span data-stu-id="fbac6-110">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel)
