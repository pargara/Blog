---
layout: "../../layouts/BlogPost.astro"
title: "Wicked PDF una gema para generar PDF's en Ruby on Rails"
description: "Lorem ipsum dolor sit amet"
pubDate: "Oct 28 2022"
heroImage: "/wicked.jpeg"
---

Como desarrolladores de Ruby on Rails es muy común que utilicemos gemas para nuestro trabajo en el día a día. Y yo creo que es genial utilizarlas, ya que básicamente son librerías que nos permiten reutilizar funcionalidades que otras personas han creado y también compartir funcionalidades con otras personas. En lo personal no creo que alguien vea como algo eficiente a “Reinventar la rueda” y mucho más cuando estamos en entornos en donde el objetivo suele ser entregar un producto o funcionalidad con la mayor calidad en un tiempo y en el menor tiempo posible y justamente Rails tiene la fama de que nos permite entregar productos en un tiempo menor comparado a otros Frameworks de otros lenguajes.

Hace algunas semanas para un proyecto en el que estaba trabajando nos pidieron que en la aplicación que ateníamos deberíamos generar reportes, en pocas palabras la idea era generar PDFS y realmente lo primero que se me ocurrió fue hacer una función de JavaScript para que al darle a un boton llamara a la función de “Imprimir” que actualmente tienen los navegadores y en general la idea puede sonar bien, pero resulta que no es la más eficiente si tenemos lineamientos específicos que seguir al momento de generar el documento y desde el punto de vista de un usuario simplemente no es la mejor experiencia. Así que luego de algunas horas de investigar alternativas di con la gema [wicked_pdf](https://rubygems.org/gems/wicked_pdf) y justamente era lo que realmente necesitaba. Básicamente, lo que hace es coger una plantilla de HTML (Que nosotros la creamos) y la convierte en un PDF gracias a [wkhtmltopdf](https://wkhtmltopdf.org/). Debería aclarar que los siguientes pasos funcionan correctamente en Ubuntu inicialmente lo intente en Fedora y no funciono, ya que no hay soporte de [wkhtmltopdf](https://wkhtmltopdf.org/) para Fedora (O por lo menos no lo vi)

Lo que primero que debemos hacer es agregar la gema a nuestro Gemfile y para esto pegas esto en el archivo Gemfile `gem 'wicked_pdf'`

Luego en nuestra terminal debemos instalar la gema con el comando `bundle install`

Después debemos de generar un inicializador con el comando `rails g wicked_pdf`

Y ya que wicked_pdf depende de wkhtmltopdf debemos de instalar los binarios (Simplemente debemos de instalar una gema más y nos instalara los binarios para la mayoría de sistemas Linux y OSX) simplemente agregas esto otro al Gemfile `gem 'wkhtmltopdf-binary'` y de nuevo hacemos `bundle install`

Además en la documentación nos sugieren que si el ejecutable de wkhtmltopdf no está en la ruta del servidor web se debe de configurar en un inicializador, este está ubicado en `/config/initializers/wicked_pdf.rb` y lo que debemos hacer es quitar el comentario de estas líneas o copiar y pegar lo siguiente

```ruby
  exe_path: '/usr/local/bin/wkhtmltopdf',
  enable_local_file_access: true
```

Y de esta forma ya habríamos terminado con las configuraciones y podríamos pasar a utilizar la gema.
Voy a asumir de que ya tienes un controlador con una vista en mi caso daré el ejemplo desde la estructura
de un CRUD

```ruby

  def show
    respond_to do |format|
      format.html
      format.pdf do
        render pdf: "nombre_documento", template: 'vista_de_el_controlador/nombre_de_la_plantilla', formats:[:html], # si no añades un header puedes quitar la coma " , "
        # Para añadir un header hacemos lo siguiente
          header: { html: { template: 'vista_de_el_controlador/nombre_del_header', formats:[:html] }}, # si no añades un footer quitas lo coma
          footer: { html: { template: 'vista_de_el_controlador/nombre_del_footer', formats:[:html]}}
      end
    end
  end
```

Ahora vamos a crear las plantillas

crearemos un archivo en la vista del controlador debería quedar algo semejante a esto: `vista_de_el_controlador/nombre_de_la_plantilla.html.erb`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reporte</title>
    <%= wicked_pdf_stylesheet_link_tag "nombre_de_las_hojas_de_estilo" -%>
  </head>
  <body>
    <!-- AQUI AGREGAS LO QUE QUIERAS MOSTRAR EN TU PDF -->
  </body>
</head>
```
Si le queremos agregar estilos a nuestro archivo en la línea 9 puedes ver el helper que se utilizara para agregarlo, el archivo debe de estar en la siguiente ruta `/app/assets/stylesheet/`

para agregar un header simplemente creas un archivo en la vista del controlador y debería tener el mismo nombre que le pusiste en el controlador, el formato es el mismo que el de la plantilla para crear el pdf, en el siguiente ejemplo se muestra como añadir una imagen como header con un helper que nos proporciona la gema
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reporte</title>
    <%= wicked_pdf_stylesheet_link_tag "nombre_de_las_hojas_de_estilo" -%>
</head>
<body>
    <div id="logo">
       <%= wicked_pdf_image_tag 'Logo.png'%>
    </div>
  </body>
</head>
```

En el caso de que quieras agregar un footer haces lo mismo que en el header. En el siguiente ejemplo se agregará el número de las páginas que tiene el documento
```html
<!doctype html>
<html>
  <head>
    <meta charset='utf-8' />
    <%= wicked_pdf_stylesheet_link_tag "pdf" -%>
    <script>
      function number_pages() {
        var vars={};
        var x=document.location.search.substring(1).split('&');
        for(var i in x) {var z=x[i].split('=',2);vars[z[0]] = decodeURIComponent(z[1]);}
        var x=['frompage','topage','page','webpage','section','subsection','subsubsection'];
        for(var i in x) {
          var y = document.getElementsByClassName(x[i]);
          for(var j=0; j<y.length; ++j) y[j].textContent = vars[x[i]];
        }
      }
    </script>
  </head>
  <body onload="number_pages()">
   Pagina <span class="page"></span> </body>
</html>
```

Como puedes ver gracias a esta [wicked_pdf](https://www.rubydoc.info/gems/wicked_pdf) generar un PDF en RoR es algo relativamente fácil. Espero que te haya servido de ayuda y si tienes alguna duda no dudes en contactarme
