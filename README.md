# Derivador de gramáticas — el cursor y las teclas

Herramienta web **estática** (un solo HTML, sin servidor, sin build, sin instalar nada) para
**derivar palabras de forma interactiva** con gramáticas **regulares (tipo 3)** e
**independientes de contexto (tipo 2)**: cada no terminal es un **cursor** que parpadea en la
cadena, y cada producción es una **tecla**. Apretás teclas, la cadena crece, y cuando no quedan
cursores… esa es tu palabra.

Antes de escribir se **declara el tipo**: en modo **tipo 3** la herramienta hace cumplir la forma
regular (lineal a derecha o a izquierda) — la producción que se escapa se marca mientras la
escribís y no se puede cargar; en modo
**tipo 2** entra cualquier forma. Ese límite es el tema de la unidad, y acá se choca contra él.

Se escribe en las **dos notaciones de la cátedra** — flechas (`S -> aS | λ`) y BNF
(`<Numero> ::= <Signo><Digito>`) — y muestra siempre la misma gramática escrita en la otra.

> 📐 **Qué clasifica.** Esta herramienta clasifica la **gramática** por la forma de sus
> producciones. **No dice nada sobre el lenguaje que genera**: decidir si el lenguaje de una
> gramática libre de contexto es regular no se puede resolver por algoritmo, y media verdad en
> material didáctico es peor que nada.

Material didáctico de la práctica de **Autómatas y Gramáticas** (UNLaM · DIIT), unidades 2A y 3A,
pensado para proyectar en clase y para que los estudiantes experimenten desde cualquier navegador
(celular incluido).

> ⚠️ **Versión 0.4 — beta.** Herramienta **solo con fines demostrativos**, construida alrededor
> de algunos ejemplos de la práctica. **Puede contener errores.** No reemplaza la teoría de la
> cátedra, la bibliografía ni a JFLAP — es un juguete didáctico para construir intuición sobre
> cómo una gramática *genera* palabras.

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

- **El tipo se declara antes de escribir**, con dos solapas: `Tipo 3 · regular` y
  `Tipo 2 · libre de contexto`. Arranca en tipo 3, que es el criterio de la cátedra: se resuelve
  como tipo 3 si se puede, y se sube en la jerarquía sólo cuando no alcanza.
- **En tipo 3 el límite se siente al escribir** — la producción que rompe la forma regular se
  marca en rojo con el motivo al lado (`S → aSb`: *tiene terminales a los dos lados del no
  terminal: no es lineal a derecha ni a izquierda*), y
  la que cambia de dirección se marca en ámbar (*ésta crece a la izquierda y arriba elegiste
  crecer a la derecha*). Al intentar cargarla, la herramienta la rechaza, recuerda las formas
  permitidas (lineal a derecha `A → aB | A → a`, lineal a izquierda `A → Ba | A → a`, más
  `S → λ`) y ofrece un botón: **subir en la jerarquía,
  cargarla como tipo 2**. La gramática que ya estaba cargada no se pierde.
- **Cartel de clasificación** — al cargar dice si la gramática es lineal a derecha, lineal a
  izquierda, si mezcla las dos direcciones, o si no tiene forma regular, y por qué, nombrando la
  producción responsable. Nunca dice "lineal" a secas: en la bibliografía *gramática lineal* es
  otra cosa (a lo sumo un no terminal por producción — `S → aSb` sí es lineal, y no es regular).
- **Nota de la práctica** 📌 — algunos presets traen una nota *curada* (no calculada): por
  ejemplo, que `<Numero>` de la slide 17 no tiene forma tipo 3 pero su lenguaje sí es regular, con
  la ER de la slide 18. Es lo único que habla del lenguaje, y sólo porque lo dice el material de
  la cátedra.
- **Jerarquía de Chomsky a la vista** — la caja de tipo 3 dentro de la de tipo 2, con un punto
  en la que corresponde. Sólo esos dos niveles: los tipos 1 y 0 no se deducen de la forma.
- **Dos notaciones** — se escribe con flechas (`S -> aS | Sb | λ`, mayúsculas = no terminales)
  o en **BNF** (`<Numero> ::= <Signo><Digito>`, no terminales entre `< >`, `::=` para asignar).
  Debajo del editor, un panel muestra **la misma gramática en la otra notación** — que es
  exactamente el ejercicio de la slide "Formas de escribir una gramática".
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
- **Modo desafío** — propone una palabra del lenguaje (obtenida por una derivación interna, de
  2 a 8 símbolos) para que la generes con las teclas; al lograrlo avisa, y **"Ver una derivación"**
  muestra una solución posible paso a paso.
- **Modo práctica** — cuestionario teórico autocorregido **desde la gramática cargada**: cadena
  mínima, pertenencia de `λ`, **recursividad**, finitud, **forma de la gramática** (lineal a
  derecha / a izquierda / mezcla / ninguna forma regular), Forma Normal de Chomsky, símbolo
  distinguido y cardinalidades. Todas las preguntas se muestran juntas
  y se corrigen al final.
- **Liana** — en una gramática regular (lineal a derecha o a izquierda) el árbol degenera en una
  "liana": un solo no terminal por nivel, siempre en el mismo extremo. La espina se dibuja
  resaltada y un cartel explica hacia dónde crece. Es la huella visual de una gramática regular.
- **Presets, agrupados por unidad**:
  - *Unidad 2A*: **G1** lineal a derecha y **G2** lineal a izquierda (las dos generan `(aa)*`,
    o sea cantidad par de `a`), **aⁿbᵐ con índices independientes** (lineal a derecha),
    **Número** en BNF (slide 17, 15 producciones — una GIC que genera un lenguaje regular),
    **Número lineal a derecha** (el mismo lenguaje, con forma tipo 3) y **EXPRESION** en BNF.
  - *Unidad 3A (tipo 2)*: la gramática de clase `S → aS | Sb | λ` (`a*b*`, mezcla las dos
    direcciones), los ejercicios 1, 2a y 2b de la Práctica 3A
    (`aⁱbʲcʲdⁱ`, `aⁱcʲdᵏbⁱ`, `cⁱaᵏbᵏdʲ`), la gramática del ejercicio de CYK (Kozen) y `aⁿbⁿ`.
- **Editor de gramática** — dos columnas con la flecha dibujada, **una fila por producción**
  (no terminal · → · lado derecho), con botones para agregar/quitar producciones e insertar `λ`.
  Un toggle **"editar como texto"** abre el modo crudo para pegar una gramática entera:

  ```
  S -> aS | Sb | λ
  A -> b A c | lambda
  ```

  Acepta `->`, `→` o `::=`, espacios dentro de las alternativas, y `λ` / `lambda` / `landa`.
  Un enlace **"⇄ escribir en BNF"** cambia la notación del editor. La cadena vacía es **λ**
  (notación de la cátedra — nunca ε). Tanto los presets como la edición se activan con
  **"Cargar gramática"** (el botón se resalta cuando hay cambios sin cargar).

## Cómo correrlo

**Online:** [sfweber.github.io/derivador-gic](https://sfweber.github.io/derivador-gic/) — corre en cualquier navegador, celular incluido. Nada que instalar.

**Local (offline):** abrí `index.html` con doble clic. No usa internet para nada.

## El par que muestra el límite

Tres gramáticas sobre `{a, b}`, todas precargadas, que separan *el lenguaje* de *la categoría de
la gramática*:

| Gramática | Categoría | En modo tipo 3 |
|---|---|---|
| `S → aS \| bB \| b \| λ` ; `B → bB \| b` | lineal a derecha | entra |
| `S → aS \| Sb \| λ` | mezcla lineal a derecha con lineal a izquierda | **la rechaza** |
| `S → aSb \| λ` | terminales a los dos lados del no terminal | **la rechaza** |

Las dos primeras generan **el mismo lenguaje**, `{aⁿbᵐ}`: los índices son independientes y cada
bloque crece por su cuenta. Mismo lenguaje, distinta categoría de gramática. La tercera es otra
cosa: ahí las `a` y las `b` **crecen juntas** en la misma producción, y los índices quedan
atados.

## Limitaciones conocidas

- **No terminales**: una mayúscula (`A`, `S`) o un nombre entre `< >` (`<Numero>`, `<PARTE>`).
  **Terminales**: siempre **un solo carácter** — minúscula, dígito, o `+ - * / ( )`.
  Sin escapes ni otros símbolos.
- Se pueden **mezclar las dos notaciones entre renglones** (la gramática se muestra en la del
  primero), pero **no dentro de un mismo renglón**: `S ::= a` se rechaza con un mensaje que pide
  `<S> ::= a`.
- La clasificación mira la **forma de las producciones**, y el cartel calculado no afirma nada
  sobre el lenguaje. Una gramática puede no tener forma regular y generar igual un lenguaje
  regular (le pasa a `<Numero>`, que está en la unidad de lenguajes regulares): "gramática
  regular" y "gramática *de un lenguaje regular*" no son lo mismo. Para los presets de la cátedra
  eso lo dice la nota 📌; para una gramática cualquiera, lo explica el docente. Tampoco decide si
  dos gramáticas son equivalentes.
- La forma regular es la **estricta** de la cátedra (un solo terminal por producción: `A → aB`,
  `A → a`; y `λ` **sólo desde el símbolo distinguido**, `S → λ`). Otros libros (Hopcroft) admiten
  `A → abB`, `A → B` y `A → λ` en cualquier no terminal; acá no: en modo tipo 3 una producción
  `B → λ` se marca y se rechaza. Las dos convenciones generan la misma clase de lenguajes; la
  diferencia es solo qué forma se acepta como "regular".
- El marcado en vivo sólo corre en el **editor estructurado**; en modo texto el veredicto llega
  al cargar.
- El símbolo distinguido **no se cuenta como no terminal**: `S ∉ N ∪ T`, como lo define la
  cátedra (slide 15 de U2A; en la fuente que cita, `G1 = ({A, B}, {a}, P1, S1)`). Si `S` aparece
  también del lado derecho, se lo sigue contando aparte y el cuestionario lo aclara.
- La derivación es manual a propósito: no hay parser automático ni chequeo de pertenencia —
  para eso está JFLAP (CYK / Brute Force Parse).
- JFLAP también tiene derivación manual: su **User Control Parse** deja elegir producción y
  variable, con árbol en vivo y deshacer. La diferencia es que JFLAP exige una **cadena
  objetivo** antes de empezar (parsing dirigido a meta); acá se genera **libre**, sin objetivo —
  la gramática como *generador* — y el modo desafío es opcional.
- El modo desafío no decide pertenencia de una palabra cualquiera: la palabra a derivar la
  **genera la propia herramienta** (por eso siempre pertenece al lenguaje y es derivable).
- El modo práctica responde **solo** lo que se calcula de la gramática cargada (cadena mínima,
  λ, recursividad, finitud, forma de la gramática, FNC, símbolo distinguido, cardinalidades); no
  incluye preguntas indecidibles como la ambigüedad.
- Sin persistencia: al recargar la página se pierde la sesión.

## Estructura

```
index.html        la herramienta completa (HTML + CSS + JS, autocontenida)
```

## Licencia

MIT — ver [LICENSE](LICENSE).

## Autor

**Federico Weber** — JTP de Autómatas y Gramáticas (UNLaM · DIIT). Material educativo.
