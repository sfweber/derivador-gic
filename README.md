# Derivador GIC — el cursor y las teclas

Herramienta web **estática** (un solo HTML, sin servidor, sin build, sin instalar nada) para
**derivar palabras con Gramáticas Independientes de Contexto de forma interactiva**: cada no
terminal es un **cursor** que parpadea en la cadena, y cada producción es una **tecla**. Apretás
teclas, la cadena crece, y cuando no quedan cursores… esa es tu palabra.

Material didáctico de la práctica de **Autómatas y Gramáticas** (UNLaM · DIIT), pensado para
proyectar en clase y para que los estudiantes experimenten desde cualquier navegador (celular
incluido).

> ⚠️ **Versión 0.2 — beta.** Herramienta **solo con fines demostrativos**, construida alrededor
> de algunos ejemplos de la práctica (Unidad 3A). **Puede contener errores.** No reemplaza la
> teoría de la cátedra, la bibliografía ni a JFLAP — es un juguete didáctico para construir
> intuición sobre cómo una gramática *genera* palabras.

## Demo

https://sfweber.github.io/derivador-gic/

## La idea

Una regla como `S → aS` se entiende mejor jugando que leyendo:

- la **`S` es un cursor** — el "agujero" pendiente en el medio de la cadena;
- **`S → aS`** escribe una `a` a la **izquierda** del cursor;
- **`S → Sb`** escribe una `b` a la **derecha**;
- **`S → λ`** borra el cursor: la palabra quedó terminada.

Lo que ya escribiste no se mueve más. De ahí salen "gratis" las intuiciones difíciles:
por qué `X → aXb` ata los exponentes (`aⁿ…bⁿ`), por qué `ba ∉ a*b*`, por qué λ es una
palabra válida.

## Qué hace

- **Cinta de derivación** — la forma sentencial actual; los no terminales son cursores que
  parpadean. Si hay más de uno (p. ej. con `S → SS`), tocás cuál querés expandir.
- **Teclado de producciones** — un botón por regla; solo se habilitan las que aplican al
  cursor seleccionado.
- **Historia** — la derivación completa paso a paso (`S ⇒ aS ⇒ ab …`) con la regla usada en
  cada paso. Con **deshacer** y **reiniciar**.
- **Árbol de derivación** — solapa junto a la historia: el árbol se dibuja en vivo a medida que
  apretás teclas; las hojas no terminales son los cursores (clickeables). De paso se ve un
  concepto central: derivaciones con distinto orden pueden dar el **mismo árbol** — el árbol
  abstrae el orden (la puerta a la idea de *ambigüedad*).
- **Palabras generadas** — colección de las palabras que completaste con la gramática actual.
- **Modo desafío** — escribís una palabra objetivo ("generá `aabcdd`") y te dice si llegaste.
- **Presets** — las gramáticas de la práctica: `a*b*`, los ejercicios 1, 2a y 2b de la
  Práctica 3A (`aⁱbʲcʲdⁱ`, `aⁱcʲdᵏbⁱ`, `cⁱaᵏbᵏdʲ`), la gramática del ejercicio de CYK
  (Kozen) y `aⁿbⁿ`.
- **Editor libre** — escribí tu propia gramática:

  ```
  S -> aS | Sb | λ
  A -> b A c | lambda
  ```

  Acepta `->` o `→`, espacios dentro de las alternativas, y `λ` / `lambda` / `landa`.
  No terminales = mayúsculas (un carácter); terminales = minúsculas. La cadena vacía es **λ**
  (notación de la cátedra — nunca ε).

## Cómo correrlo

**Local (offline):** abrí `index.html` con doble clic. No usa internet para nada.

**GitHub Pages (para compartir con estudiantes):** Settings → Pages → Branch `main` / root.

## Limitaciones conocidas

- Solo gramáticas con **no terminales de un carácter en mayúscula** y **terminales en
  minúscula** (la convención de la cátedra). Sin escapes ni símbolos especiales.
- La derivación es manual a propósito: no hay parser automático ni chequeo de pertenencia —
  para eso está JFLAP (CYK / Brute Force Parse).
- JFLAP también tiene derivación manual: su **User Control Parse** deja elegir producción y
  variable, con árbol en vivo y deshacer. La diferencia es que JFLAP exige una **cadena
  objetivo** antes de empezar (parsing dirigido a meta); acá se genera **libre**, sin objetivo —
  la gramática como *generador* — y el modo desafío es opcional.
- El modo desafío compara contra las palabras que **vos** generaste; no decide pertenencia
  por sí mismo.
- Sin persistencia: al recargar la página se pierde la sesión.

## Estructura

```
index.html        la herramienta completa (HTML + CSS + JS, autocontenida)
```

## Licencia

MIT — ver [LICENSE](LICENSE).

## Autor

**Federico Weber** — JTP de Autómatas y Gramáticas (UNLaM · DIIT). Material educativo.
