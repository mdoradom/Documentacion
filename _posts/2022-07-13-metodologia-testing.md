---
title: Metodología sobre el testeo de software
author: mario
date: 2022-07-14 16:23:00 +0800
categories: [Testing]
tags: [testing]
math: true
mermaid: true
---

Metodología sobre el testeo de software
=======================================

Las metodologías de testeo de software son **estrategias** o **acercamientos** utilizados para **probar** que una **aplicación** **se ve** y **funciona** como debería. Este documento es **mi proceso** para realizar estos procesos de prueba, tanto en **Backend** como en **Frontend**.

Primero de todo, hay que hacer una distinción entre los tipos de pruebas:

## Pruebas Funcionales vs No-Funcionales

La principal razón de utilizar varias metodologías de pruebas, es para **asegurar** que el **software** en cuestión puede **operar en multiples entornos** y **sobre varias plataformas**.

Se puede hacer una división entre **dos tipos de pruebas**, **funcionales** y no **funcionales**:

### Pruebas Funcionales

Estas metodologías implican **probar la aplicación frente a los requisitos comerciales**. 

Incorpora todo tipos de **pruebas diseñadas** para **garantizar** que cada parte del software **funciona como debe**, esto lo hacemos utilizando **casos de uso**, **proporcionados** por el **equipo de diseño** o **analista de negocios**.

Estas Pruebas normalmente se llevan a cabo en **orden** e incluyen:

- Prueba unitaria (Unit Testing)

- Pruebas de integración

- Pruebas del sistema

- Test de aceptación

### Pruebas No Funcionales

Las pruebas no funcionales incorporan todo tipos de exámenes **enfocados** en el aspecto **operacional** de una pieza de software. Esto incluye:

- Prueba de rendimiento

- Prueba de seguridad

- Prueba de usabilidad

- Prueba de compatibilidad

La clave para que tu software tenga una **calidad alta** y sea **fácil de adoptar** por el usuario final, es crear un **marco de pruebas robusto** que implemente ambas metodologías, funcional y no funcional.

## Prueba Unitaria

Las **pruebas unitarias (o Unit Testing)** son el **primer nivel** de testeo y normalmente lo **llevan a cabo** los mismo **desarrolladores**. Es el proceso que asegura **individualmente** que cada pieza del software **funciona a nivel de código**.

Los **desarrolladores** en un entorno de desarrollo basado en pruebas, generalmente **escribirán** y **ejecutarán** las pruebas **antes** de que el software o la función se **pase al equipo de prueba**.

Las pruebas unitarias se pueden **realizar manualmente**, pero la **automatización** del proceso **acelerará** los ciclos de entrega y ampliará la **cobertura (coverage)** de los tests.

También hará que **debugar sea mas sencillo**, porque encontrar el error antes implica que se tarda menos tiempo en arreglarlo que si el error fuera descubierto después en el proceso de testeo.

## Pruebas de Integración

Después de que cada unidad sea probada minuciosamente, es momento de **integrarla con otras unidades** para crear **módulos** o **componentes** que son diseñados para realizar una **tarea** o **actividad específica**.

Son **probados como grupo** a través de pruebas de integración para garantizar que los segmentos completos de una aplicación se **comporten como esperas** (esto quiere decir, que las interacciones entre unidades son perfectas).

Estas pruebas a menudo se **enmarcan en escenarios de usuario**, como iniciar una sesión en una aplicación o abrir archivos.

Estas pruebas las pueden **realizar** los **desarrolladores** o **evaluadores independientes** y generalmente se componen de una combinación de **pruebas manuales** y **funcionales automatizadas**.

## Pruebas de Sistema

Las pruebas de sistema son pruebas de **caja negra (Black Box)** que se utiliza para **evaluar el sistema completo e integrado**, como un todo, para garantizar que cumpla los requisitos especificados.

La **funcionalidad** del software se prueba de **principio a fin** y, por lo general, la lleva a cabo un **equipo de pruebas independiente** del equipo de desarrollo.

## Pruebas de Aceptación

Esta es la **ultima fase** de las pruebas funcionales y se utiliza para **evaluar** si la pieza final de software está **lista para entregar** o **no**. 

**Implica garantizar** que el producto **cumpla** con todos los **criterios comerciales originales** y que **satisfaga las necesidades** del usuario final.

Esto requiere que el producto se pruebe tanto **interna**, como **externamente**, lo que significa que deberá **ponerse en manos** de sus **usuarios finales** para realizar **beta testing** junto con el **equipo de control de calidad**. La prueba beta es clave para obtener comentarios reales de los clientes potenciales y puede abordar cualquier inquietud final sobre la usabilidad.

## Pruebas de rendimiento

Las pruebas de rendimiento son pruebas no funcionales que determinan como una aplicación **rendirá bajo ciertas condiciones**. El objetivo es probar la **sensibilidad** y **estabilidad** en una **situación de usuario real**.

Los test de rendimiento pueden ser divididos en 4 tipos:

- **Prueba de carga (Load Testing):** es el proceso de poner **cantidades incrementales** de demanda simulada en el software, para verificar que puede **asumir la carga** para lo que está diseñado o no.

- **Prueba de estrés (Stress Testing):** es coger la **prueba de carga** y **llevarla un paso mas allá**. Se usa para medir cómo **responderá el software** en su **carga máxima** o **mas allá**. El objetivo es **sobrecargar** la aplicación a propósito hasta que llegue a **punto crítico y rompa**. Esto se hace tanto en escenarios realistas, como poco realistas.

- **Prueba de resistencia (Endurance Testing / Soak Testing):** Se utiliza para **analizar** el **comportamiento del software** bajo una carga simulada determinada en un **periodo prolongado en el tiempo**. El objetivo es entender como tu sistema se comportará bajo un uso prolongado, haciendo así esta prueba un **proceso mas largo** que las pruebas de estrés o carga (las cuales están diseñadas para acabar en unas pocas horas). Una parte crucial de las pruebas de resistencia es que ayuda a **descubrir fugas de memoria**.

- **Prueba de picos (Spike Testing):** es un **tipo de test de carga** que se usa para determinar como el software responderá a una ráfaga **sustancialmente mas grande** de **actividad simultánea de usuarios o del sistema** durante períodos de **tiempo variables**. Idealmente esto ayudará a comprender qué sucederá cuando la carga aumente de forma **repentina** y **drástica**.

## Pruebas de seguridad

Son un tipo de pruebas no funcionales las cuales **determinan** si la **información** y **datos** del sistema **están protegidos**. El objetivo es **encontrar** de forma deliberada **lagunas** y **riesgos** de seguridad en el sistema que podrían resultar en el **acceso no autorizado** o la **pérdida de información** mediante el sondeo de la aplicación en busca de debilidades. 

Existen varios tipos de pruebas de seguridad, cada uno de los cuales **tiene como objetivo verificar seis principios básicos de seguridad**:

1. Integridad

2. Confidencialidad

3. Autentificación

4. Autorización

5. Disponibilidad

6. No repudio

## Pruebas de Usabilidad

Estas pruebas miden la **facilidad de uso** de una aplicación desde la perspectiva del **usuario final** y, a menudo, se **realizan durante** las etapas de **prueba del sistema** o de **aceptación**.

El objetivo es **determinar** si el **diseño visible** y la **estética** de una aplicación **cumplen** o **no** con el flujo de trabajo previsto para varios procesos, como iniciar sesión en una aplicación. Las pruebas de usabilidad son **una forma excelente** para que los equipos **revisen funciones separadas** o el **sistema en su conjunto**, para ver si es intuitivo de usar.

## Pruebas de Compatibilidad

Las pruebas de compatibilidad se utilizan para **evaluar** cómo **funcionará** una **aplicación** o **bloque del software** en diferentes entornos. Se utiliza para **verificar** que el producto sea **compatible** con múltiples **sistemas operativos**, **plataformas**, **navegadores** o **configuraciones de resolución**.

El objetivo es garantizar que la funcionalidad del software sea **compatible** de **manera constante** en cualquier entorno que espera que sea **utilizado** por los **usuarios finales**.

# Herramientas de pruebas

[Owasp Zap](https://www.zaproxy.org/)

[Burp suite](https://portswigger.net/burp)

[Postman](https://www.postman.com/)

[Selenium](https://www.selenium.dev/)

[JMeter](https://jmeter.apache.org/)

[TestComplete](https://smartbear.com/product/testcomplete/overview/)

[TestLink](https://testlink.org/)