# You Don't Know JS Yet: Get Started - 2da Edición

# Apéndice B: ¡Practica, Practica, Practica!

En este apéndice, exploraremos algunos ejercicios y sus soluciones sugeridas. Estos son solo para _comenzar_ con la práctica de los conceptos del libro.

## Practicando Comparaciones

Practiquemos el trabajo con tipos de valores y comparaciones (Capítulo 4, Pilar 3) donde será necesario involucrar la coerción.

`scheduleMeeting(..)` debe tener una hora de inicio (en formato de 24 horas como una cadena "hh:mm") y una duración de la reunión (cantidad de minutos). Debería devolver `true` si la reunión cae completamente dentro de la jornada laboral (según las horas especificadas en `dayStart` y `dayEnd`); devolver `false` si la reunión infringe los límites de la jornada laboral.

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime, durationMinutes) {
    // ..TODO..
}

scheduleMeeting("7:00", 15); // false
scheduleMeeting("07:15", 30); // false
scheduleMeeting("7:30", 30); // true
scheduleMeeting("11:30", 60); // true
scheduleMeeting("17:00", 45); // true
scheduleMeeting("17:30", 30); // false
scheduleMeeting("18:00", 15); // false
```

Intenta resolver esto tú mismo primero. Considere el uso de operadores de comparación relacional y de igualdad, y cómo la coerción afecta este código. Una vez que tenga un código que funcione, _compare_ su(s) solución(es) con el código de "Soluciones sugeridas" al final de este apéndice.

## Practicando Clausuras

Ahora practiquemos con el cierre (Capítulo 4, Pilar 1).

La función `range(..)` toma un número como su primer argumento, representando el primer número en un rango deseado de números. El segundo argumento también es un número que representa el final del rango deseado (inclusive). Si se omite el segundo argumento, se debe devolver otra función que espere ese argumento.

```js
function range(start, end) {
    // ..TODO..
}

range(3, 3); // [3]
range(3, 8); // [3,4,5,6,7,8]
range(3, 0); // []

var start3 = range(3);
var start4 = range(4);

start3(3); // [3]
start3(8); // [3,4,5,6,7,8]
start3(0); // []

start4(6); // [4,5,6]
```

Intenta resolver esto tú mismo primero.

Una vez que tenga un código que funcione, _compare_ su(s) solución(es) con el código de "Soluciones sugeridas" al final de este apéndice.

## Practicando Prototipos

Finalmente, trabajemos en `this` y objetos vinculados vía prototipo (Capítulo 4, Pilar 2).

Defina una máquina tragamonedas con tres carretes que pueden `spin()` individualmente y luego `display()` el contenido actual de todos los carretes.

El comportamiento básico de un solo carrete se define en el objeto `reel` a continuación. Pero la máquina tragamonedas necesita carretes individuales: objetos que se deleguen en un `reel`, y cada uno de los cuales tenga una propiedad de `position`.

Un carrete solo _sabe cómo_ `display()` su símbolo de tragamonedas actual, pero una máquina tragamonedas normalmente muestra tres símbolos por carrete: el tragamonedas actual (`position`), un tragamonedas arriba (`position - 1`) y una ranura debajo (`position + 1`). Por lo tanto, mostrar la máquina tragamonedas debería terminar mostrando una cuadrícula de 3 x 3 de símbolos de tragamonedas.

```js
function randMax(max) {
    return Math.trunc(1e9 * Math.random()) % max;
}

var reel = {
    symbols: ["♠", "♥", "♦", "♣", "☺", "★", "☾", "☀"],
    spin() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        this.position =
            (this.position + 100 + randMax(100)) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        return this.symbols[this.position];
    },
};

var slotMachine = {
    reels: [
        // this slot machine needs 3 separate reels
        // hint: Object.create(..)
    ],
    spin() {
        this.reels.forEach(function spinReel(reel) {
            reel.spin();
        });
    },
    display() {
        // TODO
    },
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

Intente resolver esto usted mismo primero.

Sugerencias:

-   Use el operador de módulo `%` para envolver `position` a medida que accede a los símbolos circularmente alrededor de un carrete.

-   Utilice `Object.create(..)` para crear un objeto y vincularlo a otro objeto. Una vez vinculado, la delegación permite que los objetos compartan `this` durante la invocación del método.

-   En lugar de modificar el objeto del carrete directamente para mostrar cada una de las tres posiciones, puede usar otro objeto temporal (`Object.create(..)` otra vez) con su propia `position`, para delegar.

Una vez que tenga un código que funcione, _compare_ su(s) solución(es) con el código de "Soluciones sugeridas" al final de este apéndice.

## Soluciones Sugeridas

Tenga en cuenta que estas soluciones sugeridas son solo eso: sugerencias. Hay muchas maneras diferentes de resolver estos ejercicios de práctica. Compare su enfoque con lo que ve aquí y considere los pros y los contras de cada uno.

Solución sugerida para la práctica de "Comparaciones" (Pilar 3):

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime, durationMinutes) {
    var [, meetingStartHour, meetingStartMinutes] =
        startTime.match(/^(\d{1,2}):(\d{2})$/) || [];

    durationMinutes = Number(durationMinutes);

    if (
        typeof meetingStartHour == "string" &&
        typeof meetingStartMinutes == "string"
    ) {
        let durationHours = Math.floor(durationMinutes / 60);
        durationMinutes = durationMinutes - durationHours * 60;
        let meetingEndHour = Number(meetingStartHour) + durationHours;
        let meetingEndMinutes = Number(meetingStartMinutes) + durationMinutes;

        if (meetingEndMinutes >= 60) {
            meetingEndHour = meetingEndHour + 1;
            meetingEndMinutes = meetingEndMinutes - 60;
        }

        // recomponer cadenas de tiempo totalmente calificadas
        // (para facilitar la comparación)
        let meetingStart = `${meetingStartHour.padStart(
            2,
            "0"
        )}:${meetingStartMinutes.padStart(2, "0")}`;
        let meetingEnd = `${String(meetingEndHour).padStart(2, "0")}:${String(
            meetingEndMinutes
        ).padStart(2, "0")}`;

        // NOTA: dado que las expresiones son todas cadenas,
        // las comparaciones aquí son alfabéticas, pero es
        // a salvo aquí ya que están completamente calificados
        // cadenas de tiempo (es decir, "07:15" < "07:30")
        return meetingStart >= dayStart && meetingEnd <= dayEnd;
    }

    return false;
}

scheduleMeeting("7:00", 15); // false
scheduleMeeting("07:15", 30); // false
scheduleMeeting("7:30", 30); // true
scheduleMeeting("11:30", 60); // true
scheduleMeeting("17:00", 45); // true
scheduleMeeting("17:30", 30); // false
scheduleMeeting("18:00", 15); // false
```

---

Solución sugerida para la práctica de "Cierre" (Pilar 1):

```js
function range(start, end) {
    start = Number(start) || 0;

    if (end === undefined) {
        return function getEnd(end) {
            return getRange(start, end);
        };
    } else {
        end = Number(end) || 0;
        return getRange(start, end);
    }

    // **********************

    function getRange(start, end) {
        var ret = [];
        for (let i = start; i <= end; i++) {
            ret.push(i);
        }
        return ret;
    }
}

range(3, 3); // [3]
range(3, 8); // [3,4,5,6,7,8]
range(3, 0); // []

var start3 = range(3);
var start4 = range(4);

start3(3); // [3]
start3(8); // [3,4,5,6,7,8]
start3(0); // []

start4(6); // [4,5,6]
```

---

Solución sugerida para la práctica de "Prototipos" (Pilar 2):

```js
function randMax(max) {
    return Math.trunc(1e9 * Math.random()) % max;
}

var reel = {
    symbols: ["♠", "♥", "♦", "♣", "☺", "★", "☾", "☀"],
    spin() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        this.position =
            (this.position + 100 + randMax(100)) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        return this.symbols[this.position];
    },
};

var slotMachine = {
    reels: [Object.create(reel), Object.create(reel), Object.create(reel)],
    spin() {
        this.reels.forEach(function spinReel(reel) {
            reel.spin();
        });
    },
    display() {
        var lines = [];

        // display all 3 lines on the slot machine
        for (let linePos = -1; linePos <= 1; linePos++) {
            let line = this.reels.map(function getSlot(reel) {
                var slot = Object.create(reel);
                slot.position =
                    (reel.symbols.length + reel.position + linePos) %
                    reel.symbols.length;
                return slot.display();
            });
            lines.push(line.join(" | "));
        }

        return lines.join("\n");
    },
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

Eso es todo por este libro. Pero ahora es el momento de buscar proyectos reales para practicar estas ideas. ¡Siga programando, porque esa es la mejor manera de aprender!
