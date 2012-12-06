Por qué mola GNU Screen
=======================

Introducción
------------

Este pequeño tutorial es para explicaros lo (poco) que sé sobre GNU Screen
y cómo sacarle provecho a esas largas sesiones de SSH en la consola de 
vuestros servidores, y es que, cuando trabajas en remoto tanto, a veces
necesitas más de una pantalla. Y es por eso que seguramente algún _BOFHER_
escribió este programa.

Y a petición de @ElAutoestopista os escribo este pequeño documento para que
los que no lo conozcan, metan esta útil herramienta en ese arsenal que todo
buen _sysadmin_ va construyendo y siempre tiene a mano para salvar el día y
el culo de los _lusers_.

¿Cómo usar este documento?
--------------------------

Puedes leerlo, probar las cosas que cuento aquí, puedes coger el código
fuente y alterarlo a placer, ya que este texto esta licenciado bajo una
**Creative Commons 3.0 BY-SA** 
[![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/80x15.png)](http://creativecommons.org/licenses/by-sa/3.0/deed.es_ES)
y podéis conseguir el fichero Markdown de este documento (su código fuente) 
en <https:////github.com/resetreboot/mola_screen>.

GNU Screen: ¿Para qué sirve?
----------------------------

_GNU Screen_ es una herramienta para héroes de la consola, de esos que te 
hacen _BASH one-liners_, que saben cientos de _comandos rúnicos_ que
desatan la magia del sistema. Y es que en más de una ocasión nos hemos 
encontrado con que necesitamos **más de una consola** con la que trabajar.
Alguno dirá que para eso su terminal tiene pestañas, o usan el genial
_Terminator_ para dividir su pantalla en varias consolas a la vez, pero o
bien te obligan a usar el ratón o necesitamos **un extra** para lo que
pretendemos hacer.

¿Nunca os ha pasado que se necesita lanzar un proceso, vigilando la salida
del mismo por _stdout_, en un servidor remoto pero va a tardar horas y 
es hora de irse a casa? _Screen_ tiene la solución para ese tipo de problemas.

Y es que _Screen_ permite tener varias pantallas virtuales a la vez, cambiar
entre ellas a golpe de teclado e incluso hacer un _detach_ de la pantalla y
salir del sistema sabiendo con seguridad que el proceso no se ha cerrado y
que su verborrea se quedará almacenada en algún lugar, esperando a que 
volvamos y la enganchemos de nuevo, como si no hubiera pasado nada.

Primeros pasos
--------------

Lanzar _Screen_ es tan sencillo como teclear en nuestra línea de comandos
favorita `$ screen` y nos saludará con una pantalla con la versión, la 
licencia y nos invita a pulsar la barra espaciadora para que nos cuente más
o bien presionar _Enter_ para comenzar a usarlo.

Lo siguiente que veréis es el _prompt_ de vuestro intérprete de comandos
y parecerá que nada ha cambiado. Pero probad a entrar en algún directorio y 
luego un `ls` para obtener el listado de ficheros. Después, pulsad `Ctrl-A`
y justo después la tecla `c`. El listado de ficheros habrá desaparecido y
de nuevo el _prompt_ os saludará. Hemos abierto una nueva ventana, para hacer
otra cosa sin mayor problema. ¿Cómo recuperamos ese listado que estábamos
mirando? Pulsamos `Ctrl-A` y luego la tecla `0`. En _Screen_ las ventanas que
vamos creando se numeran del 0 al número de ventanas que necesitamos. La
primera es el 0, la nueva es el 1. Así, que pulsando `Ctrl-A` y luego la tecla
`1` recuperaremos el _prompt_ nuevo que hemos creado.

Como se puede ver, _Screen_ procura no molestarte poniendo más cosas en la 
pantalla, pero cuando tengas un buen puñado de ventanas abiertas, a veces te
preguntarás en cual de ellas estas. `Ctrl-A` y luego `w` te mostrará un
listado de ventanas, con el número o **el nombre** de las ventanas abiertas,
con el nombre del proceso que funciona dentro de esa ventana y un `*` que te
indica en qué ventana estas.
