---
title: "ICLRDataTarget::GetThreadContext (Método)"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICLRDataTarget.GetThreadContext
api_location: mscordbi.dll
api_type: COM
f1_keywords: ICLRDataTarget::GetThreadContext
helpviewer_keywords:
- ICLRDataTarget::GetThreadContext method [.NET Framework debugging]
- GetThreadContext method, ICLRDataTarget interface [.NET Framework debugging]
ms.assetid: b9d8c3b5-3a2e-4225-95d4-dd052c4532c3
topic_type: apiref
caps.latest.revision: "11"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 480b65b279dbc0359b078f3f1d543f63a0bfd0f6
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2017
---
# <a name="iclrdatatargetgetthreadcontext-method"></a><span data-ttu-id="4b002-102">ICLRDataTarget::GetThreadContext (Método)</span><span class="sxs-lookup"><span data-stu-id="4b002-102">ICLRDataTarget::GetThreadContext Method</span></span>
<span data-ttu-id="4b002-103">Obtiene el contexto de ejecución actual para el subproceso determinado en el proceso de destino.</span><span class="sxs-lookup"><span data-stu-id="4b002-103">Gets the current execution context for the given thread in the target process.</span></span> <span data-ttu-id="4b002-104">Los servicios de acceso a datos de common language runtime llama a este método.</span><span class="sxs-lookup"><span data-stu-id="4b002-104">This method is called by the common language runtime data access services.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="4b002-105">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="4b002-105">Syntax</span></span>  
  
```  
HRESULT GetThreadContext (  
    [in] ULONG32            threadID,  
    [in] ULONG32            contextFlags,  
    [in] ULONG32            contextSize,  
    [out, size_is(contextSize)]   
        BYTE                *context  
);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="4b002-106">Parámetros</span><span class="sxs-lookup"><span data-stu-id="4b002-106">Parameters</span></span>  
 `threadID`  
 <span data-ttu-id="4b002-107">[in] El identificador de sistema operativo de un subproceso en el proceso de destino.</span><span class="sxs-lookup"><span data-stu-id="4b002-107">[in] The operating system identifier of a thread in the target process.</span></span>  
  
 `contextFlags`  
 <span data-ttu-id="4b002-108">[in] Marcas que especifican qué partes del contexto se devuelven.</span><span class="sxs-lookup"><span data-stu-id="4b002-108">[in] Flags that specify which parts of the context to return.</span></span> <span data-ttu-id="4b002-109">La implementación devolverá por lo menos estas partes del contexto.</span><span class="sxs-lookup"><span data-stu-id="4b002-109">The implementation will return at least these parts of the context.</span></span>  
  
 `contextSize`  
 <span data-ttu-id="4b002-110">[in] El tamaño del contexto.</span><span class="sxs-lookup"><span data-stu-id="4b002-110">[in] The size of the context.</span></span>  
  
 `context`  
 <span data-ttu-id="4b002-111">[out] Puntero a un búfer en el que se va a colocar el contexto.</span><span class="sxs-lookup"><span data-stu-id="4b002-111">[out] Pointer to a buffer in which to place the context.</span></span>  
  
 <span data-ttu-id="4b002-112">Los datos de la `context` búfer debe estar en el formato de Win32 `CONTEXT` estructura.</span><span class="sxs-lookup"><span data-stu-id="4b002-112">The data in the `context` buffer must be in the format of the Win32 `CONTEXT` structure.</span></span> <span data-ttu-id="4b002-113">El contexto especifica los datos de registro específica del procesador, por lo que la definición de Win32 `CONTEXT` estructura depende de la arquitectura del procesador.</span><span class="sxs-lookup"><span data-stu-id="4b002-113">The context specifies processor-specific register data, so the definition of the Win32 `CONTEXT` structure depends on the processor's architecture.</span></span> <span data-ttu-id="4b002-114">Consulte el archivo de encabezado WinNT.h para la definición de Win32 `CONTEXT` estructura.</span><span class="sxs-lookup"><span data-stu-id="4b002-114">Refer to the WinNT.h header file for the definition of the Win32 `CONTEXT` structure.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="4b002-115">Comentarios</span><span class="sxs-lookup"><span data-stu-id="4b002-115">Remarks</span></span>  
 <span data-ttu-id="4b002-116">Este método lo implementa el escritor de la aplicación de depuración.</span><span class="sxs-lookup"><span data-stu-id="4b002-116">This method is implemented by the writer of the debugging application.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="4b002-117">Requisitos</span><span class="sxs-lookup"><span data-stu-id="4b002-117">Requirements</span></span>  
 <span data-ttu-id="4b002-118">**Plataformas:** vea [requisitos del sistema](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="4b002-118">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="4b002-119">**Encabezado:** ClrData.idl, ClrData.h</span><span class="sxs-lookup"><span data-stu-id="4b002-119">**Header:** ClrData.idl, ClrData.h</span></span>  
  
 <span data-ttu-id="4b002-120">**Biblioteca:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="4b002-120">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="4b002-121">**Versiones de .NET framework:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="4b002-121">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="4b002-122">Vea también</span><span class="sxs-lookup"><span data-stu-id="4b002-122">See Also</span></span>  
 [<span data-ttu-id="4b002-123">ICLRDataTarget (interfaz)</span><span class="sxs-lookup"><span data-stu-id="4b002-123">ICLRDataTarget Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/iclrdatatarget-interface.md)