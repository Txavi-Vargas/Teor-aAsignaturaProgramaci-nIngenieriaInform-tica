<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un puntero a una función es una variable que almacena la dirección de memoria donde comienza el código de una función. Permite pasar funciones como argumentos a otras funciones.

C
#include <stdio.h>
#include <ctype.h>

// Definición de la función
void aMayusculasFunc(char *cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "hola mundo";
    
    // Definición del puntero a la función
    void (*aMayusculas)(char*) = aMayusculasFunc;
    
    // Invocación mediante el puntero
    aMayusculas(texto);
    
    printf("%s", texto); // Imprime: HOLA MUNDO
    return 0;
}

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda es una función anónima (sin nombre) que se puede tratar como un valor.

JavaScript:

JavaScript
let aMayusculas = (str) => str.toUpperCase();
console.log(aMayusculas("hola"));
Java:

Java
import java.util.function.Function;
Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola"));

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
Paradigma Funcional: Se centra en el uso de funciones matemáticas, evitando el cambio de estado y los datos mutables.

Multi-paradigma: Java 8 es multi-paradigma porque permite programar de forma Orientada a Objetos y también de forma Funcional (mediante lambdas y streams).

Ciudadanos de primera clase: Significa que las funciones tienen los mismos privilegios que cualquier otra variable: se pueden asignar a variables, pasar como parámetros y ser devueltas por otras funciones.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis sigue el esquema: (parámetros) -> { cuerpo }.

Si hay un solo parámetro, los paréntesis son opcionales: s -> s.length().

Si el cuerpo tiene una sola línea, las llaves y el return son opcionales: (a, b) -> a + b.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Aquí vemos cómo una función recibe a otra para ejecutarla internamente.

Java
// Java
public static String transformar(String s, Function<String, String> f) {
    return f.apply(s);
}

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

// Invocación con lambda directa (Invertir cadena)
String resultado = transformar("Hola", s -> new StringBuilder(s).reverse().toString());

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un closure ocurre cuando una función lambda accede a una variable definida en su ámbito exterior (fuera de la propia lambda). En Java, estas variables deben ser final o "efectivamente finales" (que no cambien de valor).

Java
String prefijo = "EUREKA: ";
// La lambda captura la variable 'prefijo' del contexto exterior
Function<String, String> concatenar = s -> prefijo + s;
System.out.println(transformar("descubierto", concatenar));

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia principal es el estado. Un puntero en C solo apunta a un bloque de código estático. Una función lambda (especialmente un closure) puede "empaquetar" código junto con variables de su entorno, creando una entidad que lleva información consigo.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
public static Function<Double, Double> crearDescuento(double porcentaje) {
    // Retornamos una lambda que usa el porcentaje capturado (closure)
    return cantidad -> cantidad - (cantidad * porcentaje / 100);
}

// Uso:
Function<Double, Double> descuentoBlackFriday = crearDescuento(20.0);
System.out.println(descuentoBlackFriday.apply(100.0)); // 80.0

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
En Java, una interfaz funcional es una interfaz que contiene exactamente un método abstracto.

Requisitos: Solo debe tener un método sin implementar (abstracto).

Métodos adicionales: Puede contener cualquier número de métodos default o static sin romper su naturaleza funcional.

Anotación: Se suele marcar con @FunctionalInterface para que el compilador genere un error si accidentalmente añadimos un segundo método abstracto.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Si queremos definir nuestra propia interfaz para transformar texto, se vería así:

Java
@FunctionalInterface
interface Transformador {
    // El único método abstracto: recibe String y devuelve String
    String transformar(String cadena);
}

// Ejemplo de uso:
Transformador aMayusculas = s -> s.toUpperCase();

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Para que sea realmente útil, podemos usar Generics (<T, R>). Esto permite que la interfaz trabaje con cualquier tipo de entrada y salida, no solo cadenas.

Definición genérica:

Java
@FunctionalInterface
interface TransformadorGenerico<T, R> {
    R transformar(T entrada);
}
Ejemplo: Redondear un Double a un Integer:

Java
public class Ejemplo {
    public static void main(String[] args) {
        // T es Double, R es Integer
        TransformadorGenerico<Double, Integer> redondear = d -> (int) Math.round(d);

        Integer resultado = redondear.transformar(7.8);
        System.out.println(resultado); // Imprime: 8
    }
}


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Java proporciona un catálogo para no tener que crearlas siempre:

Function<T, R>: Recibe T, devuelve R.

Predicate<T>: Recibe T, devuelve boolean.

Consumer<T>: Recibe T, no devuelve nada.

Supplier<T>: No recibe nada, devuelve T.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
Es la alternativa funcional al bucle for tradicional.

Java
List<Integer> lista = Arrays.asList(-5, 10, 0, 20);
lista.forEach(n -> {
    if (n > 0) System.out.println("Es positivo: " + n);
});
## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
PECS significa Producer Extends, Consumer Super. Se usa ? super T en el forEach porque un consumidor que sepa manejar un tipo "padre" de T, también sabrá manejar a T. Esto da flexibilidad al código.
## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Es un atajo sintáctico cuando la lambda solo llama a un método ya existente.

Java: personaInstancia::saludar

JavaScript: const ref = persona.saludar.bind(persona);

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
Estático: Math::abs

Constructor: ArrayList::new

Instancia de objeto concreto: miObjeto::toString

Instancia de objeto arbitrario: String::length

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Podemos ordenar listas de forma muy elegante combinando lambdas y referencias.

Java
// Opción manual con Lambda
personas.sort((p1, p2) -> {
    int cmp = Integer.compare(p1.getEdad(), p2.getEdad());
    return (cmp != 0) ? cmp : p1.getNombre().compareTo(p2.getNombre());
});

// Opción elegante con Comparator
personas.sort(Comparator.comparingInt(Persona::getEdad)
                        .thenComparing(Persona::getNombre));