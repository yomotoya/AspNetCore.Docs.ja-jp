---
uid: visual-studio/overview/2017/optimize-build-perf
title: ソリューションのビルドのパフォーマンスを最適化します。
author: AngelosP
description: ソリューションのビルドのパフォーマンスを最適化します。
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312142"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="c4920-103">ソリューションのビルドのパフォーマンスを最適化します。</span><span class="sxs-lookup"><span data-stu-id="c4920-103">Optimize build performance for solution</span></span>

<span data-ttu-id="c4920-104">Visual Studio 2017 15.8以降には次のメニュー項目が追加されています: **ビルド** > **ASP.NET コンパイル** > **ソリューションのビルド パフォーマンスの最適化**</span><span class="sxs-lookup"><span data-stu-id="c4920-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![新しいメニュー項目のスクリーン ショット](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="c4920-106">ASP.NET、ASP.NET プロジェクトを伴って、コンパイラのコピーは、実行時にそのビューをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="c4920-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="c4920-107">ただし、開発者のコンピューターで、コンパイラのコピーは、Visual Studio のコピーと一致しない場合ビルド パフォーマンスに影響インクリメンタル ビルドごとに 1 ~ 3 秒の順序。</span><span class="sxs-lookup"><span data-stu-id="c4920-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="c4920-108">この機能は、通常、インクリメンタル ビルドが高速化、Visual Studio の一致するようにコンパイラのプロジェクトのコピーを更新します。</span><span class="sxs-lookup"><span data-stu-id="c4920-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="c4920-109">**これは ASP.NET Framework 4.7.1 に適用されますまたはプロジェクトが後でのみ、ASP.NET Core には適用されません。**</span><span class="sxs-lookup"><span data-stu-id="c4920-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
