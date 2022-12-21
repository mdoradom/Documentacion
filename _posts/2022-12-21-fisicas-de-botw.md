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

# Como hemos tomado medidas

![](/assets/images/botw/separador.png)

# Caída libre

<iframe src="http://www.youtube.com/embed/X8050fWKUV4" width="560" height="315" frameborder="0" allowfullscreen></iframe>

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
Vx = |V| * cos α

vel vertical → MRUA (aceleración gravedad) vel horizontal * tan (sen/cos)

$$ V_y = \left | V \right |\cdot \sin\alpha $$

(substituir módulo por velocidad horizontal)

$$ V_y = (\frac{V_x}{\cos\alpha}) \cdot \sin\alpha $$

(relación trigonométrica sin α / cos α = tan α)

$$ V_y = V_x \cdot \frac{\sin\alpha}{\cos\alpha} $$ 

$$ V_y = V_x \cdot \tan\alpha $$ 

-34,65 m/s2

# Catapulta