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
Respuesta


Encapsulación: agrupar en una misma “cápsula” (clase/objeto) los datos (atributos) y el comportamiento (métodos) que operan sobre esos datos.


Ocultación de información: restringir el acceso directo a los detalles internos (representación/estado) y obligar a usar una interfaz controlada.


Ventajas de ocultar información:


Evita usos indebidos del estado (menos errores).


Permite cambiar la implementación interna sin romper el código de quien la usa.


Ayuda a mantener invariantes (consistencia del objeto).


Reduce acoplamiento: el resto del programa depende menos de detalles.



2. ¿Qué se entiende por la interfaz pública de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.
Respuesta
La interfaz pública es el conjunto de miembros accesibles desde fuera (normalmente métodos public, y a veces constructores y constantes) que define cómo se usa la clase.
Se relaciona con la ocultación porque:


La interfaz pública es lo “visible y permitido”.


La implementación interna (atributos y métodos auxiliares) se mantiene oculta (private) para controlar el acceso y preservar consistencia.



3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la interfaz pública de una clase? ¿Es fácil cambiarla?
Respuesta
Porque la interfaz pública es un contrato: todo el código cliente dependerá de ella. Si la diseñas mal, arrastras ese error a muchos sitios.
No suele ser fácil cambiarla: cambiar una firma pública o eliminar un método puede romper código existente. Por eso se intenta que la interfaz sea pequeña, clara y estable.

4. ¿Qué son las invariantes de clase y por qué la ocultación de información nos ayuda?
Respuesta
Las invariantes son condiciones que deben cumplirse siempre para que un objeto sea válido (ej.: “edad ≥ 0”, “saldo nunca negativo”, “x e y finitos”, etc.).
La ocultación ayuda porque si los atributos son private, nadie puede cambiarlos “a lo loco”: solo se modifican a través de métodos que verifican y mantienen esas invariantes.

5. Pon un ejemplo de una clase Punto en Java, con dos coordenadas, x e y, de tipo double, con un método calcularDistanciaAOrigen, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase Punto? ¿Qué significa public y private?
Respuesta
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

    public double getX() { return x; }
    public double getY() { return y; }
}



Interfaz pública: Punto(double, double), calcularDistanciaAOrigen(), getX(), getY().


public: accesible desde cualquier clase.


private: accesible solo desde dentro de la propia clase.



6. En Java, ¿A quiénes se pueden aplicar los modificadores public o private?
Respuesta
En Java se aplican a:


Clases (pero top-level no pueden ser private; sí pueden ser public o package-private).


Atributos (campos).


Métodos.


Constructores.


Clases anidadas (inner/nested classes) y miembros dentro de ellas.



7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?
Respuesta
Sí, suele haber más niveles.
En Java existen 4:


public


private


protected


sin modificador (package-private): visible solo dentro del mismo paquete.


En otros lenguajes:


C++: public, private, protected.


C#: añade variantes como internal, protected internal, etc.


Python: no hay privado real; es una convención (_ / __).



8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método calcularDistanciaAPunto(Punto otro) y explica la respuesta.
Respuesta
Están ocultos para (a) otras clases, pero no para otras instancias si estás dentro de la misma clase.
Ejemplo:
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

Explicación: aunque otro sea otra instancia, el acceso otro.x ocurre desde dentro de la clase Punto, y Java lo permite.

9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?
Respuesta


Getter: método que lee un atributo (ej. getX()).


Setter: método que modifica un atributo (ej. setX(double x)), normalmente validando reglas/invariantes.



10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?
Respuesta
No. Aquí “seguridad” significa robustez y corrección: evitar estados inválidos, errores y dependencias frágiles. No es seguridad informática contra ataques (aunque a veces puede ayudar indirectamente).

11. ¿Qué diferencia hay entre miembro de instancia y miembro de clase? ¿Los miembros de clase también se pueden ocultar?
Respuesta


Miembro de instancia: pertenece a cada objeto (cada Punto tiene su x e y).


Miembro de clase (static): pertenece a la clase como “único compartido” por todas las instancias.


Sí, los miembros de clase también se pueden ocultar: private static ... es totalmente válido.

12. Brevemente: ¿Tiene sentido que los constructores sean privados?
Respuesta
Sí. Se usa para:


Singleton (una única instancia).


Métodos factoría (static) que controlan cómo se crean objetos.


Evitar creación directa y forzar validaciones/formatos específicos.



13. ¿Cómo se indican los miembros de clase en Java? Pon un ejemplo, en la clase Punto definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores x e y máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.
Respuesta
Se indican con static.
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
Respuesta
public static Punto crearRedondeado(double x, double y) {
    double xr = Math.rint(x);
    double yr = Math.rint(y);
    return new Punto(xr, yr);
}

Sí, he usado static porque es un método de clase (factoría) y no requiere un objeto previo.

15. Cambia la implementación de Punto. En vez de dos double, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.
Respuesta
public class Punto {
    private final double[] coords = new double[2]; // [0]=x, [1]=y

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    public double calcularDistanciaAOrigen() {
        double x = coords[0];
        double y = coords[1];
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública puede seguir siendo la misma (getX, getY, constructor, etc.), aunque por dentro cambie la representación.

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
