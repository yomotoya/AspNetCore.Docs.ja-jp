---
title: ASP.NET Core のイメージ タグ ヘルパー
author: pkellner
description: イメージ タグ ヘルパーを使用する方法を示します
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="image-tag-helper-in-aspnet-core"></a>ASP.NET Core のイメージ タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net) 

イメージ タグ ヘルパーは `img` (`<img>`) タグを拡張します。 `src` タグと `boolean` 属性 `asp-append-version` が必要です。

イメージ ソース (`src`) がホスト Web サーバーの静的ファイルである場合、一意のキャッシュ バスティング文字列がイメージ ソースにクエリ パラメーターとし追加されます。 これにより、ホスト Web サーバーのファイルが変更された場合に、更新された要求パラメーターを含む一意の要求 URL が確実に生成されます。 キャッシュ バスティング文字列は、静的なイメージ ファイルのハッシュを表す一意の値です。

イメージ ソース (`src`) が静的ファイルでない場合 (たとえば、リモート URL またはファイルがサーバーに存在しない場合)、`<img>` タグの `src` 属性はキャッシュ バスティング クエリ文字列パラメーターなしで生成されます。

## <a name="image-tag-helper-attributes"></a>イメージ タグ ヘルパーの属性


### <a name="asp-append-version"></a>asp-append-version

`src` 属性と共に指定されている場合、イメージ タグ ヘルパーが呼び出されます。

有効な `img` タグ ヘルパーの例を以下に示します。

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

静的ファイルがディレクトリ *..wwwroot/images/asplogo.png* に存在する場合、生成された html は次のようになります (ハッシュは異なります)。

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

パラメーター `v` に割り当てられた値は、ディスク上のファイルのハッシュ値です。 Web サーバーが参照される静的ファイルに対する読み取りアクセスを取得できない場合、`v` パラメーターは `src` 属性に追加されません。

- - -

### <a name="src"></a>src

イメージ タグ ヘルパーをアクティブにするには、`<img>` 要素に src 属性が必要です。 

> [!NOTE]
> イメージ タグ ヘルパーはローカル Web サーバーで `Cache` プロバイダーを使用して、特定のファイルの計算された `Sha512` を格納します。 ファイルが再度要求された場合、`Sha512` は再計算する必要はありません。 キャッシュは、ファイルの `Sha512` が計算されたときにファイルにアタッチされるファイル ウォッチャーによって無効になります。

## <a name="additional-resources"></a>その他の技術情報

* <xref:performance/caching/memory>
