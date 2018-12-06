---
title: Web API 規約を使用する
author: pranavkm
description: ASP.NET Core の Web API 規約について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710076"
---
# <a name="use-web-api-conventions"></a>Web API 規約を使用する

ASP.NET Core 2.2 で、一般的な [API 文書](xref:tutorials/web-api-help-pages-using-swagger)を抽出し、これを複数のアクション、コントローラー、またはアセンブリ内のすべてのコントローラーに適用する方法を導入しました。 Web API 規約は、[[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) による個々のアクションの装飾の代わりになるものです。 アクションに適用される規約のメソッドを選択して、アクションから戻る特に一般的な "従来の" 戻り値の型と状態コードを定義できるようになります。

既定では、ASP.NET Core MVC 2.2 には一連の既定の規約 `Microsoft.AspNetCore.Mvc.DefaultApiConventions` が付属しています。 この規約は、ASP.NET Core がスキャフォールディングするコントローラーに基づいています。 アクションが成果物をスキャフォールディングするパターンに従う場合は、既定の規約を使用すると成功します。

実行時に、<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> は規約を理解します。 `ApiExplorer` は Open API ドキュメントのジェネレーターと通信するために MVC を抽象化したものです。 適用された規約の属性はアクションと関連付けられ、アクションの Swagger ドキュメントに含まれます。 API アナライザーも、規約を理解します。 従来とは異なるアクションである場合 (適用されている規約で文書化されていない状態コードを返す場合など) は、文書化を促す警告が生成されます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="apply-web-api-conventions"></a>Web API 規約を適用する

規約を適用する方法は 3 つあります。 規約では作成を行われません。 各アクションを 1 つの規約だけに関連付けることができます。 より具体的な規約 (詳細は下記) の方が優先されます。 優先順位が同じである 2 つ以上の規約が 1 つのアクションに適用されていると、選択は不明確になります。 次のオプションは、規約をアクションに適用するためにあります。より具体的なものから並んでいます。

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; 個々のアクションに適用され、適用される規約の種類と規約のメソッドが指定されます。 次の例では、規約のメソッド `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` が `Update` のアクションに適用されます。

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` (コントローラーに適用) &mdash; 規約の種類がコントローラー上のすべてのアクションに適用されます。 規約のメソッドは、どのアクションが適用されるかを決定するヒントで装飾されます (詳細は規約の作成に含まれています)。 例:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` (アセンブリに適用) &mdash; 規約の種類が現在のアセンブリのすべてのコントローラーに適用されます。 例:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Web API 規約を作成する

規約は、静的な種類でメソッドが含まれています。 これらのメソッドには `[ProducesResponseType]` または `[ProducesDefaultResponseType]` 属性の注釈が付けられています。

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

この規約をアセンブリに適用すると、より具体的なメタデータ属性がない限り、規約のメソッドが `Find` という名前のすべてのアクションに適用され、`id` という名前のパラメーターを 1 つだけ持つようになります。

`[ProducesResponseType]` と `[ProducesDefaultResponseType]` だけでなく、`[ApiConventionNameMatch]` と `[ApiConventionTypeMatch]` も、適用されるメソッドを決定する規約のメソッドに適用することができます。 例:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* メソッドに適用された `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` オプションは、"Find" というプレフィックスがある限り、規約をどのアクションとも照合できることを示します。 これには、`Find`、`FindPet`、`FindById` などのメソッドが含まれます。
* パラメーターに適用された `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` は、サフィックス識別子で終わる 1 つのパラメーターが含まれるメソッドを規約と照合できることを示します。 これには、`id` や `petId` などのパラメーターが含まれます。 `ApiConventionTypeMatch` も同様に型に適用して、パラメーターの型に制約を設けることができます。 `params[]` の引数を使用すると、明示的に一致する必要がない残りのパラメーターを示すことができます。
