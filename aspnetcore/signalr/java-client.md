---
title: ASP.NET Core SignalR の Java クライアント
author: mikaelm12
description: ASP.NET Core SignalR の Java クライアントを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 3d2cfe5f58cc41741512c68cebbbc90e8f714cba
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148786"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR の Java クライアント

によって[Mikael Mengistu](https://twitter.com/MikaelM_12)

Java クライアントでは、Java のコードは、Android アプリを含む、ASP.NET Core SignalR サーバーに接続します。 ように、 [JavaScript クライアント](xref:signalr/javascript-client)と[.NET クライアント](xref:signalr/dotnet-client)、Java クライアントでは、リアルタイムでハブにメッセージを送受信することができます。 Java クライアントとは、ASP.NET Core 2.2 で使用可能な以降です。

この記事で参照されているサンプルの Java コンソール アプリでは、SignalR の Java クライアントを使用します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="install-the-signalr-java-client-package"></a>SignalR の Java クライアント パッケージをインストールします。

*Signalr 1.0.0 preview3 35501* JAR ファイルは、SignalR ハブに接続するクライアントを使用できます。 JAR ファイルの最新のバージョン番号を検索するには、次を参照してください。、 [Maven 検索結果](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr)します。

Gradle を使用する場合に、次の行を追加、`dependencies`のセクション、 *build.gradle*ファイル。

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

内の次の行を追加して Maven を使用した場合、`<dependencies>`の要素、 *pom.xml*ファイル。

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>ハブへの接続します。

確立するために、 `HubConnection`、`HubConnectionBuilder`使用する必要があります。 接続の作成中には、ハブの URL とログ レベルを構成できます。 いずれかを呼び出すことによって、必要なオプションを構成、`HubConnectionBuilder`前にメソッド`build`します。 接続を開始`start`します。

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

呼び出し`send`ハブ メソッドを呼び出します。 ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`send`します。

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>ハブからのクライアント メソッドを呼び出す

使用`hubConnection.on`ハブで呼び出すことができるクライアントでメソッドを定義します。 作成した後は、接続を開始する前に、メソッドを定義します。

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>ログ記録を追加します。

SignalR の Java クライアントを使用して、 [SLF4J](https://www.slf4j.org/)のログ記録ライブラリです。 ライブラリのユーザーは、特定のログ出力の依存関係に導入することで独自の特定のログ記録の実装を選択できるようにする高度なログ記録 API になります。 次のコード スニペットは、使用する方法を示します`java.util.logging`SignalR Java クライアントを使用します。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

依存関係でのログ記録を構成しない場合、SLF4J は、次の警告メッセージが既定の操作なしロガーを読み込みます。

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

これは無視しても。

## <a name="known-limitations"></a>既知の制限事項

これは、Java クライアントのプレビュー リリースです。 一部の機能はサポートされていません。

* JSON プロトコルのみがサポートされています。
* Websocket トランスポートのみがサポートされています。
* ストリーミングはまだサポートされません。

## <a name="additional-resources"></a>その他の技術情報

* [Java API リファレンス](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
