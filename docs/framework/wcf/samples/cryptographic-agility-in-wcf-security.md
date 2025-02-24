---
title: Agilidade criptográfica de segurança do WCF
ms.date: 03/30/2017
ms.assetid: c2c549e5-ac19-40c5-b686-8f67f52b6dbf
ms.openlocfilehash: b8e3b6dc62baf31901520d7f5edac0529e937016
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66490943"
---
# <a name="cryptographic-agility-in-wcf-security"></a>Agilidade criptográfica de segurança do WCF

Este exemplo mostra como especificar em um algoritmo padrão/personalizadas para fornecer uma implementação criptográfica agile em um cliente do Windows Communication Foundation (WCF) e o serviço. O exemplo é composto de projetos a seguir:

**Serviço**

Isso é um serviço WCF auto-hospedado que implementa o `ICalculator` da interface e protege o ponto de extremidade usando o <xref:System.ServiceModel.WSHttpBinding> com sessão segura e confiável sessão desabilitada. O serviço define um personalizado `SecurityAlgorithmSuite` classe para especificar os algoritmos de criptografia a ser usado para segurança de mensagem.

**Cliente**

Isso é um cliente do WCF que acessa o serviço após uma autenticação bem-sucedida. Ele invoca as operações expostas pelo `ICalculator` de interface e implementadas pelo serviço. O cliente também define a mesma personalizada `SecurityAlgorithmSuite` classe para especificar os algoritmos de criptografia a ser usado para segurança de mensagem.

## <a name="to-use-this-sample"></a>Para usar este exemplo

1. Abra a solução de CryptoAgility.sln no Visual Studio 2012.

2. Pressione CTRL+SHIFT+B para criar a solução.

3. Abra o Explorador de arquivos, navegue até o diretório \WCF\Basic\Security\CryptoAgility\Service\bin e execute o arquivo de service.exe com privilégios de administrador service.exe botão direito do mouse e selecionando **executar como administrador**.

4. Navegue até o diretório \WCF\Basic\Security\CryptoAgility\Client\bin e execute o arquivo client.exe normalmente.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se este diretório não existir, vá para [Windows Communication Foundation (WCF) e o Windows Workflow Foundation (WF) exemplos do .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Security\CryptoAgility`

## <a name="see-also"></a>Consulte também

- [Segurança do Windows Communication Foundation](../feature-details/security.md)
