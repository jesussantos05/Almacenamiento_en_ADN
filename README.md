# Proyecto final: Almacenamiento de información en ADN

**Teoría de Códigos (MAT 4215), Corte 4 · Tema 10 · Integrantes:** Deiber Hernandez Nuñez, Karoll Marquez Baños, Emily Prada Beltrán, Jesús Santos Bastidas.

## Qué hace
Convierte un texto en una hebra de ADN (A, C, G, T; alfabeto q = 4), le agrega redundancia con un código de Hamming sobre GF(4), simula errores de sustitución y recupera el texto. Compara contra guardar el texto sin código.

- **Código:** Hamming (5, 3)₄, parámetros (n, M, d) = (5, 64, 3) sobre un alfabeto de q = 4 símbolos, que se escribe (5, 64, 3)₄. Corrige t = 1 sustitución por bloque de 5 bases.
- **Perfecto:** |B₁| = 1 + 5·3 = 16 y M·|B₁| = 64·16 = 1024 = 4⁵.
- **Rata:** R = 3/5, es decir 1,2 bits por base (sin código, 2).
- El núcleo (aritmética de GF(4), codificador, decodificador por síndrome y scrambling) es propio, está escrito en JavaScript y no usa librerías externas.

## Contenido de la carpeta
| Archivo | Para qué sirve |
|---|---|
| `Hamming_en_ADN.html` | Aplicativo web. Un solo archivo, funciona sin internet. Incluye el código fuente |
| `Presentacion_Almacenamiento_ADN.pdf` | Diapositivas de la ponencia (8) |

## Cómo ejecutar el aplicativo
No requiere instalar nada ni tener internet.
1. Descomprimir la carpeta.
2. Hacer doble clic en `Hamming_en_ADN.html` (abre en Chrome, Edge o Firefox).

Si el navegador no lo abre con doble clic, arrastrar el archivo a una ventana del navegador.

## Qué hay en la página
- **El código:** parámetros (n, M, d)₄ verificados en vivo, la igualdad de Hamming y la matriz H.
- **Demostración:** escribir un mensaje, mover la probabilidad de sustitución p y ver la hebra enviada y la recibida, los bloques corregidos y el texto recuperado con y sin código. La misma semilla repite el mismo daño y "Otro azar" lo cambia.
- **Experimento:** simulación de 400 mensajes de 100 bytes por cada p. Corre sola al abrir y "Repetir simulación" la vuelve a correr; sus números cambian unos puntos de una corrida a otra.
- **Química y deleciones:** rachas de una misma base y balance G + C con y sin scrambling, y el efecto de borrar una base.

Las 4 partes del aplicativo:

| Parte | Dónde está |
|---|---|
| Codificador | `encode`: texto → bytes → bases → scrambling → bloques (5, 3)₄ |
| Canal o daño | `mutate` (sustituciones con probabilidad p) y el deslizador de deleción |
| Decodificador y verificador | `decode`, `syndrome`, `decBlock` y `verify` |
| Métrica | error por base, porcentaje recuperado exacto, teoría y capacidad de Shannon |

El código fuente está en el `<script>` del HTML, entre los comentarios `CORE-START` y `CORE-END`. Se puede abrir con el Bloc de notas o VS Code.

## Resultados principales
Mensaje de 100 bytes, porcentaje recuperado exacto (400 ensayos por punto; una corrida del experimento):

| p (sustitución por base) | sin código | con Hamming (5, 3)₄ |
|---|---|---|
| 0,005 | 13,2 % | 95,8 % |
| 0,01 | 2,0 % | 89,2 % |
| 0,02 | 0,0 % | 53,5 % |
| 0,05 | 0,0 % | 6,5 % |

Con p = 0,01 el error por base baja de 0,011 a 0,0006. Un bloque falla con 2 o más errores: P = 1 − (1−p)⁵ − 5p(1−p)⁴. **Límites:** con p = 0,15 la capacidad del canal (1,15 bits por base) es menor que la rata del código (1,2) y ningún código sirve (Shannon). El código solo corrige sustituciones: una deleción desalinea toda la hebra.

## Declaración de herramientas
Para el desarrollo y análisis de este proyecto se emplearon las siguientes herramientas de apoyo:

**Entorno y lenguaje:** HTML, CSS y JavaScript, sin librerías externas. El aplicativo corre en el navegador desde un solo archivo, Hamming_en_ADN.html, que contiene el codificador, el canal de ADN, el decodificador y las métricas.

**Visualización de datos:** Las gráficas de error y de recuperación están dibujadas en SVG dentro de la propia página.

**Asistencia de Inteligencia Artificial:** Claude (Anthropic) como herramienta de apoyo conceptual para el diseño inicial de la arquitectura del código y la estructura de la simulación.

## Fuentes
1. Church, G. M., Gao, Y., & Kosuri, S. (2012). Next-generation digital information storage in DNA. *Science*, 337(6102), 1628.
2. Goldman, N. et al. (2013). Towards practical, high-capacity, low-maintenance information storage in synthesized DNA. *Nature*, 494(7435), 77–80.
3. Grass, R. N. et al. (2015). Robust chemical preservation of digital information on DNA in silica with error-correcting codes. *Angewandte Chemie*, 54(8), 2552–2555.
4. Erlich, Y., & Zielinski, D. (2017). DNA Fountain enables a robust and efficient storage architecture. *Science*, 355(6328), 950–954.
