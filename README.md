📘 README
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

🧩 ¿Qué es this?
this es una palabra reservada de JavaScript.
Hace referencia al objeto que está ejecutando una función en ese momento.
Su valor no es fijo, cambia según el contexto.

Analogía
this es como decir “yo”.
El significado de “yo” cambia dependiendo de quién esté hablando.


--------------------------------------
THIS EN DIFERENTES CONTEXTOS

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
➡️ Arrow Functions y this
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
🔗 bind
¿Qué hace?
-Crea una nueva función
-Fija el valor de this
-NO ejecuta la función inmediatamente

````
function saludar() {
  console.log(this.nombre);
}

const persona = { nombre: "Natalia" };
const saludarPersona = saludar.bind(persona);

saludarPersona();
````

----------------------------------------
☎️ call
¿Qué hace?
-Ejecuta la función inmediatamente
-Permite definir el valor de this
````
saludar.call(persona);
````

----------------------------------------
📦 apply
-Diferencia con call: Recibe los argumentos en un arreglo

Aquí tienes una estructura profesional y limpia, optimizada para que la copies y la pegues directamente en el archivo `README.md` de tu repositorio de GitHub.

---

```markdown
# JavaScript .apply() 🚀

Una guía rápida sobre el funcionamiento del método `.apply()` en JavaScript, sus diferencias con otros métodos de contexto y casos de uso.

---

## 📝 Definición

El método `.apply()` llama a una función dándole un valor de `this` de forma explícita y pasando los argumentos como un **arreglo** (o un objeto similar a un arreglo).

### Sintaxis
```javascript
funcion.apply(thisArg, [argsArray])

```

* **`thisArg`**: El valor que se utilizará como `this`.
* **`argsArray`**: Un arreglo que contiene los argumentos que se le pasarán a la función.

---

## 💡 Ejemplo Básico

Es ideal cuando quieres reutilizar una función en diferentes objetos:

```javascript
function describir(hobby, edad) {
  console.log(`Soy ${this.nombre}, tengo ${edad} años y me gusta el ${hobby}.`);
}

const persona = { nombre: "Carlos" };

// Invocación con apply
describir.apply(persona, ["Ajedrez", 28]);
// Salida: Soy Carlos, tengo 28 años y me gusta el Ajedrez.

```

---

## ⚖️ Comparativa: apply vs call vs bind

A menudo se confunden, pero la clave está en **cómo pasan los argumentos** y **cuándo se ejecutan**.

| Método | Argumentos | Ejecución |
| --- | --- | --- |
| **`.apply()`** | Un **Arreglo** `[arg1, arg2]` | Inmediata |
| **`.call()`** | Separados por **Comas** `arg1, arg2` | Inmediata |
| **`.bind()`** | Separados por **Comas** | Devuelve una nueva función |

> **Tip:** Recuerda **A** de **A**pply = **A**rray.

---

## 🛠️ Casos de Uso Comunes

### 1. Encontrar el máximo/mínimo en un arreglo (Legacy)

Antes de ES6, esta era la forma estándar de pasar un array a funciones que esperan parámetros individuales:

```javascript
const numeros = [5, 2, 9, 1, 7];
const max = Math.max.apply(null, numeros); 
console.log(max); // 9

```

### 2. Encadenar constructores

Se puede usar para delegar la inicialización de un objeto a otro constructor:

```javascript
function Producto(nombre, precio) {
  this.nombre = nombre;
  this.precio = precio;
}

function Comida(nombre, precio) {
  Producto.apply(this, [nombre, precio]);
  this.categoria = 'alimento';
}

const manzana = new Comida('Manzana', 1.5);

```

---

## ⚠️ ¿Sigue siendo relevante?

Con la llegada de **ES6**, la mayoría de los casos de uso de `.apply()` ahora se resuelven con el **Spread Operator (`...`)**, que es más legible:

``` javascript
// Antes (apply)
Math.max.apply(null, [1, 2, 3]);

// Ahora (Spread operator)
Math.max(...[1, 2, 3]);

```

Sin embargo, entender `.apply()` es fundamental para comprender el manejo de contextos en JavaScript y para trabajar en proyectos que mantengan compatibilidad con versiones anteriores.


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
