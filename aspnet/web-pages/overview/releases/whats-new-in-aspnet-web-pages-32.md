---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: 新機能については ASP.NET Web ページの 3.2 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: a55c01c1430b983fbe34654cc2c00de05e941d71
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802252"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>ASP.NET Web ページ 3.2 の新機能新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web ページ 3.2、Web ページ 3.2.2 は新機能について説明しますと[Web ページ 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web ページ 3.2

このリリースでは、バグを修正し、1 つの新しい機能を紹介します。

## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。 ASP.NET Web ページ 3.2 パッケージは、次のバージョン: &ldquo;3.2.0&rdquo;します。 インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/)します。 リリースには、NuGet での対応するローカライズされたパッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>新機能とバグ修正

1 つのバグを修正しました。 お 1 つの小さな機能の拡張機能をこのリリースで行われました。 同じクエリを検索できます[ここ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)します。

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

このリリースにロールアップ変更、 [ASP.NET Web Pages 3.2.1 ベータ リリース](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)大規模な razor ページのレンダリングで大幅なパフォーマンス向上を提供します。 参照してください[Codeplex 問題 585](https://aspnetwebstack.codeplex.com/workitem/585)します。 このリリースは、MVC 5.2.2 に揃えて配置パッケージのこのバージョンに応じて異なりますようになりました。

大きなページのレンダリングの MSN チームと連携してください。 データの 80 キロバイト以上のページがレンダリング、ときに最終的に、大きなオブジェクト ヒープのオブジェクト。 レイアウトの複数のレイヤーを使用する場合は、この効果を乗算することができます。

サーバーの結果は余分な CPU 使用率、メモリとも、もっと長い保有期間中に一時停止する[Gen 2 クリーンアップ](https://msdn.microsoft.com/en-us/library/ms973837.aspx)ガベージ コレクターでします。

分析の結果を示すテーブルを次に示します、 [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial)を実行します。 大規模なページが表示されないときに、CPU が約 68%、定数、保持されます。 要求レートが高いとガベージ コレクションのため一時停止を削減する非常に長いこと、結果が、ジェネレーション 2 のコレクションの数がほぼ完全に削除されると、テーブルを示しています。

| **区分** | **(3.2) before** | **(3.2.1) した後** | **デルタの %** |
| --- | --- | --- | --- |
| 要求の合計 (数) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| トレース時間 (秒) | 196.20 | 198.60 | 1.20% |
| 要求/秒 | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU 負荷 | 68.80% | 68.50% |  -0.40% |
| GC CPU サンプル | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| 割り当ての合計 (数) | 55,357,146 | 57,222,949 | 3.40% |
| GC の一時停止 (サンプル) の合計します。 | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (数) | 403 | 1,216 | 201.70% |
| Gen1 GC (数) | 290 | 367 | 26.60% |
| Gen2 GC (数) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU/要求 (サンプル/req) | 19.73 | 16.47 | -16.50% |

| 色の設定。 | <font style="background-color: #00ff00">Core の向上</font> | <font style="background-color: #4bacc6">パフォーマンスに良い影響</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web ページ 3.2.3 beta1

このリリースには、バグの修正プログラムのみが含まれています。 使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)をこのリリースで修正された問題の一覧を参照してください。
