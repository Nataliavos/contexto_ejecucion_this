## 📘 README
Exposición JavaScript – Contexto de ejecución y this
Curso: JavaScript – Entrenamiento como Desarrollador de Software
Tema: Contexto de ejecución y this
Nivel: Principiante

🎯 Objetivo de la exposición
Explicar de forma clara y sencilla qué es this en JavaScript, cómo cambia según el contexto de ejecución y cómo se comporta con:

-Objetos
-Funciones normales
-Arrow functions
-Clases
-bind, call y apply

🧠 Idea clave (la más importante)
this depende de cómo se llama una función, no de dónde se define.
Si el público entiende esta frase, el objetivo está cumplido.

🗣️ Introducción
Hoy vamos a hablar sobre el contexto de ejecución y la palabra clave this en JavaScript.
Este es uno de los temas que más confunde cuando uno empieza, así que la idea no es memorizar reglas, sino entenderlo con ejemplos y situaciones del día a día.
----------------------------------------
# Qué es this?
this es una palabra reservada de JavaScript.
Hace referencia al objeto que está ejecutando una función en ese momento.
Su valor no es fijo, cambia según el contexto.

````const usuario = {
  nombre: "Ana",
  rol: "Administradora",
  saludar() {
    console.log("Hola, soy " + this.nombre);
  },
  mostrarRol() {
    console.log("Mi rol es " + this.rol);
  }
};

usuario.saludar();
usuario.mostrarRol();````

Analogía
this es como decir “yo”.
El significado de “yo” cambia dependiendo de quién esté hablando.


--------------------------------------
# THIS EN DIFERENTES CONTEXTOS

1️⃣ Lexical Binding — Arrow Functions
📌 Las funciones flecha NO tienen su propio this
Heredan el this del contexto donde fueron creadas.
````
const obj = {
  nombre: "Ana",
  saludar() {
    setTimeout(() => {
      console.log(this.nombre);
    }, 500);
  }
};

obj.saludar();
````
🔹 this → obj
🔹 Funciona porque la flecha hereda el this de saludar
✅ Usar cuando quieres conservar el this exterior


2️⃣ New Binding — Instanciar objetos (new)
Cuando usas new, this apunta al nuevo objeto creado.
````
function Persona(nombre) {
  this.nombre = nombre;
}

const p1 = new Persona("Carlos");
console.log(p1.nombre);
````
🔹 this → el nuevo objeto p1
📌 new tiene alta prioridad


3️⃣ Explicit Binding — call / apply / bind
Aquí tú decides qué será this.
````
function saludar() {
  console.log(this.nombre);
}

const usuario = { nombre: "Laura" };

saludar.call(usuario);
````
🔹 this → usuario
call(this, a, b)
apply(this, [a, b])
bind(this) → devuelve una nueva función
📌 Es la forma más explícita y controlada


4️⃣ Implicit Binding — Invocación como método
Si una función se llama desde un objeto, this es ese objeto.
````
const mascota = {
  nombre: "Max",
  hablar() {
    console.log(this.nombre);
  }
};

mascota.hablar();
````
🔹 this → mascota
📌 Es el caso más común


5️⃣ Default Binding — Invocación directa
Cuando usamos this fuera de cualquier objeto o clase, JavaScript entiende que estamos en el contexto global, por eso apunta al objeto window.
````
function mostrar() {
  console.log(this);
}

mostrar();
````
🔹 En navegador → window
🔹 En modo estricto → undefined
📌 Es la razón de muchos errores con this


🥇 Orden de prioridad (muy importante)
Si varias reglas aplican, gana la de mayor prioridad:

New Binding
Explicit Binding (call, apply, bind)
Implicit Binding
Default Binding
Arrow Functions → no compiten, heredan


🧩 1️⃣ Diagrama mental del this
Piensa en this como una flecha que apunta a algo según cómo llamas la función:
````
¿Se usó new?
   └── sí → this = nuevo objeto 🆕
   └── no ↓

¿Se usó call / apply / bind?
   └── sí → this = objeto que tú indicas 👉
   └── no ↓

¿La función se llamó desde un objeto?
   └── sí → this = ese objeto 📦
   └── no ↓

¿Es arrow function?
   └── sí → hereda this del contexto padre 🧬
   └── no → this = window / undefined 🌍
````


----------------------------------------
# ➡️ Arrow Functions y this
Idea clave: Las arrow functions NO crean su propio this.
Heredan el this del contexto donde fueron creadas.

Ejemplo que falla
````
const mascota = {
  nombre: "Thor",
  mostrar() {
    setTimeout(function () {
      console.log(this.nombre);
    }, 1000);
  }
};

mascota.mostrar();
````
Resultado
-> undefined
La función normal dentro de setTimeout pierde el contexto del objeto.

Ejemplo corregido (arrow function)
````
const mascota = {
  nombre: "Thor",
  mostrar() {
    setTimeout(() => {
      console.log(this.nombre);
    }, 1000);
  }
};

mascota.mostrar();
````
Resultado
-> Thor

----------------------------------------
# 🔗 bind
¿Qué hace?
-Crea una nueva función
-Fija el valor de this
-NO ejecuta la función inmediatamente
⚠️ ¿Qué pasa si reutilizamos una función sin controlar this?
Si intentamos sacar un método del objeto y usarlo aparte, el valor de this puede cambiar o perderse.
Esto puede provocar que this.nombre o this.rol se vuelvan undefined.

````const usuario1 = {
    nombre: "Ana",
    rol: "administradora",
    presentarse () {
        console.log (`hola, soy ${this.nombre} y soy ${this.rol}`);
    }
};

const usuario2 = {
    nombre : "carlos",
    rol : "editor"
};

const usuario3 = {
    nombre: "guillermo",
    rol: "voluntario"
}

//Primer llamado del primer usario el cual tiene la funcion original

console.log()
usuario1.presentarse(); 

//ahora hacemos el ejercicio, de hacer un llamado de la funcion sin el bind que ancle al this, para verificar el error

console.log()
const presentarsesinBind = usuario1.presentarse;
presentarsesinBind(); 

//ahora procederemos a crear el ejemplo del bind pero ahora si aclando this a la funcion para permitirnos usarlo fuera del objeto original 

console.log()
const presentarsecarlos = usuario1.presentarse.bind(usuario2)
presentarsecarlos();

console.log()
const presentarseguillermo = usuario1.presentarse.bind(usuario3); 
presentarseguillermo();```` 
----------------------------------------
# ☎️ call

**Introducción**

En JavaScript, this hace referencia al objeto que está usando una función en el momento en que se ejecuta.
El problema es que a veces this no apunta al objeto que esperamos, y para eso existen métodos como call, apply y bind.

**¿Qué es call?**

call es un método que nos permite ejecutar una función indicando manualmente cuál será el valor de this.

Es decir, con call nosotros decidimos quién será this.

**Ejemplo simple**

Si tengo una función normal y un objeto, puedo usar call para que esa función use los datos del objeto, aunque no pertenezca a él.
```javascript
// Función normal
function saludar() {
  console.log("Hola, soy " + this.nombre);
}

// Objeto
const persona = {
  nombre: "Laura"
};

// Llamada SIN usar call
saludar(); 
// Resultado: Hola, soy undefined

// Llamada USANDO call
saludar.call(persona); 
// Resultado: Hola, soy Laura
```

**Ejemplo visual paso a paso**

Paso 1️⃣ Función normal
```javascript
function saludar() {
  console.log("Hola, soy " + this.nombre);
}
```

📌 Esta función usa this, pero no sabe quién es this todavía.

Paso 2️⃣ Creamos un objeto
```javascript
const persona = {
  nombre: "Laura"
};
```

📌 Tenemos un objeto con la propiedad nombre.

Paso 3️⃣ Ejecutamos la función SIN call
```javascript
saludar();
```

🧠 **¿Qué pasa?**

this NO apunta a persona
this.nombre no existe

❌ Resultado:
```javascript
Hola, soy undefined
```
Paso 4️⃣ Ejecutamos la función CON call
```javascript
saludar.call(persona);
```

🧠 **¿Qué está pasando ahora?**

call dice: 👉 “Oye función, usa persona como this”

✅ Resultado:
```javascript
Hola, soy Laura
```
**call con parámetros**
```javascript
function presentar(edad) {
  console.log(
    "Hola, soy " + this.nombre + " y tengo " + edad + " años"
  );
}

presentar.call(persona, 20);
```

✅ Resultado:
```javascript
Hola, soy Laura y tengo 20 años
```
call ejecuta una función y permite definir manualmente el valor de this.

----------------------------------------
# 📦 apply
El método `.apply()` es una herramienta fundamental en JavaScript para manejar el contexto de las funciones (el valor de `this`). Aunque con la llegada de ES6 (funciones de flecha y el operador *spread*) se usa menos, sigue siendo esencial para entender cómo funciona el lenguaje.

---

## ¿Qué es `.apply()`?

Es un método que puedes llamar sobre cualquier función. Su objetivo es ejecutar esa función permitiéndote dos cosas:

1. **Definir manualmente el valor de `this**` (el contexto).
2. **Pasar los argumentos** de la función como un **arreglo** (array).

### Sintaxis

```javascript
funcion.apply(contexto_this, [argumento1, argumento2, ...])
```
---

## Diferencia entre `apply()`, `call()` y `bind()`

Es común confundirlos, así que aquí tienes la clave para diferenciarlos:

| Método | Cómo pasa los argumentos | Ejecución |
| --- | --- | --- |
| **`call()`** | Uno por uno (comas): `f.call(obj, a, b)` | Inmediata |
| **`apply()`** | Como un **Arreglo**: `f.apply(obj, [a, b])` | Inmediata |
| **`bind()`** | Uno por uno (comas) | Devuelve una función nueva |

> **Truco de memoria:** **A**pply usa **A**rray. **C**all usa **C**omas.

---

## Ejemplo Práctico: Cambiando el contexto

Imagina que tienes un objeto "Persona" y una función suelta que saluda. Queremos que la función reconozca el nombre de la persona.

```javascript
const persona = { nombre: "Carlos" };

function saludar(saludo, puntuacion) {
  console.log(`${saludo}, mi nombre es ${this.nombre}${puntuacion}`);
}

// Usamos apply:
// 1. Pasamos 'persona' para que 'this.nombre' funcione.
// 2. Pasamos los argumentos en un array.
saludar.apply(persona, ["Hola", "!"]); 
// Resultado: "Hola, mi nombre es Carlos!"

```

---

## Casos de Uso Comunes

### 1. Encontrar el máximo en un array

Antes, `Math.max()` no aceptaba arrays, solo números sueltos. Con `apply` podíamos "engañarlo":

```javascript
const numeros = [5, 10, 2, 8];
const maximo = Math.max.apply(null, numeros); 
console.log(maximo); // 10

```

*(Nota: Hoy en día es más común usar el spread operator: `Math.max(...numeros)`).*

### Comparativa: Spread Operator vs. .apply()

| Característica | **Apply** (`.apply()`) | **Spread Operator** (`...`) |
| --- | --- | --- |
| **Sintaxis** | `func.apply(contexto, [args])` | `func(...args)` |
| **Tipo de herramienta** | Método del prototipo de Función. | Operador sintáctico de ES6. |
| **Manejo de `this**` | **Sí.** Permite definir qué será `this`. | **No.** Mantiene el contexto original. |
| **Legibilidad** | Más verbosa y compleja. | Mucho más limpia y natural. |
| **Uso en Arrays** | Limitado a llamadas de funciones. | Versátil (copiar, combinar, llamadas). |
| **Rendimiento** | Ligeramente más lento en motores modernos. | Optimizado nativamente por el motor JS. |
| **Uso con `new**` | Difícil de usar con constructores. | Funciona perfecto: `new MiClase(...args)`. |

---

### Ejemplos Visuales de Diferencia

Para entenderlo mejor, veamos cómo resolvemos el mismo problema con ambos:

#### Llamar a una función con un array

```javascript
const numeros = [10, 20, 30];

// Usando APPLY (Debes pasar un contexto, usualmente null o undefined)
Math.max.apply(null, numeros);

// Usando SPREAD (Más directo)
Math.max(...numeros);

```

### ¿Cuándo usar cuál?

* **Usa Spread (`...`) el 95% de las veces.** Es el estándar moderno, es más fácil de leer y evita errores con el valor de `this`.
* **Usa `.apply()` solo si necesitas cambiar explícitamente el contexto de `this**` en una función tradicional (no *arrow function*) y ya tienes los argumentos en un array.

### 2. Unir arrays (Array Prototype)

Puedes usarlo para empujar todos los elementos de un array dentro de otro de un solo golpe:

```javascript
let lista1 = [1, 2, 3];
let lista2 = [4, 5, 6];

lista1.push.apply(lista1, lista2);
console.log(lista1); // [1, 2, 3, 4, 5, 6]

```


----------------------------------------
🎮 Mini reto para el grupo
Pregunta
````
const usuario = {
  nombre: "Ana",
  mostrar() {
    setTimeout(function () {
      console.log(this.nombre);
    }, 1000);
  }
};

usuario.mostrar();
````
❓ ¿Qué imprime?

✅ Respuesta:
-> undefined

👉 Luego mostrar la solución con arrow function.
````
console.log("Global:", this);

const persona = {
  nombre: "Natalia",
  saludar() {
    console.log("Objeto:", this.nombre);
  }
};
persona.saludar();

function normal() {
  console.log("Función normal:", this);
}
normal();

const arrow = () => {
  console.log("Arrow:", this);
};
arrow();

function decirNombre() {
  console.log(this.nombre);
}
const user = { nombre: "Thor" };
decirNombre.call(user);
````

✅ Conclusión
this no es mágico.
Su valor depende siempre de quién llama a la función.
