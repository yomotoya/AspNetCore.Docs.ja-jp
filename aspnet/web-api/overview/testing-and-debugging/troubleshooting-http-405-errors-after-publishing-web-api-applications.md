---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: "トラブルシューティング HTTP 405 エラー発行後に Web API 2 アプリケーション |Microsoft ドキュメント"
author: rmcmurray
description: "このチュートリアルでは、実稼働 web サーバーに Web API アプリケーションの発行後に HTTP 405 エラーをトラブルシューティングする方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>トラブルシューティング HTTP 405 エラー発行後に Web API 2 アプリケーション
====================
によって[Robert McMurray](https://github.com/rmcmurray)

> このチュートリアルでは、実稼働 web サーバーに Web API アプリケーションの発行後に HTTP 405 エラーをトラブルシューティングする方法について説明します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [インターネット インフォメーション サービス (IIS)](https://www.iis.net/) (バージョン 7 以降)
> - [Web API](../../index.md) (バージョン 1 または 2)


Web API アプリケーションは、通常いくつかの一般的な HTTP 動詞を使用: GET、POST、PUT、DELETE、および場合によっては修正プログラムを適用します。 つまり、開発者可能性がありますを実行ため、Visual Studio で、または開発サーバーで正しく動作している Web API コント ローラーが返されます、実稼働サーバー上の別の IIS モジュールによってこれらの動詞が実装されている場合に、HTTP 405 実稼働サーバーに配置されるとエラーが発生します。 幸運にもこの問題は、簡単に解決されたが、解決に問題が発生した理由の説明が保証されます。

## <a name="what-causes-http-405-errors"></a>どのような原因が HTTP 405 エラー

HTTP 405 エラーをトラブルシューティングする方法を学習するための第一歩は、HTTP 405 エラーが実際には意味を理解するのにです。 HTTP はドキュメントのプライマリ管理[RFC 2616](http://www.ietf.org/rfc/rfc2616.txt)、として HTTP 405 ステータス コードを定義する***メソッドが許可されていません***、状況としては、この状態コードの詳細を説明し、&quot;メソッド指定された要求の URI で識別されるリソースの要求行は許可されません。&quot;つまり、HTTP 動詞は、特定の URL を HTTP クライアントが要求したは使用できません。

簡単に確認としてここでは、最も使用される HTTP メソッドのいくつか RFC 2616、RFC 4918 および RFC 5789 で定義されています。

| HTTP メソッド | 説明 |
| --- | --- |
| **取得** | このメソッドはからデータを取得、URI と、おそらく、最も使用される HTTP メソッドを使用します。 |
| **HEAD** | このメソッドは、要求 URI のデータを実際に取得、しません - だけで、HTTP ステータスを取得する点を除いて GET メソッドと同様です。 |
| **投稿** | このメソッドは通常、URI に新しいデータを送信する使用します。POST は、フォーム データの送信に使用されます。 |
| **PUT** | このメソッドは通常、URI に生データを送信する使用します。PUT は、Web API アプリケーションへの JSON または XML データの送信によく使用します。 |
| **削除** | このメソッドを使用するには、URI からのデータの削除をします。 |
| **オプション** | このメソッドは通常、URI に対してサポートされている HTTP メソッドの一覧の取得に使用されます。 |
| **コピーの移動** | WebDAV を使用するこれら 2 つの方法が使用され、その目的は、ひとめでわかります。 |
| **MKCOL** | WebDAV を使用するこのメソッドを使用し、指定した URI (例: はディレクトリ) のコレクションを作成に使用されます。 |
| **PROPFIND PROPPATCH** | これら 2 つのメソッドは、WebDAV を使用するために使用して、クエリまたは URI のプロパティの設定に使用されます。 |
| **ロックのロックを解除します。** | これら 2 つのメソッドは、WebDAV を使用するために使用し、作成するときに、要求 URI で識別されるリソースをロック/ロック解除に使用されます。 |
| **修正プログラム** | このメソッドは、既存の HTTP リソースの変更を使用します。 |

これらの HTTP メソッドの 1 つは、サーバーで使用する構成は、ときに、サーバーは、HTTP ステータスとその他のデータ要求を適切な応答します。 (たとえば、GET メソッドが HTTP 200 を受け取ることがあります***OK***応答、および PUT メソッドは、HTTP 201 を受け取る可能性があります***Created***応答します)。

サーバーは HTTP 501 で応答、サーバーで使用する HTTP メソッドが構成されていない場合***未実装***エラーです。

ただし、サーバーで使用する HTTP メソッドが構成されているが、指定した URI を無効にされている、サーバーが HTTP 405 で応答が***メソッドが許可されていません***エラーです。

## <a name="example-http-405-error"></a>例 HTTP 405 エラー

次の例の HTTP 要求と応答 HTTP クライアントが値の web サーバー上の Web API アプリケーションを配置しようとすると、サーバーには、PUT メソッドではない状態が許可されている HTTP エラーが返されます。 ある場合を示しています。


HTTP 要求。


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP 応答:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


この例では HTTP クライアント、web サーバー上の Web API アプリケーションの URL に有効な JSON 要求を送信するが、サーバーは PUT メソッドが、URL を使用できないことを示す HTTP 405 エラー メッセージを返しました。 これに対し、要求 URI で Web API アプリケーションのルートが一致しなかった場合、サーバーが返す HTTP 404***が見つかりません。***エラーです。

## <a name="resolving-http-405-errors"></a>HTTP 405 の解決エラー

特定の HTTP 動詞を許可されていない可能性がありますが IIS でこのエラーの主要な原因である 1 つの主要なシナリオがある理由が考えられます。 同じ動詞/メソッドを複数のハンドラーが定義されているし、ハンドラーの 1 つがから予想されるハンドラーをブロックしています。要求を処理しています。 詳細については、によっては、IIS は、最後、順序ハンドラーの入力に基づいて <li>applicationhost.config および web.config ファイルで、パス、動詞、リソースなどの最初の一致する組み合わせが使用して、要求を処理する場所を最初からハンドラーを処理します。

次の例は、PUT メソッドを使用して Web API アプリケーションにデータを送信するときに、HTTP 405 エラーを返すが IIS サーバーの applicationHost.config ファイルから抜粋です。 この例ではいくつかの HTTP ハンドラーが定義されている場合、および各ハンドラーが構成されている HTTP メソッドの異なるセットには - 一覧の最後のエントリは、他のハンドラーがある、chanc 後に使用される既定のハンドラーは、静的コンテンツ ハンドラー要求を検査する e:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

上記の例では、HTTP メソッドの個別のリストの WebDAV ハンドラーおよび ASP.NET (これは、Web API の使用) の拡張子のない URL ハンドラーを明確に定義します。 この構成では、エラーが必ずしも発生しませんが、ISAPI DLL ハンドラーが構成されているすべての HTTP メソッドに注意してください。 ただし、構成設定などの HTTP 405 エラーのトラブルシューティング時に考慮する必要があります。

上記の例では ISAPI DLL ハンドラーは、問題がありませんでした。実際には、問題が、IIS サーバーの applicationHost.config ファイルに定義されていません - Visual Studio で Web API アプリケーションの作成時に、web.config ファイルに加えられたエントリによって問題が発生しました。 次のアプリケーションの web.config ファイルからの抜粋では、問題の発生場所を示します。

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

この例では ASP.NET の拡張子のない URL ハンドラーは Web API アプリケーションで使用される追加の HTTP メソッドが含まれてで再定義されています。 ただし、WebDAV ハンドラーのような一連の HTTP メソッドが定義されているため、競合が発生します。 この特定の場合に、WebDAV ハンドラーが定義され、Web API アプリケーションを含む web サイトの WebDAV が無効になっていても、IIS によって読み込まれます。 HTTP PUT 要求の処理中には、IIS は、PUT 動詞用に定義されるために、WebDAV モジュールを呼び出します。 WebDAV モジュールが呼び出されると、その構成がチェックされ、ように HTTP 405 が返されますが、無効である、表示される***メソッドが許可されていません***WebDAV 要求のようなすべての要求にエラーがあります。 この問題を解決するには、Web API アプリケーションが定義されている web サイトの HTTP モジュールの一覧から WebDAV を削除する必要があります。 次の例は、する様子新機能を示します。

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

このシナリオは、多くの場合、アプリケーションを開発環境から運用環境に公開し、このハンドラーとモジュールの一覧は、開発および運用環境の間で異なるために発生した後に発生します。 たとえば、Web API アプリケーションの開発に Visual Studio 2012 または 2013 を使用している場合は、IIS Express 8 はテスト用の既定の web サーバーです。 この開発 web サーバーは、サーバーの製品に付属するすべての IIS 機能の縮小バージョンであり、この開発 web サーバーには、開発シナ リオで追加されたいくつかの変更が含まれています。 など、WebDAV モジュールは多くの場合にインストールされて、IIS の完全バージョンを実行している実稼働 web サーバーが、実際に使用できない可能性があります。 (IIS Express)、IIS のある開発バージョンには、WebDAV モジュールがインストールされますが、WebDAV モジュールのエントリは意図的にコメント アウト、具体的には、IIS Express の構成を変更する場合を除き、IIS Express で WebDAV モジュールを読み込むことはありませんが、機能を追加する WebDAV IIS Express のインストールに設定します。 結果として、開発用コンピューターで、web アプリケーションが正しく動作可能性がありますが、実稼働 web サーバーに Web API アプリケーションを公開するときに HTTP 405 エラーが発生する可能性があります。

## <a name="summary"></a>概要

HTTP 405 HTTP メソッドが、要求された URL の web サーバーで許可されていないときにエラーが発生します。 この条件は、特定の動詞の特定のハンドラーが定義されており、そのハンドラーが要求を処理するハンドラーをオーバーライドするときに多くの場合、見られます。

つまり、サーバーで特定の機能が実装されていないこと、HTTP 501 エラー メッセージを受信するような状況が発生した場合多くの場合、つまり、HTTP 要求と一致する、IIS の設定で定義されているハンドラーがないことを可能性がありますかことを示します何かが正しくインストールされていません、システム上の IIS 設定を変更何かがいるためにハンドラーが定義されていないサポート、特定の HTTP メソッド。 その問題を解決するのには、HTTP メソッドを持たない対応するモジュールまたはハンドラーの定義を使用しようとするすべてのアプリケーションを再インストールする必要があります。
