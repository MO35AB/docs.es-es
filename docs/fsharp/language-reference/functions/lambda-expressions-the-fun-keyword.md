---
title: 'Expresiones lambda: la palabra clave fun (F#)'
description: "Obtenga información acerca de cómo utilizar la palabra clave 'fun' de F # para definir una expresión lambda, que es una función anónima."
keywords: "visual f#, f#, programación funcional"
author: cartermp
ms.author: phcart
ms.date: 05/16/2016
ms.topic: language-reference
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: e5d3565c-d7cc-433f-a619-886ed92523a7
ms.openlocfilehash: 09f1c1787bbb4b86ec40f9e973e08104919631aa
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2017
---
# <a name="lambda-expressions-the-fun-keyword-f"></a><span data-ttu-id="519d7-104">Expresiones lambda: la palabra clave fun (F#)</span><span class="sxs-lookup"><span data-stu-id="519d7-104">Lambda Expressions: The fun Keyword (F#)</span></span>

<span data-ttu-id="519d7-105">El `fun` palabra clave se utiliza para definir una expresión lambda, es decir, una función anónima.</span><span class="sxs-lookup"><span data-stu-id="519d7-105">The `fun` keyword is used to define a lambda expression, that is, an anonymous function.</span></span>


## <a name="syntax"></a><span data-ttu-id="519d7-106">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="519d7-106">Syntax</span></span>

```fsharp
fun parameter-list -> expression
```

## <a name="remarks"></a><span data-ttu-id="519d7-107">Comentarios</span><span class="sxs-lookup"><span data-stu-id="519d7-107">Remarks</span></span>
<span data-ttu-id="519d7-108">El *lista de parámetros* normalmente consta de nombres y, opcionalmente, tipos de parámetros.</span><span class="sxs-lookup"><span data-stu-id="519d7-108">The *parameter-list* typically consists of names and, optionally, types of parameters.</span></span> <span data-ttu-id="519d7-109">Por lo general, el *lista de parámetros* puede estar formada por cualquier patrón de F #.</span><span class="sxs-lookup"><span data-stu-id="519d7-109">More generally, the *parameter-list* can be composed of any F# patterns.</span></span> <span data-ttu-id="519d7-110">Para obtener una lista completa de los modelos posibles, consulte [coincidencia de patrones](../pattern-matching.md).</span><span class="sxs-lookup"><span data-stu-id="519d7-110">For a full list of possible patterns, see [Pattern Matching](../pattern-matching.md).</span></span> <span data-ttu-id="519d7-111">Las listas de parámetros válidos incluyen los siguientes ejemplos.</span><span class="sxs-lookup"><span data-stu-id="519d7-111">Lists of valid parameters include the following examples.</span></span>

```fsharp
// Lambda expressions with parameter lists.
fun a b c -> ...
fun (a: int) b c -> ...
fun (a : int) (b : string) (c:float) -> ...

// A lambda expression with a tuple pattern.
fun (a, b) -> …

// A lambda expression with a list pattern.
fun head :: tail -> …
```

<span data-ttu-id="519d7-112">El *expresión* es el cuerpo de la función, la última expresión que genera un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="519d7-112">The *expression* is the body of the function, the last expression of which generates a return value.</span></span> <span data-ttu-id="519d7-113">A continuación se indican algunos ejemplos de expresiones lambda válidas:</span><span class="sxs-lookup"><span data-stu-id="519d7-113">Examples of valid lambda expressions include the following:</span></span>

[!code-fsharp[Main](../../../../samples/snippets/fsharp/lang-ref-1/snippet301.fs)]
    
## <a name="using-lambda-expressions"></a><span data-ttu-id="519d7-114">Uso de expresiones lambda</span><span class="sxs-lookup"><span data-stu-id="519d7-114">Using Lambda Expressions</span></span>
<span data-ttu-id="519d7-115">Las expresiones lambda son especialmente útiles cuando desea realizar operaciones en una lista o en otra colección y desea ahorrarse el trabajo de la definición de una función.</span><span class="sxs-lookup"><span data-stu-id="519d7-115">Lambda expressions are especially useful when you want to perform operations on a list or other collection and want to avoid the extra work of defining a function.</span></span> <span data-ttu-id="519d7-116">Muchas funciones de biblioteca de F # toman valores de las funciones como argumentos, y puede ser especialmente útil usar una expresión lambda en esos casos.</span><span class="sxs-lookup"><span data-stu-id="519d7-116">Many F# library functions take function values as arguments, and it can be especially convenient to use a lambda expression in those cases.</span></span> <span data-ttu-id="519d7-117">El código siguiente aplica una expresión lambda a los elementos de una lista.</span><span class="sxs-lookup"><span data-stu-id="519d7-117">The following code applies a lambda expression to elements of a list.</span></span> <span data-ttu-id="519d7-118">En este caso, la función anónima suma 1 a cada elemento de una lista.</span><span class="sxs-lookup"><span data-stu-id="519d7-118">In this case, the anonymous function adds 1 to every element of a list.</span></span>

[!code-fsharp[Main](../../../../samples/snippets/fsharp/lang-ref-1/snippet302.fs)]
    
## <a name="see-also"></a><span data-ttu-id="519d7-119">Vea también</span><span class="sxs-lookup"><span data-stu-id="519d7-119">See Also</span></span>
[<span data-ttu-id="519d7-120">Funciones</span><span class="sxs-lookup"><span data-stu-id="519d7-120">Functions</span></span>](index.md)