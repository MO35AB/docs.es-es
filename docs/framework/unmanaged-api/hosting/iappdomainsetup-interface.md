---
title: IAppDomainSetup (Interfaz)
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: IAppDomainSetup
api_location: mscoree.dll
api_type: COM
f1_keywords: IAppDomainSetup
helpviewer_keywords: IAppDomainSetup interface [.NET Framework hosting]
ms.assetid: 1844da85-c031-40bf-bea4-1a3d12a36c8c
topic_type: apiref
caps.latest.revision: "10"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: e6ed5ea00799fff70626114257efef2d06b505ae
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2017
---
# <a name="iappdomainsetup-interface"></a><span data-ttu-id="bb0d4-102">IAppDomainSetup (Interfaz)</span><span class="sxs-lookup"><span data-stu-id="bb0d4-102">IAppDomainSetup Interface</span></span>
<span data-ttu-id="bb0d4-103">Proporciona propiedades que permiten al host configurar un <xref:System.AppDomain?displayProperty=nameWithType> tipo antes de llamar a la [ICorRuntimeHost:: CreateDomainEx](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomainex-method.md) método para crearla.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-103">Provides properties that allow the host to configure an <xref:System.AppDomain?displayProperty=nameWithType> type before calling the [ICorRuntimeHost::CreateDomainEx](../../../../docs/framework/unmanaged-api/hosting/icorruntimehost-createdomainex-method.md) method to create it.</span></span>  
  
## <a name="properties"></a><span data-ttu-id="bb0d4-104">Propiedades</span><span class="sxs-lookup"><span data-stu-id="bb0d4-104">Properties</span></span>  
  
|<span data-ttu-id="bb0d4-105">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb0d4-105">Property</span></span>|<span data-ttu-id="bb0d4-106">Descripción</span><span class="sxs-lookup"><span data-stu-id="bb0d4-106">Description</span></span>|  
|--------------|-----------------|  
|<xref:System.AppDomainSetup.ApplicationBase%2A>|<span data-ttu-id="bb0d4-107">Obtiene o establece el nombre del directorio que contiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-107">Gets or sets the name of the directory that contains the application.</span></span>|  
|<xref:System.AppDomainSetup.ApplicationName%2A>|<span data-ttu-id="bb0d4-108">Obtiene o establece el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-108">Gets or sets the name of the application.</span></span>|  
|<xref:System.AppDomainSetup.CachePath%2A>|<span data-ttu-id="bb0d4-109">Obtiene o establece el nombre de un área específica de la aplicación donde archivos se han creado instantáneas.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-109">Gets or sets the name of an area specific to the application where files are shadow-copied.</span></span>|  
|<xref:System.AppDomainSetup.ConfigurationFile%2A>|<span data-ttu-id="bb0d4-110">Obtiene o establece el nombre del archivo de configuración para una aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-110">Gets or sets the name of the configuration file for an application.</span></span>|  
|<xref:System.AppDomainSetup.DynamicBase%2A>|<span data-ttu-id="bb0d4-111">Obtiene o establece el nombre del directorio donde se almacenan y se tiene acceso a los archivos dinámicamente generados.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-111">Gets or sets the name of the directory where dynamically generated files are stored and accessed.</span></span>|  
|<xref:System.AppDomainSetup.LicenseFile%2A>|<span data-ttu-id="bb0d4-112">Obtiene o establece la ruta de acceso al archivo de licencia que está asociado a este dominio.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-112">Gets or sets the path to the license file that is associated with this domain.</span></span>|  
|<xref:System.AppDomainSetup.PrivateBinPath%2A>|<span data-ttu-id="bb0d4-113">Obtiene o establece la lista de directorios que se combina con la <xref:System.AppDomainSetup.ApplicationBase%2A> directorio para buscar ensamblados privados.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-113">Gets or sets the list of directories combined with the <xref:System.AppDomainSetup.ApplicationBase%2A> directory to probe for private assemblies.</span></span>|  
|<xref:System.AppDomainSetup.PrivateBinPathProbe%2A>|<span data-ttu-id="bb0d4-114">Obtiene o establece un valor de cadena que incluye o excluye <xref:System.AppDomainSetup.ApplicationBase%2A> desde la ruta de acceso de búsqueda para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-114">Gets or sets a string value that includes or excludes <xref:System.AppDomainSetup.ApplicationBase%2A> from the search path for the application.</span></span>|  
|<xref:System.AppDomainSetup.ShadowCopyDirectories%2A>|<span data-ttu-id="bb0d4-115">Obtiene o establece los nombres de los directorios que contienen los ensamblados que se han creado instantáneas.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-115">Gets or sets the names of the directories that contain assemblies to be shadow-copied.</span></span>|  
|<xref:System.AppDomainSetup.ShadowCopyFiles%2A>|<span data-ttu-id="bb0d4-116">Obtiene o establece una cadena que indica si la operación de instantánea está activada o desactivada.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-116">Gets or sets a string that indicates whether shadow-copying is turned on or off.</span></span> <span data-ttu-id="bb0d4-117">Los valores válidos son "true" o "false".</span><span class="sxs-lookup"><span data-stu-id="bb0d4-117">Valid values are "true" or "false".</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="bb0d4-118">Comentarios</span><span class="sxs-lookup"><span data-stu-id="bb0d4-118">Remarks</span></span>  
 <span data-ttu-id="bb0d4-119">El `IAppDomainSetup` interfaz corresponde a los recursos administrados <xref:System.IAppDomainSetup> interfaz, es posible que el <xref:System.AppDomainSetup> escriba implementa.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-119">The `IAppDomainSetup` interface corresponds to the managed <xref:System.IAppDomainSetup> interface, which the <xref:System.AppDomainSetup> type implements.</span></span> <span data-ttu-id="bb0d4-120">Consulte <xref:System.IAppDomainSetup?displayProperty=nameWithType> para obtener descripciones detalladas de sus propiedades.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-120">See <xref:System.IAppDomainSetup?displayProperty=nameWithType> for detailed descriptions of its properties.</span></span>  
  
 <span data-ttu-id="bb0d4-121">`IAppDomainSetup`representa la información de enlace de ensamblado que se puede agregar a un <xref:System.AppDomain> instancia antes de su creación.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-121">`IAppDomainSetup` represents assembly binding information that can be added to an <xref:System.AppDomain> instance before its creation.</span></span> <span data-ttu-id="bb0d4-122">Por ejemplo, un host puede establecer el <xref:System.AppDomainSetup.ApplicationBase%2A> propiedad para establecer un directorio raíz, que common language runtime (CLR) busca los ensamblados administrados.</span><span class="sxs-lookup"><span data-stu-id="bb0d4-122">For example, a host can set the <xref:System.AppDomainSetup.ApplicationBase%2A> property to establish a root directory, which the common language runtime (CLR) probes for managed assemblies.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="bb0d4-123">Requisitos</span><span class="sxs-lookup"><span data-stu-id="bb0d4-123">Requirements</span></span>  
 <span data-ttu-id="bb0d4-124">**Plataformas:** vea [requisitos del sistema](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="bb0d4-124">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="bb0d4-125">**Encabezado:** MSCorEE.h</span><span class="sxs-lookup"><span data-stu-id="bb0d4-125">**Header:** MSCorEE.h</span></span>  
  
 <span data-ttu-id="bb0d4-126">**Biblioteca:** incluye como recurso en MSCorEE.dll</span><span class="sxs-lookup"><span data-stu-id="bb0d4-126">**Library:** Included as a resource in MSCorEE.dll</span></span>  
  
 <span data-ttu-id="bb0d4-127">**Versiones de .NET framework:**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="bb0d4-127">**.NET Framework Versions:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="bb0d4-128">Vea también</span><span class="sxs-lookup"><span data-stu-id="bb0d4-128">See Also</span></span>  
 <xref:System.AppDomain>  
 <xref:System.AppDomainSetup>  
 <xref:System.IAppDomainSetup>  
 [<span data-ttu-id="bb0d4-129">Interfaces de hospedaje</span><span class="sxs-lookup"><span data-stu-id="bb0d4-129">Hosting Interfaces</span></span>](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)