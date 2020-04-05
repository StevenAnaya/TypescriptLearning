# TYPESCRIPT

**Typescript** es un Superset de Javascript, que en palabras comunes, es un lenguaje de programación que se construye encima de Javascript y permite agregarle nuevas funcionalidades y algunas ventajas que no tiene Javascript Perse.

**Typescript** no puede ser ejecutado por los navegadores o los runtimes como Node. Es decir, Typescript viene como una herramienta con su propio compilador que resuelve a codigo Javascript tradicional.

**Typescript** nos permite identificar errores tempranos y escribir un codigo mucho más limpio y libre de errores, sumandole, la super funcionalidad de tipado que nos provee.

## Instalación

Para instalar **Typescript** debemos correr el siguiente comando:

`npm install -g typescript`

- Este comando instala globalmente el paquete y podemos correr el compilador de forma manual usando `tsc [file name]`

## Core Types

- number: `1, 5.3, -10`
- string: `any type of string`
- boolean: `true or false`. No diferencia entre truthy/falsy values, unicamente booleans literales.
-object: `{age: 30}`. Objetos de cualquier tipo, pero pueden definirse el tipo del objeto
- array: `[]`. Puede contener otros tipos incluso array anidados.
- tuple: `[]`. Es de tamaño fijo y permite más de un tipo a la vez.
- enum: `enum { NEW, OLD }`. Son enumerados globalmente como identificadores de constantes.
- any: `*`. Permite agregar cualquier tipo de forma flexible. Se recomienda evitar este tipo ya que deshabilita las habilidades de **Typescript**.

## Basic Typing Sintax

Aca tendremos alguna sintaxis básica que usamos para definir tipos en Typescript:

``` typescript
  let name = 'Carlos'; //Inferred by Typescript as string type
  let surname:string; //Explicit type
  surname = 10; //Throw a typing error

  function add(n1: number, n2: number) {
    return n1 + n2
  }

  add('1', 2.5) // Throw an error due to invalid argument type

  let hobbies = ['Sports', 'VideoGames'] //Inferred to type Array with String content
  let job:string[];
  job = [2, 3, 4] //Throw an error due to invalid type inside the Array type
```

- El código que agregamos como tipo no se compila al código Javascript que nos entrega **Typescript**.

## Inferred vs Explicit

**Typescript** al igual que Javascript puede inferir el tipo asignado dependiendo del contenido de la variable. Es decir, si creamos una variable y le asignamos directamente un numero literal, este inferirá el tipo y lo pondra como `number` directamente. **Typescript** hace un buen trabajo con la inferencia de los tipos y en casos en los que tenemos de inicializar y asignar variables podemos evitar el tipado y dejar que **Typescript** se encargue de ello.

``` typescript
// TYPE INFERRED BY TYPESCRIPT
// Good practice use this type
const person = {
  name: 'Steven',
  age: 23
};

console.log(person.name)

// TYPE EXPLICITY
//Suboptimal implementation
const person2: object = {
  name: 'Steven',
  age: 23
}

//Property name is not defined on person2 object type
console.log(person2.name)

// TYPE EXPLICITY WITH PROPERTIES DEFINITIONS
const person3: {
  name: string;
  age: number;
} = {
  name: 'Steven',
  age: 23
}

//It works fine with property definition on object type
console.log(person3.name)
```

## Tupĺe

Son arreglos de dimensión fija y con tipos fijos. La notación de Tuple es la siguiente:

``` typescript
  let obj: {
    name: string;
    age: number;
    jobs: [number, string]
  } = {
    name: 'Steve',
    age: 30,
    jobs: [33, 'Software Developer']
  }

  obj.jobs = ['1', 'Designer'] // Throw an error due to first place must be a number

  obj.jobs = [1, 'Designer', 'Job description'] // Throw an error due to the fixed length on the tuple
```

- Una consideración con `Tuple` es que cuando hacemos una insercción de datos con el metodo `push()` este no arroja error al modificar la dimensión, por lo tanto, debemos estar precavidos con este comportamiento.

## Enum

Los `Enum` son tipos personalizados que nos entregan identificadores unicos globales que podemos usar como valores unicos permitidos.

``` typescript
  //Default Identifiers
  enum Role { ADMIN, AUTHOR, USER };
  ADMIN // 0
  AUTHOR // 1
  USER // 2

  // Custom identifiers
  enum Role { ADMIN = 'ADMIN', AUTHOR = 100, USER = 'USER' }
```
## Union Types

Los tipos `Union` nos permiten ser flexibles y establecer una propiedad que puede ser de más de 1 tipo.

``` typescript
  function combine(in1: number | string, in2: number | string) {
    let result;

    if (typeof in1 === 'number' && typeof in2 === 'number') {
      result = in1 + in2;
    } else {
      result = in1.toString() + in2.toString();
    }

    return result;
  }
```

- Estos tipos nos permiten indicar que una propiedad puede ser de más de un tipo.
- La comparación de tipos con Javascript no es requerida, pero en algunos casos puede que sea util para operaciones dependiendo del tipo que llega.

## Literal Types

Funcionan de igual manera que los valores literales en Javascript, pero estos son asignados como tipos. Es decir, podemos establecer que un tipo String puede ser en especifico una cadena de texto definida.

``` typescript
  let resultType: 'as-text' | 'as-number' | 'as-boolean';

  resultType = 2; // Value must be 'as-text' 'as-number' 'as-boolean'
```

- Esta es una combinación de un `Union` y `Literal Types`.

## Type Aliases

Nos permite crear tipos personalizados que pueden contener cualquier clase de tipo y los `Enum` y `Union`.

``` typescript
  type Combinable = number | string;
  type ConversionDescriptor = 'as-number' | 'as-text';
  enum PERMS { ADMIN, USER }
  type Perms = PERMS | number;
  let per: Perms;
```

## Functions

Las funciones se componen en cuanto a tipos, de dos maneras:

- Argumentos: Especifica el tipo de cada argumento recibido.
- Valores de retorno: Especifica el tipo del valor que retornará.

``` typescript
  function add(n1: number, n2: number): number {
    return n1 + n2;
  }

  function add2(n1: number, n2: number) {
    return n1 + n2;
  }

  function print(message: string): void {
    console.log(message);
  }

  function undef(): undefined {
    return;
  }
```

- Los ejemplos usan valores explicitos, pero al igual que en la mayoria, **Typescript** nos permite usar inferencia para no definir el tipo que vamos a retornar.
- El tipo `void` nos permite indicar que nuestra función no retornará nada. A diferencia de undefined, este sabe que dentro de la declaración de la función no debe existir la palabra `return`.

### Function Types

Con **Typescript** podemos definir tipos personalizados para nuestras funciones.

``` typescript
  function add(n1: number, n2: number) {
    return n1 + n2; //Inferred return type
  };

  function print(message: string) {
    console.log(message);
  }

  let combineValues: (a: number, b: number) => number;

  combineValues = print('Print my message on console'); // Throw Function type error
  combineValues = add(2, 2); // 4
```

Incluso, podemos especificar los tipos que tendran los callback dentro de nuestras funciones:

``` typescript
  function addAndHandle(n1: number, cb: (res: number) => void) {
    const result = n1++;
    cb(result);
  }

  addAndHandle(5, (result) => {
    console.log(result);
    return result; //Works fine
  })
```

- Algo importante a aclarar es el tipo de retorno de la función y es que dicha función `cb` está dispuesta a no retornar nada, pero aún así estamos retornando el resultado. Este comportamiento no arrojara error debido a que **Typescript** interpreta con `void` un "No me importa que retornes" y no se preocupa por restringir la regla a una declaración sin retorno. 

## Type Assertions

Existen situaciones en los que nosotros sabemos demasiado sobre un valor lo que podria inferir **Typescript**, en esos casos, podemos indicarle al compilador que no haga ninguna validación y deje en nuestras manos la responsabilidad de validación de tipo.

Estos no realizar ningun tipo de validación o de reestructuración de datos. No tiene impacto en el momento de ejucución y es usado enteramente por el compilador.

Existen dos tipos de sintaxis con `Type Assertions`:

- `Angle-bracket`:

``` typescript
  let someValue: any = "this is a string";
  let srtlength: number = (<string>someValue).length;
```

- `as - sintaxis`:

``` typescript
  let someValue: any = "this is a string";
  let srtLength: number = (someValue as string).length;
```

- Los dos ejemplos son equivalentes y la preferencia entre uno y otro es cuestión de gustos. Sin embargo, cuando usamos **Typescript** con JSX solo podemos usar la sintaxis `as`;

## Interfaces

Son un principio central de **Typescript** que permite definir la estructura o forma de un valor. Las `Interfaces` toman el rol de nombrar y definir esos tipos.

``` typescript
function printLabel(labeledObj: { label: string }) {
  console.log(labeledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};

//USING INTERFACE

interface LabeledValue {
    label: string;
    size?: number;
}

function printLabel(labeledObj: LabeledValue) {
    console.log(labeledObj.label);
}

let myObj = {label: "Size 10 Object"};
printLabel(myObj);
```
- Las `Interfaces` no requieren que el orden de las propiedades sea exacta, el validador unicamente velará por la presencia de los elementos que son requeridos dentro de la Interfaz.

### Readonly properties

Estas propiedades nos permiten definir permisos unicamente de modificación cuando son creadas las instancias, es decir, no se puede modificar ningun valor que este especificado como `readonly`:

``` typescript
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

**Typescript** tiene un tipo llamado `ReadonlyArray<T>` con todos los metodos mutables removidos. Igualmente, podemos usar este mismo ejemplo con `Type Assertions`:

``` typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!

//TYPE ASSERTIONS
a = ro as number[];
```

Existen escenarios en los que al momento de pasar objetos a una funcion/clase que tienen una interfaz, los escribimos mal y puede que Javascript falle silenciosamente. Para esto, podemos usar `Type Assertions` o indicarle a una nuestra interfaz que podemos recibir más valores de los que definimos para que no haga `excess property checks` y arroje error por cualquier cosa.

``` typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);

interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}

//THIS IS THE BETTER APPROACH
let squareOptions = { colour: "red" }
let mySquare = createSquare(squareOptions)
```

### Function Types

Las `Interfaces` son capaces de describir un amplio espectro de estructuras o formas las cuales pueden tomar los objetos Javascript. `Interfaces` también pueden describir tipos de funciones. Para describir una función tenemos que seguir la siguiente estructura:

``` typescript
  interface SearchFunc {
    (source: string, subString: string): boolean;
  }

  let mySearch: SearchFunc;
  mySearch = function (src: string, sub: string): boolean {
    let result = source.search(subString);
    return result > -1;
  }
```

- El nombre de los parametros no tienen validaciones. Podemos sobreescribir el nombre de las varialbes siempre y cuando sigamos la estructura que definimos en la `Interface`.

### Extend Interfaces

Las `Interfaces` pueden extender sus propiedades de otras que ya han sido creadas anteriormente.

``` typescript
  interface Shape {
    a: number;
  }

  interface Props {
    color: string;
  }

  interface Circle extends Shape, Props {
    ratio: number;
    position: number;
  }

  let circle = {} as Square;
  square.color = "blue";
  square.a = 10;
  square.ratio = 5;
```
## Generics

La mayor parte del desarrollo de software lo pasamos creando componentes que no solo tienen APIs consistentes y bien definidas, aunque tambien son reusables. Los componentes que son capaces de trabajar con cualquier set de datos nos darán capacidades de flexibilidad importantes para construir sistemas grandes.

En lenguajes como C# o Java, existen herramientas como `generics` para realizar componentes reusables. Eso significa que podemos trabajar con componentes que no solo soportar tipos de una sola variedad. Esto significa que los usuarios pueden usar estos componentes con sus propios datos.

Las `Generics` son declaraciones(Tipos) que retornarán lo que sea que se le haya pasado como tipo. Podemos asimilarlo como una Pure Function del paradigma funcional. 

``` typescript
function identity(arg: number): number {
    return arg;
}

//This a unsafe example of a Generic
function identity(arg: any): any {
    return arg;
}
```
- Las funciones descritas arribas son algun intento de funciones `Generics` pero vemos algunas restricciones como el tipo estatico o la perdida de las validaciones con el tipo `any`.

En lugar de lo anterios, necesitamos capturar el tipo del argumento de forma que podamos tambien usarla para declarar que será retornado. Aquí vamos a usar `type variable`, un tipo especial de variable que opera sobre el tipo y no sobre el valor.

``` typescript
function identity<T>(arg: T): T {
    return arg;
}
```

- Ahora, agregamos un `type variable` T para la función. Esta T nos permite capturar el tipo que el usuario provee, por ejemplo: `number`. Luego, usamos T de nuevo como el tipo de retorno. En profundidad, tenemos ahora el mismo tipo en el argumento y en el retorno.

Podemos decir que esta versión de la función es `Generic` debido a que puede trabajar con un amplio numero de tipos. A diferencia de usar `any`, este es tan preciso como la primer versión que usaba tipo `number`para el argumento y el retorno.

Existen dos formas de llamar una función tipo `Generic`:

- `Explicit`

``` typescript
  let a = identity<string>("myString")
```

- `Inferred`

``` typescript
  let a = indenty("myString");
```

- Podemos notar que no es necesario pasar el tipo explicito debido a que el asigna a T el valor de "myString". El tipo `Inferred` puede ser corto y mucho más claro y es común ver está, pero puede que necesites usar el `Explicit` cuando el inferred de Typescript no funcione bien.