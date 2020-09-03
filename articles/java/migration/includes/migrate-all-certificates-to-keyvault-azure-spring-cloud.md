---
author: yevster
ms.author: yebronsh
ms.date: 7/17/2020
ms.openlocfilehash: 1e7957a03cb96254a0318774a1620c7143ae6fc4
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062061"
---
### <a name="migrate-all-certificates-to-keyvault"></a>すべての証明書を KeyVault に移行する

Azure Spring Cloud は JRE キーストアへのアクセスを提供しないため、Azure KeyVault に証明書を移行し、KeyVault 内の証明書にアクセスするようにアプリケーション コードを変更する必要があります。 詳細については、「[Key Vault 証明書の概要](/azure/key-vault/certificates/certificate-scenarios)」と [Java 用の Azure Key Vault の証明書クライアント ライブラリ](/java/api/overview/azure/security-keyvault-certificates-readme)に関するページを参照してください。