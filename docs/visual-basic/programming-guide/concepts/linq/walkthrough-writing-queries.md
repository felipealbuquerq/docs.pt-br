---
title: Gravando consultas no Visual Basic
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], writing
- LINQ [Visual Basic], walkthroughs
- LINQ [Visual Basic], writing queries
- writing LINQ queries [Visual Basic]
ms.assetid: f0045808-b9fe-4d31-88d1-473d9957211e
ms.openlocfilehash: 256075ad5de5b88595dd4be7f199a74b5912c082
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70046544"
---
# <a name="walkthrough-writing-queries-in-visual-basic"></a>Passo a passo: Gravando consultas no Visual Basic

Este tutorial demonstra como você pode usar Visual Basic recursos de idioma para [!INCLUDE[vbteclinqext](~/includes/vbteclinqext-md.md)] escrever expressões de consulta. O tutorial demonstra como criar consultas em uma lista de objetos de aluno, como executar as consultas e como modificá-las. As consultas incorporam vários recursos, incluindo inicializadores de objeto, inferência de tipo local e tipos anônimos.

Depois de concluir este passo a passos, você estará pronto para passar para os exemplos e a documentação do [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] provedor específico no qual você está interessado. [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]os provedores [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]incluem, LINQ to DataSet e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].

## <a name="create-a-project"></a>Criar um projeto

### <a name="to-create-a-console-application-project"></a>Para criar um projeto de aplicativo de console

1. Inicie o Visual Studio.

2. No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.

3. Na lista **modelos instalados** , clique em **Visual Basic**.

4. Na lista de tipos de projeto, clique em **aplicativo de console**. Na caixa **nome** , digite um nome para o projeto e clique em **OK**.

    Um projeto é criado. Por padrão, ele contém uma referência a System. Core. dll. Além disso, a lista de namespaces importados na [página referências, designer de projeto (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic) inclui o <xref:System.Linq?displayProperty=nameWithType> namespace.

5. Na [página compilar, designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic), verifique se **Option Infer** está definida como **on**.

## <a name="add-an-in-memory-data-source"></a>Adicionar uma Fonte de Dados na Memória

A fonte de dados para as consultas neste passo a passos é uma `Student` lista de objetos. Cada `Student` objeto contém um nome, um sobrenome, um ano de classe e uma classificação acadêmica no corpo do aluno.

### <a name="to-add-the-data-source"></a>Para adicionar a fonte de dados

- Defina uma `Student` classe e crie uma lista de instâncias da classe.

  > [!IMPORTANT]
  > O código necessário para definir a `Student` classe e criar a lista usada nos exemplos explicativos é fornecido em [como: Crie uma lista de itens](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md). Você pode copiá-lo de lá e colá-lo em seu projeto. O novo código substitui o código que apareceu quando você criou o projeto.

### <a name="to-add-a-new-student-to-the-students-list"></a>Para adicionar um novo aluno à lista de alunos

- Siga o padrão no `getStudents` método para adicionar outra instância `Student` da classe à lista. Adicionar o aluno apresentará a você os inicializadores de objeto. Para obter mais informações, [consulte inicializadores de objeto: Tipos](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)nomeados e anônimos.

## <a name="create-a-query"></a>Criar uma consulta

Quando executado, a consulta adicionada nesta seção produz uma lista dos alunos cuja classificação acadêmica os coloca nos dez principais. Como a consulta seleciona o objeto `Student` completo a cada vez, o tipo do resultado da consulta `IEnumerable(Of Student)`é. No entanto, o tipo da consulta normalmente não é especificado nas definições de consulta. Em vez disso, o compilador usa a inferência de tipo local para determinar o tipo. Para obter mais informações, consulte inferência de [tipo local](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md). A variável de intervalo da consulta `currentStudent`,, serve como uma referência a `Student` cada instância na origem, `students`, fornecendo acesso às propriedades de cada objeto no `students`.

### <a name="to-create-a-simple-query"></a>Para criar uma consulta simples

1. Localize o local no `Main` método do projeto que está marcado da seguinte maneira:

    [!code-vb[VbLINQWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#1)]

    Copie o código a seguir e cole-o em.

    [!code-vb[VbLINQWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#2)]

2. Posicione o ponteiro do mouse `studentQuery` sobre em seu código para verificar se o tipo atribuído ao compilador `IEnumerable(Of Student)`é.

## <a name="run-the-query"></a>Executar a Consulta

A variável `studentQuery` contém a definição da consulta, não os resultados da execução da consulta. Um mecanismo típico para executar uma consulta é um `For Each` loop. Cada elemento na sequência retornada é acessado por meio da variável de iteração de loop. Para obter mais informações sobre a execução da consulta, consulte [escrevendo sua primeira consulta LINQ](../../../../visual-basic/programming-guide/concepts/linq/writing-your-first-linq-query.md).

### <a name="to-run-the-query"></a>Para executar a consulta

1. Adicione o seguinte `For Each` loop abaixo da consulta em seu projeto.

    [!code-vb[VbLINQWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#3)]

2. Posicione o ponteiro do mouse sobre a variável `studentRecord` de controle de loop para ver seu tipo de dados. O tipo de `studentRecord` é inferido `Student`como, porque `studentQuery` retorna uma coleção de `Student` instâncias.

3. Crie e execute o aplicativo pressionando CTRL + F5. Observe os resultados na janela do console.

## <a name="modify-the-query"></a>Modificar a Consulta

É mais fácil verificar os resultados da consulta se eles estiverem em uma ordem especificada. Você pode classificar a sequência retornada com base em qualquer campo disponível.

### <a name="to-order-the-results"></a>Para ordenar os resultados

1. Adicione a seguinte `Order By` cláusula entre a `Where` instrução e a `Select` instrução da consulta. A `Order By` cláusula solicitará os resultados em ordem alfabética de a a Z, de acordo com o sobrenome de cada aluno.

    ```
    Order By currentStudent.Last Ascending
    ```

2. Para ordenar por sobrenome e, em seguida, primeiro nome, adicione os dois campos à consulta:

    ```
    Order By currentStudent.Last Ascending, currentStudent.First Ascending
    ```

    Você também pode especificar `Descending` para ordenar de Z a a.

3. Crie e execute o aplicativo pressionando CTRL + F5. Observe os resultados na janela do console.

### <a name="to-introduce-a-local-identifier"></a>Para introduzir um identificador local

1. Adicione o código nesta seção para introduzir um identificador local na expressão de consulta. O identificador local manterá um resultado intermediário. No exemplo a seguir, `name` é um identificador que contém uma concatenação dos nomes e sobrenomes do aluno. Um identificador local pode ser usado para conveniência ou pode melhorar o desempenho armazenando os resultados de uma expressão que, de outra forma, seriam calculados várias vezes.

    [!code-vb[VbLINQWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#4)]

2. Crie e execute o aplicativo pressionando CTRL + F5. Observe os resultados na janela do console.

### <a name="to-project-one-field-in-the-select-clause"></a>Para projetar um campo na cláusula Select

1. Adicione a consulta e `For Each` o loop desta seção para criar uma consulta que produza uma sequência cujos elementos diferem dos elementos na origem. No exemplo a seguir, a origem é uma coleção de `Student` objetos, mas apenas um membro de cada objeto é retornado: o primeiro nome de alunos cujo sobrenome é Garcia. Como `currentStudent.First` é uma cadeia de caracteres, o tipo de dados da sequência `studentQuery3` retornada `IEnumerable(Of String)`por é, uma sequência de cadeias de caracteres. Como nos exemplos anteriores, a atribuição de um tipo de dados `studentQuery3` para é deixada para o compilador determinar usando a inferência de tipo local.

    [!code-vb[VbLINQWalkthrough#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#5)]

2. Posicione o ponteiro do mouse `studentQuery3` sobre em seu código para verificar se o tipo atribuído `IEnumerable(Of String)`é.

3. Crie e execute o aplicativo pressionando CTRL + F5. Observe os resultados na janela do console.

### <a name="to-create-an-anonymous-type-in-the-select-clause"></a>Para criar um tipo anônimo na cláusula Select

1. Adicione o código desta seção para ver como os tipos anônimos são usados em consultas. Você os usa em consultas quando deseja retornar vários campos da fonte de dados em vez de registros completos (`currentStudent` registros nos exemplos anteriores) ou campos únicos (`First` na seção anterior). Em vez de definir um novo tipo nomeado que contém os campos que você deseja incluir no resultado, você especifica os campos na `Select` cláusula e o compilador cria um tipo anônimo com esses campos como suas propriedades. Para obter mais informações, consulte [Tipos anônimos](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md).

    O exemplo a seguir cria uma consulta que retorna o nome e a classificação dos seniores cuja classificação acadêmica está entre 1 e 10, em ordem de classificação acadêmica. Neste exemplo, o tipo de `studentQuery4` deve ser inferido porque a `Select` cláusula retorna uma instância de um tipo anônimo e um tipo anônimo não tem nenhum nome utilizável.

    [!code-vb[VbLINQWalkthrough#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#6)]

2. Crie e execute o aplicativo pressionando CTRL + F5. Observe os resultados na janela do console.

## <a name="additional-examples"></a>Exemplos adicionais

Agora que você entende os conceitos básicos, veja a seguir uma lista de exemplos adicionais para ilustrar a flexibilidade e [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] o poder das consultas. Cada exemplo é precedido por uma breve descrição do que ele faz. Posicione o ponteiro do mouse sobre a variável de resultado da consulta para cada consulta para ver o tipo inferido. Use um `For Each` loop para produzir os resultados.

[!code-vb[VbLINQWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQWalkthrough/VB/Class1.vb#7)]

## <a name="additional-information"></a>Informações adicionais

Depois de se familiarizar com os conceitos básicos do trabalho com consultas, você estará pronto para ler a documentação e os exemplos para o tipo [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] específico de provedor no qual você está interessado:

- [LINQ to Objects](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)

- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)

- [LINQ to XML](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md)

- [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)

## <a name="see-also"></a>Consulte também

- [LINQ (consulta integrada à linguagem) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/index.md)
- [Introdução ao LINQ no Visual Basic](../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
- [Inferência de Tipo de Variável Local](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Inicializadores de objeto: Tipos nomeados e anônimos](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [Tipos Anônimos](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [Introdução ao LINQ no Visual Basic](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [Consultas](../../../../visual-basic/language-reference/queries/index.md)
