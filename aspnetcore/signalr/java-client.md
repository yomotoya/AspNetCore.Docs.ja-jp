---
title: ASP.NET Core SignalR の Java クライアント
author: mikaelm12
description: ASP.NET Core SignalR の Java クライアントを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482919"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR の Java クライアント

によって[Mikael Mengistu](https://twitter.com/MikaelM_12)

Java クライアントでは、Java のコードは、Android アプリを含む、ASP.NET Core SignalR サーバーに接続します。 ように、 [JavaScript クライアント](xref:signalr/javascript-client)と[.NET クライアント](xref:signalr/dotnet-client)、Java クライアントでは、リアルタイムでハブにメッセージを送受信することができます。 Java クライアントとは、ASP.NET Core 2.2 で使用可能な以降です。

この記事で参照されているサンプルの Java コンソール アプリでは、SignalR の Java クライアントを使用します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="install-the-signalr-java-client-package"></a>SignalR の Java クライアント パッケージをインストールします。

*Signalr 0.1.0 preview2 35174* JAR ファイルは、SignalR ハブに接続するクライアントを使用できます。 JAR ファイルの最新のバージョン番号を検索するには、次を参照してください。、 [Maven 検索結果](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav)します。

Gradle を使用する場合に、次の行を追加、`dependencies`のセクション、 *build.gradle*ファイル。

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

内の次の行を追加して Maven を使用した場合、`<dependencies>`の要素、 *pom.xml*ファイル。

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>ハブへの接続します。

確立するために、 `HubConnection`、`HubConnectionBuilder`使用する必要があります。 接続の作成中には、ハブの URL とログ レベルを構成できます。 いずれかを呼び出すことによって、必要なオプションを構成、`HubConnectionBuilder`前にメソッド`build`します。 接続を開始`start`します。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

呼び出し`send`ハブ メソッドを呼び出します。 ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`send`します。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>ハブからのクライアント メソッドを呼び出す

使用`hubConnection.on`ハブで呼び出すことができるクライアントでメソッドを定義します。 作成した後は、接続を開始する前に、メソッドを定義します。

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>既知の制限事項

これは、Java クライアントの初期のプレビュー リリースです。 まだサポートされていない多くの機能があります。 次のギャップは、今後のリリースで作業中は。

* プリミティブ型だけでは、パラメーターとして受け入れることができ、型を返します。
* Api は同期的です。
* 「送信」呼び出しの種類のみが現時点でサポートされています。 「呼び出し」と戻り値のストリーミングはサポートされていません。
* JSON プロトコルのみがサポートされています。
* Websocket トランスポートのみがサポートされています。

## <a name="additional-resources"></a>その他の技術情報

* [Java API リファレンス](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
