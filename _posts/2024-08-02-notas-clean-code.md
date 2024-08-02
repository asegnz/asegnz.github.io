---
layout: post
title: "Notas de Clean Code, de Robert C. Martin"
date: "2024-08-02 15:45:00 +0200"
categories: notas resumen libro clean code
---

Aquí os comparto las notas del libro recomendado por Autentia que me leí hace un tiempo....

# Notas Clean Code

En este post voy a dejar todas mis notas que resumen las ideas importantes de Uncle Bob.

## 1. Prólogo

1. En el mundo del desarrollo del software, el 80% de lo que hacemos es repararlo, es decir, mantenerlo.

### 1.1. Principios Lean
1. **Seiri: (ordenado)**: convenir nombres, localización de las cosas
2. **Seiton o tidyness (sistemático)**: allá por donde lo buscas es donde tiene que estar.
3. **Seiso (cleaning)**: Borra comentarios y tira la basura que tengas en el escritorio
4. **Seiketsu (estandarización)**: Cómo mantener el espacio de trabajo (workspace) limpio
5. **Shutsuke (disciplina)**: Seguir las prácticas, voluntad para mejorar

### 1.2 Notas esenciales

* Existen dos acciones importantes para aprender *craftmanship:* tener conocimiento y trabajo duro
* Hay que entender que el después implica nunca
* El trabajo de un manager es controlar los hitos, el trabajo de un desarrollador es defender el código limpio§

### 1.3 The Primal Comundrum

* Las ñapas del pasado te van a ralentizar ¡y lo sabes!
* Es falso que haya que hacer ñapas para cumplir la entrega
* Para la próxima línea que escribas, recuerda que eres su autor y que el resto de los que te lean serán quien juzguen tu esfuerzo. Escribe como si la persona que viene después de ti fuera un asesino y supiera dónde vives.
* Somos los responsables de comunicar bien. Esto es difícil al desarrollar, pero hay que intentar hablar un lenguaje común.
* El ratio de tiempo consumido de lectura frente al de escritura es de 10 a 1. Estamos constantemente esforzándonos en leer y entender el código antiguo. **Queremos** leer código bien escrito y que sea fácil de entender

## 2. Nombres con significado

* Por favor, utiliza nombres fáciles de pronunciar, si no las discusiones sobre los términos que evoca una variable serán impronunciables
* Evita dar información de más, por ejemplo, no nombres como lista una variable que es una lista. Eso sería nombrar su implementación en lugar del concepto de colección (usa el plural). `ProductData` no aporta nada y es lo mismo que `Product`
* Usa constantes siempre que puedas (¡y nómbralas bien!)
* Ya no se necesita nombrar una variable con el nombre de su tipo p. ejemplo: `PhoneNumber phoneString` (a esto se le conoce como Hungarian Notation).
* Ni tampoco indicar que es un miembro, p. ejemplo: prefijo `m_atributo`
* Tampoco es necesario añadir un prefijo a una Interfaz, p. ejemplo: `public interface IShape` vs `public interface Shape`. Tampoco es necesario llamarlo `ShapeFactoryImpl`, con `ShapeFactory` nos vale.
* Puede tener sentido usar las variables `i` y `j` para recorrer bucles por su tradición. Por favor, evita utilizar `l` u otros nombres de variables sin significado. **La claridad es la clave**.
* Las clases e instancias deben ser nombres; acompañados de [noun phrase names](https://en.wikipedia.org/wiki/Noun_phrase#Components). Evita siempre sufijos como `Manager` o `Processor`. Las clases no deben contener ningún verbo.
* Cuando tenemos un constructor con sobrecarga, es mejor utilizar métodos estáticos tipo factoría y declarar el constructor privado.
* Di lo que significa pues significa lo que dices. No nombres variables con nombres graciosos o propios de alguna *jerga*
* Usa lenguaje común con tus compañeros. Acuña un glosario de términos si es necesario y ponedlo en práctica. Si hay un nuevo término, comunícalo y añádelo.
* Evita utilizar la misma palabra para dos propósitos distintos: hay que intentar siempre desambigüar el contexto de las cosas y saber a lo que nos referimos en todo momento.
* Sé preciso en el lenguaje que utilizas: `add` puede ser demasiado genérico y quizá te interese más decir `append` o `insert`. No son exactamente lo mismo ;)
* Si has diseñado una solución colocando un patrón: nómbralo (p. ejemplo, `AccountVisitor`). Los que lean tu código también serán desarrolladores y sabrán de qué estás hablando.
* Nunca, nunca, nunca y bajo ningún concepto escribas un prefijo común (p. ejemplo: el acrónimo del proyecto) en el nombre de tus clases. Dejarías un legado horrible y te odiarán por ello.
* Nombrar bien las cosas es un trabajo difícil que no todo el mundo llega a aprender. No tengas miedo a renombrar por el temor a que tus compañeros te echen la bronca. Si no lo haces hoy, es una deuda que se cobrará mañana y nunca se terminará de pagar.

## 3. Funciones/Métodos/Procedimientos/Subrutinas

* Se puede extraer una función larga inentendible a pequeños métodos haciéndola entendible. En la mayoría de los IDE's hay un atajo para extraer a una función desde un código fuente. Si tu IDE no lo tiene, cambia de IDE.
* Si hay un if anidado en una función... huele mal. Si el cuerpo de un if tiene más de una sentencia... también.
* Hay dos reglas de oro en las funciones:
    * deben ser pequeñas
    * deben ser incluso aún más pequeñas
* Una función debe contener como máximo tres sentencias. Si tiene más, huele muuuu mal.
* Recuerda que al extraer a funciones más pequeñas tu función grande la clave es nombrar bien qué hace la función pequeña. De esta forma dejamos el código más legible.
* Las funciones/métodos deben hacer **una sola cosa**, y deben hacerla bien.
    * es mucho más fácil de testear una función que hace una sóla cosa que permutar posibilidades y olvidarnos de un caso.
* Si observas en tu función que tienes una sección para declarar, inicializar y convertir variables, es un síntoma de que tu función hace más de una cosa. ¡Divide y vencerás!
* Una función no debe mezclar distintos niveles de abstracción, es decir, mezclar conceptos de alto nivel con implementaciones de bajo nivel. Resulta confuso y cambia el contexto para la persona que viene detrás.
* Cuidadito con utilizar un switch, ya que rompe las dos primeras letras de los principios SOLID. Se puede justificar su uso utilizando el patrón `Abstract Factory` y restringiendo la visibilidad.
* Recuerda que elegir un buen nombre para una función es una dura batalla. No dudes en dedicarle más tiempo del que requiere.
* Sé consistente con el nombre que le pones a las cosas. No puedes nombrar un día `retrieve` y otro `get`...
* El número ideal de parámetros de una función es cero. **Con tres ya te has pasado**.
* A más número de parámetros que tenga una función, más difícil es testear la misma. Pónselo fácil a tu yo futuro.
* Lo que devuelve una función es más difícil de entender que sus parámetros de entrada
* Si tienes una función void y tiene un parámetro de entrada, trata de devolverlo en la salida. De esta forma el que venga detrás de ti sabrá de forma más intuitiva qué es lo que hace.
* Si tienes un parámetro booleano para una función **lo estás haciendo mal**. Deberías separar la lógica en dos funciones debidamente nombradas.
* Una función con dos parámetros es más difícil que entender que una con sólo uno. Mientras que para un argumento una función es fácil de entender (verbo + complemento directo), con dos hay una breve pausa para tratar de entender qué hace el segundo.
* La típica función de assertEquals(expected, actual) no tiene un orden lógico.
* La típica función de assertEquals(message, expected, actual) tiene un orden catastrófico. ¿Nadie se ha equivocado pensando que el `message` era el `expected`?
* Si necesitas pasar 3 parámetros para una función, lo más probable es que puedas envolver alguno de esos parámetros en una clase.
* Siguiendo la regla anterior, en el momento de pasar una lista parametrizable, recuerda que como máximo debes pasar 3 parámetros.
* El nombre de los métodos tiene que definirse en función de la acción sobre los parámetros. `assertExpectedEqualsActual` es peor que `assertEquals`.
* Jamás tengas una función que haga más de una cosa. Y sobretodo: no escondas aquello que hace. ¿Quién no se ha encontrado algún getter de algún desalmado que hace más de lo que pensabas?
* Trata de nombrar un método de tal forma que no tengas la necesidad de mirar la firma del mismo. Es un doble*check en el que se pierde algo de contexto de lo que estabas haciendo.
* Separa los métodos que hagan cosas y los que recuperen cosas. Es confuso ver una función que devuelve algo y cuyos parámetros se hayan visto afectados.
* Es mejor devolver excepciones que códigos de error. Su gestión es más cómoda y permite jerarquías.
* En caso de captura de excepciones, al ser confuso el control de cuando va bien y cuando hay un error, se recomienda que el cuerpo del try, el cuerpo del catch y el del finally sean un método.
* Si hay un try en un método debe ser de lo primerísimo que aparezca y que no haya nada después del catch/finally.
* Si existe un enumerado `Error` con el tipo de error que estamos dando, tenemos un problema de dependencias. No tiene sentido que un cliente deba redesplegar porque se añada un nuevo campo al enumerado.
* Dijkstra decía que sólo debía haber un return, ningún goto, ningún break, etc. Bob nos comenta que tiene cierta parte de razón, pero que esto es únicamente útil en funciones que sean demasiado grandes, las cuáles tenemos que evitar.
* Ten una cobertura de tests suficiente como para aventurarte a refactorizar sin miedo.
* Las funciones son los verbos y las clases son los nombres.

## 4. Comentarios

* Los comentarios no son como Hitler. No son puramente buenos, de hecho, a veces los comentarios son una maligna necesidad. Si los lenguajes de programación fueran más expresivos o si tuviéramos el talento para expresarlo con ellos, no necesitaríamos comentarios de forma frecuente.
* El uso de comentarios se hace para compensar el fallo de expresarnos mediante el código. Mención especial a la palabra «fallo», los comentarios siempre son un fallo. Debemos tenerlos debido a no saber expresarnos pero nunca es motivo de celebración.
* Siempre que quieras poner un comentario, piensa si no puedes reescribir el código para evitarlo.
* Los comentarios mienten, no siempre pero sí frecuentemente. Los desarrolladores no solemos mantenerlos.
* La única fuente de verdad sólo se puede encontrar en el código.
* Debemos gastar toda la energía posible para intentar minimizar el uso de comentarios en el código.
* Es posible que un comentario lo debas sustituir por una función que diga más o menos lo mismo.
* El mejor comentario es aquel que puedes llegar a evitar.
* Es posible que un comentario de patrones de fecha sí que sea útil, pero debería estar en su sitio, en una clase de conversión de fechas. Quizá ubicado allí deje de tener sentido.
* Un buen comentario puede ser aquel que intente explicar una intención detrás de la implementación para que el que venga detrás la tenga en cuenta. Puede que no pienses que es la mejor solución, pero al menos se ha molestado en que la entiendas.
* A veces estás usando una librería y sus métodos no son muy claros. Ahí puedes poner un comentario de lo que se está intentando hacer.
* Cuando tienes que hacer algo "ineficiente" porque una librería funciona de aquella manera, está justificado poner un comentario para evitar el refactor del desarrollador que venga detrás.
* Usa los `TODO` pero úsalos diciendo qué falta y qué se debería proponer como solución
* Usa los `TODO` cuando no puedas hacer algo en ese preciso momento. No es una excusa para trabajar mal.
* Un getter no debe tener un comentario. Un field de una clase tampoco.
* Es estúpido tener una regla que toda función debe tener un javadoc o toda variable tener un comentario. Esto es como romper una ventana de un edificio abandonado, propiciando la `teoria de las ventanas rotas`.
* Evitar comentarios de cambios que se hacen en una clase a modo de diario de abordo. Ya tenemos git y los commits para eso, amén de ser más precisos.
* No comentes el código, tenemos un excelente control de versiones por si hubiera que hacer un revert en el futuro.
* No comentes una función sobre un comportamiento general del negocio/sistema. No comentes, pero si comentas, cíñete al alcance de la función.
* No copies y pegues texto de un RFC. Si acaso linka el RFC con el comentario y el link, pero no hace falta detallar todo el algoritmo.
* No pongas un comentario ambiguo. Si ya de por sí no deberías necesitar el comentario, no explicar bien lo que quieres decir es aún peor.
* Una función debería ser autodescriptible, sin necesidad de comentarios.
* Javadoc sólo es útil para APIs públicas. Generar javadoc que no va a ser leído es una pérdida de tiempo.



## 5. Formateo
* Formatear es importante. No se puede ignorar y se debe tratar de forma religiosa. El formato del código trata sobre comunicación y ésta es una skill de primer orden de negocio para un desarrollador.
    * Hacerlo funcionar no es una skill de primer orden de negocio.
* El estilo de código que generes hoy, deja un precedente para el mañana. ¿Te acuerdas de la `teoría de las ventanas rotas`? Puedes afectar negativamente a tener un código sostenible.
* Una clase no debería tener más de 500 líneas.
    * Lo normal debería estar siempre por debajo de 200
* Queremos que un fichero, una clase, un script, etc... tenga la misma apariencia que un periódico: el nombre debe ser simple pero autoexplicable.
    * Arriba deberían aparecer las funciones públicas y más importantes. Al final del fichero lo que carece de relevancia.
* Cada línea en blanco separa un nuevo concepto, por ejemplo, la declaración del paquete, de los imports y la firma de la clase. Respétala por el bien de la vista de todos.
* Evita comentarios de Javadoc absurdos que generan líneas sin aportar nada más.
* ¿Te has perdido alguna vez entre la herencia de clases para ver quién define una función?
    * Esto es muy frustrante porque intentas entender *qué* hace el sistema y tienes que recordar *dónde* están las piezas que lo componen.
* Los conceptos relacionados deben estar verticalmente pegados uno a otro. Conceptos que están tan íntimamente relacionados no deberían estar separados en clases o ficheros diferentes. Cómo estén de separados estén es la medida de cómo de importantes son entre sí. Así evitamos que nuestros futuros lectores tengan que estar todo el día haciendo scroll.
* Las declaraciones de variables deben estar muy cerca de su uso.
* Los atributos de una clase deben estar declaradas al principio de la clase. Declararlas en la mitad de la declaración de getters/setters es delito.
* Otra punto de vista de declarar primero las funciones públicas y luego las privadas es que podemos leer de más alto nivel a más bajo nivel. Lo ideal sería leer las funciones públicas y saber qué hace esta clase y no mirar el detalle de la función privada.
* Intenta limitar el tamaño de línea a 120, aunque se pueda a 200 no es legible. Nunca, nunca, nunca deberías hacer scroll a la derecha.
* El código de un método estático debería ir pegado al nombre de la clase. Yo no estoy muy de acuerdo con esto: es correcto que está íntimamente relacionado pero no lo veo legible.

## 6. Objetos y estructuras de datos

* Hay una razón para hacer nuestros atributos privados: para evitar que alguien dependa de ellos.
* No añadas setters sin una buena razón. Estás haciendo tus variables públicas.
* Los métodos obligan a que definas una política de acceso a los datos.
* Ley de Demeter: un módulo no debería conocer los detalles de los objetos que manipula.
    * o lo que es lo mismo: habla con amigos, no con extraños.
    * no deberías invocar a un método de un objeto tras hacer un get.
* En el código procedural es fácil añadir nuevas funcionas mientras que en el código orientado objetos es más fácil añadir nuevas clases sin cambiar los métodos.
* Evita crear un híbrido de objetos y estructura de datos.
* No añadas lógica de negocio a un Active Record (como un DTO Entidad de base de datos)

## 7. Gestión de errores
* La gestión de errores es importante, pero si oscurece la lógica es incorrecto.
* Usa excepciones en lugar de «return codes»
* Las «checked exceptions» rompen el principio «Open/Closed». Puedes usar unchecked exceptions.
* Añade logs cuando capturas excepciones, por dar algo de contexto al código del porqué se produce tal excepción.
* Un método nunca debería devolver «null». Nos estamos cargando de trabajo y de «ifs» futuros.
    * Una lista vacía es mejor que devolver «null».
* De igual manera, un parámetro de una función no debería ser tampoco «null».
* El código limpio es legible pero también debe ser robusto. No son objetivos contrapuestos.

## 8) Delimitación de dominios

* Debemos integrar de forma limpia el código de terceros
* Usa genéricos en las estructuras de datos siempre que sea posible.
* Evita devolver una estructura de datos en un getter. Es mejor que devuelvas un «getById(id..)»
* No es trabajo nuestro testear una librería de terceros. Sólo su integración.
* Puedes hacer test de una librería de terceros con el propósito de aprender sobre lo que hace dicha librería.
* En un futuro, si cambias de versión de la librería de terceros, estos tests detectarán si el comportamiento de dicha librería sigue siendo el mismo o ha cambiado.
* Si no conoces el código que se va a hacer, trabaja con interfaces con una idea alto nivel de lo que hace.
* El código en los límites de dominios necesita una clara separación y los tests definen lo que se espera. Debemos evitar que nuestro código conozca demasiado acerca de las particularidades de terceros.
* Es mejor depender de algo que tú controlas que no de algo que no controlas.
* Lo ideal es usar una capa de adaptación de nuestra "interfaz ideal" al verdadero API del tercero para minimizar el impacto.

## 9) Tests unitarios

* Las tres leyes del «Test Driven Development»:
    * No debes escribir código productivo hasta que hayas escrito un test que falle.
    * No debes escribir más que un tes unitario que sea suficiente para fallar. No compilar lo consideramos fallar.
    * No debes escribir más código productivo del suficiente para pasar el test.
* Los tests y el código productivo se escriben juntos.
* Con esta idea, escribiremos docenas de tests todos los días. Si seguimos trabajando así, aquellos tests cubrirán virtualmente todo nuestro código productivo.
* Mantén los tests limpios. Cuanto peor los hagas más susceptibles serán de cambiar con el código de producción.
    * Esto tiene un corolario: ¿recuerdas la `teoría de las ventanas rotas`? Si tienes los tests de esta manera, se acabarán descuidando, metiendo defectos y dará miedo cambiar el código productivo.
* Los tests son tan importante como el código de producción. No son ciudadanos de segunda.
    * Requieren pensar, diseñar y cariño. Se deben mantener limpios como el código de producción.
* ¿Qué hace un test limpio? Tres cosas: la legibilidad, la legibilidad y la legibilidad.
    * La legibilidad se logra con la claridad, simplicidad y expresivo.
* En los tests se puede ser relativamente ineficiente, no es código productivo. Por ejemplo, puedes usar StringBuffers o concatenar un String a pelo sin el típico StringBuilder...
* Puedes usar «un sólo concepto por test» o «un sólo aserto en el test» como buena guía para hacer un test.
    * Tampoco hay que volverse loco: recordemos que debe ser legible.
* Los tests deben cumplir FIRST:
    * **Fast**: Los tests deben ser rápidos para que se ejecuten muy a menudo. Si empiezan a ser lentos, vamos a tener un problema.
    * **Independent or isolated**: Un test no debe depender de otros, ni en orden, ni en lógica.
    * **Repeatable**: Esto es algo variopinto, ya que no se refiere a un test unitario al poder lanzarse en "QA" o en "local".
    * **Self*validating**: o automáticos. Los tests deben decir si se valida bien o mal. No debe haber ninguna manualidad para decir si algo está bien o mal.
    * **Timely**: A tiempo. Los tests se deben hacer antes del código productivo. Si se hacen después, es posible que no hayas diseñado el código productivo de tal forma que sea testable.

## 10) Clases

* Si debemos tener un test de algo privado de una clase, podemos ampliar su alcance dejando el acceso por paquete o protected. Así si el test tiene el mismo paquete puede acceder a lo que debemos testear.
* Las clases, cuanto más pequeñas, mejor. ¡Con las clases estamos midiendo responsabilidades!
* El nombrado es el primer camino para determinar el tamaño de una clase y de sus responsabilidades. Si no podemos precisar un nombre, quizá tiene demasiadas responsabilidades.
* Deberíamos ser capaces de escribir una breve descripción de la clase con 25 palabras sin utilizar «condicionales», «conjunciones», «disyunciones» o «peros».
* El principio de única responsabilidad (*Single Responsibility Principle* o *SRP*) dice que una clase o módulo debe tener sólo una razón para cambiar. O lo que es lo mismo: «sólo una responsabilidad».
    * Es uno de los más importantes conceptos de la programación orientada a objetos. De hecho, es uno de los conceptos más fáciles de entender y de adoptar.
* Tenemos un problema y es que dejamos de pensar cuando el programa funciona. Lo que tenemos que hacer es que funcione, sí, pero refactorizar a posteriori si vemos que algo podría mejorarse.
* Todo sistema que se pueda tallar va a contener un montón de lógica y complejidad.
    * El primer objetivo en controlarla es organizarla para conseguir los siguientes objetivos:
        * dónde buscar algo
        * entender qué hace ese algo
        * abstraerse de la complejidad de todo lo demás
    * Básicamente, queremos que nuestros sistemas se compongan de clases pequeñas, no largas. Cada pequeña clase tiene una pequeña responsabilidad, una razón para cambiar y colabora con otras pequeñas clases para conseguir el objetivo deseado en el comportamiento de los sistemas.
* Cuantas más atributos de una clase cambie un método, más cohesión tiene para la clase.
* Por si aún no te has enterado: ¡las clases deben ser pequeñas!
* Cuando las clases pierdan cohesión, sepáralas.
* Para la mayoría de los sistemas, el cambio es contínuo. Prepárate para el cambio.
* Normalmente cuando tenemos clases muy pequeñas, el tiempo que invertimos para entenderlas es mínimo.
* Las clases abstractas deben representar conceptos solamente.
* ***Las necesidades cambiarán*** y, por tanto, el código también.

## 11) Sistemas
* Debemos separar los procesos de arranque de los procesos normales de negocio.
    * Por ejemplo, en Spring se utiliza la inyección de dependencias automáticamente, pero hay otros sistemas que no lo hacen.
* Mockea objetos de construcción si no son el sujeto sobre el que haces el test (sut o Subject Under Test).
* La aplicación no debe ser consciente de cómo se construye. Sabe que se hace apropiadamente, pero ya está.
* Es un mito que podamos tener sistemas "correctas" desde el primer momento. Debemos mejorar éstos mediante user stories, refactorizar y permitir el uso de historias de usuario mañana.
  * Esta es la esencia de iterativo e incremental, del Agile.
  * Siguiendo TDD, refactorizando y con clean code podemos realizar todo esto a bajo nivel en el software.  
  * Pero, ¿qué pasa con los sistemas? Pues que todo se puede mejorar y ampliar si separamos los conceptos. Hay que saber delegar, incluso en el software.
* Separar el dominio de la persistencia te permite testar unitariamente de una forma más apropiada. Si puedes ignorar cualquier framework para construir tu negocio, éste no se verá afectado por sus cambios.
* Un óptimo sistema de arquitectura consiste en modularizar y separar los dominios: inputs, persistencia, negocio, eventos... son capas que debemos modularizar.
* Sabemos que es mejor dar responsabilidades a las personas más cualificadas, por tanto: es mejor posponer las decisiones hasta el último momento posible. De esta manera, podremos elegir la mejor decisión con la información más completa.
* Los standares están bien pare reutilizar ideas y componentes y hacer que todo encaje genial, sin embargo, no hay que descuidar el fin para el que se hacen: entregar valor.
* Utiliza lo más simple que pueda funcionar.


## 12) Emergencias... o mejor dicho fuegos?

* Un diseño es simple si (en orden de importancia):
  * Ejecuta todos los tests.
  * No contiene duplicación de código.
  * Expresa la intención del programador.
  * Minimiza el número de clases y métodos.
* Un sistema que no se puede probar no debería ser desplegado nunca.
* Tener tests te permite refactorizar.
* La duplicación es el principal enemigo de un buen sistema de diseño.
* Lo más claro que hagas el código implica que el resto tardará menos en entenderlo:
  * Elige bien los nombres, clases pequeñas y evita que el resto se sorprenda cuando vean lo que hacen.
  * Agrega patrones como Command o Visitor a tus clases para que se sepa exactamente qué hacen.
* Tests unitarios bien escritos también ayudan a expresar la intención del lenguaje y sirve como una autodocumentación de qué tiene que hacer la clase que estás testeando.
* Lo más importante es que lo intentes: trata de no irte a solucionar otro problema, hazlo legible, entendible y expresivo.
* Sobre las mínimas clases y métodos: imagínate una arquitectura o standard que te obliga a crear una interfaz por cada clase. Elige ser pragmático.

## 13) Concurrencia

* La concurrencia puede mejorar el rendimiento, pero no siempre.
* El diseño puede cambiar para abordar una solución concurrente.
* La concurrencia provoca más trabajo, tanto a nivel de rendimiento como de escribir código
* Corregir problemas concurrente es complejo, incluso para problemas simples.
  * Algunos bugs pueden no reproducirse.
* La concurrencia requiere un cambio fundamental en la estrategia de diseño.
* Nos podemos defender de la concurrencia mediante los siguientes principios:
  * Single Responsibility Principle:
    * El código concurrente debe tener sus propios desafíos, estar localizado y tener su propio ciclo de vida de desarrollo.
      * Mantén lejos tu código concurrente de código no concurrente.
  * Limita el alcance de los datos:
    * La solución es poner un synchronized en las secciones críticas.
      * Reduce el número de sitios donde pones un synchronized. Si es más de uno y se te olvida vas a provocar un problema de concurrencia de seguro.
      * Encapsula los datos y evita que otros accedan y los modifiquen.
  * Copia los datos:
    * Y trátalos como sólo lectura.
    * Esto puede generar una sobrecarga en el recolector de basura y en el tiempo de creación: merece la pena si te ahorras un problema concurrente.
    * Si vas a modificar datos, hazlo en un solo hilo.
    * Los hilos deben ser tan independientes como sean posibles
    * Vamos que no tengas variables globales compartidas por varios hilos. Es algo de primero de carrera.
  * Conoce tu lenguaje:
    * Usa clases thread-safe como colecciones o librerías.
 * Conoce los modelos de ejecución de los hilos:
   * **Bound Resources**: Recursos con tamaño ajustado que se usan en un entorno concurrente: conexiones a bases de datos, buffers de lectura y escritura, etc...
   * **Mutual Exclusion**: Sólo un hilo puede acceder al dato compartido al mismo tiempo.
   * **Starvation**: Si dejas a los hilos que se ejecutan rápido que se lancen los primeros, puede que lleguen los "lentos" a no ejecutarse nunca.
   * **Deadlock**: Cuando hay 2 hilos esperando el uno por el otro para terminar. También lo que se conoce como interbloqueo.
   * **Livelock**: Cuando un hilo trata de entrar en el lock pero ningún Thread le deja durante un tiempo largo.
* Mantén los synchronized pequeños:
  * Cada synchronized implica un bloqueo para garantizar que solamente un hilo puede pasar por ahí a la vez.
* Testea el código susceptible a ser concurrente:
  * Si algún test falla porque en alguna secuencia algo no fue bien, revisa bien qué ha pasado. No lo ignores.
* Rompe tu sistema y separa las clases que son conscientes de la concurrencia de las que no lo son.

## 14) Refinamientos del código

* Inicialmente no vas a escribir el código más correcto y limpio. Lo ideal es que inicialmente sea código "sucio" y se vaya mejorando. Piensa cuando eras pequeño y tenías que entregar algo sin tachones. A la primera no salía, ¿a que no?
* Antes de refactorizar algo, tienes que hacer TDD y eso implica tener tests. Haz pequeños cambios asegurándote que los tests pasan a cada cambio. Si falla algo, arréglalo antes de seguir mejorándolo.
* La cosa del buen software muchas veces es particionar el código y creando lugares apropiados para el mismo. Separar los conceptos hacen el código mucho más fácil de leer y mantener.

## 17) Smells y heurísticas
Este capítulo es una síntesis de todo lo que ya hemos hablado.
* En Refactoring, Martin Fowler identifica varios "code smells". Basado en esa lista, tenemos una lista donde se clasifica cada code smell:
  * **Comentarios**:
    * **C1**: Información inapropiada. @author, last-modified date, son cosas que no deberían aparecer en el código. Ya tenemos el control de versiones para eso.
    * **C2**: Comentario obsoleto, viejo, irrelevante... Si te encuentras uno bórralo. Si no puedes, al menos actualízalo.
    * **C3**: Comentario redundante. No hace falta explicarlo. Por ejemplo: los javadocs autogenerados.
    * **C4**: Comentario pobre: utiliza buenas palabras y trata de que sea el mejor comentario escrito jamás.
    * **C5**: Código comentado: Elimínalo. En el control de versiones vas a seguir teniéndolo
  * **Entorno**:
    * **E1**: Para arrancar una aplicación deberías tener un comando que lo arranque y ya. No debería ser ir a buscar las bolas de dragón para que algo funcione.
    * **E2**: De forma idéntica al anterior, deberías poder arrancar todos los tests unitarios e integrados de la aplicación.
  * Funciones/Métodos:
    * **F1**: Demasiados argumentos: más de 3 argumentos no está aconsejado para funciones. Yo diría que más de dos ya empieza a oler. Lo ideal es 1 o ninguno.
    * **F2**: Objetos de salida: la gente no se espera que los objetos cambien en lo que se devuelve, si no que si le aplicas algo a alguien, ese alguien debe ser el modificado, no el resultante.
    * **F3**: Parámetros "flag": Indican que tu función va a hacer más de una cosa y se deberían evitar.
    * **F4**: Funciones muertas: Funciones que nunca se llaman deben borrarse. Tu control de versiones siempre lo recordarán.
  * General:
    * **G1**: Múltiples lenguajes de programación en un fichero: Por favor, no mezcles Java con XML salvo que no quede más remedio.
    * **G2**: El caso de uso más común no está implementado: Siguiendo el principio de la mínima sorpresa, si tenemos un caso de uso para una clase de lo más habitual y no está implementado, el desarrollador que venga por detrás nos mirará con odio y empezará a enfangarse con todo, ya que habremos perdido su confianza.
    * **G3**: Incorrecto comportamiento en casos límites: por favor, indica los casos límites de tu código y testéalos. No dejes nada a la intuición o a la suerte.
    * **G4**: Apagar la seguridad: No quites los warnings ni apagues los tests. Aunque te convenzas de que lo harás luego, todos sabemos que no lo harás y lo sabes.
    * **G5**: Duplicación: Dice que una repetición es una oportunidad perdida de abstracción. Yo no estoy totalmente de acuerdo, es posible que sea mejor duplicar y cambiar algo que abstraer e implementar erróneamente algo. Muchas veces el abstraer es un YAGNI de manual y no es tan necesario en la práctica. Eso sí, si somos puristas, Uncle Bob en este punto tiene razón. La mayoría de duplicaciones de código se pueden extraer a un método y el Ctrl + C y Ctrl + V se debería prohibir :)
    * **G6**: Código a equivocado nivel de abstracción. Las constantes, variables o funciones de utilidad basadas en el detalle de implementación no deben ir en la clase base.
    * **G7**: Las clases base dependen de sus clases derivadas. No hagas esto :)
    * **G8**: Demasiada información: No te centres en crear una clase con muchos métodos a implementar por tus subclases. Los buenos desarrolladores aprenden a limitar qué se expone en interfaces, clases y módulos. Cuantos menos métodos tenga una clase, mejor será.
    * **G9**: Código muerto. Elimínalo.
    * **G10**: Separación vertical. Con esto se quiere comentar que una variable o función se debe declarar cercanamente a donde se declara. Sobre las variables estoy de acuerdo, pero sobre las funciones: existen los IDE's que te llevan directamente a esa función. Dónde la declares, tanto da.
    * **G11**: Inconsistencia. Básicamente sigue el mismo estilo. Si a algo lo llamas Patata, luego no lo llames Boniato. Lenguaje ubícuo, por favor. Nombra bien los métodos, por ejemplo, delete, remove, etc..
    * **G12**: Desorden. Usar un constructor por defecto en una clase donde es obligatorio rellenar los atributos es de muy relajaos. Haz tu código bien.
    * **G13**: Acople artificial. Invierte tiempo para decidir dónde colocas funciiones, constantes o variables. No las mezcles en un sitio a mano y las dejes ahí.
    * **G14**: La envidia de la feature: imagina que tienes un objeto Cuenta donde están tus movimientos bancarios. Luego tienes un método "calculadorDeNomina(Cuenta c)" que utiliza cosas del objeto cuenta y actualiza el saldo. Quizá el método calculadorDeNómina(..) deban estar dentro de la clase Cuenta, ya que accede a mucha información de dicho objeto. En este caso, el método calculadorDeNómina(..) tiene envidia del scope del objeto Cuenta. En castellano esta expresión no parece tener mucho sentido.
    * **G15**: Parámetros selectores. Evita parámetros booleanos en los métodos.
    * **G16**: Intenciones oscuras: magic numbers, notación húngara, y cosas raras oscurece las intenciones del autor. Evítalas.
    * **G17**: Responsabilidad desplazada. A veces ponemos una constante donde no se debe o donde no es intuitivo para la mayoría de los mortales.
    * **G18**: Métodos estáticos inapropiados: Usa no estáticos por defecto.  
    * **G19**: Variables sin explicación: Las variables siempre deben tener sentido y un correcto nombrado.
    * **G20**: Funciones deben decir lo que hacen.
    * **G21**: Debes entender el algoritmo
    * **G22**: Haz las dependencias lógicas que sean físicas. Es decir, aunque sean dos conceptos diferentes, si uno depende del otro, mantenlos cerca o en el mismo scope.
    * **G23**: Usa polimorfismo a if/else o switch statements.
    * **G24**: Sigue las convenciones estándar del lenguaje o framework que estés usando.
    * **G25**: Sustituye los magic numbers a constantes con un nombre correcto.
    * **G26**: Sé preciso: desarrolla lo que utilices y no mates moscas a cañonazos.
    * **G27**: Es mejor una clase abstracta que un switch con enumerados chulis.
    * **G28**: Trata de evitar sentencias con booleanos. Mejor extraelas a una función donde sea más legible lo que quiere decir (o lo que intenta decir) el if.
    * **G29**: No pongas condicionales negados.
    * **G30**: Las funciones sólo deben hacer una sóla cosa.
    * **G31**: No esconder los acoples temporales. Si algo necesita estar acoplado, no lo hagas difícil de seguir.
    * **G32**: No seas arbitrario cuando estructures tu código.
    * **G33**: Encapsula casos límite en variables para permitir revisarlos.
    * **G34**: Las funciones sólo pueden descender un nivel de abstración. Es decir, una función sólo puede invocar a objeto.metodo(), no a objeto.property.método()
    * **G35**: Haz las variables de configuración configurables.
    * **G36**: Parecido a la G34, evita saber de un módulo de forma transitiva. Si A sabe de B y B de C, A no debe saber de C ni llamarlo directamente. Esto se conoce como la ley de Demeter o escribir código tímido.
  * **Java**:
    * **J1**: Evita las wildcard cuando importes bibliotecas. Solo confunden y aportan ruido cuando programas si te traes algo de más.
    * **J2**: No heredes constantes. En lugar de usar herencia, usa un static import si no quieres nombrar la clase cada vez que la usas.
    * **J3**: Si puedes usa Enums por encima de constantes.
  * **Naming**:
    * **N1**: Elige nombres descriptivos.
    * **N2**: Elige nombres descriptivos con el apropiado nivel de abstración.
    * **N3**: Usa la nomenclatura estándar cuando sea posible.
    * **N4**: Usa nombres no ambiguos.
    * **N5**: Usa nombres largos para scopes que son largos.
    * **N6**: Evita usar encodings como m_ o f. Son notaciones que no son interesantes.
    * **N7**: Los nombres deben describir side effects.
  * **Tests**:
    * **T1**: No hay suficientes tests.
    * **T2**: Usa una herramienta de cobertura de tests.
    * **T3**: No quites los tests triviales. Pueden no ser triviales para otra persona.
    * **T4**: Los tests ignorados alimentan la incertidumbre. Se ignoraron porque ¿ya no aplican o porque se querían arreglar?
    * **T5**: Testea los casos límites.
    * **T6**: Cuando encuentres un bug, haz tests de la función que ha dado el bug, probablemente encuentres más errores.
    * **T7**: Patrones de fallo pueden revelarte un problema.
    * **T8**: Los patrones de cobertura de test te pueden revelar otro problema.
    * **T9**: Los tests deben ser rápidos, ¿qué es eso de que tarden media hora?
