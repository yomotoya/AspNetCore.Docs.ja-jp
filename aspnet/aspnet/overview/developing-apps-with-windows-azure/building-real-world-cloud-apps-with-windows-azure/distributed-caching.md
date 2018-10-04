---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: 分散キャッシュ (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 26866ef9d13a198aab627ccf0f5e1ff3d2304427
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577549"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>分散キャッシュ (実際のクラウド アプリの構築と Azure)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


前の章では、一時的な障害処理について説明しましたし、サーキット ブレーカー戦略としてキャッシュを説明します。 この章では、キャッシュ、さらに詳しく知り、それを使用するための一般的なパターンを使用するタイミングなど、Azure での実装方法。

## <a name="what-is-distributed-caching"></a>分散キャッシュとは

キャッシュは、メモリ内データを格納する高スループットと、よくアクセスされるアプリケーションのデータへの待機時間の短いアクセスを提供します。 キャッシュの最も便利な型、つまり、個々 の web サーバーのメモリが他のクラウド リソースにデータが保存されないと、すべてのアプリケーションの web サーバーに利用可能になって、キャッシュされたデータ分散キャッシュは、クラウド アプリの (またはその他のクラウドの Vm、ar電子メール アプリケーションで使用)。

![複数の web サーバーが同じキャッシュ サーバーへのアクセスを示す図](distributed-caching/_static/image1.png)

アプリケーションのスケールを追加または削除、サーバーまたはサーバーがアップグレードまたはエラーのために置き換えられます場合、キャッシュされたデータはアプリケーションを実行するすべてのサーバーにアクセス可能なままです。

アプリケーションの応答性を向上させるには、永続的なデータ ストアの待機時間の長いデータ アクセスを回避するには、キャッシュが大幅にします。 たとえば、キャッシュからデータを取得するリレーショナル データベースから取得するより高速です。

キャッシュのメリットが減少永続的なデータ ストアの送信データ転送がある場合にコストの削減になる可能性永続的なデータ ストアへのトラフィックが課金されます。

## <a name="when-to-use-distributed-caching"></a>分散キャッシュを使用する場合

キャッシュのしくみ、データの書き込みよりも多くの読み取りを実行して、データ モデルがキャッシュ内のデータ格納および取得に使用するキー/値組織がどのようにサポートする場合、アプリケーション ワークロードに最適です。 アプリケーションのユーザーが、多くの一般的なデータは; を共有する場合にもさらに便利ですたとえば、キャッシュことはできません。 として多くのメリットが各ユーザーは通常、そのユーザーに固有のデータを取得する場合。 キャッシュでしたが、非常に効果的である例では、データが頻繁に変更しないと、すべてのお客様は、同じデータを見ているため、製品カタログです。

キャッシュの利点がますます測定可能なほど、アプリケーションのスケーラビリティ、永続的なデータ ストアの待機時間の遅延とスループットの制限がアプリケーション全体のパフォーマンス上の制限の詳細。 ただし、パフォーマンスも以外の理由でキャッシュを実装する場合があります。 データをユーザーに表示するときに完全に最新の状態にする必要はありません、キャッシュへのアクセスは、永続的なデータ ストアが応答していないか使用できない場合のサーキット ブレーカーとして使用できます。

## <a name="popular-cache-population-strategies"></a>人気のあるキャッシュの作成方法

キャッシュからデータを取得できるようにするには、するために格納することがあります最初があります。 これには、キャッシュに必要なデータを取得するためのいくつかの方法があります。

- 必要に応じてキャッシュ アサイド/

    アプリケーションがキャッシュからデータを取得しようし、れるように、使用可能な次回、アプリケーションがキャッシュでデータを格納、キャッシュにデータ ("miss") がない場合。 アプリケーションが、同じデータを取得しようとしています。 次に何を探しています (「ヒット」)、キャッシュ内を検索します。 データベースのキャッシュされたデータが変更されたことをフェッチを防ぐためには、データ ストアを変更する場合、キャッシュを無効です。
- バック グラウンド データ プッシュ

    バック グラウンド サービスからデータをプッシュをキャッシュに定期的なスケジュールと、アプリで常にプル キャッシュします。 このアプローチの works すばらしいことを常に必要としない高待機時間データ ソースの最新のデータを返します。
- サーキット ブレーカー

    アプリケーションは、通常、永続的なデータ ストアと直接通信が、永続的なデータ ストアに、可用性の問題がある場合は、アプリケーションがキャッシュからデータを取得します。 データは、キャッシュを確保またはバック グラウンド データ プッシュの戦略を使用してキャッシュに格納されている可能性があります。 これは、パフォーマンスの強化戦略ではなく、戦略を処理するエラーです。

キャッシュにデータを最新に保つするには、アプリケーションの作成、更新、またはデータを削除するときの関連するキャッシュ エントリを削除できます。 アプリケーションの場合もありますがわずかに古いデータの取得にいい場合、データは、古いキャッシュの制限を設定する構成可能な有効期限の時間を利用できます。

絶対有効期限 (キャッシュ項目の作成からの時間) またはスライディング有効期限 (前回のキャッシュ項目にアクセスした時間の長さ) を構成することができます。 絶対有効期限は、データが古くなりすぎないようにするキャッシュの有効期限のメカニズムに依存するときに使用されます。 Fix It アプリで古いキャッシュ項目を手動で削除し、キャッシュ内の最新のデータを保持するスライド式有効期限を使用します。 選択した有効期限ポリシーに関係なく、キャッシュは自動的に、最も古い (最も最近の使用または LRU) の項目を削除、キャッシュのメモリ制限に達したとき。

## <a name="sample-cache-aside-code-for-fix-it-app"></a>アプリ Fix It サンプル キャッシュ アサイド コード

次のサンプル コードで確認キャッシュ最初 Fix It タスクを取得するときにします。 それを返すタスクがキャッシュで見つかった場合見つからない場合は、データベースから取得し、キャッシュに格納します。 キャッシュを追加することは、変更、`FindTaskByIdAsync`メソッドが強調表示されます。

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

更新または Fix It タスクを削除すると、(削除)、キャッシュされたタスクを無効にする必要があります。 それ以外の場合、今後は、キャッシュから古いデータを取得するタスクを引き続きの読み取りを試みます。

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

これらは、単純なキャッシュのコードを説明するためのサンプルダウンロード可能な Fix It プロジェクトのキャッシュが実装されていません。

## <a name="azure-caching-services"></a>Azure のキャッシュ サービス

Azure は、次のキャッシュ サービスを提供しています: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx)と[Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx)します。 Azure Redis cache は広く普及しているに基づいている[オープン ソース Redis Cache](http://redis.io/)とほとんどの最初の選択肢は、シナリオをキャッシュします。

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET のセッション状態キャッシュ プロバイダーを使用します。

説明したように、 [web 開発のベスト プラクティスの章](web-development-best-practices.md)、セッション状態の使用を回避するためにはお勧めします。 アプリケーションは、セッション状態を必要とする場合、次のベスト プラクティスはスケール アウト (web サーバーの複数のインスタンス) は有効にするために、既定のメモリ内プロバイダーを回避するためには。 SQL Server の ASP.NET セッション状態プロバイダーにより、セッションの状態を使用する複数の web サーバーで実行されているサイトが、メモリ内プロバイダーと比較して高待機時間コストが発生します。 セッション状態を使用した場合に最適なソリューションがなど、キャッシュ プロバイダーを使用するには、 [Azure Cache のセッション状態プロバイダー](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx)します。

## <a name="summary"></a>まとめ

Fix It アプリを実装に引き続き、データベースが利用できない場合に、読み取り操作の応答性の高いするアプリを有効にして応答時間とスケーラビリティを向上させるためにキャッシュする方法を説明しました。 [[次へ] の章](queue-centric-work-pattern.md)さらに、スケーラビリティが向上し、引き続き、書き込み操作の応答性の高いアプリを作成する方法を紹介します。

## <a name="resources"></a>リソース

キャッシュの詳細については、次のリソースを参照してください。

ドキュメント

- [Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx)します。 Azure でのキャッシュに関する MSDN の公式ドキュメント。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。 キャッシュのガイダンスとキャッシュ アサイド パターンを参照してください。
- [フェール セーフ: 回復力のあるクラウド アーキテクチャについて](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)します。 Marc Mercuri、Ulrich Homann、Andrew Townhill してホワイト ペーパー。 キャッシュのセクションを参照してください。
- [Azure Cloud Services で大規模なサービスを設計するためのベスト プラクティス](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)します。 W. Mark Simms、Michael Thomassy、ホワイト ペーパー。 分散キャッシュのセクションを参照してください。
- [分散キャッシュ スケーラビリティへのパスを利用](https://msdn.microsoft.com/magazine/dd942840.aspx)します。 古い (2009 年) の MSDN Magazine の記事、一般的な分散キャッシュをわかりやすく概要フェール セーフとベスト プラクティスのホワイト ペーパーのキャッシュのセクションより詳細に移動します。

ビデオ

- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、9 部構成シリーズ。 クラウド アプリを設計する方法を 400 番台のビューを表示します。 このシリーズの理論に重点を置いています、理由です。操作方法に関する詳細については、Mark Simms によって構築ビッグ シリーズを参照してください。 1時 24分: 14 でエピソード 3 の開始時にキャッシュの説明を参照してください。
- [ビルドの大きな: Azure のお客様 - パート I から学んだ教訓](https://channel9.msdn.com/Events/Build/2012/3-029)します。Simon Davies は、分散キャッシュを開始位置として 46:00 について説明します。 フェール セーフ シリーズが操作方法の詳細になると似ています。 プレゼンテーションは、2013年で導入された Azure App Service で Web アプリのキャッシュ サービスのについては説明しませんので、2012 年 10 月 31 日が指定されました。

コード サンプル

- [クラウド サービス Azure の基礎](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)します。 分散キャッシュを実装するサンプル アプリケーション。 付随するブログの投稿を参照してください。[クラウド サービスの基礎: 基本のキャッシュ](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx)します。

> [!div class="step-by-step"]
> [前へ](transient-fault-handling.md)
> [次へ](queue-centric-work-pattern.md)
