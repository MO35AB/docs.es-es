---
title: 'Convenciones de código de F #'
description: 'Obtenga información acerca de expresiones y directrices generales al escribir código de F #.'
ms.date: 05/14/2018
ms.openlocfilehash: f3d16f735ddc1901aeaa5ebb39e2fa2b70a3d836
ms.sourcegitcommit: 43924acbdbb3981d103e11049bbe460457d42073
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2018
---
# <a name="f-coding-conventions"></a><span data-ttu-id="f98ca-103">Convenciones de código de F #</span><span class="sxs-lookup"><span data-stu-id="f98ca-103">F# coding conventions</span></span>

<span data-ttu-id="f98ca-104">Las siguientes convenciones Formula de experiencia de trabajar con grandes F # dotarlo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-104">The following conventions are formulated from experience working with large F# codebases.</span></span> <span data-ttu-id="f98ca-105">El [cinco principios de buen código F #](index.md#five-principles-of-good-f-code) constituyen el fundamento de cada recomendación.</span><span class="sxs-lookup"><span data-stu-id="f98ca-105">The [Five principles of good F# code](index.md#five-principles-of-good-f-code) are the foundation of each recommendation.</span></span> <span data-ttu-id="f98ca-106">Están relacionados con el [directrices de diseño del componente de F #](component-design-guidelines.md), pero son aplicables a cualquier código de F #, no sólo componentes, como las bibliotecas.</span><span class="sxs-lookup"><span data-stu-id="f98ca-106">They are related to the [F# component design guidelines](component-design-guidelines.md), but are applicable for any F# code, not just components such as libraries.</span></span>

## <a name="organizing-code"></a><span data-ttu-id="f98ca-107">Organización del código</span><span class="sxs-lookup"><span data-stu-id="f98ca-107">Organizing code</span></span>

<span data-ttu-id="f98ca-108">F # incluye dos métodos principales para organizar el código: módulos y espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="f98ca-108">F# features two primary ways to organize code: modules and namespaces.</span></span> <span data-ttu-id="f98ca-109">Estos son similares, pero tienen las siguientes diferencias:</span><span class="sxs-lookup"><span data-stu-id="f98ca-109">These are similar, but do have the following differences:</span></span>

* <span data-ttu-id="f98ca-110">Espacios de nombres se compilan como espacios de nombres. NET.</span><span class="sxs-lookup"><span data-stu-id="f98ca-110">Namespaces are compiled as .NET namespaces.</span></span> <span data-ttu-id="f98ca-111">Los módulos se compilan como las clases estáticas.</span><span class="sxs-lookup"><span data-stu-id="f98ca-111">Modules are compiled as static classes.</span></span>
* <span data-ttu-id="f98ca-112">Espacios de nombres siempre son de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="f98ca-112">Namespaces are always top level.</span></span> <span data-ttu-id="f98ca-113">Pueden ser módulos anidados dentro de otros módulos y de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="f98ca-113">Modules can be top-level and nested within other modules.</span></span>
* <span data-ttu-id="f98ca-114">Espacios de nombres pueden abarcar varios archivos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-114">Namespaces can span multiple files.</span></span> <span data-ttu-id="f98ca-115">No se pueden módulos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-115">Modules cannot.</span></span>
* <span data-ttu-id="f98ca-116">Los módulos se pueden decorar con `[<RequireQualifiedAccess>]` y `[<AutoOpen>]`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-116">Modules can be decorated with `[<RequireQualifiedAccess>]` and `[<AutoOpen>]`.</span></span>

<span data-ttu-id="f98ca-117">Las instrucciones siguientes le ayudará a utilizar para organizar el código.</span><span class="sxs-lookup"><span data-stu-id="f98ca-117">The following guidelines will help you use these to organize your code.</span></span>

### <a name="prefer-namespaces-at-the-top-level"></a><span data-ttu-id="f98ca-118">Prefiere espacios de nombres en el nivel superior</span><span class="sxs-lookup"><span data-stu-id="f98ca-118">Prefer namespaces at the top level</span></span>

<span data-ttu-id="f98ca-119">Para cualquier código públicamente consumible, espacios de nombres son preferenciales para módulos en el nivel superior.</span><span class="sxs-lookup"><span data-stu-id="f98ca-119">For any publicly consumable code, namespaces are preferential to modules at the top level.</span></span> <span data-ttu-id="f98ca-120">Dado que se compilen como espacios de nombres. NET, son de C# pueden utilizar con ningún problema.</span><span class="sxs-lookup"><span data-stu-id="f98ca-120">Because they are compiled as .NET namespaces, they are consumable from C# with no issue.</span></span>

```fsharp
// Good!
namespace MyCode

type MyClass() =
    ...
```

<span data-ttu-id="f98ca-121">Uso de un módulo de nivel superior no puede parecer diferente cuando se les llame solo desde F #, pero para los consumidores de C#, quizá le sorprenda llamadores al tener que calificar `MyClass` con el `MyCode` módulo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-121">Using a top-level module may not appear different when called only from F#, but for C# consumers, callers may be surprised by having to qualify `MyClass` with the `MyCode` module.</span></span>

```fsharp
// Bad!
module MyCode

type MyClass() =
    ...
```

### <a name="carefully-apply-autoopen"></a><span data-ttu-id="f98ca-122">Se aplican con cuidado `[<AutoOpen>]`</span><span class="sxs-lookup"><span data-stu-id="f98ca-122">Carefully apply `[<AutoOpen>]`</span></span>

<span data-ttu-id="f98ca-123">El `[<AutoOpen>]` construcción puede contamina el ámbito de lo que está disponible para los llamadores y la respuesta a algo procedencia es "mágica".</span><span class="sxs-lookup"><span data-stu-id="f98ca-123">The `[<AutoOpen>]` construct can pollute the scope of what is available to callers, and the answer to where something comes from is "magic".</span></span> <span data-ttu-id="f98ca-124">Esto no suele ser muy útil.</span><span class="sxs-lookup"><span data-stu-id="f98ca-124">This is generally not a good thing.</span></span> <span data-ttu-id="f98ca-125">Una excepción a esta regla es la biblioteca básica de F # propio (aunque este hecho es un poco controvertido).</span><span class="sxs-lookup"><span data-stu-id="f98ca-125">An exception to this rule is the F# Core Library itself (though this fact is also a bit controversial).</span></span>

<span data-ttu-id="f98ca-126">Sin embargo, resulta una comodidad si dispone de funcionalidad de la aplicación auxiliar para una API pública que desea organizar por separado a través de esa API pública.</span><span class="sxs-lookup"><span data-stu-id="f98ca-126">However, it is a convenience if you have helper functionality for a public API that you wish to organize separately from that public API.</span></span>

```fsharp
module MyAPI =
    [<AutoOpen>]
    module private Helpers =
        let helper1 x y z =
            ...


    let myFunction1 x =
        let y = ...
        let z = ...

        helper1 x y z
```

<span data-ttu-id="f98ca-127">Esto le permite detalles de implementación independiente correctamente a través de la API pública de una función sin tener que calificar totalmente un Ayudante cada vez que lo llama.</span><span class="sxs-lookup"><span data-stu-id="f98ca-127">This lets you cleanly separate implementation details from the public API of a function without having to fully qualify a helper each time you call it.</span></span>

<span data-ttu-id="f98ca-128">Además, exponer los métodos de extensión y generadores de expresiones en el nivel de espacio de nombres puede claridad expresarse con `[<AutoOpen>]`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-128">Additionally, exposing extension methods and expression builders at the namespace level can be neatly expressed with `[<AutoOpen>]`.</span></span>

### <a name="use-requirequalifiedaccess-whenever-names-could-conflict-or-you-feel-it-helps-with-readability"></a><span data-ttu-id="f98ca-129">Use `[<RequireQualifiedAccess>]` siempre nombres pudieron entrar en conflicto o le parecería que ayuda a mejorar la legibilidad</span><span class="sxs-lookup"><span data-stu-id="f98ca-129">Use `[<RequireQualifiedAccess>]` whenever names could conflict or you feel it helps with readability</span></span>

<span data-ttu-id="f98ca-130">Agregar el `[<RequireQualifiedAccess>]` atributo a un módulo indica que no se puede abrir el módulo y que las referencias a los elementos del módulo requieren explícita calificar el acceso.</span><span class="sxs-lookup"><span data-stu-id="f98ca-130">Adding the `[<RequireQualifiedAccess>]` attribute to a module indicates that the module may not be opened and that references to the elements of the module require explicit qualified access.</span></span> <span data-ttu-id="f98ca-131">Por ejemplo, el `Microsoft.FSharp.Collections.List` módulo tiene este atributo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-131">For example, the `Microsoft.FSharp.Collections.List` module has this attribute.</span></span>

<span data-ttu-id="f98ca-132">Esto es útil cuando las funciones y los valores en el módulo tienen nombres que es probables que entran en conflicto con los nombres de otros módulos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-132">This is useful when functions and values in the module have names that are likely to conflict with names in other modules.</span></span> <span data-ttu-id="f98ca-133">Que necesiten acceso completo puede aumentar considerablemente el mantenimiento a largo plazo y evolvability de una biblioteca.</span><span class="sxs-lookup"><span data-stu-id="f98ca-133">Requiring qualified access can greatly increase the long-term maintainability and evolvability of a library.</span></span>

```fsharp
[<RequireQualifiedAccess>]
module StringTokenization =
    let parse s = ...

...

let s = getAString()
let parsed = StringTokenization.parse s // Must qualify to use 'parse'
```

### <a name="sort-open-statements-topologically"></a><span data-ttu-id="f98ca-134">Ordenación `open` instrucciones topológicamente</span><span class="sxs-lookup"><span data-stu-id="f98ca-134">Sort `open` statements topologically</span></span>

<span data-ttu-id="f98ca-135">En F #, el orden de las declaraciones es importante, incluidos con `open` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="f98ca-135">In F#, the order of declarations matters, including with `open` statements.</span></span> <span data-ttu-id="f98ca-136">Esto es distinto de C#, donde el efecto de `using` y `using static` es independiente de la orden de esas instrucciones en un archivo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-136">This is unlike C#, where the effect of `using` and `using static` is independent of the ordering of those statements in a file.</span></span>

<span data-ttu-id="f98ca-137">En F #, pueden reemplazar elementos abiertos en un ámbito otros ya está presente.</span><span class="sxs-lookup"><span data-stu-id="f98ca-137">In F#, elements opened into a scope can shadow others already present.</span></span> <span data-ttu-id="f98ca-138">Esto significa que la reordenación `open` instrucciones podrían alterar el significado del código.</span><span class="sxs-lookup"><span data-stu-id="f98ca-138">This means that reordering `open` statements could alter the meaning of code.</span></span> <span data-ttu-id="f98ca-139">Como resultado, cualquier arbitrario ordenar de todos los `open` instrucciones (por ejemplo, por orden alfanumérico) generalmente no se recomienda, menos generar un comportamiento diferente que cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="f98ca-139">As a result, any arbitrary sorting of all `open` statements (for example, alphanumerically) is generally not recommended, lest you generate different behavior that you might expect.</span></span>

<span data-ttu-id="f98ca-140">En su lugar, se recomienda que las ordenar [topológicamente](https://en.wikipedia.org/wiki/Topological_sorting); es decir, ordenar su `open` las instrucciones en el orden en que _capas_ del sistema se definen.</span><span class="sxs-lookup"><span data-stu-id="f98ca-140">Instead, we recommend that you sort them [topologically](https://en.wikipedia.org/wiki/Topological_sorting); that is, order your `open` statements in the order in which _layers_ of your system are defined.</span></span> <span data-ttu-id="f98ca-141">También se puede considerar realizar alfanumérico de ordenación en diferentes capas topológicas.</span><span class="sxs-lookup"><span data-stu-id="f98ca-141">Doing alphanumeric sorting within different topological layers may also be considered.</span></span>

<span data-ttu-id="f98ca-142">Por ejemplo, mostramos la ordenación topológica para el archivo F # compilador servicio público API:</span><span class="sxs-lookup"><span data-stu-id="f98ca-142">As an example, here is the topological sorting for the F# compiler service public API file:</span></span>

```fsharp
namespace Microsoft.FSharp.Compiler.SourceCodeServices

open System
open System.Collections.Generic
open System.Collections.Concurrent
open System.Diagnostics
open System.IO
open System.Reflection
open System.Text

open Microsoft.FSharp.Compiler
open Microsoft.FSharp.Compiler.AbstractIL
open Microsoft.FSharp.Compiler.AbstractIL.Diagnostics
open Microsoft.FSharp.Compiler.AbstractIL.IL
open Microsoft.FSharp.Compiler.AbstractIL.ILBinaryReader
open Microsoft.FSharp.Compiler.AbstractIL.Internal
open Microsoft.FSharp.Compiler.AbstractIL.Internal.Library

open Microsoft.FSharp.Compiler.AccessibilityLogic
open Microsoft.FSharp.Compiler.Ast
open Microsoft.FSharp.Compiler.CompileOps
open Microsoft.FSharp.Compiler.CompileOptions
open Microsoft.FSharp.Compiler.Driver
open Microsoft.FSharp.Compiler.ErrorLogger
open Microsoft.FSharp.Compiler.Infos
open Microsoft.FSharp.Compiler.InfoReader
open Microsoft.FSharp.Compiler.Lexhelp
open Microsoft.FSharp.Compiler.Layout
open Microsoft.FSharp.Compiler.Lib
open Microsoft.FSharp.Compiler.NameResolution
open Microsoft.FSharp.Compiler.PrettyNaming
open Microsoft.FSharp.Compiler.Parser
open Microsoft.FSharp.Compiler.Range
open Microsoft.FSharp.Compiler.Tast
open Microsoft.FSharp.Compiler.Tastops
open Microsoft.FSharp.Compiler.TcGlobals
open Microsoft.FSharp.Compiler.TypeChecker
open Microsoft.FSharp.Compiler.SourceCodeServices.SymbolHelpers

open Internal.Utilities
open Internal.Utilities.Collections
```

<span data-ttu-id="f98ca-143">Tenga en cuenta que un salto de línea separa las capas de topología con cada capa que se ordenan alfabéticamente posteriormente.</span><span class="sxs-lookup"><span data-stu-id="f98ca-143">Note that a line break separates topological layers, with each layer being sorted alphanumerically afterwards.</span></span> <span data-ttu-id="f98ca-144">Esto organiza limpiamente código sin accidentalmente sombrear valores.</span><span class="sxs-lookup"><span data-stu-id="f98ca-144">This cleanly organizes code without accidentally shadowing values.</span></span>

## <a name="use-classes-to-contain-values-that-have-side-effects"></a><span data-ttu-id="f98ca-145">Usar las clases para contener valores que tienen efectos secundarios</span><span class="sxs-lookup"><span data-stu-id="f98ca-145">Use classes to contain values that have side effects</span></span>

<span data-ttu-id="f98ca-146">Hay muchas veces al inicializar un valor puede tener efectos secundarios, como crear instancias de un contexto de una base de datos u otro recurso remoto.</span><span class="sxs-lookup"><span data-stu-id="f98ca-146">There are many times when initializing a value can have side effects, such as instantiating a context to a database or other remote resource.</span></span> <span data-ttu-id="f98ca-147">Es tentador inicializar estas cosas en un módulo y usar en las funciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="f98ca-147">It is tempting to initialize such things in a module and use it in subsequent functions:</span></span>

```fsharp
// This is bad!
module MyApi =
    let dep1 = File.ReadAllText "/Users/{your name}/connectionstring.txt"
    let dep2 = Environment.GetEnvironmentVariable "DEP_2"

    let private r = Random()
    let dep3() = r.Next() // Problematic if multiple threads use this

    let function1 arg = doStuffWith dep1 dep2 dep3 arg
    let function2 arg = doSutffWith dep1 dep2 dep3 arg
```

<span data-ttu-id="f98ca-148">Esto suele ser una mala idea por diversas razones:</span><span class="sxs-lookup"><span data-stu-id="f98ca-148">This is frequently a bad idea for a few reasons:</span></span>

<span data-ttu-id="f98ca-149">En primer lugar, se inserta la configuración de la aplicación en el código base con `dep1` y `dep2`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-149">First, application configuration is pushed into the codebase with `dep1` and `dep2`.</span></span> <span data-ttu-id="f98ca-150">Esto es difícil de mantener en dotarlo mayor.</span><span class="sxs-lookup"><span data-stu-id="f98ca-150">This is difficult to maintain in larger codebases.</span></span>

<span data-ttu-id="f98ca-151">En segundo lugar, inicializadas estáticamente datos no deben incluir valores que no son seguros para subprocesos si su componente utilice varios subprocesos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-151">Second, statically initialized data should not include values that are not thread safe if your component will itself use multiple threads.</span></span> <span data-ttu-id="f98ca-152">Esto se infringe claramente `dep3`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-152">This is clearly violated by `dep3`.</span></span>

<span data-ttu-id="f98ca-153">Por último, inicialización del módulo se compila en un constructor estático para la unidad de compilación completo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-153">Finally, module initialization compiles into a static constructor for the entire compilation unit.</span></span> <span data-ttu-id="f98ca-154">Si se produce un error en la inicialización de un valor de límite permiten en ese módulo, se manifiesta como un `TypeInitializationException` que se almacena en la caché durante toda la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f98ca-154">If any error occurs in let-bound value initialization in that module, it manifests as a `TypeInitializationException` that is then cached for the entire lifetime of the application.</span></span> <span data-ttu-id="f98ca-155">Esto puede ser difícil de diagnosticar.</span><span class="sxs-lookup"><span data-stu-id="f98ca-155">This can be difficult to diagnose.</span></span> <span data-ttu-id="f98ca-156">Normalmente hay una excepción interna que puede intentar volver a analizar, pero, a continuación, si no existe, no hay ningún indicando la causa raíz.</span><span class="sxs-lookup"><span data-stu-id="f98ca-156">There is usually an inner exception that you can attempt to reason about, but if there is not, then there is no telling what the root cause is.</span></span>

<span data-ttu-id="f98ca-157">En su lugar, puede utilizar una clase simple para almacenar las dependencias:</span><span class="sxs-lookup"><span data-stu-id="f98ca-157">Instead, just use a simple class to hold dependencies:</span></span>

```fsharp
type MyParametricApi(dep1, dep2, dep3) =
    member __.Function1 arg1 = doStuffWith dep1 dep2 dep3 arg1
    member __.Function2 arg2 = doStuffWith dep1 dep2 dep3 arg2
```

<span data-ttu-id="f98ca-158">Esto permite lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f98ca-158">This enables the following:</span></span>

1. <span data-ttu-id="f98ca-159">Insertar cualquier estado dependiente fuera de la propia API.</span><span class="sxs-lookup"><span data-stu-id="f98ca-159">Pushing any dependent state outside of the API itself.</span></span>
2. <span data-ttu-id="f98ca-160">Ahora es posible fuera de la API de configuración.</span><span class="sxs-lookup"><span data-stu-id="f98ca-160">Configuration can now be done outside of the API.</span></span>
3. <span data-ttu-id="f98ca-161">Errores de inicialización de los valores dependientes no suelen presentarse como un `TypeInitializationException`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-161">Errors in initialization for dependent values are not likely to manifest as a `TypeInitializationException`.</span></span>
4. <span data-ttu-id="f98ca-162">La API ahora es más fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="f98ca-162">The API is now easier to test.</span></span>

## <a name="error-management"></a><span data-ttu-id="f98ca-163">Administración de errores</span><span class="sxs-lookup"><span data-stu-id="f98ca-163">Error management</span></span>

<span data-ttu-id="f98ca-164">La administración de errores en sistemas de gran tamaño es una tarea muy compleja y matizada y no hay ningún silver viñetas de asegurarse de sus sistemas son tolerantes a errores y se comportan correctamente.</span><span class="sxs-lookup"><span data-stu-id="f98ca-164">Error management in large systems is a complex and nuanced endeavor, and there are no silver bullets in ensuring your systems are fault-tolerant and behave well.</span></span> <span data-ttu-id="f98ca-165">Las siguientes directrices deberían proporcionar instrucciones para navegar por este espacio difícil.</span><span class="sxs-lookup"><span data-stu-id="f98ca-165">The following guidelines should offer guidance in navigating this difficult space.</span></span>

### <a name="represent-error-cases-and-illegal-state-in-types-intrinsic-to-your-domain"></a><span data-ttu-id="f98ca-166">Representar los casos de error y el estado no válido en tipos intrínsecos a su dominio</span><span class="sxs-lookup"><span data-stu-id="f98ca-166">Represent error cases and illegal state in types intrinsic to your domain</span></span>

<span data-ttu-id="f98ca-167">Con [uniones discriminadas](../language-reference/discriminated-unions.md), F # proporciona la capacidad para representar el estado del programa defectuoso en el sistema de tipos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-167">With [Discriminated Unions](../language-reference/discriminated-unions.md), F# gives you the ability to represent faulty program state in your type system.</span></span> <span data-ttu-id="f98ca-168">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f98ca-168">For example:</span></span>

```fsharp
type MoneyWithdrawalResult =
    | Success of amount:decimal
    | InsufficientFunds of balance:decimal
    | CardExpired of DateTime
    | UndisclosedFailure
```

<span data-ttu-id="f98ca-169">En este caso, hay tres maneras conocidos que retirar dinero de una cuenta bancaria puede producir un error.</span><span class="sxs-lookup"><span data-stu-id="f98ca-169">In this case, there are three known ways that withdrawing money from a bank account can fail.</span></span> <span data-ttu-id="f98ca-170">Cada caso de error está representado en el tipo y, por tanto, se puede tratar con seguridad en todo el sistema.</span><span class="sxs-lookup"><span data-stu-id="f98ca-170">Each error case is represented in the type, and can thus be dealt with safely throughout the program.</span></span>

```fsharp
let handleWithdrawal amount =
    let w = withdrawMoney amount
    match w with
    | Success am -> printfn "Successfully withdrew %f" am
    | InsufficientFunds balance -> printfn "Failed: balance is %f" balance
    | CardExpired expiredDate -> printfn "Failed: card expired on %O" expiredDate
    | UndisclosedFailure -> printfn "Failed: unknown"
```

<span data-ttu-id="f98ca-171">En general, si puede modelar las distintas maneras en que algo puede **producirá un error** en su dominio, a continuación, código de control de errores ya no se tratan como algo que debe tratar con además de flujo del programa normal.</span><span class="sxs-lookup"><span data-stu-id="f98ca-171">In general, if you can model the different ways that something can **fail** in your domain, then error handling code is no longer treated as something you must deal with in addition to regular program flow.</span></span> <span data-ttu-id="f98ca-172">Es simplemente una parte del flujo normal del programa y no se considera **excepcional**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-172">It is simply a part of normal program flow, and not considered **exceptional**.</span></span> <span data-ttu-id="f98ca-173">Hay dos ventajas principales para esto:</span><span class="sxs-lookup"><span data-stu-id="f98ca-173">There are two primary benefits to this:</span></span>

1. <span data-ttu-id="f98ca-174">Es fácil de mantener a medida que cambia su dominio con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-174">It is easier to maintain as your domain changes over time.</span></span>
2. <span data-ttu-id="f98ca-175">Casos de error son fáciles de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="f98ca-175">Error cases are easier to unit test.</span></span>

### <a name="use-exceptions-when-errors-cannot-be-represented-with-types"></a><span data-ttu-id="f98ca-176">Utilizar excepciones cuando no se puede representar errores con tipos</span><span class="sxs-lookup"><span data-stu-id="f98ca-176">Use exceptions when errors cannot be represented with types</span></span>

<span data-ttu-id="f98ca-177">No todos los errores se pueden representar en un dominio del problema.</span><span class="sxs-lookup"><span data-stu-id="f98ca-177">Not all errors can be represented in a problem domain.</span></span> <span data-ttu-id="f98ca-178">Estos tipos de errores están *excepcional* por naturaleza, por lo tanto, la capacidad de generar y capturar excepciones en F #.</span><span class="sxs-lookup"><span data-stu-id="f98ca-178">These kinds of faults are *exceptional* in nature, hence the ability to raise and catch exceptions in F#.</span></span>

<span data-ttu-id="f98ca-179">En primer lugar, se recomienda que lea la [directrices de diseño de excepción](../../standard/design-guidelines/exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="f98ca-179">First, it is recommended that you read the [Exception Design Guidelines](../../standard/design-guidelines/exceptions.md).</span></span> <span data-ttu-id="f98ca-180">También son aplicables a F #.</span><span class="sxs-lookup"><span data-stu-id="f98ca-180">These are also applicable to F#.</span></span>

<span data-ttu-id="f98ca-181">Deben tener en cuenta las construcciones principales disponibles en F # para los propósitos de producir excepciones en el siguiente orden de preferencia:</span><span class="sxs-lookup"><span data-stu-id="f98ca-181">The main constructs available in F# for the purposes of raising exceptions should be considered in the following order of preference:</span></span>

| <span data-ttu-id="f98ca-182">Función</span><span class="sxs-lookup"><span data-stu-id="f98ca-182">Function</span></span> | <span data-ttu-id="f98ca-183">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="f98ca-183">Syntax</span></span> | <span data-ttu-id="f98ca-184">Propósito</span><span class="sxs-lookup"><span data-stu-id="f98ca-184">Purpose</span></span> |
|----------|--------|---------|
| `nullArg` | `nullArg "argumentName"` | <span data-ttu-id="f98ca-185">Genera un `System.ArgumentNullException` con el nombre de argumento especificados.</span><span class="sxs-lookup"><span data-stu-id="f98ca-185">Raises a `System.ArgumentNullException` with the specified argument name.</span></span> |
| `invalidArg` | `invalidArg "argumentName" "message"` | <span data-ttu-id="f98ca-186">Genera un `System.ArgumentException` con un nombre de argumento especificado y un mensaje.</span><span class="sxs-lookup"><span data-stu-id="f98ca-186">Raises a `System.ArgumentException` with a specified argument name and message.</span></span> |
| `invalidOp` | `invalidOp "message"` | <span data-ttu-id="f98ca-187">Genera un `System.InvalidOperationException` con el mensaje especificado.</span><span class="sxs-lookup"><span data-stu-id="f98ca-187">Raises a `System.InvalidOperationException` with the specified message.</span></span> |
|`raise`| `raise (ExceptionType("message"))` | <span data-ttu-id="f98ca-188">Mecanismo de uso general para producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="f98ca-188">General-purpose mechanism for throwing exceptions.</span></span> |
| `failwith` | `failwith "message"` | <span data-ttu-id="f98ca-189">Genera un `System.Exception` con el mensaje especificado.</span><span class="sxs-lookup"><span data-stu-id="f98ca-189">Raises a `System.Exception` with the specified message.</span></span> |
| `failwithf` | `failwithf "format string" argForFormatString` | <span data-ttu-id="f98ca-190">Genera un `System.Exception` con un mensaje determinado por la cadena de formato y sus entradas.</span><span class="sxs-lookup"><span data-stu-id="f98ca-190">Raises a `System.Exception` with a message determined by the format string and its inputs.</span></span> |

<span data-ttu-id="f98ca-191">Use `nullArg`, `invalidArg` y `invalidOp` como mecanismo para producir `ArgumentNullException`, `ArgumentException` y `InvalidOperationException` cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="f98ca-191">Use `nullArg`, `invalidArg` and `invalidOp` as the mechanism to throw `ArgumentNullException`, `ArgumentException` and `InvalidOperationException` when appropriate.</span></span>

<span data-ttu-id="f98ca-192">El `failwith` y `failwithf` funciones deben evitar en general porque activan la base de `Exception` escriba, no una excepción específica.</span><span class="sxs-lookup"><span data-stu-id="f98ca-192">The `failwith` and `failwithf` functions should generally be avoided because they raise the base `Exception` type, not a specific exception.</span></span> <span data-ttu-id="f98ca-193">Según la [directrices de diseño de excepción](../../standard/design-guidelines/exceptions.md), que desea producir excepciones más específicas cuando sea posible.</span><span class="sxs-lookup"><span data-stu-id="f98ca-193">As per the [Exception Design Guidelines](../../standard/design-guidelines/exceptions.md), you want to raise more specific exceptions when you can.</span></span>

### <a name="using-exception-handling-syntax"></a><span data-ttu-id="f98ca-194">Con la sintaxis de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="f98ca-194">Using exception-handling syntax</span></span>

<span data-ttu-id="f98ca-195">F # admite patrones de excepción mediante la `try...with` sintaxis:</span><span class="sxs-lookup"><span data-stu-id="f98ca-195">F# supports exception patterns via the `try...with` syntax:</span></span>

```fsharp
try
    tryGetFileContents()
with
| :? System.IO.FileNotFoundException as e -> // Do something with it here
| :? System.Security.SecurityException as e -> // Do something with it here
```

<span data-ttu-id="f98ca-196">Reconciliar funcionalidad para realizar en el caso de una excepción con la coincidencia de modelos puede ser un poco difícil si desea mantener el código de limpieza.</span><span class="sxs-lookup"><span data-stu-id="f98ca-196">Reconciling functionality to perform in the face of an exception with pattern matching can be a bit tricky if you wish to keep the code clean.</span></span> <span data-ttu-id="f98ca-197">Un modo para controlar esto consiste en usar [modelos activos](../language-reference/active-patterns.md) como medio para la funcionalidad de grupo que rodea a un caso de error con una excepción propia.</span><span class="sxs-lookup"><span data-stu-id="f98ca-197">One such way to handle this is to use [active patterns](../language-reference/active-patterns.md) as a means to group functionality surrounding an error case with an exception itself.</span></span> <span data-ttu-id="f98ca-198">Por ejemplo, puede consumir una API que, cuando produce una excepción, incluye información valiosa en los metadatos de excepción.</span><span class="sxs-lookup"><span data-stu-id="f98ca-198">For example, you may be consuming an API that, when it throws an exception, encloses valuable information in the exception metadata.</span></span> <span data-ttu-id="f98ca-199">Desempaquetar un valor adecuado en el cuerpo de la excepción capturada en el modelo activo y devolver que el valor puede ser útil en algunas situaciones.</span><span class="sxs-lookup"><span data-stu-id="f98ca-199">Unwrapping a useful value in the body of the captured exception inside the Active Pattern and returning that value can be helpful in some situations.</span></span>

### <a name="do-not-use-monadic-error-handling-to-replace-exceptions"></a><span data-ttu-id="f98ca-200">No use para reemplazar las excepciones de control de errores monádico</span><span class="sxs-lookup"><span data-stu-id="f98ca-200">Do not use monadic error handling to replace exceptions</span></span>

<span data-ttu-id="f98ca-201">Las excepciones se ven como algo taboo en la programación funcional.</span><span class="sxs-lookup"><span data-stu-id="f98ca-201">Exceptions are seen as somewhat taboo in functional programming.</span></span> <span data-ttu-id="f98ca-202">De hecho, las excepciones infringen pureza, por lo que es seguro para tener en cuenta no es funcional.</span><span class="sxs-lookup"><span data-stu-id="f98ca-202">Indeed, exceptions violate purity, so it's safe to consider them not-quite functional.</span></span> <span data-ttu-id="f98ca-203">Sin embargo, esto omite la realidad de donde se debe ejecutar código y que pueden producirse errores en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f98ca-203">However, this ignores the reality of where code must run, and that runtime errors can occur.</span></span> <span data-ttu-id="f98ca-204">Por lo general, escribir código en la suposición de que la mayoría de aspectos no son puro ni total, para minimizar las sorpresas desagradables.</span><span class="sxs-lookup"><span data-stu-id="f98ca-204">In general, write code on the assumption that most things are neither pure nor total, to minimize unpleasant surprises.</span></span>

<span data-ttu-id="f98ca-205">Es importante tener en cuenta los siguientes puntos fuertes/aspectos básicos de excepciones con respecto a su relevancia y adecuación en el tiempo de ejecución de .NET y el ecosistema de idiomas como un todo:</span><span class="sxs-lookup"><span data-stu-id="f98ca-205">It is important to consider the following core strengths/aspects of Exceptions with respect to their relevance and appropriateness in the .NET runtime and cross-language ecosystem as a whole:</span></span>

1. <span data-ttu-id="f98ca-206">Contienen información de diagnóstico detallada, lo cual es muy útil al depurar un problema.</span><span class="sxs-lookup"><span data-stu-id="f98ca-206">They contain detailed diagnostic information, which is very helpful when debugging an issue.</span></span>
2. <span data-ttu-id="f98ca-207">Están bien definidas por el tiempo de ejecución y otros lenguajes. NET.</span><span class="sxs-lookup"><span data-stu-id="f98ca-207">They are well-understood by the runtime and other .NET languages.</span></span>
3. <span data-ttu-id="f98ca-208">Pueden reducir reutilizable significativo en comparación con el código sale del camino a *evitar* excepciones mediante la implementación de un subconjunto de sus semánticas de manera ad hoc.</span><span class="sxs-lookup"><span data-stu-id="f98ca-208">They can reduce significant boilerplate when compared with code that goes out of its way to *avoid* exceptions by implementing some subset of their semantics on an ad-hoc basis.</span></span>

<span data-ttu-id="f98ca-209">Este tercer punto es fundamental.</span><span class="sxs-lookup"><span data-stu-id="f98ca-209">This third point is critical.</span></span> <span data-ttu-id="f98ca-210">No triviales para operaciones más complejas, si no se puede utilizar excepciones puede producir trabajar con estructuras similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f98ca-210">For nontrivial complex operations, failing to use exceptions can result in dealing with structures like this:</span></span>

```fsharp
Result<Result<MyType, string>, string list>
```

<span data-ttu-id="f98ca-211">Fácilmente lo que puede provocar frágil código como en "stringly escrito" errores de coincidencia de patrón:</span><span class="sxs-lookup"><span data-stu-id="f98ca-211">Which can easily lead to fragile code like pattern matching on "stringly-typed" errors:</span></span>

```fsharp
let result = doStuff()
match result with
| Ok r -> ...
| Error e ->
    if e.Contains "Error string 1" then ...
    elif e.Contains "Error string 2" then ...
    else ... // Who knows?
```

<span data-ttu-id="f98ca-212">Además, puede resultar tentador pasar cualquier excepción en el deseo de una función de "simple" que devuelva un tipo "más adecuado":</span><span class="sxs-lookup"><span data-stu-id="f98ca-212">Additionally, it can be tempting to swallow any exception in the desire for a "simple" function that returns a "nicer" type:</span></span>

```fsharp
// This is bad!
let tryReadAllText (path : string) =
    try System.IO.File.ReadAllText path |> Some
    with _ -> None
```

<span data-ttu-id="f98ca-213">Por desgracia, `tryReadAllText` puede producir numerosas excepciones en función de la gran cantidad de cosas que pueden ocurrir en un sistema de archivos, y este código inmediatamente descarta toda la información sobre lo que podría realmente se causa del error en su entorno.</span><span class="sxs-lookup"><span data-stu-id="f98ca-213">Unfortunately, `tryReadAllText` can throw numerous exceptions based on the myriad of things that can happen on a file system, and this code discards away any information about what might actually be going wrong in your environment.</span></span> <span data-ttu-id="f98ca-214">Si reemplaza este código con un tipo de resultado, le volver a analizar el mensaje "stringly escrito" error:</span><span class="sxs-lookup"><span data-stu-id="f98ca-214">If you replace this code with a result type, then you're back to "stringly-typed" error message parsing:</span></span>

```fsharp
// This is bad!
let tryReadAllText (path : string) =
    try System.IO.File.ReadAllText path |> Ok
    with e -> Error e.Message

let r = tryReadAllText "path-to-file"
match r with
| Ok text -> ...
| Error e ->
    if e.Contains "uh oh, here we go again..." then ...
    else ...
```

<span data-ttu-id="f98ca-215">Y la colocación del propio objeto de excepción en el `Error` constructor simplemente obliga a tratar correctamente con el tipo de excepción en el sitio de llamada en lugar de en la función.</span><span class="sxs-lookup"><span data-stu-id="f98ca-215">And placing the exception object itself in the `Error` constructor just forces you to properly deal with the exception type at the call site rather than in the function.</span></span> <span data-ttu-id="f98ca-216">Hacerlo de forma eficaz crea excepciones comprobadas, que son notablemente unfun tratar como un llamador de una API.</span><span class="sxs-lookup"><span data-stu-id="f98ca-216">Doing this effectively creates checked exceptions, which are notoriously unfun to deal with as a caller of an API.</span></span>

<span data-ttu-id="f98ca-217">Es una buena alternativa a los ejemplos anteriores detectar *específico* excepciones y devolver un valor significativo en el contexto de esta excepción.</span><span class="sxs-lookup"><span data-stu-id="f98ca-217">A good alternative to the above examples is to catch *specific* exceptions and return a meaningful value in the context of that exception.</span></span> <span data-ttu-id="f98ca-218">Si modifica la `tryReadAllText` funcionar como sigue, `None` tiene más significado:</span><span class="sxs-lookup"><span data-stu-id="f98ca-218">If you modify the `tryReadAllText` function as follows, `None` has more meaning:</span></span>

```fsharp
let tryReadAllTextIfPresent (path : string) =
    try System.IO.File.ReadAllText path |> Some
    with :? FileNotFoundException -> None
```

<span data-ttu-id="f98ca-219">En lugar de funcionar como comodín, esta función ahora controlará correctamente el caso cuando un archivo no se encontró y asignar ese significado para un carácter de retorno.</span><span class="sxs-lookup"><span data-stu-id="f98ca-219">Instead of functioning as a catch-all, this function will now properly handle the case when a file was not found and assign that meaning to a return.</span></span> <span data-ttu-id="f98ca-220">Este valor devuelto puede asignar a ese caso de error, mientras no descarta cualquier información contextual o hacer que los llamadores para tratar un caso que podrían no ser pertinente en ese momento en el código.</span><span class="sxs-lookup"><span data-stu-id="f98ca-220">This return value can map to that error case, while not discarding any contextual information or forcing callers to deal with a case that may not be relevant at that point in the code.</span></span>

<span data-ttu-id="f98ca-221">Tipos como `Result<'Success, 'Error>` son adecuados para las operaciones básicas que no están anidados y son perfectos para representar algo podría cualquier valor devuelto opcionales tipos en F # *algo* o *nada*.</span><span class="sxs-lookup"><span data-stu-id="f98ca-221">Types such as `Result<'Success, 'Error>` are appropriate for basic operations where they aren't nested, and F# optional types are perfect for representing when something could either return *something* or *nothing*.</span></span> <span data-ttu-id="f98ca-222">Sin embargo, no son un sustituto para las excepciones y no debe usarse en un intento de reemplazar las excepciones.</span><span class="sxs-lookup"><span data-stu-id="f98ca-222">They are not a replacement for exceptions, though, and should not be used in an attempt to replace exceptions.</span></span> <span data-ttu-id="f98ca-223">En su lugar, se debe aplicar con prudencia a los aspectos específicos de dirección de la excepción y la directiva de administración de errores de maneras de destino.</span><span class="sxs-lookup"><span data-stu-id="f98ca-223">Rather, they should be applied judiciously to address specific aspects of exception and error management policy in targeted ways.</span></span>

## <a name="partial-application-and-point-free-programming"></a><span data-ttu-id="f98ca-224">Aplicación parcial y libre de punto de programación</span><span class="sxs-lookup"><span data-stu-id="f98ca-224">Partial application and point-free programming</span></span>

<span data-ttu-id="f98ca-225">F # admite la aplicación parcial y por lo tanto, varias maneras de programa en un estilo libre de punto.</span><span class="sxs-lookup"><span data-stu-id="f98ca-225">F# supports partial application, and thus, various ways to program in a point-free style.</span></span> <span data-ttu-id="f98ca-226">Esto puede ser beneficioso para reutilizar el código dentro de un módulo o la implementación de algo, pero en general no es algo para exponer públicamente.</span><span class="sxs-lookup"><span data-stu-id="f98ca-226">This can be beneficial for code reuse within a module or the implementation of something, but it is generally not something to expose publicly.</span></span> <span data-ttu-id="f98ca-227">En general, libre de punto de programación no es un virtud de por sí y puede agregar una barrera cognitiva significativa para las personas que no se sumergen en el estilo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-227">In general, point-free programming is not a virtue in and of itself, and can add a significant cognitive barrier for people who are not immersed in the style.</span></span>

### <a name="do-not-use-partial-application-and-currying-in-public-apis"></a><span data-ttu-id="f98ca-228">No use aplicación parcial y la currificación en las API públicas</span><span class="sxs-lookup"><span data-stu-id="f98ca-228">Do not use partial application and currying in public APIs</span></span>

<span data-ttu-id="f98ca-229">Con pocas excepciones, el uso de la aplicación parcial en las API públicas puede resultar confuso para los consumidores.</span><span class="sxs-lookup"><span data-stu-id="f98ca-229">With little exception, the use of partial application in public APIs can be confusing for consumers.</span></span> <span data-ttu-id="f98ca-230">Por lo general, `let`-son valores enlazados en el código de F # **valores**, no **función valores**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-230">Usually, `let`-bound values in F# code are **values**, not **function values**.</span></span> <span data-ttu-id="f98ca-231">Combinación de los valores y los valores de función puede dar lugar a para guardar un pequeño número de líneas de código a cambio de una gran cantidad de sobrecarga cognitiva, especialmente si se combina con operadores como `>>` para la creación de funciones.</span><span class="sxs-lookup"><span data-stu-id="f98ca-231">Mixing together values and function values can result in saving a small number of lines of code in exchange for quite a bit of cognitive overhead, especially if combined with operators such as `>>` to compose functions.</span></span>

### <a name="consider-the-tooling-implications-for-point-free-programming"></a><span data-ttu-id="f98ca-232">Tenga en cuenta las implicaciones de herramientas para la programación libre de punto</span><span class="sxs-lookup"><span data-stu-id="f98ca-232">Consider the tooling implications for point-free programming</span></span>

<span data-ttu-id="f98ca-233">Funciones currificadas no etiqueta sus argumentos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-233">Curried functions do not label their arguments.</span></span> <span data-ttu-id="f98ca-234">Esto tiene implicaciones de herramientas.</span><span class="sxs-lookup"><span data-stu-id="f98ca-234">This has tooling implications.</span></span> <span data-ttu-id="f98ca-235">Tenga en cuenta las dos funciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="f98ca-235">Consider the following two functions:</span></span>

```fsharp
let func name age =
    printfn "My name is %s and I am %d years old!" name age

let funcWithApplication =
    printfn "My name is %s and I am %d years old!"
```

<span data-ttu-id="f98ca-236">Ambas son funciones válidas, pero `funcWithApplication` es una función currificada.</span><span class="sxs-lookup"><span data-stu-id="f98ca-236">Both are valid functions, but `funcWithApplication` is a curried function.</span></span> <span data-ttu-id="f98ca-237">Cuando sitúa sobre sus tipos en un editor, verá lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f98ca-237">When you hover over their types in an editor, you see this:</span></span>

```fsharp
val func : name:string -> age:int -> unit

val funcWithApplication : (string -> int -> unit)
```

<span data-ttu-id="f98ca-238">En el sitio de llamada, información sobre herramientas en herramientas como Visual Studio, no tendrá información significativa para saber lo que el `string` y `int` realmente representan tipos de entrada.</span><span class="sxs-lookup"><span data-stu-id="f98ca-238">At the call site, tooltips in tooling such as Visual Studio will not give you meaningful information as to what the `string` and `int` input types actually represent.</span></span>

<span data-ttu-id="f98ca-239">Si se producen libre de punto de código como `funcWithApplication` es públicamente consumible, se recomienda realizar una expansión de η completa para que las herramientas pueden elegir en hasta nombres significativos para los argumentos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-239">If you encounter point-free code like `funcWithApplication` that is publicly consumable, it is recommended to do a full η-expansion so that tooling can pick up on meaningful names for arguments.</span></span>

<span data-ttu-id="f98ca-240">Además, libre de punto de código de depuración puede ser complicada, si no imposible.</span><span class="sxs-lookup"><span data-stu-id="f98ca-240">Furthermore, debugging point-free code can be challenging, if not impossible.</span></span> <span data-ttu-id="f98ca-241">Herramientas de depuración se basan en valores enlazados a los nombres (por ejemplo, `let` enlaces) que puede inspeccionar la mitad de los valores intermedios a través de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="f98ca-241">Debugging tools rely on values bound to names (for example, `let` bindings) so that you can inspect intermediate values midway through execution.</span></span> <span data-ttu-id="f98ca-242">Cuando el código no tiene ningún valor para inspeccionar, no hay nada para depurar.</span><span class="sxs-lookup"><span data-stu-id="f98ca-242">When your code has no values to inspect, there is nothing to debug.</span></span> <span data-ttu-id="f98ca-243">En el futuro, herramientas de depuración pueden evolucionar para sintetizar estos valores en función de las rutas de acceso ejecutadas con anterioridad, pero no es una buena idea para protegerse de los resultados probables en *posibles* funciones de depuración.</span><span class="sxs-lookup"><span data-stu-id="f98ca-243">In the future, debugging tools may evolve to synthesize these values based on previously executed paths, but it's not a good idea to hedge your bets on *potential* debugging functionality.</span></span>

### <a name="consider-partial-application-as-a-technique-to-reduce-internal-boilerplate"></a><span data-ttu-id="f98ca-244">Considere la posibilidad de aplicación parcial como una técnica para reducir reutilizable interno</span><span class="sxs-lookup"><span data-stu-id="f98ca-244">Consider partial application as a technique to reduce internal boilerplate</span></span>

<span data-ttu-id="f98ca-245">A diferencia de lo anterior, aplicación parcial es una herramienta maravillosa para reducir reutilizable dentro de una aplicación o el funcionamiento interno de un poco más de una API.</span><span class="sxs-lookup"><span data-stu-id="f98ca-245">In contrast to the previous point, partial application is a wonderful tool for reducing boilerplate inside of an application or the deeper internals of an API.</span></span> <span data-ttu-id="f98ca-246">Puede resultar útil para la implementación de las API más complicadas, donde reutilizable a menudo es una molestia para tratar de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="f98ca-246">It can be helpful for unit testing the implementation of more complicated APIs, where boilerplate is often a pain to deal with.</span></span> <span data-ttu-id="f98ca-247">Por ejemplo, el código siguiente muestra cómo puede llevar a cabo los marcos de simulación más ofrecen sin realizar una dependencia externa en un marco de trabajo y tener que aprender un relacionados personalizada API.</span><span class="sxs-lookup"><span data-stu-id="f98ca-247">For example, the following code shows how you can accomplish what most mocking frameworks give you without taking an external dependency on such a framework and having to learn a related bespoke API.</span></span>

<span data-ttu-id="f98ca-248">Por ejemplo, considere la topografía de solución siguientes:</span><span class="sxs-lookup"><span data-stu-id="f98ca-248">For example, consider the following solution topography:</span></span>

```
MySolution.sln
|_/ImplementationLogic.fsproj
|_/ImplementationLogic.Tests.fsproj
|_/API.fsproj
```

<span data-ttu-id="f98ca-249">`ImplementationLogic.fsproj` puede exponer el código, como:</span><span class="sxs-lookup"><span data-stu-id="f98ca-249">`ImplementationLogic.fsproj` might expose code such as:</span></span>

```fsharp
module Transactions =
    let doTransaction txnContext txnType balance =
        ...

type Transactor(ctx, currentBalance) =
    member __.ExecuteTransaction(txnType) =
        Transactions.doTransaction ctx txtType currentBalance
        ...
```

<span data-ttu-id="f98ca-250">Pruebas unitarias `Transactions.doTransaction` en `ImplementationLogic.Tests.fspoj` es fácil:</span><span class="sxs-lookup"><span data-stu-id="f98ca-250">Unit testing `Transactions.doTransaction` in `ImplementationLogic.Tests.fspoj` is easy:</span></span>

```fsharp
namespace TransactionsTestingUtil

open Transactions

module TransactionsTestable =
    let getTestableTransactionRoutine mockContext = Transactions.doTransaction mockContext
```

<span data-ttu-id="f98ca-251">Aplicar parcialmente `doTransaction` con un contexto de simulación objeto le permite llamar a la función en todas las pruebas unitarias sin necesidad de crear un contexto simulado cada vez:</span><span class="sxs-lookup"><span data-stu-id="f98ca-251">Partially applying `doTransaction` with a mocking context object lets you call the function in all of your unit tests without needing to construct a mocked context each time:</span></span>

```fsharp
namespace TransactionTests

open Xunit
open TransactionTypes
open TransactionsTestingUtil
open TransactionsTestingUtil.TransactionsTestable

let testableContext =
    { new ITransactionContext with
        member __.TheFirstMember() = ...
        member __.TheSecondMember() = ... }

let transactionRoutine = getTestableTransactionRoutine testableContext

[<Fact>]
let ``Test withdrawal transaction with 0.0 for balance``() =
    let expected = ...
    let actual = transactionRoutine TransactionType.Withdraw 0.0
    Assert.Equal(expected, actual)
```

<span data-ttu-id="f98ca-252">Esta técnica no se debe aplicar universalmente a todo el código de base, pero es una buena manera de reducir reutilizable para complejas funciones internas y los elementos internos de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="f98ca-252">This technique should not be universally applied to your entire codebase, but it is a good way to reduce boilerplate for complicated internals and unit testing those internals.</span></span>

## <a name="access-control"></a><span data-ttu-id="f98ca-253">Control de acceso</span><span class="sxs-lookup"><span data-stu-id="f98ca-253">Access control</span></span>

<span data-ttu-id="f98ca-254">F # tiene varias opciones para [el control de acceso](../language-reference/access-control.md)heredar del lo que está disponible en el tiempo de ejecución. NET.</span><span class="sxs-lookup"><span data-stu-id="f98ca-254">F# has multiple options for [Access control](../language-reference/access-control.md), inherited from what is available in the .NET runtime.</span></span> <span data-ttu-id="f98ca-255">Estos no son simplemente puede usar para los tipos: puede usarlos para funciones, demasiado.</span><span class="sxs-lookup"><span data-stu-id="f98ca-255">These are not just usable for types - you can use them for functions, too.</span></span>

* <span data-ttu-id="f98ca-256">Prefiere no -`public` tipos y miembros hasta que se necesiten para poder usarse públicamente.</span><span class="sxs-lookup"><span data-stu-id="f98ca-256">Prefer non-`public` types and members until you need them to be publicly consumable.</span></span> <span data-ttu-id="f98ca-257">Esto también reduce el par de los consumidores a</span><span class="sxs-lookup"><span data-stu-id="f98ca-257">This also minimizes what consumers couple to</span></span>
* <span data-ttu-id="f98ca-258">Intentar mantener toda la funcionalidad de aplicación auxiliar `private`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-258">Strive to keep all helper functionality `private`.</span></span>
* <span data-ttu-id="f98ca-259">Considere el uso de `[<AutoOpen>]` en un módulo de funciones de aplicación auxiliar que dejen de estar numerosas privado.</span><span class="sxs-lookup"><span data-stu-id="f98ca-259">Consider the use of `[<AutoOpen>]` on a private module of helper functions if they become numerous.</span></span>

## <a name="type-inference-and-generics"></a><span data-ttu-id="f98ca-260">Inferencia de tipos y tipos genéricos</span><span class="sxs-lookup"><span data-stu-id="f98ca-260">Type inference and generics</span></span>

<span data-ttu-id="f98ca-261">Inferencia de tipos puede evitarle escribir mucho código reutilizable.</span><span class="sxs-lookup"><span data-stu-id="f98ca-261">Type inference can save you from typing a lot of boilerplate.</span></span> <span data-ttu-id="f98ca-262">Y generalización automática en el compilador de F # puede ayudarle a escribir código más genérico con casi ningún esfuerzo adicional por su parte.</span><span class="sxs-lookup"><span data-stu-id="f98ca-262">And automatic generalization in the F# compiler can help you write more generic code with almost no extra effort on your part.</span></span> <span data-ttu-id="f98ca-263">Sin embargo, estas características no están universalmente buenas.</span><span class="sxs-lookup"><span data-stu-id="f98ca-263">However, these features are not universally good.</span></span>

* <span data-ttu-id="f98ca-264">Considere la posibilidad de etiquetar los nombres de argumento con un tipo explícito en las API públicas y no se debe confiar en la inferencia de tipo para este.</span><span class="sxs-lookup"><span data-stu-id="f98ca-264">Consider labeling argument names with explicit types in public APIs and do not rely on type inference for this.</span></span>

    <span data-ttu-id="f98ca-265">La razón de ello es que **debe tener** el control de la forma de la API, no el compilador.</span><span class="sxs-lookup"><span data-stu-id="f98ca-265">The reason for this is that **you** should be in control of the shape of your API, not the compiler.</span></span> <span data-ttu-id="f98ca-266">Aunque el compilador puede realizar un trabajo bien en inferir tipos para usted, es posible tener la forma de cambiar su API si el funcionamiento interno en que se basa ha cambiado los tipos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-266">Although the compiler can do a fine job at inferring types for you, it is possible to have the shape of your API change if the internals it relies on have changed types.</span></span> <span data-ttu-id="f98ca-267">Puede tratarse de lo que desee, pero casi con toda seguridad supondrá un cambio de la API de importante que los consumidores de nivel inferiores, a continuación, tendrá que tratar con.</span><span class="sxs-lookup"><span data-stu-id="f98ca-267">This may be what you want, but it will almost certainly result in a breaking API change that downstream consumers will then have to deal with.</span></span> <span data-ttu-id="f98ca-268">En su lugar, si puede controlar explícitamente la forma de la API pública, puede controlar estos cambios.</span><span class="sxs-lookup"><span data-stu-id="f98ca-268">Instead, if you explicitly control the shape of your public API, then you can control these breaking changes.</span></span> <span data-ttu-id="f98ca-269">En términos DDD, esto puede considerarse como una capa contra daño.</span><span class="sxs-lookup"><span data-stu-id="f98ca-269">In DDD terms, this can be thought of as an Anti-corruption layer.</span></span>

* <span data-ttu-id="f98ca-270">Considere la posibilidad de dar un nombre descriptivo para los argumentos genéricos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-270">Consider giving a meaningful name to your generic arguments.</span></span>

    <span data-ttu-id="f98ca-271">A menos que se está escribiendo código realmente genérico que no es específico de un dominio en particular, un nombre descriptivo puede ayudar a otros programadores comprender el dominio que está trabajando en.</span><span class="sxs-lookup"><span data-stu-id="f98ca-271">Unless you are writing truly generic code that is not specific to a particular domain, a meaningful name can help other programmers understanding the domain they're working in.</span></span> <span data-ttu-id="f98ca-272">Por ejemplo, un parámetro de tipo denominado `'Document` en el contexto de la interacción con un documento de base de datos resulta más claro que se pueden aceptar tipos de documento genérico por la función o el miembro que está trabajando.</span><span class="sxs-lookup"><span data-stu-id="f98ca-272">For example, a type parameter named `'Document` in the context of interacting with a document database makes it clearer that generic document types can be accepted by the function or member you are working with.</span></span>

* <span data-ttu-id="f98ca-273">Considere la posibilidad de nomenclatura de parámetros de tipo genérico con PascalCase.</span><span class="sxs-lookup"><span data-stu-id="f98ca-273">Consider naming generic type parameters with PascalCase.</span></span>

    <span data-ttu-id="f98ca-274">Se trata de la forma general para realizar acciones en. NET, por lo que se recomienda que use PascalCase en lugar de snake_case o camelCase.</span><span class="sxs-lookup"><span data-stu-id="f98ca-274">This is the general way to do things in .NET, so it's recommended that you use PascalCase rather than snake_case or camelCase.</span></span>

<span data-ttu-id="f98ca-275">Por último, la generalización automática no siempre es una ventaja para las personas que son nuevas en F # o un código base grande.</span><span class="sxs-lookup"><span data-stu-id="f98ca-275">Finally, automatic generalization is not always a boon for people who are new to F# or a large codebase.</span></span> <span data-ttu-id="f98ca-276">Hay sobrecarga cognitiva en utilización de componentes que son genéricos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-276">There is cognitive overhead in using components that are generic.</span></span> <span data-ttu-id="f98ca-277">Además, si automáticamente generalizadas funciones no se utilizan con diferentes tipos de entrada (si están destinadas a usarse como tales let), entonces representa ningún beneficio real a ellos ser genérico en ese momento en el tiempo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-277">Furthermore, if automatically generalized functions are not used with different input types (let alone if they are intended to be used as such), then there is no real benefit to them being generic at that point in time.</span></span> <span data-ttu-id="f98ca-278">Siempre tenga en cuenta si el código que se escribe realmente se beneficiará de ser genérico.</span><span class="sxs-lookup"><span data-stu-id="f98ca-278">Always consider if the code you are writing will actually benefit from being generic.</span></span>

## <a name="performance"></a><span data-ttu-id="f98ca-279">Rendimiento</span><span class="sxs-lookup"><span data-stu-id="f98ca-279">Performance</span></span>

<span data-ttu-id="f98ca-280">Valores de F # son inmutables de forma predeterminada, lo que permite evitar ciertas clases de errores (especialmente los relacionados con simultaneidad y el paralelismo).</span><span class="sxs-lookup"><span data-stu-id="f98ca-280">F# values are immutable by default, which allows you to avoid certain classes of bugs (especially those involving concurrency and parallelism).</span></span> <span data-ttu-id="f98ca-281">Sin embargo, en algunos casos, para lograr una eficiencia óptima (o incluso razonable) de tiempo de ejecución o las asignaciones de memoria, un intervalo de trabajo puede mejor implementarse mediante el uso de mutación en lugar de estado.</span><span class="sxs-lookup"><span data-stu-id="f98ca-281">However, in certain cases, in order to achieve optimal (or even reasonable) efficiency of execution time or memory allocations, a span of work may best be implemented by using in-place mutation of state.</span></span> <span data-ttu-id="f98ca-282">Esto es posible en una base de participación con F # con el `mutable` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="f98ca-282">This is possible in an opt-in basis with F# with the `mutable` keyword.</span></span>

<span data-ttu-id="f98ca-283">Sin embargo, uso de `mutable` en F # pueden considerar no concuerda con pureza funcional.</span><span class="sxs-lookup"><span data-stu-id="f98ca-283">However, use of `mutable` in F# may feel at odds with functional purity.</span></span> <span data-ttu-id="f98ca-284">Esto es correcto, si está ajustando las expectativas de pureza a [transparencia referencial](https://en.wikipedia.org/wiki/Referential_transparency).</span><span class="sxs-lookup"><span data-stu-id="f98ca-284">This is fine, if you adjust expectations from purity to [referential transparency](https://en.wikipedia.org/wiki/Referential_transparency).</span></span> <span data-ttu-id="f98ca-285">La transparencia referencial - no pureza - es el objetivo final al escribir funciones de F #.</span><span class="sxs-lookup"><span data-stu-id="f98ca-285">Referential transparency - not purity - is the end goal when writing F# functions.</span></span> <span data-ttu-id="f98ca-286">Esto le permite escribir una interfaz funcional a través de una implementación basada en mutación para código crítico en rendimiento.</span><span class="sxs-lookup"><span data-stu-id="f98ca-286">This allows you to write a functional interface over a mutation-based implementation for performance critical code.</span></span>

### <a name="wrap-mutable-code-in-immutable-interfaces"></a><span data-ttu-id="f98ca-287">Incluir código mutable en interfaces inmutable</span><span class="sxs-lookup"><span data-stu-id="f98ca-287">Wrap mutable code in immutable interfaces</span></span>

<span data-ttu-id="f98ca-288">Con transparencia referencial como un objetivo, es fundamental para escribir código que no expone el underbelly mutable de las funciones esenciales para el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="f98ca-288">With referential transparency as a goal, it is critical to write code that does not expose the mutable underbelly of performance-critical functions.</span></span> <span data-ttu-id="f98ca-289">Por ejemplo, el código siguiente implementa la `Array.contains` función en la biblioteca básica de F #:</span><span class="sxs-lookup"><span data-stu-id="f98ca-289">For example, the following code implements the `Array.contains` function in the F# core library:</span></span>

```fsharp
[<CompiledName("Contains")>]
let inline contains value (array:'T[]) =
    checkNonNull "array" array
    let mutable state = false
    let mutable i = 0
    while not state && i < array.Length do
        state <- value = array.[i]
        i <- i + 1
    state
```

<span data-ttu-id="f98ca-290">Llamar varias veces a esta función no modifica la matriz subyacente, ni es necesario mantener cualquier estado mutable en usarlo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-290">Calling this function multiple times does not change the underlying array, nor does it require you to maintain any mutable state in consuming it.</span></span> <span data-ttu-id="f98ca-291">Es forma referencial transparente, incluso aunque casi todas las líneas de código dentro de él emplea mutación.</span><span class="sxs-lookup"><span data-stu-id="f98ca-291">It is referentially transparent, even though almost every line of code within it uses mutation.</span></span>

### <a name="consider-encapsulating-mutable-data-in-classes"></a><span data-ttu-id="f98ca-292">Considere la posibilidad de encapsulación de datos mutables en clases</span><span class="sxs-lookup"><span data-stu-id="f98ca-292">Consider encapsulating mutable data in classes</span></span>

<span data-ttu-id="f98ca-293">En el ejemplo anterior se utiliza una única función para encapsular las operaciones con datos mutables.</span><span class="sxs-lookup"><span data-stu-id="f98ca-293">The previous example used a single function to encapsulate operations using mutable data.</span></span> <span data-ttu-id="f98ca-294">No siempre es suficiente para conjuntos más complejos de datos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-294">This is not always sufficient for more complex sets of data.</span></span> <span data-ttu-id="f98ca-295">Tenga en cuenta los siguientes conjuntos de funciones:</span><span class="sxs-lookup"><span data-stu-id="f98ca-295">Consider the following sets of functions:</span></span>

```fsharp
open System.Collections.Generic

let addToClosureTable (key, value) (t: Dictionary<_,_>) =
    if not (t.ContainsKey(key)) then
        t.Add(key, value)
    else
        t.[key] <- value

let closureTableCount (t: Dictionary<_,_>) = t.Count

let closureTableContains (key, value) (t: Dictionary<_, HashSet<_>>) =
    match t.TryGetValue(key) with
    | (true, v) -> v.Equals(value)
    | (false, _) -> false
```

<span data-ttu-id="f98ca-296">Este código es un rendimiento superior, pero expone la estructura de datos basados en mutaciones que los llamadores son responsables de mantener.</span><span class="sxs-lookup"><span data-stu-id="f98ca-296">This code is performant, but it exposes the mutation-based data structure that callers are responsible for maintaining.</span></span> <span data-ttu-id="f98ca-297">Esto puede colocarse dentro de una clase sin miembros subyacente que puede cambiar:</span><span class="sxs-lookup"><span data-stu-id="f98ca-297">This can be wrapped inside of a class with no underlying members that can change:</span></span>

```fsharp
open System.Collections.Generic

/// The results of computing the LALR(1) closure of an LR(0) kernel
type Closure1Table() =
    let t = Dictionary<Item0, HashSet<TerminalIndex>>()

    member __.Add(key, value) =
        if not (t.ContainsKey(key)) then
            t.Add(key, value)
        else
            t.[key] <- value

    member __.Count = t.Count

    member __.Contains(key, value) =
        match t.TryGetValue(key) with
        | (true, v) -> v.Equals(value)
        | (false, _) -> false
```

<span data-ttu-id="f98ca-298">`Closure1Table` Encapsula la estructura de datos basados en mutación subyacente, por lo que no forzar los llamadores para mantener la estructura de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="f98ca-298">`Closure1Table` encapsulates the underlying mutation-based data structure, thereby not forcing callers to maintain the underlying data structure.</span></span> <span data-ttu-id="f98ca-299">Las clases son una manera eficaz para encapsular datos y las rutinas que están basados en mutación sin exponer los detalles a los llamadores.</span><span class="sxs-lookup"><span data-stu-id="f98ca-299">Classes are a powerful way to encapsulate data and routines that are mutation-based without exposing the details to callers.</span></span>

### <a name="prefer-let-mutable-to-reference-cells"></a><span data-ttu-id="f98ca-300">Prefiere `let mutable` a las celdas de referencia</span><span class="sxs-lookup"><span data-stu-id="f98ca-300">Prefer `let mutable` to reference cells</span></span>

<span data-ttu-id="f98ca-301">Celdas de referencia son una manera de representar la referencia a un valor en lugar de como el propio valor.</span><span class="sxs-lookup"><span data-stu-id="f98ca-301">Reference cells are a way to represent the reference to a value rather than the value itself.</span></span> <span data-ttu-id="f98ca-302">Aunque se puede utilizar para el código crítico para el rendimiento, que generalmente no se recomiendan.</span><span class="sxs-lookup"><span data-stu-id="f98ca-302">Although they can be used for performance-critical code, they are generally not recommended.</span></span> <span data-ttu-id="f98ca-303">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f98ca-303">Consider the following example:</span></span>

```fsharp
let kernels =
    let acc = ref Set.empty

    processWorkList startKernels (fun kernel ->
        if not ((!acc).Contains(kernel)) then
            acc := (!acc).Add(kernel)
        ...)

    !acc |> Seq.toList
```

<span data-ttu-id="f98ca-304">El uso de una celda de referencia "contamina" todo el código subsiguiente con necesidad de desreferenciación y volver a hacer referencia a los datos subyacentes.</span><span class="sxs-lookup"><span data-stu-id="f98ca-304">The use of a reference cell now "pollutes" all subsequent code with having to dereference and re-reference the underlying data.</span></span> <span data-ttu-id="f98ca-305">En su lugar, considere la posibilidad de `let mutable`:</span><span class="sxs-lookup"><span data-stu-id="f98ca-305">Instead, consider `let mutable`:</span></span>

```fsharp
let kernels =
    let mutable acc = Set.empty

    processWorkList startKernels (fun kernel ->
        if not (acc.Contains(kernel)) then
            acc <- acc.Add(kernel)
        ...)

    acc |> Seq.toList
```

<span data-ttu-id="f98ca-306">Además el punto único de mutación en medio de la expresión lambda, todos los demás código que toca `acc` puede hacerlo de manera que es similar al uso de una normal `let`-valor inmutable del enlace.</span><span class="sxs-lookup"><span data-stu-id="f98ca-306">Aside from the single point of mutation in the middle of the lambda expression, all other code that touches `acc` can do so in a manner that is no different to the usage of a normal `let`-bound immutable value.</span></span> <span data-ttu-id="f98ca-307">Así resultará más fácil cambiar con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-307">This will make it easier to change over time.</span></span>

## <a name="object-programming"></a><span data-ttu-id="f98ca-308">Programación de objetos</span><span class="sxs-lookup"><span data-stu-id="f98ca-308">Object programming</span></span>

<span data-ttu-id="f98ca-309">F # es totalmente compatible con objetos y conceptos de (orientado a objetos) orientada a objetos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-309">F# has full support for objects and object-oriented (OO) concepts.</span></span> <span data-ttu-id="f98ca-310">Aunque muchos de los conceptos de orientado a objetos son eficaces y útil, no todas ellas son idóneos para su uso.</span><span class="sxs-lookup"><span data-stu-id="f98ca-310">Although many OO concepts are powerful and useful, not all of them are ideal to use.</span></span> <span data-ttu-id="f98ca-311">Las listas siguientes proporcionan directrices sobre categorías de características de orientado a objetos en un nivel superior.</span><span class="sxs-lookup"><span data-stu-id="f98ca-311">The following lists offer guidance on categories of OO features at a high level.</span></span>

<span data-ttu-id="f98ca-312">**Considere la posibilidad de utilizar estas características en muchas situaciones:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-312">**Consider using these features in many situations:**</span></span>

* <span data-ttu-id="f98ca-313">La notación de puntos (`x.Length`)</span><span class="sxs-lookup"><span data-stu-id="f98ca-313">Dot notation (`x.Length`)</span></span>
* <span data-ttu-id="f98ca-314">Miembros de instancia</span><span class="sxs-lookup"><span data-stu-id="f98ca-314">Instance members</span></span>
* <span data-ttu-id="f98ca-315">Constructores implícitos</span><span class="sxs-lookup"><span data-stu-id="f98ca-315">Implicit constructors</span></span>
* <span data-ttu-id="f98ca-316">Miembros estáticos</span><span class="sxs-lookup"><span data-stu-id="f98ca-316">Static members</span></span>
* <span data-ttu-id="f98ca-317">Notación de indizador (`arr.[x]`)</span><span class="sxs-lookup"><span data-stu-id="f98ca-317">Indexer notation (`arr.[x]`)</span></span>
* <span data-ttu-id="f98ca-318">Argumentos opcionales y con nombre</span><span class="sxs-lookup"><span data-stu-id="f98ca-318">Named and Optional arguments</span></span>
* <span data-ttu-id="f98ca-319">Interfaces y las implementaciones de interfaz</span><span class="sxs-lookup"><span data-stu-id="f98ca-319">Interfaces and interface implementations</span></span>

<span data-ttu-id="f98ca-320">**No alcanzar para estas características en primer lugar, pero con prudencia aplicarlos cuando están cómodos solucionar un problema:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-320">**Don't reach for these features first, but do judiciously apply them when they are convenient to solve a problem:**</span></span>

* <span data-ttu-id="f98ca-321">Sobrecarga de métodos</span><span class="sxs-lookup"><span data-stu-id="f98ca-321">Method overloading</span></span>
* <span data-ttu-id="f98ca-322">Datos mutables encapsulados</span><span class="sxs-lookup"><span data-stu-id="f98ca-322">Encapsulated mutable data</span></span>
* <span data-ttu-id="f98ca-323">Operadores en tipos</span><span class="sxs-lookup"><span data-stu-id="f98ca-323">Operators on types</span></span>
* <span data-ttu-id="f98ca-324">Propiedades de automático</span><span class="sxs-lookup"><span data-stu-id="f98ca-324">Auto properties</span></span>
* <span data-ttu-id="f98ca-325">Implementar `IDisposable` y `IEnumerable`</span><span class="sxs-lookup"><span data-stu-id="f98ca-325">Implementing `IDisposable` and `IEnumerable`</span></span>
* <span data-ttu-id="f98ca-326">Extensiones de tipo</span><span class="sxs-lookup"><span data-stu-id="f98ca-326">Type extensions</span></span>
* <span data-ttu-id="f98ca-327">Eventos</span><span class="sxs-lookup"><span data-stu-id="f98ca-327">Events</span></span>
* <span data-ttu-id="f98ca-328">Estructuras</span><span class="sxs-lookup"><span data-stu-id="f98ca-328">Structs</span></span>
* <span data-ttu-id="f98ca-329">Delegados</span><span class="sxs-lookup"><span data-stu-id="f98ca-329">Delegates</span></span>
* <span data-ttu-id="f98ca-330">Enumeraciones</span><span class="sxs-lookup"><span data-stu-id="f98ca-330">Enums</span></span>

<span data-ttu-id="f98ca-331">**Por lo general evitar estas características a menos que se debe utilizar:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-331">**Generally avoid these features unless you must use them:**</span></span>

* <span data-ttu-id="f98ca-332">Las jerarquías de tipos basado en la herencia y la herencia de implementación</span><span class="sxs-lookup"><span data-stu-id="f98ca-332">Inheritance-based type hierarchies and implementation inheritance</span></span>
* <span data-ttu-id="f98ca-333">Valores NULL y `Unchecked.defaultof<_>`</span><span class="sxs-lookup"><span data-stu-id="f98ca-333">Nulls and `Unchecked.defaultof<_>`</span></span>

### <a name="prefer-composition-over-inheritance"></a><span data-ttu-id="f98ca-334">Preferir composición de herencia</span><span class="sxs-lookup"><span data-stu-id="f98ca-334">Prefer composition over inheritance</span></span>

<span data-ttu-id="f98ca-335">[Composición con herencia](https://en.wikipedia.org/wiki/Composition_over_inheritance) es un muy larga que puede cumplir buen código F #.</span><span class="sxs-lookup"><span data-stu-id="f98ca-335">[Composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance) is a long-standing idiom that good F# code can adhere to.</span></span> <span data-ttu-id="f98ca-336">El principio fundamental es que no debería exponer una clase base y forzar que los llamadores pueden heredar de esa clase base para obtener funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="f98ca-336">The fundamental principle is that you should not expose a base class and force callers to inherit from that base class to get functionality.</span></span>

### <a name="use-object-expressions-to-implement-interfaces-if-you-dont-need-a-class"></a><span data-ttu-id="f98ca-337">Usar expresiones de objeto para implementar interfaces si ya no necesita una clase</span><span class="sxs-lookup"><span data-stu-id="f98ca-337">Use object expressions to implement interfaces if you don't need a class</span></span>

<span data-ttu-id="f98ca-338">[Expresiones de objeto](../language-reference/object-expressions.md) te permiten implementar interfaces sobre la marcha, enlazar la interfaz implementada a un valor sin necesidad de hacerlo dentro de una clase.</span><span class="sxs-lookup"><span data-stu-id="f98ca-338">[Object Expressions](../language-reference/object-expressions.md) allow you to implement interfaces on the fly, binding the implemented interface to a value without needing to do so inside of a class.</span></span> <span data-ttu-id="f98ca-339">Esto resulta útil, sobre todo si se _sólo_ tiene que implementar la interfaz y no es necesario para una clase completa.</span><span class="sxs-lookup"><span data-stu-id="f98ca-339">This is convenient, especially if you _only_ need to implement the interface and have no need for a full class.</span></span>

<span data-ttu-id="f98ca-340">Por ejemplo, este es el código que se ejecuta en [Ionide](http://ionide.io/) para proporcionar una acción de corrección del código si ha agregado un símbolo que no tiene un `open` instrucción para:</span><span class="sxs-lookup"><span data-stu-id="f98ca-340">For example, here is the code that is run in [Ionide](http://ionide.io/) to provide a code fix action if you've added a symbol that you don't have an `open` statement for:</span></span>

```fsharp
    let private createProvider () =
        { new CodeActionProvider with
            member this.provideCodeActions(doc, range, context, ct) =
                let diagnostics = context.diagnostics
                let diagnostic = diagnostics |> Seq.tryFind (fun d -> d.message.Contains "Unused open statement")
                let res =
                    match diagnostic with
                    | None -> [||]
                    | Some d ->
                        let line = doc.lineAt d.range.start.line
                        let cmd = createEmpty<Command>
                        cmd.title <- "Remove unused open"
                        cmd.command <- "fsharp.unusedOpenFix"
                        cmd.arguments <- Some ([| doc |> unbox; line.range |> unbox; |] |> ResizeArray)
                        [|cmd |]
                res
                |> ResizeArray
                |> U2.Case1
        }
```

<span data-ttu-id="f98ca-341">Dado que no es necesario para una clase al interactuar con la API de código de Visual Studio, expresiones de objeto son una herramienta ideal para este.</span><span class="sxs-lookup"><span data-stu-id="f98ca-341">Because there is no need for a class when interacting with the Visual Studio Code API, Object Expressions are an ideal tool for this.</span></span> <span data-ttu-id="f98ca-342">También son valiosos para pruebas unitarias, cuando desea extraer una interfaz con rutinas de prueba el código auxiliar de manera ad hoc.</span><span class="sxs-lookup"><span data-stu-id="f98ca-342">They are also valuable for unit testing, when you want to stub out an interface with test routines in an ad hoc manner.</span></span>

## <a name="type-abbreviations"></a><span data-ttu-id="f98ca-343">Abreviaturas de tipo</span><span class="sxs-lookup"><span data-stu-id="f98ca-343">Type Abbreviations</span></span>

<span data-ttu-id="f98ca-344">[Abreviaturas de tipo](../language-reference/type-abbreviations.md) son una forma cómoda de asignar una etiqueta a otro tipo, como una firma de función o un tipo más complejo.</span><span class="sxs-lookup"><span data-stu-id="f98ca-344">[Type Abbreviations](../language-reference/type-abbreviations.md) are a convenient way to assign a label to another type, such as a function signature or a more complex type.</span></span> <span data-ttu-id="f98ca-345">Por ejemplo, el siguiente alias asigna una etiqueta a lo que se necesita para definir un cálculo con [CNTK](https://www.microsoft.com/cognitive-toolkit/), un profundo aprendizaje biblioteca:</span><span class="sxs-lookup"><span data-stu-id="f98ca-345">For example, the following alias assigns a label to what's needed to define a computation with [CNTK](https://www.microsoft.com/cognitive-toolkit/), a deep learning library:</span></span>

```fsharp
open CNTK

// DeviceDescriptor, Variable, and Function all come from CNTK
type Computation = DeviceDescriptor -> Variable -> Function
```

<span data-ttu-id="f98ca-346">El `Computation` nombre es una forma cómoda para indicar cualquier función que coincide con la signatura es alias.</span><span class="sxs-lookup"><span data-stu-id="f98ca-346">The `Computation` name is a convenient way to denote any function that matches the signature it is aliasing.</span></span> <span data-ttu-id="f98ca-347">Utilizando las abreviaturas de tipo parecido a esto es cómodo y permite el código más conciso.</span><span class="sxs-lookup"><span data-stu-id="f98ca-347">Using Type Abbreviations like this is convenient and allows for more succinct code.</span></span>

### <a name="avoid-using-type-abbreviations-to-represent-your-domain"></a><span data-ttu-id="f98ca-348">Evite el uso de las abreviaturas de tipo para representar el dominio</span><span class="sxs-lookup"><span data-stu-id="f98ca-348">Avoid using Type Abbreviations to represent your domain</span></span>

<span data-ttu-id="f98ca-349">Aunque las abreviaturas de tipo son convenientes para asignar un nombre a las firmas de función, puede resultar confusos cuando abreviar otros tipos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-349">Although Type Abbreviations are convenient for giving a name to function signatures, they can be confusing when abbreviating other types.</span></span> <span data-ttu-id="f98ca-350">Tenga en cuenta esta abreviatura:</span><span class="sxs-lookup"><span data-stu-id="f98ca-350">Consider this abbreviation:</span></span>

```fsharp
// Does not actually abstract integers.
type BufferSize = int
```

<span data-ttu-id="f98ca-351">Esto puede resultar confuso de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="f98ca-351">This can be confusing in multiple ways:</span></span>

* <span data-ttu-id="f98ca-352">`BufferSize` no es una abstracción; es simplemente otro nombre para un número entero.</span><span class="sxs-lookup"><span data-stu-id="f98ca-352">`BufferSize` is not an abstraction; it is just another name for an integer.</span></span>
* <span data-ttu-id="f98ca-353">Si `BufferSize` se expone en una API pública, puede fácilmente malinterpretarse implica algo más que `int`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-353">If `BufferSize` is exposed in a public API, it can easily be misinterpreted to mean more than just `int`.</span></span> <span data-ttu-id="f98ca-354">En general, los tipos de dominio tiene varios atributos a ellos y no son tipos primitivos como `int`.</span><span class="sxs-lookup"><span data-stu-id="f98ca-354">Generally, domain types have multiple attributes to them and are not primitive types like `int`.</span></span> <span data-ttu-id="f98ca-355">Esta abreviatura infringe esa suposición.</span><span class="sxs-lookup"><span data-stu-id="f98ca-355">This abbreviation violates that assumption.</span></span>
* <span data-ttu-id="f98ca-356">Las mayúsculas y minúsculas de `BufferSize` (PascalCase) implica que este tipo contiene más datos.</span><span class="sxs-lookup"><span data-stu-id="f98ca-356">The casing of `BufferSize` (PascalCase) implies that this type holds more data.</span></span>
* <span data-ttu-id="f98ca-357">Este alias no ofrece una mayor claridad, en comparación con el suministro de un argumento con nombre a una función.</span><span class="sxs-lookup"><span data-stu-id="f98ca-357">This alias does not offer increased clarity compared with providing a named argument to a function.</span></span>
* <span data-ttu-id="f98ca-358">La abreviatura no se manifestará en IL compilado; es simplemente un entero y este alias es una construcción de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="f98ca-358">The abbreviation will not manifest in compiled IL; it is just an integer and this alias is a compile-time construct.</span></span>

```fsharp
module Networking =
    ...
    let send data (bufferSize: int) =
        ...
```

<span data-ttu-id="f98ca-359">En resumen, el problema con las abreviaturas de tipo es que son **no** abstracciones de los tipos que son abreviar.</span><span class="sxs-lookup"><span data-stu-id="f98ca-359">In summary, the pitfall with Type Abbreviations is that they are **not** abstractions over the types they are abbreviating.</span></span> <span data-ttu-id="f98ca-360">En el ejemplo anterior, `BufferSize` es simplemente una `int` tras los bastidores, con ningún dato adicional ni cualquier mejora en el sistema de tipo además de lo que `int` ya tiene.</span><span class="sxs-lookup"><span data-stu-id="f98ca-360">In the previous example, `BufferSize` is just an `int` under the covers, with no additional data, nor any benefits from the type system besides what `int` already has.</span></span>