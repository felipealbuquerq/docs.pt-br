---
title: Marshaling de classes, estruturas e uniões
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data marshaling, classes
- marshaling, unions
- marshaling, structures
- marshaling, samples
- data marshaling, structures
- platform invoke, marshaling data
- marshaling, classes
- data marshaling, unions
- data marshaling, samples
- data marshaling, platform invoke
- marshaling, platform invoke
ms.assetid: 027832a2-9b43-4fd9-9b45-7f4196261a4e
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 2411b69dac6ef8945336a4c4e014cbf6687f702a
ms.sourcegitcommit: 56ac30a336668124cb7d95d8ace16bd985875147
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65469723"
---
# <a name="marshaling-classes-structures-and-unions"></a>Marshaling de classes, estruturas e uniões
Estruturas e classes são semelhantes no .NET Framework. Ambas podem ter campos, propriedades e eventos. Elas também podem ter métodos estáticos e não estáticos. Uma diferença importante é que estruturas são tipos de valor e classes são tipos de referência.  
  
 A tabela a seguir lista as opções de marshaling de classes, estruturas e uniões, descreve o uso delas e fornece um link para a amostra de invocação de plataforma correspondente.  
  
|Tipo|Descrição|Amostra|  
|----------|-----------------|------------|  
|Classe por valor.|Passa uma classe com membros de inteiro como um parâmetro In/Out, como no caso gerenciado.|Exemplo SysTime|  
|Estrutura por valor.|Passa estruturas como parâmetros In.|Amostra de estruturas|  
|Estrutura por referência.|Passa estruturas como parâmetros In/Out.|Exemplo de OSInfo|  
|Estrutura com estruturas aninhadas (mescladas).|Passa uma classe que representa uma estrutura com estruturas aninhadas na função não gerenciada. A estrutura é mesclada em uma estrutura grande no protótipo gerenciado.|Exemplo FindFile|  
|Estrutura com um ponteiro para outra estrutura.|Passa uma estrutura que contém um ponteiro para uma segunda estrutura como um membro.|Exemplo de estruturas|  
|Matriz de estruturas com inteiros por valor.|Passa uma matriz de estruturas que contêm somente inteiros como um parâmetro In/Out. Os membros da matriz podem ser alterados.|Exemplo de matrizes|  
|Matriz de estruturas com inteiros e cadeias de caracteres por referência.|Passa uma matriz de estruturas que contêm inteiros e cadeias de caracteres como um parâmetro Out. A função chamada aloca memória para a matriz.|Exemplo de OutArrayOfStructs|  
|Uniões com tipos de valor.|Passa uniões com tipos de valor (integer e double).|Exemplo de uniões|  
|Uniões com tipos mistos.|Passa uniões com tipos mistos (integer e string).|Exemplo de uniões|  
|Valores nulos na estrutura.|Passa uma referência nula (**Nothing** no Visual Basic) em vez de uma referência a um tipo de valor.|Exemplo HandleRef|  
  
## <a name="structures-sample"></a>Amostra de estruturas  
 Este exemplo demonstra como passar uma estrutura que aponta para uma segunda estrutura, passar uma estrutura com uma estrutura inserida e passar uma estrutura com uma matriz inserida.  
  
 A amostra de Structs usa as seguintes funções não gerenciadas, mostradas com a sua declaração de função original:  
  
- **TestStructInStruct** exportado de PinvokeLib.dll.  
  
    ```  
    int TestStructInStruct(MYPERSON2* pPerson2);  
    ```  
  
- **TestStructInStruct3** exportado de PinvokeLib.dll.  
  
    ```  
    void TestStructInStruct3(MYPERSON3 person3);  
    ```  
  
- **TestArrayInStruct** exportado de PinvokeLib.dll.  
  
    ```  
    void TestArrayInStruct( MYARRAYSTRUCT* pStruct );  
    ```  
  
 [PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) é uma biblioteca personalizada não gerenciada que contém implementações para as funções listadas anteriormente e quatro estruturas: **MYPERSON**, **MYPERSON2**, **MYPERSON3** e **MYARRAYSTRUCT**. Essas estruturas contêm os seguintes elementos:  
  
```  
typedef struct _MYPERSON  
{  
   char* first;   
   char* last;   
} MYPERSON, *LP_MYPERSON;  
  
typedef struct _MYPERSON2  
{  
   MYPERSON* person;  
   int age;   
} MYPERSON2, *LP_MYPERSON2;  
  
typedef struct _MYPERSON3  
{  
   MYPERSON person;  
   int age;   
} MYPERSON3;  
  
typedef struct _MYARRAYSTRUCT  
{  
   bool flag;  
   int vals[ 3 ];   
} MYARRAYSTRUCT;  
```  
  
 As estruturas gerenciadas `MyPerson`, `MyPerson2`, `MyPerson3` e `MyArrayStruct` têm as seguintes características:  
  
- `MyPerson` contém apenas membros de cadeia de caracteres. O campo [CharSet](specifying-a-character-set.md) define as cadeias de caracteres em formato ANSI quando passadas para a função não gerenciada.  
  
- `MyPerson2` contém uma **IntPtr** para a estrutura `MyPerson`. O tipo **IntPtr** substitui o ponteiro original para a estrutura não gerenciada porque aplicativos do .NET Framework não usam ponteiros, a menos que o código esteja marcado como **não seguro**.  
  
- `MyPerson3` contém `MyPerson` como uma estrutura inserida. Uma estrutura inserida em outra estrutura pode ser mesclada colocando-se os elementos da estrutura inserida diretamente para a estrutura principal ou pode ser deixada como uma estrutura inserida, o que é feito neste exemplo.  
  
- `MyArrayStruct` contém uma matriz de inteiros. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> define o valor de enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para **ByValArray**, que é usado para indicar o número de elementos na matriz.  
  
 Para todas as estruturas nesta amostra, o atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute> é aplicado para garantir que os membros sejam organizados na memória em sequência, na ordem em que aparecem.  
  
 A classe `LibWrap` contém protótipos gerenciados para os métodos `TestStructInStruct`, `TestStructInStruct3` e `TestArrayInStruct` chamados pela classe `App`. Cada protótipo declara um único parâmetro, da seguinte maneira:  
  
- `TestStructInStruct` declara uma referência ao tipo `MyPerson2` como seu parâmetro.  
  
- `TestStructInStruct3` declara o tipo `MyPerson3` como seu parâmetro e passa o parâmetro por valor.  
  
- `TestArrayInStruct` declara uma referência ao tipo `MyArrayStruct` como seu parâmetro.  
  
 Estruturas como argumentos para métodos são passadas por valor, a menos que o parâmetro contenha a palavra-chave **ref** (**ByRef** no Visual Basic). Por exemplo, o método `TestStructInStruct` passa uma referência (o valor de um endereço) para um objeto do tipo `MyPerson2` para código não gerenciado. Para manipular a estrutura para a qual `MyPerson2` aponta, a amostra cria um buffer de tamanho especificado e retorna o endereço dele combinando os métodos <xref:System.Runtime.InteropServices.Marshal.AllocCoTaskMem%2A?displayProperty=nameWithType> e <xref:System.Runtime.InteropServices.Marshal.SizeOf%2A?displayProperty=nameWithType>. Em seguida, a amostra copia o conteúdo da estrutura gerenciada para o buffer não gerenciado. Por fim, a amostra usa o método <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%2A?displayProperty=nameWithType> para realizar marshaling de dados do buffer não gerenciado para um objeto gerenciado e o método <xref:System.Runtime.InteropServices.Marshal.FreeCoTaskMem%2A?displayProperty=nameWithType> para liberar o bloco de memória não gerenciado.  
  
### <a name="declaring-prototypes"></a>Declarando Protótipos  
 [!code-cpp[Conceptual.Interop.Marshaling#23](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/structures.cpp#23)]
 [!code-csharp[Conceptual.Interop.Marshaling#23](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/structures.cs#23)]
 [!code-vb[Conceptual.Interop.Marshaling#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/structures.vb#23)]  
  
### <a name="calling-functions"></a>Chamando Funções  
 [!code-cpp[Conceptual.Interop.Marshaling#24](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/structures.cpp#24)]
 [!code-csharp[Conceptual.Interop.Marshaling#24](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/structures.cs#24)]
 [!code-vb[Conceptual.Interop.Marshaling#24](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/structures.vb#24)]  
  
## <a name="findfile-sample"></a>Exemplo FindFile  
 Esta amostra demonstra como passar uma estrutura que contém uma segunda estrutura inserida para uma função não gerenciada. Ele também demonstra como usar o atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> para declarar uma matriz de tamanho fixo dentro da estrutura. Nesta amostra, os elementos de estrutura inserida são adicionados à estrutura pai. Para obter um exemplo de uma estrutura inserida que não é mesclada, consulte [Amostra de estruturas](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/eadtsekz(v=vs.100)).  
  
 A amostra de FindFile usa a seguinte função não gerenciada, mostrada com a respectiva declaração de função original:  
  
- **FindFirstFile** exportado de Kernel32.dll.  
  
    ```  
    HANDLE FindFirstFile(LPCTSTR lpFileName, LPWIN32_FIND_DATA lpFindFileData);  
    ```  
  
 A estrutura original passada para a função contém os seguintes elementos:  
  
```  
typedef struct _WIN32_FIND_DATA   
{  
  DWORD    dwFileAttributes;   
  FILETIME ftCreationTime;   
  FILETIME ftLastAccessTime;   
  FILETIME ftLastWriteTime;   
  DWORD    nFileSizeHigh;   
  DWORD    nFileSizeLow;   
  DWORD    dwReserved0;   
  DWORD    dwReserved1;   
  TCHAR    cFileName[ MAX_PATH ];   
  TCHAR    cAlternateFileName[ 14 ];   
} WIN32_FIND_DATA, *PWIN32_FIND_DATA;  
```  
  
 Neste exemplo, a classe `FindData` contém um membro de dados correspondente para cada elemento da estrutura original e da estrutura inserida. Em vez de dois buffers de caracteres originais, a classe substitui cadeias de caracteres. **MarshalAsAttribute** define a enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para **ByValTStr**, que é usado para identificar as matrizes de caracteres embutidas e de comprimento fixo que aparecem dentro de estruturas não gerenciadas.  
  
 A classe `LibWrap` contém um protótipo gerenciado do método `FindFirstFile`, que passa a classe `FindData` como um parâmetro. O parâmetro deve ser declarado com os atributos <xref:System.Runtime.InteropServices.InAttribute> e <xref:System.Runtime.InteropServices.OutAttribute> porque as classes, que são tipos de referência, são passadas como parâmetros In por padrão.  
  
### <a name="declaring-prototypes"></a>Declarando Protótipos  
 [!code-cpp[Conceptual.Interop.Marshaling#17](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/findfile.cpp#17)]
 [!code-csharp[Conceptual.Interop.Marshaling#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/findfile.cs#17)]
 [!code-vb[Conceptual.Interop.Marshaling#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/findfile.vb#17)]  
  
### <a name="calling-functions"></a>Chamando Funções  
 [!code-cpp[Conceptual.Interop.Marshaling#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/findfile.cpp#18)]
 [!code-csharp[Conceptual.Interop.Marshaling#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/findfile.cs#18)]
 [!code-vb[Conceptual.Interop.Marshaling#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/findfile.vb#18)]  
  
## <a name="unions-sample"></a>Exemplo de uniões  
 Este exemplo demonstra como passar estruturas que contêm apenas tipos de valor e estruturas que contém um tipo de valor e uma cadeia de caracteres como parâmetros para uma função não gerenciada esperando uma união. Uma união representa um local de memória que pode ser compartilhado por duas ou mais variáveis.  
  
 A amostra de Unions usa a seguinte função não gerenciada, mostrada com a respectiva declaração de função original:  
  
- **TestUnion** exportado de PinvokeLib.dll.  
  
    ```  
    void TestUnion(MYUNION u, int type);  
    ```  
  
 [PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) é uma biblioteca personalizada não gerenciada que contém uma implementação para a função listada anteriormente e duas uniões, **MYUNION** e **MYUNION2**. As uniões contêm os seguintes elementos:  
  
```  
union MYUNION  
{  
    int number;  
    double d;  
}  
  
union MYUNION2  
{  
    int i;  
    char str[128];  
};  
```  
  
 No código gerenciado, as uniões são definidas como estruturas. A estrutura `MyUnion` contém dois tipos de valor como membros: um integer e um double. O atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute> está definido para controlar a posição precisa de cada membro de dados. O atributo <xref:System.Runtime.InteropServices.FieldOffsetAttribute> indica a posição física dos campos dentro da representação não gerenciada de uma união. Observe que ambos os membros têm os mesmos valores de deslocamento, de modo que os membros podem definir a mesma parte da memória.  
  
 `MyUnion2_1` e `MyUnion2_2` contêm um tipo de valor (integer) e uma cadeia de caracteres, respectivamente. No código gerenciado, não é permitido que os tipos de valor e os tipos de referência se sobreponham. Esta amostra usa a sobrecarga de método para permitir que o chamador use ambos os tipos ao chamar a mesma função não gerenciada. O layout de `MyUnion2_1` é explícito e tem um valor de deslocamento preciso. Por outro lado, `MyUnion2_2` tem um layout sequencial, pois layouts explícitos não são permitidos com tipos de referência. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> define a enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para **ByValTStr**, que é usado para identificar as matrizes de caracteres embutidas e de comprimento fixo que aparecem dentro da representação não gerenciada da união.  
  
 A classe `LibWrap` contém os protótipos para os métodos `TestUnion` e `TestUnion2`. `TestUnion2` está sobrecarregado para declarar `MyUnion2_1` ou `MyUnion2_2` como parâmetros.  
  
### <a name="declaring-prototypes"></a>Declarando Protótipos  
 [!code-cpp[Conceptual.Interop.Marshaling#28](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/unions.cpp#28)]
 [!code-csharp[Conceptual.Interop.Marshaling#28](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/unions.cs#28)]
 [!code-vb[Conceptual.Interop.Marshaling#28](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/unions.vb#28)]  
  
### <a name="calling-functions"></a>Chamando Funções  
 [!code-cpp[Conceptual.Interop.Marshaling#29](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/unions.cpp#29)]
 [!code-csharp[Conceptual.Interop.Marshaling#29](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/unions.cs#29)]
 [!code-vb[Conceptual.Interop.Marshaling#29](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/unions.vb#29)]  
  
## <a name="systime-sample"></a>Exemplo SysTime  
 Este exemplo demonstra como passar um ponteiro para uma classe para uma função não gerenciada que espera um ponteiro para uma estrutura.  
  
 A amostra de SysTime usa a seguinte função não gerenciada, mostrada com a respectiva declaração de função original:  
  
- **GetSystemTime** exportado de Kernel32.dll.  
  
    ```  
    VOID GetSystemTime(LPSYSTEMTIME lpSystemTime);  
    ```  
  
 A estrutura original passada para a função contém os seguintes elementos:  
  
```  
typedef struct _SYSTEMTIME {   
    WORD wYear;   
    WORD wMonth;   
    WORD wDayOfWeek;   
    WORD wDay;   
    WORD wHour;   
    WORD wMinute;   
    WORD wSecond;   
    WORD wMilliseconds;   
} SYSTEMTIME, *PSYSTEMTIME;  
```  
  
 Neste exemplo, a classe `SystemTime` contém os elementos da estrutura original representados como membros de classe. O atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute> é definido para garantir que os membros sejam organizados na memória em sequência, na ordem em que aparecem.  
  
 A classe `LibWrap` contém um protótipo gerenciado do método `GetSystemTime`, que passa a classe `SystemTime` como um parâmetro In/Out por padrão. O parâmetro deve ser declarado com os atributos <xref:System.Runtime.InteropServices.InAttribute> e <xref:System.Runtime.InteropServices.OutAttribute> porque as classes, que são tipos de referência, são passadas como parâmetros In por padrão. Para o chamador receber os resultados, esses [atributos direcionais](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100)) devem ser aplicados explicitamente. A classe `App` cria uma nova instância da classe `SystemTime` e acessa seus campos de dados.  
  
### <a name="code-samples"></a>Exemplos de código  
 [!code-cpp[Conceptual.Interop.Marshaling#25](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/systime.cpp#25)]
 [!code-csharp[Conceptual.Interop.Marshaling#25](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/systime.cs#25)]
 [!code-vb[Conceptual.Interop.Marshaling#25](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/systime.vb#25)]  
  
## <a name="outarrayofstructs-sample"></a>Exemplo OutArrayOfStructs  
 Este exemplo mostra como passar uma matriz de estruturas que contém inteiros e cadeias de caracteres como parâmetros Out para uma função não gerenciada.  
  
 Este exemplo demonstra como chamar uma função nativa usando a classe <xref:System.Runtime.InteropServices.Marshal> e usando código não seguro.  
  
 Esta amostra usa funções wrapper e invocações de plataforma definidas em [PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll), também fornecido nos arquivos de origem. Ela usa a função `TestOutArrayOfStructs` e a estrutura `MYSTRSTRUCT2`. A estrutura contém os seguintes elementos:  
  
```  
typedef struct _MYSTRSTRUCT2  
{  
   char* buffer;  
   UINT size;   
} MYSTRSTRUCT2;  
```  
  
 A classe `MyStruct` contém um objeto de cadeia de caracteres ANSI. O campo <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet> especifica o formato ANSI. `MyUnsafeStruct` é uma estrutura que contém um tipo <xref:System.IntPtr> em vez de uma cadeia de caracteres.  
  
 A classe `LibWrap` contém o método de protótipo `TestOutArrayOfStructs` sobrecarregado. Se um método declara um ponteiro como um parâmetro, a classe deve ser marcada com a palavra-chave `unsafe`. Já que Visual Basic não é capaz de usar código não gerenciado, o método sobrecarregado, o modificador não seguro e a estrutura `MyUnsafeStruct` são desnecessários.  
  
 A classe `App` implementa o método `UsingMarshaling`, que executa todas as tarefas necessárias para passar a matriz. A matriz é marcada com a palavra-chave `out` (`ByRef` no Visual Basic) para indicar que os dados passam do receptor para o chamador. A implementação usa os seguintes métodos de classe <xref:System.Runtime.InteropServices.Marshal>:  
  
- <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%2A> para realizar marshaling de dados do buffer não gerenciado para um objeto gerenciado.  
  
- <xref:System.Runtime.InteropServices.Marshal.DestroyStructure%2A> para liberar a memória reservada para cadeias de caracteres na estrutura.  
  
- <xref:System.Runtime.InteropServices.Marshal.FreeCoTaskMem%2A> para liberar a memória reservada para a matriz.  
  
 Como mencionado anteriormente, o C# permite código não gerenciado e o Visual Basic não. Na amostra do C#, `UsingUnsafePointer` é uma implementação de método alternativo que usa ponteiros em vez da classe <xref:System.Runtime.InteropServices.Marshal> para passar de volta a matriz que contém a estrutura `MyUnsafeStruct`.  
  
### <a name="declaring-prototypes"></a>Declarando Protótipos  
 [!code-cpp[Conceptual.Interop.Marshaling#20](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/outarrayofstructs.cpp#20)]
 [!code-csharp[Conceptual.Interop.Marshaling#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/outarrayofstructs.cs#20)]
 [!code-vb[Conceptual.Interop.Marshaling#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/outarrayofstructs.vb#20)]  
  
### <a name="calling-functions"></a>Chamando Funções  
 [!code-cpp[Conceptual.Interop.Marshaling#21](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/outarrayofstructs.cpp#21)]
 [!code-csharp[Conceptual.Interop.Marshaling#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/outarrayofstructs.cs#21)]
 [!code-vb[Conceptual.Interop.Marshaling#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/outarrayofstructs.vb#21)]  
  
## <a name="see-also"></a>Consulte também

- [Marshaling de dados com a invocação de plataforma](marshaling-data-with-platform-invoke.md)
- [Marshaling em cadeias de caracteres](marshaling-strings.md)
- [Marshaling de diversos tipos de matrizes](marshaling-different-types-of-arrays.md)
