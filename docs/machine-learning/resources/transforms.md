---
title: Transformações de dados
description: Explore os componentes de engenharia de recursos com suporte no ML.NET.
author: natke
ms.author: nakersha
ms.date: 04/02/2019
ms.openlocfilehash: cbcdef5b8f5f6334d5545f100976347ade9ee6fd
ms.sourcegitcommit: 3eeea78f52ca771087a6736c23f74600cc662658
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68671866"
---
# <a name="data-transformations"></a>Transformações de dados

As transformações de dados são usadas para:
- preparar dados para treinamento de modelo
- aplicar um modelo importado no formato TensorFlow ou ONNX
- pós-processar dados após passá-los por um modelo

As transformações neste guia retornam classes que implementam a interface [IEstimator](xref:Microsoft.ML.IEstimator%601). Transformações de dados podem ser encadeadas. Cada transformação espera e produz dados de tipos e formatos específicos, especificados na documentação de referência vinculada.

Algumas transformações de dados requerem dados de treinamento para calcular seus parâmetros. Por exemplo: o transformador <xref:Microsoft.ML.NormalizationCatalog.NormalizeMeanVariance%2A> calcula a média e a variância dos dados de treinamento durante a operação `Fit()` e usa esses parâmetros na operação `Transform()`. 

Outras transformações de dados não requerem dados de treinamento. Por exemplo: a transformação <xref:Microsoft.ML.ImageEstimatorsCatalog.ConvertToGrayscale*> pode executar a operação `Transform()` sem ter visto nenhum dado de treinamento durante a operação `Fit()`.

## <a name="column-mapping-and-grouping"></a>Agrupamento e mapeamento de coluna

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A> | Concatenar uma ou mais colunas de entrada em uma nova coluna de saída |
| <xref:Microsoft.ML.TransformExtensionsCatalog.CopyColumns%2A> | Copiar e renomear uma ou mais colunas de entrada |
| <xref:Microsoft.ML.TransformExtensionsCatalog.DropColumns%2A> | Remover uma ou mais colunas de entrada |
| <xref:Microsoft.ML.TransformExtensionsCatalog.SelectColumns%2A> | Selecione uma ou mais colunas para manter dos dados de entrada |

## <a name="normalization-and-scaling"></a>Normalização e dimensionamento

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeMeanVariance%2A> | Subtrair a média (dos dados de treinamento) e dividir pela variação (dos dados de treinamento) |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeLogMeanVariance%2A> | Normalizar com base no logaritmo dos dados de treinamento |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeLpNorm%2A> | Dimensionar vetores de entrada pela sua [lp-norm](https://en.wikipedia.org/wiki/Lp_space#The_p-norm_in_finite_dimensions), em que p é 1, 2 ou infinito. Padronizar para a norma l2 (distância euclidiana) |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeGlobalContrast%2A> | Dimensionar cada valor em uma linha subtraindo a média dos dados da linha e dividir pelo desvio padrão ou pela norma l2 (dos dados da linha) e multiplicar por um fator de escala configurável (padrão 2) |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeBinning%2A> | Atribuir o valor de entrada a um índice bin e dividir pelo número de categorias para produzir um valor flutuante entre 0 e 1. Os limites do bin são calculados para distribuir uniformemente os dados de treinamento em compartimentos |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeSupervisedBinning%2A> | Atribuir o valor de entrada a um bin baseado em sua correlação com a coluna de rótulo |
| <xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax%2A> | Dimensionar a entrada pela diferença entre os valores mínimo e máximo nos dados de treinamento |

## <a name="conversions-between-data-types"></a>Conversões entre tipos de dados

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.ConvertType%2A> | Converter o tipo de uma coluna de entrada em um novo tipo |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValue*> | Mapear valores para chaves (categorias) com base no dicionário de mapeamentos fornecido |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey*> | Mapear valores para chaves (categorias) criando o mapeamento a partir dos dados de entrada |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapKeyToValue*> | Converter chaves de volta para seus valores originais |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.MapKeyToVector*> | Converter chaves de volta para vetores de valores originais |
| <xref:Microsoft.ML.ConversionsCatalog.MapKeyToBinaryVector*> | Converter chaves de volta para um vetor binário de valores originais |
| <xref:Microsoft.ML.ConversionsExtensionsCatalog.Hash*> | Resumir o valor na coluna de entrada |

## <a name="text-transformations"></a>Transformações de texto

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.TextCatalog.FeaturizeText*> | Transformar uma coluna de texto em uma matriz flutuante de contagens de diagramas e de caracteres | 
| <xref:Microsoft.ML.TextCatalog.TokenizeIntoWords*> | Dividir uma ou mais colunas de texto em palavras individuais |
| <xref:Microsoft.ML.TextCatalog.TokenizeIntoCharactersAsKeys*> | Dividir uma ou mais colunas de texto em caracteres individuais flutua sobre um conjunto de tópicos |
| <xref:Microsoft.ML.TextCatalog.NormalizeText*> | Alterar maiúsculas e minúsculas, remover marcas diacríticas, sinais de pontuação e números |
| <xref:Microsoft.ML.TextCatalog.ProduceNgrams*> | Transformar a coluna de texto em um local de contagens de ngrams (sequências de palavras consecutivas)|
| <xref:Microsoft.ML.TextCatalog.ProduceWordBags*> | Transformar a coluna de texto em um local de contagens de vetores ngrams |
| <xref:Microsoft.ML.TextCatalog.ProduceHashedNgrams*> | Transformar coluna de texto em um vetor de contagens de ngram hashed |
| <xref:Microsoft.ML.TextCatalog.ProduceHashedWordBags*> | Transformar coluna de texto em um local de contagens de ngram hashed |
| <xref:Microsoft.ML.TextCatalog.RemoveDefaultStopWords*>  | Remover palavras irrelevantes para o idioma especificado das colunas de entrada |
| <xref:Microsoft.ML.TextCatalog.RemoveStopWords*> | Remover palavras irrelevantes especificadas de colunas de entrada |
| <xref:Microsoft.ML.TextCatalog.LatentDirichletAllocation*> | Transformar um documento (representado como um vetor de flutuadores) em um vetor de flutuantes em um conjunto de tópicos |
| <xref:Microsoft.ML.TextCatalog.ApplyWordEmbedding*> | Converter vetores de tokens de texto em vetores de sentença usando um modelo pré-treinado |

## <a name="image-transformations"></a>Transformações de imagem

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ConvertToGrayscale*> | Converter uma imagem em escala de cinza |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ConvertToImage*> | Converter um vetor de pixels para <xref:Microsoft.ML.Transforms.Image.ImageDataViewType> |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ExtractPixels*> | Converter pixels de imagem de entrada em um vetor de números |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.LoadImages*> | Carregar imagens de uma pasta na memória |
| <xref:Microsoft.ML.ImageEstimatorsCatalog.ResizeImages*> | Redimensionar imagens |
| <xref:Microsoft.ML.OnnxCatalog.DnnFeaturizeImage*> | Aplica um modelo de DNN (rede neural profunda) pré-treinado para transformar uma imagem de entrada em um vetor de recurso |

## <a name="categorical-data-transformations"></a>Transformações de dados categóricos

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.CategoricalCatalog.OneHotEncoding*> | Converter uma ou mais colunas de texto em vetores codificados [one-hot](https://en.wikipedia.org/wiki/One-hot) |
| <xref:Microsoft.ML.CategoricalCatalog.OneHotHashEncoding*> | Converter uma ou mais colunas de texto em vetores codificados em one-hot com base em hash |

## <a name="time-series-data-transformations"></a>Transformações de dados de série temporal

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectAnomalyBySrCnn*> | Detectar anomalias nos dados de série temporal de entrada usando o algoritmo SR (Spectral Residual) |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectChangePointBySsa*> | Detectar pontos de alteração nos dados de série temporal usando SSA (análise de espectro singular) |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectIidChangePoint*> | Detectar pontos de alteração em dados de série temporal IID (independentes e distribuídos de forma idêntica) usando estimativas de densidade de kernel adaptável e pontuações de Martingale |
| <xref:Microsoft.ML.TimeSeriesCatalog.ForecastBySsa*> | Prever dados de série temporal usando SSA (análise de espectro singular) |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectSpikeBySsa*> | Detectar picos nos dados de série temporal usando a SSA (análise de espectro singular) |
| <xref:Microsoft.ML.TimeSeriesCatalog.DetectIidSpike*> | Detectar picos em dados de série temporal IID (independentes e distribuídos de forma idêntica) usando estimativas de densidade de kernel adaptável e pontuações de Martingale |

## <a name="missing-values"></a>Valores ausentes

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.ExtensionsCatalog.IndicateMissingValues*> | Criar uma nova coluna de saída booleana, cujo valor é verdadeiro quando o valor na coluna de entrada está ausente |
| <xref:Microsoft.ML.ExtensionsCatalog.ReplaceMissingValues*> | Criar uma nova coluna de saída, cujo valor é configurado para um valor padrão se o valor estiver ausente da coluna de entrada e o valor de entrada, caso contrário |

## <a name="feature-selection"></a>Seleção de recursos

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.FeatureSelectionCatalog.SelectFeaturesBasedOnCount*> | Selecionar recursos cujos valores não padrão são maiores que um limite |
| <xref:Microsoft.ML.FeatureSelectionCatalog.SelectFeaturesBasedOnMutualInformation*> | Selecione os recursos nos quais os dados na coluna de rótulo são mais dependentes |

## <a name="feature-transformations"></a>Transformações de recurso

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.KernelExpansionCatalog.ApproximatedKernelMap*> | Mapear cada vetor de entrada em um espaço de recurso dimensional inferior, em que os produtos internos aproximam uma função de kernel, para que os recursos possam ser usados como entradas para os algoritmos lineares |
| <xref:Microsoft.ML.PcaCatalog.ProjectToPrincipalComponents*> | Reduzir as dimensões do vetor de recurso de entrada aplicando o algoritmo de Análise de Componente Principal |

## <a name="explainability-transformations"></a>Transformações de explicabilidade

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.ExplainabilityCatalog.CalculateFeatureContribution*> | Calcular pontuações de contribuição para cada elemento de um vetor de recurso |

## <a name="calibration-transformations"></a>Transformações de calibração

| Transformar | Definição |
| --- | --- |
|<xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Platt%28System.String%2CSystem.String%2CSystem.String%29> | Transforma uma pontuação bruta de classificador binário em uma probabilidade de classe usando a regressão logística com parâmetros estimados usando os dados de treinamento |
| <xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Platt%28System.Double%2CSystem.Double%2CSystem.String%29> | Transforma uma pontuação bruta de classificador binário em uma probabilidade de classe usando a regressão logística com parâmetros fixos |
| <xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Naive*> | Transforma uma pontuação bruta de classificador binário em uma probabilidade de classe atribuindo pontuações a compartimentos e calculando a probabilidade com base na distribuição entre os compartimentos |
| <xref:Microsoft.ML.BinaryClassificationCatalog.CalibratorsCatalog.Isotonic*> | Transforma uma pontuação bruta de classificador binário em uma probabilidade de classe atribuindo pontuações a compartimentos, em que a posição dos limites e o tamanho dos compartimentos são estimados usando os dados de treinamento  |

## <a name="deep-learning-transformations"></a>Transformações de aprendizado profundo

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.OnnxCatalog.ApplyOnnxModel*> | Transformar os dados de entrada com um modelo ONNX importado |
| <xref:Microsoft.ML.TensorflowCatalog.LoadTensorFlowModel*> | Transformar os dados de entrada com um modelo TensorFlow importado |

## <a name="custom-transformations"></a>Transformações personalizadas

| Transformar | Definição |
| --- | --- |
| <xref:Microsoft.ML.CustomMappingCatalog.CustomMapping*> | Transformar colunas existentes em novas com um mapeamento definido pelo usuário |
