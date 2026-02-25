<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

<!--
TEMA 2. Encapsulación
1. En Programación Orientada a Objetos (POO), ¿Qué buscan la encapsulación y la ocultación de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación y la ocultación de información buscan organizar y proteger el estado de los objetos. Mediante la encapsulación se agrupan en una misma unidad (la clase) los datos (atributos) y las operaciones (métodos) que trabajan con esos datos, de forma similar a un struct en C que agrupa campos, pero añadiendo funciones “pegadas” al tipo. La ocultación de información complementa esto restringiendo el acceso directo a la representación interna, de manera que el estado no se manipule “a mano” desde cualquier parte del programa.

La idea práctica es obligar a interactuar con el objeto a través de una interfaz controlada (métodos públicos). En Java esto se consigue típicamente declarando atributos como private y exponiendo solo los métodos necesarios para consultar o modificar el estado de forma segura.

Ventajas de ocultar información:

Evita usos indebidos del estado (menos errores).

Permite cambiar la implementación interna sin romper el código de quien la usa.

Ayuda a mantener invariantes (consistencia del objeto, que el estado siempre sea válido).

Reduce acoplamiento: el resto del programa depende menos de detalles.


2. ¿Qué se entiende por la interfaz pública de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública es el conjunto de miembros accesibles desde fuera (normalmente métodos public, y a veces constructores y constantes) que define cómo se usa la clase.(Las operaciones que se pueden realizar).

La relación con la ocultación de información es que la interfaz pública marca lo que está permitido y lo que se garantiza hacia el exterior, mientras que los detalles internos quedan ocultos. Al declarar atributos y métodos auxiliares como private, se impide que el resto del programa dependa de cómo se guarda el estado o de pasos internos de cálculo, manteniendo la consistencia del objeto y la estabilidad del código que lo usa.


3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la interfaz pública de una clase? ¿Es fácil cambiarla?

Si se diseña sin cuidado, se obliga a que muchas partes del programa dependan de métodos innecesarios, nombres confusos o comportamientos poco claros, y ese problema se propaga. Además, si se expone demasiado (por ejemplo, muchos setters sin control), se facilita que se deje al objeto en estados inválidos.

No suele ser fácil cambiarla, porque cualquier cambio en un método public (nombre, parámetros, tipo de retorno o eliminación) puede romper el código que lo llama. Por eso se busca que la interfaz pública sea lo más pequeña y coherente posible, dejando los detalles y ayudas internas como private para poder modificarlos sin afectar a quien usa la clase.


4. ¿Qué son las invariantes de clase y por qué la ocultación de información nos ayuda?

Las invariantes de clase se entienden como reglas que deben cumplirse siempre para que los objetos de esa clase estén en un estado válido. Se trata de condiciones sobre el estado interno, por ejemplo que una edad no sea negativa, que un saldo no baje de cero, o que ciertos valores tengan coherencia entre sí. Se consideran “invariantes” porque deberían mantenerse antes y después de ejecutar cualquier método público, evitando estados imposibles o incoherentes.

La ocultación de información ayuda porque impide modificar directamente los atributos desde fuera. Si los atributos se declaran private, no se puede hacer una asignación arbitraria que rompa la regla, y cualquier cambio debe pasar por métodos públicos que pueden validar datos y mantener la coherencia. Así, el objeto no depende de que quien lo use “se porte bien”, sino de que el propio diseño obliga a conservar esas invariantes.


5. Pon un ejemplo de una clase Punto en Java, con dos coordenadas, x e y, de tipo double, con un método calcularDistanciaAOrigen, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase Punto? ¿Qué significa public y private?

public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() { 
        return x; 
    }

    public double getY() { 
        return y; 
    }

}

Interfaz pública: Punto(double, double), calcularDistanciaAOrigen(), getX(), getY().

public: accesible desde cualquier clase.

private: accesible solo desde dentro de la propia clase.


6. En Java, ¿A quiénes se pueden aplicar los modificadores public o private?

En Java, public y private se pueden poner en campos (atributos), métodos y constructores para controlar quién puede usarlos. public significa “se puede acceder desde cualquier clase”, y private significa “solo se puede acceder desde dentro de la misma clase”.

También se pueden aplicar a clases (y otros tipos como interfaces/enums), pero con una regla: una clase “normal” de archivo (top-level) no puede ser private, solo puede ser public o sin modificador (visible solo dentro del paquete)(esto se da en la siguiente pregunta). En cambio, las clases anidadas dentro de otra clase sí pueden ser private, porque ahí tiene sentido ocultarlas como detalle interno.


7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí, suele haber más niveles.

-En Java existen 4:

public: Accesible desde cualquier lugar.

private: Accesible sólo dentro de la propia clase.

protected: Accesible en el mismo paquete y desde subclases.

sin modificador (package-private): visible solo dentro del mismo paquete.

-En otros lenguajes(Extra):

C++: public, private, protected.

C#: añade variantes como internal, protected internal, etc.

Python: no hay privado real; es una convención (_ / __).


8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método calcularDistanciaAPunto(Punto otro) y explica la respuesta.

Los miembros de instancia private están ocultos para otras clases, pero no para otras instancias si el acceso se realiza desde dentro de la misma clase. En Java, private significa “solo accesible desde el código de la propia clase”, no “solo accesible desde este objeto concreto”. Por eso una instancia puede acceder a los campos privados de otra instancia siempre que el acceso ocurra en un método de la misma clase.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // permitido: misma clase
        double dy = this.y - otro.y; // permitido: misma clase
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Explicación: Aunque otro sea otra instancia, el acceso otro.x ocurre desde dentro de la clase Punto, y Java lo permite.

Explicación más práctica: En calcularDistanciaAPunto(Punto otro), otro es un Punto. Los campos x e y son privados de la clase Punto. Como el método está escrito dentro de Punto, ese código tiene permiso para acceder a x e y de cualquier instancia de Punto, incluida otro.


9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos getter y setter son métodos que se usan para controlar el acceso a los atributos de un objeto cuando estos se mantienen ocultos (por ejemplo, con private). Un getter devuelve el valor de un atributo, permitiendo consultarlo desde fuera sin acceder directamente al campo. Un setter cambia el valor de un atributo, concentrando en un único punto la modificación del estado(todos los cambios pasen por el mismo método, cada uno por el suyo vaya, setEdad para modificar edad, setMatricula para modificar matrícula y etc...)(La idea general es que cada cambio se haga pasando por un método).

En Java se suelen ver como getX() para leer y setX(...) para escribir. La ventaja frente a exponer el atributo directamente es que se puede añadir lógica: comprobar valores, impedir cambios inválidos o mantener coherencia con otros campos. Así, el acceso al estado se hace de forma controlada, aun así, no siempre conviene crear setters para todo.


10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No. Aquí “seguridad” significa robustez y corrección: evitar estados inválidos, errores y dependencias frágiles. No es seguridad informática contra ataques (aunque a veces puede ayudar indirectamente).
Pero el objetivo principal en POO al hablar de ocultación es la calidad del diseño y la prevención de bugs, no la defensa contra atacantes.


11. ¿Qué diferencia hay entre miembro de instancia y miembro de clase? ¿Los miembros de clase también se pueden ocultar?

Miembro de instancia: Un miembro de instancia pertenece a cada objeto concreto: cada vez que se crea un Punto, ese objeto tiene sus propios valores de x e y. Por eso dos objetos distintos pueden tener estados distintos.

Miembro de clase (static): Un miembro de clase (marcado como static) pertenece a la clase como un único valor compartido, de modo que todas las instancias ven el mismo dato; por ejemplo, un contador de cuántos objetos se han creado o una constante común.

Los miembros de clase también se pueden ocultar exactamente igual que los de instancia. En Java se puede escribir private static ... para que ese campo o método solo sea accesible desde dentro de la propia clase, aunque sea compartido. Esto es útil para guardar detalles internos (por ejemplo, una tabla de constantes, un contador interno o un método auxiliar) sin exponerlos como parte de la interfaz pública.


12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Tiene sentido que un constructor sea private cuando se quiere controlar estrictamente cómo se crean los objetos. Al hacerlo privado, se impide que otras clases usen new directamente, y se obliga a crear instancias mediante métodos de la propia clase. Esto permite centralizar reglas de creación y evitar objetos mal construidos o con datos inválidos.

Un caso típico es el patrón Singleton, donde se quiere garantizar que exista una única instancia. 

Otro uso común son los métodos factoría static (por ejemplo crear(...)), que pueden validar parámetros, devolver instancias ya existentes (caché) o elegir entre distintas implementaciones sin que quien use la clase tenga que saberlo.

También se emplea para impedir la instanciación cuando no tiene sentido crear objetos, como en clases “utilidad” que solo contienen métodos static (por ejemplo Math). En ese caso, un constructor privado evita que se haga new por error y deja claro el uso esperado de la clase.


13. ¿Cómo se indican los miembros de clase en Java? Pon un ejemplo, en la clase Punto definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores x e y máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase en Java se indican con static, lo que significa que pertenecen a la clase y no a cada objeto individual. En lugar de tener una copia por instancia, existe una única copia compartida por todas las instancias, por lo que es útil para contadores, constantes o valores globales relacionados con el tipo. Para consultarlos no hace falta crear un objeto: se accede con el nombre de la clase (Punto.getMaxX()).

public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}


14. Como sería un método factoría dentro de la clase Punto para construir un Punto a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado static?

public static Punto crearRedondeado(double x, double y) {
    double xr = Math.rint(x);
    double yr = Math.rint(y);
    return new Punto(xr, yr);
}

Sí, he usado static porque es un método y miembro de clase (factoría) y no requiere un objeto previo.

onviene hacer un método static cuando no depende del estado de un objeto concreto. Es decir, no necesita this. Suele pasar en tres casos típicos: funciones de utilidad (como Math.sqrt), métodos factoría para crear objetos (como Punto.crearRedondeado(...)), o lógica que trabaja con datos que se pasan por parámetros y no con atributos de una instancia.

Conviene que no sea static cuando el método usa o modifica los atributos del objeto (su estado). Ahí tiene sentido que exista un this y que el método se llame sobre una instancia concreta, por ejemplo p.calcularDistanciaAOrigen() o p.mover(dx, dy). Esto hace más claro “sobre qué objeto” se está actuando.

Como regla simple: si al escribir el método se necesita acceder a x, y u otros atributos del objeto, normalmente no debe ser static. Si solo usa parámetros o constantes de clase, normalmente sí puede ser static.


15. Cambia la implementación de Punto. En vez de dos double, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

public class Punto {
    private final double[] coords = new double[2]; // [0]=x, [1]=y

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() { 
        return coords[0]; 
    }

    public double getY() { 
        return coords[1]; 
    }

    public double calcularDistanciaAOrigen() {
        double x = coords[0];
        double y = coords[1];
        return Math.sqrt(x * x + y * y);
    }
}

Se ha cambiado la representación interna de dos double a un array de dos posiciones, pero se han mantenido los mismos métodos públicos (constructor, getX(), getY(), calcularDistanciaAOrigen()), así que quien use la clase no tendría por qué enterarse del cambio. Esto es precisamente un ejemplo de ocultación de información: la interfaz pública permanece estable mientras la implementación interna puede variar.


16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?
Respuesta
No es mejor declararlo público: con getters/setters puedes:


Validar valores (mantener invariantes).


Cambiar la implementación interna sin romper a los clientes.


Registrar cambios, lanzar excepciones, etc.


Convención habitual: atributos privados y acceso controlado mediante métodos. Sí, esto está muy relacionado con preservar invariantes.

17. ¿Qué significa que una clase sea inmutable? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?
Respuesta


Inmutable: después de construirse, su estado no cambia (no hay cambios en atributos observables).


Método modificador: método que cambia el estado del objeto.


Un modificador no siempre es un “setter”: puede ser incrementar(), añadirElemento(), rotar(), etc.


Ventajas de inmutabilidad:


Más fácil de razonar (menos efectos laterales).


Más segura en concurrencia (hilos).


Menos bugs por estados intermedios inválidos.



18. ¿Es recomendable incluir métodos "setter" siempre y como convención?
Respuesta
No. Poner setters por rutina suele romper encapsulación. Lo recomendable es:


Exponer solo los cambios que tengan sentido en el dominio.


Preferir objetos inmutables cuando encaje.


Usar setters solo si realmente necesitas mutabilidad controlada.



19. ¿La clase String en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?
Respuesta
String es inmutable.
Al concatenar (a + b) se crea un nuevo String (no modifica el anterior). Para concatenar muchas veces de forma eficiente se usa StringBuilder (o StringBuffer si necesitas sincronización).

20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?
Respuesta
Depende: puedes comparar por identidad (misma referencia) o por contenido (mismos datos).
En Java:


== compara referencias (identidad).


equals() compara contenido si está sobreescrito.


Por defecto, equals() en Object se comporta como == (identidad). Para comparar cadenas: usar a.equals(b) (o Objects.equals(a,b) si puede haber null).

21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?
Respuesta
Wrappers son clases que “envuelven” tipos primitivos para tratarlos como objetos: int → Integer, double → Double, etc.
En Java existe autoboxing/unboxing (automático en muchos casos):


Integer a = 5; (autoboxing)


int b = a; (unboxing)


Ventajas:


Permiten usar primitivos en colecciones genéricas (List<Integer>).


Ofrecen métodos útiles (Integer.parseInt, etc.).


Permiten null para representar “ausencia”.


No todos los lenguajes OO tienen primitivos separados (por ejemplo, algunos tratan todo como objeto), así que no siempre hacen falta wrappers.

22. ¿En POO qué es un tipo de dato enumerado? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?
Respuesta
Un enumerado define un conjunto finito y cerrado de valores posibles (ej.: LUNES..DOMINGO).
En Java, enum es una forma especial de clase: puede tener atributos, métodos y constructores.
Ventajas de encapsulación:


Limita valores a un conjunto válido (evita “strings mágicos”).


Puede incluir lógica asociada (métodos) sin exponer detalles.


Permite invariantes “por diseño” (no existen valores fuera del enum).



23. Crea un tipo enumerado en Java que se llame Mes, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.
Respuesta
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private final int ordinalEnAnio;
    private final int dias;

    private Mes(int ordinalEnAnio, int dias) {
        this.ordinalEnAnio = ordinalEnAnio;
        this.dias = dias;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinalEnAnio() {
        return ordinalEnAnio;
    }
}


24. Añade a la clase Mes del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro enHemisferioNorte). Es decir: esDePrimavera(boolean esHemisferioNorte), esDeVerano(boolean esHemisferioNorte), esDeOtoño(boolean esHemisferioNorte), esDeInvierno(boolean esHemisferioNorte)
Respuesta
public boolean esDePrimavera(boolean esHemisferioNorte) {
    // Norte: Mar-Abr-May | Sur: Sep-Oct-Nov
    return esHemisferioNorte
            ? (this == MARZO || this == ABRIL || this == MAYO)
            : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
}

public boolean esDeVerano(boolean esHemisferioNorte) {
    // Norte: Jun-Jul-Ago | Sur: Dic-Ene-Feb
    return esHemisferioNorte
            ? (this == JUNIO || this == JULIO || this == AGOSTO)
            : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
}

public boolean esDeOtoño(boolean esHemisferioNorte) {
    // Norte: Sep-Oct-Nov | Sur: Mar-Abr-May
    return esHemisferioNorte
            ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
            : (this == MARZO || this == ABRIL || this == MAYO);
}

public boolean esDeInvierno(boolean esHemisferioNorte) {
    // Norte: Dic-Ene-Feb | Sur: Jun-Jul-Ago
    return esHemisferioNorte
            ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
            : (this == JUNIO || this == JULIO || this == AGOSTO);
}
--!>
