---
layout: post
title: "Calcula el coste de tus consultas a ChatGPT"
date: "2024-11-11 00:00:00 +0100"
categories: chatgpt tokens
---

En este tutorial te enseñaré un ejemplo de cómo se pueden tokenizar las consultas que hacemos a [ChatGPT](https://chatgpt.com/) para que sepamos lo que nos está costando cada interacción.

# Calcula el coste de tus consultas a ChatGPT

## 1. Introducción

En los días en los que vivimos es fácil encontrarnos con interacciones con el [ChatGPT](https://chatgpt.com/) de [OpenAI](https://openai.com/). Ya sea para preguntarle cosas que no sabemos o cualquier cosa que se nos ocurra. Creo que no hace falta introducir qué es, pero os daré una breve pincelada.

[OpenAI](https://openai.com/) es una organización que investiga y desarrolla inteligencia artificial para que beneficie a toda la humanidad. Uno de los productos más conocidos es [ChatGPT](https://chatgpt.com/), un modelo de lenguaje basado en IA que genera respuestas a tus preguntas, como si de otra persona se tratase.

Por otro lado, en este tutorial nos vamos a enfocar en el modelo [GPT-4](https://www.xataka.com/basics/gpt-4-que-como-funciona-como-usarlo-que-puedes-hacer-este-modelo-lenguaje-inteligencia-artificial), ya que es el más grande y preciso que todos los modelos que le preceden, pudiendo manejar conversaciones complejas y respuestas más detalladas. Si queremos una respuesta fiable, pero elaborada, utilizaremos este modelo. Sin embargo, si queremos un modelo más barato, no tan fiable pero rápido, podemos usar el modelo GPT-4 Mini.

Los *tokens* son fragmentos de texto con los que el modelo trabaja. Pueden ser palabras, partes de palabras, intenciones, etc... Cuantos más *tokens* se usan para preguntarle (input) algo a ChatGPT, más espacio ocupa y más recursos consume. También hay que tener en cuenta que ChatGPT nos responde (output) con *tokens*, por tanto, también nos afecta al espacio y recursos que consume.

Los *tokens* son la unidad de facturación de OpenAI, por tanto, es bueno controlar cuántos usamos en cada momento para intentar buscar la eficiencia cuando trabajemos con ChatGPT. Sin ir más lejos, a día de hoy estos son [los precios](https://openai.com/api/pricing/) de los *tokens* para los modelos GPT-4 y GPT-4 mini de OpenAI:

- GPT-4 tiene un costo de 2.5 dólares por cada 1 millón de *tokens* de entrada (input) y 10 dólares por cada millón de *tokens* de salida (output).
- GPT-4 mini es más económico, por 0.15 dólares por cada millón de *tokens* de entrada (input) y 0.60 dólares por cada millón de *tokens* de salida (output)

## 2. Entorno

El tutorial está escrito usando el siguiente entorno:

* Hardware: Apple M1 Pro 32GB 1TB SDD
* Sistema Operativo: MacOS 15.0.1
* Programas utilizados:
  * Homebrew: 4.4.4
  * pyenv: 2.4.17
  * python: 3.13
  * Java: 17.0.9 temurin
  * Apache Maven: 3.8.6

## 3. Biblioteca tiktoken

[Tiktoken](https://github.com/openai/tiktoken) es un rápido tokenizador [Byte pair encoding](https://en.wikipedia.org/wiki/Byte_pair_encoding) para el uso en modelos de OpenAI.

### 3.1. Instalación tiktoken

Esta librería está en [Python](https://www.python.org/), por lo que vamos a usar [homebrew](https://brew.sh/) para instalar pyenv y de ahí instalaremos la versión de python que queramos.

Actualizamos brew a la última versión.

```bash
brew upgrade
```

Instalamos el gestor de versiones de python.

```bash
brew install pyenv
```

Elegimos la versión 3.13 de Python. Hemos elegido esta porque es la más actual,

```bash
pyenv install 3.13
```

Verificamos que tenemos python instalado y la versión que hemos elegido:

```bash
python --version
Python 3.13.0
```

Creamos un entorno de pruebas en python para instalar la biblioteca de tiktoken:

```bash
python3 -m venv ~/my_env
source ~/my_env/bin/activate
pip install tiktoken
```

Vamos a generar un fichero que voy a llamar openai-tokens.py:

```bash
touch openai-tokens.py
```

### 3.3. Jugando con tiktoken

Dentro de este fichero, vamos a colocar el siguiente trozo de código:

```python
import tiktoken

# Recuperamos la codificación asociada a gpt-4o y gpt-4o-mini
encoding = tiktoken.get_encoding("o200k_base")

texto = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

print("Generando tokens para el siguiente texto:\n", texto)

tokens = encoding.encode(texto)

print(tokens)
```

Y lo ejecutamos:

```bash
python openai-tokens.py
Generando tokens para el siguiente texto:
 Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
[61495, 38714, 25840, 2353, 36204, 11, 54472, 91785, 45688, 11, 10412, 621, 160226, 14725, 173578, 4518, 110546, 859, 79682, 78404, 151394, 13, 21952, 51474, 687, 23874, 126377, 11, 39559, 15647, 73375, 138821, 99386, 1191, 199011, 58560, 4518, 51155, 488, 513, 11235, 127896, 131925, 13, 121159, 185481, 3288, 627, 25840, 306, 147652, 306, 145757, 82872, 15368, 274, 168333, 79682, 5658, 150234, 64611, 150207, 13, 1771, 185000, 29021, 171447, 266, 118278, 55268, 2893, 440, 2477, 11, 24871, 306, 62745, 2780, 143411, 152505, 76322, 278, 5727, 1211, 893, 152097, 13]
```

Con intención de hacer un benchmark entre diferentes librerías de Python y Java vamos a conocer el tiempo que tardamos en convertir a texto una cadena dada. Ahora vamos a importar la librería _time_ directamente y vamos a imprimir por consola el tiempo en milisegundos de lo que ha tardado.

```python
import tiktoken,time

# Recuperamos la codificación asociada a gpt-4o y gpt-4o-mini
encoding = tiktoken.get_encoding("o200k_base")

texto = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

print("Generando prompt tokens para el siguiente texto:\n", texto)

inicio = time.time()
tokens = encoding.encode(texto)
tiempo = (time.time() - inicio) * 1000

print(tokens)
print("El tiempo que ha tardado en convertir prompt tokens ha sido de ", tiempo)
```

Ejecutamos otra vez y ya lo tenemos:

```
python openai-tokens.py
Generando prompt tokens para el siguiente texto:
 Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
[61495, 38714, 25840, 2353, 36204, 11, 54472, 91785, 45688, 11, 10412, 621, 160226, 14725, 173578, 4518, 110546, 859, 79682, 78404, 151394, 13, 21952, 51474, 687, 23874, 126377, 11, 39559, 15647, 73375, 138821, 99386, 1191, 199011, 58560, 4518, 51155, 488, 513, 11235, 127896, 131925, 13, 121159, 185481, 3288, 627, 25840, 306, 147652, 306, 145757, 82872, 15368, 274, 168333, 79682, 5658, 150234, 64611, 150207, 13, 1771, 185000, 29021, 171447, 266, 118278, 55268, 2893, 440, 2477, 11, 24871, 306, 62745, 2780, 143411, 152505, 76322, 278, 5727, 1211, 893, 152097, 13]
El tiempo que ha tardado en convertir prompt tokens ha sido de  0.44226646423339844
```

Se ha tardado 0,442 milisegundos en ejecutar esta conversión. Las 69 palabras han sido traducidas en 87 tokens.

Antes de seguir con la otra biblioteca, vemos que tenemos una lista de *tokens* en integer, pero, ¿esto realmente qué es?
Podemos convertir a texto estos prompt *tokens* para saber exactamente qué es lo que está guardando en estos números.

Vamos a crear otro script de python que he nombrado como **openai-tokens-detail.py**

```bash
touch openai-tokens-detail.py
```

Ahora vamos a preguntar qué significa cada uno de estos números enteros con el siguiente código:

```python
import tiktoken

# Recuperamos la codificación asociada a gpt-4o y gpt-4o-mini
encoding = tiktoken.get_encoding("o200k_base")

tokens = [encoding.decode_single_token_bytes(token) for token in [61495, 38714, 25840, 2353, 36204, 11, 54472, 91785, 45688, 11, 10412, 621, 160226, 14725, 173578, 4518, 110546, 859, 79682, 78404, 151394, 13, 21952, 51474, 687, 23874, 126377, 11, 39559, 15647, 73375, 138821, 99386, 1191, 199011, 58560, 4518, 51155, 488, 513, 11235, 127896, 131925, 13, 121159, 185481, 3288, 627, 25840, 306, 147652, 306, 145757, 82872, 15368, 274, 168333, 79682, 5658, 150234, 64611, 150207, 13, 1771, 185000, 29021, 171447, 266, 118278, 55268, 2893, 440, 2477, 11, 24871, 306, 62745, 2780, 143411, 152505, 76322, 278, 5727, 1211, 893, 152097, 13]]

print(tokens)
```

De nuevo, ejecutamos:

```bash
python openai-tokens-detail.py
[b'Lorem', b' ipsum', b' dolor', b' sit', b' amet', b',', b' consectetur', b' adipiscing', b' elit', b',', b' sed', b' do', b' eiusmod', b' tempor', b' incididunt', b' ut', b' labore', b' et', b' dolore', b' magna', b' aliqua', b'.', b' Ut', b' enim', b' ad', b' minim', b' veniam', b',', b' quis', b' nost', b'rud', b' exercitation', b' ullam', b'co', b' laboris', b' nisi', b' ut', b' aliqu', b'ip', b' ex', b' ea', b' commodo', b' consequat', b'.', b' Duis', b' aute', b' ir', b'ure', b' dolor', b' in', b' reprehenderit', b' in', b' voluptate', b' velit', b' esse', b' c', b'illum', b' dolore', b' eu', b' fugiat', b' nulla', b' pariatur', b'.', b' Ex', b'cepteur', b' sint', b' occaec', b'at', b' cupid', b'atat', b' non', b' pro', b'ident', b',', b' sunt', b' in', b' culpa', b' qui', b' officia', b' deserunt', b' moll', b'it', b' anim', b' id', b' est', b' laborum', b'.']
```

Como se puede observar, delante de cada literal tenemos una _b_ que significa que todo texto detrás es un **bytearray**. Con un texto tan largo no podemos apreciar a simple vista cómo se traduce el texto en *tokens*. Vamos con un ejemplo más cortito:

```
El perro corre rápido y el gato corre lento.
[4422, 96439, **14025**, 41693, 342, 650, 99767, **14025**, 120905, 13]
[b'El', b' perro', **b' corre'**, b' r\xc3\xa1pido', b' y', b' el', b' gato', **b' corre'**, b' lento', b'.']
```

Vemos que se reutilizan las expresiones exactas como en el caso de "corre" y el propio "." tiene un *token*. Nos invita a pensar que **siempre que convirtamos texto, va a haber más tokens que palabras.** Veamos otro ejemplo:

```
el perro corre, el perro juega, el perro duerme.
[296, 96439, 14025, 11, 650, 96439, 145144, 11, 650, 96439, 116318, 1047, 13]
[b'el', b' perro', b' corre', b',', b' el', b' perro', b' juega', b',', b' el', b' perro', b' duer', b'me', b'.']
```

Podemos visualizar que la primera palabra de la frase (el) se codifica de forma especial (296).

# 4. Biblioteca jtokkit

La biblioteca [jtokkit](https://github.com/knuddelsgmbh/jtokkit) se centra en ser rápida y eficiente en tareas del procesamiento de lenguaje natural usando los modelos de OpenAI. Añade una interfaz fácil de usar para tokenizar el texto. Tiene las mismas capacidades en Java que [tiktoken](https://github.com/openai/tiktoken) en Python.

## 4.1. Instalación

Como requerimientos necesitamos Java17 y Maven. Para instalarlos recomiendo [sdkman](https://sdkman.io/). En [adictos](https://adictosaltrabajo.com/) tienes un tutorial de [cómo instalar java con sdkman](https://adictosaltrabajo.com/2017/04/10/gestiona-tus-herramientas-de-desarrollo-con-sdkman/).

Vamos a desarrollar una pequeña consola interactiva donde podamos escribir lo que le queramos preguntar a ChatGPT, nos calcule el número de tokens y el coste de los tokens de entrada y de salida.

Para añadir la biblioteca [jtokkit](https://github.com/knuddelsgmbh/jtokkit) solamente hay que incluirla como dependencia en maven:

```xml
<dependency>
    <groupId>com.knuddels</groupId>
    <artifactId>jtokkit</artifactId>
    <version>1.1.0</version>
</dependency>
```

La función de codificación en *tokens* se nos quedaría así:

```java
import com.knuddels.jtokkit.Encodings;
import com.knuddels.jtokkit.api.Encoding;
import com.knuddels.jtokkit.api.EncodingRegistry;
import com.knuddels.jtokkit.api.EncodingType;
import com.knuddels.jtokkit.api.IntArrayList;

// (...)

private int numeroTokens(String consulta) {
    EncodingRegistry registry = Encodings.newDefaultEncodingRegistry();
    Encoding enc = registry.getEncoding(EncodingType.O200K_BASE); //o200k_base
    long inicio = System.nanoTime();
    IntArrayList encoded = enc.encode(consulta);
    long fin = System.nanoTime();
    System.out.println("Esta conversión ha tardado "+ (fin - inicio) +" nanosegundos");
    System.out.println("La consulta '" + consulta + "' se ha codificado en " + encoded + " y son " + encoded.size() + " tokens.");
    return encoded.size();
}
```


Puedes descargarte [mi proyecto funcionando desde Github](https://github.com/asegnz/openai-tokens-calculator).

Para ejecutarlo, tenemos que introducir los siguientes comandos:

```bash
mvn clean install
mvn exec:java -Dexec.mainClass="com.asegnz.App"
```

Se nos abrirá la aplicación por consola y podremos utilizarla para calcular el coste de nuestras interacciones con ChatGPT:

```
Bienvenid@ a la calculadora de tokens de modelos de OpenAI

Seleccione una opción:
1. Introducir consulta a ChatGPT
2. Cambiar variable max_tokens (50) para la respuesta estimada de ChatGPT
3. Cambiar coste por millón de inputs tokens (2.50 $)
4. Cambiar coste por millón de output tokens (10 $)
5. Salir
Ingrese el número de la opción deseada: 1
Ingrese su consulta a ChatGPT: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
Esta conversión ha tardado 464750 nanosegundos
La consulta 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.' se ha codificado en [61495, 38714, 25840, 2353, 36204, 11, 54472, 91785, 45688, 11, 10412, 621, 160226, 14725, 173578, 4518, 110546, 859, 79682, 78404, 151394, 13, 21952, 51474, 687, 23874, 126377, 11, 39559, 15647, 73375, 138821, 99386, 1191, 199011, 58560, 4518, 51155, 488, 513, 11235, 127896, 131925, 13, 121159, 185481, 3288, 627, 25840, 306, 147652, 306, 145757, 82872, 15368, 274, 168333, 79682, 5658, 150234, 64611, 150207, 13, 1771, 185000, 29021, 171447, 266, 118278, 55268, 2893, 440, 2477, 11, 24871, 306, 62745, 2780, 143411, 152505, 76322, 278, 5727, 1211, 893, 152097, 13] y son 87 tokens.
El coste de la consulta es: 0.0002175000 $ y el coste esperado de la respuesta de ChatGPT es: 0.0005000000 $
Su consulta tiene un coste asociado de 0.0007175000 $

```

En esta consola podemos introducir exactamente la misma consulta de prueba con la librería de tiktoken y vamos a verificar que se sigue codificando a los mismos números enteros. Además, verificamos que ha tardado 464750 nanosegundos, un resultado muy similar a los 0,442 milisegundos que hicimos de la prueba en python. 

# 5. Conclusiones

Aunque este ejercicio representa una prueba de concepto en el uso de la librería de cálculo de *tokens*, su aplicabilidad potencial es notable en diversas circunstancias. La capacidad de medir de manera precisa el consumo de *tokens* constituye un pilar fundamental para la optimización de recursos. Este enfoque no solo permite reformular las consultas a ChatGPT para reducir el gasto en *tokens*, sino también facilita una evaluación detallada del coste asociado a cada interacción. Así, es posible establecer tarifas ajustadas y mejorar la eficiencia en los procesos de negocio, maximizando el valor obtenido a partir de cada inversión en el uso de modelos de lenguaje.

