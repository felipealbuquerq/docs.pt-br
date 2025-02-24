---
title: Explicar previsões do modelo usando Importância do Recurso de Permutação
description: Entender a importância de recursos de modelos com a Importância de recursos de permutação no ML.NET
ms.date: 08/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to
ms.openlocfilehash: 9617582c79b2278e3a68e7acf84568247b81eca1
ms.sourcegitcommit: 1b020356e421a9314dd525539da12463d980ce7a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70167651"
---
# <a name="explain-model-predictions-using-permutation-feature-importance"></a>Explicar previsões do modelo usando Importância do Recurso de Permutação

Aprenda a explicar as previsões de modelo de machine learning do ML.NET compreendendo a contribuição que os recursos dão para previsões usando PFI (Importância de Recurso de Permutação).

Modelos de machine learning geralmente são considerados caixas pretas que pegam entradas e geram uma saída. As etapas intermediárias ou as interações entre os recursos que influenciam a saída raramente são compreendidas. Conforme o aprendizado de máquina é introduzido em mais aspectos da vida diária, como serviços de saúde, é de extrema importância entender por que um modelo de machine learning toma as decisões que ele toma. Por exemplo, se os diagnósticos forem feitos por um modelo de machine learning, os profissionais de saúde precisarão de uma maneira de examinar os fatores que contribuíram para esse diagnóstico. Fornecer o diagnóstico certo pode fazer uma grande diferença em se um paciente tem uma recuperação rápida ou não. Portanto, quanto maior o nível de capacidade de explicação de um modelo, mais confiança os profissionais de saúde terão em aceitar ou rejeitar as decisões tomadas pelo modelo.

Várias técnicas são usadas para explicar os modelos, uma delas é a PFI. PFI é uma técnica usada para explicar os modelos de classificação e regressão inspirados pelo artigo de [Breiman chamado *Random Forests* (Florestas aleatórias) ](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)(veja a seção 10). Em um alto nível, a maneira como eles funcionam é embaralhando aleatoriamente um recurso de dados por vez para todo o conjunto de dados e calculando o quanto a métrica de desempenho de interesse diminui. Quanto maior a alteração, mais importante é esse recurso. 

Além disso, ao realçar os recursos mais importantes, construtores de modelo podem se concentrar no uso de um subconjunto de recursos mais significativos que pode reduzir o ruído e tempo de treinamento.

## <a name="load-the-data"></a>Carregar os dados

Os recursos no conjunto de dados que está sendo usado para este exemplo estão nas colunas 1 a 12. A meta é prever `Price`. 

| Column | Recurso | DESCRIÇÃO 
| --- | --- | --- |
| 1 | CrimeRate | Taxa de criminalidade per capita
| 2 | ResidentialZones | Zonas residenciais da cidade
| 3 | CommercialZones | Zonas não residenciais da cidade
| 4 | NearWater | Proximidade a recursos hídricos
| 5 | ToxicWasteLevels | Níveis de toxicidade (PPM)
| 6 | AverageRoomNumber | Número médio de ambientes na casa
| 7 | HomeAge | Idade da casa
| 8 | BusinessCenterDistance | Distância até o bairro comercial mais próximo
| 9 | HighwayAccess | Proximidade de rodovias
| 10 | TaxRate | Taxa de imposto sobre propriedade
| 11 | StudentTeacherRatio | Taxa de alunos para professores
| 12 | PercentPopulationBelowPoverty | Percentual da população vivendo abaixo da linha de pobreza
| 13 | Preço | Preço da casa

Um exemplo do conjunto de dados é mostrado abaixo:

```text
1,24,13,1,0.59,3,96,11,23,608,14,13,32
4,80,18,1,0.37,5,14,7,4,346,19,13,41
2,98,16,1,0.25,10,5,1,8,689,13,36,12
```

Os dados desta amostra podem ser modelados por uma classe como `HousingPriceData` e carregados em uma [`IDataView`](xref:Microsoft.ML.IDataView).

```csharp
class HousingPriceData
{
    [LoadColumn(0)]
    public float CrimeRate { get; set; }

    [LoadColumn(1)]
    public float ResidentialZones { get; set; }

    [LoadColumn(2)]
    public float CommercialZones { get; set; }

    [LoadColumn(3)]
    public float NearWater { get; set; }

    [LoadColumn(4)]
    public float ToxicWasteLevels { get; set; }

    [LoadColumn(5)]
    public float AverageRoomNumber { get; set; }

    [LoadColumn(6)]
    public float HomeAge { get; set; }

    [LoadColumn(7)]
    public float BusinessCenterDistance { get; set; }

    [LoadColumn(8)]
    public float HighwayAccess { get; set; }

    [LoadColumn(9)]
    public float TaxRate { get; set; }

    [LoadColumn(10)]
    public float StudentTeacherRatio { get; set; }

    [LoadColumn(11)]
    public float PercentPopulationBelowPoverty { get; set; }

    [LoadColumn(12)]
    [ColumnName("Label")]
    public float Price { get; set; }
}
```

## <a name="train-the-model"></a>Treinar o modelo

O exemplo de código a seguir ilustra o processo de treinamento de um modelo de regressão linear para prever preços de casa.

```csharp
// 1. Get the column name of input features.
string[] featureColumnNames = 
    data.Schema
        .Select(column => column.Name)
        .Where(columnName => columnName != "Label").ToArray();

// 2. Define estimator with data pre-processing steps
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", featureColumnNames)
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// 3. Create transformer using the data pre-processing estimator
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// 4. Pre-process the training data
IDataView preprocessedTrainData = dataPrepTransformer.Transform(data);

// 5. Define Stochastic Dual Coordinate Ascent machine learning estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// 6. Train machine learning model
var sdcaModel = sdcaEstimator.Fit(preprocessedTrainData);
```

## <a name="explain-the-model-with-permutation-feature-importance-pfi"></a>Explicar o modelo com PFI (Importância de Recurso de Permutação)

No ML.NET, use o método [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) para suas respectivas tarefas.

```csharp
ImmutableArray<RegressionMetricsStatistics> permutationFeatureImportance = 
    mlContext
        .Regression
        .PermutationFeatureImportance(sdcaModel, preprocessedTrainData, permutationCount:3);
```

O resultado de usar [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) no conjunto de dados de treinamento é um [`ImmutableArray`](xref:System.Collections.Immutable.ImmutableArray) de objetos [`RegressionMetricsStatistics`](xref:Microsoft.ML.Data.RegressionMetricsStatistics). [`RegressionMetricsStatistics`](xref:Microsoft.ML.Data.RegressionMetricsStatistics) fornece estatísticas resumidas, como média e desvio padrão para várias observações de [`RegressionMetrics`](xref:Microsoft.ML.Data.RegressionMetrics) igual ao número de permutações especificado pelo parâmetro `permutationCount`.

A importância ou, neste caso, a redução média absoluta de métrica R ao quadrado calculada por [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) pode ser ordenada da mais importante para a menos importante.  

```csharp
// Order features by importance
var featureImportanceMetrics =
    permutationFeatureImportance
        .Select((metric, index) => new { index, metric.RSquared })
        .OrderByDescending(myFeatures => Math.Abs(myFeatures.RSquared.Mean));

Console.WriteLine("Feature\tPFI");

foreach (var feature in featureImportanceMetrics)
{
    Console.WriteLine($"{featureColumnNames[feature.index],-20}|\t{feature.RSquared.Mean:F6}");
}
```

Imprimir os valores para cada um dos recursos em `featureImportanceMetrics` geraria saída semelhante à abaixo. Lembre-se de que você deve esperar ver resultados diferentes, pois esses valores variam conforme os dados apresentados.  

| Recurso | Alterar para R ao quadrado |
|:--|:--:|
HighwayAccess       |   -0,042731
StudentTeacherRatio |   -0,012730
BusinessCenterDistance| -0,010491
TaxRate             |   -0,008545
AverageRoomNumber   |   -0,003949
CrimeRate           |   -0,003665
CommercialZones     |   0,002749
HomeAge             |   -0,002426
ResidentialZones    |   -0,002319
NearWater           |   0,000203
PercentPopulationLivingBelowPoverty|    0,000031
ToxicWasteLevels    |   -0,000019

Vamos analisar os cinco recursos mais importantes para este conjunto de dados, o preço de uma casa previsto por esse modelo é influenciado pela sua proximidade a rodovias, pela proporção de alunos para professor das escolas na área, pela proximidade com centros de emprego importantes, pela taxa de impostos sobre propriedade e pelo número médio de ambientes na casa.
