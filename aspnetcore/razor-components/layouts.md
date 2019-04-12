---
title: Razor のコンポーネントのレイアウト
author: guardrex
description: Razor コンポーネント向けのレイアウトを再利用可能なコンポーネントを作成する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515643"
---
# <a name="razor-components-layouts"></a>Razor のコンポーネントのレイアウト

によって[Rainer Stropek](https://www.timecockpit.com)

通常、アプリには、1 つ以上のコンポーネントが含まれます。 メニューの著作権のメッセージ、およびロゴなどのレイアウト要素は、すべてのコンポーネントに存在する必要があります。 すべてのアプリのコンポーネントにこれらのレイアウト要素のコードをコピー、効率的な方法はありません。 このような重複では、管理するが難しいと、おそらく時間の経過と共に一貫性のないコンテンツにつながります。 *レイアウト*この問題を解決します。

技術的には、レイアウトには、もう 1 つのコンポーネントです。 Razor テンプレートまたはレイアウトが定義されているC#コードし、含めることができます[データ バインディング](xref:razor-components/components#data-binding)、[依存関係の注入](xref:razor-components/dependency-injection)、およびその他のコンポーネントの通常の機能。

その他の 2 つの側面が有効にする、*コンポーネント*に、*レイアウト*

* レイアウト コンポーネントが継承する必要があります`LayoutComponentBase`します。 `LayoutComponentBase` 定義、`Body`レイアウト内にレンダリングされるコンテンツを含むプロパティ。
* レイアウト コンポーネントを使用して、`Body`プロパティを本文の内容が表示されるはずの場所を指定する、Razor 構文を使用してレンダリング`@Body`します。 レンダリング中に`@Body`レイアウトのコンテンツで置き換えられます。

レイアウト コンポーネントの Razor テンプレートを次のコード サンプルに示します。 使用に注意してください`LayoutComponentBase`と`@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a>コンポーネントでのレイアウトを使用します。

Razor ディレクティブを使用して、`@layout`コンポーネントにレイアウトを適用します。 コンパイラは変換には、このディレクティブを`LayoutAttribute`、これコンポーネント クラスに適用されます。

次のコード サンプルでは、概念を示します。 このコンポーネントの内容が挿入、 *MasterLayout*の位置にある`@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>一元的なレイアウトの選択

すべてのフォルダーのアプリは、という名前のテンプレート ファイルを含めることができます必要に応じて *_ViewImports.cshtml*します。 コンパイラには、すべての同じフォルダーに Razor テンプレートとそのすべてのサブフォルダーで再帰的にビューのインポートのファイルで指定されたディレクティブが含まれています。 そのため、 *_ViewImports.cshtml*ファイルを含む`@layout MainLayout`でフォルダー内のコンポーネントのすべてのこと、 *MainLayout*レイアウト。 繰り返しを追加する必要はありません`@layout`のすべてに、 *.razor*ファイル。

既定のテンプレートを使用している、 *_ViewImports.cshtml*レイアウトの選択するためのメカニズムです。 新しく作成されたアプリが含まれる、 *_ViewImports.cshtml*ファイル、*コンポーネント、またはページ*フォルダー。

## <a name="nested-layouts"></a>入れ子になったレイアウト

アプリは、入れ子になったレイアウトで構成できます。 コンポーネントは、さらに別のレイアウトを参照するレイアウトを参照できます。 たとえば、入れ子のレイアウトは、複数レベルのメニュー構造を反映するために使用できます。

次のコード サンプルでは、入れ子になったレイアウトを使用する方法を示します。 *EpisodesComponent.cshtml*ファイルは、コンポーネントを表示します。 コンポーネントがレイアウトを参照することに注意してください`MasterListLayout`します。

*EpisodesComponent.cshtml*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

*MasterListLayout.cshtml*ファイルは、提供、`MasterListLayout`します。 レイアウトは、別のレイアウトを参照`MasterLayout`埋め込まれる進んでいます。

*MasterListLayout.cshtml*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

最後に、`MasterLayout`ヘッダー、フッター、およびメイン メニューなど、最上位のレイアウト要素が含まれています。

*MasterLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
