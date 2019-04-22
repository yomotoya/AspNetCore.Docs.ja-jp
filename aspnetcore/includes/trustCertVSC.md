---
ms.openlocfilehash: 260f774fdba4d16a4fcb00ac1c699acf4d1bf5b5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615390"
---
* <span data-ttu-id="3b453-101">次のコマンドを実行し、HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="3b453-101">Trust the HTTPS development certificate by running the following command:</span></span>

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="3b453-102">上記のコマンドでは、次のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b453-102">The preceding command displays the following dialog:</span></span>

  ![セキュリティ警告のダイアログ](~/getting-started/_static/cert.png)

* <span data-ttu-id="3b453-104">開発証明書を信頼することに同意する場合は、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b453-104">Select **Yes** if you agree to trust the development certificate.</span></span>

  <span data-ttu-id="3b453-105">詳細については、[ASP.NET Core HTTPS 開発証明書の信頼](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)に関する記事をご覧ください</span><span class="sxs-lookup"><span data-stu-id="3b453-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>
  
