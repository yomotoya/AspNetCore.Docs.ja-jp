---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: "分散キャッシュ (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント"
author: MikeWasson
description: "Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 24ede9cb9289c84140f6e2573f9d526f19cac64b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>分散キャッシュ (実際のクラウド アプリのビルドと Azure)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


前のチャプターは一時的なエラー処理を調べるし、遮断器戦略としてキャッシュを説明します。 この章では、キャッシュ、に関する背景情報が、これを使用するための一般的なパターンを使用する場合など、Azure に実装する方法です。

## <a name="what-is-distributed-caching"></a>分散キャッシュとは

キャッシュは、メモリ内のデータを格納することにより高いスループット、低待機時間のアクセス、よくアクセスされるアプリケーションのデータを提供します。 クラウド アプリケーション キャッシュの最も便利な型は分散キャッシュ、つまり他のクラウド リソースは、個々 の web サーバーのメモリでは、データは格納されませんし、キャッシュされたデータは使用できるすべてのアプリケーションの web サーバー (またはその他のクラウド Vm その ar電子メール アプリケーションで使用)。

![同じキャッシュ サーバーにアクセスする複数の web サーバーを示す図](distributed-caching/_static/image1.png)

アプリケーションのスケーラビリティが追加またはサーバーを削除してアップグレードまたはエラーのためサーバーが置き換えられますときや、キャッシュされたデータがアプリケーションを実行するすべてのサーバーにアクセスできる状態にします。

永続的なデータ ストアの待機時間の長いデータ アクセスを回避するには、キャッシュが大幅に向上させることがアプリケーションの応答性。 たとえば、キャッシュからデータを取得することは、リレーショナル データベースから取得するよりもはるかに高速です。

キャッシュする側な利点が減少 egress データがある場合にコストの削減になる可能性永続データ ストアへのトラフィックが、永続データ ストアに対して請求します。

## <a name="when-to-use-distributed-caching"></a>分散キャッシュを使用する場合

キャッシュの動作が、データの書き込みよりも多くの読み取りを実行して、データ モデルが格納およびキャッシュ内のデータの取得に使用するキー/値組織をサポートしているときに、アプリケーションのワークロードに最適です。 アプリケーションのユーザーが多数の一般的なデータを共有する場合にも役に立つたとえば、キャッシュことはできません。 多くの利点場合は、各ユーザーは、通常そのユーザーに固有のデータを取得します。 キャッシュでしたが、非常に効果的である例は、製品カタログ、データが頻繁に変更しないと、すべてのお客様は、同じデータを見るがあるためです。

キャッシュの利点は、なりますますます測定可能なほど、アプリケーションのスケーラビリティ、スループットの制限と、永続データ ストアの待機時間の遅延になったときにアプリケーション全体のパフォーマンス上の制限の詳細。 ただし、パフォーマンスも以外の理由でキャッシュを実装する場合があります。 キャッシュへのアクセスは、データをユーザーに表示されると完全に最新の状態にする必要はありません、永続的なデータ ストアは、応答しない状態または利用できないときに、遮断器のとして使用できます。

## <a name="popular-cache-population-strategies"></a>一般的なキャッシュ カタログの作成方法

キャッシュからデータを取得できるようにするには、するために保存することがありますまずがあります。 これには、キャッシュに必要なデータを取得するためのいくつかの方法があります。

- 必要に応じてキャッシュ アサイド/

    キャッシュからデータを取得しようとするアプリケーションとキャッシュには、データ ("miss") が割り当てられていない、ときに、アプリケーション データはキャッシュに格納、それが使用できるように、次回です。 次回アプリケーションが、同じデータを取得しようとしています。 何を探しています (「ヒット」)、キャッシュ内を検索します。 データベースで変更されたキャッシュされたデータのフェッチを防ぐためには、データ ストアを変更する場合、キャッシュを無効です。
- バック グラウンド データ プッシュ

    バック グラウンド サービスからデータをプッシュをキャッシュに定期的なスケジュールと、アプリで常にプル キャッシュします。 このアプローチはすばらしいするを常に必要としない待機時間の長いデータ ソースでは、最新のデータを返します。
- 遮断器

    アプリケーションは、通常、永続データ ストアと直接通信が、永続的なデータ ストアに可用性の問題がある場合は、アプリケーションがキャッシュからデータを取得します。 データは、キャッシュ アサイドまたはバック グラウンド データ プッシュの戦略を使用してキャッシュに格納された可能性があります。 これは、パフォーマンスの向上戦略ではなく、戦略の処理エラーです。

現在キャッシュ内のデータを保持、するために、アプリケーションを作成、更新、またはデータの削除時に関連するキャッシュ エントリを削除できます。 である場合も、アプリケーションが若干古いデータが表示されることができるデータはどれだけ古いキャッシュ制限を設定すると、構成可能な有効期限時間に依存することができます。

絶対有効期限 (キャッシュ項目が作成されてからの時間の長さ) またはスライディング有効期限 (前回のキャッシュ項目にアクセスした時間の長さ) を構成することができます。 絶対有効期限は、データが古くなることを防止するキャッシュの有効期限メカニズムに依存する場合に使用されます。 修正アプリでは、古くなったキャッシュ アイテムを手動で削除をおし、スライド式有効期限を使用して、キャッシュ内の最新のデータを保持します。 選択した有効期限ポリシーに関係なく、キャッシュは自動的に、最も古い最も最近使用した (LRU) の項目を削除、キャッシュのメモリ制限に達したときにします。

## <a name="sample-cache-aside-code-for-fix-it-app"></a>キャッシュ アサイドのサンプル コードを修正アプリ

次のサンプル コードで確認キャッシュ最初の修正にタスクを取得するときにします。 タスクがキャッシュ内で見つかった場合返します。存在しない場合、データベースから取得し、キャッシュに格納します。 変更すると、キャッシュを追加する、`FindTaskByIdAsync`メソッドが強調表示されます。

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

更新または修正、タスクを削除すると (削除)、キャッシュされたタスクを無効にする必要があります。 それ以外の場合、今後は、そのタスクは引き続きキャッシュから古いデータに読み取ろうとします。

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

これらは、単純なキャッシュ コードを説明するためにサンプルプロジェクトの修正、ダウンロード可能なキャッシュが実装されていません。

## <a name="azure-caching-services"></a>Azure のキャッシュ サービス

Azure は、次のキャッシュ サービスを提供しています: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx)と[Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx)です。 Azure Redis cache は、人気の高いに基づいて[オープン ソース Redis Cache](http://redis.io/)とほとんどの最初の選択肢がシナリオをキャッシュします。

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET セッション状態のキャッシュ プロバイダーを使用します。

説明したように、 [web 開発のベスト プラクティスの章](web-development-best-practices.md)、セッション状態を使用しないようにお勧めします。 アプリケーションは、セッション状態を必要とする場合、次のベスト プラクティスをスケール アウト (web サーバーの複数のインスタンス) を有効にしないために、既定のメモリ内のプロバイダーを回避するのにです。 SQL Server の ASP.NET セッション状態プロバイダーにより、セッション状態を使用する複数の web サーバーで実行されるサイトが、メモリ内のプロバイダーと比較して待機時間の長いコストが発生します。 セッション状態を使用する必要がある場合、最適なソリューションがなど、キャッシュ プロバイダーを使用するには、 [Azure Cache のセッション状態プロバイダー](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx)です。

## <a name="summary"></a>まとめ

どのように修正アプリが応答時間とスケーラビリティを向上するために、アプリケーション データベースが利用できない場合に読み取り操作の応答して続行するためのキャッシュを実装する可能性がありますを見てきました。 [次のチャプター](queue-centric-work-pattern.md)はさらに、スケーラビリティが向上し、書き込み操作の応答性を引き続きアプリを作成する方法を説明します。

## <a name="resources"></a>リソース

キャッシュの詳細については、次のリソースを参照してください。

ドキュメント

- [Azure Cache](https://msdn.microsoft.com/library/gg278356.aspx)です。 Azure でのキャッシュに関する公式の MSDN ドキュメントです。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。 キャッシュのガイダンスとキャッシュア サイド パターンを参照してください。
- [フェール セーフ: 回復力のあるクラウド アーキテクチャについて](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)です。 Marc Mercuri、Ulrich Homann、Andrew Townhill、ホワイト ペーパー。 キャッシュのセクションを参照してください。
- [Azure クラウド サービスで大規模なサービスのデザインに関するヒント集](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)です。 W. Mark Simms、Michael Thomassy、ホワイト ペーパー。 分散キャッシュ セクションを参照してください。
- [分散スケーラビリティへのパスでキャッシュ](https://msdn.microsoft.com/magazine/dd942840.aspx)です。 古い (2009 年) の MSDN マガジンの記事、一般的な分散キャッシュを明確に記述の概要フェール セーフとベスト プラクティスのホワイト ペーパーのキャッシュ セクションより詳細に移動します。

ビデオ

- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、系列を 9 つの部分で構成します。 クラウド アプリを設計する方法の 400 レベルのビューを表示します。 この系列が理論に焦点を当てており、理由です。操作方法に関する詳細については、Mark Simms で構築大規模な系列を参照してください。 1時 24分: 14 始まるエピソード 3 のキャッシュの説明を参照してください。
- [構築 Big: Azure のお客様 - パート I から学んだ教訓](https://channel9.msdn.com/Events/Build/2012/3-029)です。Simon Davies は、分散キャッシュを開始位置として 46:00 について説明します。 フェール セーフ系列の詳細の操作方法に関する詳細情報になるに似ています。 プレゼンテーションは、2013年で導入された Azure App service Web Apps のキャッシュ サービスのについては説明しませんので、2012 年 10 月 31 日が指定されました。

コード サンプル

- [クラウド サービスの基礎 azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)です。 分散キャッシュを実装するサンプル アプリケーション。 付随するブログの投稿を参照してください[クラウド サービスの基礎: キャッシュの基礎](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx)です。

>[!div class="step-by-step"]
[前へ](transient-fault-handling.md)
[次へ](queue-centric-work-pattern.md)
