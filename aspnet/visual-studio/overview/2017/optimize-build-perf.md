---
uid: visual-studio/overview/2017/optimize-build-perf
title: ソリューションのビルドのパフォーマンスを最適化します。
author: tfitzmac
description: ソリューションのビルドのパフォーマンスを最適化します。
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909894"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="aa754-103">ソリューションのビルドのパフォーマンスを最適化します。</span><span class="sxs-lookup"><span data-stu-id="aa754-103">Optimize build performance for solution</span></span>
<span data-ttu-id="aa754-104">Visual Studio 2017 15.8年後から 新しいメニュー項目を追加および**ビルド > ASP.NET コンパイル > ソリューションのビルド パフォーマンスの最適化**します。</span><span class="sxs-lookup"><span data-stu-id="aa754-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![新しいメニュー項目のスクリーン ショット](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="aa754-106">ASP.NET では、コンパイラのコピーを実行して、ASP.NET プロジェクトは、実行時にそのビューをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="aa754-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="aa754-107">ただし、開発者コンピューターで、コンパイラのコピーは、Visual Studio のコピーと一致しない場合、ビルドのパフォーマンスが影響を受けるインクリメンタル ビルドごとに 1 ~ 3 秒の順序。</span><span class="sxs-lookup"><span data-stu-id="aa754-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="aa754-108">この機能では、Visual Studio のインクリメンタル ビルドを高速化する必要がありますが一致するようにコンパイラのプロジェクトのコピーを更新します。</span><span class="sxs-lookup"><span data-stu-id="aa754-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="aa754-109">これは ASP.NET フレームワークのプロジェクトのみに適用されます、ASP.NET Core には適用されません。</span><span class="sxs-lookup"><span data-stu-id="aa754-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
