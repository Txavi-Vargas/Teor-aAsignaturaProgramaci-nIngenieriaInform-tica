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

<!--
TEMA 1. Clases y objetos

1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Encapsulación: La encapsulación es la idea de que un objeto guarda sus datos por dentro y tú solo puedes interactuar con ellos a través de una interfaz controlada. (normalmente, métodos como getters y setters).
Abstracción: Modelar únicamente las características esenciales de una entidad y exponer una interfaz de uso (métodos) ocultando los detalles de implementación.
Herencia: Herencia permite crear clases especializadas a partir de una clase más general, reutilizando código y facilitando la extensión mediante extends y el override.
Polimorfismo: Diferentes clases relacionadas (por herencia/interfaz) pueden implementar el mismo método y, al invocarlo mediante una referencia común, se ejecuta la versión correspondiente al tipo real del objeto.
Composición: Construir un objeto usando otros objetos dentro, en vez de heredar de una clase base.


2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Dinámicos: Python, JavaScript. Compilados: C++, Java, C#, ¿Rust?.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Ensamblador -> Estructurada o procedural o imperativa -> Modular.

Ensamblador: Sólo tenemos la secuencia y un salto arbitrario(GOTO)(puedes poner que salte tu código a cualquier línea posterior o anterior).

Estructurada: Nos da instrucciones de control modernas (condicional (if)) y (iterativas(until, for, do while)), sin GOTO. (Mejora calidad).

Modular: Organiza el software en módulos reutilizables con interfaces, mejora mantenimiento y escalabilidad.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Identidad: Es la dirección de memoria.

Estado: Valor de los atributos que tenga un objeto en un determinado momento, (lo que en struct eran campos).

Comportamiento: Métodos, son las funciones que todos los atributos de esa clase pueden hacer.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Clase: “Molde/plantilla” que define estado (atributos) y comportamiento (métodos).

Objeto: Son las variables de tipo de (alguna clase) con un estado concreto de sus atributos. (La entidad concreta).

Instancia: Objeto de una clase. En Java, “objeto” e “instancia” se usan casi como sinónimos. Una instancia es justamente un objeto que existe en un momento de la ejecución.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En Java, los objetos (lo que creas con new) se guardan en el heap. En el stack van las variables locales y las referencias a esos objetos.
No es igual en todos los lenguajes: en C/C++ puedes tener objetos en stack o heap y normalmente se libera memoria a mano.
Recolección de basura (GC): sistema automático que detecta objetos ya inaccesibles (sin referencias) y libera su memoria.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Método: función definida dentro de una clase que describe una operación/comportamiento; puede actuar sobre el estado del objeto (sus atributos).

Sobrecarga (overloading): varios métodos con el mismo nombre en la misma clase, pero con firma distinta (distinto número, tipo u orden de parámetros). Se decide en compilación.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println("Distancia al origen: " + p.calculaDistanciaAOrigen());
    }
}

class Punto {
    // visibilidad por defecto (package-private)
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada es el método:
public static void main(String[] args) ya que es el primer método que la Java Virtual Machine busca y ejecuta cuando arrancas un programa en Java.
¿Qué es static y para qué vale?

static significa que pertenece a la clase, no a un objeto (no necesitas new para usarlo).
Se usa para:
1.Métodos “de utilidad”.
2.Atributos compartidos por todas las instancias.

¿Solo se usa en main?
No. Se usa en atributos, métodos, bloques estáticos y clases internas.

¿Para qué se combina con final?
final significa no modificable.
static final se usa para constantes de clase (un valor único para todos y que no cambia).


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar se usa javac sobre el .java, y se genera un fichero .class. Para ejecutar, se usa java indicando el nombre de la clase sin el .class.

Java es compilado pero no a código máquina directamente: se compila a bytecode, que es un "código intermedio" pensado para ejecutarse en cualquier sistema.

La máquina virtual (JVM) es el programa que corre en tu ordenador y que ejecuta ese bytecode(lo interpreta y muchas veces lo transforma en código máquina mientras lo ejecuta).

El bytecode es lo que va dentro de los .class: el resultado de compilar el .java y lo que entiende la JVM.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

new crea un objeto (reserva memoria) y llama a su constructor.

Constructor: método especial sin tipo de retorno y con el mismo nombre que la clase, para inicializar el objeto.

La clase Empleado tendría atributos DNI, nombre y apellidos, y un constructor que reciba esos valores y los asigne al objeto.

Ejemplo:

class Empleado {
    private String dni;
    private String nombre;
    private String apellidos;

    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

this es la referencia al objeto actual (la instancia) y se usa para acceder a sus atributos y métodos, especialmente para diferenciar atributos del objeto (this.x) de parámetros o variables locales (x).

No se llama igual en todos los lenguajes (por ejemplo, en Python suele ser self).

En Punto se usa para diferenciar atributos del objeto frente a parámetros o variables locales con el mismo nombre.

class Punto {
    private int x;
    private int y;

    public Punto(int x, int y) {
        this.x = x;   // this.x es el atributo, x es el parámetro
        this.y = y;
    }
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado.

public double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java el paso de parámetros es por valor siempre: en objetos se copia la referencia, por lo que modificar atributos dentro del método afecta al mismo objeto fuera; sin embargo, reasignar la referencia dentro del método no cambia la referencia externa. Si se quiere evitar modificar el objeto original, se trabaja con una copia.

Para primitivos(int, float, double y etc), se copia el valor numérico: modificarlo dentro del método no afecta al valor fuera.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

toString() devuelve una representación en texto del objeto y puede sobrescribirse para mostrar información útil(ya hay una básica, pero nosotros la reescribimos para personalizarla y mostrar información útil).

Pones @Override para indicar que estás sobrescribiendo un método que ya existe en la superclase (Object), como toString().

Existe concepto equivalente en muchos lenguajes (métodos especiales de conversión a cadena).

Ejemplo:

class Punto {
    private int x, y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Un struct en C agrupa datos, pero no integra de forma nativa comportamiento asociado (métodos), encapsulación/visibilidad y mecanismos POO (como polimorfismo).

Una clase reúne datos (atributos) y comportamiento (métodos), y permite encapsular esos datos controlando el acceso (public/private).


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Se emula definiendo un struct con x e y y una función que calcule la distancia al origen recibiendo como argumento la dirección del struct.

this no existe implícitamente: se reemplaza por un parámetro explícito que identifica sobre qué “instancia” se opera.

#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double distanciaAlOrigen(const Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

-->
