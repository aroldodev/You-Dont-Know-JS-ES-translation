# You Don't Know JS Yet: Get Started - 2da Edición

# Apéndice A: Explorando más a fondo

En este apéndice, vamos a explorar algunos temas del texto del capítulo principal con un poco más de detalle. Piense en este contenido como una vista previa opcional de algunos de los detalles más matizados cubiertos en el resto de la serie de libros.

## Valores vs. Referencias

En el Capítulo 2, presentamos los dos tipos principales de valores: primitivos y objetos. Pero aún no discutimos una diferencia clave entre los dos: cómo se asignan y se transmiten estos valores.

En muchos idiomas, el desarrollador puede elegir entre asignar/pasar un valor como el valor en sí mismo o como una referencia al valor. En JS, sin embargo, esta decisión está totalmente determinada por el tipo de valor. Eso sorprende a muchos desarrolladores de otros lenguajes cuando comienzan a usar JS.

Si asigna/pasa un valor en sí mismo, el valor se copia. Por ejemplo:

```js
var myName = "Kyle";

var yourName = myName;
```

Aquí, la variable `yourName` tiene una copia separada de la cadena `"Kyle"` del valor que está almacenado en `myName`. Esto se debe a que el valor es un primitivo, y los valores primitivos siempre se asignan/pasan como **copias de valor**.

Así es como puede probar que hay dos valores separados involucrados:

```js
var myName = "Kyle";

var yourName = myName;

myName = "Frank";

console.log(myName);
// Frank

console.log(yourName);
// Kyle
```

¿Ves cómo `yourName` no se vio afectado por la reasignación de `myName` a `"Frank"`? Eso es porque cada variable tiene su propia copia del valor.

Por el contrario, las referencias son la idea de que dos o más variables apuntan al mismo valor, de modo que la modificación de este valor compartido se reflejaría en un acceso a través de cualquiera de esas referencias. En JS, solo los valores de los objetos (matrices, objetos, funciones, etc.) se tratan como referencias.

Considere:

```js
var myAddress = {
    street: "123 JS Blvd",
    city: "Austin",
    state: "TX",
};

var yourAddress = myAddress;

// ¡Tengo que mudarme a una casa nueva!
myAddress.street = "456 TS Ave";

console.log(yourAddress.street);
// 456 TS Ave
```

Debido a que el valor asignado a `myAddress` es un objeto, se mantiene/asigna por referencia y, por lo tanto, la asignación a la variable `yourAddress` es una copia de la referencia, no el valor del objeto en sí. Es por eso que el valor actualizado asignado a `myAddress.street` se refleja cuando accedemos a `yourAddress.street`. `myAddress` y `yourAddress` tienen copias de la referencia al único objeto compartido, por lo que una actualización de uno es una actualización de ambos.

Nuevamente, JS elige el comportamiento de copia del valor frente a copia de la referencia en función del tipo de valor. Las primitivos se mantienen por valor, los objetos se mantienen por referencia. No hay forma de anular esto en JS, de ninguna forma.

## Demasiadas formas de Funciones

Recuerde este fragmento de la sección "Funciones" en el Capítulo 2:

```js
var awesomeFunction = function (coolThings) {
    // ..
    return amazingStuff;
};
```

La expresión de función aquí se denomina _expresión de función anónima_, ya que no tiene un identificador de nombre entre la palabra clave `function` y la lista de parámetros `(..)`. Este punto confunde a muchos desarrolladores de JS porque a partir de ES6, JS realiza una "inferencia de nombre" en una función anónima:

```js
awesomeFunction.name;
// "awesomeFunction"
```

La propiedad `name` de una función revelará su nombre dado directamente (en el caso de una declaración) o su nombre inferido en el caso de una expresión de función anónima. Las herramientas de desarrollo suelen utilizar ese valor al inspeccionar el valor de una función o al informar un seguimiento del stack de errores.

Entonces, incluso una expresión de función anónima _podría_ obtener un nombre. Sin embargo, la inferencia de nombres solo ocurre en casos limitados, como cuando se asigna la expresión de función (con `=`). Si pasa una expresión de función como argumento a una llamada de función, por ejemplo, no se produce ninguna inferencia de nombre; la propiedad `name` será una cadena vacía, y la consola del desarrollador generalmente informará "(función anónima)".

Incluso si se infiere un nombre, **sigue siendo una función anónima.** ¿Por qué? Debido a que el nombre inferido es un valor de cadena de metadatos, no un identificador disponible para hacer referencia a la función. Una función anónima no tiene un identificador para usar para referirse a sí misma desde dentro de sí misma, para recursividad, desvinculación de eventos, etc.

Compare la forma de expresión de función anónima:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function someName(coolThings) {
    // ..
    return amazingStuff;
};

awesomeFunction.name;
// "someName"
```

Esta expresión de función es una _expresión de función con nombre_, ya que el identificador `someName` está directamente asociado con la expresión de función en tiempo de compilación; la asociación con el identificador `awesomeFunction` todavía no ocurre hasta el tiempo de ejecución en el momento de esa declaración. Esos dos identificadores no tienen que coincidir; a veces tiene sentido que sean diferentes, otras veces es mejor que sean iguales.

Observe también que el nombre explícito de la función, el identificador `someName`, tiene prioridad cuando se asigna un _name_ para la propiedad `name`.

¿Deben las expresiones de función ser nombradas o anónimas? Las opiniones varían ampliamente al respecto. La mayoría de los desarrolladores tienden a no preocuparse por el uso de funciones anónimas. Son más cortos e, indudablemente, más comunes en la amplia esfera del código JS que existe.

En mi opinión, si existe una función en su programa, tiene un propósito; de lo contrario, ¡sácaquela! Y si tiene un propósito, que tenga un nombre natural que describa ese propósito.

Si una función tiene un nombre, usted, el autor del código, debe incluir ese nombre en el código, para que el lector no tenga que inferir ese nombre al leer y ejecutar mentalmente el código fuente de esa función. Incluso el cuerpo de una función trivial como `x * 2` debe leerse para inferir un nombre como "doble" o "multBy2"; ese breve trabajo mental adicional es innecesario cuando podría tomarse un segundo para nombrar la función "doble" o "multBy2" _una vez_, ahorrando al lector ese trabajo mental repetido cada vez que se lea en el futuro.

Lamentablemente, en algunos aspectos, hay muchas otras formas de definición de funciones en JS a principios de 2020 (¡tal vez más en el futuro!).

Aquí hay algunos formularios de declaración más:

```js
// generator function declaration
function *two() { .. }

// async function declaration
async function three() { .. }

// async generator function declaration
async function *four() { .. }

// named function export declaration (ES6 modules)
export function five() { .. }
```

Y aquí hay algunas más de las (¡muchas!) formas de expresión de funciones:

```js
// IIFE
(function(){ .. })();
(function namedIIFE(){ .. })();

// asynchronous IIFE
(async function(){ .. })();
(async function namedAIIFE(){ .. })();

// arrow function expressions
var f;
f = () => 42;
f = x => x * 2;
f = (x) => x * 2;
f = (x,y) => x * y;
f = x => ({ x: x * 2 });
f = x => { return x * 2; };
f = async x => {
    var y = await doSomethingAsync(x);
    return y * 2;
};
someOperation( x => x * 2 );
// ..
```

Tenga en cuenta que las expresiones de función de flecha son **sintácticamente anónimas**, lo que significa que la sintaxis no proporciona una forma de proporcionar un identificador de nombre directo para la función. La expresión de función puede obtener un nombre inferido, pero solo si es una de las formas de asignación, no en la forma (¡más común!) de ser pasada como un argumento de llamada de función (como en la última línea del fragmento).

Dado que no creo que las funciones anónimas sean una buena idea para usar con frecuencia en sus programas, no soy partidario de usar la forma de función de flecha `=>`. Este tipo de función en realidad tiene un propósito específico (es decir, manejar la palabra clave `this` léxicamente), pero eso no significa que debamos usarla para cada función que escribimos. Utilizar la herramienta más adecuada para cada trabajo.

Las funciones también se pueden especificar en definiciones de clases y definiciones de objetos literales. Por lo general, se denominan "métodos" cuando están en estas formas, aunque en JS este término no tiene mucha diferencia observable sobre "función":

```js
class SomethingKindaGreat {
    // metodos de clase
    coolMethod() { .. }   // no comas!
    boringMethod() { .. }
}

var EntirelyDifferent = {
    // metodos de objeto
    coolMethod() { .. },   // comas!
    boringMethod() { .. },

    // propiedad de expresión de función (anónima)
    oldSchool: function() { .. }
};
```

¡Uf! Esas son muchas maneras diferentes de definir funciones.

No hay una ruta de atajo simple aquí; solo tiene que familiarizarse con todos los formularios de función para que pueda reconocerlos en el código existente y usarlos adecuadamente en el código que escribe. ¡Estudíelos de cerca y practique!

## Comparación condicional coercitiva

Sí, el nombre de esa sección es bastante complicado. ¿Pero de qué estamos hablando? Estamos hablando de expresiones condicionales que necesitan realizar comparaciones orientadas a la coerción para tomar sus decisiones.

`if` y `? ¨` Las sentencias ternarias, así como las cláusulas de prueba en los bucles `while` y `for`, realizan una comparación de valores implícita. ¿Pero de qué tipo? ¿Es "estricto" o "coercitivo"? Ambos, en realidad.

Considere:

```js
var x = 1;

if (x) {
    // ¡se ejecutará!
}

while (x) {
    // se ejecutará, una vez!
    x = false;
}
```

Podría pensar en estas expresiones condicionales `(x)` como esta:

```js
var x = 1;

if (x == true) {
    // ¡se ejecutará!
}

while (x == true) {
    // se ejecutará, una vez!
    x = false;
}
```

En este caso específico, el valor de `x` siendo `1`, ese modelo mental funciona, pero no es preciso en términos más generales. Considere:

```js
var x = "hello";

if (x) {
    // ¡se ejecutará!
}

if (x == true) {
    // no se ejecuta :(
}
```

Ups. Entonces, ¿qué está haciendo realmente la sentencia `if`? Este es el modelo mental más preciso:

```js
var x = "hello";

if (Boolean(x) == true) {
    // ¡se ejecutará!
}

// que es lo mismo que:

if (Boolean(x) === true) {
     // ¡se ejecutará!
```

Dado que la función `Boolean(..)` siempre devuelve un valor de tipo booleano, `==` frente a `===` en este fragmento es irrelevante; ambos harán lo mismo. Pero la parte importante es ver que antes de la comparación, se produce una coerción, de cualquier tipo que sea `x` actualmente, a booleano.

Simplemente no puede escapar de las coerciones en las comparaciones de JS. Anímase y apréndalos.

## "Clases" prototipo

En el Capítulo 3, presentamos prototipos y mostramos cómo podemos vincular objetos a través de una cadena de prototipos.

Otra forma de conectar tales enlaces de prototipo sirvió como el (honestamente, feo) predecesor de la elegancia del sistema de "clases" de ES6 (consulte el Capítulo 2, "Clases"), y se conoce como clases prototípicas.

| TIP:                                                                                                                                                          |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Si bien este estilo de código es bastante poco común en JS en estos días, ¡todavía es bastante común que te pregunten sobre él en las entrevistas de trabajo! |

Primero recordemos el estilo de codificación `Object.create(..)`:

```js
var Classroom = {
    welcome() {
        console.log("Welcome, students!");
    },
};

var mathClass = Object.create(Classroom);

mathClass.welcome();
// Welcome, students!
```

Aquí, un objeto `mathClass` está vinculado a través de su prototipo a un objeto `Classroom`. A través de este vínculo, la llamada a la función `mathClass.welcome()` se delega al método definido en `Classroom`.

El patrón de clase prototípico habría etiquetado este comportamiento de delegación como "herencia" y, alternativamente, lo habría definido (con el mismo comportamiento) como:

```js
function Classroom() {
    // ..
}

Classroom.prototype.welcome = function hello() {
    console.log("Welcome, students!");
};

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Todas las funciones por defecto hacen referencia a un objeto vacío en una propiedad llamada `prototity`. A pesar del nombre confuso, este **no** es el _prototipo_ de la función (donde la función está vinculada al prototipo), sino el objeto prototipo al que _vincularse_ cuando se crean otros objetos llamando a la función con `new`.

Agregamos una propiedad de `welcome` en ese objeto vacío (llamado `Classroom.prototype`), apuntando a la función `hello()`.

Luego `new Classroom()` crea un nuevo objeto (asignado a `mathClass`), y el prototipo lo vincula al objeto `Classroom.prototype` existente.

Aunque `mathClass` no tiene una propiedad/función `welcome()`, delega con éxito a la función `Classroom.prototype.welcome()`.

Este patrón de "clase prototípica" ahora se desaconseja enfáticamente, a favor de usar el mecanismo de `class` de ES6:

```js
class Classroom {
    constructor() {
        // ..
    }

    welcome() {
        console.log("Welcome, students!");
    }
}

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Debajo de las cubiertas, el mismo enlace prototipo está conectado, pero esta sintaxis de `clase` se ajusta al patrón de diseño orientado a la clase mucho más limpiamente que las "clases prototipo".
