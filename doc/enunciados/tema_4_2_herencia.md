<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
La herencia es como el ADN en programación: permite que una clase "hija" herede las características y habilidades de una clase "padre".

La relación "A es-un B"
Decimos que "A es-un B" cuando la clase hija es una versión más específica de la clase padre. Por ejemplo: Un Artillero "es un" Soldado. Esto significa que todo lo que un Soldado sabe hacer, el Artillero también lo sabe hacer.

Las dos implicaciones principales
Herencia de estado y comportamiento: El hijo no tiene que volver a inventar la rueda. Hereda las variables (estado) y las funciones (comportamiento) del padre. Si el padre tiene un nombre y sabe saludar, el hijo automáticamente tiene nombre y sabe saludar.

Compatibilidad de tipos (Polimorfismo): Como un Artillero "es un" Soldado, puedes tratarlo como si fuera un Soldado cualquiera. Esto te permite guardar distintos tipos de soldados en una misma lista.

Ejemplo en Java
Aquí tienes el código simplificado para entender cómo funciona esto en la práctica:

Java
// Clase Padre
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("¡A la orden! Soy el soldado " + nombre);
    }
}

// Clase Hija 1
class Artillero extends Soldado {
    private int cohetes = 5;

    public Artillero(String nombre) {
        super(nombre); // Llama al constructor del padre
    }

    public int getCohetes() { return cohetes; }
    public void dispararCohete() { System.out.println("¡Boom! Cohete disparado."); }
}

// Clase Hija 2
class Zapador extends Soldado {
    private int minas = 10;

    public Zapador(String nombre) {
        super(nombre);
    }

    public int getMinas() { return minas; }
    public void ponerMina() { System.out.println("Mina colocada con cuidado."); }
}

// --- PRUEBA DE COMPATIBILIDAD DE TIPOS ---
public class Main {
    public static void main(String[] args) {
        // Creamos un array de tipo "Soldado" (el tipo general)
        Soldado[] peloton = {
            new Artillero("Ramírez"),
            new Zapador("Pérez"),
            new Artillero("García")
        };

        // Recorremos el array. Todos saludan porque todos "son soldados"
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
1. ¿Cuántos constructores se ejecutan y en qué orden?
Se ejecutan dos constructores (en el caso de nuestro ejemplo): el de la clase padre (Soldado) y el de la clase hija (Artillero o Zapador).

El orden siempre es de arriba hacia abajo (del padre al hijo):

Primero se ejecuta el constructor de la clase base (padre).

Luego se ejecuta el constructor de la clase derivada (hija).

Piénsalo así: No puede existir un "Artillero" si primero no se han establecido los cimientos de lo que le hace ser un "Soldado".

2. ¿Qué significa super dentro de un constructor?
La palabra super es como un teléfono directo con el padre. Dentro de un constructor, super(...) significa: "Oye, ejecuta el constructor de mi padre con estos datos".

En el ejemplo anterior:

Java
public Artillero(String nombre) {
    super(nombre); // <--- Aquí le estamos pasando el nombre al papá (Soldado)
    this.cohetes = 5;
}
Como el atributo nombre es privado en la clase Soldado, el Artillero no puede tocarlo directamente. La única forma de asignarle un nombre al nacer es pidiéndoselo amablemente al padre a través de super.

3. Si el padre no tiene constructor sin parámetros, ¿debo llamar a super siempre?
Sí, es obligatorio.

Java, por defecto, intenta poner un super(); (invisible) al principio de cada constructor hijo. Pero hay una regla:

Si el padre tiene un constructor vacío: Java lo llama solo y no tienes que escribir nada.

Si el padre SOLO tiene constructores con parámetros (como nuestro Soldado(String nombre)): Java se "pierde" y no sabe qué datos enviar. Entonces, tú estás obligado a escribir super(...) manualmente y pasarle los datos que el padre necesita.

En resumen: Si tu clase Soldado no puede nacer "vacía", cualquier hijo suyo está obligado a llamar a super en la primera línea de su constructor para darle al padre lo que necesita para existir.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
1. ¿Están los atributos privados en la memoria del hijo?
Sí, absolutamente. Cuando creas un Artillero, en la memoria RAM se reserva un espacio para todo lo que ese objeto necesita. Esto incluye:

Lo que define el padre (el atributo nombre).

Lo que define el hijo (el atributo cohetes).

Aunque el atributo sea private, forma parte de la instancia. Si no estuviera ahí, el método saludar() (que el hijo heredó) no sabría qué nombre imprimir. El objeto es un "todo" compacto.

2. ¿Significa eso que puedo usarlos desde el código del hijo?
No directamente.

Aquí es donde entra la diferencia entre existencia y visibilidad:

Existencia: El atributo nombre existe dentro del objeto Artillero.

Visibilidad: Como está marcado como private, el código dentro de la clase Artillero no tiene permiso para tocarlo directamente.

Ejemplo práctico:
Imagina que dentro de la clase Artillero intentas hacer esto:

Java
class Artillero extends Soldado {
    // ...
    public void cambiarNombre(String nuevoNombre) {
        this.nombre = nuevoNombre; // ¡ERROR DE COMPILACIÓN!
    }
}
¿Por qué da error si el nombre está en la memoria?
Porque private significa: "Solo los métodos que estén escritos dentro de la clase Soldado pueden tocar esto". Es una medida de seguridad (encapsulamiento).

¿Cómo se soluciona?
Si quieres que el hijo pueda usarlo, tienes dos opciones:

Cambiar private por protected: Esto es como decir "Privado para el mundo, pero abierto para mi familia (hijos)".

Usar métodos públicos del padre: El hijo puede llamar a un método getNombre() que sea público en el padre.

En resumen: El Artillero lleva el nombre "en la mochila" (memoria), pero la mochila está cerrada con llave y solo el Soldado tiene la llave para abrirla.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
Esta es la verdadera "magia" de la orientación a objetos. La extensibilidad es la capacidad de tu programa para crecer y añadir nuevas funciones sin tener que romper o reescribir lo que ya funciona.

1. ¿Qué implica la compatibilidad de tipos?
Implica que tu código es "a prueba de futuro". Al programar pensando en la clase general (Soldado) en lugar de en las clases específicas (Artillero, Zapador), creas un sistema flexible.

Si mañana el ejército decide contratar francotiradores, médicos o ingenieros, no tienes que cambiar el código que gestiona el pelotón. El sistema simplemente los acepta porque todos cumplen con la "interfaz" de ser un Soldado.

2. Ilustración: Añadimos un Francotirador
Vamos a crear un nuevo tipo de soldado. Fíjate que es una clase totalmente nueva, pero no tocaremos ni una sola línea del código que recorre el array y pide los saludos.

Java
// --- NUEVO TIPO DE SOLDADO (Añadido después) ---
class Francotirador extends Soldado {
    private int precision;

    public Francotirador(String nombre, int precision) {
        super(nombre);
        this.precision = precision;
    }

    public void apuntar() {
        System.out.println("Objetivo en la mira...");
    }
}

// --- EL CÓDIGO DE GESTIÓN NO CAMBIA ---
public class Cuartel {
    public static void main(String[] args) {
        // El array sigue siendo de tipo Soldado
        Soldado[] peloton = {
            new Artillero("Ramírez"),
            new Zapador("Pérez"),
            new Francotirador("López", 95) // <--- ¡Funciona automáticamente!
        };

        // Este bucle es "ciego" a los tipos específicos.
        // Solo sabe que son Soldados y que los Soldados saludan.
        for (Soldado s : peloton) {
            s.saludar(); // <--- CERO MODIFICACIONES AQUÍ
        }
    }
}
3. Conclusión
Antes de la herencia: Si querías añadir un nuevo tipo, tenías que crear un array nuevo, un bucle nuevo y mucha lógica repetida.

Con herencia y compatibilidad de tipos: El código que ya tenías escrito para Soldado es reutilizable para cualquier cosa que "sea un" Soldado en el futuro.

Esto permite que un sistema sea abierto para la extensión (puedes añadir clases) pero cerrado para la modificación (no tienes que tocar el código que ya funciona).

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
¿Referencia de supertipo a objeto de subtipo? Sí. Puedes decir Soldado s = new Artillero();. El "mando a distancia" es de Soldado, pero el "aparato" es un Artillero.

¿Invocar métodos del subtipo? No directamente. Si el mando es de Soldado, solo ves los botones de Soldado. No puedes ver el botón "dispararCohete" aunque el objeto real sea un Artillero.

Upcasting: Es subir en la jerarquía (de Artillero a Soldado). Es automático y seguro.

Downcasting: Es bajar en la jerarquía (de Soldado a Artillero). Hay que hacerlo manual: (Artillero) s. Es arriesgado porque si el soldado no era realmente un artillero, el programa explota.

instanceof: Es un operador para preguntar: "¿Tú eres de este tipo?". Devuelve true o false.

Java
for (Soldado s : peloton) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Cohetes: " + a.getCohetes());
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso protected es el punto medio: el atributo no es público (para todo el mundo), pero tampoco es privado (solo para la clase padre). Es visible para la clase padre y para todas sus clases hijas.

En Java se implementa con la palabra clave protected.

Java
class Soldado {
    protected String nombre; // Las hijas ahora pueden "leerlo" directamente
}

class Zapador extends Soldado {
    public void ponerMina() {
        // Ahora puedo usar 'nombre' directamente sin getters
        System.out.println(nombre + " está colocando una mina...");
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
¿Hay una clase base para todos? En muchos lenguajes modernos, sí.

¿En todos los lenguajes? No. En C++, por ejemplo, puedes tener clases que no hereden de nada.

En Java: Sí. Todas las clases, si no especificas nada, heredan automáticamente de la clase Object. Por eso todos los objetos en Java tienen métodos como toString() o equals().

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es cuando una clase hija intenta heredar de dos o más padres a la vez (ej. un SoldadoAnfibio que hereda de SoldadoTierra y SoldadoMar).

En Java: No existe la herencia múltiple de clases. Java lo prohíbe para evitar líos (como el "Problema del Diamante", donde no se sabe qué método heredar si ambos padres tienen uno igual). Java usa Interfaces para solucionar esto.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
Las excepciones son objetos. Si heredan de RuntimeException, son no controladas (no obligan a usar try-catch).

Java
class UsuarioNoEncontradoException extends RuntimeException {
    private String usuario;

    // Constructor normal
    public UsuarioNoEncontradoException(String mensaje, String usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Sobrecarga para incluir la causa (Throwable cause)
    public UsuarioNoEncontradoException(String mensaje, String usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }
}

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
La herencia es una herramienta muy potente, pero rígida. Si heredas de una clase solo porque te gusta un método que tiene, estás obligando a tu nueva clase a ser "hija" de la otra para siempre.

El problema del "paquete completo": Al heredar, te llevas todo (métodos que no quieres, variables que no usas y posibles fallos).

Acoplamiento fuerte: Si la clase padre cambia algo en el futuro, es muy probable que tu clase hija deje de funcionar. Estás creando una dependencia total.

Regla de oro: Solo usa herencia si realmente puedes decir que "A es un B". Si solo quieres usar una herramienta de B, mejor usa composición.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Esta frase significa que es mejor construir objetos uniedo piezas que heredando genes.

Composición (Relación "Tiene-un"): Un Coche tiene un Motor. Si el motor se rompe o quiero cambiarlo por uno eléctrico, simplemente cambio la pieza. Es flexible y ocurre en "tiempo de ejecución".

Herencia (Relación "Es-un"): Un Coche Eléctrico es un Coche. Esto se decide al escribir el código y no se puede cambiar mientras el programa corre. Es mucho más difícil de modificar si los requisitos del proyecto cambian.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
La encapsulación dice que los detalles internos de una clase deben estar ocultos. Sin embargo, la herencia crea una relación tan íntima que el hijo suele depender de cómo está implementado el padre.

Dependencia de la implementación: Si el padre cambia un método interno que el hijo usaba (o sobreescribía), el hijo puede empezar a comportarse de forma errática sin que hayas tocado su código.

Exposición innecesaria: A veces, al heredar, expones a través del hijo métodos del padre que deberían haber permanecido ocultos para el resto del programa.

En resumen: el hijo "sabe demasiado" sobre las tripas del padre, y cualquier cambio en las tripas del padre afecta al hijo.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Aquí comparamos cómo modelaríamos a un Estudiante y un Trabajador que comparten nombre y DNI.

Opción A: Por Herencia (Relación "Es-un")
Aquí, tanto Estudiante como Trabajador nacen de Persona.

Java
class Persona {
    String nombre;
    String dni;
}

class Estudiante extends Persona {
    int legajo;
}

class Trabajador extends Persona {
    double sueldo;
}
Opción B: Por Composición (Relación "Tiene-un")
Aquí, DatosPersonales es una pieza que se le entrega al objeto al construirlo.

Java
class DatosPersonales {
    String nombre;
    String dni;

    public DatosPersonales(String nombre, String dni) {
        this.nombre = nombre;
        this.dni = dni;
    }
}

class Estudiante {
    private DatosPersonales datos; // El estudiante TIENE datos
    int legajo;

    public Estudiante(DatosPersonales datos, int legajo) {
        this.datos = datos;
        this.legajo = legajo;
    }
}

class Trabajador {
    private DatosPersonales datos; // El trabajador TIENE datos
    double sueldo;

    public Trabajador(DatosPersonales datos, double sueldo) {
        this.datos = datos;
        this.sueldo = sueldo;
    }
}
¿Cuál es la gran diferencia?
En la Composición (B), si una persona deja de estudiar y empieza a trabajar, podrías pasarle su objeto DatosPersonales al nuevo objeto Trabajador. En la Herencia (A), el estudiante y el trabajador son objetos totalmente distintos y rígidos; no puedes "convertir" a uno en otro fácilmente.
