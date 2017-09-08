---
title: "ネイティブなモバイル アプリケーションのバックエンド サービスを作成します。"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: f2b74d6739112909ee05e036e904d5e30868fdbe
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>ネイティブなモバイル アプリケーションのバックエンド サービスを作成します。

によって[Steve Smith](http://ardalis.com)

モバイル アプリは、ASP.NET Core バックエンド サービスと通信に簡単にできます。

[表示またはバックエンド サービスのサンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>サンプルのネイティブ モバイル アプリ

このチュートリアルでは、ネイティブ モバイル アプリをサポートするために ASP.NET Core MVC を使用してバックエンド サービスを作成する方法を示します。 使用して、 [Xamarin フォーム ToDoRest アプリ](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/)をその native client と Android、iOS、Windows ユニバーサルおよび Window Phone のデバイス用の個別のネイティブ クライアントが含まれています。 ネイティブ アプリを作成する (および必要な空き Xamarin ツールをインストールする) にリンクされているチュートリアルに従ってしたりできるよう Xamarin サンプル ソリューションをダウンロードします。 Xamarin サンプルには、この記事の ASP.NET Core アプリケーションに置き換えます (クライアントで必要な変更はありません)、ASP.NET Web API 2 services プロジェクトが含まれています。

![Android フォンで実行されているは Rest アプリケーションに](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>フィーチャー

ToDoRest アプリには、一覧表示する、追加、削除、および作業項目の更新がサポートしています。 各アイテムには、ID、名前やノートでは、まだ完了されてがあるかどうかを示すプロパティです。

アイテムのメイン ビュー、上記のようには、各項目の名前を一覧表示し、かどうかは、チェック マークの付いた実行を示します。

タップすると、`+`アイコンは、追加の項目 ダイアログを開きます。

![追加の項目 ダイアログ ボックス](native-mobile-backend/_static/todo-android-new-item.png)

メイン リスト 画面で項目をタップすると、編集 ダイアログ ボックスで、項目の名前、メモ、および元に戻す設定を変更できる、または項目を削除することができますが開きます。

![項目のダイアログ ボックスを編集します。](native-mobile-backend/_static/todo-android-edit-item.png)

このサンプルは、既定では読み取り専用の操作を許可するように developer.xamarin.com でホストされているバックエンド サービスを使用して構成されます。 お使いのコンピューターで実行されている次のセクションで作成された ASP.NET Core アプリケーションに対してを自分でテストするには、アプリを更新する必要があります`RestUrl`定数。 移動し、`ToDoREST`プロジェクトから開き、 *Constants.cs*ファイル。 置換、`RestUrl`マシンの ip アドレスを含む URL を使用してアドレス (localhost または 127.0.0.1、コンピューターからではなく、デバイス エミュレーターからこのアドレスが使用されるため)。 ポート番号も (5000) が含まれます。 デバイスで、サービスが動作するかをテストするために、アクティブなファイアウォールがこのポートへのアクセスをブロックしていないことを確認します。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "csharp"} -->

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>ASP.NET Core プロジェクトを作成します。

Visual Studio で新しい ASP.NET Core Web アプリケーションを作成します。 認証なしの Web API テンプレートを選択します。 プロジェクトに名前を*ToDoApi*です。

![選択されている Web API プロジェクト テンプレートを使って新しい ASP.NET Web アプリケーション ダイアログ](native-mobile-backend/_static/web-api-template.png)

アプリケーションは、5000 のポートに加えられたすべての要求に応答する必要があります。 更新*Program.cs*に含める`.UseUrls("http://*:5000")`これを実現します。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> IIS Express、既定では、ローカル以外の要求を無視する背後ではなく、直接、アプリケーションを実行することを確認してください。 実行`dotnet run`コマンド プロンプトから、または、Visual Studio ツールバーのデバッグ ターゲット ドロップダウン リストから、アプリケーション名のプロファイルを選択します。

作業項目を表すためのモデル クラスを追加します。 マークは必須フィールドを使用して、`[Required]`属性。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

API のメソッドでは、データを操作するいくつかの方法が必要です。 使用して同じ`IToDoRepository`インターフェイスの元の Xamarin サンプルの使用。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

このサンプルでは、実装はプライベート項目のコレクションだけを使用します。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

実装を構成する*Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

作成する準備ができたら、この時点で、 *ToDoItemsController*です。

> [!TIP]
> Api の web の作成の詳細を学習[構築 Your 最初 Web API Core MVC の ASP.NET と Visual Studio で](../tutorials/first-web-api.md)です。

## <a name="creating-the-controller"></a>コント ローラーを作成します。

新しいコント ローラーをプロジェクトに追加*ToDoItemsController*です。 Microsoft.AspNetCore.Mvc.Controller から継承してする必要があります。 追加、`Route`コント ローラーではじまるパスへの要求を処理することを示すために属性`api/todoitems`です。 `[controller]`ルート内の変数が、コント ローラーの名前に置き換えられます (を省略すると、`Controller`サフィックス)、およびグローバルのルートで特に役立ちます。 詳細については[ルーティング](../fundamentals/routing.md)です。

コント ローラーが必要です、`IToDoRepository`を関数には、コント ローラーのコンス トラクターでは、この型のインスタンスを要求します。 実行時に、このインスタンスが行われますのフレームワークのサポートを使用して[依存性の注入](../fundamentals/dependency-injection.md)です。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

この API は、データ ソースに対して CRUD (作成、読み取り、更新、削除) 操作を実行する 4 つの異なる HTTP 動詞をサポートします。 HTTP GET 要求に対応する読み取り操作は、これらの最も簡単なのです。

### <a name="reading-items"></a>項目の読み取り

GET 要求では項目の一覧を要求する、`List`メソッドです。 `[HttpGet]`属性を`List`メソッドでは、このアクションが GET 要求を処理する必要がありますのみを示します。 このアクションのルートがコント ローラーで指定されたルートです。 必ずしも、ルートの一部として、アクションの名前を使用する必要はありません。 各アクションに一意で明確なルートがあることを確認する必要があります。 ルーティング属性は、コント ローラーと特定のルートを構築するメソッドのレベルの両方に適用できます。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List`メソッド 200 OK の応答コードとそのすべての JSON としてシリアル化、ToDo 項目を返します。

などのさまざまなツールを使用して、新しい API メソッドをテストする[Postman](https://www.getpostman.com/docs/)、ここで示すようにします。

![Todoitems とを示す、返される次の 3 つの項目の JSON 応答の本文の GET 要求を示す postman コンソール](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>項目の作成

慣例により、新しいデータ項目の作成、HTTP POST 動詞にマップされます。 `Create`メソッドには、`[HttpPost]`属性が適用されているし、受け入れる、`ToDoItem`インスタンス。 以降、 `item` 、通知の本文で渡される引数、このパラメーターで装飾されて、`[FromBody]`属性。

メソッドの内部有効性および前の存在、データ ストア内の項目がチェックして、リポジトリを使用して追加されますの問題が発生しなかった場合。 チェック`ModelState.IsValid`実行[モデルの検証](../mvc/models/validation.md)、し、ユーザー入力を受け付けるすべての API メソッドで行う必要があります。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

サンプルは、モバイル クライアントに渡されるエラー コードを含む列挙型を使用します。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

要求の本文の JSON 形式で新しいオブジェクトを提供する POST 動詞を選択して Postman を使って新しいアイテムを追加することをテストします。 指定する要求ヘッダーを追加する必要がありますも、`Content-Type`の`application/json`します。

![POST と応答を示す postman コンソール](native-mobile-backend/_static/postman-post.png)

メソッドは、応答で新しく作成されたアイテムを返します。

### <a name="updating-items"></a>アイテムの更新

レコードの変更は実行 HTTP PUT 要求を使用します。 この変更以外、`Edit`メソッドはほぼ同じである`Create`です。 レコードが見つからない場合、`Edit`アクションが返されます、 `NotFound` (404) 応答です。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Postman をテストするには、put 動詞を変更します。 要求の本文で更新されたオブジェクト データを指定します。

![PUT と応答を示す postman コンソール](native-mobile-backend/_static/postman-put.png)

このメソッドが戻る、 `NoContent` (204) 応答の整合性の既存の API が成功するとします。

### <a name="deleting-items"></a>アイテムの削除

レコードを削除する操作は、サービスに DELETE 要求を行うと、削除する項目の ID を渡すことによって行います。 存在しない項目の要求を受信する更新では、として`NotFound`応答します。 それ以外の場合、要求が成功した場合になります、 `NoContent` (204) 応答です。

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

削除機能をテストする場合は nothing に必要である、要求の本文に注意してください。

![削除と応答を示す postman コンソール](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Web API の一般的な規則

アプリのバックエンド サービスを開発する際、一貫性のある一連の規則または横断的関心事を処理するためのポリシーを起動することができます。 たとえば前に、示したサービスで受信が見つかりませんでした。 特定のレコードの要求、 `NotFound` 、応答ではなく、`BadRequest`応答します。 同様に、コマンドが常に確認の種類のバインドされたモデルに渡されたこのサービスに加えられた`ModelState.IsValid`返されると、`BadRequest`の無効なモデルの種類。

独自の Api の一般的なポリシーを識別したら、通常をカプセル化できますで、[フィルター](../mvc/controllers/filters.md)です。 詳細については[ASP.NET Core MVC アプリケーションで共通の API ポリシーをカプセル化する](https://msdn.microsoft.com/magazine/mt767699.aspx)です。
