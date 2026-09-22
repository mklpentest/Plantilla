# Manual de uso — Plantilla LaTeX estilo IOC/EAC

Plantilla para documentos de entrega (EAC) con el estilo visual del IOC:
cabecera institucional, caja de título gris, barras de sección grises y
pie de página con tabla de metadatos.

Archivo base: `ioc-eac-template.tex`
Requiere: `pdflatex` (TeX Live estándar) — sin paquetes externos raros.

## Compilación

Compilar **dos veces** siempre (necesario para que `Pàgina X de Y` y,
si activas el índice, los números de página sean correctos):

```bash
pdflatex ioc-eac-template.tex
pdflatex ioc-eac-template.tex
```

## 1. Datos del documento

Al principio del `.tex`, bloque `DATOS DEL DOCUMENTO`. Cambia estos
valores en cada nuevo EAC:

| Comando | Qué es |
|---|---|
| `\nomAlumne` | Nombre y apellidos (aparece en cabecera y portada) |
| `\ciclePrograma` | Nombre del ciclo/programa |
| `\modulNom` | Nombre del módulo |
| `\codiActivitat` | Código de la actividad (p. ej. `EAC1`) |
| `\cursSemestre` | Curso y semestre |
| `\subtitolActivitat` | Subtítulo de la actividad |
| `\codiDocument` | Código de documento (pie de página) |
| `\nomExercici` | Nombre del ejercicio (pie de página) |
| `\versioDocument` | Versión del documento |
| `\fitxerDocument` | Nombre de fichero de entrega |
| `\dataLliurament` | Fecha límite de entrega |

## 2. Interruptores opcionales

Justo debajo de los paquetes, antes de los datos del documento:

```latex
\newif\ifiocfooter  \iocfootertrue   % pie de página
\newif\ifiocindex   \iocindexfalse   % índice
\newif\ifiocbib     \iocbibfalse     % bibliografía
```

Pon `...false` o `...true` según quieras:

- **`\iocfootertrue` / `\iocfooterfalse`** — muestra u oculta la tabla
  de metadatos del pie en todas las páginas.
- **`\iocindextrue`** — inserta `\tableofcontents` (como "Índex") justo
  después de la portada, en página aparte. Las barras de sección
  (`\seccioEAC`) se añaden automáticamente al índice.
- **`\iocbibtrue`** — activa el bloque `thebibliography` al final del
  documento (sección "Bibliografia"). No usa BibTeX: añade las entradas
  a mano.

## 3. Comandos de contenido

### Portada

```latex
\portadaEAC
```

Se llama **una sola vez**, justo tras `\begin{document}`. Dibuja la
cabecera institucional, el nombre del alumno y la caja gris de título.
No la repitas ni la llames en otra página.

### Barra de sección

```latex
\seccioEAC{1. Introducció}
```

Dibuja la barra gris de título de sección. Úsala tantas veces como
apartados tengas. El texto que le pases es literal (no se numera
solo), así que escribe el número si lo quieres: `\seccioEAC{2. Activitat a realitzar}`.

### Contenido normal

Debajo de cada `\seccioEAC{...}` escribe el contenido con comandos
LaTeX normales: párrafos, `enumerate`/`itemize` (el paquete
`enumitem` ya está cargado), `\textbf`, `\textit`, tablas, imágenes
(`\includegraphics`, con `graphicx` ya cargado), etc.

### Bibliografía (si `\iocbibtrue`)

Al final del documento, edita las entradas de ejemplo:

```latex
\begin{thebibliography}{99}
  \bibitem{clave} Autor. \textit{Título}. Editorial/URL, año.
\end{thebibliography}
```

Cita en el texto con `\cite{clave}`.

## 4. Estructura mínima de un documento nuevo

```latex
\begin{document}

\portadaEAC

\ifiocindex
  \renewcommand{\contentsname}{Índex}
  \tableofcontents
  \newpage
\fi

\seccioEAC{Enunciat}
Texto...

\seccioEAC{1. Introducció}
Texto...

\seccioEAC{2. Activitat a realitzar}
Texto...

\ifiocbib
  \renewcommand{\refname}{Bibliografia}
  \ifiocindex\addcontentsline{toc}{section}{Bibliografia}\fi
  \begin{thebibliography}{99}
    \bibitem{ref1} ...
  \end{thebibliography}
\fi

\end{document}
```

(Este bloque ya viene hecho en la plantilla; solo tienes que rellenar
el contenido entre las llamadas a `\seccioEAC`.)

## 5. Notas y límites

- El pie de página usa `lastpage`, así que `Pàgina X de Y` solo es
  correcto tras la **segunda** compilación.
- Los colores del estilo (`iocgray`, `iocbarragris`, `iocblau`) se
  definen cerca del principio del `.tex`; cambia el valor si el color
  institucional real difiere.
- La plantilla no usa `babel` (el paquete de idioma catalán no está
  instalado en todos los TeX Live); los acentos catalanes van
  directos en UTF-8 y compilan bien igualmente. Si tu sistema sí tiene
  `babel` con catalán, puedes añadir `\usepackage[catalan]{babel}` sin
  romper nada.
- `\seccioEAC` no es un `\section` real: no numera automáticamente ni
  genera marcadores de PDF (bookmarks). Si necesitas eso, sustitúyelo
  por `\section{...}` y adapta el estilo de la barra gris como
  `\titleformat` (paquete `titlesec`), o pide que se amplíe la
  plantilla.
