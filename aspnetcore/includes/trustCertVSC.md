---
ms.openlocfilehash: 260f774fdba4d16a4fcb00ac1c699acf4d1bf5b5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615390"
---
* 次のコマンドを実行し、HTTPS 開発証明書を信頼します。

  ```console
  dotnet dev-certs https --trust
  ```

  上記のコマンドでは、次のダイアログが表示されます。

  ![セキュリティ警告のダイアログ](~/getting-started/_static/cert.png)

* 開発証明書を信頼することに同意する場合は、**[はい]** を選択します。

  詳細については、[ASP.NET Core HTTPS 開発証明書の信頼](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)に関する記事をご覧ください
  
