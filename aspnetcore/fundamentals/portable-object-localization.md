---
title: "ローカライズの移植性のオブジェクトを構成します。"
author: sebastienros
description: "この記事では、ポータブル オブジェクト ファイルを紹介し、Orchard のコア フレームワークと ASP.NET Core アプリケーションで使用するための手順の概要を示します。"
keywords: "ASP.NET Core、ローカリゼーション、カルチャ、言語、ポータブル オブジェクト"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Orchard コアでポータブル オブジェクト ローカリゼーションを構成します。

によって[Sébastien Ros](https://github.com/sebastienros)と[Scott Addie](https://twitter.com/Scott_Addie)

この記事で ASP.NET Core アプリケーションのポータブル オブジェクト (PO) ファイルを使用するための手順を追って、 [Orchard コア](https://github.com/OrchardCMS/OrchardCore)フレームワークです。

**注:** Orchard コアは、Microsoft 製品ではありません。 したがって、マイクロソフトはサポートされませんこの機能します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-a-po-file"></a>PO ファイルとは何ですか。

PO ファイルは、特定の言語の翻訳された文字列を含むテキスト ファイルとして配布します。 PO ファイルを代わりに使用するいくつか利点*.resx*ファイルが含まれます。
- PO ファイルは、複数形化; をサポートします。*.resx*複数形化をサポートしていないファイルです。
- PO ファイルはのようにコンパイルされない*.resx*ファイル。 そのため、特別なツールとビルド手順は必要ありません。
- PO ファイルは、オンライン編集ツールを共同でうまく機能します。

### <a name="example"></a>例

フランス語、その複数形の 1 つを含む 2 つの文字列の翻訳を含むサンプル PO ファイルを次に示します。

*fr.po*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

この例では、次の構文を使用します。

- `#:`: 変換対象の文字列のコンテキストを示すコメントです。 同じ文字列が使用されているに応じて異なる方法で変換可能性があります。
- `msgid`: 無変換文字列。
- `msgstr`: 翻訳された文字列。

複数形化のサポートの場合は、複数のエントリを定義できます。

- `msgid_plural`: 無変換複数形文字列。
- `msgstr[0]`: 0 の場合、翻訳された文字列。
- `msgstr[N]`: 大文字の n。 の翻訳された文字列

PO ファイルの仕様を参照して[ここ](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html)です。

## <a name="configuring-po-file-support-in-aspnet-core"></a>ASP.NET Core の PO ファイル サポートの構成

この例は、Visual Studio 2017 プロジェクト テンプレートから生成された ASP.NET Core MVC アプリケーションに基づいています。

### <a name="referencing-the-package"></a>パッケージを参照します。

参照を追加、 `OrchardCore.Localization.Core` NuGet パッケージです。 使用可能になる[MyGet](https://www.myget.org/)次のパッケージ ソースで: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

*.Csproj*ファイルにはここで、次のような行が含まれています (バージョン番号が異なる場合があります)。

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>サービスを登録します。

必要なサービスを追加、`ConfigureServices`メソッドの*Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

必要なミドルウェアを追加、`Configure`メソッドの*Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

選択肢の Razor ビューには、次のコードを追加します。 *About.cshtml*は、この例で使用します。

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

`IViewLocalizer`インスタンスが挿入され、テキスト"Hello world!"を変換するために使用します。

### <a name="creating-a-po-file"></a>PO ファイルを作成します。

という名前のファイルを作成する *<culture code>.po*アプリケーション ルート フォルダー。 この例では、ファイル名は*fr.po*フランス語の言語が使用されるため。

[!code-text[Main](localization/sample/POLocalization/fr.po)]

このファイルは、変換する文字列とフランス語に翻訳された文字列の両方を格納します。 翻訳は、必要な場合、その親カルチャに戻します。 この例では、 *fr.po*カルチャが要求された場合、ファイルが使用される`fr-FR`または`fr-CA`です。

### <a name="testing-the-application"></a>アプリケーションのテスト

アプリケーションを実行し、URL に移動して`/Home/About`です。 テキスト**Hello world!** 表示されます。

URL に移動`/Home/About?culture=fr-FR`です。 テキスト**Bonjour le monde!** 表示されます。

## <a name="pluralization"></a>複数形化

PO ファイルは、複数形化フォームをサポートし、これは、文字列と同じする必要がある変換基数に応じて異なる場合に役立ちます。 このタスクは、各言語が基数に基づいて、使用する文字列を選択するカスタム ルールを定義するため、複雑なが作成されます。

Orchard ローカリゼーション パッケージでは、異なるこれら複数形のフォームを自動的に起動する API を提供します。

### <a name="creating-pluralization-po-files"></a>複数形化 PO ファイルの作成

次の内容を追加する前に説明した*fr.po*ファイル。

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

参照してください[PO ファイルは?](#what-is-a-po-file)この例では、各エントリが表す内容の説明。

### <a name="adding-a-language-using-different-pluralization-forms"></a>異なる複数形化フォームを使用する言語を追加します。

英語とフランス語の文字列は、前の例で使用されました。 英語とフランス語複数形化の 2 つの形式があり、同じ形式の規則を共有するいずれかの基数が最初の複数形にマップされています。 その他の任意の基数は、2 番目の複数形にマップされます。

すべての言語では、同じ規則を共有します。 これを複数形の 3 つの形式を持つ、チェコ語の言語を示します。

作成、`cs.po`次のように、ファイルし、複数形化が次の 3 つの異なる翻訳必要がありますに注意してください。

[!code-text[Main](localization/sample/POLocalization/cs.po)]

チェコ語のローカライズ版は、そのまま追加`"cs"`でサポートされているカルチャの一覧に、`ConfigureServices`メソッド。

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

編集、 *Views/Home/About.cshtml*いくつかの基数のローカライズされた、複数形の文字列をレンダリングするファイル。

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**注:**カウントを表すに実際のシナリオで変数が使用します。 ここでは、非常に特殊なケースを公開する 3 つの異なる値を使用して同じコードを繰り返します。

カルチャを切り替えると、次をご覧ください。

`/Home/About`の場合:

```html
There is one item.
There are 2 items.
There are 5 items.
```

`/Home/About?culture=fr`の場合:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

`/Home/About?culture=cs`の場合:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

チェコ語のカルチャの 3 つの翻訳が異なることに注意してください。 フランス語と英語のカルチャでは、2 つの最後の翻訳された文字列を同じ構造を共有します。

## <a name="advanced-tasks"></a>高度なタスク

### <a name="contextualizing-strings"></a>文字列を付けた

アプリケーションは、多くの場合、複数の場所で変換するために、文字列を含んでいます。 同じ文字列では、アプリ (Razor ビューまたはクラス ファイル) 内の特定の場所に別の翻訳があります。 PO ファイルは、表される文字列の分類に使用することができます、ファイルのコンテキストの概念をサポートします。 ファイルのコンテキストを使用して、文字列を翻訳できますが異なり、ファイルのコンテキスト (またはファイルのコンテキストが不足している) によって異なります。

PO ローカリゼーション サービスは、完全クラスまたは文字列を変換するときに使用されるビューの名前を使用します。 これは、値の設定、`msgctxt`エントリです。

前に、マイナーの追加を検討してください*fr.po*例です。 Razor ビューにある*Views/Home/About.cshtml*予約を設定して、ファイルのコンテキストとして定義できる`msgctxt`エントリの値。

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

`msgctxt`よう設定すると、テキストが変換に移動するとき`/Home/About?culture=fr-FR`です。 移動するときに、変換が発生しない`/Home/Contact?culture=fr-FR`です。

指定されたファイルのコンテキストで特定のエントリが一致しないときに、Orchard コアの代替機構は、コンテキストなしの適切なの PO ファイルを探します。 に対して定義されている特定のファイル コンテキストがあると仮定した場合は*Views/Home/Contact.cshtml*に間を移動する、`/Home/Contact?culture=fr-FR`など PO ファイルを読み込みます。

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>PO ファイルの場所を変更します。

PO ファイルの既定の場所で変更できます`ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

この例では、PO ファイルが読み込まれた、*ローカリゼーション*フォルダーです。

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>ローカライズされたファイルを検索するためのカスタム ロジックを実装します。

PO ファイルを検索するより複雑なロジックが必要なときに、`OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider`インターフェイスは実装されており、サービスとして登録されていることができます。 これは、PO ファイルは、さまざまな場所に保存することができる場合、またはファイルをフォルダー階層内にある必要があるときに便利です。

### <a name="using-a-different-default-pluralized-language"></a>複数化ごとに異なる既定言語を使用してください。

パッケージに含まれる、`Plural`は複数形の 2 つの形式に固有の拡張メソッド。 他の複数形のフォームを必要とする言語では、拡張メソッドを作成します。 既定の言語のすべてのローカライズ ファイルを提供する必要はありません、拡張メソッドを持つ&mdash;元の文字列は既に、コード内で直接使用できます。

汎用的なを使用する`Plural(int count, string[] pluralForms, params object[] arguments)`を翻訳の文字列配列を受け入れるオーバー ロードします。