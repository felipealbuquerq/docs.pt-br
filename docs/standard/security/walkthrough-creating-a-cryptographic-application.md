---
title: 'Passo a passo: criar um aplicativo criptográfico'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [NET Framework], example
- cryptography [NET Framework], cryptographic application example
- cryptography [NET Framework], application example
ms.assetid: abf48c11-1e72-431d-9562-39cf23e1a8ff
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 5cdd2f5538be0e39b5dd3a378825ccf81f314c03
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69916275"
---
# <a name="walkthrough-creating-a-cryptographic-application"></a>Passo a passo: criar um aplicativo criptográfico
Este tutorial demonstra como criptografar e descriptografar conteúdo. Os exemplos de código são projetados para um aplicativo Windows Forms. Este aplicativo não demonstra cenários do mundo real, como o uso de cartões inteligentes. Em vez disso, ele demonstra os conceitos básicos de criptografia e descriptografia.  
  
 Este tutorial usa as seguintes diretrizes para criptografia:  
  
- Use a <xref:System.Security.Cryptography.RijndaelManaged> classe, um algoritmo simétrico, para criptografar e descriptografar dados usando seu <xref:System.Security.Cryptography.SymmetricAlgorithm.Key%2A> automaticamente <xref:System.Security.Cryptography.SymmetricAlgorithm.IV%2A>gerado e.  
  
- Use o <xref:System.Security.Cryptography.RSACryptoServiceProvider>, um algoritmo assimétrico, para criptografar e descriptografar a chave para <xref:System.Security.Cryptography.RijndaelManaged>os dados criptografados pelo. Os algoritmos assimétricos são mais bem usados para quantidades menores de dados, como uma chave.  
  
    > [!NOTE]
    > Se você quiser proteger os dados em seu computador em vez de trocar o conteúdo criptografado por outras pessoas, considere <xref:System.Security.Cryptography.ProtectedData> usar <xref:System.Security.Cryptography.ProtectedMemory> as classes ou.  
  
 A tabela a seguir resume as tarefas de criptografia neste tópico.  
  
|Tarefa|Descrição|  
|----------|-----------------|  
|Criando um aplicativo Windows Forms|Lista os controles que são necessários para executar o aplicativo.|  
|Declarando objetos globais|Declara as variáveis de caminho da cadeia <xref:System.Security.Cryptography.CspParameters>de caracteres, <xref:System.Security.Cryptography.RSACryptoServiceProvider> o e o para <xref:System.Windows.Forms.Form> ter o contexto global da classe.|  
|Criando uma chave assimétrica|Cria um par de valor de chave pública e privada assimétrica e atribui a ele um nome de contêiner de chave.|  
|Criptografando um arquivo|Exibe uma caixa de diálogo para selecionar um arquivo para criptografia e criptografa o arquivo.|  
|Descriptografando um arquivo|Exibe uma caixa de diálogo para selecionar um arquivo criptografado para descriptografia e descriptografar o arquivo.|  
|Obtendo uma chave privada|Obtém o par de chaves completo usando o nome do contêiner de chave.|  
|Exportando uma chave pública|Salva a chave em um arquivo XML somente com parâmetros públicos.|  
|Importando uma chave pública|Carrega a chave de um arquivo XML no contêiner de chave.|  
|Testando o aplicativo|Lista os procedimentos para testar este aplicativo.|  
  
## <a name="prerequisites"></a>Pré-requisitos  
 Você precisa dos seguintes componentes para concluir esta instrução passo a passo:  
  
- Referências aos namespaces <xref:System.IO> e <xref:System.Security.Cryptography>.  
  
## <a name="creating-a-windows-forms-application"></a>Criando um aplicativo Windows Forms  
 A maioria dos exemplos de código neste passo a passos é projetada para ser manipuladores de eventos para controles de botão. A tabela a seguir lista os controles necessários para o aplicativo de exemplo e seus nomes necessários para corresponder aos exemplos de código.  
  
|Controle|Nome|Propriedade Text (conforme necessário)|  
|-------------|----------|---------------------------------|  
|<xref:System.Windows.Forms.Button>|`buttonEncryptFile`|Criptografar arquivo|  
|<xref:System.Windows.Forms.Button>|`buttonDecryptFile`|Descriptografar arquivo|  
|<xref:System.Windows.Forms.Button>|`buttonCreateAsmKeys`|Criar chaves|  
|<xref:System.Windows.Forms.Button>|`buttonExportPublicKey`|Exportar chave pública|  
|<xref:System.Windows.Forms.Button>|`buttonImportPublicKey`|Importar chave pública|  
|<xref:System.Windows.Forms.Button>|`buttonGetPrivateKey`|Obter chave privada|  
|<xref:System.Windows.Forms.Label>|`label1`||  
|<xref:System.Windows.Forms.OpenFileDialog>|`openFileDialog1`||  
|<xref:System.Windows.Forms.OpenFileDialog>|`openFileDialog2`||  
  
 Clique duas vezes nos botões no designer do Visual Studio para criar seus manipuladores de eventos.  
  
## <a name="declaring-global-objects"></a>Declarando objetos globais  
 Adicione o código a seguir ao construtor do formulário. Edite as variáveis de cadeia de caracteres para seu ambiente e preferências.  
  
 [!code-csharp[CryptoWalkThru#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#1)]
 [!code-vb[CryptoWalkThru#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#1)]  
  
## <a name="creating-an-asymmetric-key"></a>Criando uma chave assimétrica  
 Essa tarefa cria uma chave assimétrica que criptografa e descriptografa a <xref:System.Security.Cryptography.RijndaelManaged> chave. Essa chave foi usada para criptografar o conteúdo e exibe o nome do contêiner de chave no controle rótulo.  
  
 Adicione o código a seguir como `Click` o manipulador de eventos `Create Keys` para o`buttonCreateAsmKeys_Click`botão ().  
  
 [!code-csharp[CryptoWalkThru#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#2)]
 [!code-vb[CryptoWalkThru#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#2)]  
  
## <a name="encrypting-a-file"></a>Criptografando um arquivo  
 Essa tarefa envolve dois métodos: o método manipulador de eventos para `Encrypt File` o botão`buttonEncryptFile_Click`() e `EncryptFile` o método. O primeiro método exibe uma caixa de diálogo para selecionar um arquivo e passa o nome do arquivo para o segundo método, que executa a criptografia.  
  
 O conteúdo criptografado, a chave e o IV são todos salvos <xref:System.IO.FileStream>em um, que é conhecido como o pacote de criptografia.  
  
 O `EncryptFile` método faz o seguinte:  
  
1. Cria um <xref:System.Security.Cryptography.RijndaelManaged> algoritmo simétrico para criptografar o conteúdo.  
  
2. Cria um <xref:System.Security.Cryptography.RSACryptoServiceProvider> objeto para criptografar <xref:System.Security.Cryptography.RijndaelManaged> a chave.  
  
3. Usa um <xref:System.Security.Cryptography.CryptoStream> objeto para ler e criptografar <xref:System.IO.FileStream> o do arquivo de origem, em blocos de bytes, em um <xref:System.IO.FileStream> objeto de destino para o arquivo criptografado.  
  
4. Determina os comprimentos da chave criptografada e IV e cria matrizes de bytes de seus valores de comprimento.  
  
5. Grava a chave, o IV e seus valores de comprimento para o pacote criptografado.  
  
 O pacote de criptografia usa o seguinte formato:  
  
- Comprimento da chave, bytes 0-3  
  
- Comprimento de IV, bytes 4-7  
  
- Chave criptografada  
  
- IV  
  
- Texto cifrado  
  
 Você pode usar os comprimentos da chave e do IV para determinar os pontos de partida e os comprimentos de todas as partes do pacote de criptografia, que podem ser usados para descriptografar o arquivo.  
  
 Adicione o código a seguir como `Click` o manipulador de eventos `Encrypt File` para o`buttonEncryptFile_Click`botão ().  
  
 [!code-csharp[CryptoWalkThru#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#3)]
 [!code-vb[CryptoWalkThru#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#3)]  
  
 Adicione o seguinte `EncryptFile` método ao formulário.  
  
 [!code-csharp[CryptoWalkThru#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#5)]
 [!code-vb[CryptoWalkThru#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#5)]  
  
## <a name="decrypting-a-file"></a>Descriptografando um arquivo  
 Essa tarefa envolve dois métodos, o método manipulador de eventos para `Decrypt File` o botão`buttonDecryptFile_Click`() e o `DecryptFile` método. O primeiro método exibe uma caixa de diálogo para selecionar um arquivo e passa seu nome de arquivo para o segundo método, que executa a descriptografia.  
  
 O `Decrypt` método faz o seguinte:  
  
1. Cria um <xref:System.Security.Cryptography.RijndaelManaged> algoritmo simétrico para descriptografar o conteúdo.  
  
2. Lê os primeiros oito bytes do <xref:System.IO.FileStream> pacote criptografado em matrizes de bytes para obter os comprimentos da chave criptografada e o IV.  
  
3. Extrai a chave e o IV do pacote de criptografia em matrizes de bytes.  
  
4. Cria um <xref:System.Security.Cryptography.RSACryptoServiceProvider> objeto para descriptografar a <xref:System.Security.Cryptography.RijndaelManaged> chave.  
  
5. Usa um <xref:System.Security.Cryptography.CryptoStream> objeto para ler e descriptografar a seção <xref:System.IO.FileStream> de texto cifrado do pacote de criptografia, em blocos <xref:System.IO.FileStream> de bytes, no objeto do arquivo descriptografado. Quando isso for concluído, a descriptografia será concluída.  
  
 Adicione o código a seguir como `Click` o manipulador de eventos `Decrypt File` para o botão.  
  
 [!code-csharp[CryptoWalkThru#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#4)]
 [!code-vb[CryptoWalkThru#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#4)]  
  
 Adicione o seguinte `DecryptFile` método ao formulário.  
  
 [!code-csharp[CryptoWalkThru#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#6)]
 [!code-vb[CryptoWalkThru#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#6)]  
  
## <a name="exporting-a-public-key"></a>Exportando uma chave pública  
 Essa tarefa salva a chave criada pelo `Create Keys` botão em um arquivo. Ele exporta apenas os parâmetros públicos.  
  
 Essa tarefa simula o cenário de Alice dando a Bob sua chave pública para que ele possa criptografar arquivos. Ele e outros que tenham essa chave pública não poderão descriptografá-los porque não têm o par de chaves completo com parâmetros privados.  
  
 Adicione o código a seguir como `Click` o manipulador de eventos `Export Public Key` para o`buttonExportPublicKey_Click`botão ().  
  
 [!code-csharp[CryptoWalkThru#8](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#8)]
 [!code-vb[CryptoWalkThru#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#8)]  
  
## <a name="importing-a-public-key"></a>Importando uma chave pública  
 Essa tarefa carrega a chave somente com parâmetros públicos, conforme criado pelo `Export Public Key` botão e o define como o nome do contêiner de chave.  
  
 Esta tarefa simula o cenário de Bob carregando a chave de Alice com apenas parâmetros públicos, portanto, ele pode criptografar arquivos.  
  
 Adicione o código a seguir como `Click` o manipulador de eventos `Import Public Key` para o`buttonImportPublicKey_Click`botão ().  
  
 [!code-csharp[CryptoWalkThru#9](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#9)]
 [!code-vb[CryptoWalkThru#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#9)]  
  
## <a name="getting-a-private-key"></a>Obtendo uma chave privada  
 Essa tarefa define o nome do contêiner de chave como o nome da chave criada usando o `Create Keys` botão. O contêiner de chave conterá o par de chaves completo com parâmetros privados.  
  
 Esta tarefa simula o cenário de Alice usando sua chave privada para descriptografar arquivos criptografados por Bob.  
  
 Adicione o código a seguir como `Click` o manipulador de eventos `Get Private Key` para o`buttonGetPrivateKey_Click`botão ().  
  
 [!code-csharp[CryptoWalkThru#7](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#7)]
 [!code-vb[CryptoWalkThru#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#7)]  
  
## <a name="testing-the-application"></a>Testando o aplicativo  
 Depois de criar o aplicativo, execute os seguintes cenários de teste.  
  
#### <a name="to-create-keys-encrypt-and-decrypt"></a>Para criar chaves, criptografar e descriptografar  
  
1. Clique no botão `Create Keys`. O rótulo exibe o nome da chave e mostra que ele é um par de chaves completo.  
  
2. Clique no botão `Export Public Key`. Observe que a exportação dos parâmetros de chave pública não altera a chave atual.  
  
3. Clique no `Encrypt File` botão e selecione um arquivo.  
  
4. Clique no `Decrypt File` botão e selecione o arquivo que acabou de ser criptografado.  
  
5. Examine o arquivo apenas descriptografado.  
  
6. Feche o aplicativo e reinicie-o para testar a recuperação de contêineres de chave persistentes no próximo cenário.  
  
#### <a name="to-encrypt-using-the-public-key"></a>Para criptografar usando a chave pública  
  
1. Clique no botão `Import Public Key`. O rótulo exibe o nome da chave e mostra que ele é somente público.  
  
2. Clique no `Encrypt File` botão e selecione um arquivo.  
  
3. Clique no `Decrypt File` botão e selecione o arquivo que acabou de ser criptografado. Isso irá falhar porque você deve ter a chave privada para descriptografar.  
  
 Este cenário demonstra como ter apenas a chave pública para criptografar um arquivo para outra pessoa. Normalmente, essa pessoa daria apenas a chave pública e a chave privada para a descriptografia.  
  
#### <a name="to-decrypt-using-the-private-key"></a>Para descriptografar usando a chave privada  
  
1. Clique no botão `Get Private Key`. O rótulo exibe o nome da chave e mostra se ele é o par de chaves completo.  
  
2. Clique no `Decrypt File` botão e selecione o arquivo que acabou de ser criptografado. Isso será bem-sucedido porque você tem o par de chaves completo a ser descriptografado.  
  
## <a name="see-also"></a>Consulte também

- [Serviços criptográficos](../../../docs/standard/security/cryptographic-services.md)
