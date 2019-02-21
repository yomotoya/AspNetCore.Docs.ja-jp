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
# <a name="optimize-build-performance-for-solution"></a>ソリューションのビルドのパフォーマンスを最適化します。

Visual Studio 2017 15.8以降には次のメニュー項目が追加されています: **ビルド** > **ASP.NET コンパイル** > **ソリューションのビルド パフォーマンスの最適化**。

![新しいメニュー項目のスクリーン ショット](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET、ASP.NET プロジェクトを伴って、コンパイラのコピーは、実行時にそのビューをコンパイルします。 ただし、開発者のコンピューターで、コンパイラのコピーは、Visual Studio のコピーと一致しない場合ビルド パフォーマンスに影響インクリメンタル ビルドごとに 1 ~ 3 秒の順序。 この機能は、通常、インクリメンタル ビルドが高速化、Visual Studio の一致するようにコンパイラのプロジェクトのコピーを更新します。

**これは ASP.NET Framework 4.7.1 に適用されますまたはプロジェクトが後でのみ、ASP.NET Core には適用されません。**
