<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
Polimorfismo: Literalmente significa "muchas formas". Es la capacidad de un objeto de comportarse de distintas maneras según el contexto. En POO, sirve para que puedas darle la misma orden a diferentes objetos (usando una referencia común) y que cada uno responda a su manera.

Sobreescritura (Overriding): Es cuando una clase hija decide que el método que heredó de su padre no le sirve tal cual y lo vuelve a escribir para adaptarlo a su propia naturaleza.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
Consiste en que el ordenador no sabe qué versión del método va a ejecutar hasta el momento exacto de la ejecución (runtime).

Relación con polimorfismo: Es el motor que lo hace posible. Si tienes un Soldado que en realidad es un Artillero, la ligadura dinámica decide en el último milisegundo: "¡Espera! No uses el saludar del soldado, usa el del artillero".

Comparativa:

Java: Es el comportamiento por defecto. No tienes que pedirlo.

C++: Tienes que indicarlo explícitamente usando la palabra clave virtual. Si no lo pones, el enlace es estático (se decide al compilar).

Python: Es dinámico por naturaleza (Duck Typing). Todo es tardío en Python.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
Aquí vemos cómo el mismo array trata a todos como soldados, pero cada uno responde con su propia "personalidad":

Java
class Soldado {
    public void saludar() { System.out.println("Soldado saludando."); }
}

class Zapador extends Soldado {
    @Override
    public void saludar() { System.out.println("Zapador: ¡Cuidado con las minas! Saludos."); }
}

class Artillero extends Soldado {
    // El artillero no sobreescribe, usa el del padre
}

public class TestPolimorfismo {
    public static void main(String[] args) {
        Soldado[] peloton = { new Zapador(), new Artillero() };

        for (Soldado s : peloton) {
            s.saludar(); // Aquí ocurre la magia del polimorfismo
        }
    }
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, puedes (y a veces debes) aprovechar lo que ya hacía el padre. Usamos la palabra clave super.

Java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Hace lo que hace un soldado normal
        System.out.println("ZAPADOR A SUS ORDENES"); // Añade su toque personal
    }
}

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Restricciones: El método sobreescrito debe tener la misma firma (nombre y parámetros). El tipo de retorno debe ser el mismo o un subtipo (retorno covariante).

Diferencia clave:

Sobreescritura (Overriding): Cambias el comportamiento de un método heredado (misma firma, distinta clase).

Sobrecarga (Overloading): Creas varios métodos con el mismo nombre pero con distintos parámetros (misma clase).

@Override: Es una anotación que le dice al compilador: "Oye, mi intención es sobreescribir". Si te equivocas en una letra del nombre, el compilador te avisará con un error. Úsala siempre para evitar errores tontos.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí. En Java, cada vez que haces un System.out.println(objeto), Java llama internamente al método toString(). Como todas las clases heredan de Object, si tú sobreescribes toString() en tu clase, estás usando polimorfismo para que Java sepa cómo imprimir tu objeto específico.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Clase Abstracta: Es una clase "incompleta". Es una idea, no un objeto real. No puedes hacer new Soldado() si la clase es abstracta.

Método Abstracto: Es un método que se declara pero no se implementa (no tiene llaves {}). Es una promesa: "Todos mis hijos sabrán hacer esto, pero cada uno a su manera".

Java
abstract class Soldado {
    public void saludar() { /* ... */ } 
    public abstract void atacar(); // Sin cuerpo, se pone 'abstract' aquí y en la clase
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
Clase final: No puede tener hijos. Se corta la herencia. (Ejemplo: La clase String en Java es final por seguridad y rendimiento).

Método final: No puede ser sobreescrito por los hijos.

Relación con polimorfismo: El final es el enemigo del polimorfismo; se usa cuando quieres asegurar que un comportamiento no cambie nunca.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Las interfaces son contratos 100% puros. Mientras que una clase abstracta puede tener algo de código, una interfaz tradicional solo dice qué hay que hacer, no cómo.

Diferencia principal: Una clase solo puede heredar de un padre (clase), pero puede implementar muchas interfaces.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
Aquí vemos cómo la clase Linea puede trabajar con puntos sin saber si son de 2 o 3 dimensiones:

Java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto p);
}

class Punto2D extends Punto {
    double x, y;
    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto2D) {
            Punto2D p2 = (Punto2D) p; // Downcasting
            return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
        }
        return 0;
    }
}

class Linea {
    Punto inicio, fin;
    public double longitud() {
        return inicio.calcularDistanciaA(fin); // Polimorfismo total
    }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
En Java, las interfaces pueden heredar de otras interfaces. Y a diferencia de las clases, una interfaz sí puede heredar de varias interfaces a la vez.

Java
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
Esto permite crear contratos cada vez más específicos partiendo de bases sencillas.