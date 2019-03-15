---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665510"
---
# <a name="aspnet-core-web-api-controller-sample"></a>ASP.NET Core Web API コントローラーのサンプル

このサンプル アプリは、次のプロジェクトで構成されています。

- **WebApiSample.Api.22**:.NET Core 2.2 を対象とする ASP.NET Core 2.2 プロジェクト。
- **WebApiSample.Api.21**:.NET Core 2.1 を対象とする ASP.NET Core 2.1 プロジェクト。
- **WebApiSample.Api.Pre21**:.NET Core 2.0 を対象とする ASP.NET Core 2.0 プロジェクト。
- **WebApiSample.DataAccess**:2 つの Web API プロジェクトのデータ アクセス層として機能する .NET Standard 2.0 クラス ライブラリ。

このサンプルでは、Web API コントローラー作成のバリエーションを示します。

- [ControllerBase からクラスを派生する](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [ApiControllerAttribute でクラスに注釈を付ける](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
