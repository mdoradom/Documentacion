---
title: Físicas de The Legend of Zelda - Breath of the Wild
author: [mario, marta]
date: 2022-12-21 12:00:00 +0800
categories: [Fisica]
tags: [fisica, analisis, videojuegos]
render_with_liquid: false
math: true
image:
  path: /assets/images/botw/portada.png
  width: 1366   # in pixels
  height: 768   # in pixels
  alt: Físicas de The Legend of Zelda - Breath of the Wild
---

# Introducción

![](/assets/images/botw/separador.png)

Hemos decidido analizar el videojuego **The Legend of Zelda: Breath of the Wild** ya que uno de sus grandes logros, es haber creado un sistema de físicas bastante elaborado que interactua entre si.

Este sistema contempla que los metales son conductores, entonces si utilizas armas eléctricas, puedes crear corrientes eléctricas para así resolver puzzles.

![](https://i.imgur.com/miBXgFC.jpeg)

También, los objetos tienes propiedades físicas básicas. Por ejemplo, si están en una tabla y esa tabla es empujada por un objeto pesado, los objetos son disparados.

<blockquote class="twitter-tweet" data-theme="dark"><p lang="en" dir="ltr">BotW physics in a nutshell <a href="https://twitter.com/hashtag/BreathoftheWild?src=hash&amp;ref_src=twsrc%5Etfw">#BreathoftheWild</a> <a href="https://twitter.com/hashtag/NintendoSwitch?src=hash&amp;ref_src=twsrc%5Etfw">#NintendoSwitch</a> <a href="https://t.co/o341FlzcpI">pic.twitter.com/o341FlzcpI</a></p>&mdash; Pinwheel Popper (@PinwheelPopper) <a href="https://twitter.com/PinwheelPopper/status/1162063176520704001?ref_src=twsrc%5Etfw">August 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Otro ejemplo que nos hemos encontrado mientras grabábamos el material necesario para el análisis, es que si disparas una flecha contra piedra, la flecha queda suelta, y las ráfagas de viento las hacen rodar por el suelo, sin embargo, si es disparada contra una superficie en la que hay tierra o césped, la flecha queda clavada y no se mueve por las ráfagas de viento.

<iframe src="https://www.youtube.com/embed/nTexDIfNbLU" width="560" height="315" frameborder="0"></iframe>

<iframe src="https://www.youtube.com/embed/D3FdJk9vwDA" width="560" height="315" frameborder="0"></iframe>

# Como hemos tomado medidas

![](/assets/images/botw/separador.png)

# Caída libre

![](/assets/images/botw/separador.png)

Primero de todo, hemos analizado una caída libre lanzando al personaje de Link desde una torre, así podremos saber la gravedad que hay dentro del videojuego y ver si es un valor realista.

<iframe src="https://www.youtube.com/embed/TtMEWS2lYz0" width="560" height="315" frameborder="0"></iframe>

![](/assets/images/botw/torre1.png){: width="1920" height="1080" }
_Comparativa de tamaño entre Link y la torre_

<iframe src="https://www.youtube.com/embed/wsFKrTRr9J4" width="560" height="315" frameborder="0"></iframe>

- **h suelo:** 0 m
- **h torre:** 78.2 m
- **t caída:** 2.152 s
- **a:** ?

Al ser una caída libre, hemos utilizado la forma del MRUA, ya que es un movimiento acelerado por la gravedad.

## Tiempo en el juego

$$ X(t) = X_{0} + v_{0}t + \frac{1}{2} \cdot at^{2} $$

$$  0 = 78.2 + 0 + \frac{1}{2} \cdot 2.152 \cdot a^{2} $$

$$ a^{2} = \frac{-78.2 - \frac{1}{2}}{2.152} = ¿¿¿¿-33.83????$$

Haciendo los cálculos, podemos observar que la fuerza de la gravedad es de unos **-33.83 m/s<sup>2</sup>** o **-19.9 links/s<sup>2</sup>**, lo cual es un valor muy superior al de la tierra (**9.8 m/s<sup>2</sup>**), esto significa que, en el mundo de The Legend of Zelda: Breath of the Wild, hay mas gravedad que en Júpiter (**24.79 m/s<sup>2</sup>**).

## Tiempo en la vida real

Haciendo los cálculos con la gravedad que tenemos en el planeta tierra, podemos obtener el tiempo que tardaría Link en caer desde lo alto de la torre hasta el suelo y ver la diferencia con los tiempos que hemos medido dentro del juego.

Utilizamos la formula del **MRUA:**

$$ X(t) = X_{0} + v_{0}t + \frac{1}{2} \cdot at^{2} $$

$$  0 = 78.2 + 0 + \frac{1}{2} \cdot t \cdot 9.81^{2} $$

Haciendo los cálculos, obtenemos que la misma caída con unos valores de gravedad realistas, la caída duraría **3.99 segundos**.

# Tiro parabólico

![](/assets/images/botw/separador.png)

Para analizar que factores físicos tiene en cuenta el videojuego a la hora de realizar un tiro parabólico, hemos disparado una flecha con un arco.

Para calcular el tiro parabólico, necesitamos saber el **ángulo** en el cual sale disparada la flecha, **cuanto tiempo tarda** en impactar contra el suelo, a **cuanta distancia impacta**.

Para obtener el **ángulo**, **INSERTAR COSA DE MARTRA**

Para saber cuanto **tiempo tarda** en impactar contra el suelo, hemos disparado la flecha y cronometrado cuanto tiempo tarda en impactar el suelo.

<iframe src="https://www.youtube.com/embed/q6r8Fk71xdA" width="560" height="315" frameborder="0"></iframe>

Para medir la **distancia que ha recorrido la flecha**, hemos hecho una captura colocando el personaje desde la posición que se ha disparado la flecha, y otra dónde ha impactado la flecha, y utilizando al personaje como forma de medida, hemos sacado la distancia.

![](/assets/images/botw/tiro_parabolico.png)

Al final con todos estos valores obtenemos que la flecha es disparada con unos **14.3 grados** de inclinación y recorre **64.6 metros** en **1 segundo**.

![](/assets/images/botw/tiro_parabolico.png)

A continuación, vamos a descomponer el vector velocidad en dos componentes: velocidad horizontal y velocidad vertical.

Para calcular la velocidad horizontal, tenemos que dividir la distancia que recorre la flecha entre el tiempo que tarda en recorrerla. Esto es así ya que asumimos que es una velocidad constante, ya que al ser una flecha, la fricción con el aire es mínima.

$$ \frac{64.6}{1} = 64.6$$

Esto nos da como resultado que la flecha tiene una velocidad horizontal de **64.6 m/s** o **38 links/s**.

vel vertical → MRUA (aceleración gravedad) vel horizontal * tan (sen/cos)

$$ V_x = \left | V \right |\cdot \cos\alpha $$

$$ V_y = \left | V \right |\cdot \sin\alpha $$

Substituimos el módulo por la velocidad horizontal.

$$ V_y = (\frac{V_x}{\cos\alpha}) \cdot \sin\alpha $$

Relación trigonométrica sin α / cos α = tan α

$$ V_y = V_x \cdot \frac{\sin\alpha}{\cos\alpha} $$ 

$$ V_y = V_x \cdot \tan\alpha $$ 

$$ V_y = 64.6 \cdot \tan 14.3^{\circ} = 16.47m/s $$

Esto nos da que la velocidad vertical es igual a **16.47 m/s** o **9.69 links/s**.

Para calcular la gravedad, hemos utilizado la formula del MRUA, con la componente de velocidad vertical, ya que para esta velocidad está influenciada por la aceleración que ejerce la gravedad, sin embargo, en la horizontal no.

**MRUA:**

$$ X(t) = X_{0} + v_{0}t + \frac{1}{2} \cdot at^{2} $$

$$ 0 = 0.85 + 16.47 \cdot 1 + \frac{1}{2} \cdot a \cdot 1^{2} $$

$$ a = \frac{(-0.85) - (-16.47) \cdot 1}{1^{2} \cdot \frac{1}{2}} = 32.09 m/s^{2}$$

*0.85 m = 0.5 links (altura inicial desde la que se lanza la flecha)

Obtenemos que, la fuerza de la gravedad es igual a: **-32.09 m/s<sup>2</sup>** o **-18.88 links/s<sup>2</sup>**

# Catapulta

![](/assets/images/botw/separador.png)

Primero necesitamos saber la velocidad a la que sale despedido link. Teniendo en cuenta la **gravedad**, el **tiempo** que tarda en caer y la **posición inicial**.

**MRUA:**

$$ X(t) = X_{0} + v_{0}t + \frac{1}{2} \cdot at^{2} $$

$$ 0 = 2.55 + v_{0} \cdot 2.44 + \frac{1}{2} \cdot (-33.34) \cdot 2.44^{2} $$

$$ V_{0} = 93.32 m/s $$

https://www.zonadiet.com/tablas/pesoideal.cgi

Peso link = 65kg

85 milésimas en salir despedido desde el contacto de la piedra

Velocidad inicial / Tiempo en salir despedido

$$ \frac{93.32}{0.085} = 1097.88 m/s^{2} $$

aceleración * masa = fuerza que recibe link

$$ 1097.88 \cdot 65 =  71.82kN $$

Este valor procede de estudios militares sobre paracaidistas: el cuerpo humano de un deportista podría soportar una deceleración máxima de unos 15 G, es decir, 12 kN para una masa de 80 kg.

https://www.petzl.com/ES/es/Sport/¿Como-se-ha-determinado-la-fuerza-de-choque-maxima-de-12kN-?ProductName=MAMBO-10-1-mm#:~:text=Este%20valor%20procede%20de%20estudios,una%20masa%20de%2080%20kg.

con la velocidad inicial calculamos la energía cinética

$$ \frac{1}{2} \cdot mv^{2}$$

$$ \frac{1}{2} \cdot 65 \cdot 93.32^{2} = 283.03kJ$$

Teniendo en cuenta la piedra no da toda la energía en el choque con la plataforma, no podemos saber la masa exacta de la piedra, pero podemos determinar la masa mínima, esto lo podemos saber igualando la energía potencial de la piedra con la energía cinética de link, al hacer esto, sacaríamos la masa de la piedra si disipara toda su energía al contactar con la plataforma y no al contactar con el suelo.

Para la altura, hemos cogido la altura total, porque, si pusiéramos la altura total - la plataforma, estaríamos diciendo que toda la energía se transfiere cuando la piedra toca la plataforma, pero eso no es verdad, así que asumimos que la energía es transferida de forma uniforme mientras la piedra empuja la plataforma y que transfiere toda la energía cuando toca el suelo. 

Así que esto es una aproximación de si toda la energía de la piedra fuera transferida a Link, lo cual es mentira porque en el vídeo se aprecia que la piedra aun tiene energía al tocar el suelo. Así que estos cálculos serían una cota inferior.

$$ Ep = m \cdot g \cdot h $$

$$ m \cdot \left | -33.34 \right | \cdot 17.85 = 283.03kJ $$

$$ m = \frac{283030}{\left | -33.34 \right | \cdot 17.85} = 475.59kg $$

475.59kg es la masa mínima que tendría que tener la piedra para lanzar disparado a Link a esa velocidad.

Para comprobar si en el juego, se conservan las energías, hemos mirado la energía potencial en el punto máximo de altura y la hemos comparado con la energía cinética que tiene Link al ser lanzado. 

$$ Ep = m \cdot g \cdot h $$

$$ Ep = 65 \cdot \left | -33.34 \right | \cdot 21.7 - 2.55 = 47.02kJ$$

$$ 283.03 - 47.02 = 236.01kJ $$

Hay una pérdida de energía de 236.01kJ porque motivos (tirarse un triple)

# Salto de Link

![](/assets/images/botw/separador.png)

<iframe src="https://www.youtube.com/embed/S1UJQTvV9-w" width="560" height="315" frameborder="0"></iframe>

![foto medidas salto](/assets/images/botw/salto.png)

**MRUA:**

$$ X(t) = X_{0} + v_{0}t + \frac{1}{2} \cdot at^{2} $$

$$ 0 = 0 + v_{0} \cdot 0.27 + \frac{1}{2} \cdot (-33.34) \cdot 0.27^{2} $$

$$ v_{0} = 0.95m/s $$

Link al saltar, tiene una velocidad inicial de 0.95 m/s.

No podemos calcular la fuerza con la que salta Link, ya que la animación es instantánea, no podemos medir cuanto tiempo Link está acelerando.

También hemos observado que el videojuego no tiene en cuenta la masa de Link, ya que salta la misma altura cuando tiene equipada una armadura y sus armas.

<iframe src="https://www.youtube.com/embed/kXUfvma6VYQ" width="560" height="315" frameborder="0"></iframe>

![foto medidas salto](/assets/images/botw/salto2.png)