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

Hemos decidido analizar el videojuego **The Legend of Zelda: Breath of the Wild** ya que es un título el que uno de sus logros, es haber creado un sistema de físicas bastante elaborado que interactua entre si.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">BotW physics in a nutshell <a href="https://twitter.com/hashtag/BreathoftheWild?src=hash&amp;ref_src=twsrc%5Etfw">#BreathoftheWild</a> <a href="https://twitter.com/hashtag/NintendoSwitch?src=hash&amp;ref_src=twsrc%5Etfw">#NintendoSwitch</a> <a href="https://t.co/o341FlzcpI">pic.twitter.com/o341FlzcpI</a></p>&mdash; Pinwheel Popper (@PinwheelPopper) <a href="https://twitter.com/PinwheelPopper/status/1162063176520704001?ref_src=twsrc%5Etfw">August 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<iframe id="reddit-embed" src="https://www.redditmedia.com/r/Breath_of_the_Wild/comments/xkvzbc/man_you_really_constantly_break_weapons_shit_game/?ref_source=embed&amp;ref=share&amp;embed=true&amp;theme=dark" sandbox="allow-scripts allow-same-origin allow-popups" style="border: none;" height="407" width="640" scrolling="no"></iframe>

# Como hemos tomado medidas

![](/assets/images/botw/separador.png)

# Caída libre

<iframe src="https://www.youtube.com/embed/X8050fWKUV4" width="560" height="315" frameborder="0"></iframe>

## MRUA

### Tiempo en el juego

$$ X(t) = X_{0} + V_{0}t + 1/2 \cdot at^{2} $$

0 (altura suelo) = 78,2m (altura torre) + 0 + ½ * 2,152 (tiempo q tarda en caer) * a

$$  X(0) = 78.2 + 0 + 1/2 \cdot (2.152 * a)^{2} $$

**-33.83 m/s<sup>2</sup>**


### Tiempo en la vida real

$$ X(t) = X_{0} + V_{0}t + \frac{1}{2} \cdot at^{2} $$

0 (altura suelo) = 78,2m (altura torre) + 0 + ½ * t^2 (tiempo q tarda en caer) * 9,81

$$  X(0) = 78.2 + 0 + \frac{1}{2} \cdot (t * 9.81)^{2} $$

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