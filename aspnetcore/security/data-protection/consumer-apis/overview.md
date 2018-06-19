---
title: ASP.NET Core のコンシューマー Api の概要
author: rick-anderson
description: さまざまなコンシューマー ASP.NET Core data protection ライブラリ内で利用可能な Api の概要が表示されます。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 5d161ed8fbc39bcf4a970644480b4e909810b555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30076008"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>ASP.NET Core のコンシューマー Api の概要

`IDataProtectionProvider`と`IDataProtector`インターフェイスは、コンシューマーを使用するには、データ保護システムを使用して、基本インターフェイスです。 ファイルに配置している、 [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)パッケージです。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

プロバイダーのインターフェイスでは、データ保護システムのルートを表します。 保護やデータ保護の解除を直接使用できません。 代わりに、コンシューマーがへの参照を取得する必要があります、`IDataProtector`を呼び出して`IDataProtectionProvider.CreateProtector(purpose)`ここで、目的は、目的のコンシューマーのユース ケースを説明する文字列。 参照してください[目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)を目的として、このパラメーターの適切な値を選択する方法についてははるかにします。

## <a name="idataprotector"></a>IDataProtector

保護機能のインターフェイスへの呼び出しによって返される`CreateProtector`とそのコンシューマーは、実行に使用できるこのインターフェイスは、保護し、操作の保護を解除します。

一部のデータを保護するデータの渡し、`Protect`メソッドです。 どの変換 byte[] -> byte[]、メソッドを定義する基本インターフェイスですがありますが、オーバー ロード (拡張メソッドとして提供) の文字列に変換する文字列]-> [もします。 2 つの方法で提供されるセキュリティは同じです。開発者は、いずれかのオーバー ロードは、ユース ケースに対して最も便利な方法を選択してください。 選択すると、オーバー ロードの保護によって返される値に関係なくメソッドはようになりました (enciphered および不正使用防止機能も使用可能な)、および保護アプリケーションを使用すると、信頼されていないクライアントに送信できます。

保護されていた一部のデータの保護を解除する保護されたデータの渡し、`Unprotect`メソッドです。 (Byte[] がある-開発者の利便性のためのベースと文字列ベースのオーバー ロードします)。保護されたペイロードは、事前に呼び出したによって生成された場合`Protect`この同じ`IDataProtector`、`Unprotect`メソッドは、元の保護されていないペイロードを返します。 保護されたペイロードが改ざんされたまたは異なるによって生成されたかどうか`IDataProtector`、 `Unprotect` CryptographicException をスローします。

異なると同じ概念`IDataProtector`ties が目的の概念をバックアップします。 場合は、次の 2 つ`IDataProtector`インスタンスが同じルートから生成された`IDataProtectionProvider`がへの呼び出しでそれぞれ異なる目的の文字列を使用して`IDataProtectionProvider.CreateProtector`、両者するいると見なされ、[異なるプロテクター](xref:security/data-protection/consumer-apis/purpose-strings)、し、いずれかの保護を解除することはできません他のによって生成されるペイロード。

## <a name="consuming-these-interfaces"></a>これらのインターフェイスを使用

コンポーネントがかかることが目的の使用方法、DI に対応するコンポーネントです、`IDataProtectionProvider`コンス トラクターのパラメーターと、コンポーネントがインスタンス化されるとき、DI システムがそのこのサービスを自動的に提供します。

> [!NOTE]
> 一部のアプリケーション (コンソール アプリケーション、ASP.NET 4.x アプリケーションなど) があります DI に対応するため、ここで説明する機構を使用できません。 これらのシナリオを参照してください、[非 DI 対応したシナリオ](xref:security/data-protection/configuration/non-di-scenarios)のインスタンスの取得の詳細については、ドキュメント、 `IDataProtection` DI を経由せずプロバイダー。

次の例では、3 つの概念を示しています。

1. [データ保護システム追加](xref:security/data-protection/configuration/overview)サービス コンテナー

2. インスタンスを受信する DI を使用して、 `IDataProtectionProvider`、および

3. 作成する、`IDataProtector`から、`IDataProtectionProvider`を保護し、データ保護の解除に使用するとします。

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Microsoft.AspNetCore.DataProtection.Abstractions パッケージには、拡張メソッドが含まれています。`IServiceProvider.GetDataProtector`開発者便宜を図っています。 カプセル化単一の操作として両方を取得する、`IDataProtectionProvider`したサービス プロバイダーと呼び出し元から`IDataProtectionProvider.CreateProtector`です。 次の例では、その使用法を示します。

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> インスタンス`IDataProtectionProvider`と`IDataProtector`は複数の呼び出し元のスレッド セーフです。 目的としていること、コンポーネントへの参照を取得したら、`IDataProtector`呼び出しを経由して`CreateProtector`、その参照に複数の呼び出しは使用`Protect`と`Unprotect`です。 呼び出し`Unprotect`保護されているペイロードを検証または解読できない場合 CryptographicException がスローされます。 一部のコンポーネントがエラーを無視することも中に保護を解除します。認証 cookie が読み取られますコンポーネント可能性がありますこのエラーを処理およびこれがなかった cookie まったくかのように、要求を処理ではなく完全要求に失敗します。 この動作をするコンポーネントは、すべての例外を受け入れるではなく CryptographicException をキャッチする必要があります具体的には。
