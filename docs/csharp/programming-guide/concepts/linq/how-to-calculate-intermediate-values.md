---
title: Procedimiento para calcular valores intermedios (C#)
ms.date: 07/20/2015
ms.assetid: 7fd3001f-f8f9-4bce-879f-d4c7af8a04fe
ms.openlocfilehash: 12c8a87114859ce7b47b8206acb454cdc2838470
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54604336"
---
# <a name="how-to-calculate-intermediate-values-c"></a>Procedimiento para calcular valores intermedios (C#)
Este ejemplo muestra cómo calcular valores intermedios que se pueden usar para ordenar, filtrar y seleccionar.  
  
## <a name="example"></a>Ejemplo  
 El siguiente ejemplo utiliza la cláusula `Let`.  
  
 Este ejemplo utiliza el siguiente documento XML: [Archivo XML de ejemplo: Datos numéricos (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-numerical-data-linq-to-xml.md).  
  
```csharp  
XElement root = XElement.Load("Data.xml");  
IEnumerable<decimal> extensions =  
    from el in root.Elements("Data")  
    let extension = (decimal)el.Element("Quantity") * (decimal)el.Element("Price")  
    where extension >= 25  
    orderby extension  
    select extension;  
foreach (decimal ex in extensions)  
    Console.WriteLine(ex);  
```  
  
 Este código genera el siguiente resultado:  
  
```  
55.92  
73.50  
89.99  
198.00  
435.00  
```  
  
## <a name="example"></a>Ejemplo  
 El siguiente ejemplo muestra la misma consulta sobre un XML que se encuentra en un espacio de nombres. Para obtener más información, vea [Working with XML Namespaces (C#)](../../../../csharp/programming-guide/concepts/linq/working-with-xml-namespaces.md) (Trabajar con espacios de nombres XML [C#]).  
  
 Este ejemplo utiliza el siguiente documento XML: [Archivo XML de ejemplo: Datos numéricos de un espacio de nombres](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-numerical-data-in-a-namespace.md).  
  
```csharp  
XElement root = XElement.Load("DataInNamespace.xml");  
XNamespace ad = "http://www.adatum.com";  
IEnumerable<decimal> extensions =  
    from el in root.Elements(ad + "Data")  
    let extension = (decimal)el.Element(ad + "Quantity") * (decimal)el.Element(ad + "Price")  
    where extension >= 25  
    orderby extension  
    select extension;  
foreach (decimal ex in extensions)  
    Console.WriteLine(ex);  
```  
  
 Este código genera el siguiente resultado:  
  
```  
55.92  
73.50  
89.99  
198.00  
435.00  
```  
  
## <a name="see-also"></a>Vea también

- [Consultas básicas (LINQ to XML) (C#)](../../../../csharp/programming-guide/concepts/linq/basic-queries-linq-to-xml.md)
