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
# <a name="optimize-build-performance-for-solution"></a>ソリューションのビルドのパフォーマンスを最適化します。
Visual Studio 2017 15.8年後から 新しいメニュー項目を追加および**ビルド > ASP.NET コンパイル > ソリューションのビルド パフォーマンスの最適化**します。

![新しいメニュー項目のスクリーン ショット](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET では、コンパイラのコピーを実行して、ASP.NET プロジェクトは、実行時にそのビューをコンパイルします。 ただし、開発者コンピューターで、コンパイラのコピーは、Visual Studio のコピーと一致しない場合、ビルドのパフォーマンスが影響を受けるインクリメンタル ビルドごとに 1 ~ 3 秒の順序。 この機能では、Visual Studio のインクリメンタル ビルドを高速化する必要がありますが一致するようにコンパイラのプロジェクトのコピーを更新します。

これは ASP.NET フレームワークのプロジェクトのみに適用されます、ASP.NET Core には適用されません。
