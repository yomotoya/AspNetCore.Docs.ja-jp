---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: パフォーマンス向上のためのページ (Razor) サイトの ASP.NET web データのキャッシュ |Microsoft Docs
author: tfitzmac
description: 高速化する web サイト - つまり、保存することによりキャッシュのデータを取得または処理にかなりの時間を通常の結果をしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 4134c80d7eed4752c90a06aab796a0fd8c2a9782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383410"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>パフォーマンス向上のための ASP.NET Web Pages (Razor) サイトでデータのキャッシュ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトに高速なパフォーマンスの情報をキャッシュするためのヘルパーを使用する方法について説明します。 格納することにより、web サイトを短縮できます&#8212;キャッシュは、&#8212;通常を取得または処理にかなりの時間がかかるし、を頻繁に変更されないデータの結果。
> 
> **学習内容。** 
> 
> - Web サイトの応答性を向上させるためにキャッシュを使用する方法。
> 
> この記事で導入された ASP.NET 機能を次に示します。
> 
> - `WebCache`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


ユーザーは、サイトからページを要求するたびに要求に対処するために作業を web サーバーにします。 ページの一部で、サーバーは、データベースからデータの取得などの (比較的) に長い時間がかかるタスクを実行する必要があります。 場合でも、これらのタスクを絶対の用語で時間の長い使用しないサイトには、大量のトラフィックが発生した場合、一連の複雑なまたは低速のタスクを実行する web サーバーとなる個々 の要求が、多くの作業追加できます。 これにより、サイトのパフォーマンスが最終的には影響します。

このような状況で、web サイトのパフォーマンスを向上させる方法の 1 つでは、データをキャッシュします。 情報は、各ユーザーに変更する必要はありませんし時間は、サイトが同じ情報については、繰り返しの要求を取得します再フェッチまたは再計算するには、代わりに、機密性の高い 1 回のデータをフェッチして結果を格納します。 次回の要求を受信については、するだけを元にキャッシュから。

一般に、頻繁に変更されない情報をキャッシュします。 キャッシュに情報を格納するときに、web サーバーのメモリに格納されます。 どのくらいの期間キャッシュするように、秒から日に指定できます。 キャッシュの期間が経過すると、情報がキャッシュから自動的に削除します。

> [!NOTE]
> キャッシュ内のエントリ削除の可能性あり上の理由から他にも、期限が切れるしました。 など、web サーバーが一時的が残り少なくなったら、メモリとメモリを再利用できる 1 つの方法は、キャッシュからエントリをスローすることによって。 わかる、情報をキャッシュに配置した場合でもを確認して必要なときではまだありますがあります。


Web サイトが、現在の気温と天気予報を表示するページを想像してください。 この種の情報を取得するには、外部のサービスに要求を送信する可能性があります。 この情報は多く (たとえば 2 時間期間) 内で変更されないため、および外部の呼び出しは、時間と帯域幅が必要なため、キャッシュの候補を勧めします。

## <a name="adding-caching-to-a-page"></a>ページのキャッシュの追加

ASP.NET には、`WebCache`をキャッシュにデータを追加し、サイトにキャッシュを追加するが容易にするヘルパー。 この手順では、現在の時刻をキャッシュするページを作成します。 現在の時刻が多くの場合は変更されると、さらに計算する複雑なはないため、実際の例はありません。 ただし、これはキャッシュの動作を説明することをお勧めです。

1. という名前の新しいページを追加*WebCache.cshtml* web サイトにします。
2. 次のコードとマークアップをページに追加します。

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    データをキャッシュするときに配置する名前を使用してキャッシュにこれは、web サイトで一意です。 この場合、という名前のキャッシュ エントリを使用します`CachedTime`します。 これは、`cacheItemKey`コード例に示すようにします。

    コードを読み取り、`CachedTime`エントリをキャッシュします。 (つまり、キャッシュ エントリがない場合 null) 値が返された場合、コードは単データをキャッシュする時の変数の値を設定します。

    ただし、キャッシュ エントリが存在しない場合 (つまり、null の)、コード、時刻の値を設定、それをキャッシュに追加および有効期限の値を 1 分に設定します。 1 分後にキャッシュ エントリは破棄されます。 (キャッシュ内の項目の既定の有効期限の値は 20 分です)。コマンドは、`WebCache.Set(cacheItemKey, time, 1, false)`キャッシュに現在の時刻の値を追加し、1 分に、有効期限を設定する方法を示しています。 設定、 *slidingExpiration*パラメーターを`false`有効期限が要求されるたびに更新されないことを意味します。 最初にキャッシュに追加された後、正確に 1 分に期限が切れます。 この値を設定する場合`true`1 分の有効期限がキャッシュから値が要求されるたびにリセットされます。

    このコードは、データをキャッシュするときに常に使用する必要があります、パターンを示しています。 キャッシュから何かを取得する前に常にまず、かどうか、`WebCache.Get`メソッドが null を返しました。 キャッシュ エントリが期限切れになった可能性がありますか、削除されているその他の何らかの理由がキャッシュ内に、指定したエントリが保証されることはありませんので注意してください。
3. 実行*WebCache.cshtml*ブラウザーでします。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。最初に、ページを要求するには時間データがキャッシュにないし、コードは、時刻の値をキャッシュに追加する必要があります。

    ![キャッシュ-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. 更新*WebCache.cshtml*ブラウザーにします。 今回は、時刻のデータがキャッシュにします。 前回のページを表示した後、時間が変更されていないことに注意してください。

    ![キャッシュ 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. キャッシュを空にするのには 1 分間待機し、ページを更新します。 ページ、もう一度ことを示します時刻のデータがキャッシュに見つかりませんでした更新時刻が、キャッシュに追加されます。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [グラフでデータを表示する](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API リファレンス](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)
