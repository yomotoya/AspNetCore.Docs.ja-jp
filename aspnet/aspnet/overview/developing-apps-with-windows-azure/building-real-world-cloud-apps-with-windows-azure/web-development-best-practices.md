---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web 開発のベスト プラクティス (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: f31798032c3b690fcb6dfa8326580255f3412826
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371039"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web 開発のベスト プラクティス (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


最初の 3 つのパターンが、アジャイル開発プロセスの設定について残りの部分は、アーキテクチャとコードの詳細についてはします。 これは web 開発のベスト プラクティスのコレクションです。

- [ステートレス web サーバー](#stateless)スマート ロード バランサーの背後にします。
- [セッション状態を回避](#sessionstate)(またはこれを回避する場合は、データベースではなく、分散キャッシュを使用して)。
- [CDN を使用して、](#cdn)エッジ キャッシュの静的ファイル資産 (画像、スクリプト) にします。
- [.NET 4.5 の非同期サポートを使用して、](#async)呼び出しがブロックされないようにします。

これらのプラクティスは、クラウド アプリのだけでなく、すべての web 開発用に有効ながクラウド アプリにとって特に重要です。 これらが連携して、クラウド環境によって提供される非常に柔軟なスケーリングの使用を最適化するためです。 これらのプラクティスを順守しない場合は、アプリケーションがスケール アップするときの制限事項に実行します。

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>スマート ロード バランサーの背後にあるステートレス web 層

*ステートレス web 層*web サーバーのメモリまたはファイル システムで任意のアプリケーション データを保存しないことを意味します。 ステートレス web 層を維持する方法より優れたカスタマー エクスペリエンスを提供して、コストを削減することができます。

- Web 層はステートレス ロード バランサー背後にある場合は、すばやくアプリケーション トラフィックの変更に応答して、動的に追加またはサーバーを削除することができます。 位置するリソースに対してのみ支払いますサーバーの実際に使用する限り、クラウド環境では、需要の変化に応答するには、その機能は大幅な節約に変換できます。
- ステートレス web 層は、アプリケーションをスケール アウト アーキテクチャはるかに簡単です。 使用すると、スケールのニーズをより迅速に応答し、開発とテスト プロセスの支出コストの削減もできます。
- オンプレミスのサーバーと同様に、クラウド サーバーは、修正プログラムを適用し、場合によっては; を再起動する必要があります。web 層がステートレスの場合は、サーバーが一時的にダウンした場合、トラフィックを再ルーティングは発生しませんエラーまたは予期しない動作します。

Web セッションの状態を保存する必要はほとんどの実際のアプリケーションここでの主なポイントでは、web サーバーに保存されません。 Cookie/キャッシュ プロバイダーを使用して ASP.NET セッション状態でプロセス サーバー側のクライアントなど、他の方法で状態を保存できます。 ファイルを保存する[Windows Azure Blob storage](unstructured-blob-storage.md)ローカル ファイル システムの代わりにします。

Web 層はステートレスである場合は、Windows Azure Web サイトでアプリケーションの拡張が簡単にする方法の例として、次を参照してください。、**スケール**管理ポータルで Windows Azure Web サイトの [] タブ。

![[スケール] タブ](web-development-best-practices/_static/image1.png)

Web サーバーを追加する場合は、右側にだけインスタンス数のスライダーをドラッグできます。 5 に設定し、をクリックして**保存**、数秒で web サイトのトラフィックを処理する Windows Azure に 5 つの web サーバーがあるとします。

![5 つのインスタンス](web-development-best-practices/_static/image2.png)

同じくらい簡単に、3 または 1 に送れば、インスタンス数を設定できます。 かかるコストの削減を開始するバックをスケールするときに Windows Azure は、1 時間ではなく、分単位で課金されますので、すぐにします。

Windows Azure に自動的に CPU 使用率に基づいて web サーバーの数を増減することもわかります。 次の例では、web サーバーの数が 2 以上に低下 CPU 使用率が 60% を下回るときにし、最大 4 まで web サーバーの数は増やすことが CPU 使用率が 80% を上回った場合します。

![CPU 使用率をスケールします。](web-development-best-practices/_static/image3.png)

または、わかっている場合、サイトがあるビジー状態業務時間中でしょうか。 Windows Azure は日中に複数のサーバーを実行し、1 台のサーバーを使用している evenings、夜間、週末を減らすことがわかります。 次の一連のスクリーン ショットでは、午前 8 時から午後 5 時までの作業時間中に業務時間外での 1 つのサーバーと 4 台のサーバーを実行する web サイトを設定する方法を示します。

![スケジュールされたスケーリングします。](web-development-best-practices/_static/image4.png)

![スケジュールされた時刻を設定します。](web-development-best-practices/_static/image5.png)

![日中のスケジュール](web-development-best-practices/_static/image6.png)

![Weeknight スケジュール](web-development-best-practices/_static/image7.png)

![週末のスケジュール](web-development-best-practices/_static/image8.png)

このすべてをスクリプトでも、ポータルで実行するのです。

動的に追加またはステートレス web 層を保持することで、サーバーの Vm を削除するを妨げる障害を回避する限り、スケール アウトするアプリケーションの機能は、Windows Azure でほぼ制限ではありません。

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>セッション状態を回避します。

ユーザー セッションは、何らかの形式の状態を保存しないようにする実際のクラウド アプリで実用的ではない多くの場合が、パフォーマンスとスケーラビリティを他よりも詳細に影響を与えるいくつかの方法です。 状態を保存した場合は、状態の量を小さくと、cookie に格納することをお勧めします。 [次へ] の最適なソリューションがのプロバイダーで ASP.NET セッション状態を使用するには利用できない場合、[分散型メモリ内キャッシュ](distributed-caching.md#sessionstate)します。 パフォーマンスとスケーラビリティの観点から最悪のソリューションでは、データベースを使用するセッション状態プロバイダーをサポートします。

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>CDN を使用して、静的ファイル資産をキャッシュするには

CDN は、Content Delivery Network の略語です。 CDN プロバイダーに画像やスクリプト ファイルなどの静的ファイル アセットを指定して、キャッシュされたの比較的迅速な応答と低待機時間を取得する任意の場所のユーザーにアプリケーションにアクセスできるように、プロバイダーは、世界中のデータ センターでこれらのファイルをキャッシュ資産にします。 これは、全体の読み込み時間、サイトの高速化され、web サーバーの負荷を軽減します。 Cdn は、地理的に広く配布される対象ユーザーにアクセスしている場合に特に重要です。

Windows Azure では、CDN、および Windows Azure または任意の web ホスティング環境で実行されるアプリケーションで他の Cdn を使用することができます。

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>.NET 4.5 の非同期サポートを使用して、呼び出しがブロックされないようにするには

.NET 4.5 では、簡単にタスクを非同期的に処理するために c# および VB のプログラミング言語を拡張します。 非同期プログラミングの利点は、同時に複数の web サービス呼び出しを開始する場合などの並列処理状況のだけではなく。 またより効率的に実行するように web サーバーで高負荷条件下で信頼性の高い。 Web サーバーでは、使用できる、スレッドの数を制限のみがあり、高負荷条件下で、使用中のすべてのスレッドが着信要求スレッドが解放されるまで待機します。 アプリケーション コードでは、データベース クエリと web サービスの呼び出しを非同期になどのタスクを処理しない場合、多数のスレッドは、サーバーが I/O 応答を待機している間不必要に関連付けします。 これにより、高負荷条件下で、サーバーが処理できるトラフィックの量が制限されます。 非同期プログラミングでは、web サービスまたはデータを返すデータベースを待機しているスレッドは、データまで解放まで新しい要求の処理は、受信します。 ビジー状態の web サーバーでは、数百または何千もの要求の処理できるように速やかを解放するスレッドを待機してそれ以外の場合のようです。

既に説明したように、それらを増やすには、web サイトを処理する web サーバーの数を減らして簡単です。 したがって、サーバーがより高いスループットを実現する場合のコストを削減してそれらの多くには、それ以外の場合よりも少数のサーバー トラフィック特定ボリュームの必要があるため、必要はありません。

.NET 4.5 の非同期プログラミング モデルのサポートが ASP.NET 4.5 には Web フォーム、MVC、および Web API に含まれますEntity Framework 6、および、 [Windows Azure ストレージ API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)します。

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 での非同期サポート

ASP.NET 4.5 でのサポートの非同期プログラミングが言語にだけでなく、MVC、Web フォーム、Web API フレームワークにも追加されました。 たとえば、ASP.NET MVC コント ローラー アクション メソッドは web 要求からデータを受信し、ブラウザーに送信される HTML を作成するビューにデータを渡します。 多くの場合、アクション メソッドは、web ページに表示するには、web ページに入力されたデータを保存するか、データベースや web サービスからデータを取得する必要があります。 これらのシナリオでは、アクション メソッドを非同期にする簡単な: 返す代わりに、 *ActionResult*オブジェクトを返す*タスク&lt;ActionResult&gt;* メソッドをマーク*async*キーワード。 メソッド内で場合、行のコードが待機時間に関連する操作を開始をマークする、await キーワードを使用します。

データベース クエリのリポジトリ メソッドを呼び出す単純なアクション メソッドを次に示します。

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

同じデータベースの呼び出しを非同期に処理するメソッドを次に示します。

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

内部では、コンパイラは、適切な非同期コードを生成します。 アプリケーションが呼び出しを行うときに`FindTaskByIdAsync`、ASP.NET では、`FindTask`要求し、ワーカー スレッドをアンワインドして、別の要求の処理で使用できるようにします。 ときに、`FindTask`要求を実行するには、その呼び出し後に続くコードの処理を続行するスレッドを再起動します。 までに暫定的に、`FindTask`これ以外の応答を待機する関連付けが有益な処理の実行に利用できるスレッドがあるデータが返されると、要求が開始します。

非同期コードは、いくつかのオーバーヘッドはありませんが、低負荷条件下でそのオーバーヘッドはごくわずかであり、中に利用可能なスレッドを待機するそれ以外の場合に記録される要求を処理できる高負荷条件下でします。

この種の ASP.NET 1.1 では、以降の非同期プログラミングを行うようなっていますが、簡単ではありません、エラーが発生しやすい、およびデバッグが困難でした。 ASP.NET 4.5 でのコーディングを簡略化しました、これで、もうしない理由はありません。

### <a name="async-support-in-entity-framework-6"></a>Entity Framework 6 での非同期サポート

4.5 での非同期サポートの一部として web サービスの呼び出し、ソケット、およびファイル システム I/O の非同期サポートをリリースしましたが、web アプリケーションの最も一般的なパターンは、データベースとデータ ライブラリは、非同期をサポートしていませんでした。 これで Entity Framework 6 は、データベースへのアクセスの非同期サポートを追加します。

Entity Framework 6 では、クエリや、データベースに送信されるコマンドが発生するすべてのメソッドは、非同期バージョンがあります。 次の例での非同期バージョンを示しています、*検索*メソッド。

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

この非同期サポートの機能の挿入、削除、更新、および単純な検索のだけでなく、LINQ クエリでも機能します。

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

`Async`のバージョン、`ToList`メソッドでこのコードにより、データベースに送信されるクエリ メソッドであるためです。 `Where`と`OrderByDescending`メソッドは、クエリのみを構成中に、`ToListAsync`メソッドは、クエリを実行し、応答を格納、`result`変数。

## <a name="summary"></a>まとめ

すべての web フレームワークと任意のクラウド環境をプログラミングでは、こちらの web 開発のベスト プラクティスを実装できますが、簡単には、ASP.NET と Windows Azure のツールがあります。 これらのパターンに従うと、web 層を簡単にスケールでき、各サーバーは多くのトラフィックを処理できるので、経費を最小限に抑えるします。

[[次へ] の章](single-sign-on.md)クラウドでのシングル サインオン シナリオの使用方法になります。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

ステートレスな web サーバー:

- [Microsoft Patterns and Practices - 自動スケール ガイダンス](https://msdn.microsoft.com/library/dn589774.aspx)します。
- [無効化の ARR のインスタンスの Windows Azure Web サイトでのアフィニティ](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)します。 Erez Benari、によるブログの投稿には、Windows Azure Web サイトでのセッション アフィニティがについて説明します。

CDN:

- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 の部分で構成します。 1時 34分: 00 エピソード 3 の開始時に CDN ディスカッションを参照してください。
- [Microsoft のパターンとプラクティス静的コンテンツ ホスティング パターン](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN レビュー](http://www.cdnreviews.com/)します。 多くの Cdn の概要です。

非同期のプログラミング:

- [ASP.NET MVC 4 での非同期メソッドを使用して](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)します。 Rick anderson のチュートリアルです。
- [非同期を使用した非同期プログラミングと Await (c# および Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)します。 非同期プログラミングの原理、ASP.NET 4.5 では、そのしくみ、およびそれを実装するコードを記述する方法を説明する MSDN ホワイト ペーパー。
- [Entity Framework の非同期クエリと保存](https://msdn.microsoft.com/data/jj819165)
- [非同期を使用して ASP.NET Web アプリケーションを構築する方法](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7)します。 Rowan miller のビデオ プレゼンテーションです。 グラフィックのデモが含まれています。 非同期プログラミングの高負荷条件下で web サーバーのスループットが大幅に増加を促進できます。
- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 の部分で構成します。 スケーラビリティ上の非同期プログラミングの影響についてのディスカッション、エピソード 4 および 8 のエピソードを参照してください。
- [ASP.NET 4.5 と重要な gotcha で非同期のメソッドを使用するマジック](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx)します。 ASP.NET Web フォーム アプリケーションでの非同期の使用について主に Scott Hanselman によるブログ投稿。

追加の web 開発のベスト プラクティスでは、次のリソースを参照してください。

- [修正プログラム、サンプル アプリケーションのベスト プラクティス](the-fix-it-sample-application.md#bestpractices)します。 この電子書籍の付録には、さまざまな Fix It アプリケーションで実装されたベスト プラクティスが一覧表示します。
- [Web 開発者のチェックリスト](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [前へ](continuous-integration-and-continuous-delivery.md)
> [次へ](single-sign-on.md)
