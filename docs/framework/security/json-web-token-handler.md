---
title: Manipulador de Token da Web JSON
ms.date: 03/30/2017
ms.assetid: 9968f12e-e05d-4e6a-9b65-6896c0e31ab1
ms.openlocfilehash: 5ffaaaebd87c63b784a749f8998d43d14fe768e1
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64641118"
---
# <a name="json-web-token-handler"></a>Manipulador de Token da Web JSON
A extensão do Manipulador de Tokens da Web JSON para Windows Identity Foundation permite que você crie e valide JWT (Tokens da Web JSON) em seus aplicativos. O Manipulador de Tokens JWT pode ser configurado para ser executado no pipeline WIF como outros manipuladores de tokens de segurança internos, mas também pode ser usado de forma independente para executar a validação do token em aplicativos leves. O Manipulador de Tokens JWT é particularmente útil ao usar um esquema de token portador OAuth 2.0, como a autenticação no Active Directory do Microsoft Azure.  
  
 O Manipulador de Tokens JWT está disponível como um pacote NuGet. Consulte [Baixando o pacote do manipulador de token Web JSON](../../../docs/framework/security/downloading-the-json-web-token-handler-package.md) para obter mais informações.  
  
## <a name="scenarios"></a>Cenários  
 O Manipulador de Tokens JWT permite os seguintes cenários principais:  
  
- **Validar um Token JWT em um aplicativo de servidor**: Nesse cenário, uma empresa chamada Litware tem um aplicativo de servidor que usa WIF para manipular solicitações de logon da web. A Litware deseja permitir que seu aplicativo use tokens JWT para autenticação. O aplicativo é atualizado com o Manipulador de Tokens JWT e depois a configuração do aplicativo é atualizada para adicionar o Manipulador de Tokens JWT no pipeline WIF. Depois que as atualizações são feitas e uma nova solicitação entra no pipeline WIF, o token do JWT é validado usando o novo manipulador, e a autenticação bem-sucedida ocorre.  
  
- **Validar um Token JWT em um serviço Web REST**: Nesse cenário, uma empresa chamada Litware tem um serviço web REST que é protegido pelo Windows Azure Active Directory. As solicitações para o serviço Web devem ser autenticadas pelo AD do Microsoft Azure, que emite um token JWT na autenticação bem-sucedida. A Litware tem um aplicativo cliente que precisa acessar o serviço Web. O cliente faz uma solicitação ao serviço Web e apresenta seu token JWT do AD do Microsoft Azure, que é validado pelo serviço Web usando o Manipulador de Tokens JWT. Depois que o Manipulador de Tokens JWT valida o token, o recurso desejado é retornado para o cliente pelo serviço Web.  
  
## <a name="features"></a>Recursos  
 O Manipulador de Tokens JWT oferece os seguintes recursos:  
  
- **Validar um Token JWT**: Os tokens JWT podem ser facilmente validados pela lógica de validação do manipulador de tokens, como parte do pipeline WIF do aplicativo ou chamados independentemente do WIF  
  
- **Criar um JWT Token**: O manipulador de tokens JWT pode ser usado para criar tokens JWT para autorização em serviços de downstream
