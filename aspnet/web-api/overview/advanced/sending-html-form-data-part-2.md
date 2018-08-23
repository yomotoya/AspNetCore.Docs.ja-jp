---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: ASP.NET Web API で HTML フォーム データを送信しますファイルのアップロードとマルチパート MIME |。Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 875f9ac62901dfbafc8224af2982c1daf3afc9c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838360"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>ASP.NET Web API で HTML フォーム データを送信しますファイルのアップロードとマルチパート MIME。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>パート 2: ファイルのアップロードとマルチパート MIME

このチュートリアルでは、web API にファイルをアップロードする方法を示します。 マルチパートの MIME データを処理する方法も説明します。

> [!NOTE]
> [完成したプロジェクトをダウンロード](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)します。


ファイルをアップロードするための HTML フォームの例を次に示します。

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

このフォームには、テキスト入力コントロールとファイルの入力コントロールが含まれています。 フォームには、ファイルの入力コントロールが含まれている場合、 **enctype**属性は常に&quot;マルチパート/フォーム データ&quot;フォームをマルチパート MIME メッセージとして送信されることを指定します。

マルチパート MIME メッセージの形式は、要求の例を見て理解する最も簡単なです。

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

このメッセージは 2 つに分かれています*パーツ*、それぞれのフォーム コントロールのいずれか。 ダッシュで始まる行では、一部の境界が示されます。

> [!NOTE]
> 一部の境界には、ランダムなコンポーネントが含まれています (&quot;41184676334&quot;) 境界文字列が表示されないこと誤ってメッセージ部分の内部ことを確認します。


各メッセージ部分には、後に一部の内容が 1 つまたは複数のヘッダーが含まれています。

- Content-disposition ヘッダーには、コントロールの名前が含まれています。 ファイル、そのファイル名も含まれます。
- Content-type ヘッダーには、一部のデータについて説明します。 このヘッダーを省略すると、既定値はテキスト/プレーンにします。

前の例では、ユーザーがコンテンツの種類のイメージ/jpeg; GrandCanyon.jpg、という名前のファイルをアップロードテキスト入力の値が、&quot;夏休み&quot;します。

## <a name="file-upload"></a>ファイルのアップロード

これでマルチパート MIME メッセージからファイルを読み取り、Web API コント ローラーを見てみましょう。 コント ローラーは、ファイルを非同期的に読み取ります。 Web API を使用して非同期のアクションをサポートしている、[タスク ベースのプログラミング モデル](https://msdn.microsoft.com/library/dd460693.aspx)します。 最初に、サポートする .NET Framework 4.5 を対象としている場合にコードをここでは、 **async**と**await**キーワード。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

コント ローラー アクションがパラメーターを受け取らないことに注意してください。 メディアの種類のフォーマッタを呼び出すことがなく、アクション内で要求本文を処理できないためにです。

**IsMultipartContent**メソッドは、マルチパート MIME メッセージが要求に含まれるかどうかを確認します。 それ以外の場合は、コント ローラーは、HTTP 状態コード 415 (Unsupported Media Type) を返します。

**MultipartFormDataStreamProvider**クラスは、アップロードされたファイルのファイル ストリームを割り当てるヘルパー オブジェクト。 マルチパート MIME メッセージを読み取り、呼び出し、 **ReadAsMultipartAsync**メソッド。 このメソッドは、すべてのメッセージ部分を抽出し、によって提供されるストリームに書き込みます、 **MultipartFormDataStreamProvider**します。

メソッドが完了したらからのファイルに関する情報を取得できます、 **FileData**コレクションであるプロパティの**MultipartFileData**オブジェクト。

- **MultipartFileData.FileName**ファイルの保存場所、サーバーでローカル ファイルの名前です。
- **MultipartFileData.Headers**パーツのヘッダーが含まれています (*いない*要求ヘッダー)。 これを使用するには、コンテンツにアクセスする\_Disposition および Content-type ヘッダー。

名前からわかるように、 **ReadAsMultipartAsync**は非同期のメソッドです。 メソッドの完了後に作業を実行するを使用する[継続タスク](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0) または**await**キーワード (.NET 4.5)。

.NET Framework 4.0 バージョンの前のコードを次に示します。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>フォーム コントロールのデータの読み取り

前に示した HTML フォームには、テキスト入力コントロールが必要があります。

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

コントロールの値を取得することができます、**フォーム データ**のプロパティ、 **MultipartFormDataStreamProvider**します。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**フォーム データ**は、 **NameValueCollection**フォーム コントロールの名前/値ペアを格納しています。 コレクションは、重複するキーを含めることができます。 このフォームを検討してください。

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

要求本文は、次のようになります。

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

その場合は、**フォーム データ**コレクションには、次のキー/値ペアにが含まれます。

- 乗車: ラウンドト リップ
- オプション: 無着陸
- オプション: 日付
- 接続クライアント数: ウィンドウ
