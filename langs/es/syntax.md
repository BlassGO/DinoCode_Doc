## Estructura
> DinoCode utiliza bloques de código guardados en memoria. Estos bloques se pueden considerar como funciones.
  * **Bloques**
    
    Los bloques tienen un identificador único (`TAG`). Esta etiqueta siempre debe iniciar con dos puntos `:`, y puede tener cualquier carácter alfanumérico y guiones bajos (a-z, A-Z, 0-9, _).
    ```shell
    :esta_es_una_etiqueta
    ```
    Cada `TAG` separa una sección del código, y puede ser llamado (en diferentes contextos) durante la ejecución.
       > * Los espacios en blanco antes del `TAG` no están permitidos.
       > * `TAG`s duplicados simplemente se sobreescribirán.
       > * Una sección termina cuando un nuevo `TAG` aparece.
       > * Una sección solo puede ser llamada después de su definición, las secciones no pueden ser llamadas desde una línea que está antes de su `TAG`.
    
    Todas las secciones se mantienen en memoria a excepción de `:main`, que se ejecuta directamente en tiempo de ejecución y permite controlar las demás secciones.
       > Considerando esto, la estructura estándar de un DinoScript se vería así:
      ```javascript
      :custom_tag

      :custom_tag_2

      :custom_tag_3

      :main
         Imprimir "Bienvenido!"
      ```

     
## Comentarios
> Los comentarios son texto que debe ignorarse (no interpretarse).
  * **Comentario simple de una línea** (#Comentario)
    ```shell
    #Simplemente ignorar

    Mensaje Con "test" #Simplemente ignorar 
    ```
  * **Comentario Multi-Línea** (#* Líneas *#)
    ```
    #*  Simplemente ignorar
    y esto
    y esto  *#
    ```
    ```
    #*
     Línea 1
     Línea 2
     Línea 3
    *#
    ```
    > **ADVERTENCIA:** No es posible contener una sección `TAG` en un comentario multi-línea, una vez que secciones anteriores ya han sido definidas, esto se debe a que las Secciones `TAG` no se procesan y solo se guardan en memoria.
       >
       > Esto funcionará:
          #*
          :Ignorar_este_tag
          code
          code
          *#
          :NO_ignorar_este_tag
       > Esto también:
          #:Ignorar_este_tag
          :NO_ignorar_este_tag
       >
       > Pero esto NO funcionará como se esperaría, en su lugar `#*` se guardará en memoria dentro de `:tag_previo` y `code code *#` se guardará en el segundo TAG (`:Este_tag_NO_se_ignorara`).
          :tag_previo
          #*
          :Este_tag_NO_se_ignorara
          code
          code
          *#
          :otro_tag

## Tipos de datos
> Para definir un `DATO` es necesario guardarlo en una `Variable`, las Variables en DinoCode no requieren una declaración previa como en muchos lenguajes, basta con asignarle un valor a cualquier Variable.
   > ```javascript
   > Definir VARIABLE Con DATO
   > ```
   > Si se desea limpiar una Variable se puede usar esta lógica:
   > ```javascript
   > Definir VARIABLE Con
   > ```
> Se soportan 5 tipos de DATOS:
>
> |          DATO         |                          EJEMPLO                         |
> |:---------------------:|:--------------------------------------------------------:|
> |         Entero        |                     Definir X con 256                    |
> | Flotante<br>(Decimal) |                    Definir Y con 7.324                   |
> |         Cadena        |               Definir HUH con "Hola mundo!"              |
> |       Booleanos       | Definir PRUEBA con VERDADERO<br>Definir PRUEBA con FALSO |
> |        Arreglos       |      Definir LISTA con "Primero" "Segundo" "Tercero"     |
>
> **NOTA:** Los Arreglos en DinoCode empiezan desde la posición 1 y no desde la 0 como en otros lenguajes, esto facilita la lógica de uso.

## Argumentos
> Los argumentos son parámetros que se desean enviar a `Funciones Nativas` o Secciones `TAG`.

> Se admiten dos tipos de argumentos:
> * [Argumentos Literales](#delimitador) de comillas dobles.
> ```javascript
> Imprimir "Hola mundo!"
> ```
> * [Argumentos Expresivos](#expresiones).
> ```javascript
> Imprimir % 5 + 5 %
> ```

> Cuando se envían Argumentos a Secciones `TAG`, estos se almacenan en un Arreglo que facilita su uso y ordenamiento. Este Arreglo comparte el mismo nombre del identificador de la Sección, es decir, su `TAG`.
> ```javascript
> :Huh
>    Imprimir Huh[1]
>    Imprimir Huh[2]
> 
> :main
>    Huh "Argumento Literal 1" "Argumento Literal 2"
> ```
>
> Algunas Funciones Nativas de DinoCode soportan el uso de [CONECTORES](#conectores) , los conectores también pueden usarse al enviar Argumentos a las Secciones `TAG`.
> 
> **->** Cuando se usan Conectores Nativos, el nombre del Arreglo no respetará el nombre del Conector si este usó otro idioma como Español, su nombre estándar será en Inglés.
> ```javascript
> :Huh
>    Imprimir Huh[1]
>    Imprimir Huh[2]
>    Imprimir With[1]
> 
> :main
>    Huh "Argumento Literal 1" "Argumento Literal 2" Con "Argumento del Conector 1"
> ```

## Expresiones
> Las expresiones son referencias de código especiales que tienen una `Resolución`, es decir, una expresión producirá un resultado que puede ser más pequeño o extremadamente más grande que la expresión inicial.

<!-- tabs:start -->

<!-- tab:%Expresión% -->
Las expresiones porcentuales `% %` se usan para dar un procesamiento adicional a `números`, `cadenas de texto` o `variables` y obtener su resultado.

**->** Adicionalmente, son las únicas que pueden acceder a Variables externas definidas dentro del programa principal.

### Variables
> Por defecto, para hacer referencia a una Variable no se requiere de ninguna Expresión adicional:
> ```javascript
> Definir NOMBRE con "BlassGO"
> Mensaje "Información" Con NOMBRE
> ```
> 
> Sin embargo, también es posible obtener el valor de una Variable dentro de un [Argumento Literal](#delimitador) usando `Expresiones` y así construir información más compleja.
> ```javascript
> Definir NOMBRE con "BlassGO"
> Mensaje "Información" Con "Nombre del usuario: %NOMBRE%"
> ```
> 
> La concatenación de Variables está permitida dentro de una Expresión.
> ```javascript
> Definir NOMBRE con "Blass"
> Definir APELLIDO con "GO"
> Mensaje "Información" Con "Nombre del usuario: % NOMBRE APELLIDO %"
> ```
> 
> Además, se pueden concatenar nuevas cadenas dentro de una misma Expresión.
> ```javascript
> Definir NOMBRE con "Blass"
> Definir APELLIDO con "GO"
> Mensaje "Información" Con "Nombre del usuario: % NOMBRE " y " APELLIDO %"
> ```

### Operadores aritméticos
> Como se detalla en [Tipos de datos](#tipos-de-datos) se soportan números tanto enteros como decimales, esto implica que si el Resultado de una Expressión porcentual pertenece a cualquiera de estos dos, se podrá usar sin la necesidad de encerrarlo en un [Argumento Literal](#delimitador) 
> 
> ```javascript
> Mensaje "Resultado" Con % 5.6 + 265 %
> ```
> 
> | Signo |              Operación             |     Ejemplo     |
> |:-----:|:----------------------------------:|:---------------:|
> |   +   |                Suma                |     % 5+5 %     |
> |   -   |                Resta               |     % 10-5 %    |
> |   *   |           Multiplicación           |   % 7.8 * 67 %  |
> |   /   |              División              |     % 3/2 %     |
> |   //  | División entera<br>(Sin decimales) |     % 3//2 %    |
> |   **  |            Potenciación            |    % 3**21 %    |
> |  ( )  |             Agrupación             | % (3//2+4)**4 % |

<!-- tab:$(Expresión) -->
Las expresiones evaluadas o de llamado `$( )` se usan para llamar cualquier Función existente, esto incluye a `Funciones Nativas de AHK`, `Funciones Nativas de DinoCode` y Secciones `TAG`.

**->** A diferencia de las Expresiones Porcentuales, el resultado de las Expresiones Evaluadas se suele considerar `LITERAL`, generalmente no es necesario encerrarlas en un [Argumento Literal](#delimitador).

**->** Si se usa esta Expresión desde una Expresión Porcentual `% %` o similares, es necesario preveer el tipo de `DATO` de salida. Las Expresiones Porcentuales son más sensibles, y si `$( )` produce una CADENA, se espera que esté obligatoriamente entre comillas.

>
> En este ejemplo se usa la función nativa de AHK `StrLen` para obtener la longitud de la cadena y almacenarla en la variable `LONGITUD`.
> ```javascript
> Definir LONGITUD con $(StrLen "Cuántos carácteres tiene esta cadena?")
> Mensaje "Información" Con "La cadena tiene una longitud de: %LONGITUD%"
> ```

<!-- tabs:end -->


## Contadores
> Suman o restan una sola unidad (1), a cualquier variable numérica. Son acortadores que ahorran el planteamiento de una operación matemática completa.
>

> **Contador ascendente:**
> ```javascript
> :MAIN
>    Definir X Con 1
>    X++
>    Imprimir X
> ```
> El anterior ejemplo sería equivalente a:
> ```javascript
> :MAIN
>    Definir X Con 1
>    Definir X Con % X + 1 %
>    Imprimir X
> ```

> **Contador descendente:**
> ```javascript
> :MAIN
>    Definir X Con 1
>    X--
>    Imprimir X
> ```
> El anterior ejemplo sería equivalente a:
> ```javascript
> :MAIN
>    Definir X Con 1
>    Definir X Con % X - 1 %
>    Imprimir X
> ```



## Condicionales
> Los bloques condicionales, son pedazos de código de una o varias líneas, que únicamente se ejecutarán si la condición impuesta es verdadera.
>
> **->** En DinoCode se utiliza el indentado (mejor conocido como sangría o margen izquierdo) para determinar cuáles líneas son parte del bloque condicional, tomando de referencia el inicio de la condición.
>
> ```javascript
> Si <condición>
>    línea condicional
>    línea condicional
> línea fuera de la condición
> ```

> Para determinar si una condición es `VERDADERA` o `FALSA`, el intérprete resuelve la condición como una Expresión Porcentual `% %` y analiza el resultado siguiendo esta lógica:
>
> |         **Resultado**         | **VERDADERO** | **FALSO** |
> |:-----------------------------:|:-------------:|:---------:|
> | Cualquier valor distinto de 0 |       X       |           |
> |               0               |               |     X     |
> |          Nulo (vació)         |               |     X     |


> 
> * **Operadores Relacionales**
> 
>    Establecen si existe algún tipo de relación entre dos `DATOS`.
>    
>    | **Operador** | **Ejemplo** |                                    **Relación**                                    |
>    |:------------:|:-----------:|:----------------------------------------------------------------------------------:|
>    |      ==      |     x==y    |                            x es completamente igual a y                            |
>    |       =      |     x=y     | x es igual a y <br>(Si son CADENAS, no se distingue entre Mayúsculas o Minúsculas) |
>    |      !=      |     x!=y    |                                 x es diferente de y                                |
>    |       >      |     x>y     |                                  x es mayor que y                                  |
>    |       <      |     x<y     |                                  x es menor que y                                  |
>    |      >=      |     x>=y    |                              x es mayor o igual que y                              |
>    |      <=      |     x<=y    |                              x es menor o igual que y                              |
>    |      ~=      |     x~=y    |      Cuando y es un patrón Regex,<br>se comprueba si el patrón coincide con x      |

> 
> * **Operadores lógicos**
> 
>    Principalmente usados para unir varias Condiciones como una sola Condición.
>    
>    | **Operador** |    **Ejemplo**   |                                      **Explicación**                                      |
>    |:------------:|:----------------:|:-----------------------------------------------------------------------------------------:|
>    |    && AND    |  (x=y) && (x>4)  | Si (x=y) es Verdadero **Y** (x>4) también es Verdadero,<br>toda la condición es Verdadera |
>    |    \|\| OR   | (x=y) \|\| (x>4) |     Si (x=y) es Verdadero **O** (x>4) es Verdadero,<br>toda la condición es Verdadera     |
>    |     ! NOT    |      !(x=y)      |                   Si (x=y) es Verdadero, invertirlo a FALSO o viceversa                   |


> En este ejemplo, se comprueba si `X` es igual a `5`, como la condición se cumple, se imprime `X`.
> ```javascript
> :MAIN
>    Definir X Con 5
>    Si X=5
>       Imprimir X
> ```
> En este ejemplo, se comprueba si `CADENA` es igual a `Hello world`, como la condición NO se cumple, no se imprime nada.
> ```javascript
> :MAIN
>    Definir CADENA Con "H u h"
>    Si CADENA="Hello world"
>       Imprimir CADENA
> ```
> En este ejemplo, se comprueba si `X` es igual a `5` y si `CADENA` es igual a `Hello world`, como esta segunda no se cumple, toda la condición es FALSA, por ende, no se imprime nada.
> ```javascript
> :MAIN
>    Definir X Con 5
>    Definir CADENA Con "H u h"
>    Si (x=5) && (CADENA="Hello world")
>       Imprimir X
>       Imprimir CADENA
> ```

> 
> * **Si - SiNo**
> 
>   `SiNo` establece un bloque opcional, que se ejecutará solo si la condición del anterior bloque falló.
>
>     ```javascript
>     Si <condición>
>        línea condicional
>        línea condicional
>     SiNo
>        línea condicional
>        línea condicional
>     línea fuera de la condición
>     ```
>    
>    Además, `SiNo` puede iniciar una nueva condición, pudiendo crear una secuencia de varios bloques condicionales, en donde solo uno de ellos se ejecutará.
>
>     ```javascript
>     Si <condición>
>        línea condicional
>        línea condicional
>     SiNo Si <condición>
>        línea condicional
>        línea condicional
>     SiNo Si <condición>
>        línea condicional
>        línea condicional
>     SiNo
>        línea condicional
>        línea condicional
>     línea fuera de la condición
>     ```

> En este ejemplo, solo la condición del tercer bloque se cumple, por tanto, solo ese bloque será ejecutado.
> 
> **->** Aunque la primera condición es verdadera, tiene un operador lógico de negación `!`, que invierte su resultado a FALSO.
> ```javascript
> :MAIN
>    Definir X Con 5
>    Definir CADENA Con "H u h"
>    Si !(X=5)
>       Imprimir X
>    SiNo Si (CADENA="Hello world")
>       Imprimir CADENA
>    SiNo Si (X=5) && (CADENA="H u h")
>       Imprimir X 
>       Imprimir CADENA
>    SiNo
>       Imprimir "No se encontro nada"
> ```

## Bucles
> Son un tipo de bloque [CONDICIONAL](#condicionales), con la diferencia de que su bloque se ejecutará cuantas veces sea necesario, hasta que su condición sea FALSA.
>
> ```javascript
> Mientras <condición>
>    línea condicional
>    línea condicional
> línea fuera del bucle
> ```

> En este ejemplo, se comprueba si `X` es igual a `5`, como la condición se cumple siempre, se creará un bucle infinito, es decir, el Script se quedará atrapado dentro del bucle y no continuará.
>
> **->** Cuando se definen condiciones para bucles, se debe analizar cuidadosamente la lógica de la condición.
> ```javascript
> :MAIN
>    Definir X Con 5
>    Mientras X=5
>       Imprimir X
>    Imprimir "Fuera del bucle"
> ```

> En este ejemplo, se comprueba si `X` es menor o igual a `5`, de tal forma que si el valor de `X` excede a `5`, la condición será FALSA y terminará el bucle.
> 
> **->** Para asegurarnos de que el valor de `X` incremente, se puede usar el operador `++`, el cual suma `1` a cualquier variable.
>
> ```javascript
> :MAIN
>    Definir X Con 1
>    Mientras X<=5
>       Imprimir X
>       X++
>    Imprimir "Fuera del bucle"
> ```


> DinoCode soporta un bucle inverso `Hasta`, el cual en lugar de mantener el bucle mientras la condición sea VERDADERA, lo mantiene mientras esta sea FALSA.
>
> ```javascript
> :MAIN
>    Definir X Con 0
>    Hasta X=5
>       X++
>       Imprimir X
>    Imprimir "Fuera del bucle"
> ```

> * **Romper**
> 
>   Romper tiene un valor exclusivo para bucles, permitiendo detener el bucle desde dentro de su bloque.
>
> ```javascript
> :MAIN
>    Definir X Con 1
>    Mientras VERDADERO
>       Si X<=5
>          Imprimir X
>       SiNo
>          Romper
>       X++
>    Imprimir "Fuera del bucle"
> ```

> **->** Al usar Expresiones adicionales, se debe considerar que solo las Expresiones de llamado `$( )` se mantienen en cada iteración. Las Expresiones Porcentuales `% %` se resuelven en la primera lectura de línea, manteniendo únicamente su Resolución inicial pero NO la Expresión como tal, para preservarla, se debe situar dentro de `$( )`.
> ```javascript
> :CHECK
>    Si (CHECK[1]<=CHECK[2])
>       Retornar VERDADERO
>    SiNo
>       Retornar FALSO
> :MAIN
>    Definir X Con 1
>    Mientras $(CHECK X 5)
>       Imprimir X
>       X++
>    Imprimir "Fuera del bucle"
> ```



## Funciones
> Las Secciones `TAG` de DinoCode pueden trabajar de dos modos:
  * **Como Función**
     
     Este llamado se asume como `LOCAL`, es decir, trabajará en un espacio separado que no afectará al bloque desde el cual se llamó. Por tanto, cualquier `DATO` definido durante su ejecución se limpiará al terminar. Este es el llamado por defecto de cualquier Sección.

     > En este ejemplo, se llama a `:SECCION_LOCAL` desde `:MAIN` y se imprime el valor de `X` definido localmente, después se intenta hacer una segunda impresión del mismo valor de `X`, sin embargo, esta segunda impresión generará un Error, ya que `X` no existe dentro de `:MAIN` y su valor desapareció al concluir la ejecución de `:SECCION_LOCAL`.
     > ```javascript
     > :SECCION_LOCAL
     >    Definir X con 7.8
     >    Imprimir X
     > 
     > :MAIN
     >    SECCION_LOCAL
     >    Imprimir X
     > ```
     > 
     > En este ejemplo, cada impresión de `X` tendrá un valor diferente, correspondiente al definido en su Sección.
     > ```javascript
     > :SECCION_LOCAL
     >    Definir X con 7.8
     >    Imprimir X
     > 
     > :MAIN
     >    Definir X con 1000
     >    SECCION_LOCAL
     >    Imprimir X
     > ```

     Una de las principales ventajas de las funciones es que pueden recibir Argumentos (Parámetros), y retornar una Resolución.
     > En este ejemplo se le envián dos Argumentos numéricos a `:SUMA` desde `:MAIN` y se imprime su sumatoria.
     > ```javascript
     > :SUMA
     >    Imprimir % SUMA[1] + SUMA[2] %
     > 
     > :MAIN
     >    SUMA 1000 7.8
     > ```
     >
     > En este ejemplo se usa `:SUMA` únicamente para [RETORNAR](#retornar) la sumatoria de los dos Argumentos numéricos que recibió y este Resultado Retornado, se almacena en la Variable `RESULTADO` de `:MAIN` para procedentemente imprimirlo.
     > ```javascript
     > :SUMA
     >    Retornar SUMA[1] + SUMA[2]
     > 
     > :MAIN
     >    Definir RESULTADO Con $(SUMA 1000 7.8)
     >    Imprimir RESULTADO
     > ```
     

  * **Como Extensión**
     
     Este llamado se asume como una extensión del bloque desde el cual se llamó, es decir, NO trabajará localmente, en su lugar, los `DATO`s son compartidos y cualquier modificación a los mismos afectará a ambos bloques. Esto también aplica para nuevos datos definidos durante su ejecución.

     En contraparte, no soporta Argumentos.
     > ```javascript
     > :DATOS
     >    Definir Y con 10
     >    Definir Z con 2.5
     > 
     > :MAIN
     >    Definir X con 66
     >    Sección DATOS
     >    Imprimir % (X+Y)*Z %
     > ```

   * **Global**
     > Una Variable puede ser globalizada, de tal forma que NUNCA trabaje localmente (puede ser modificada o definida desde dentro de cualquier función).
     > ```javascript
     > GLOBAL Variable Variable ...
     > ```
     > En este ejemplo, `X` se define como una variable `GLOBAL`, y por tanto, puede ser usada incluso después de la ejecución de la función.
     > ```javascript
     > :SECCION_LOCAL
     >    Global X
     >    Definir X con 7.8
     >    Imprimir X
     > :MAIN
     >    SECCION_LOCAL
     >    Imprimir X
     > ```


## Retornar
Retornar es una acción especial usada principalmente en Funciones, permite que una Sección devuelva un `DATO` y finalice su ejecución. En DinoCode esta acción utiliza la misma lógica de las [Expresiones Porcentuales](#expresiones), pudiendo retornar números, cadenas o variables, pero adicionalmente soporta la devolución de Arreglos completos.

> ```javascript
> :MULTI
>    Retornar "Resultado: " MULTI[1] * MULTI[2]
> 
> :MAIN
>    Imprimir $(MULTI 1000 7.8)
> ```
> 
> Como se mencionó en [Argumentos](#argumentos), los parámetros que se pasan a las Funciones se convierten en Arreglos, es posible aprovechar esto para apilar más información y retornar un Arreglo más grande.
> ```javascript
> :DATOS
>    Definir DATOS[3] con "Dato adicional"
>    Retornar DATOS
> 
> :MAIN
>    Definir INFO con $(DATOS "Blass" "GO")
>    Imprimir INFO[1]
>    Imprimir INFO[2]
>    Imprimir INFO[3]
> ```
>
> **TIP:**  Se puede usar Retornar dentro de una Expresión `$( )` y aunque es equivalente a usar una Expresión Porcentual (en la mayoría de los casos), puede ser útil.
> ```javascript
> :MAIN
>    Definir X con 1000
>    Definir Y con 7.8
>    Imprimir $(Retornar "Resultado: " X * Y)
> ```

## Conectores
> Los conectores en DinoCode son un concepto que permite crear `Sentencias Naturales` (cercanas al lenguaje natural humano). Funcionan como una unión hacia la `Función Principal` y agrupan sus propios Argumentos.
> 
> **->** Los conectores pueden tener cualquier carácter alfanumérico y guiones bajos (igual que los `TAG`).

> Conectores Nativos:
> 
> | **Conector** |   **Soporte**  | **Lista de Apodos** |
> |:------------:|:--------------:|:-------------------:|
> |      Con     | Inglés/Español |       =, With       |
> |      En      | Inglés/Español |          In         |
> |     Hacia    | Inglés/Español |       to, unto      |
>

> Además de los Conectores Nativos, es posible agregar nuevos conectores:
>
> ```javascript
> :TAG -> Conector, Conector, Conector, ...
> ```
> En este ejemplo, se define un nuevo Conector `Restar` para la función `Sumar`, de esta forma, la función puede generar diferentes resultados según los Conectores involucrados durante su llamado. 
> 
> ```javascript
> :Sumar -> Restar
>    Si Restar[1]
>       Mensaje "Resultado" Con % Sumar[1] + With[1] - Restar[1] %
>    SiNo
>       Mensaje "Resultado" Con % Sumar[1] + With[1] %
> 
> :main
>    Sumar 10 Con 5
>    Sumar 2.3 Con 100 Restar 90
>
> ```
> Es posible sobreescribir Conectores Nativos. 
> ```javascript
> :Sumar -> Con, Restar
>    Si Restar[1]
>       Mensaje "Resultado" Con % Sumar[1] + Con[1] - Restar[1] %
>    SiNo
>       Mensaje "Resultado" Con % Sumar[1] + Con[1] %
> 
> :main
>    Sumar 2.3 Con 100 Restar 90
>
> ```
> 
> **->** Este es un ejemplo muy práctico del uso de Conectores, a partir de la Función Nativa `SubStr`, se crea una nueva Función simplificada (`Extraer`).
> 
> ```javascript
> :Extraer -> Desde, Hasta, Usando
>    Retornar "$(SubStr Usando[1] Desde[1] Hasta[1])"
> 
> :main
>    Definir CADENA Con "Hello world!"
>    Mensaje "Resultado" Con $(Extraer desde 1 hasta 5 usando CADENA)
> 
> ```

## Apodos
> Los apodos permiten que una misma Función pueda ser llamada con diferentes nombres.
> 
> **->** No tienen restricciones de carácteres, es decir, a pesar de que el nombre de una Función no permita carácteres especiales, sus Apodos sí los pueden tener.
>
> ```javascript
> :TAG -> TAG = [Apodo_1, Apodo_2, Apodo_3]
> ```

> En este ejemplo, se le asignan el apodo `+`  a la función `Sumar`, permitiendo un llamado con cualquiera de los dos nombres.
> 
> ```javascript
> :Sumar -> Sumar = [+]
>    Mensaje "Resultado" Con % Sumar[1] + With[1] %
> 
> :main
>    Sumar 2.3 Con 100
>    + 2.3 Con 100
>
> ```
> 
> Los Conectores también soportan apodos, incluso pueden tener el mismo apodo de la Función principal.
> 
> **->** Si se define un apodo para un Conector Nativo, usando su nombre estándar (Inglés), se soportaran sus apodos nativos en otros idiomas, además, de los nuevos apodos.
> 
> ```javascript
> :Sumar -> Sumar = [+], With = [+]
>    Mensaje "Resultado" Con % Sumar[1] + With[1] %
> 
> :main
>    Sumar 2.3 Con 100
>    + 2.3 + 100
>
> ```
>
> Usando esta lógica, es posible brindar soporte a múltiples idiomas (aunque se recomienda no abusar de su uso, ya que representan un gasto adicional durante el reconocimiento de la Función).
> ```javascript
> :Sumar -> Sumar = [add, Добавлять, 追加], With = [к, と]
>    Mensaje "Resultado" Con % Sumar[1] + With[1] %
> 
> :main
>    Sumar 2.3 Con 100
>    Add 2.3 With 100
>    Добавлять 2.3 к 100
>    追加 2.3 と 100
>
> ```

## Delimitador
> Por defecto, las comillas dobles `" "` agrupan los parámetros que deben tomarse como texto literal (por lo que si desea agregar una comilla doble literal dentro de un grupo de comillas dobles, debe [Escaparla](#escape)).
```javascript
Mensaje "T í t u l o" Con "Huh!"
```
   > * Dentro de un grupo de comillas aún es posible usar Expresiones: `$( )`, `% %`.
   > * La Resolución de cualquier Expresión dentro de un grupo de comillas se contendrá, incluso si estos resultados contienen múltiples comillas dobles no romperán la agrupación.

## Escape
> Permite "ocultar" carácteres, de tal forma que aunque el carácter tenga algún valor especial para el intérprete, se ignore. Su uso se centra exclusivamente en [Argumentos Literales](#delimitador) 
  * **Escape de un solo carácter**
    > Cualquier carácter precedido por una barra invertida `\` será escapado.
    >
    > **->** Es posible cambiar el carácter de escape `\` por cualquier otro, por ejemplo, si se desea cambiarlo por `/`, bastaría con: `Escape /`
    >
    > ```javascript
    > Mensaje "Prueba" Con "Carácteres escapados: \" \% \$"
    > ```
    > Si el carácter escapado es alfabético, se considerará como un carácter especial.
    > ```javascript
    > Mensaje "Prueba" Con "Primera línea\nSegunda Línea\n\tTercera línea con Tab"
    > ```
    > **ADVERTENCIA**: Existe una diferencia entre usar un carácter escapado alfabético en Minúsculas y uno en Mayúsculas. En distros Linux se suele usar el carácter `\n` conocido como "linefeed" para representar un salto de línea, sin embargo, en sistemas operativos Windows, se prefiere una combinación de "retorno de carro"-->`\r`, junto a "linefeed"-->`\n`. Por comodidad en DinoCode el uso de `\n` NO representa realmente un "linefeed", sino la combinación `\r\n`. Para usar un "linefeed" real, se puede persistir cambiando el carácter a Mayúscula, es decir, `\N`.

## Hilos
> Permite ejecutar Secciones `TAG` asincronicamente, es decir, la sección usará un espacio de ejecución independiente.
> 
> **->** Los `HILOS` se consideran extensiones del bloque, es decir, los datos son compartidos (NO LOCAL).
> 
| **OPCIONES** | **EXPLICACIÓN** |
|:---:|:---:|
| Hilo TAG **TIEMPO** | - Crea un HILO nuevo y establece un temporizador (milisegundos) que se reiniciará constantemente.<br>- Si el TIEMPO es negativo, el temporizador solo funcionará una vez y suspenderá el HILO (sin eliminarlo). |
| Hilo TAG **Off** | - Suspende el temporizador del HILO. |
| Hilo TAG **On** | - Activa el temporizador si fue suspendido. |
| Hilo TAG **Delete** | - Elimina completamente el HILO. |                                                                                                                                                                           |

> En este ejemplo, la sección `ALERTA` se inicia como un `HILO` secundario que será llamado cada `1000` milisegundos (1s), y al no detener el flujo del Script, da paso al Mensaje del HILO principal (`MAIN`), después, al segundo, se mostrará un segundo mensaje que corresponde al nuevo HILO creado, y se elimina dicho HILO (Evitando seguir con el llamado cada 1000 milisegundos).
> ```javascript
> :ALERTA
>   Msg "Segundo" con "Mensaje del HILO secundario"
>   Hilo ALERTA Delete
> :MAIN
>   Hilo ALERTA 1000
>   Msg "Primero" con "Mensaje del HILO principal"
> ```
>
> Este es un ejemplo mucho más práctico del uso de HILOS, la sección `INCREMENTAR` actúa como un contador que incrementará cada `100` milisegundos y destruirá el HILO secundario únicamente cuando se alcancen 100 unidades.
> ```javascript
> :INCREMENTAR
>   CONTADOR++
>   Imprimir CONTADOR
>   Si (CONTADOR >= 100)
>      Hilo INCREMENTAR Delete
>      Msg "Hilo 2" con "El contador ya NO incrementará."
> :MAIN
>   Definir CONTADOR con 0
>   Hilo INCREMENTAR 100
>   Msg "Hilo 1" con "Este mensaje no impide el incremento del contador."
> ```

## Evaluador
> Permite `evaluar` cadenas de texto como expresiones porcentuales `% %`.
> 
> **->** Se soportan múltiples Expresiones en un mismo llamado, cada una debe estar separada por una coma `,`.
> 
> **->** Retorna un Arreglo, en donde cada elemento corresponde a cada Expresión evaluada.
> 
> ```javascript
> Eval "Expresión 1, Expresión 2, Expresión 3, ..."
> ```

> Para incluir una expresión `$( )` dentro de la evaluación, se deben escapar los carácteres especiales `\$( )`.
> ```javascript
> :MAIN
>    Definir HUH Con "Huh?"
>    Definir RESULTADOS Con $(Eval "5+10, (2*5)**6, HUH, \$(Input)")
>    Imprimir RESULTADOS[1]
>    Imprimir RESULTADOS[2]
>    Imprimir RESULTADOS[3]
>    Imprimir RESULTADOS[4]
>
> ```


## Interfaz gráfica
> Permite aprovechar la capacidad de AHK de crear interfaces gráficas de usuario (GUI).
>
> **->** Nativamente solo se soporta la función `Gui`, etiquetas o funciones especiales existentes para el control de GUI en AHK, podrían no estar disponibles.


> **->** A continuación, se muestran algunos ejemplos básicos, para mayor información [PRESIONE AQUÍ](https://www.autohotkey.com/docs/v1/lib/Gui.htm ':ignore').
> 
> En este ejemplo, se construye una GUI con un identificador único `custom3`, en donde se añade un botón `PRESIONAME` que está vinculado a la sección `OKAY` (la cual será llamada ante cualquier evento dentro del botón). Finalmente, se muestra la ventana `Show` con una dimensión de `200x100` píxeles.
> ```javascript
> :OKAY
>    Mensaje "Ole" Con "Hello world!"
> :MAIN
>    Gui "custom3: Destroy"
>    Gui "custom3: Add, Button, gOKAY, PRESIONAME"
>    Gui "custom3: Show, w200 h100, Mi Ventana"
>    Esperar_ventana "Mi Ventana"
>
> ```
> <p align="center"> <img width="300" height="183" src="https://raw.githubusercontent.com/BlassGO/DinoCode_Doc/main/images/gui.jpg"></p>
> 
> En este ejemplo, se construye una GUI con un identificador único `MyGUI`, en donde se añade un EditBox (que servirá como entrada de datos) con una Variable Global `CONTENIDO`, y un botón `ENVIAR` que está vinculado a la sección `RECIBIR` (la cual solicitará a la GUI actualizar cualquier Variable definida). Finalmente, se muestra la ventana `Show` con una dimensión de `200x100` píxeles.
> ```javascript
> :RECIBIR
>    Gui "MyGUI: Submit, NoHide"
>    Mensaje "Recibido" Con CONTENIDO
> :MAIN
>    Gui "MyGUI: Destroy"
>    Gui "MyGUI: Add, Edit, vCONTENIDO"
>    Gui "MyGUI: Add, Button, gRECIBIR, ENVIAR"
>    Gui "MyGUI: Show, w200 h100, Mi Ventana"
>    Esperar_ventana "Mi Ventana"
>
> ```
> <p align="center"> <img width="300" height="183" src="https://raw.githubusercontent.com/BlassGO/DinoCode_Doc/main/images/gui2.jpg"></p>



## Modulación
> DinoCode permite la importación de librerías externas, dichas librerías pueden definir nuevas funciones o variables.
> ```javascript
>  Importar Extensión.config "Carpeta\Extensión 2.config"
> ```


## JavaScript/VBS
   DinoCode soporta hosteo de lenguajes externos pertenecientes a Active Scripting (ActiveX), a través de bloques específicos que comparten valores entre sí.

   > * **Soportado:**
   >    * JavaScript (JS)
   >    * VBScript (VBS)
   >
   > * **Características:**
   >     *  Los bloques tienen acceso a las Variables/Arreglos definidos en DinoCode.
   >     * Las nuevas Variables que se definan dentro del bloque serán compartidas con DinoCode.
   >     * Los DATOS de las Variables/Arreglos compartidos se mantendrán sincronizados.
   >
   > * **Observaciones:**
   >    * Las nuevas Variables/Arreglos a compartir con DinoCode, NO deben ser declaradas formalmente (`let, dim, ..`), de lo contrario trabajaran localmente dentro del bloque y su valor desaparecerá.
   >    * Actualmente, solo JavaScript puede compartir nuevos Arreglos, VBScript solo Variables, pero los Arreglos definidos dentro de DinoCode se pueden compartir con cualquier bloque.


   ```javascript
   :main
        Definir HUH Con "Desde DinoCode"
        Definir NUMEROS Con 1 2 "Muéstrame" 4

        Usar JavaScript
            arreglo_java = [1, 2, 3, 4]
            shell = new ActiveXObject("WScript.Shell")
            shell.Popup("JavaS---> " + HUH)              //Muestra "Desde DinoCode"
            HUH = "Desde JavaScript"                     //Actualiza HUH
        Mensaje "Actualizado" Con HUH                    #Muestra "Desde JavaScript"
        Mensaje "Nuevo" Con arreglo_java[2]              #Los Arreglos en JS inician desde la posición 0, entonces, mostrará "3"
    
        Usar VBScript
            RESULTADO=MsgBox (NUMEROS(3),48, "Huh")       'Muestra "Muéstrame"
            HUH="Desde VBScript"
        Mensaje "Recibido" Con " %HUH% \n Y 'RESULTADO' terminó con %RESULTADO%"
   
   ``` 

   ## Experimentales
> Las características experimentales son implementaciones que no han sido aprobadas como un estándar para DinoCode, esto implica posibles BUGs o resultados inesperados. Se recomienda no abusar de su uso, ya que bien podrían mejorarse o removerse a futuro.

  * **Apilación de bloques en una sola línea**
    > Por defecto, DinoCode utiliza una interpretación de línea a línea. El delimitador `;` permite apilar varias líneas en una sola línea, sin embargo, este no funciona como una separación de línea, sino que alerta al intérprete que desde ese punto se debe resolver cada segmento en diferentes subprocesos, respetando siempre la sincronía de cada segmento, pero NO necesariamente la de sus Expresiones.
    > 
    > **->** No se soportan condicionales, bucles, y en general, bloques que usen la indentación.
    >
    > Su uso es ideal, siempre y cuando, las Expresiones Porcentuales `% %` existentes en cada Segmento, NO hagan referencia a `DATOS` definidos dentro de la apilación, ya que estas se resuelven mucho antes del procesamiento de cada Segmento
    >
    >
    >Lo siguiente NO funcionará:
    > ```javascript
    > :MAIN
    >    Imprimir $(Definir X con 66; Retornar "%X%")
    > ```
    >
    > Ejemplo funcional:
     > ```javascript
    > :MAIN
    >    Imprimir $(Definir X con 66; Retornar X)
    >    Imprimir $(Definir Y con 10; Definir Z con 2.5; Retornar "Resultado: " (X+Y)*Z)
    > ```

  * **Llamado dinámico**
    > Se pueden usar Expresiones Porcentuales `% %` dentro del nombre de una Función o Conector (durante su llamado), permitiendo el uso de nombres dinámicos o parciales.
    > 
    > ```javascript
    > :MAIN
    >    Definir huh con "Imprimir"
    >    %huh% "Hello world!"
    > ```
