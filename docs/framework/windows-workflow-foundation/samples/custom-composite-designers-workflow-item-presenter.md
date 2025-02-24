---
title: Designer de compostos personalizados - apresentador de item de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: f85224cf-9e30-44a5-9a81-3bc438a34364
ms.openlocfilehash: 239f7ccd81d5bb60eed32298220df215b09e3e47
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70038367"
---
# <a name="custom-composite-designers---workflow-item-presenter"></a>Designer de compostos personalizados - apresentador de item de fluxo de trabalho
O <xref:System.Activities.Presentation.WorkflowItemPresenter> é um tipo de chave no modelo de programação WF designer que permite a criação de uma "área de destino" em que uma atividade arbitrária pode ser colocada. Este exemplo mostra como criar um designer de atividade que superfícies como uma "área de destino".

 Este exemplo demonstra:

## <a name="demonstrates"></a>Demonstra

- Criando um designer personalizado de atividade com <xref:System.Activities.Presentation.WorkflowItemPresenter>.

- Registrando o designer personalizado que usa o armazenamento de metadados.

- Programando a caixa de ferramentas rehosted declarativamente e imperativa.

## <a name="sample-details"></a>Detalhes de exemplo
 O código para esse exemplo mostra:

- O designer personalizado de atividade é compilado para a classe de `SimpleNativeActivity` .

- A criação de um designer personalizado de atividade com <xref:System.Activities.Presentation.WorkflowItemPresenter>.

```xaml
<sap:ActivityDesigner x:Class="Microsoft.Samples.UsingWorkflowItemPresenter.SimpleNativeDesigner"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:sap="clr-namespace:System.Activities.Presentation;assembly=System.Activities.Presentation"
    xmlns:sapv="clr-namespace:System.Activities.Presentation.View;assembly=System.Activities.Presentation">
    <sap:ActivityDesigner.Resources>
        <DataTemplate x:Key="Collapsed">
            <StackPanel>
                <TextBlock>This is the collapsed view</TextBlock>
            </StackPanel>
        </DataTemplate>
        <DataTemplate x:Key="Expanded">
            <StackPanel>
                <TextBlock>Custom Text</TextBlock>
                <sap:WorkflowItemPresenter Item="{Binding Path=ModelItem.Body, Mode=TwoWay}"
                                        HintText="Please drop an activity here" />
            </StackPanel>
        </DataTemplate>
        <Style x:Key="ExpandOrCollapsedStyle" TargetType="{x:Type ContentPresenter}">
            <Setter Property="ContentTemplate" Value="{DynamicResource Collapsed}"/>
            <Style.Triggers>
                <DataTrigger Binding="{Binding Path=ShowExpanded}" Value="true">
                    <Setter Property="ContentTemplate" Value="{DynamicResource Expanded}"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </sap:ActivityDesigner.Resources>
    <Grid>
        <ContentPresenter Style="{DynamicResource ExpandOrCollapsedStyle}" Content="{Binding}" />
    </Grid>
</sap:ActivityDesigner>
```

 Observe o uso de associação de dados de WPF associar a `ModelItem.Body`. `ModelItem`é a propriedade <xref:System.Activities.Presentation.ActivityDesigner> que se refere ao objeto subjacente para o qual o designer está sendo usado, nesse caso, **SimpleNativeActivity**.

#### <a name="to-setup-build-and-run-the-sample"></a>A configuração, compilação, e executar o exemplo

1. Abra a solução no Visual Studio 2010.

2. Pressione F5 para compilar e executar o aplicativo.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] e exemplos. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\CustomActivities\CustomActivityDesigners\WorkflowItemPresenter`  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Activities.Presentation.WorkflowItemPresenter>
- [Desenvolvendo aplicativos com o Designer de Fluxo de Trabalho](/visualstudio/workflow-designer/developing-applications-with-the-workflow-designer)
