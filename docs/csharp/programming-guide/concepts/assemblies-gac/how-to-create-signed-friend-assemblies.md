---
title: 'Como: Criar assemblies amigáveis com sinal (C#)'
ms.date: 07/20/2015
ms.assetid: bab62063-61e6-453f-905f-77673df9534e
ms.openlocfilehash: 7715726a200150b044fb8e97216fa02d0e784838
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69595931"
---
# <a name="how-to-create-signed-friend-assemblies-c"></a>Como: Criar assemblies amigáveis com sinal (C#)
Este exemplo mostra como usar assemblies amigáveis com assemblies que têm nomes fortes. Os dois assemblies devem ter nomes fortes. Embora os dois assemblies neste exemplo usem as mesmas chaves, você pode usar chaves diferentes para dois assemblies.  
  
### <a name="to-create-a-signed-assembly-and-a-friend-assembly"></a>Para criar um assembly assinado e um assembly amigável  
  
1. Abra um prompt de comando.  
  
2. Use a seguinte sequência de comandos com a ferramenta Nome Forte para gerar um keyfile e exibir sua chave pública. Para saber mais, veja [Sn.exe (Ferramenta de Nome Forte)](../../../../framework/tools/sn-exe-strong-name-tool.md).  
  
    1. Gere uma chave de nome forte para este exemplo e armazene-a no arquivo FriendAssemblies.snk:  
  
         `sn -k FriendAssemblies.snk`  
  
    2. Extraia a chave pública de FriendAssemblies.snk e coloque-a no FriendAssemblies.publickey:  
  
         `sn -p FriendAssemblies.snk FriendAssemblies.publickey`  
  
    3. Exiba a chave pública armazenada no arquivo FriendAssemblies.publickey:  
  
         `sn -tp FriendAssemblies.publickey`  
  
3. Crie um arquivo do C# chamado `friend_signed_A` que contenha o seguinte código. O código usa o atributo <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> para declarar friend_signed_B como um assembly autorizado.  
  
     A ferramenta Nome Forte gera uma nova chave pública sempre que for executada. Portanto, você deve substituir a chave pública no código a seguir com a chave pública que você acabou de gerar, conforme mostrado no exemplo a seguir.  
  
    ```csharp  
    // friend_signed_A.cs  
    // Compile with:   
    // csc /target:library /keyfile:FriendAssemblies.snk friend_signed_A.cs  
    using System.Runtime.CompilerServices;  
  
    [assembly: InternalsVisibleTo("friend_signed_B, PublicKey=0024000004800000940000000602000000240000525341310004000001000100e3aedce99b7e10823920206f8e46cd5558b4ec7345bd1a5b201ffe71660625dcb8f9a08687d881c8f65a0dcf042f81475d2e88f3e3e273c8311ee40f952db306c02fbfc5d8bc6ee1e924e6ec8fe8c01932e0648a0d3e5695134af3bb7fab370d3012d083fa6b83179dd3d031053f72fc1f7da8459140b0af5afc4d2804deccb6")]  
    class Class1  
    {  
        public void Test()  
        {  
            System.Console.WriteLine("Class1.Test");  
            System.Console.ReadLine();  
        }  
    }  
    ```  
  
4. Compile e assine o friend_signed_A usando o seguinte comando.  
  
    ```csharp  
    csc /target:library /keyfile:FriendAssemblies.snk friend_signed_A.cs  
    ```  
  
5. Crie um arquivo do C# chamado `friend_signed_B` e que contenha o código a seguir. Como o friend_signed_A especifica o friend_signed_B como um assembly amigável, o código em friend_signed_B pode acessar tipos `internal` e membros do friend_signed_A. O arquivo contém o seguinte código.  
  
    ```csharp  
    // friend_signed_B.cs  
    // Compile with:   
    // csc /keyfile:FriendAssemblies.snk /r:friend_signed_A.dll /out:friend_signed_B.exe friend_signed_B.cs  
    public class Program  
    {  
        static void Main()  
        {  
            Class1 inst = new Class1();  
            inst.Test();  
        }  
    }  
    ```  
  
6. Compile e assine o friend_signed_B, usando o comando a seguir.  
  
    ```csharp  
    csc /keyfile:FriendAssemblies.snk /r:friend_signed_A.dll /out:friend_signed_B.exe friend_signed_B.cs  
    ```  
  
     O nome do assembly gerado pelo compilador deve corresponder ao nome do assembly amigável passado para o atributo <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>. Você deve especificar explicitamente o nome do assembly de saída (.exe ou .dll) usando a opção do compilador `/out`.  Para obter mais informações, consulte [/out (opções do compilador C#)](../../../language-reference/compiler-options/out-compiler-option.md).  
  
7. Execute o arquivo friend_signed_B.exe.  
  
     O programa imprime a cadeia de caracteres "Class1.Test".  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Há semelhanças entre o atributo <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> e a classe <xref:System.Security.Permissions.StrongNameIdentityPermission>. A principal diferença é que <xref:System.Security.Permissions.StrongNameIdentityPermission> pode solicitar permissões de segurança para executar uma determinada seção de código, enquanto o atributo <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> controla a visibilidade de membros e tipos de `internal`.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- [Assemblies no .NET](../../../../standard/assembly/index.md)
- [Assemblies Amigáveis](../../../../standard/assembly/friend-assemblies.md)
- [Como: Criar assemblies amigáveis sem sinal (C#)](./how-to-create-unsigned-friend-assemblies.md)
- [/keyfile](../../../language-reference/compiler-options/keyfile-compiler-option.md)
- [Sn.exe (Ferramenta Nome Forte)](../../../../framework/tools/sn-exe-strong-name-tool.md)
- [Criar e usar assemblies de nomes fortes](../../../../framework/app-domains/create-and-use-strong-named-assemblies.md)
- [Guia de Programação em C#](../../index.md)
