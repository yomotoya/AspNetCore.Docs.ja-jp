---
title: "コンシューマー Api の概要"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a>コンシューマー Api の概要

IDataProtectionProvider と IDataProtector インターフェイスは、コンシューマーを使用するには、データ保護システムを使用して基本的なインターフェイスです。 Microsoft.AspNetCore.DataProtection.Abstractions パッケージ内にあります。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

プロバイダーのインターフェイスでは、データ保護システムのルートを表します。 保護やデータ保護の解除を直接使用できません。 代わりに、コンシューマーは、IDataProtectionProvider.CreateProtector(purpose)、ここで、目的は、目的のコンシューマーのユース ケースを説明する文字列を呼び出すことによって IDataProtector への参照を取得する必要があります。 参照してください[目的文字列](purpose-strings.md)を目的として、このパラメーターの適切な値を選択する方法についてははるかにします。

## <a name="idataprotector"></a>IDataProtector

プロテクター インターフェイスが CreateProtector への呼び出しによって返され、は、コンシューマーは、実行に使用できるこのインターフェイスは、保護し、操作の保護を解除します。

一部のデータを保護するには、保護するメソッドにデータを渡します。 どの変換 byte[] -> byte[]、メソッドを定義する基本インターフェイスですがありますも、オーバー ロード (拡張メソッドとして提供) の文字列に変換する文字列]-> [です。 2 つの方法で提供されるセキュリティは同じです。開発者は、いずれかのオーバー ロードは、ユース ケースに対して最も便利な方法を選択してください。 選択すると、オーバー ロードの保護によって返される値に関係なくメソッドはようになりました (enciphered および不正使用防止機能も使用可能な)、および保護アプリケーションを使用すると、信頼されていないクライアントに送信できます。

保護されていた一部のデータの保護を解除するには、保護の解除メソッドに、保護されたデータを渡します。 (Byte[] がある-開発者の利便性のためのベースと文字列ベースのオーバー ロードします)。保護されたペイロードは、この同じ IDataProtector を保護するに旧の呼び出しによって生成された、Unprotect メソッドは、元の保護されていないペイロードを返します。 保護されたペイロードは、改ざんされたまたは異なる IDataProtector によって生成された、Unprotect メソッド CryptographicException がスローされます。

異なる IDataProtector と同一の概念は、目的の概念に関連付けます。 IDataProtector の 2 つのインスタンスが同じルート IDataProtectionProvider からが IDataProtectionProvider.CreateProtector への呼び出しでそれぞれ異なる目的の文字列を使用して生成されたかどうかと見なされる[異なるプロテクター](purpose-strings.md)、し、もう一方によって生成されるペイロードの保護を解除する 1 つはできません。

## <a name="consuming-these-interfaces"></a>これらのインターフェイスを使用

DI 対応のコンポーネントの使用目的は、コンポーネントが、コンス トラクターで IDataProtectionProvider パラメーターを受け取るし、コンポーネントがインスタンス化されるとき、DI システムがそのこのサービスを自動的に提供します。

> [!NOTE]
> 一部のアプリケーション (コンソール アプリケーション、ASP.NET 4.x アプリケーションなど) があります DI に対応するため、ここで説明する機構を使用できません。 これらのシナリオを参照してください、[非 DI 対応したシナリオ](../configuration/non-di-scenarios.md)DI を経由せず IDataProtection プロバイダーのインスタンスを取得の詳細については、ドキュメントです。

次の例では、3 つの概念を示しています。

1. [データ保護システムに追加する](../configuration/overview.md)サービス コンテナー

2. DI を使用して、IDataProtectionProvider のインスタンスを取得し、

3. IDataProtectionProvider から IDataProtector を作成し、保護し、データ保護の解除に使用します。

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Microsoft.AspNetCore.DataProtection.Abstractions パッケージには、開発者便宜を図って IServiceProvider.GetDataProtector 拡張メソッドが含まれています。 これは、単一の操作を使用しているサービス プロバイダーからの IDataProtectionProvider の取得と IDataProtectionProvider.CreateProtector を呼び出すことの両方としてカプセル化します。 次の例では、その使用法を示します。

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> IDataProtectionProvider と IDataProtector のインスタンスが、スレッド セーフである複数の呼び出し元のです。 ものではコンポーネントが CreateProtector 呼び出しを経由して IDataProtector への参照を取得したら、その参照は保護する複数の呼び出しを使用し、Unprotect Unprotect.A 呼び出しは、保護されているペイロードができない場合は CryptographicException にスローされます。検証するか、データを解読します。 一部のコンポーネントがエラーを無視することも中に保護を解除します。認証 cookie が読み取られますコンポーネント可能性がありますこのエラーを処理およびこれがなかった cookie まったくかのように、要求を処理ではなく完全要求に失敗します。 この動作をするコンポーネントは、すべての例外を受け入れるではなく CryptographicException をキャッチする必要があります具体的には。
