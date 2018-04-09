---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: 3.2 ページを ASP.NET Web の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2 の新機能
====================
によって[Microsoft](https://github.com/microsoft)

このトピックでは、ASP.NET Web Pages 3.2 の場合、Web ページ 3.2.2 新機能について説明し、 [Web ページ 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

このリリースでは、バグを修正し、1 つの新しい機能を紹介します。

## <a name="download"></a>ダウンロード

ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。 次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。 ASP.NET Web Pages 3.2 パッケージは、次のバージョン: &ldquo;3.2.0&rdquo;です。 インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/)です。 リリースには、NuGet で対応するローカライズ版パッケージも含まれています。

インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>新しい機能とバグ修正

1 つのバグを修正し、このリリースでは 1 つの補助機能強化が行われたおいたします。 同じクエリを検出した[ここ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)です。

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

このリリースにロールアップ変更、 [ASP.NET Web Pages 3.2.1 ベータ リリース](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)大きな razor ページのレンダリングに大幅なパフォーマンス向上を提供します。 参照してください[Codeplex 問題 585](https://aspnetwebstack.codeplex.com/workitem/585)です。 このリリースは、MVC 5.2.2 に揃えて配置パッケージは、今すぐこのバージョンに依存します。

MSN チームに取り組んで大きなページを表示します。 場合のページは、80 キロバイトを超えるデータをレンダリングお最終的に大きなオブジェクト ヒープのオブジェクトとします。 レイアウトの複数のレイヤーを使用する場合は、この特殊効果を乗算することができます。

サーバーの結果は余分な CPU 使用率、メモリ、およびでも長期保持期間を長くを一時停止中に[Gen 2 クリーンアップ](https://msdn.microsoft.com/en-us/library/ms973837.aspx)ガベージ コレクターでします。

分析の結果を示す表を次に示します、 [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial)実行します。 大規模なページが表示されないときに、CPU が約 68% 定数が保持されます。 表は、ジェネレーション 2 のコレクションの数がほぼ完全に削除されると、その結果として要求率が高く、ガベージ コレクションのため一時停止を削減する非常に長いことを示します。

| **区分** | **(3.2) before** | **(3.2.1) した後** | **デルタ %** |
| --- | --- | --- | --- |
| 合計要求 (数) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| トレースの実行間隔 (秒) | 196.20 | 198.60 | 1.20% |
| 要求/秒 | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU の負荷 | 68.80% | 68.50% |  -0.40% |
| GC CPU サンプル | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| 割り当ての合計 (数) | 55,357,146 | 57,222,949 | 3.40% |
| 合計の GC 一時停止 (サンプル) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (数) | 403 | 1,216 | 201.70% |
| Gen1 GC (数) | 290 | 367 | 26.60% |
| Gen2 GC (数) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU (サンプル/要求数) を要求/ | 19.73 | 16.47 | -16.50% |

| 色の設定。 | <font style="background-color: #00ff00">コアの向上</font> | <font style="background-color: #4bacc6">パフォーマンスへ悪影響が正の値</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

このリリースには、バグの修正プログラムのみが含まれています。 使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)今回のリリースで修正された問題の一覧を表示します。
