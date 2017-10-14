---
title: "データ保護 Api を使用した作業の開始"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>データ保護 Api を使用した作業の開始

<a name="security-data-protection-getting-started"></a>

その最も簡単なデータの保護では、次の手順で構成されます。

1. データ保護プロバイダーからのデータ保護機能を作成します。

2. 保護するデータを保護するメソッドを呼び出します。

3. プレーン テキストに復帰するデータの保護の解除メソッドを呼び出します。

既に ASP.NET または SignalR などほとんどのフレームワークは、データ保護システムを構成し、依存関係の挿入を使用してアクセスするサービス コンテナーに追加します。 次の例は、依存関係の挿入のサービス コンテナーを構成して、データ保護スタックの登録、DI 経由でデータ保護プロバイダーを受信、保護機能と保護し、保護を解除するデータの作成

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

プロテクターを作成する場合は、1 つ以上を指定する必要があります[目的文字列](consumer-apis/purpose-strings.md)です。 目的の文字列は、消費者間の分離を提供、たとえば「緑」の目的の文字列で作成された、保護機能では、「紫」の目的を使用して、保護機能によって提供されるデータの保護を解除できません。 します。

>[!TIP]
> IDataProtectionProvider と IDataProtector のインスタンスが、スレッド セーフである複数の呼び出し元のです。 コンポーネントでは、呼び出し CreateProtector に IDataProtector への参照を取得、いったんを使用することを参照する protect に複数の呼び出しのためのものです。
>
>保護の解除の呼び出しが、CryptographicException を保護されているペイロードを検証または解読できない場合にスローされます。 一部のコンポーネントがエラーを無視することも中に保護を解除します。認証 cookie が読み取られますコンポーネント可能性がありますこのエラーを処理およびこれがなかった cookie まったくかのように、要求を処理ではなく完全要求に失敗します。 この動作をするコンポーネントは、すべての例外を受け入れるではなく CryptographicException をキャッチする必要があります具体的には。
