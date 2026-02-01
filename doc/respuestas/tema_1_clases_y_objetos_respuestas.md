<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

<!-- TEMA 1. Clases y objetos

1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Encapsulación: agrupa datos y comportamiento y controla el acceso a los detalles internos.
Abstracción: modela lo esencial y oculta complejidad innecesaria.
Herencia: permite crear clases nuevas reutilizando y extendiendo otras existentes.
Polimorfismo: la misma operación puede comportarse de forma distinta según el tipo real del objeto.*/


2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java, C#, C++, Python.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Estructurada: organiza el programa usando secuencia, selección e iteración, evitando saltos descontrolados, y apoyándose en funciones/procedimientos y bloques.
Modular: divide el software en módulos con responsabilidades claras e interfaces definidas para facilitar reutilización y mantenimiento.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Identidad, estado (valores de atributos), comportamiento (métodos/operaciones).

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Clase: plantilla que define atributos y métodos.
Objeto: entidad concreta creada a partir de una clase.
Instancia: objeto visto como “ejemplar” de una clase.
No todos los lenguajes POO se basan estrictamente en clases; algunos son prototípicos (aunque puedan tener sintaxis tipo clase).


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

Depende del lenguaje y del entorno de ejecución: a menudo los objetos viven en el heap, pero algunos lenguajes permiten objetos en stack o gestionados de otras formas.
No es igual en todos.
Recolección de basura (garbage collection): mecanismo automático que detecta objetos ya inaccesibles y recupera su memoria.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Método: función asociada a una clase/objeto que define comportamiento.
Sobrecarga: mismo nombre de método con distintas listas de parámetros (número y/o tipos), resuelta en compilación.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

Una clase llamada Punto con dos atributos x e y (visibilidad por defecto) y un método calculaDistanciaAOrigen que devuelve la distancia desde (x,y) hasta (0,0) usando la fórmula de la distancia euclídea. Además, se crea una instancia y se llama a ese método.


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada es el método main.
static indica que pertenece a la clase, no a una instancia, y permite usarlo sin crear objetos. No se usa solo en main; también en métodos y atributos “de clase”.
static final se usa típicamente para constantes: valor fijo, compartido por toda la clase.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Se compila desde consola con el compilador de Java y se ejecuta con el lanzador de Java.
Java es compilado a bytecode, no directamente a código máquina.
La JVM (máquina virtual) ejecuta ese bytecode (interpretándolo y/o compilándolo en caliente).
Los ficheros .class contienen el bytecode resultante de la compilación.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

new crea un objeto (reserva memoria) y llama a su constructor.
Un constructor es una rutina especial de inicialización que se ejecuta al crear el objeto, normalmente para dejarlo en un estado válido.
La clase Empleado tendría atributos DNI, nombre y apellidos, y un constructor que reciba esos valores y los asigne al objeto.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

this es la referencia al objeto actual (la instancia sobre la que se invoca el método).
No se llama igual en todos los lenguajes (por ejemplo, en Python suele ser self).
En Punto se usa para diferenciar atributos del objeto frente a parámetros o variables locales con el mismo nombre.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Añadir un método distanciaA que reciba un Punto y calcule la distancia euclídea entre el punto actual (this) y el punto recibido, usando diferencias en x e y y la raíz cuadrada.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java el paso es por valor: siempre se copia el valor del argumento.
Para objetos, lo que se copia es la referencia: modificar atributos del objeto dentro del método afecta al mismo objeto fuera del método, pero reasignar la referencia dentro no cambia la referencia externa.
Para int (primitivo), se copia el valor numérico: modificarlo dentro del método no afecta al valor fuera.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

toString() devuelve una representación en texto del objeto y puede sobrescribirse para mostrar información útil.
Existe concepto equivalente en muchos lenguajes (métodos especiales de conversión a cadena)


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Un struct en C agrupa datos, pero no integra de forma nativa comportamiento asociado (métodos), encapsulación/visibilidad y mecanismos POO (como polimorfismo).
Una clase combina datos + operaciones y suele ofrecer control de acceso y otras herramientas del paradigma.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Se emula definiendo un struct con x e y y una función que calcule la distancia al origen recibiendo como argumento la dirección del struct.
this no existe implícitamente: se reemplaza por un parámetro explícito que identifica sobre qué “instancia” se opera.

-->
