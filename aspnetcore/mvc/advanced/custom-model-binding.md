---
title: "カスタム モデル バインディング"
author: ardalis
description: "ASP.NET Core mvc モデル バインディングをカスタマイズします。"
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 85d5ca18944e774d1f2577459c6c45acde01e4d9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="custom-model-binding"></a>カスタム モデル バインディング

によって[Steve Smith](https://ardalis.com/)

モデル バインディングでは、モデルの種類 (渡されるとメソッドの引数として) ではなく HTTP 要求よりもを直接操作するコント ローラーのアクションを許可します。 入力方向の要求データとアプリケーションのモデル間のマッピングは、モデル バインダーによって処理されます。 開発者は、カスタム モデル バインダー (ただし、通常、独自のプロバイダーを記述する必要はありません) を実装することによって、組み込みのモデル バインディング機能を拡張できます。

[GitHub から表示またはダウンロードのサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>既定のモデル バインダーの制限事項

既定のモデル バインダーは、ほとんどの一般的な .NET Core データ型をサポートし、ほとんどの開発者のニーズを満たす必要があります。 テキスト ベースの入力を要求の種類のモデルに直接バインドを享受できます。 それをバインドする前に、入力を変換する必要があります。 たとえばがある場合、モデル データの検索に使用できるキー。 キーに基づいてデータをフェッチするのにカスタム モデル バインダーを使用することができます。

## <a name="model-binding-review"></a>モデル バインディングのレビュー

モデル バインドは、上で動作の種類に固有の定義を使用します。 A*単純型*が入力内の 1 つの文字列から変換されます。 A*複合型*が複数の入力値から変換されます。 フレームワークの存在に基づいて差を決定する、`TypeConverter`です。 単純ながある場合に実行する型コンバーターを作成することをお勧めします。 `string`  ->  `SomeType`外部リソースを必要としないマッピングします。

独自のカスタム モデル バインダーを作成する前にどのように既存のモデルを確認する価値のバインダーが実装されているをお勧めします。 検討してください、 [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)これは、base64 でエンコードされた文字列をバイト配列に変換を使用できます。 バイト配列は多くの場合、ファイルまたはデータベース BLOB フィールドとして格納されます。

### <a name="working-with-the-bytearraymodelbinder"></a>ByteArrayModelBinder の操作

バイナリ データを表す、Base64 でエンコードされた文字列を使用できます。 たとえば、次の図は、文字列としてエンコードできます。

![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")

エンコードされた文字列の一部は、次の図で示されます。

![エンコードされた dotnet bot](custom-model-binding/images/encoded-bot.png "dotnet bot エンコード")

指示に従って、[サンプルの README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)を base64 でエンコードされた文字列をファイルに変換します。

ASP.NET Core MVC の base64 でエンコードされた文字列を撮影して使用することができます、`ByteArrayModelBinder`バイト配列に変換します。 [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider)を実装する[IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider)マップ`byte[]`引数`ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

独自のカスタム モデル バインダーを作成するときにすることができます独自に実装`IModelBinderProvider`入力するかを使用して、 [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute)です。

使用する次の例に示します`ByteArrayModelBinder`を base64 でエンコードされた文字列に変換する、`byte[]`し、結果をファイルに保存します。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

などのツールを使用してこの api メソッドに base64 でエンコードされた文字列を投稿する[Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

バインダーは、要求データを適切に名前付きプロパティまたは引数をバインドできます、限りモデル バインドは成功します。 次の例は、使用する方法を示しています。`ByteArrayModelBinder`ビュー モデルを含む。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>カスタム モデル バインダーのサンプル

このセクションで、カスタム モデル バインダーを実装します。

- 受信した要求データを厳密に型指定されたキーの引数に変換します。
- Entity Framework のコアを使用すると、関連するエンティティをフェッチします。
- 関連するエンティティを引数としてアクション メソッドに渡します。

次のサンプルは、`ModelBinder`属性を`Author`モデル。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

上記のコードでは、`ModelBinder`属性の型を指定`IModelBinder`バインドを使用する必要のある`Author`アクションのパラメーターです。 

`AuthorEntityBinder`に連結するため、`Author`エンティティ フレームワークのコアを使用してデータ ソースからエンティティをフェッチしてパラメーターと`authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

次のコードを使用する方法を示しています、`AuthorEntityBinder`アクション メソッドで。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder`を適用する属性を使用することができます、`AuthorEntityBinder`既定の規則を使用していないパラメーターにします。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

引数の名前が既定値はありませんので、この例では`authorId`、パラメーターを使用して、指定されている`ModelBinder`属性。 コント ローラーとアクションの両方のメソッドは、アクション メソッド内のエンティティの検索と比較して簡素化することに注意してください。 Entity Framework のコアを使用して、作成者をフェッチするためのロジックは、モデル バインダーに移動されます。 作成者モデルにバインドし、次に役立つことがいくつかのメソッドがあるときはかなり単純化できます、[ドライ原則](http://deviq.com/don-t-repeat-yourself/)です。

適用することができます、`ModelBinder`属性個々 のモデルのプロパティを (など、viewmodel で) またはアクション メソッドのパラメーターを特定のモデル バインダーまたはその型またはアクションだけのモデル名を指定します。

### <a name="implementing-a-modelbinderprovider"></a>ModelBinderProvider を実装します。

実装する属性を適用するには、代わりに`IModelBinderProvider`です。 これは、組み込みフレームワーク バインダーの実装方法です。 型を指定する場合に、バインダーが動作するか、それによって生成される値の引数の型を指定する**いない**入力、バインダーを受け入れます。 次のバインダー プロバイダーが連動、`AuthorEntityBinder`です。 使用する必要はありません、プロバイダーの MVC のコレクションに追加されたとき、`ModelBinder`属性に`Author`または`Author`パラメーターを入力します。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> 注: 上記のコードを返します、`BinderTypeModelBinder`です。 `BinderTypeModelBinder`モデル バインダーのファクトリとして機能し、依存性の注入 (DI) を提供します。 `AuthorEntityBinder` DI EF コアにアクセスする必要があります。 使用して`BinderTypeModelBinder`DI からサービスが、モデル バインダーに必要な場合です。

カスタム モデル バインダー プロバイダーを使用する追加の`ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

モデル バインダーを評価するときに、プロバイダーのコレクションは順序どおりにチェックします。 バインダーを返します最初のプロバイダーが使用されます。

次の図は、デバッガーからのモデル バインダーに既定値を示します。

![既定のモデル バインダー](custom-model-binding/images/default-model-binders.png "既定のモデル バインダー")

コレクションの末尾には、プロバイダーを追加すると、組み込みのモデル バインダー前に、カスタムのバインダーに呼び出される可能性があります。 この例では、カスタムのプロバイダーがの使用されることを確認するコレクションの先頭に追加される`Author`アクションの引数。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>推奨事項とベスト プラクティス

カスタム モデル バインダー。
- ステータス コードを設定または結果を返すを試行しないでください (たとえば、404 Not Found)。 モデル バインドに失敗した場合、[アクション フィルター](xref:mvc/controllers/filters)自体アクション メソッド内のロジックは、エラーを処理する必要がありますか。
- コードの繰り返しとアクション メソッドから横断的関心事を排除する最も便利です。
- 通常、カスタムの型を文字列に変換に使用しないで、 [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter)は通常より良いオプション。
