---
ms.openlocfilehash: 242a9952cb47d170aceffa1aa392071eb40cc6ab
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67857215"
---
### <a name="rsacng-and-dsacng-are-once-again-usable-in-partial-trust-scenarios"></a>RSACng e DSACng podem ser usados novamente em cenários de confiança parcial

|   |   |
|---|---|
|Detalhes|CngLightup (usado em várias APIs de criptografia de nível mais elevado, como <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType>) e <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType>, em alguns casos, dependem da confiança total. Eles incluem P/Invokes sem declarar permissões de <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType> e caminhos de código em que <xref:System.Security.Cryptography.CngKey?displayProperty=nameWithType> tem demandas de permissão de <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType>. A partir do .NET Framework 4.6.2, CngLightup foi usado para mudar para <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> sempre que possível. Como resultado, aplicativos de confiança parcial que usavam <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType> com êxito começaram a falhar e a lançar exceções <xref:System.Security.SecurityException>. Essa alteração adiciona as declarações necessárias para que todas as funções que usam CngLightup tenham as permissões necessárias.|
|Sugestão|Se essa alteração do .NET Framework 4.6.2 tiver afetado seus aplicativos de confiança parcial de forma negativa, atualize para o .NET Framework 4.7.1.|
|Escopo|Microsoft Edge|
|Versão|4.6.2|
|Tipo|Tempo de execução|
|APIs afetadas|<ul><li><xref:System.Security.Cryptography.DSACng.%23ctor(System.Security.Cryptography.CngKey)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.Key?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.LegalKeySizes?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.CreateSignature(System.Byte[])?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.DSACng.VerifySignature(System.Byte[],System.Byte[])?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.%23ctor(System.Security.Cryptography.CngKey)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.Key?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.Decrypt(System.Byte[],System.Security.Cryptography.RSAEncryptionPadding)?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.RSACng.SignHash(System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType></li></ul>|

