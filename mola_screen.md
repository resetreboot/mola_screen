# Por qué mola GNU Screen

## Introducción

Este pequeño tutorial es para explicaros lo (poco) que sé sobre GNU Screen
y cómo sacarle provecho a esas largas sesiones de SSH en la consola de 
vuestros servidores, y es que, cuando trabajas en remoto tanto, a veces
necesitas más de una pantalla. Y es por eso que seguramente algún *BOFHER*
escribió este programa.

Y a petición de @ElAutoestopista os escribo este pequeño documento para que
los que no lo conozcan, metan esta útil herramienta en ese arsenal que todo
buen *sysadmin* va construyendo y siempre tiene a mano para salvar el día y
el culo de los *lusers*.

## ¿Cómo usar este documento?

Puedes leerlo, probar las cosas que cuento aquí, puedes coger el código
fuente y alterarlo a placer, ya que este texto esta licenciado bajo una
**Creative Commons 3.0 BY-SA** 
[![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/80x15.png)](http://creativecommons.org/licenses/by-sa/3.0/deed.es_ES)
y podéis conseguir el fichero Markdown de este documento (su código fuente) 
en <https:////github.com/resetreboot/mola_screen>.

## GNU Screen: ¿Para qué sirve?

*GNU Screen* es una herramienta para héroes de la consola, de esos que te 
hacen *BASH one-liners*, que saben cientos de *comandos rúnicos* que
desatan la magia del sistema. Y es que en más de una ocasión nos hemos 
encontrado con que necesitamos **más de una consola** con la que trabajar.
Alguno dirá que para eso su terminal tiene pestañas, o usan el genial
*terminator* para dividir su pantalla en varias consolas a la vez, pero o
bien te obligan a usar el ratón o necesitamos **un extra** para lo que
pretendemos hacer.

¿Nunca os ha pasado que se necesita lanzar un proceso, vigilando la salida
del mismo por *stdout*, en un servidor remoto pero va a tardar horas y 
es hora de irse a casa? *Screen* tiene la solución para ese tipo de problemas.

Y es que *Screen* permite tener varias pantallas virtuales a la vez, cambiar
entre ellas a golpe de teclado e incluso hacer un *detach* de la pantalla y
salir del sistema sabiendo con seguridad que el proceso no se ha cerrado y
que su verborrea se quedará almacenada en algún lugar, esperando a que 
volvamos y la enganchemos de nuevo, como si no hubiera pasado nada.

## Primeros pasos

Lanzar *Screen* es tan sencillo como teclear en nuestra línea de comandos
favorita `$ screen` y nos saludará con una pantalla con la versión, la 
licencia y nos invita a pulsar la barra espaciadora para que nos cuente más
o bien presionar *Enter* para comenzar a usarlo.

Lo siguiente que veréis es el *prompt* de vuestro intérprete de comandos
y parecerá que nada ha cambiado. Pero probad a entrar en algún directorio y 
luego un `ls` para obtener el listado de ficheros. Después, pulsad `Ctrl-a`
y justo después la tecla `c`. El listado de ficheros habrá desaparecido y
de nuevo el *prompt* os saludará. Hemos abierto una nueva ventana, para hacer
otra cosa sin mayor problema. ¿Cómo recuperamos ese listado que estábamos
mirando? Pulsamos `Ctrl-a` y luego la tecla `0`. En *Screen* las ventanas que
vamos creando se numeran del 0 al número de ventanas que necesitamos. La
primera es el 0, la nueva es el 1. Así, que pulsando `Ctrl-a` y luego la tecla
`1` recuperaremos el *prompt* nuevo que hemos creado.

Como se puede ver, *Screen* procura no molestarte poniendo más cosas en la 
pantalla, pero cuando tengas un buen puñado de ventanas abiertas, a veces te
preguntarás en cual de ellas estas. `Ctrl-a` y luego `w` te mostrará un
listado de ventanas, con el número o **el nombre** de las ventanas abiertas,
con el nombre del proceso que funciona dentro de esa ventana y un `*` que te
indica en qué ventana estas.

Puede suceder que tener un `(1)\*bash $` como nombre de una ventana no 
resulte muy descriptivo, pero *Screen* nos permite darles nombres a las 
ventanas, con `Ctrl-a` y luego `A` (ojo, mayúscula) podremos bautizar a la 
ventana actual con el nombre que deseemos. Así, al pedir el listado de 
ventanas, veremos sus nombres y podremos identificarlas rápidamente.

## Pantalla partida

*Screen* permite tener más de una ventana en nuestra pantalla, de forma que
puedas tener más de una consola o programa a la vez, y así controlar diversos
procesos sin mayor problema. Un gran ejemplo de este funcionamiento es el
plugin de *user list* para `irssi`, que requiere de *Screen* para funcionar,
partiendo la pantalla en dos partes; `irssi` y la salida del plugin, en
vertical.

La forma básica de gestionar ventanas es la siguiente.

* `Ctrl-a` + `S`: Divide la pantalla en dos, horizontalmente.
* `Ctrl-a` + `<Tab>`: Pasa de una ventana a la siguiente.
* `Ctrl-a` + `Q`: Elimina todas las ventanas y te deja con una sola.
* `Ctrl-a` + `X`: Elimina la ventana actual.

## Sesiones

Ya he comentado las funciones de *detach*, como la que uso en un servidor de
Minecraft que llevo. Esta hecho en Java y Notch (el creador) no le ha puesto
un modo *daemon* que permita lanzarlo sin tener una salida por consola. Pues
bien, gracias a *Screen* lanzo el servidor, tengo la consola de control y con
`Ctrl-a` y luego la tecla `d` la sesión se desacopla de la consola y puedo
cerrar mi sesión de SSH con el servidor, sabiendo que el programa continúa 
funcionando. Cuando deseo reiniciar el servidor, o bien lanzar algún comando,
sólo tengo que invocar a *Screen* tal que así:

    $ screen -r

Que le indica que deseo reconectar con la sesión que tengo iniciada en alguna
parte. El problema viene cuando tienes más de una sesión de *Screen* iniciada
e intentas recuperar alguna. El programa te avisará de que tienes más de una
sesión y que debes de indicarle el nombre de la sesión que deseas recuperar
justo después del parámetro `-r`.

Además, si sueles tener varias sesiones de *Screen* en tu servidor, y las
abres y cierras constantemente `screen -ls` te listará las sesiones abiertas
actualmente, y asi escoger la que realmente quieres recuperar.

Otra utilidad muy chula, especialmente si llevas un *padawan*, es compartir
una sesión de *Screen* ya abierta, pudiendo más de una persona ver lo mismo
como una sesión colaborativa. Por cierto, por razones de seguridad, sólo un
usuario puede conectarse a una sesión abierta por ese mismo usuario. Así que
ya sabéis.

Para conectar a una sesión que ya esta abierta:

    $ screen -x

Y ya está, consola multiplayer al canto para todos. Para salir de la sesión,
 usando el comando de *detach* abandonaréis la sesión sin cerrarla y sin 
fastidiar a los otros que estén conectados en ese momento.

## Movimientos y gestión del *buffer*

Al usar *Screen* la gestión del *buffer* ya no corre a cargo de nuestra 
terminal, si no del propio *Screen*, con lo que si desplazais el mismo en 
vuestra terminal, os llevaréis una pequeña decepción. 

Por supuesto, existen comandos que nos permitirán gestionar el *buffer* y 
copiar los contenidos del mismo e incluso realizar una especie de captura de 
pantalla. Para empezar, `Ctrl-a` + `ESC` activará el llamado *copy mode* y 
podréis desplazaros por el *buffer* usando las teclas de cursor, además, con 
la barra espaciadora podremos marcar un inicio y un fin de aquello que 
deseamos copiar. Tras pulsar *Enter* lo seleccionado se almacenará y *Screen* 
saldrá del *copy mode*. Si deseáis salir del modo sin copiar nada, 
pulsad `ESC` de nuevo. 

Si te has decidido a copiar algo, para poder pegarlo hay que pulsar 
`Ctrl-a` + `] ` y aparecerá a partir de la posición actual del cursor. 
