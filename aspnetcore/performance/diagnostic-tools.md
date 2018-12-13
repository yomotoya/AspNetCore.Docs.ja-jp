---
title: パフォーマンス診断ツール
author: mjrousos
description: ASP.NET Core アプリのパフォーマンスの問題を診断するための便利なツールです。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329201"
---
# <a name="performance-diagnostic-tools"></a>パフォーマンスの診断ツール

によって[Mike Rousos](https://github.com/mjrousos)

この記事では、ASP.NET Core でのパフォーマンスの問題を診断するためのツールを紹介します。

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio 診断ツール

[プロファイリングと診断ツール](/visualstudio/profiling)パフォーマンスの問題の調査を開始点としては、Visual Studio に組み込まれています。 これらのツールとは、強力で、Visual Studio 開発環境から使用すると便利です。 ツールは、ASP.NET Core アプリでの CPU 使用率、メモリ使用量とパフォーマンス イベントの分析を使用できます。

詳細については[Visual Studio ドキュメント](/visualstudio/profiling/profiling-overview)します。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview)アプリの詳細なパフォーマンス データを提供します。 Application Insights は、自動的に反応率、エラー率、依存関係の応答時間に関するデータを収集します。 Application Insights では、アプリにカスタム イベントとメトリックが特定のログ記録をサポートします。

Application Insights は、さまざまな環境で使用できます。

* Azure で動作するように最適化します。
* 運用、開発、およびステージング環境で機能します。
* ローカルで動作[Visual Studio](/azure/application-insights/app-insights-visual-studio)または他のホスト環境にします。

詳細については、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」を参照してください。

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview)は .NET のパフォーマンスの問題を診断するためには、具体的には、.NET チームによって作成されたパフォーマンス分析ツールです。 PerfView は、CPU 使用率、メモリおよび GC の動作、パフォーマンス イベント、およびウォール クロック時間の分析を使用できます。

PerfView および開始する方法の詳細については、 [PerfView のチュートリアル ビデオ](http://channel9.msdn.com/Series/PerfView-Tutorial)またはツールで使用可能なユーザー ガイドを参照してまたは[github](https://github.com/Microsoft/perfview)します。

## <a name="perfcollect"></a>PerfCollect

PerfView が .NET シナリオ用の便利なパフォーマンス分析ツールの中でしか実行できません Windows のため、これを使用して、Linux 環境で実行されている ASP.NET Core アプリからトレースを収集することはできません。

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)ネイティブ Linux のプロファイリング ツールを使用する bash スクリプトは、([Perf](https://perf.wiki.kernel.org/index.php/Main_Page)と[LTTng](https://lttng.org/)) PerfView で分析できる Linux 上のトレースを収集します。 PerfCollect は、PerfView を直接使用することはできません、Linux 環境でパフォーマンスの問題を表示する場合に便利です。 代わりに、PerfCollect すると、PerfView を使用して Windows コンピューター上で、分析し、.NET Core アプリからトレースを収集できます。

インストールして PerfCollect の使用方法の詳細については、使用可能な[github](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)します。
