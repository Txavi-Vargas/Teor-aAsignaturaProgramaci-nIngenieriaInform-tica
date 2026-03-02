<!-- Posible prompt: <prompt> Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son: - C/C++ sin orientación a objetos. - Temas de Java previos: Clases y Objetos, Encapsulación. Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código). Por favor, escribe en impersonal las respuestas. </prompt> ---- -->
<!--
TEMA 3. Excepciones
1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función raiz ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.
Respuesta

En C, si una función como raiz detecta un error (por ejemplo, recibir un número negativo), no puede “lanzar” una excepción, así que debe indicar el error de alguna forma explícita para que el código llamador lo gestione e informe al usuario. La idea es separar responsabilidades: raiz detecta el error y lo comunica; el llamador decide qué mensaje mostrar y qué hacer después.

Una primera opción común es devolver un código de estado (éxito/error) y entregar el resultado real mediante un parámetro de salida. Esto permite que el llamador compruebe el retorno y, sólo si es correcto, use el resultado. Es una forma clara y muy usada en APIs de C porque obliga a revisar el estado.

Una segunda opción es usar errno (u otra variable global de error) para que la función deje “registrada” la causa del fallo y el llamador la consulte. Esto tiene la ventaja de no cambiar la firma con parámetros extra, pero introduce dependencia de estado global (y es más delicado en código concurrente si no se gestiona bien).

// Opción 1: código de estado + parámetro de salida
#include <math.h>

int raiz(float x, float *out) {
    if (x < 0.0f) return 0;     // 0 = error
    *out = sqrtf(x);
    return 1;                  // 1 = ok
}

// Uso
#include <stdio.h>
int main() {
    float r;
    float x = -9.0f;
    if (!raiz(x, &r)) {
        printf("Error: no se puede calcular raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }
}
// Opción 2: usar errno
#include <math.h>
#include <errno.h>

float raiz(float x) {
    if (x < 0.0f) {
        errno = EDOM;          // "domain error"
        return 0.0f;           // valor de retorno no fiable en error
    }
    errno = 0;
    return sqrtf(x);
}

// Uso
#include <stdio.h>
#include <string.h>

int main() {
    errno = 0;
    float x = -9.0f;
    float r = raiz(x);
    if (errno != 0) {
        printf("Error (%d): %s\n", errno, strerror(errno));
    } else {
        printf("Resultado: %f\n", r);
    }
}
2. Brevemente ¿Qué es una "excepción"? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?
Respuesta

Una excepción es un mecanismo del lenguaje para representar y señalizar un error o situación anómala que impide continuar con el flujo normal de ejecución. En lugar de devolver códigos de error por todas partes, se “lanza” un objeto/valor de error y el lenguaje se encarga de buscar un lugar apropiado para manejarlo.

Al implementar funciones, se usan excepciones para separar el camino normal del camino de error: el código “normal” queda más limpio y el error se expresa de forma explícita, con información asociada (tipo de error, mensaje, etc.). Además, se puede decidir si un método maneja el problema o si lo deja subir para que lo trate un nivel superior.

Al llamar a funciones, se usan excepciones para concentrar el tratamiento de errores en un punto (por ejemplo, en main o en una capa de servicio), evitando tener que comprobar retornos tras cada llamada. Así se mejora la legibilidad y se fuerza a pensar qué errores se van a tratar y cuáles se consideran imposibles o de programación.

3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase Calculadora y llama a dicho método desde el método main, mostrando cómo se puede controlar desde fuera.
Respuesta

En Java, la forma habitual de indicar que una entrada no es válida (como un número negativo para una raíz cuadrada real) es lanzar una excepción desde el método. El método se centra en calcular cuando es posible y, si no lo es, comunica el problema de manera estructurada mediante una excepción.

El control “desde fuera” se realiza rodeando la llamada con un bloque try-catch. De este modo, Calculadora.raiz no imprime nada ni decide el mensaje al usuario: sólo detecta la situación incorrecta; el main decide qué mostrar y cómo continuar el programa.

class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo: " + x);
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        double x = -9.0;

        try {
            double r = Calculadora.raiz(x);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
4. ¿Qué es "lanzar" una excepción? ¿Qué es "controlar" o "capturar" una excepción? ¿Qué es que se "propague" una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.
Respuesta

Lanzar una excepción significa crear (o reutilizar) una excepción y señalar al runtime que ha ocurrido un error usando throw. En ese instante se interrumpe el flujo normal de esa función: el método ya no sigue ejecutando las siguientes líneas “normales” y se empieza a buscar un manejador.

Capturar o controlar una excepción significa tener un bloque catch que coincide con el tipo de excepción lanzada. Cuando se captura, se ejecuta el código del catch y se puede decidir qué hacer: informar, recuperar, relanzar, etc. Si no hay catch adecuado en ese método, la excepción no se controla ahí.

La propagación es el proceso por el cual la excepción “sube” por la pila de llamadas: Java va saliendo del método donde ocurrió, luego del método que lo llamó, etc., hasta encontrar un catch compatible. Los métodos por los que pasa terminan de forma abrupta (no se reanudan) y se “desenrolla” la pila; sólo se ejecutarán bloques finally que existan en el camino. Si nadie la captura, el programa termina mostrando el stack trace.

class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Negativo: " + x);
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            System.out.println(Calculadora.raiz(-9.0));
            System.out.println("Esto no se ejecuta si se lanza la excepcion");
        } catch (IllegalArgumentException e) {
            System.out.println("Capturada en main: " + e.getMessage());
        }
        System.out.println("Programa continua despues del catch");
    }
}
5. ¿Qué ventajas tiene frente a C, la "propagación natural" de las excepciones a través de la pila (stack) de llamadas?
Respuesta

La propagación natural evita que cada función tenga que devolver y comprobar códigos de error continuamente. En C, cuando se llama a varias funciones encadenadas, se suele terminar con mucho código repetido del estilo “si ha fallado, devuelve error”, lo que ensucia el camino principal y hace más fácil olvidarse de comprobar un retorno.

En Java, si un nivel bajo detecta un error, se lanza la excepción y esta sube automáticamente hasta un nivel donde tenga sentido tratarla (por ejemplo, el main o una capa de interfaz). Eso permite que la lógica de negocio se escriba de forma más limpia y que el manejo de errores se concentre donde se puede dar una respuesta coherente.

Además, el stack trace que acompaña a la excepción da un contexto de depuración muy valioso: se ve la cadena exacta de llamadas que llevó al fallo. En C, lograr una información equivalente suele requerir instrumentación manual o herramientas externas.

6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?
Respuesta

En Java, las excepciones son objetos (instancias de clases que derivan de Throwable). Esto permite encapsular información del error dentro de un tipo con estructura: mensaje, causa, traza de pila y cualquier dato adicional que se quiera añadir mediante campos y métodos.

La encapsulación ayuda porque el error se convierte en algo “transportable” y autoexplicativo: el método que detecta el problema puede adjuntar detalles, y el método que lo maneja puede acceder a ellos de forma ordenada. Se evita depender de variables globales o códigos mágicos sin significado.

Por tanto, sí: se pueden crear excepciones personalizadas creando una clase que extienda Exception (controlada) o RuntimeException (no controlada). Esto mejora la claridad, porque el tipo de excepción ya comunica qué ha fallado a nivel conceptual.

class RaizNegativaException extends Exception {
    public RaizNegativaException(double x) {
        super("No existe raiz real para x=" + x);
    }
}
7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué información esencial lleva cualquier objeto excepción que es muy útil tener cuando se llega a un manejador?
Respuesta

Un objeto excepción en Java suele aportar información esencial que en C normalmente no viene “de serie” de forma tan integrada. Por ejemplo, suele incluir un mensaje descriptivo (getMessage()), que permite mostrar o registrar una explicación humana del problema sin inventar códigos numéricos en el llamador.

También incluye la traza de pila (stack trace), que indica la ruta exacta de llamadas que llevó al error. Esto es extremadamente útil para depurar, porque identifica el punto de fallo y el contexto de ejecución, sin necesidad de añadir printf o logs manuales en cada función.

Además, muchas excepciones pueden incluir una causa (getCause()), que enlaza un error de bajo nivel con otro de alto nivel. Esto permite conservar el detalle técnico original y, a la vez, presentar una excepción más significativa para la capa superior.

8. En Java, sobre el bloque "try-catch", ¿se pueden tener más de un bloque catch? ¿cuántos bloques catch se ejecutan?
Respuesta

En Java se pueden encadenar varios bloques catch después de un try. Esto se usa cuando se quiere manejar de forma distinta diferentes tipos de excepciones que podrían ocurrir dentro del mismo bloque try.

Cuando ocurre una excepción, Java busca el primer catch compatible (según el tipo de la excepción y la jerarquía de clases). Sólo se ejecuta un único bloque catch: el primero que encaje. Los demás catch se saltan.

Por esa razón, el orden importa: se deben colocar primero los catch más específicos y después los más generales, porque si se pone uno general antes, los específicos podrían volverse inalcanzables (error de compilación).

try {
    // ...
} catch (NumberFormatException e) {
    // ...
} catch (IllegalArgumentException e) {
    // ...
} catch (Exception e) {
    // ultimo, mas general
}
9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con finally, tanto con catch como sin él.
Respuesta

Para garantizar que cierto código se ejecuta “pase lo que pase” (haya o no haya excepción), Java ofrece el bloque finally. La idea es colocar ahí acciones de limpieza como cerrar ficheros, liberar recursos o restaurar estados, de forma que se ejecuten incluso si el flujo normal se interrumpe por una excepción.

Si se incluye catch, se puede manejar la excepción y aun así ejecutar el finally. Si no se incluye catch, la excepción seguirá propagándose, pero el finally se ejecutará igualmente antes de abandonar el método.

Esto soluciona un problema típico: sin finally, una excepción en medio del uso de un recurso podría saltarse la parte donde se cierra el recurso, dejando fugas o estados inconsistentes.

// finally con catch
try {
    // abrir/usar recurso
    System.out.println(Calculadora.raiz(-9.0));
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Limpieza: cerrar recursos aqui");
}
// finally sin catch (la excepcion se propaga)
try {
    System.out.println(Calculadora.raiz(-9.0));
} finally {
    System.out.println("Limpieza: se ejecuta antes de propagarse");
}
10. En Java, el bloque finally puede ir sin catch? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un return en medio del try?
Respuesta

En Java, finally puede ir sin catch: el patrón try-finally es válido y se usa cuando no interesa manejar la excepción en ese nivel, pero sí garantizar una limpieza. En ese caso, si ocurre una excepción, se ejecuta el finally y después la excepción continúa propagándose.

El bloque finally se ejecuta tanto si ocurre una excepción como si no ocurre. También se ejecuta si dentro del try se hace un return: antes de devolver el valor, Java ejecuta el finally y luego completa el retorno.

Una excepción notable es si la JVM se detiene de forma abrupta (por ejemplo, System.exit(...) o un fallo grave), donde no se puede garantizar la ejecución del finally. En situaciones normales de control de flujo, finally actúa como una garantía de salida.

static int f() {
    try {
        return 42;
    } finally {
        System.out.println("finally se ejecuta antes del return");
    }
}
11. En Java, qué son las excepciones "controladas" y las "no controladas"? ¿Qué papel juega RuntimeException? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.
Respuesta

En Java, las excepciones controladas (checked) son las que el compilador obliga a manejar: o bien se capturan con try-catch, o bien se declaran con throws en la firma del método. Suelen representar errores esperables del entorno, como fallos de E/S, red o ficheros. Las excepciones no controladas (unchecked) no obligan a nada en compilación y suelen indicar errores de programación o violaciones de precondiciones.

RuntimeException es la clase base típica de las excepciones no controladas. Si una excepción hereda de RuntimeException, el compilador no exige capturarla ni declararla. Se usa cuando el error indica que el código llamador ha usado mal una función (por ejemplo, pasar un argumento inválido) o cuando el programa está en un estado del que no se espera recuperar localmente.

Ejemplos típicos: controladas como IOException, FileNotFoundException, SQLException; no controladas como NullPointerException, ArrayIndexOutOfBoundsException, IllegalArgumentException. También se pueden crear propias: una DatosInvalidosException extends Exception (controlada) o una FormatoInvalidoException extends RuntimeException (no controlada).

Situaciones donde suele preferirse excepción controlada (checked):

Lectura de un fichero que puede no existir o no tener permisos (IOException).

Acceso a red donde puede fallar la conexión (IOException/SocketException).

Acceso a base de datos donde puede fallar una consulta (SQLException).

Parseo de entrada externa donde se quiere obligar a manejar el fallo (excepción propia extends Exception).

Situaciones donde suele preferirse excepción no controlada (unchecked):

Argumentos inválidos a un método público (IllegalArgumentException).

Estado interno imposible si el programa está bien escrito (IllegalStateException).

Uso de una referencia nula por error de programación (NullPointerException).

Índices fuera de rango por bug (IndexOutOfBoundsException).

12. ¿Qué es y para qué se usa throws? ¿Por qué es alternativa a capturar una excepción controlada?
Respuesta

throws es parte de la firma de un método en Java y se usa para declarar que ese método puede lanzar ciertas excepciones. Sirve como contrato: quien llame a ese método sabe que debe manejar esas excepciones (capturándolas) o declararlas a su vez.

Es alternativa a capturar una excepción controlada porque un método puede decidir que no tiene sentido manejar el error en ese nivel. Por ejemplo, un método de bajo nivel puede no conocer cómo informar al usuario o cómo recuperarse, así que declara throws y deja que una capa superior lo resuelva.

Esto favorece la separación de responsabilidades: el método se centra en su tarea principal, y el manejo de errores se ubica donde se tiene contexto suficiente para decidir (por ejemplo, en main, en la interfaz o en una capa de servicio).

13. Pon un ejemplo en Java de firma de método que incluya throws, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del finally.
Respuesta

Para un método que abre un fichero, una excepción típica controlada es FileNotFoundException (subclase de IOException). Si no interesa manejarla ahí, se declara con throws para que el llamador decida qué hacer (mostrar mensaje, reintentar, etc.). Aun así, si se han creado recursos parcialmente, se debe asegurar la limpieza.

Con finally se puede garantizar el cierre del recurso si llegó a abrirse. En la práctica moderna, se usa mucho try-with-resources, pero el enunciado pide finally, así que se muestra con cierre manual.

import java.io.*;

class Lector {
    public static String leerPrimeraLinea(String ruta) throws FileNotFoundException, IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta));
            return br.readLine();
        } finally {
            if (br != null) {
                try { br.close(); } catch (IOException e) { /* ignorar o registrar */ }
            }
        }
    }
}
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println(Lector.leerPrimeraLinea("no_existe.txt"));
        } catch (IOException e) {
            System.out.println("Error al leer fichero: " + e.getMessage());
        }
    }
}
14. ¿Podemos poner en throws excepciones no controladas, como RuntimeException? ¿Debería el método llamador entonces poner try-catch en ese caso? ¿Qué sentido tendría?
Respuesta

Se puede poner en throws una excepción no controlada (por ejemplo, RuntimeException o una subclase), porque Java lo permite sintácticamente. Sin embargo, no cambia las obligaciones del compilador: al ser no controlada, el llamador no está obligado a capturarla ni a declararla.

El llamador sólo debería poner try-catch si realmente puede recuperarse o si necesita traducir el error a otra forma (por ejemplo, mostrar un mensaje al usuario, registrar, devolver un estado HTTP, etc.). Capturar por capturar suele ocultar errores de programación y dificulta depuración.

El sentido de declarar throws RuntimeException suele ser documental: expresa que el método puede fallar si se incumplen precondiciones. Aun así, lo más habitual es documentarlo en comentarios o en la elección del tipo de excepción (IllegalArgumentException) sin necesidad de throws.

15. ¿Cuándo se recomienda usar excepciones controladas, como IOException, y cuándo no controladas como IllegalArgumentException? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?
Respuesta

Se recomienda usar excepciones controladas cuando el fallo es razonablemente esperable por causas externas y el programa podría actuar en consecuencia: ficheros que no existen, red que falla, permisos insuficientes, etc. Se fuerza así a que el programador piense en el tratamiento del error en algún punto del flujo.

Se recomienda usar excepciones no controladas cuando el problema indica un mal uso de la API o un bug: argumentos inválidos, estados imposibles, índices fuera de rango, nulos inesperados. En esos casos, obligar a capturar en cada llamada suele añadir ruido y puede esconder fallos que deberían corregirse, no “manejarse”.

No todos los lenguajes distinguen formalmente entre checked y unchecked. Por ejemplo, Java sí lo hace, mientras que otros lenguajes con excepciones (como C#) no obligan a declarar/capturar en compilación. En los lenguajes donde sólo existe una opción práctica, lo más habitual es el modelo “no controlado”: se lanzan excepciones y se capturan sólo donde tenga sentido.

16. ¿Tiene sentido lanzar excepciones dentro del catch? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.
Respuesta

Tiene sentido lanzar excepciones dentro de un catch cuando se quiere cambiar el nivel de abstracción del error o cuando se detecta que no se puede recuperar y se desea comunicar el fallo de otra forma. Por ejemplo, se puede capturar una excepción muy técnica y lanzar una más comprensible para la capa superior.

Se puede relanzar la misma excepción capturada usando throw; (en Java es throw e;). Esto tiene sentido cuando se necesita hacer algo adicional (registrar, limpiar, añadir contexto) pero no se quiere “comerse” el error. A menudo, en vez de relanzar exactamente la misma, se lanza una nueva que tenga como causa la original.

Ejemplo de lanzar otra excepción desde catch:

try {
    System.out.println(Calculadora.raiz(-9.0));
} catch (IllegalArgumentException e) {
    throw new RuntimeException("Fallo al calcular raiz en modulo X", e);
}

Ejemplo de relanzar la misma excepción tras registrar:

try {
    System.out.println(Calculadora.raiz(-9.0));
} catch (IllegalArgumentException e) {
    System.out.println("Log: argumento invalido detectado");
    throw e; // se propaga la misma excepcion
}
17. ¿En qué consiste que una excepción sea la "causa" de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?
Respuesta

Que una excepción sea la causa de otra significa que una excepción “de más alto nivel” guarda dentro la excepción original que provocó el problema. Esto permite traducir el error a un contexto más útil (por ejemplo, “fallo al cargar configuración”), sin perder el detalle técnico real (por ejemplo, FileNotFoundException).

En Java se hace pasando la excepción original como segundo parámetro al constructor (new X(mensaje, causa)) o usando initCause. Así, el manejador superior puede consultar getCause() y se conserva una cadena completa de fallos.

Cuando una excepción se imprime (por ejemplo, con printStackTrace()), si tiene causa, normalmente se muestra también, con un texto tipo “Caused by: ...” y la traza asociada. Eso hace más fácil entender qué fallo profundo originó el problema visible arriba.

class ConfigException extends Exception {
    public ConfigException(String msg, Throwable cause) {
        super(msg, cause);
    }
}

class ConfigLoader {
    public static String cargar(String ruta) throws ConfigException {
        try (java.io.BufferedReader br = new java.io.BufferedReader(new java.io.FileReader(ruta))) {
            return br.readLine();
        } catch (java.io.IOException e) {
            throw new ConfigException("No se pudo cargar la configuracion desde: " + ruta, e);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            System.out.println(ConfigLoader.cargar("no_existe.cfg"));
        } catch (ConfigException e) {
            e.printStackTrace(); // mostrara la causa como "Caused by"
        }
    }
}
-->