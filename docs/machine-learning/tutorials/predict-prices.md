---
title: 'Tutorial: Prever os preços usando regressão'
description: Este tutorial mostra como criar um modelo de regressão usando do ML.NET para prever os preços, especificamente, as tarifas de táxi de Nova York.
ms.date: 05/09/2019
ms.topic: tutorial
ms.custom: mvc, seodec18, title-hack-0516
ms.openlocfilehash: fe3afab4cbd3f77ed4498cc5081180910d7d0b9e
ms.sourcegitcommit: cdf67135a98a5a51913dacddb58e004a3c867802
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69666618"
---
# <a name="tutorial-predict-prices-using-regression-with-mlnet"></a>Tutorial: Prever os preços usando regressão com ML.NET

Este tutorial mostra como criar um [modelo de regressão](../resources/glossary.md#regression) usando do ML.NET para prever os preços, especificamente, as tarifas de táxi de Nova York.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
> * Preparar e compreender os dados
> * Carregar e transformar os dados
> * Escolher um algoritmo de aprendizado
> * Treinar o modelo
> * Avaliar o modelo
> * Usar o modelo para previsões

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017 15.6 ou posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) com a carga de trabalho "Desenvolvimento de plataforma cruzada do .NET Core" instalada.

## <a name="create-a-console-application"></a>Criar um aplicativo de console

1. Crie um **Aplicativo de Console do .NET Core** chamado "TaxiFarePrediction".

1. Crie um diretório chamado *Dados* em seu projeto para armazenar o conjunto de dados e arquivos de modelo.

1. Instale o pacote NuGet **Microsoft.ML**:

    No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote, selecione a guia **Procurar**, pesquise por **Microsoft.ML**, selecione o pacote na lista e selecione o botão **Instalar**. Selecione o botão **OK** na caixa de diálogo **Visualizar Alterações** e selecione o botão **Aceito** na caixa de diálogo **Aceitação da Licença**, se concordar com o termos de licença para os pacotes listados. Faça o mesmo para o pacote do NuGet **Microsoft.ML.FastTree**.

## <a name="prepare-and-understand-the-data"></a>Preparar e compreender os dados

1. Baixe os conjuntos de dados [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-train.csv) e [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-test.csv) e salve-os na pasta *Dados* criada na etapa anterior. Esses conjuntos de dados são usados para treinar o modelo de aprendizado de máquina e, em seguida, avaliar a precisão dele. Esses conjuntos de dados são originalmente do [Conjunto de dados NYC TLC Taxi Trip](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml).

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse em cada um dos arquivos \*.csv e selecione **Propriedades**. Em **Avançado**, altere o valor de **Copiar para Diretório de Saída** para **Copiar se for mais novo**.

1. Abra o conjunto de dados **taxi-fare-train.csv** e observe os cabeçalhos de coluna na primeira linha. Dê uma olhada em cada uma das colunas. Compreenda os dados e decida quais colunas são **recursos** e qual é o **rótulo**.

O `label` é a coluna que você deseja prever. O `Features`identificado são as entradas que você atribui ao modelo para prever o `Label`.

O conjunto de dados fornecido contém as seguintes colunas:

* **vendor_id:** A ID do taxista é um recurso.
* **rate_code:** O tipo de tarifa da corrida de táxi é um recurso.
* **passenger_count:** O número de passageiros na corrida é um recurso.
* **trip_time_in_secs:** O tempo que levou a corrida. Você deseja prever a tarifa da viagem antes de sua conclusão. No momento, você não sabe a duração da viagem. Portanto, o tempo da viagem não é um recurso e você excluirá essa coluna do modelo.
* **trip_distance:** A distância da corrida é um recurso.
* **payment_type:** A forma de pagamento (dinheiro ou cartão de crédito) é um recurso.
* **fare_amount:** A tarifa total de táxi paga é o rótulo.

## <a name="create-data-classes"></a>Criar classes de dados

Crie classes para os dados de entrada e as previsões:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Novo Item**.
1. Na caixa de diálogo **Adicionar Novo Item**, selecione **Classe** e altere o campo **Nome** para *TaxiTrip.cs*. Em seguida, selecione o botão **Adicionar**.
1. Adicione as seguintes diretivas `using` ao novo arquivo:

   [!code-csharp[AddUsings](~/samples/machine-learning/tutorials/TaxiFarePrediction/TaxiTrip.cs#1 "Add necessary usings")]

Remova a definição de classe existente e adicione o seguinte código, que tem duas classes `TaxiTrip` e `TaxiTripFarePrediction`, ao arquivo *TaxiTrip.cs*:

[!code-csharp[DefineTaxiTrip](~/samples/machine-learning/tutorials/TaxiFarePrediction/TaxiTrip.cs#2 "Define the taxi trip and fare predictions classes")]

`TaxiTrip` é a classe de dados de entrada e tem definições para cada uma das colunas do conjunto de dados. Use o atributo <xref:Microsoft.ML.Data.LoadColumnAttribute> para especificar os índices das colunas de origem no conjunto de dados.

A classe `TaxiTripFarePrediction` representa os resultados previstos. Ela tem um único campo de float, `FareAmount`, com um atributo `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> aplicado. No caso da tarefa de regressão, a coluna **Pontuação** contém valores de rótulo previsto.

> [!NOTE]
> Use o tipo `float` para representar valores de ponto flutuante nas classes de dados de entrada e previsão.

### <a name="define-data-and-model-paths"></a>Definir dados e caminhos de modelo

Adicione as seguintes instruções `using` adicionais ao início do arquivo *Program.cs*:

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#1 "Add necessary usings")]

Você precisa criar três campos para conter os caminhos para os arquivos com conjuntos de dados e o arquivo para salvar o modelo:

* `_trainDataPath` contém o caminho para o arquivo com o conjunto de dados usado para treinar o modelo.
* `_testDataPath` contém o caminho para o arquivo com o conjunto de dados usado para avaliar o modelo.
* `_modelPath` contém o caminho para o arquivo em que o modelo treinado está armazenado.

Adicione o seguinte código logo acima do método `Main` para especificar estes caminhos e para a variável `_textLoader`:

[!code-csharp[InitializePaths](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#2 "Define variables to store the data file paths")]

Todas as operações do ML.NET iniciam na [classe MLContext](xref:Microsoft.ML.MLContext). Inicializar `mlContext` cria um novo ambiente do ML.NET que pode ser compartilhado entre os objetos do fluxo de trabalho de criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

### <a name="initialize-variables-in-main"></a>Inicializar variáveis em Main

Substitua a linha `Console.WriteLine("Hello World!")` no método `Main` pelo seguinte código para declarar e inicializar a variável `mlContext`:

[!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#3 "Create the ML Context")]

Adicione o seguinte como a linha seguinte de código no método `Main` para chamar o método `Train`:

[!code-csharp[Train](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#5 "Train your model")]

O método `Train()` executa as seguintes tarefas:

* Carrega os dados.
* Extrai e transforma os dados.
* Treina o modelo.
* Retorna o modelo.

O método `Train` treina o modelo. Crie esse método logo abaixo de `Main`, usando o seguinte código:

```csharp
public static ITransformer Train(MLContext mlContext, string dataPath)
{

}
```

## <a name="load-and-transform-data"></a>Carregar e transformar dados

O ML.NET usa a [classe IDataView](xref:Microsoft.ML.IDataView) como uma maneira flexível e eficiente de descrever dados tabulares de texto ou numéricos. O `IDataView` pode carregar arquivos de texto ou em tempo real (por exemplo, banco de dados SQL ou arquivos de log). Adicione o seguinte código como a primeira linha do método `Train()`:

[!code-csharp[LoadTrainData](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#6 "loading training dataset")]

Como você deseja prever as tarifas de corrida de táxi, a coluna `FareAmount` é o `Label` que você vai prever (a saída do modelo). Use a classe de transformação `CopyColumnsEstimator` para copiar `FareAmount` e adicione o seguinte código: 

[!code-csharp[CopyColumnsEstimator](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#7 "Use the CopyColumnsEstimator")]

O algoritmo que treina o modelo requer recursos **numéricos**, então, é necessário transformar os valores de dados categóricos (`VendorId`, `RateCode` e `PaymentType`) em números (`VendorIdEncoded`, `RateCodeEncoded` e `PaymentTypeEncoded`). Para fazer isso, use a classe de transformação [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer), que atribui valores de chave numérica diferentes para os diferentes valores em cada uma das colunas e adicione o seguinte código:

[!code-csharp[OneHotEncodingEstimator](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#8 "Use the OneHotEncodingEstimator")]

A última etapa na preparação de dados combina todas as colunas de recursos na coluna **Features** usando a classe de transformação `mlContext.Transforms.Concatenate`. Por padrão, um algoritmo de aprendizado processa apenas os recursos da coluna **Features**. Adicione o seguinte código:

[!code-csharp[ColumnConcatenatingEstimator](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#9 "Use the ColumnConcatenatingEstimator")]

## <a name="choose-a-learning-algorithm"></a>Escolher um algoritmo de aprendizado

Esse problema é sobre prever uma tarifa de corrida de táxi em Nova York. À primeira vista, pode parecer que depende simplesmente da distância percorrida. No entanto, os taxistas em Nova York cobram valores variados por outros fatores, como passageiros adicionais ou pagamento por cartão de crédito em vez de dinheiro. Você deseja prever o valor de preço, que é um valor real, com base em outros fatores no conjunto de dados. Para fazer isso, você deve escolher uma tarefa de aprendizado de máquina de [regressão](../resources/glossary.md#regression).

Acrescente a tarefa de aprendizado de máquina [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) às definições de transformação de dados adicionando o seguinte como a próxima linha de código em `Train()`:

[!code-csharp[FastTreeRegressionTrainer](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#10 "Add the FastTreeRegressionTrainer")]

## <a name="train-the-model"></a>Treinar o modelo

Ajuste o modelo à `dataview` do treinamento e retorne o modelo treinado adicionando a seguinte linha de código ao método `Train()`:

[!code-csharp[TrainModel](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#11 "Train the model")]

O método [Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) treina o modelo transformando o conjunto de dados e aplicando o treinamento.

Retorne o modelo treinado com a seguinte linha de código no método `Train()`:

[!code-csharp[ReturnModel](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#12 "Return the model")]

## <a name="evaluate-the-model"></a>Avaliar o modelo

Em seguida, avalie o desempenho do modelo com seus dados de teste para garantia de qualidade e validação. Crie o método `Evaluate()`, logo após `Train()`, com o código a seguir:

```csharp
private static void Evaluate(MLContext mlContext, ITransformer model)
{

}
```

O método `Evaluate` executa as seguintes tarefas:

* Carrega o conjunto de dados de teste.
* Cria o avaliador de regressão.
* Avalia o modelo e cria métricas.
* Exibe as métricas.

Adicione uma chamada ao novo método a partir do método `Main`, logo abaixo da chamada do método `Train`, usando o seguinte código:

[!code-csharp[CallEvaluate](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#14 "Call the Evaluate method")]

Carregue o conjunto de dados de teste usando o método [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A). Avalie o modelo usando esse conjunto de dados como uma verificação de qualidade adicionando o seguinte código ao método `Evaluate`:

[!code-csharp[LoadTestDataset](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#15 "Load the test dataset")]

Em seguida, transforme os dados `Test` adicionando o seguinte código a `EvaluateModel()`:

[!code-csharp[PredictWithTransformer](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#16 "Predict using the Transformer")]

O método [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) faz previsões para testar as linhas de entrada do conjunto de dados.

O método `RegressionContext.Evaluate` calcula as métricas de qualidade para o `PredictionModel` usando o conjunto de dados especificado. Ele retorna um objeto <xref:Microsoft.ML.Data.RegressionMetrics> que contém as métricas gerais computadas pelos avaliadores de regressão. 

Para exibi-los a fim de determinar a qualidade do modelo, é preciso primeiro obter as métricas. Adicione o seguinte código ao método `Evaluate` como a linha seguinte:

[!code-csharp[ComputeMetrics](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#17 "Compute Metrics")]

Depois que você define a previsão, o método [Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) avalia o modelo, que compara os valores previstos com os `Labels` reais no conjunto de dados de teste e retorna métricas sobre o desempenho do modelo.

Adicione o seguinte código para avaliar o modelo e produzir as métricas de avaliação:

```csharp
Console.WriteLine();
Console.WriteLine($"*************************************************");
Console.WriteLine($"*       Model quality metrics evaluation         ");
Console.WriteLine($"*------------------------------------------------");
```

[RSquared](../resources/glossary.md#coefficient-of-determination) é outra métrica de avaliação dos modelos de regressão. RSquared aceita valores entre 0 e 1. Quanto mais perto de 1 estiver o valor, melhor o modelo será. Adicione o seguinte código ao método `Evaluate` para exibir o valor RSquared:

[!code-csharp[DisplayRSquared](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#18 "Display the RSquared metric.")]

[RMS](../resources/glossary.md##root-of-mean-squared-error-rmse) é uma das métricas de avaliação do modelo de regressão. Quanto menor, melhor o modelo. Adicione o seguinte código ao método `Evaluate` para exibir o valor RMS:

[!code-csharp[DisplayRMS](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#19 "Display the RMS metric.")]

## <a name="use-the-model-for-predictions"></a>Usar o modelo para previsões

Crie o método `TestSinglePrediction`, logo após o método `Evaluate`, usando o seguinte código:

```csharp
private static void TestSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

O método `TestSinglePrediction` executa as seguintes tarefas:

* Cria um único comentário dos dados de teste.
* Prevê o valor da tarifa com base nos dados de teste.
* Combina dados de teste e previsões para relatórios.
* Exibe os resultados previstos.

Adicione uma chamada ao novo método a partir do método `Main`, logo abaixo da chamada do método `Evaluate`, usando o seguinte código:

[!code-csharp[CallTestSinglePrediction](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#20 "Call the TestSinglePrediction method")]

Use o `PredictionEngine` para prever a tarifa adicionando o seguinte código a `TestSinglePrediction()`:

[!code-csharp[MakePredictionEngine](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#22 "Create the PredictionFunction")]

A [classe PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência que permite passar uma única instância de dados e então executar uma previsão nela.

Este tutorial usa uma viagem de teste dentro dessa classe. Posteriormente, você pode adicionar outros cenários para fazer experiências com o modelo. Adicione uma corrida de teste à previsão do modelo treinado no método `TestSinglePrediction()` ao criar uma instância de `TaxiTrip`:

[!code-csharp[PredictionData](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#23 "Create test data for single prediction")]

Em seguida, preveja a tarifa com base em uma única instância dos dados de corrida táxi e passe-a para o `PredictionEngine` adicionando o seguinte como as próximas linhas do código no método `TestSinglePrediction()`:

[!code-csharp[Predict](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#24 "Create a prediction of taxi fare")]

A função [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) faz uma previsão em uma única instância de dados.

Para exibir a tarifa prevista da corrida especificada, adicione o código a seguir ao método `TestSinglePrediction`:

[!code-csharp[Predict](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#25 "Display the prediction.")]

Execute o programa para ver a tarifa de táxi prevista para o seu caso de teste.

Parabéns! Agora você criou com sucesso um modelo de aprendizado de máquina para prever tarifas de táxi, avaliou sua precisão e o usou para fazer previsões. Encontre o código-fonte deste tutorial no repositório do GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TaxiFarePrediction).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Preparar e compreender os dados
> * Criar um pipeline de aprendizado
> * Carregar e transformar os dados
> * Escolher um algoritmo de aprendizado
> * Treinar o modelo
> * Avaliar o modelo
> * Usar o modelo para previsões

Avance para o próximo tutorial para saber mais.

> [!div class="nextstepaction"]
> [Clustering de Iris](iris-clustering.md)
