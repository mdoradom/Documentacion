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

<iframe src="https://www.youtube.com/embed/X8050fWKUV4" width="560" height="315" frameborder="0"></iframe>

## MRUA

### Tiempo en el juego

$$ X(t) = X_{0} + V_{0}t + \frac{1}{2} \cdot at^{2} $$

0 (altura suelo) = 78,2m (altura torre) + 0 + ½ * 2,152 (tiempo q tarda en caer) * a

$$  X(0) = 78.2 + 0 + \frac{1}{2} \cdot 2.152 \cdot a^{2} $$

**-33.83 m/s<sup>2</sup>**


### Tiempo en la vida real

$$ X(t) = X_{0} + V_{0}t + \frac{1}{2} \cdot at^{2} $$

0 (altura suelo) = 78,2m (altura torre) + 0 + ½ * t^2 (tiempo q tarda en caer) * 9,81

$$  X(0) = 78.2 + 0 + \frac{1}{2} \cdot t \cdot 9.81^{2} $$

**3.99 segundos**

# Tiro parabólico

38 link/s vel horizontal → const, porque hay poca fricción con aire

$$ V_x = \left | V \right |\cdot \cos\alpha $$

vel vertical → MRUA (aceleración gravedad) vel horizontal * tan (sen/cos)

$$ V_y = \left | V \right |\cdot \sin\alpha $$

(substituir módulo por velocidad horizontal)

$$ V_y = (\frac{V_x}{\cos\alpha}) \cdot \sin\alpha $$

(relación trigonométrica sin α / cos α = tan α)

$$ V_y = V_x \cdot \frac{\sin\alpha}{\cos\alpha} $$ 

$$ V_y = V_x \cdot \tan\alpha $$ 

**-34,65 m/s<sup>2</sup>**

# Catapulta

Primero necesitamos saber la velocidad a la que sale despedido link. Teniendo en cuenta la **gravedad**, el **tiempo** que tarda en caer y la **posición inicial**.

**MRUA:**

$$ X(t) = X_{0} + V_{0}t + \frac{1}{2} \cdot at^{2} $$

$$ 0 = 2.55 + V_{0} \cdot 2.44 + \frac{1}{2} \cdot (-33.34) \cdot 2.44^{2} $$

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

$$ Ep = m \cdot g \cdot h $$

$$ m \cdot \left | -33.34 \right | \cdot 17.85 = 283.03kJ $$

$$ m = \frac{283030}{\left | -33.34 \right | \cdot 17.85} = 475.59kg $$

475.59kg es la masa mínima que tendría que tener la piedra para lanzar disparado a Link a esa velocidad.

Para comprobar si en el juego, se conservan las energías, hemos mirado la energía potencial en el punto máximo de altura y la hemos comparado con la energía cinética que tiene Link al ser lanzado.

$$ Ep = m \cdot g \cdot h $$

$$ Ep = 65 \cdot \left | -33.34 \right | \cdot 21.7 - 2.55 = 47.02kJ$$

$$ 283.03 - 47.02 = 236.01kJ $$

Hay una pérdida de energía de 236.01kJ porque motivos (tirarse un triple)