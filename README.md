# Introducción a C#

## Primeros pasos: Nociones básicas

### La directiva `using`

````csharp
using Microsoft.Xna.Framework;
````

> 🌏 https://dev.to/rafaeljcamara/c-using-keyword-1fih

El uso de `using` es llamado en c# **directive using**. Esta directiva lo que nos permite es **no tener que estar instanciando el objeto que queremos utilizar continuamente**.

Por ejemplo, en este caso: `using Microsoft.Xna.Framework.Graphics;` nos permite utilizar la instrucción que hay más abajo:
`_graphics = new GraphicsDeviceManager(this);` **sin necesidad de tener que estar haciendo**:

`var graphics = new Microsoft.Xna.Framework.Graphics.GraphicsDeviceManager(this);`

Así que sería como "pre-instanciar" la clase que nos permita luego hacer la instacia per sé.

### La directiva `namespace`

`namespace c_tgc_game;`

Lo que hace es definir como un "scope" al que van a pertenecer las variables. Es decir, que todas las variables que creemos dentro de este `namespace` van a pertenecer únicamente a éste, y si hay otra variable llamada **exactamente igual** en otro `namespace`, **no** presentarán conflictos entre ellos.

### Modificadores de acceso

- Ya sabemos que hay ciertos lenguajes de backend (Java, C#, C++) que son deominados como `lenguajes de programación orientado a ibjetos (POO)`. En este tipo de lenguajes, existe algo llamado `modificadores de acceso` (`public`, `private` y `protected`), que determina el **grado de accesibilidad** de una variable:

- Si una variable es `public` (**pública**) es accesible desde cualquier parte del código. 
- Si es `private` (**privada**), lo es solo desde el mismo fichero. 

- Si es `protected`, **protegida**, es accesible desde cualquier parte del código, **excepto** desde el mismo fichero.


## 1. Creando una API REST con .NET

Para empezar, la herramienta utilizada para desarrollar aplicaciones web, en el caso de C#, es **ASP.NET**. 

> 🌏 https://learn.microsoft.com/es-es/aspnet/core/fundamentals/apis?view=aspnetcore-9.0

Vamos a realizar una **API mínima**, que es lo recomendado por Microsoft para configuraciones mínimas.

La otra opción serían las **APIS basadas en controladores**, pero eso lo dejaremos para otro proyecto futuro.

> 🌏 https://learn.microsoft.com/es-es/aspnet/core/tutorials/min-web-api?view=aspnetcore-9.0&tabs=visual-studio-code#create-an-api-project

**Dentro de la carpeta** donde vayamos a crear el proyecto, debemos escribir en la terminal la siguiente instrucción:

```bash
dotnet new web -o TodoApi
cd TodoApi
code -r ../TodoApi
```

**TodoApi** es el nombre del proyecto que le dan en el tutorial, pero nosotros vamos a llamarlo **c-basic-api**

Nos lanzará por terminal un mensaje como este:

````bash
The template "ASP.NET Core Empty" was created successfully.

Processing post-creation actions...
Restoring /c-basic-api/c-basic-api.csproj:
Restore succeeded.
````

Y veremos que se ha creado una carpeta con el nombre elegido por nosotros y con lo necesario para comenzar. 

Si vemos el fichero `Program.cs` veremos el siguiente código:

````csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
````

Según la documentación oficial:

> Crea los elementos WebApplicationBuilder y WebApplication con valores predeterminados preconfigurados.
> Crea un punto de conexión / HTTP GET que devuelve Hello World!.

Es decir, que la instrucción ``var builder = WebApplication.CreateBuilder(args)`` nos permite crear una instancia de la clase ``WebApplication`` con la cual comenzar a definir la API, y que ``.MapGet`` es una función que define una ruta (`/`) y que devuelve un string (si hacemos una llamada a esa ruta) que devuelve ``Hello World``.

> Para que nuestra API funcione correctamente, debemos ejecutar el siguiente comando en la terminal:
> ``dotnet dev-certs https --trust``.
> Esto nos permitirá **obtener un importante certificado** para hacer funcionar nuestra API en nuestro entorno local.

> 🌏 https://learn.microsoft.com/es-es/aspnet/core/tutorials/min-web-api?view=aspnetcore-9.0&tabs=visual-studio-code#run-the-app

Pero a mí me surge una pregunta: ¿Qué estructura se supone que debo utilizar en una API realizada en C#?

## 2. Arquitectura de una API mínima en .NET

> 🌏 Fuente: https://treblle.com/blog/how-to-structure-your-minimal-api-in-net


Para empezar, ¿qué es una *API mínima*?

> Minimal API is a streamlined approach to building REST APIs in .NET, focusing on brevity
> in code, minimal configuration, and a significant reduction in the usual formalities associated with traditional methods.

Es decir, está hecha para *APIS simples* donde no requiramos de una gran lógica ni de un gran desarrollo. En aplicaciones grandes se usarán en la mayoría de los casos una API basada en controladores, pero en este caso vamos a ver cómo organizar una API mínima.

Aunque en la página nos desarrollan un poco más las diferencias, vamos a fijarnos concretamente en esta:

> Better fit for *Vertical slice architecture*: Minimal API aligns perfectly with vertical 
> slice architecture, which centers around building applications around specific features, 
> often involving just one endpoint. This focus on feature logic aligns seamlessly with Minimal API's principles.

### ¿Qué es *Vertical slice architecture*?

> 🌏 https://www.jimmybogard.com/vertical-slice-architecture/

En esta página, lo define así:

> So what is a "Vertical Slice Architecture"? In this style, my architecture is built around distinct requests, encapsulating and grouping all concerns from front-end to back. You take a normal "n-tier" or hexagonal/whatever architecture and remove the gates and barriers across those layers, and couple along the axis of change:

Pero podemos encontrar una definición más sencilla en esta otra:

> 🌏 https://medium.com/@anujguptaninja/vertical-slice-architecture-structuring-vertical-slices-in-your-application-674825367c3d

> Vertical Slice Architecture focuses on separating features into individual vertical slices instead of organizing the entire system by layers. Each slice encapsulates all aspects of a feature, including business logic, data access, and presentation logic.

Es decir, *cada feature, con sus TODOs, casos de uso, endpoints, etc se agrupan en su propia capa, quedando así separadas del resto*. 

> - Loose coupling between features.
> - Better scalability as new features can be added without affecting others.
> - Higher maintainability as each feature is isolated and self-contained.

El objetivo es *aislar el acoplamiento entre las distintas features*, *tener una mejor escalabilidad donde al modificar un caso de uso afecte a una menor proporción de capas*, y *ayudar al mantenimiento y aislamiento de las mismas*.

#### Vertical Slice Architecture vs Featured Architecture

Una de las dudas que pueden surgir, es que la *Vertical Slice Architecture* (VSA) se *parece bastante* a la *Featured Architecture*, pues ambas tienen una organización *muy parecida*. Las dos se estructuran *a partir de features*, lo cual puede inducir a confusión. Sin embargo, en lo que se diferencian  es en el *planteamiento de la misma*:

- VSA es *un principio*, donde enfoca el código en torno a *casos de uso*. Es decir, que luego se emplee la denominación de *feature* no es más que una conveniencia etimológica que nos permite organizar el código en torno a ese concepto, pero <u>siempre teniendo en cuenta que tratamos *casos de uso</u>. Eso significa que *no siempre* corresponderán a una feature, pues depende más bien aquello que englobe y no tanto de qué se trate o no de una feature como tal.
Cada capa (layer) encapsula todo lo necesario para ese caso: endpoint, handler/command/query, validación, acceso a datos, mapeos, etc. La idea clave es acoplar a lo que cambia junto (feature/caso de uso) y no por capas técnicas.

- Por otro, *Featured Architecture* se enfoca en, literalmente, eso: *las features*, por lo que existe la posibilidad de un *mayor acoplamiento* dado que no se busca lo que sí pretende *VSA* (es decir,  el mayor desacople posible entre capas), sino el agrupamiento por *features* como tal, independientemente de a cuántas capas se terminen afectando.

> 📝 En la fuente de _medium_ mencionada anteriormente, en el apartado de _Folder Structure_, puedes ver un ejemplo tangible de VSA

## 3. Creando nuestra primera ruta.

Vamos a crear la primera ruta donde devolveremos unos datos obtenidos del **INE** (Instituto Nacional de Estadística). 

Lo primero de todo es crear el fichero donde escribiremos el código. Teniendo en cuenta el VSA, vamos a llamar a la carpeta `DatosINE`.

> 🖌️ Es un nombre provisional, susceptible a cambio.

Bien, ahora que sabemos que lo que queremos es crear una ruta `GET` (porque queremos devolver unos datos cuando desde el lado cliente se nos haga una petición), vamos a hacerlo siguiendo el patrón **CQRS**.

### ¿Qué es CQRS?

> 🌏 https://martinfowler.com/bliki/CQRS.html

**CQRS**, que responde a la abreviatura de **Command Query Responsability Segregation**, es un patrón que pretende **separar** las peticiones http en **dos tipos**: 
1. **Query**, que son aquellas consultas **que no modifican nada**.
2. **Command**, que son aquellas que **sí** modifican algo.

Por ejemplo: una petición `GET` **siempre** será **query**, porque es una mera consulta de datos; por otro, las peticiones `POST` y `PUT` será consideradas **commands**, porque ambas **modifican** algo (ya sea creando un objeto o actualizándolo).

> 📝 Tienes más información del problema que pretende resolver y su enfoque aquí: https://learn.microsoft.com/es-es/azure/architecture/patterns/cqrs

> ‼️ Es mi primera vez aplicándolo en un lenguaje de backend, con unas reglas léxicas bastante distintas al front, así que no te preocupes si cometes errores 📚.

#### Integrando CQRS

Si buscamos información sobre cómo implementar CQRS en .NET, encontraremos una librería llamada `MediatR`:

> 🌏 https://www.netmentor.es/entrada/tutorial-mediatr-dotnet

Se trata de una librería **muy popular** que se utiliza frecuentemente con este patrón, puesto que permite incluir el patrón `mediator` de una manera escalable y funcional. Sin embargo, para un proyecto pequeño puede resultar *overkill*. Dado que estamos aprendiendo, vamos a intentar gestionar algunos aspectos nosotros mismos para aprovechar y aprender.

> 👩🏼‍💻 Si quieres saber cuáles son las ventajas de implementar CQRS, puedes leer más al respecto aquí:
> https://learn.microsoft.com/es-es/azure/architecture/patterns/cqrs#benefits-of-cqrs

> 🦄 En esta otra web también está muy bien explicado: https://www.kurrent.io/cqrs-pattern

Tomemos un ejemplo de código de cómo implementar CQRS (según la documentación oficial de Microsoft):

> 🌏 > 🔗 https://learn.microsoft.com/es-es/azure/architecture/patterns/cqrs#example

1. Supongamos que tenemos esta clase simple: 

````csharp
namespace ReadModel
{
  public class ProductInventory
  {
    public int Id { get; set; }
    public string Name { get; set; }
    public int CurrentStock { get; set; }
  }
}
````

Una clase pública que tiene como propiedades un `Id`, `Name` y `CurrentStock`

> 📚 En C# es buena práctica nombrar a las propiedes **públicas** y **protected** utilizando el formato `PascalCase`, mientras que las **privadas** se escriben en minúscula y precedidas por **_**. Ejemplo en código:
> `````csharp
> public class Animal
> {
>    protected int Age;
>    public string Name
>    private string _internalId;
> }
> `````

Y lo que queremos es poder hacer una serie de acciones con la clase que hemos creado. En este caso, son productos, así que queremos **añadir productos a nuestro inventario**, por ejemplo.
La manera común sería hacerlo en un archivo distinto con otro nombre: quizás una clase llamada `ProductInventoryRepository` donde desarrollaramos esa acción, por ejemplificar.

Sin embargo, si seguimos el patrón CQRS, lo adecuado será **crearnos una clase `Handler` que maneje estas vicisitudes**:


``````csharp
public class ProductsCommandHandler :
    ICommandHandler<AddToInventory>,
{
  private readonly IRepository<Product> repository;

  public ProductsCommandHandler (IRepository<Product> repository)
  {
    this.repository = repository;
  }


  void Handle (AddToInventory command)
  {
    ...
  }
}
``````


## TODO Instalación Swagger

> 🌏 https://learn.microsoft.com/es-es/aspnet/core/tutorials/min-web-api?view=aspnetcore-9.0&tabs=visual-studio-code#install-swagger-tooling

