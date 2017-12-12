---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "パフォーマンス向上のためのページ (Razor) サイトの ASP.NET Web のデータ キャッシュ |Microsoft ドキュメント"
author: tfitzmac
description: "時間を短縮できます web サイトをストア - は、キャッシュのデータを取得または処理にかなりの時間を通常の結果をしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: c747fef33a6d1db19f09fd0303c47d689b956687
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>パフォーマンス向上のための ASP.NET Web Pages (Razor) サイトのデータのキャッシュ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、パフォーマンスを向上させる ASP.NET Web Pages (Razor) の web サイトに情報をキャッシュするためのヘルパーを使用する方法について説明します。 Web サイトをスピードアップするには、ストア &#8212; ことでキャッシュ &#8212; は、多くの場合、通常は夥しいを取得または処理にかなりの時間とデータの結果は変更されません。
> 
> **学習する内容。** 
> 
> - キャッシュを使用して、web サイトの応答性を向上させる方法です。
> 
> アーティクルで導入された ASP.NET 機能を次に示します。
> 
> - `WebCache`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


他のユーザーは、サイトからページを要求するたびに、web サーバーは、いくつかの作業を行う要求を処理するためにします。 ページの一部で、サーバーは、時間がかかる (比較的)、データベースからデータを取得するなどのタスクを実行する必要があります。 場合でも、サイトには、大量のトラフィックが発生した場合、これらのタスクは、絶対的に長いとらない、一連の web サーバーの複雑なまたは低速のタスクを実行する個々 の要求は、多くの作業追加できます。 これにより、サイトのパフォーマンスが最終的には影響します。

次のような状況で、web サイトのパフォーマンスを向上させるために 1 つの方法は、データをキャッシュします。 サイトが、同じ情報を繰り返し要求を取得する場合と情報が各ユーザーに変更する必要はありません、時間がない場合再フェッチまたは再計算するには、代わりに、機密性の高い 1 回に、データをフェッチして結果を格納します。 次回、要求を受け取ったことについては、だけ取得してキャッシュからです。

一般に、頻繁に変化しない情報をキャッシュします。 キャッシュ内の情報を格納するときに、web サーバー上のメモリに格納されます。 時間キャッシュするように、日に秒からを指定することができます。 キャッシュ期間が経過すると、情報は自動的にキャッシュから削除します。

> [!NOTE]
> キャッシュ内のエントリが削除される上の理由から以外の場合、期限が切れるしました。 たとえば、web サーバーが一時的に実行される低メモリ、およびメモリが再利用できる 1 つの方法はキャッシュからエントリをスローすることによって。 わかる、情報をキャッシュに配置した場合でも、確認しなければならない場合に必要なときではまだ存在します。


Web サイトが、現在の温度と天気予報を表示するページを想像してください。 この種の情報を取得するには、外部サービスに要求を送信する可能性があります。 この情報はあまり (2 時間の時間内など) を変更しないため、および外部呼び出しには、時間と帯域幅が必要があるため、適切な候補はキャッシュです。

## <a name="adding-caching-to-a-page"></a>キャッシュ ページを追加します。

ASP.NET には、`WebCache`ヘルパーをサイトにキャッシュを追加し、キャッシュにデータを追加するが容易です。 この手順では、現在の時刻をキャッシュするページを作成します。 現在の時刻は、多くの場合、その変更があり、さらにではないを計算する複雑なため実際の例ではありません。 ただしはキャッシュの動作を説明することをお勧めします。

1. という名前の新しいページを追加*WebCache.cshtml* web サイトを参照します。
2. ページには、次のコードとマークアップを追加します。

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    データをキャッシュする場合に配置する名前を使用してキャッシュにこれは、web サイト全体で一意です。 この場合は、という名前のキャッシュ エントリを使用します`CachedTime`です。 これは、`cacheItemKey`のコード例に示すようにします。

    コードを読み取り、`CachedTime`エントリをキャッシュします。 (つまり、キャッシュ エントリがない場合 null) 値が返される場合は、コードは、データをキャッシュする時間変数の値をだけ設定します。

    ただし、キャッシュ エントリが存在しない場合 (つまり、null である)、コード、時刻の値を設定、キャッシュに追加および有効期限の値を 1 分に設定します。 1 分後、キャッシュ エントリは破棄されます。 (キャッシュ内のアイテムを既定の有効期限の値は 20 分です)。コマンドは、`WebCache.Set(cacheItemKey, time, 1, false)`キャッシュに現在の時刻値を追加し、1 分に、有効期限を設定する方法を示しています。 設定、 *slidingExpiration*パラメーターを`false`有効期限が要求されるたびに更新されていないことを意味します。 最初にキャッシュに追加された後に、正確に 1 分に期限が切れます。 この値を設定する場合`true`1 分の有効期限は、値がキャッシュから要求されるたびをリセットします。

    このコードは、データをキャッシュするときに常に使用する必要があります、パターンを示します。 キャッシュからのものを取得する前に常に最初がチェックするかどうか、`WebCache.Get`メソッドが null を返しました。 キャッシュ エントリの期限が切れている可能性がありますかが削除された可能性が何らかの理由により、キャッシュ内にある、指定したエントリが保証されることはありませんのでことに注意してください。
3. 実行*WebCache.cshtml*ブラウザーでします。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。初めて、ページを要求する時のデータは、キャッシュではありません、コードには、キャッシュに時刻の値を追加するには

    ![キャッシュ-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. 更新*WebCache.cshtml*ブラウザーにします。 現時点では、時間のデータは、キャッシュにです。 前回のページを表示した後、時間が変更されていないことに注意してください。

    ![キャッシュ-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. キャッシュを空にするのには 1 分間待機し、ページを更新します。 ページ再度ことを示す時のデータがキャッシュに見つかりませんでしたが更新された時間が、キャッシュに追加されます。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [グラフのデータを表示します。](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API リファレンス](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)
