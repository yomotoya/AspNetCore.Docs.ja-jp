---
title: Razor のコンポーネントのクラス ライブラリ
author: guardrex
description: コンポーネントを含める、外部コンポーネント ライブラリの Razor コンポーネント アプリにする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515463"
---
# <a name="razor-components-class-libraries"></a>Razor のコンポーネントのクラス ライブラリ

によって[Simon Timms](https://github.com/stimms)

コンポーネントは、Razor クラス ライブラリ プロジェクト間で共有できます。 コンポーネントから指定できます。

* ソリューション内の別のプロジェクト。
* NuGet パッケージ。
* 参照先の .NET ライブラリです。

コンポーネントが通常の .NET 型であるのと同様、Razor クラス ライブラリによって提供されるコンポーネントは、通常の .NET アセンブリをします。

使用して、 `razorclasslib` (Razor クラス ライブラリ) テンプレートと、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンド。

```console
dotnet new razorclasslib -o MyComponentLib1
```

コンポーネントの Razor ファイルの追加 (*.razor*) Razor クラス ライブラリにします。

既存のプロジェクトにライブラリを追加するには、使用、 [dotnet sln](/dotnet/core/tools/dotnet-sln)コマンド。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Razor クラス ライブラリは、ASP.NET Core の Preview 3 で Blazor アプリと互換性がありません。
>
> Blazor と剃刀コンポーネント アプリで共有できるライブラリのコンポーネントを作成するには、によって作成された Blazor クラス ライブラリを使用、`blazorlib`テンプレート。
>
> Razor クラス ライブラリは、静的なアセットを ASP.NET Core の Preview 3 でサポートされていません。 コンポーネントのライブラリを使用して、`blazorlib`テンプレートは、イメージ、JavaScript、およびスタイル シートなどの静的ファイルを含めることができます。 ビルド時に、静的ファイルがビルドされたアセンブリ ファイルに埋め込まれる (*.dll*)、そのリソースを追加する方法の詳細について心配することがなく、コンポーネントの消費量をことができます。 含まれるすべてのファイル、`content`ディレクトリは、埋め込みリソースとしてマークされます。

## <a name="consume-a-library-component"></a>ライブラリのコンポーネントを使用します。

別のプロジェクトでのライブラリで定義されているコンポーネントを使用するために、 [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label)ディレクティブを使用する必要があります。 名前では、個々 のコンポーネントを追加する可能性があります。

ディレクティブの一般的な形式です。

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

たとえば、次のディレクティブが追加されます`Component1`の`MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

ただし、共通するすべてのワイルドカードを使用してアセンブリからコンポーネントが含まれますが (`*`)。

```cshtml
@addTagHelper *, MyComponentLib1
```

`@addTagHelper`にディレクティブを含めることが *_ViewImport.cshtml*コンポーネントを 1 つのページまたはフォルダー内のページのセットにプロジェクト全体の使用または適用します。 `@addTagHelper`インプレース ディレクティブ、コンポーネント ライブラリのコンポーネント使用できるように格納されている場合、アプリと同じアセンブリ。

## <a name="build-pack-and-ship-to-nuget"></a>ビルド、パック、および NuGet に出荷

コンポーネント ライブラリは、標準の .NET ライブラリであるために、パッケージ化し、それらを NuGet はパッケージ化と配布の任意のライブラリを NuGet 異なりません。 使用してパッケージを実行、 [dotnet pack](/dotnet/core/tools/dotnet-pack)コマンド。

```console
dotnet pack
```

NuGet を使用するパッケージをアップロード、 [dotnet nuget 発行](/dotnet/core/tools/dotnet-nuget-push)コマンド。

```console
dotnet nuget publish
```

使用する場合、`blazorlib`テンプレート、静的なリソースが、NuGet パッケージに含まれています。 コンシューマーは、リソースを手動でインストールする必要のないように、スクリプトやスタイル シート、ライブラリ使用者は自動的に受け取ります。
