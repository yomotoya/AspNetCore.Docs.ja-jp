---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: トラブルシューティング HTTP 405 エラー発行後に Web API 2 アプリケーション |Microsoft Docs
author: rmcmurray
description: このチュートリアルでは、実稼働 web サーバーに Web API アプリケーションを発行した後、HTTP 405 エラーをトラブルシューティングする方法について説明します。
ms.author: aspnetcontent
ms.date: 05/01/2014
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 7dd7fd1fc6be9bc2f843c293222179a9774dff3c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827865"
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>トラブルシューティング HTTP 405 エラー発行後に Web API 2 アプリケーション
====================
によって[Robert McMurray](https://github.com/rmcmurray)

> このチュートリアルでは、実稼働 web サーバーに Web API アプリケーションを発行した後、HTTP 405 エラーをトラブルシューティングする方法について説明します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [インターネット インフォメーション サービス (IIS)](https://www.iis.net/) (バージョン 7 以降)
> - [Web API](../../index.md) (バージョン 1 または 2)


通常、web API アプリケーションにいくつかの一般的な HTTP 動詞が使用: GET、POST、PUT、DELETE、および場合によっては修正プログラムを適用します。 開発者が、実行可能性がありますまたは Visual Studio で開発サーバーで正しく機能する Web API コント ローラーが返すが状態になります、実稼働サーバー上の別の IIS モジュールによってこれらの動詞が実装されている場合に、HTTP 405 実稼働サーバーに配置されるとエラーが発生します。 幸いにもこの問題は、簡単に解決しますが、解像度の発行に値する問題が発生している理由の説明。

## <a name="what-causes-http-405-errors"></a>どのような原因が HTTP 405 エラー

HTTP 405 エラーをトラブルシューティングする方法を学習するための第一歩 HTTP 405 エラーが実際に意味を理解することです。 HTTP はドキュメントの管理の主な[RFC 2616](http://www.ietf.org/rfc/rfc2616.txt)、として HTTP 405 状態コードを定義する***Method Not Allowed***、状況としては、この状態コードの詳細を説明し、&quot;メソッド指定された要求の URI で識別されるリソースの要求行が許可されていません。&quot;つまり、HTTP 動詞は、HTTP クライアントが要求された特定の URL には使用できません。

について簡単に確認としてここでは最も使用される HTTP メソッドのいくつか定義されている RFC 2616、RFC 4918、および RFC 5789。

| HTTP メソッド | 説明 |
| --- | --- |
| **GET** | データを取得、URI と、おそらく最も使用される HTTP メソッドをこのメソッドが使用されます。 |
| **ヘッド** | このメソッドは、要求 URI のデータの取得には実際に - HTTP 状態だけを取得する点を除いて、GET メソッドと同様です。 |
| **投稿** | このメソッドは通常、URI に新しいデータを送信する使用します。投稿がフォーム データの送信によく使用されます。 |
| **PUT** | このメソッドは通常、URI に生データを送信する使用します。PUT は、Web API アプリケーションへの JSON または XML データを送信するよく使用されます。 |
| **DELETE** | このメソッドを使用して、URI からデータを削除します。 |
| **オプション** | このメソッドは通常、URI でサポートされる HTTP メソッドの一覧の取得に使用します。 |
| **コピーの移動** | これら 2 つのメソッドに WebDAV を使用して、その目的は一目瞭然です。 |
| **MKCOL** | WebDAV を使用するこのメソッドを使用して、指定した URI (例: ディレクトリ) コレクションの作成に使用されます。 |
| **実行します。** | これら 2 つのメソッドに WebDAV を使用して、クエリまたは URI のプロパティの設定に使用されます。 |
| **ロックのロックを解除します。** | WebDAV を使用する 2 つのメソッドを使用し、作成するときに、要求 URI で識別されるリソースをロック/ロック解除するために使用します。 |
| **修正プログラム** | このメソッドは、既存の HTTP リソースの変更に使用されます。 |

これらの HTTP メソッドのいずれかを構成サーバーで使用するため、サーバーは HTTP ステータスとその他のデータ要求の適切な応答します。 (たとえば、GET メソッドが HTTP 200 を受け取ることがあります***OK***応答、および PUT メソッドは、HTTP 201 を受け取ることがあります***Created***応答します)。

サーバーで使用する HTTP メソッドが構成されていない場合、サーバーは、HTTP 501 で応答***未実装***エラー。

ただし、サーバーで、使用する HTTP メソッドが構成されている、指定した URI を無効になっている場合は、サーバーが、HTTP 405 で応答が***Method Not Allowed***エラー。

## <a name="example-http-405-error"></a>例 HTTP 405 エラー

次の例の HTTP 要求と応答で HTTP クライアントが、web サーバー上の Web API アプリケーションに値を入力しようとして、PUT メソッドではない状態が許可されている HTTP エラーを返しますの状況を示しています。


HTTP 要求:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP 応答:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


この例では、HTTP クライアントは、web サーバー上の Web API アプリケーションの URL に有効な JSON 要求を送信が、サーバーを PUT メソッドが、URL の許可されないことを示す HTTP 405 エラー メッセージを返しました。 これに対し、要求 URI で Web API アプリケーションのルートが一致しなかった場合、サーバーが返す HTTP 404***見つかりません***エラー。

## <a name="resolving-http-405-errors"></a>解決 HTTP 405 エラー

特定の HTTP 動詞を許可しない場合がありますが、IIS でこのエラーの主要な原因である 1 つの主なシナリオがある理由が考えられます同じ動詞/メソッドを複数のハンドラーが定義されていると、ハンドラーの 1 つが予期されるハンドラーをブロックしている。要求を処理します。 詳細についてを使用して IIS が要求を処理するパス、動詞、リソースなどの最初の一致する組み合わせを使用は、applicationHost.config および web.config ファイルで受注ハンドラー エントリに基づいて最後に最初のハンドラーを処理します。

次の例は、PUT メソッドを使用して、Web API アプリケーションにデータを送信する場合、HTTP 405 エラーを返すことがある IIS サーバーの applicationHost.config ファイルから抜粋です。 この抜粋では、いくつかの HTTP ハンドラーが定義されている場合、各ハンドラーが構成される HTTP メソッドの異なるセット - し、一覧の最後のエントリは、他のハンドラーが、chanc になった後に使用される既定のハンドラーは、静的コンテンツ ハンドラー要求を確認する電子メール:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

上記の例では、HTTP メソッドの別の一覧については、WebDAV ハンドラーと ASP.NET (これは、Web API の使用) の拡張子のない URL のハンドラーを明確に定義します。 この構成でエラーが必ずしも発生されていないすべての HTTP メソッドの ISAPI DLL ハンドラーが構成されていることに注意してください。 ただし、構成設定などの HTTP 405 エラーのトラブルシューティング時に考慮する必要があります。

上記の例では ISAPI DLL ハンドラー問題がありませんでした。実際には、問題が IIS サーバーの applicationHost.config ファイルで定義されていません - が原因で Visual Studio で Web API アプリケーションの作成時に、web.config ファイルに加えられたエントリ。 次のアプリケーションの web.config ファイルからの抜粋では、問題の発生場所を示します。

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

この抜粋では、ASP.NET の拡張機能のない URL のハンドラーが再定義、Web API アプリケーションで使用される追加の HTTP メソッドが含まれます。 ただし、WebDAV ハンドラーのような一連の HTTP メソッドが定義されている、ため、競合が発生します。 この特定のケースでは、WebDAV ハンドラーが定義され、Web API アプリケーションが含まれている web サイトの WebDAV が無効になっている場合でも、IIS によって読み込まれます。 HTTP PUT 要求を処理中には、IIS は、PUT 動詞に定義されているために、WebDAV モジュールを呼び出します。 その構成を確認し、HTTP 405 が返されますためには、無効が表示されます、WebDAV モジュールが呼び出されると、 ***Method Not Allowed*** WebDAV 要求のようなすべての要求のエラー。 この問題を解決するには、Web API アプリケーションが定義されている web サイト用の HTTP モジュールの一覧から WebDAV を削除する必要があります。 次の例では、表示する可能性がありますようになります。

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

このシナリオは、多くの場合、アプリケーションが開発環境から実稼働環境に発行され、ハンドラーとモジュールの一覧は、開発および運用環境の間で異なるため、これが発生した後に発生します。 たとえば、Web API アプリケーションの開発に Visual Studio 2012 または 2013 を使用する場合 IIS Express 8 はテスト用の既定の web サーバー。 この開発 web サーバーはサーバー製品に付属する完全な IIS 機能のスケール ダウン バージョンであり、この開発 web サーバーには、開発シナ リオで追加されたいくつかの変更が含まれています。 たとえば、実際に使用できない可能性がありますが、完全なバージョンの IIS を実行している実稼働 web サーバー上 WebDAV モジュールはインストール多くの場合。 (IIS Express)、IIS の開発バージョンには、WebDAV モジュールがインストールされますが、WebDAV モジュールのエントリは意図的にコメント アウト、具体的には、IIS Express の構成を変更しない限り、IIS Express で WebDAV モジュールがロードされませんのでIIS Express のインストールに WebDAV 機能を追加する設定。 その結果、開発用コンピューターで、web アプリケーションが正しく動作可能性がありますが、実稼働 web サーバーに Web API アプリケーションを発行するときに、HTTP 405 エラーが発生する可能性があります。

## <a name="summary"></a>まとめ

HTTP 405 エラーが発生する HTTP メソッドが要求された URL の web サーバーで許可されません。 この条件は、特定の動作の特定のハンドラーが定義されており、そのハンドラーが要求を処理するハンドラーをオーバーライドするときに多くの場合、見られます。

サーバーで特定の機能が実装されていません、つまり、HTTP 501 エラー メッセージを受信するような状況が発生した場合多くの場合、つまり、HTTP 要求に一致する、IIS の設定で定義されているハンドラーがないことをおそらく、何かが正しくインストールされていません、システム上またはハンドラーが定義されていないサポート、特定の HTTP メソッドが存在するように、IIS の設定が変更何かを示します。 その問題を解決するには、HTTP メソッドを持たない対応するモジュールまたはハンドラーの定義を使用しようとする任意のアプリケーションを再インストールする必要があります。
