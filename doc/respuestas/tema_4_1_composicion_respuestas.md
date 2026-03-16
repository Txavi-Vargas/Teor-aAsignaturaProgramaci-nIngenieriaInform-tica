<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----

# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
En el lenguaje C, la composición se realiza incluyendo una estructura como miembro de otra. En este caso, se define primero una estructura Punto que agrupa dos valores de tipo real. Posteriormente, la estructura Linea se compone de dos elementos de tipo struct Punto. Esta técnica permite organizar los datos de manera jerárquica, facilitando la legibilidad y el mantenimiento del código al agrupar datos relacionados.

Para procesar estas estructuras, se definen funciones que operan sobre ellas. La función de distancia aplica la fórmula euclídea utilizando las coordenadas de dos estructuras Punto. Por su parte, la función de longitud de la línea simplemente delega el cálculo en la func#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double calcularDistancia(struct Punto a, struct Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double calcularLongitud(struct Linea l) {
    return calcularDistancia(l.p1, l.p2);
}

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
En Java, la composición se implementa mediante atributos de clase que son referencias a otros objetos. A diferencia de C, la orientación a objetos permite proteger el estado interno mediante el uso de modificadores de acceso. Al declarar los atributos como private, se impide que código externo modifique las coordenadas o los puntos de la línea directamente, obligando a interactuar a través del constructor y los métodos definidos.

Para garantizar la inmutabilidad, se utiliza la palabra reservada final en los atributos y se evita proporcionar métodos "setter". De este modo, una vez que un objeto Punto o Linea es instanciado, su estado no puede cambiar. El comportamiento (cálculo de longitud) se encapsula dentro de la clase Linea, la cual utiliza los métodos de la clase Punto para realizar su cometido, promoviendo una alta cohesión.
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double calcularLongitud() {
        return p1.calcularDistancia(p2);
    }
}

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad es un concepto que define cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase en un momento dado. Es fundamental para establecer las reglas de negocio y la estructura de los datos, indicando si una relación es de uno a uno, uno a muchos o muchos a muchos. En el diseño de sistemas, ayuda a determinar si se debe usar un atributo simple o una colección (como un array o una lista).

En el ejemplo de la línea y el punto, la multiplicidad se desglosa de la siguiente manera:

De Linea a Punto: La multiplicidad es 2 (o exactamente dos). Una instancia de Linea está compuesta obligatoriamente por dos objetos Punto que representan sus extremos.

De Punto a Linea: La multiplicidad es 0..* (de cero a muchos). Un objeto Punto puede existir de forma independiente sin pertenecer a ninguna línea, o puede ser parte de una o múltiples líneas simultáneamente.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La distinción entre composición fuerte y débil reside en el grado de acoplamiento y la dependencia del ciclo de vida entre el objeto contenedor y los objetos contenidos. En la composición fuerte, existe una relación de pertenencia absoluta: los objetos hijos no tienen sentido de existencia sin su padre. Por el contrario, en la composición débil, los objetos hijos pueden existir independientemente y ser compartidos por diferentes contenedores.

En cuanto al ciclo de vida, la consecuencia principal es que en la composición fuerte, cuando el objeto contenedor se destruye, los objetos contenidos también deben ser destruidos. En la composición débil, la destrucción del contenedor no afecta a la existencia de los objetos contenidos, los cuales permanecen en memoria mientras existan otras referencias a ellos.

En la terminología estándar de orientación a objetos, la composición débil se conoce generalmente como agregación (o asociación, en un nivel más genérico). Por otro lado, el término composición se reserva para la variante fuerte, donde la existencia de las partes está ligada indisolublemente a la existencia del todo.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
En los casos descritos, se trata de una relación de dependencia. A diferencia de la composición, donde una clase "tiene" a otra como parte de su estructura permanente (un atributo), la dependencia representa una relación de uso temporal. Se suele describir como una relación "usa-un", indicando que una clase requiere de otra para llevar a cabo una tarea específica de forma momentánea.

La dependencia es el vínculo más débil entre clases, ya que no implica que el objeto sea parte de la identidad o del estado del contenedor. Una vez que el método termina su ejecución, la relación desaparece, lo que permite un menor acoplamiento entre los componentes del sistema en comparación con la composición.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
Para implementar una composición fuerte en Java, se suele realizar la instanciación de los objetos internos dentro del constructor de la clase contenedora o realizar copias defensivas. De este modo, la Linea "posee" sus propios puntos y nadie desde fuera puede modificarlos ni tiene acceso a la misma instancia de memoria, asegurando que mueran con la línea.

En la composición débil (agregación), la Linea recibe referencias de puntos que ya han sido creados fuera de ella. La clase simplemente almacena esas referencias. Si la Linea se destruye, los objetos Punto originales siguen existiendo fuera de ella, ya que fueron creados y son referenciados en otros ámbitos del programa.
// Composición Fuerte: La línea crea y posee sus puntos
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// Composición Débil (Agregación): La línea recibe puntos externos
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto externo1, Punto externo2) {
        this.p1 = externo1;
        this.p2 = externo2;
    }
}
## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En Java, la destrucción de objetos no es responsabilidad del programador, a diferencia de C donde se utilizaría free(). La gestión de memoria la realiza el Garbage Collector (Recolector de Basura). Este mecanismo identifica automáticamente los objetos que ya no tienen ninguna referencia activa apuntando a ellos y libera el espacio que ocupan en la memoria.

En una composición fuerte, los puntos están referenciados únicamente por los atributos privados de la Linea. Cuando el objeto Linea deja de ser referenciado y se vuelve elegible para el Recolector de Basura, las referencias internas a los objetos Punto también se pierden. En ese momento, al no haber más formas de acceder a esos puntos, el Recolector de Basura entiende que puede destruirlos.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
La implementación requiere gestionar manualmente un array y una variable contador para simular una lista dinámica. Se debe asegurar que el director sea siempre un objeto que resida dentro del array de profesores. Al eliminar un profesor, es crítico verificar que no sea el director actual para no dejar al departamento en un estado inconsistente (violación de la invariante).

La encapsulación se mantiene al no exponer el array directamente. Se proporcionan métodos para añadir elementos y acceder a ellos individualmente por posición, de modo que el usuario de la clase no necesita conocer la estructura de datos interna (el array de tamaño fijo 50).
public class Departamento {
    private String nombre;
    private Profesor[] listaProfesores;
    private int contador;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        this.nombre = nombre;
        this.listaProfesores = new Profesor[50];
        this.contador = 0;
        aniadirProfesor(directorInicial);
        this.director = directorInicial;
    }

    public void aniadirProfesor(Profesor p) {
        if (contador >= 50) throw new IllegalStateException("Capacidad máxima alcanzada.");
        listaProfesores[contador++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= contador) throw new IllegalArgumentException("Posición inexistente.");
        if (listaProfesores[pos] == director) {
            throw new RuntimeException("No se puede eliminar al director si no se cambia primero.");
        }
        for (int i = pos; i < contador - 1; i++) {
            listaProfesores[i] = listaProfesores[i + 1];
        }
        listaProfesores[--contador] = null;
    }

    public void cambiarDirector(Profesor nuevo) {
        boolean estaEnLista = false;
        for (int i = 0; i < contador; i++) {
            if (listaProfesores[i] == nuevo) { estaEnLista = true; break; }
        }
        if (!estaEnLista) throw new IllegalArgumentException("El director debe pertenecer al departamento.");
        this.director = nuevo;
    }

    public int getNumProfesores() { return contador; }
    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= contador) throw new IllegalArgumentException("Posición inválida.");
        return listaProfesores[pos];
    }
}
## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
Al utilizar la interfaz List (y su implementación ArrayList), se delega la gestión del tamaño y el desplazamiento de elementos en las inserciones y borrados. Se elimina la necesidad de mantener un contador manual y los bucles for para desplazar elementos al eliminar, ya que el método remove() de la lista se encarga de reestructurar la colección internamente.

El problema de devolver la lista interna directamente es la fuga de encapsulación. Si un método devuelve la referencia listaProfesores, cualquier agente externo podría añadir o borrar elementos saltándose las reglas de la clase (como borrar al director o añadir más de 50). Para resolverlo, se debe devolver una copia de la lista o una vista inmutable mediante Collections.unmodifiableList(listaProfesores).
import java.util.*;

public class DepartamentoList {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(Profesor director) {
        profesores.add(director);
        this.director = director;
    }

    public void eliminarProfesor(int pos) {
        if (profesores.get(pos) == director) throw new RuntimeException("Error: es el director.");
        profesores.remove(pos);
    }

    public List<Profesor> getTodosLosProfesores() {
        // Se devuelve una vista protegida para evitar modificaciones externas
        return Collections.unmodifiableList(profesores);
    }
}
## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
La composición recursiva ocurre cuando una clase tiene un atributo de su propio tipo. Esto permite crear estructuras en cadena o en árbol de profundidad indefinida. En el caso de la clase Persona, el atributo madre es otra referencia a Persona. La cadena termina cuando este atributo es null, indicando que no hay más información en la jerarquía hacia atrás.

Otros ejemplos clásicos de composiciones recursivas en computación incluyen las listas enlazadas (donde un nodo apunta a otro nodo), los sistemas de archivos (donde una carpeta contiene otras carpetas) y las estructuras de comentarios en redes sociales (donde una respuesta es en sí misma un comentario que puede tener más respuestas).
import java.util.*;

public class DepartamentoList {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(Profesor director) {
        profesores.add(director);
        this.director = director;
    }

    public void eliminarProfesor(int pos) {
        if (profesores.get(pos) == director) throw new RuntimeException("Error: es el director.");
        profesores.remove(pos);
    }

    public List<Profesor> getTodosLosProfesores() {
        // Se devuelve una vista protegida para evitar modificaciones externas
        return Collections.unmodifiableList(profesores);
    }
}
## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Una relación bidireccional permite que ambos objetos involucrados en la composición tengan una referencia mutua. En el ejemplo anterior, no solo el Departamento sabría qué profesores tiene, sino que cada Profesor tendría un atributo para saber a qué Departamento pertenece. Esto facilita la navegación entre objetos en ambos sentidos sin necesidad de búsquedas externas.

Para implementar esto, la clase Profesor debe añadir un atributo private Departamento depto. El reto principal es mantener la consistencia: cuando un profesor se añade a un departamento, el departamento debe actualizar su lista y el profesor debe actualizar su referencia al departamento. Esto suele hacerse centralizando la lógica en un solo método "vinculador" que realice ambas asignaciones para evitar que un objeto apunte a un sitio y el otro a uno distinto.
-->