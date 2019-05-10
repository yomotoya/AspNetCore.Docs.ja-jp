---
title: ASP.NET Core でのデータ保護 Api の概要します。
author: rick-anderson
description: ASP.NET Core データ保護 Api を使用して保護すると、アプリのデータを復号化する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087640"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>ASP.NET Core でのデータ保護 Api の概要します。

<a name="security-data-protection-getting-started"></a>

その最も簡単な保護データには、次の手順で構成されます。

1. データ保護プロバイダーからのデータ保護機能を作成します。

2. 呼び出す、`Protect`を保護するデータを持つメソッド。

3. 呼び出す、`Unprotect`をプレーン テキストに戻す有効にするデータを持つメソッド。

ほとんどのフレームワークと ASP.NET Core SignalR などのアプリ モデルは既にデータ保護システムを構成し、依存関係の挿入を使用してアクセスするサービス コンテナーに追加します。 次の例では、依存関係の挿入サービス コンテナーを構成して、データ保護スタックの登録、DI を使用してデータ保護プロバイダーの受信、保護機能と保護し、保護を解除するデータの作成を示します。

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

プロテクターを作成するときに、1 つまたは複数を指定する必要があります[目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)します。 目的の文字列は、消費者間の分離を提供します。 たとえば、「緑色」の目的の文字列で作成された保護機能は、「紫」の目的を持つプロテクターによって提供されるデータの保護を解除できないでしょう。

>[!TIP]
> インスタンス`IDataProtectionProvider`と`IDataProtector`は複数の呼び出し元のスレッド セーフです。 意図したものをコンポーネントへの参照を取得したら、`IDataProtector`呼び出しに`CreateProtector`、複数の呼び出しの参照が使用されます`Protect`と`Unprotect`します。
>
>呼び出し`Unprotect`CryptographicException 保護されたペイロードの検証または解読できない場合がスローされます。 一部のコンポーネントがエラーを無視することが中に保護を解除します。認証 cookie を読み取るコンポーネント可能性がありますこのエラーを処理しがなかった cookie すべての場合と、要求を処理ではなくも、最初からの要求は失敗します。 この動作をするコンポーネントは、すべての例外の飲み込みではなく CryptographicException を明示的にキャッチする必要があります。
