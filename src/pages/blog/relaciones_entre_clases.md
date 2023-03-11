---
layout: "../../layouts/BlogPost.astro"
title: "Relaciones entre clases con ruby"
description: "Lorem ipsum dolor sit amet"
pubDate: "Feb 19 2023"
heroImage: ""
---

Hace algunos dias estuve trabajando en un pequeño proyecto y me encontre en una pequeña confusion no sabia muy bien la diferencia entre un `require`
`require_relative` me las ingenie para seguir con el proyecto y afortunadamente tuve la oportunidad de que alguien de la empresa en la que estoy me diera 
una pequeña clase sobre las relaciones de una clase. Asi que voy a compartir un poco lo que aprendi



¿Cuando debo usar `require`?
----------------------------

La directiva require se utiliza para cargar librerías de Ruby que están disponibles globalmente en el sistema. Cuando utilizas require, 
Ruby busca la librería en las rutas definidas en la variable de entorno `LOAD_PATH`. Si la librería se encuentra en alguna de estas 
rutas, Ruby la carga y la hace disponible en tu programa.

Por ejemplo, si necesitas utilizar la librería json para manipular archivos en formato JSON, puedes utilizar la directiva 
`require 'json'` al principio de tu programa para cargar la librería.

¿Cuando debo usar `require_relative`?
-------------------------------------

La directiva `require_relative` se utiliza para cargar archivos de código de Ruby que se encuentran en la misma carpeta que tu programa, 
o en una subcarpeta de la misma. Al utilizar `require_relative`, puedes cargar archivos sin necesidad de especificar la ruta completa 
desde la raíz del sistema de archivos.

Por ejemplo, si tienes un archivo llamado `mi_clase.rb` en la misma carpeta que tu programa, puedes utilizar la directiva 
`require_relative` `'mi_clase'` para cargar el archivo.

¿Cómo se relacionan las clases con `require` y `require_relative`?
------------------------------------------------------------------


Cuando trabajas con clases en Ruby, es posible que necesites dividir tu código en múltiples archivos para hacerlo más fácil de entender 
y mantener. En este caso, puedes utilizar `require` y `require_relative` para cargar los archivos que contienen tus clases.

Por ejemplo, supongamos que tienes una clase Perro en un archivo llamado perro.rb, y otra clase Gato en un archivo llamado gato.rb. 
Si quieres utilizar ambas clases en tu programa principal, puedes utilizar `require_relative` para cargar ambos archivos al principio de tu programa:

```ruby
require_relative 'perro'
require_relative 'gato'

p = Perro.new
g = Gato.new
```
