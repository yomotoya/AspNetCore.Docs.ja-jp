---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web 開発のベスト プラクティス (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 4c43b256018d91e89b3427f90fc5c6cd018641f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873522"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web 開発のベスト プラクティス (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


最初の 3 つのパターンが、アジャイル開発プロセス; の設定に関するされました残りの部分は、アーキテクチャ、およびコードについてはします。 これは web 開発のベスト プラクティスのコレクションです。

- [ステートレス web サーバー](#stateless)スマート ロード バランサーの背後にします。
- [セッション状態を回避する](#sessionstate)(またはこれを回避できない場合は、データベースではなく、分散キャッシュを使用して)。
- [CDN を使用して](#cdn)エッジ キャッシュ静的ファイル資産 (イメージ、スクリプト) にします。
- [.NET 4.5 の非同期サポートを使用して](#async)呼び出しがブロックされないようにします。

これらのプラクティスは有効でクラウド アプリだけでなく、すべての web 開発いるクラウド アプリにとって特に重要です。 それらが連携して、クラウド環境で提供される非常に柔軟なスケーリングの使用を最適化するためです。 これらのプラクティスに従わない場合は、アプリケーションがスケール アップするときの制限事項に実行します。

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>スマート ロード バランサーの背後のステートレス web 層

*ステートレス web 層*web サーバーのメモリまたはファイル システムで任意のアプリケーション データを保存しないことを意味します。 ステートレスな web 層を維持すると、カスタマー エクスペリエンスの向上とコストの節約の両方を実行できます。

- 場合は、web 層はステートレスなロード バランサーの背後に、すばやくアプリケーション トラフィックにおいて変更に応答して、動的に追加またはサーバーを削除することができます。 場所だけ支払えば用のサーバー リソースの実際に使用する限り、クラウド環境では、需要の変化に対応する機能は大幅な節約に翻訳できます。
- ステートレス web 層アーキテクチャ非常に簡単です、アプリケーションのスケール アウトします。 使用すると、ニーズをより迅速にスケーリングに応答し、開発、およびプロセスでテストに低コストを費やすすぎるできます。
- 内部設置型サーバーと同様に、クラウドのサーバーは、修正プログラムを適用し、場合によっては; を再起動する必要があります。web 層がステートレスの場合は、サーバーが一時的にダウンした場合は、トラフィックを再ルーティングは発生しませんエラーや予期しない動作します。

ほとんどの実際のアプリケーションは web セッションの状態を保存する必要があります。ここでの主なポイントでは、web サーバーに保存されません。 など、他の方法で状態を保存するにはキャッシュ プロバイダーを使用して ASP.NET セッション状態のプロセス サーバー側の内外の cookie でクライアントにします。 内のファイルを格納する[Windows Azure Blob ストレージ](unstructured-blob-storage.md)ローカル ファイル システムの代わりにします。

として、web 層がステートレスの場合は、Windows Azure Web サイトでアプリケーションの拡張が容易な方法の例は、次を参照してください。、**スケール** タブに、Windows Azure Web サイトの管理ポータルで。

![[スケール] タブ](web-development-best-practices/_static/image1.png)

Web サーバーを追加する場合は、右側にだけ、インスタンス数のスライダーをドラッグできます。 5 に設定し、をクリックして**保存**、され、秒以内 5 の web サーバーで Windows Azure の web サイトのトラフィックを処理します。

![5 つのインスタンス](web-development-best-practices/_static/image2.png)

インスタンスの数を 3 または 1 まで戻します同じくらい簡単に設定できます。 コストの削減を開始するバックアップをスケールする場合、1 時間ではなく、Windows Azure が、分単位で請求ためにすぐにします。

Windows Azure に自動的に CPU 使用率に基づいて、web サーバーの数を増減する見分けることができます。 次の例では、CPU 使用率が 60% 未満になるときに web サーバーの数を低下し、は、2 を最小限に最大 4 の web サーバーの数が増えたり CPU 使用率が 80% を超えた場合。

![CPU 使用率の拡大縮小します。](web-development-best-practices/_static/image3.png)

または、what わかっている場合は、サイトがだけになるビジー状態で稼働時間中にしますか。 Windows Azure は日中に複数のサーバーを実行し、evenings、夜間に、1 台のサーバーと週末に減らすことがわかります。 次の一連のスクリーン ショットは、午前 8 時から午後 5 時までの作業時間中に業務時間外に 1 つのサーバーと 4 つのサーバーを実行するために、web サイトをセットアップする方法を示します。

![スケジュールの拡大縮小します。](web-development-best-practices/_static/image4.png)

![スケジュールされた時刻を設定します。](web-development-best-practices/_static/image5.png)

![Daytime スケジュール](web-development-best-practices/_static/image6.png)

![Weeknight スケジュール](web-development-best-practices/_static/image7.png)

![週末のスケジュール](web-development-best-practices/_static/image8.png)

すべてを実行して、ポータルと同様に同様のスクリプトでもちろんです。

障害を動的に追加または web 層の状態を保持することでサーバーの Vm を削除しない限り、スケール アウトするため、アプリケーションの機能は、Windows Azure でほぼ制限ではありません。

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>セッション状態を回避します。

ユーザー セッションは、何らかの形式の状態を格納しないようにする実際のクラウド アプリでは実用的ではない多くの場合が、パフォーマンスとスケーラビリティを他のものより詳細に影響を与えるいくつかの方法です。 状態を保存した場合は、状態の量を少なく抑え、cookie に保存することをお勧めします。 [次へ] の最適なソリューションが ASP.NET セッション状態プロバイダーを使用するのにはできない場合、[分散型インメモリ キャッシュ](distributed-caching.md#sessionstate)です。 パフォーマンスとスケーラビリティの観点から最悪の解決策は、データベースを使用するセッション状態プロバイダーをバックアップします。

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>CDN をキャッシュを使用する静的ファイル資産

CDN は、コンテンツ配信ネットワークの頭字語です。 CDN プロバイダーに画像やスクリプト ファイルなどの静的ファイル アセットを指定して、取得できるように、アプリケーションをユーザーにアクセスする場所には比較的迅速な応答と低待機時間、キャッシュされたは、プロバイダーは、世界各地のデータ センターでこれらのファイルをキャッシュ資産にします。 これは、サイトの全体的な読み込み時間が高速化し、web サーバーの負荷を軽減します。 Cdn は、地理的に広く配布される対象ユーザーにアクセスしている場合に特に重要です。

Windows Azure は、CDN を持ち、Windows Azure や、すべての web ホスティング環境で実行されているアプリケーションで他の Cdn を使用することができます。

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>.NET 4.5 の非同期サポートを使用して呼び出しがブロックされないように

.NET 4.5 が簡単にそのタスクを非同期的に処理するために、c# および VB のプログラミング言語が強化されています。 非同期プログラミングの利点は、同時に複数の web サービス呼び出しを開始するときなどの並列処理の状況のだけではなくです。 Web サーバーをより効率的に実行することもでき、高負荷条件下で信頼性の高い。 Web サーバーのみ、使用可能なスレッド数を限定あり高負荷条件下で、使用中のすべてのスレッドが受信要求を持っているスレッドが解放するまで待機します。 アプリケーション コードでは、クエリと web サービス呼び出しを非同期的にデータベースと同じようにタスクを処理しない場合スレッドの数が不必要に使用されているサーバーが I/O 応答の待機中です。 これにより、高負荷条件下で、サーバーで処理できるトラフィック量が制限されます。 非同期プログラミングでは、web サービスまたはデータを返すデータベースを待機しているスレッドは、データまで解放新しい要求を処理までは、受信します。 ビジー状態の web サーバーは、数百または数千の要求処理できます速やかを解放するスレッドを待機してそれ以外の場合のようです。

既に説明したとおり、それらを増やすには、web サイトの処理の web サーバーの数を削減するくらい簡単です。 その場合は、サーバーには、高いスループットを実現できます、必要はありません、コストを削減してそれらの多くには、それ以外の場合よりも少ないサーバーの特定のトラフィック量が必要なためです。

.NET 4.5 の非同期プログラミング モデルのサポートは、Web フォーム、MVC、および Web API の ASP.NET 4.5 に含まれるEntity Framework 6、および、 [Windows Azure ストレージ API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)です。

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 での非同期のサポート

ASP.NET 4.5 で言語に対してだけでなく、MVC、Web フォーム、および Web API フレームワークにも追加された非同期プログラミングのサポートします。 たとえば、ASP.NET MVC コント ローラー アクション メソッドは、web 要求からデータを受信し、ブラウザーに送信するための HTML を作成するビューにデータを渡します。 多くの場合、アクション メソッドは、web ページに入力されたデータを保存するか、web ページに表示するために、データベースや web サービスからデータを取得する必要があります。 そのようなシナリオでは簡単に非同期アクション メソッドを作成: を返す代わりに、 *ActionResult*オブジェクトを返す*タスク&lt;ActionResult&gt;* とメソッドをマーク*async*キーワード。 メソッドの内部場合待機時間に関連する操作が開始されるコードの行をマークする、await キーワードを使用します。

リポジトリのメソッドを呼び出す、データベース クエリの単純なアクション メソッドを次に示します。

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

データベースの呼び出しを非同期的に処理する同じメソッドを次に示します。

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

内部では、コンパイラは、適切な非同期コードを生成します。 アプリケーションが呼び出しを行うときに`FindTaskByIdAsync`、asp.net、`FindTask`要求、ワーカー スレッドをアンワインドし、別の要求を処理できるようになります。 ときに、`FindTask`要求を実行するには、その呼び出しの後に続くコードの処理を続行するスレッドを再起動します。 まで暫定的に、`FindTask`要求が開始されがそれ以外の場合、応答の待機を結び付けるは有効な作業を行うには使用可能なスレッドがあるデータが返されるとします。

非同期コードのオーバーヘッドが、低負荷条件下でオーバーヘッドがごくわずかであり、中にそれ以外の場合、使用可能なスレッドの待機を保持は要求を処理できる高負荷条件下で。

この種の非同期プログラミング ASP.NET 1.1 以降を実行しているが、作成が困難に、エラーが生じやすい、およびデバッグが困難でした。 これでは簡略化して、ASP.NET 4.5 でのコーディング、もはやしない理由はありません。

### <a name="async-support-in-entity-framework-6"></a>Entity Framework 6 での非同期のサポート

4.5 での非同期のサポートの一部としてリリース時に web サービスの呼び出し、ソケット、およびファイル システム I/O、非同期のサポートが、web アプリケーションの最も一般的なパターンは、データベースにヒットして、データのライブラリは、非同期をサポートしていませんでした。 これで Entity Framework 6 は、データベース アクセスのための非同期のサポートを追加します。

Entity Framework 6 では、クエリやデータベースに送信されるコマンドが発生するすべてのメソッドは、非同期バージョンを保持します。 次の例では、非同期バージョンを示しています、*検索*メソッドです。

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

LINQ クエリのでも、この非同期のサポートが挿入、削除、更新、および単純な検索のだけでなく動作します。

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

`Async`のバージョン、`ToList`メソッド、データベースに送信されるクエリの原因となるメソッドでこのコードであるためです。 `Where`と`OrderByDescending`メソッドのみ、クエリを構成中に、`ToListAsync`メソッドが、クエリを実行しで応答を格納、`result`変数。

## <a name="summary"></a>まとめ

すべての web プログラミング フレームワークと任意のクラウド環境では、こちらの web 開発のベスト プラクティスを実装することができますを簡単には、ASP.NET と Windows Azure のツールがあります。 これらのパターンに従うと場合、ことが簡単にスケール アウトする、web 層と各サーバーはより多くのトラフィックを処理できるため、費を最小限に抑えるします。

[次のチャプター](single-sign-on.md)クラウドがシングル サインオン シナリオを使用する方法を見ます。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

ステートレス web サーバー:

- [Microsoft Patterns and Practices - 自動スケール ガイダンス](https://msdn.microsoft.com/library/dn589774.aspx)です。
- [無効化の ARR のインスタンスの Windows Azure Web サイトのアフィニティ](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)です。 Erez Benari でブログの投稿では、セッション アフィニティを Windows Azure Web サイトについて説明します。

CDN:

- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 つの部分で構成します。 エピソード 3 が 1時 34分: 00 以降の CDN の説明を参照してください。
- [Microsoft のパターンとプラクティス静的コンテンツをホストしているパターン](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN レビュー](http://www.cdnreviews.com/)です。 多くの Cdn の概要です。

非同期のプログラミング:

- [ASP.NET MVC 4 で非同期メソッドを使用して](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)です。 Rick anderson のチュートリアルです。
- [非同期を使用した非同期プログラミングおよび Await (c# および Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)です。 非同期プログラミングの論理的根拠は、ASP.NET 4.5 での動作方法およびそれを実装するコードを記述する方法を説明する MSDN のホワイト ペーパー
- [Entity Framework 非同期クエリと保存](https://msdn.microsoft.com/data/jj819165)
- [非同期を使用して ASP.NET Web アプリケーションを構築する方法](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7)です。 Rowan Miller によるビデオ プレゼンテーションです。 グラフィックのデモを含む非同期プログラミングのことが容易にするための高負荷条件下で web サーバーのスループットを大幅に増加します。
- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 つの部分で構成します。 非同期プログラミングのスケーラビリティ上の影響に関するディスカッション、エピソード 4 とエピソード 8 を参照してください。
- [非同期のメソッドでは ASP.NET 4.5 と重大な落とし穴を使用するマジック](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx)です。 ブログの投稿 Scott Hanselman による基本的には、ASP.NET Web フォーム アプリケーションで非同期を使用します。

追加の web 開発のベスト プラクティスでは、次のリソースを参照してください。

- [修正プログラム、サンプル アプリケーションのベスト プラクティス](the-fix-it-sample-application.md#bestpractices)です。 この電子書籍の付録には、修正、アプリケーションで実装されたベスト プラクティスの数が一覧表示します。
- [Web 開発者のチェックリスト](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [前へ](continuous-integration-and-continuous-delivery.md)
> [次へ](single-sign-on.md)
