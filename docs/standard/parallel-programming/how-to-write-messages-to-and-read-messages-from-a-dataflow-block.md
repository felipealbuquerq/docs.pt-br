---
title: 'Como: gravar mensagens em um bloco de fluxo de dados e ler mensagens dele'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, reading and writing messages
ms.assetid: 1a9bf078-aa82-46eb-b95a-f87237f028c5
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 22d0f8abd1481bfd75a0d08f49b28cebf78bb4cb
ms.sourcegitcommit: 1b020356e421a9314dd525539da12463d980ce7a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70169147"
---
# <a name="how-to-write-messages-to-and-read-messages-from-a-dataflow-block"></a>Como: gravar mensagens em um bloco de fluxo de dados e ler mensagens dele
Este documento descreve como usar a Biblioteca de fluxo de dados TPL para gravar mensagens e ler mensagens de um bloco de fluxo de dados. A Biblioteca de fluxo de dados TPL fornece métodos síncronos e assíncronos para escrever mensagens e ler mensagens de um bloco de fluxo de dados. Este documento usa a classe <xref:System.Threading.Tasks.Dataflow.BufferBlock%601?displayProperty=nameWithType>. A classe <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> armazenas as memórias e se comporta como uma fonte de mensagem e como um alvo de mensagem.  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="writing-to-and-reading-from-a-dataflow-block-synchronously"></a>Gravar e ler um bloco de fluxo de dados de forma síncrona  
 O exemplo a seguir usa o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> para gravar em um bloco de fluxo de dados <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> e o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A> para ler do mesmo objeto.  
  
 [!code-csharp[TPLDataflow_ReadWrite#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#2)]
 [!code-vb[TPLDataflow_ReadWrite#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#2)]  
  
 Você também pode usar o método <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> para ler a partir de um bloco de fluxo de dados, como mostrado no exemplo a seguir. O método <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> não bloqueia o thread atual e é útil quando ocasionalmente pesquisa dados.  
  
 [!code-csharp[TPLDataflow_ReadWrite#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#3)]
 [!code-vb[TPLDataflow_ReadWrite#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#3)]  
  
 Como o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> atua de forma síncrona, o objeto <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> nos exemplos anteriores recebe todos os dados antes do segundo loop ler os dados. O exemplo a seguir amplia o primeiro exemplo usando <xref:System.Threading.Tasks.Parallel.Invoke%2A> para ler e gravar simultaneamente no bloco de mensagens. Como <xref:System.Threading.Tasks.Parallel.Invoke%2A> executa ações simultaneamente, os valores não são gravados no objeto <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> em qualquer ordem específica.  
  
 [!code-csharp[TPLDataflow_ReadWrite#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#4)]
 [!code-vb[TPLDataflow_ReadWrite#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#4)]  
  
## <a name="writing-to-and-reading-from-a-dataflow-block-asynchronously"></a>Gravar e ler um bloco de fluxo de dados de forma assíncrona  
 O exemplo a seguir usa o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> para gravar de forma assíncrona para um objeto <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> e o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> para ler de forma assíncrona a partir do mesmo objeto. Este exemplo usa os operadores [async](../../csharp/language-reference/keywords/async.md) e [await](../../csharp/language-reference/operators/await.md) ([Async](../../visual-basic/language-reference/modifiers/async.md) e [Await](../../visual-basic/language-reference/operators/await-operator.md) no Visual Basic) para enviar dados de forma assíncrona e ler dados do bloco de destino. O método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> é útil quando você deve habilitar um bloco de fluxo de dados para adiar mensagens. O método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> é útil quando você deseja atuar nos dados quando esses dados se tornam disponíveis. Para obter mais informações sobre como as mensagens se propagam entre os blocos de mensagens, confira a seção Passagem de mensagem em [Fluxo de dados](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md).  
  
 [!code-csharp[TPLDataflow_ReadWrite#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#5)]
 [!code-vb[TPLDataflow_ReadWrite#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#5)]  
  
## <a name="a-complete-example"></a>Exemplo completo  
 O exemplo a seguir mostra o código completo desse documento.  
  
 [!code-csharp[TPLDataflow_ReadWrite#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs#1)]
 [!code-vb[TPLDataflow_ReadWrite#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb#1)]  
  
## <a name="next-steps"></a>Próximas etapas  
 Esse exemplo mostra como ler e gravar diretamente para um bloco de mensagens. Você também pode conectar blocos de fluxo de dados para formar *pipelines*, que são sequências lineares de blocos de fluxo de dados, ou então *redes*, que são gráficos de blocos de fluxo de dados. Em uma rede ou pipeline, origens propagam dados assincronamente para destinos assim que os dados ficam disponíveis. Para obter um exemplo que cria um pipeline de fluxo de dados básico, confira [Passo a passo: criando um pipeline de fluxo de dados](../../../docs/standard/parallel-programming/walkthrough-creating-a-dataflow-pipeline.md). Para obter um exemplo que cria uma rede de fluxo de dados mais complexa, confira [Passo a passo: usando um fluxo de dados em um aplicativo do Windows Forms](../../../docs/standard/parallel-programming/walkthrough-using-dataflow-in-a-windows-forms-application.md).  
  
## <a name="see-also"></a>Consulte também

- [Fluxo de dados](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)
