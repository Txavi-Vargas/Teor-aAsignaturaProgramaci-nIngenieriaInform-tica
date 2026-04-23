<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
Antes de que existieran los "Generics", usábamos el tipo más básico posible para guardar cualquier cosa.

En C (void*): Es un puntero que no sabe a qué apunta. Puede apuntar a un int, un char o una estructura.

En Java (Object): Como todo hereda de Object, un array de Object puede guardar cualquier cosa.

Ejemplo en Java:

Java
public class Bolsa {
    private Object[] elementos = new Object[10];
    private int contador = 0;

    public void meter(Object algo) { elementos[contador++] = algo; }
    public Object sacar(int i) { return elementos[i]; }
}
Aquí puedes meter un String y luego un Integer. La bolsa no se queja, pero es "peligrosa" (luego veremos por qué).
## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
Es un estilo de programación donde el tipo de dato es un parámetro. En lugar de decir "esta función suma enteros", dices "esta función trabaja con el tipo T", y ya decidiré qué es T más adelante.

¿Es el ejemplo anterior genericidad? Es un intento básico (polimorfismo de tipos), pero no es "programación genérica" pura en el sentido moderno, porque no hay chequeo de tipos en la compilación. Es simplemente usar un "contenedor gigante" donde cabe todo.
## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
Si usamos Object o void*, perdemos la seguridad:

Casting manual: Al sacar algo de la bolsa de Object, tienes que decirle a Java: (String) bolsa.sacar(0). Si te equivocas y era un número, el programa explota en ejecución.

Falta de control: El compilador no puede ayudarte. Te deja meter un "Gato" en una lista que tú pensabas usar solo para "Perros". El error solo se ve cuando el programa ya está funcionando y falla.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Si usamos Object o void*, perdemos la seguridad:

Casting manual: Al sacar algo de la bolsa de Object, tienes que decirle a Java: (String) bolsa.sacar(0). Si te equivocas y era un número, el programa explota en ejecución.

Falta de control: El compilador no puede ayudarte. Te deja meter un "Gato" en una lista que tú pensabas usar solo para "Perros". El error solo se ve cuando el programa ya está funcionando y falla.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
Ambos lenguajes permiten crear una lista que solo acepte String.

En Java (Generics):

Java
List<String> lista = new ArrayList<>();
lista.add("Hola");
// lista.add(10); // ¡ERROR en compilación! (Seguridad)
String texto = lista.get(0); // No hace falta casting
En C++ (Templates):

C++
std::vector<std::string> lista;
lista.push_back("Hola");
std::string texto = lista[0]; // Seguridad total

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Aquí los dos lenguajes toman caminos totalmente distintos:

Java (Type Erasure): El compilador usa la genericidad para comprobar que no metas la pata, pero una vez que comprueba que todo es correcto, borra los tipos genéricos y los sustituye por Object (o el tipo base). Solo existe una versión de la clase en el archivo .class.

C++ (Instanciación de plantillas): El compilador genera un código fuente distinto para cada tipo. Si usas vector<int> y vector<string>, C++ escribe dos clases diferentes por ti. Es más rápido en ejecución pero el archivo final es más grande.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
Esta clase permite guardar dos cosas de distinto tipo:

Java
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) { this.primero = primero; this.segundo = segundo; }
    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

// Ejemplo: Devolver media (Double) y desviación (Double)
public Par<Double, Double> calcularEstadisticas(double[] datos) {
    // ... cálculos ...
    return new Par<>(10.5, 2.1);
}

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
Fíjate en la diferencia de seguridad:

Con Object: Object seleccionaUno(Object a, Object b). Si le pasas un Gato y un Perro, te devuelve un Object. No sabes qué es y tienes que arriesgarte con un casting.

Con Generics: <T> T seleccionaUno(T a, T b). Si le pasas dos Soldado, el compilador garantiza que lo que sale es un Soldado. Además, si intentas pasar un Soldado y un Coche, el compilador te da un error porque no son del mismo tipo T.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Podemos obligar a que T sea "de la familia" de los números.

Solución con Generics (Punto):

Java
public class Punto<T extends Number> {
    private T x, y;
    public Punto(T x, T y) { this.x = x; this.y = y; }

    public double distanciaA(Punto<T> otro) {
        return Math.sqrt(Math.pow(x.doubleValue() - otro.x.doubleValue(), 2) + 
                         Math.pow(y.doubleValue() - otro.y.doubleValue(), 2));
    }
}
Type Erasure: Tras compilar, T se convierte en Number.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Sin Generics (Number): Podrías crear un Punto con una X que es Integer y una Y que es Double. Es muy flexible, pero getX() siempre devuelve el tipo genérico Number.

Con Generics (<T extends Number>): Obligas a que ambas coordenadas sean del mismo tipo T. Si el punto es de Integer, tanto X como Y tienen que ser Integer. getX() te devolverá exactamente un Integer, sin necesidad de castings.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta
Para evitar el instanceof, hacemos que la interfaz sea genérica sobre sí misma:

Java
public interface Punto<T> {
    double distanciaA(T otro);
}

public class Punto2D implements Punto<Punto2D> {
    private double x, y;
    @Override
    public double distanciaA(Punto2D p2d) { // ¡Ya no necesito casting!
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2));
    }
}

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
Aquí Java se comporta de forma curiosa:

Arrays (Covariantes): String[] es subtipo de Object[]. Java lo permite, pero es peligroso. Si intentas meter un Integer en un String[] usando una referencia de Object[], el programa explota en ejecución con un ArrayStoreException.

Listas (Invariantes): List<String> NO es subtipo de List<Object>. Java lo prohíbe en compilación para evitar el error anterior. Son tipos totalmente hermanos, no padre e hijo.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Los comodines sirven para relajar la invarianza de las listas de forma segura:

? extends T (Lectura): Acepta T o cualquier hijo. Útil para sacar datos (sumar números).

? super T (Escritura): Acepta T o cualquier padre. Útil para meter datos en una lista.

Ejemplos:

// (i) Sumar: acepta lista de Integer, Double, etc.
public double sumar(List<? extends Number> lista) {
    double s = 0;
    for (Number n : lista) s += n.doubleValue();
    return s;
}

// (ii) Añadir: acepta lista de Number o de Object para meter un Integer
public void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
}