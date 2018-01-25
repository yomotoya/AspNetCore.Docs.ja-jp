---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "ASP.NET Web API で HTML フォームのデータを送信する: ファイルのアップロードとマルチパート MIME |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>ASP.NET Web API で HTML フォームのデータを送信する: ファイルのアップロードとマルチパート MIME
====================
によって[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>パート 2: ファイルのアップロードとマルチパート MIME

このチュートリアルでは、web API にファイルをアップロードする方法を示します。 マルチパート MIME データを処理する方法についても説明します。

> [!NOTE]
> [完成したプロジェクトをダウンロード](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)です。


ファイルをアップロードするための HTML フォームの例を次に示します。

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

このフォームには、テキスト入力コントロールとファイルの入力コントロールが含まれています。 フォームには、ファイルの入力コントロールが含まれている場合、 **enctype**属性は常に&quot;マルチパート フォーム データ&quot;フォームをマルチパート MIME メッセージとして送信されることを指定します。

マルチパート MIME メッセージの形式は、要求の例を調べることで理解する最も簡単なです。

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

このメッセージは 2 つに分かれています*パーツ*、フォーム コントロールごとに 1 つです。 ダッシュで始まる行では、部品の境界が示されます。

> [!NOTE]
> 一部の境界には、ランダムなコンポーネントが含まれています (&quot;41184676334&quot;) を境界文字列が表示されないこと誤ってメッセージ部分の内部を確認します。


各メッセージ部分には、パーツのコンテンツを続けて、1 つまたは複数のヘッダーが含まれています。

- Content-disposition ヘッダーには、コントロールの名前が含まれています。 ファイル、そのファイル名も含まれます。
- Content-type ヘッダーには、一部のデータについて説明します。 このヘッダーを省略すると、既定値はテキスト/プレーンにします。

前の例では、ユーザーがコンテンツの種類の画像/jpeg 形式; GrandCanyon.jpg をという名前のファイルをアップロードテキスト入力の値と&quot;夏休み&quot;です。

## <a name="file-upload"></a>ファイルのアップロード

これでマルチパート MIME メッセージからファイルを読み取り、Web API コント ローラーを見てみましょう。 コント ローラーが非同期的に、ファイルを読み取る。 Web API を使用して非同期のアクションをサポートしている、[タスク ベース プログラミング モデル](https://msdn.microsoft.com/library/dd460693.aspx)です。 最初に、次のコードは、サポートする .NET Framework 4.5 を対象としている場合、 **async**と**await**キーワード。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

コント ローラーのアクションが、すべてのパラメーターを受け取らないことに注意してください。 現在、メディア タイプ フォーマッタを呼び出すことがなく、アクション内の要求本文を処理するためです。

**IsMultipartContent**メソッドでは、マルチパート MIME メッセージが要求に含まれているかどうかを確認します。 それ以外の場合は、コント ローラーは、HTTP 状態コード 415 (サポートされていないメディアの種類) を返します。

**MultipartFormDataStreamProvider**クラスは、アップロードされたファイルのファイル ストリームの割り当てを行うためのヘルパー オブジェクト。 マルチパート MIME メッセージを読み取りを呼び出して、 **ReadAsMultipartAsync**メソッドです。 このメソッドは、すべてのメッセージ部分を抽出し、によって提供されるストリームに書き込みます、 **MultipartFormDataStreamProvider**です。

メソッドが完了したら、ファイルからの情報を取得できます、 **FileData**プロパティは、これは、コレクションの**MultipartFileData**オブジェクト。

- **MultipartFileData.FileName**ファイルの保存場所、サーバーでローカル ファイル名を指定します。
- **MultipartFileData.Headers**パーツのヘッダーが含まれています (*いない*要求ヘッダー)。 これを使用するには、コンテンツにアクセスする\_廃棄、Content-type ヘッダー。

名前からわかるように、 **ReadAsMultipartAsync**非同期メソッドです。 メソッドが終了した後で作業を実行する、[継続タスク](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0 の場合) または**await**キーワード (.NET 4.5)。

.NET Framework 4.0 バージョンの前のコードを次に示します。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>フォーム コントロールのデータの読み取り

前に示した HTML フォームには、テキスト入力コントロールが必要があります。

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

コントロールの値を取得することができます、 **FormData**のプロパティ、 **MultipartFormDataStreamProvider**です。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData**は、 **NameValueCollection**フォーム コントロールの名前/値ペアを格納します。 コレクションには、重複するキーを含めることができます。 このフォームを考慮してください。

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

要求本文は、次のようになります。

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

その場合は、 **FormData**コレクションには、次のキー/値ペアにが含まれます。

- トリップ: ラウンドト リップ
- options: nonstop
- options: dates
- 接続クライアント数: ウィンドウ
