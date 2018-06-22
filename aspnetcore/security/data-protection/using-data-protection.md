---
title: ASP.NET Core のデータ保護 Api の概要します。
author: rick-anderson
description: 保護して、アプリ内のデータを復号化の ASP.NET Core データ保護 Api を使用する方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: ab2551d87d1a2cd22e9f421cabe0288311cb2ec3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275750"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>ASP.NET Core のデータ保護 Api の概要します。

<a name="security-data-protection-getting-started"></a>

その最も簡単な保護データで、次の手順で構成されます。

1. データ保護プロバイダーからのデータ保護機能を作成します。

2. 呼び出す、`Protect`を保護するデータを持つメソッドです。

3. 呼び出す、`Unprotect`プレーン テキストに戻るに変換するデータを持つメソッドです。

ほとんどのフレームワークと ASP.NET SignalR などのアプリ モデルは既にデータ保護システムを設定し、依存関係の挿入を使用してアクセスするサービス コンテナーに追加します。 次の例は、依存関係の挿入のサービス コンテナーを構成して、データ保護スタックの登録、DI 経由でデータ保護プロバイダーを受信、保護機能と保護し、保護を解除するデータの作成

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

プロテクターを作成する場合は、1 つ以上を指定する必要があります[目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)です。 目的の文字列は、消費者間の分離を提供します。 たとえば、「緑」の目的の文字列で作成された、保護機能できなくなります「紫」の目的を使用して、保護機能によって提供されるデータの保護を解除します。

>[!TIP]
> インスタンス`IDataProtectionProvider`と`IDataProtector`は複数の呼び出し元のスレッド セーフです。 目的としていること、コンポーネントへの参照を取得したら、`IDataProtector`呼び出しを経由して`CreateProtector`、その参照に複数の呼び出しは使用`Protect`と`Unprotect`です。
>
>呼び出し`Unprotect`保護されているペイロードを検証または解読できない場合 CryptographicException がスローされます。 一部のコンポーネントがエラーを無視することも中に保護を解除します。認証 cookie が読み取られますコンポーネント可能性がありますこのエラーを処理およびこれがなかった cookie まったくかのように、要求を処理ではなく完全要求に失敗します。 この動作をするコンポーネントは、すべての例外を受け入れるではなく CryptographicException をキャッチする必要があります具体的には。
