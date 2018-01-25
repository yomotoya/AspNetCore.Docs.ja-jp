---
title: "画像のタグ ヘルパー |Microsoft ドキュメント"
author: pkellner
description: "イメージ タグ ヘルパーを使用する方法を示しています。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: d0857e1926c341b2357bc824fa379c4fc30affbc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

著者: [Peter Kellner](http://peterkellner.net) 

イメージ タグ ヘルパーの強化、 `img` (`<img>`) タグ。 必要な`src`タグだけでなく`boolean`属性`asp-append-version`です。

場合、イメージ ソース (`src`) 静的なファイルでは、ホスト web サーバーでは、文字列を破壊一意のキャッシュ、画像のソースにクエリ パラメーターとして付加されます。 これにより、ホストの web サーバー上のファイルが変更された場合、一意の要求 URL が生成されることを更新された要求パラメーターを含みます。 文字列を破壊キャッシュは、静的なイメージ ファイルのハッシュを表す一意の値です。

場合、イメージ ソース (`src`) は (たとえばリモート URL またはファイル サーバーに存在しません、) 静的ファイルではありません、`<img>`タグの`src`クエリ文字列パラメーターを破壊キャッシュなしの属性を生成します。

## <a name="image-tag-helper-attributes"></a>イメージ タグ ヘルパー属性


### <a name="asp-append-version"></a>asp-append-version

と共に指定されている場合、`src`イメージ タグ ヘルパーの属性が呼び出されます。

有効な例`img`タグ ヘルパーは。

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

静的ファイルがディレクトリに存在する場合は*.wwwroot/images/asplogo.png*生成された html が (ハッシュは異なるされます)、次のようにします。

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

パラメーターに割り当てられた値`v`ディスク上のファイルのハッシュ値です。 Web サーバーがへの読み取りアクセスを取得できない場合、静的なファイル参照、いいえ`v`にパラメーターを追加、`src`属性。

- - -

### <a name="src"></a>src

イメージ タグ ヘルパーをアクティブ化する src 属性が必要な`<img>`要素。 

> [!NOTE]
> イメージ タグ ヘルパーを使用して、 `Cache` 、集計を格納するローカル web サーバー上のプロバイダー`Sha512`の指定されたファイル。 ファイルが再度要求される場合、`Sha512`再計算する必要はありません。 ファイルに関連付けられているファイルの監視でキャッシュが無効になりませんときに、ファイルの`Sha512`計算されます。

## <a name="additional-resources"></a>その他の技術情報

* <xref:performance/caching/memory>
