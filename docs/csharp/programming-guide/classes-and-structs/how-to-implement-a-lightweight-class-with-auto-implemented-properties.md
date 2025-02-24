---
title: 'Como: implementar uma classe leve com propriedades autoimplementadas – Guia de Programação em C#'
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- auto-implemented properties [C#]
- properties [C#], auto-implemented
ms.assetid: 1dc5a8ad-a4f7-4f32-8506-3fc6d8c8bfed
ms.openlocfilehash: 4cbed8145487325d8b06882bbab843321a49d0d3
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69596908"
---
# <a name="how-to-implement-a-lightweight-class-with-auto-implemented-properties-c-programming-guide"></a>Como: implementar uma classe leve com propriedades autoimplementadas (Guia de Programação em C#)

Este exemplo mostra como criar uma classe leve imutável que serve apenas para encapsular um conjunto de propriedades autoimplementadas. Use esse tipo de constructo em vez de um struct quando for necessário usar a semântica do tipo de referência.

É possível tornar uma propriedade imutável de duas maneiras:
- É possível declarar o acessador [set](../../language-reference/keywords/set.md) como [privado](../../language-reference/keywords/private.md).  A propriedade será configurável somente dentro do tipo, mas será imutável para os consumidores.

  Ao declarar um acessador privado `set`, não é possível usar um inicializador de objeto para inicializar a propriedade. É necessário usar um construtor ou um método de fábrica.
- É possível declarar somente o acessador [get](../../language-reference/keywords/get.md), o que torna a propriedade imutável em todos os lugares, exceto no construtor do tipo.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra duas maneiras de implementar uma classe imutável que tem propriedades autoimplementadas. Entre essas maneiras, uma declara uma das propriedades com um `set` privado e outra declara uma das propriedades somente com um `get`.  A primeira classe usa um construtor somente para inicializar as propriedades e a segunda classe usa um método de fábrica estático que chama um construtor.

```csharp
// This class is immutable. After an object is created,
// it cannot be modified from outside the class. It uses a
// constructor to initialize its properties.
class Contact
{
    // Read-only properties.
    public string Name { get; }
    public string Address { get; private set; }

    // Public constructor.
    public Contact(string contactName, string contactAddress)
    {
        Name = contactName;
        Address = contactAddress;
    }
}

// This class is immutable. After an object is created,
// it cannot be modified from outside the class. It uses a
// static method and private constructor to initialize its properties.
public class Contact2
{
    // Read-only properties.
    public string Name { get; private set; }
    public string Address { get; }

    // Private constructor.
    private Contact2(string contactName, string contactAddress)
    {
        Name = contactName;
        Address = contactAddress;
    }

    // Public factory method.
    public static Contact2 CreateContact(string name, string address)
    {
        return new Contact2(name, address);
    }
}

public class Program
{
    static void Main()
    {
        // Some simple data sources.
        string[] names = {"Terry Adams","Fadi Fakhouri", "Hanying Feng",
                            "Cesar Garcia", "Debra Garcia"};
        string[] addresses = {"123 Main St.", "345 Cypress Ave.", "678 1st Ave",
                                "12 108th St.", "89 E. 42nd St."};

        // Simple query to demonstrate object creation in select clause.
        // Create Contact objects by using a constructor.
        var query1 = from i in Enumerable.Range(0, 5)
                    select new Contact(names[i], addresses[i]);

        // List elements cannot be modified by client code.
        var list = query1.ToList();
        foreach (var contact in list)
        {
            Console.WriteLine("{0}, {1}", contact.Name, contact.Address);
        }

        // Create Contact2 objects by using a static factory method.
        var query2 = from i in Enumerable.Range(0, 5)
                        select Contact2.CreateContact(names[i], addresses[i]);

        // Console output is identical to query1.
        var list2 = query2.ToList();

        // List elements cannot be modified by client code.
        // CS0272:
        // list2[0].Name = "Eugene Zabokritski";

        // Keep the console open in debug mode.
        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
}

/* Output:
    Terry Adams, 123 Main St.
    Fadi Fakhouri, 345 Cypress Ave.
    Hanying Feng, 678 1st Ave
    Cesar Garcia, 12 108th St.
    Debra Garcia, 89 E. 42nd St.
*/
```

O compilador cria campos de suporte para cada propriedade autoimplementada. Os campos não são acessíveis diretamente do código-fonte.

## <a name="see-also"></a>Consulte também

- [Propriedades](./properties.md)
- [struct](../../language-reference/keywords/struct.md)
- [Inicializadores de objeto e coleção](./object-and-collection-initializers.md)
