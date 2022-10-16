---
layout: "../../layouts/BlogPost.astro"
title: "Wicked PDF una gema para generar PDF's en Ruby on Rails"
description: "Lorem ipsum dolor sit amet"
pubDate: "Jul 08 2022"
heroImage: "/placeholder-hero.jpg"
---

Como desarrolladores de Ruby on Rails es muy comun que utilizemos gemas para nuestro trabajo en el dia a dia.
y yo creo que es genial utilizarlas ya que basicamente son librerias que nos permiten reutilizar funcionalidades
que otras personas han creado y tambien compartir funcionalidades con otras personas. En lo personal no creo que alguien vea como algo eficiente a "Reinventar la rueda" y mucho mas cuando estamos en entornos en donde el
objetivo suele ser entregar un producto o funcionalidad con la mayor calidad en un tiempo y en el menor tiempo posible y justamente Rails
tiene la fama de que nos permite entregar productos en un tiempo menor comparado a otros Frameworks de otros lenguajes.

Hace algunas semanas para un proyecto en el que estaba trabajando nos pidieron que en la aplicacion que teniamos deberiamos generar reportes,
en pocas palabras la idea era generar unos PDF's y realmente lo primero que se me ocurrio fue hacer una funcion de JavaScript para que al 
darle a un boton llamara a la funcion de "Imprimir" que actualmente tienen los navegadores y en general la idea puede sonar bien, pero resulta que no es la mas eficiente si tenemos lineamientos especificos que seguir a el momento de generar el documento y desde el punto de vista de un
usuario simplemente no es la mejor experiencia. Asi que luego de algunas horas de investigar alternativas di con la gema "Wicked" y justamente
era lo que realmente necesitaba. Basicamente lo que hace es coger una plantilla de HTML (Que nosotros la creamos) y la convierte en un PDF
gracias a [wkhtmltopdf](https://wkhtmltopdf.org/) Deberia aclarar que los siguientes pasos funcionan correctamente en Ubuntu inicialmete lo
intente en Fedora y no funciono ya que no hay soporte de [wkhtmltopdf](https://wkhtmltopdf.org/) para Fedora (O por lo menos no lo vi)

lo que primero que debemos hacer es agregar la gema a nuestro Gemfile y para eso tenemos 2 opciones

1. escribir esto en la terminal `bundle add wicked_pdf` 
2. pegar esto en el archivo Gemfile `gem 'wicked_pdf'`

Luego en nustra terminal debemos instalar la gema con el comando `bundle install`

despues debemos de generar un inicializador con el comando `rails g wicked_pdf`

y ya que wicked_pdf depende de [wkhtmltopdf](https://wkhtmltopdf.org/) debemos de instalar los binarios (Simplemente debemos de instalar una
gema mas y nos instalara los binarios para la mayoria de sistemas Linux y OSX) y de nuevo tenemos 2 opciones

1. escribir esto en la terminal `bundle add wkhtmltopdf-binary` 
2. pegar esto en el archivo Gemfile `gem 'wkhtmltopdf-binary'`

ademas en la documentacion nos sugieren que si el ejecutable de wkhtmltopdf no esta en la ruta del servidor web se debe de configurar en un 
inicializador este esta ubicado en `/config/initializers/wicked_pdf.rb` y lo que debemos hacer es quitar el comentario de estas lineas o
copiar y pegar

````ruby
  exe_path: '/usr/local/bin/wkhtmltopdf',
  enable_local_file_access: true


