---
title: Procedimiento Configurar un personalizados WS-Metadata Exchange Binding
ms.date: 03/30/2017
helpviewer_keywords:
- WS-Metadata Exchange [WCF]
- WS-Metadata Exchange [WCF], configuring a custom binding
ms.assetid: cdba4d73-da64-4805-bc56-9822becfd1e4
ms.openlocfilehash: 4328306a6b67d2eac498ec48d1769bdf4bd5f81e
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54642469"
---
# <a name="how-to-configure-a-custom-ws-metadata-exchange-binding"></a>Procedimiento Configurar un personalizados WS-Metadata Exchange Binding
En este tema se explica cómo configurar un enlace personalizado de intercambio de WS-Metadata. Windows Communication Foundation (WCF) incluye cuatro enlaces de metadatos definido por el sistema, pero puede publicar metadatos usando cualquier enlace que desee. En este tema, se mostrará cómo publicar metadatos mediante `wsHttpBinding`. Este enlace le da la opción de exponer los metadatos de una manera segura. El código de este artículo se basa en el [Introducción](../../../../docs/framework/wcf/samples/getting-started-sample.md).  
  
### <a name="using-a-configuration-file"></a>El uso de un archivo de configuración  
  
1.  En el archivo de configuración del servicio, agregue un comportamiento del servicio que contenga la etiqueta `serviceMetadata`:  
  
    ```xml  
    <behaviors>  
       <serviceBehaviors>  
         <behavior name="CalculatorServiceBehavior">  
           <serviceMetadata httpGetEnabled="True"/>  
         </behavior>  
       </serviceBehaviors>  
    </behaviors>  
    ```  
  
2.  Agregue un atributo `behaviorConfiguration` a la etiqueta del servicio que hace referencia a este nuevo comportamiento:  
  
    ```xml  
    <service        name="Microsoft.ServiceModel.Samples.CalculatorService"  
    behaviorConfiguration="CalculatorServiceBehavior">   
    ```  
  
3.  Agregue un extremo de metadatos que especifique mex como dirección, `wsHttpBinding` como enlace, y <xref:System.ServiceModel.Description.IMetadataExchange> como contrato:  
  
    ```xml  
    <endpoint address="mex"  
              binding="wsHttpBinding"  
              contract="IMetadataExchange" />  
    ```  
  
4.  Para comprobar que el extremo de intercambio de metadatos está funcionando correctamente, agregue una etiqueta de extremo en el archivo de configuración del cliente:  
  
    ```xml  
    <endpoint name="MyMexEndpoint"               address="http://localhost:8000/servicemodelsamples/service/mex"  
              binding="wsHttpBinding"  
              contract="IMetadataExchange"/>  
    ```  
  
5.  En el método Main () del cliente, cree una nueva instancia <xref:System.ServiceModel.Description.MetadataExchangeClient>, establezca su propiedad <xref:System.ServiceModel.Description.MetadataExchangeClient.ResolveMetadataReferences%2A> en `true`, llame <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> y, a continuación, recorra en iteración la colección de metadatos devuelta:  
  
    ```  
    string mexAddress = "http://localhost:8000/servicemodelsamples/service/mex";  
  
    MetadataExchangeClient mexClient = new MetadataExchangeClient("MyMexEndpoint");  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet mdSet = mexClient.GetMetadata(new EndpointAddress(mexAddress));  
    foreach (MetadataSection section in mdSet.MetadataSections)  
    Console.WriteLine("Metadata section: " + section.Dialect.ToString());  
    ```  
  
### <a name="configuring-by-code"></a>Configurar por código  
  
1.  Cree una instancia de enlace <xref:System.ServiceModel.WSHttpBinding>:  
  
    ```  
    WSHttpBinding binding = new WSHttpBinding();  
    ```  
  
2.  Cree una instancia <xref:System.ServiceModel.ServiceHost>.  
  
    ```  
    ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress);  
    ```  
  
3.  Agregue un extremo de servicio y agregue una instancia <xref:System.ServiceModel.Description.ServiceMetadataBehavior>:  
  
    ```  
    serviceHost.AddServiceEndpoint(typeof(ICalculator), binding, baseAddress);  
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();  
    smb.HttpGetEnabled = true;  
    serviceHost.Description.Behaviors.Add(smb);  
    ```  
  
4.  Agregue un extremo de intercambio de metadatos, especificando <xref:System.ServiceModel.WSHttpBinding> creado anteriormente:  
  
    ```  
    serviceHost.AddServiceEndpoint(typeof(IMetadataExchange), binding, mexAddress);  
    ```  
  
5.  Para comprobar que el extremo de intercambio de metadatos está funcionando correctamente, agregue una etiqueta de extremo en el archivo de configuración del cliente:  
  
    ```xml  
    <endpoint name="MyMexEndpoint"               address="http://localhost:8000/servicemodelsamples/service/mex"  
              binding="wsHttpBinding"  
              contract="IMetadataExchange"/>  
    ```  
  
6.  En el método Main () del cliente, cree una nueva instancia <xref:System.ServiceModel.Description.MetadataExchangeClient>, establezca la propiedad <xref:System.ServiceModel.Description.MetadataExchangeClient.ResolveMetadataReferences%2A> en `true`, llame <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> y, a continuación, recorra en iteración la colección de metadatos devuelta:  
  
    ```  
    string mexAddress = "http://localhost:8000/servicemodelsamples/service/mex";  
  
    MetadataExchangeClient mexClient = new MetadataExchangeClient("MyMexEndpoint");  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet mdSet = mexClient.GetMetadata(new EndpointAddress(mexAddress));  
    foreach (MetadataSection section in mdSet.MetadataSections)  
    Console.WriteLine("Metadata section: " + section.Dialect.ToString());  
    ```  
  
## <a name="see-also"></a>Vea también
- [Comportamiento de publicación de metadatos](../../../../docs/framework/wcf/samples/metadata-publishing-behavior.md)
- [Recuperación de metadatos](../../../../docs/framework/wcf/samples/retrieve-metadata.md)
- [Metadatos](../../../../docs/framework/wcf/feature-details/metadata.md)
- [Publicación de metadatos](../../../../docs/framework/wcf/feature-details/publishing-metadata.md)
- [Publicación de puntos de conexión de metadatos](../../../../docs/framework/wcf/publishing-metadata-endpoints.md)
