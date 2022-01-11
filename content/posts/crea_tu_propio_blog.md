---
title: "Crea tu propio blog de IT o Ciberseguridad con Hugo y AWS S3"
author: ""
type: ""
date: 2022-01-10T19:25:16+01:00
subtitle: ""
image: ""
tags: [hugo, obsidian]
---
# Crea tu propio blog de IT o Ciberseguridad con Hugo y AWS S3.

Para mi primer post he pensado que lo mejor es hacer una pequeña guía de cómo he creado este blog. Está basado en [Hugo](https://gohugo.io/), que es un conversor de markdown a a HTML. Hace años empecé con [Jekyll](https://jekyllrb.com/), pero ahora me he pasado a Hugo y la verdad me gusta más. 
Para alojamiento, primero pensé en usar github pages, pero al final he seguido la idea de [Graham Helton](https://www.twitter.com/grahamhelton3) y me he decidido por usar un bucket de AWS S3; no es gratis, pero si muy barato, y me permite tener mi propio nombre de dominio. 

Graham tiene una serie de posts en inglés donde explica muy bien el proceso a seguir y los motivos para su elección, podéis encontrarlos [aquí](https://www.grahamhelton.com/blog/infosecblog1/). Yo no voy a explicar las cosas en tanto detalle, pero si seguiré todos los pasos necesarios para al menos tener la infraestructura y publicar nuestro blog con su propio dominio, nada de github.io o similares, creo que merece la pena tener nuestro propio sitio para ir haciendo nuestra "marca".

### Por qué tener tu propio blog
Escribir un blog requiere una gran inversión de tiempo normalmente, y es posible que muy poca gente lo lea; en mi caso es muy probable que solo lo lea yo 😅.

Dicho esto, por qué molestarte en tener tu propio blog, qué puedo aportar yo que no esté mil veces explicado por otros. En mi caso el motivo principal es poner por escrito en forma de guía cosas que voy aprendiendo en mi trabajo o en ratos que echo en aprender nuevas cosas que después me pueden servir en el trabajo. Siempre(casi) tomo notas de todo lo que hago/instalo/configuro, así cuando cinco minutos después he olvidado lo que hice, puedo volver a mis notas. 

Un blog me obliga(o eso espero) a invertir algo más de tiempo en mis notas para ciertos proyectos o ideas, y no hay mejor forma de aprender algo que explicar para otros lo que estás haciendo. Otro motivo por supuesto es compartir lo que hago para que pueda ser útil a otros, de la misma forma que yo aprendo de muchas otras personas del mundillo que dedican un montón de tiempo en sus propios blogs.
También quiero usar este blog como una  excusa para practicar escritura técnica y exposición de ideas, que siempre viene bien 😊.

### Herramientas
Además de la mencionada  [Hugo](https://gohugo.io/), para crear los posts en Markdown utilizo [Obsidian](https://obsidian.md/), también recomendada por [Graham Helton](https://www.twitter.com/grahamhelton3), y que yo descubrí sobre todo gracias a [Belén](https://twitter.com/ladybenko), a la que sigo en Twitter y que cada dos por tres explica por qué y cómo utilizarlo para tomar notas de manera mucho más eficiente a como yo lo hacía hasta ahora con otras herramientas (Cherrytree, Joplin, y otras). Personalmente, a raíz de sus hilos, yo he empezado a utilizarla en mi trabajo como una Wiki para consumo interno, y estoy encantado con el cambio. Otra buena opción es [VSCode](https://code.visualstudio.com/), que yo suelo utilizar más para scripting, ya sea en Python o Powershell, o para configuraciones de Terraform y Ansible. Y específica para Markdown me gusta mucho también [Typora](https://typora.io/), muy intuitiva y minimalista.

En esta guía voy a explicar como utilizar todo esto para crear tu blog usando **Windows** como SO de base - si, lo se, ¡Sacrilegio!. Lo reconozco, uso Windows como mi SO principal, porque gran parte de mi trabajo consiste en hacer documentación y presentaciones, para esto me manejo mucho mejor con el paquete Office de Microsoft, así que directamente trabajo en Windows y utilizo [WSL](https://docs.microsoft.com/en-us/windows/wsl/) con [Windows Terminal](https://aka.ms/terminal) cuando necesito usar Linux. 

Por último, para utilizar Hugo desde la línea de comandos, junto con otras muchas herramientas, utilizo [Chocolatey](https://chocolatey.org/), un gran gestor de paquetes para Windows. Para instalarlo, es muy fácil, sigue las instrucciones [aquí](https://chocolatey.org/install) desde tu terminal de Windows 😊.

### Preparando el terreno en local
Una vez que tenemos Windows Terminal y Chocolatey instalado, podemos empezar con la configuración de nuestra infraestructura para el blog.

#### Instalando [Hugo](https://gohugo.io/) y creando nuestro blog
Abrimos Terminal y ejecutamos:
```bash
choco install hugo-extended -confirm
```

![hugo installation command](/images/post1/hugo_install.png)

Y ya está, Hugo instalado, ¿A que mola chocolatey?
El siguiente paso es crear un nuevo proyecto, hacemos un nuevo directorio, por ejemplo 'blog', y ejecutamos:
```bash
mkdir blog
cd blog
hugo new site .
```

Esto crea la estructura necesaria para nuestro sitio web.
#### Eligiendo un tema para nuestro blog
Con Hugo podemos elegir entre muchos temas disponibles, para ver el catálogo disponible podemos ir al sitio de temas en esta [página](https://themes.gohugo.io/).
Yo he elegido el tema [Beautiful Hugo](https://github.com/halogenica/beautifulhugo), aunque el [Hello-friend-NG](https://github.com/rhazdon/hugo-theme-hello-friend-ng) también está muy bien para empezar.

![temas_hugo](/images/post1/temas.png)

Una vez elegido, vamos a la página de GitHub del tema y copiamos el enlace para añadir el tema a nuestro blog.
Para ello vamos simplemente a clonar el repositorio en un directorio que debe llamarse 'themes'.
```bash
mkdir themes
cd themes
git clone https://github.com/halogenica/beautifulhugo
```

![instalando tema](/images/post1/install_beautiful_hugo.PNG)

El siguiente paso es copiar un par de archivos de configuración de el sitio de ejemplo que viene con el tema elegido. Copiamos ` themes\beautifulhugo\exampleSite\config.toml`
a la raiz de nuestro blog y también `\themes\beautifulhugo\archetypes\default.md` al directorio archetypes de nuestro blog, sobrescribiendo el archivo por defecto.

_Config.toml_ contiene varios parámetros de configuración para el blog, y _default.md_
se utiliza como template para los nuevos posts que creemos con _Hugo_.

#### Configuración básica
Podemos (debemos) editar el archivo de configuración  copiado del tema (config.toml) para adaptarlo a nuestro blog.
```yaml
baseurl = "https://cibermanchego.com"
DefaultContentLanguage = "en"
publishDir = "docs"
title = "Cibemanchego"
theme = "beautifulhugo"
metaDataFormat = "yaml"
pygmentsStyle = "trac"
pygmentsUseClasses = true
pygmentsCodeFences = true
pygmentsCodefencesGuessSyntax = true

[Params]

#  homeTitle = "Beautiful Hugo Theme" # Set a different text for the header on the home page

 subtitle = "Miguelitos and cybersecurity"
 mainSections = ["post","posts"]
 logo = "img/avatar.jfif" # Expecting square dimensions
 favicon = "img/favicon.ico"
 dateFormat = "January 2, 2006"

```

En mi caso he cambiado la URL a la de mi sitio, el título, el logo y el **publishDir**, lo demás lo he dejado por defecto. He puesto como **publishDir** _docs_ y ahí se generará todo el contenido cuando tengamos nuestro posts listos para publicar.

#### Probando en local
Hugo nos permite probar nuestro sitio en local antes de publicar, además los cambios que hagamos se actualizan en tiempo real, lo que nos permite comprobar cambios sobre la marcha.
Creamos primero un post de prueba con Hugo:

```
hugo new posts\nuevo_post.md
```

Este comando utiliza el archivo de `archetypes\default.md` para crear el post, y genera un archivo de Markdown con unos valores iniciales donde podemos añadir, tags y demás, y que podemos encontrar en `content\posts\nuevo_post.md`

![nuevo post](/images/post1/nuevo_post.PNG)

Ahora ya es cosa nuestra añadir nuestro contenido con montones de exploits y vulnerabilidades nuevas - o una sencilla guía como esta :).

Ejecutamos el servidor local de hugo para ver el resultado; desde la raiz de nuestro blog:
```
hugo server
```
Al final del terminal vemos que podemos acceder a nuestro blog via http://localhost:1313

![hugo server](/images/post1/hugo_server.PNG)

> El Alien de la captura es el fondo de pantalla de mi terminal de Windows, está guapo eh 😛

Y vemos el resultado con el tema aplicado en todo su esplendor.

![blog](/images/post1/blog.PNG)

#### Subiéndolo a Internet
Una vez que tenemos un nuevo post listo para publicar, el siguiente paso sería decidir como vamos a poner nuestro flamante blog ahí afuera para que todo el mundo lo vea.
Una buena opción es usar **GitHub pages**, que fue mi opción inicial hasta decidirme por un bucket de AWS. En este [post](https://4bes.nl/2021/08/29/create-a-website-with-hugo-and-github-pages/) podéis seguir una guía sencilla de como configurarlo de esta manera. 

Yo he preferido seguir el ejemplo de [Graham](https://www.twitter.com/grahamhelton3) y utilizar AWS con mi propio dominio. He seguido sus pasos que explico a continuación.

Pero primero debemos generar nuestro blog con Hugo. Una vez que estamos satisfechos con nuestro nuevo post, ejecutamos desde la raiz del blog:
```
hugo
```

Y ya está, esto genera todo los archivos necesarios y los copia en **docs**, ya que lo hemos especificado así en el archivo de configuración. Ahora necesitamos configurar el sitio desde donde nuestro blog será accesible públicamente.

#### Configurando nuestro dominio y S3 bucket
Creamos una cuenta en AWS si no la tenemos ya y vamos a la consola de [route53](https://console.aws.amazon.com/route53/v2/home#Dashboard) para comprar un nuevo dominio. Hacemos click en **Register Domain** y elegimos un dominio que nos guste y esté disponible. En mi caso he elegido cibermanchego.com, que cuesta 12$ al año. Hay opciones más baratas, pero no hay mucha diferencia y ya que voy a usar AWS para alojar el blog, así tengo todo en un mismo sitio.

![dominio](/images/post1/select_domain.png)

El siguiente paso es crear el bucket donde vamos a subir todos los archivos de nuestro blog.

> **Es importante tener claro que este bucket va a ser de acceso público, así que cuidado con poner aquí archivos privados**

Creamos el bucket con el mismo nombre que el dominio que hemos comprado en el paso anterior.

![bucket1](/images/post1/bucket1.PNG)

Desbloqueamos el acceso público, pues es justo lo que necesitamos.

![bucket2](/images/post1/bucket2.PNG)

Lo siguiente es habilitar la opción de *Static Web hosting*, y añadir **index.html** como _Index Document_

![web hosting](/images/post1/web_hosting.PNG)

Añadimos la política de acceso público, copiando el siguiente texto y cambiando la URL a la de nuestro dominio:

```json
{
    "Version": "2008-10-17",
    "Id": "PolicyForPublicWebsiteContent",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cibermanchego.com/*"
        }
    ]
}
```

Lo pegamos en _Permissions - Bucket Policy_ de nuestro bucket:

![bucket policy](/images/post1/bucket_policy.PNG)

El bucket ya está listo para ser usado en nuestro blog, pero todavía tenemos que configurar el DNS de nuestro nuevo dominio.
En la consola de [Route53](https://console.aws.amazon.com/route53/v2/hostedzones#) vamos a _Hosted Zones_ donde la zona de nuestro dominio debería ya estar creada.
Añadimos un nuevo registro te tipo A y lo asociamos a nuestro bucket.

![record A](/images/post1/record_a.PNG)

También podemos crear otro registro de tipo CNAME para asociar _www_ a nuestro dominio.

![record CNAME](/images/post1/record_cname.PNG)

#### Subiendo nuestro blog al bucket

Solo nos queda subir los archivos generados por Hugo a nuestro bucket.
Vamos a nuestra consola de [S3](https://s3.console.aws.amazon.com/s3/home), a nuestro bucket, y hacemos click en **Upload**

![upload](/images/post1/upload.PNG)

Aquí desde el navegador de Windows arrastramos y soltamos todo el contenido del directorio **docs** donde antes se generó el contenido de nuestro blog.

Todo el proceso de generación de un nuevo post y subida de archivos a S3 para hacerlo público en nuestro blog se puede "automatizar",
esa es parte de la gracia de usar Hugo y AWS S3, pero eso igual ya lo explico en otro post 😉.

Ya solo nos queda abrir en nuestro navegador nuestro nombre de dominio, y ahí deberíamos ver en todo su esplendor nuestra primera entrada.

