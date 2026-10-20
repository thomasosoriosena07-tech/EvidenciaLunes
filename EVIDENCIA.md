# Paquete para el agente del Diario de Estudio

**Aprendiz:** Thomas Osorio R
**Curso:** El nuevo programador · Módulo 01 · Taller guiado
**Fecha:** 20 de octubre de 2026

Este documento reúne las cuatro estaciones del taller:

1. El prompt inicial: describir la primera versión.
2. El AGENTS.md: las reglas permanentes del proyecto.
3. La petición de plan: una funcionalidad nueva.
4. La revisión: detectar los errores de un plan.

---

## Estación 1 · El prompt inicial

**Ejercicio:** escribir el prompt de la primera versión con sus cinco partes (rol, contexto, tarea, restricciones y formato de salida). La web debe registrar sesiones (fecha, tema y minutos), mostrar la racha actual y la lista de sesiones, y guardar los datos en el navegador.

### Mi prompt

```markdown
# Prompt · Diario de Estudio (primera versión)

## 1. Rol
Actúa como un desarrollador web front-end con experiencia que escribe código claro
para principiantes. El código lo leerá y mantendrá un estudiante que está empezando
a programar, así que prioriza la legibilidad sobre los trucos.

## 2. Contexto
Estoy creando el Diario de Estudio, una web personal para anotar mis sesiones de estudio.
El objetivo es motivarme viendo mi racha: cuántos días seguidos llevo estudiando.
Solo la usaré yo, en mi navegador, sin servidor ni cuentas de usuario.
Es la primera versión y el proyecto está vacío.

## 3. Tarea
Crea la primera versión con estas funcionalidades:

1. Registrar una sesión con un formulario de tres campos:
   - Fecha: obligatoria; por defecto, la fecha de hoy; no se admiten fechas futuras.
   - Tema: texto obligatorio (sin contar espacios al inicio y al final); máximo 80 caracteres.
   - Minutos: número entero mayor que 0 (máximo 1440, los minutos de un día).
   - Si un dato no es válido, muestra un mensaje junto al campo y no guardes la sesión.
2. Mostrar la racha actual en un lugar destacado, con el texto «X días seguidos».
3. Mostrar la lista de sesiones, de la más reciente a la más antigua, con fecha, tema
   y minutos. Si no hay sesiones, muestra «Aún no hay sesiones registradas».
4. Guardar los datos en el navegador con localStorage, para que sigan ahí al cerrar
   y volver a abrir la página.

Reglas de la racha:
- La racha es el número de días consecutivos con al menos una sesión.
- Varias sesiones en el mismo día cuentan como un solo día.
- Si hoy hay sesión, la racha se cuenta desde hoy hacia atrás.
- Si hoy todavía no hay sesión pero ayer sí, la racha se cuenta desde ayer
  (no se rompe hasta que termina el día).
- Si no hubo sesión ni hoy ni ayer, la racha es 0.

Reglas de fechas (obligatorias):
- Usa siempre la fecha local del usuario, guardada como texto «AAAA-MM-DD».
- No uses `toISOString()` para obtener la fecha: devuelve la hora UTC y puede cambiar
  el día (en Colombia, UTC−5, a partir de las 7:00 p. m. ya marca el día siguiente).
- No uses `new Date("AAAA-MM-DD")`: lo interpreta como UTC y puede dar el día anterior.
  Construye las fechas con `new Date(año, mes - 1, día)`.

## 4. Restricciones
- Solo HTML, CSS y JavaScript puro: sin frameworks, sin librerías y sin CDN.
- Exactamente tres archivos: index.html, styles.css y app.js.
- Debe funcionar abriendo index.html con doble clic, sin instalar nada ni usar un servidor.
- No añadas funcionalidades que no estén en esta lista (editar, borrar, gráficos, exportar…).
- Si los datos guardados en localStorage están dañados, la página no debe romperse:
  empieza con la lista vacía y muestra un aviso.
- Textos de la interfaz en español; nombres de variables y funciones en inglés.

## 5. Formato de salida
1. El contenido completo de los tres archivos, cada uno en su propio bloque de código
   con el nombre del archivo como título.
2. Un resumen de 5 líneas como máximo sobre qué hace cada archivo.
3. Una lista con las decisiones que hayas tomado por tu cuenta (lo que no estaba en este prompt).
4. Los pasos para probar a mano la racha y las fechas.
```

### Comparación con la solución

- ✔ Define el rol y para quién es el código (un estudiante principiante).
- ✔ Explica el objetivo: motivarse viendo la racha.
- ✔ Enumera las funcionalidades con sus campos y reglas (minutos entero mayor que 0, tema obligatorio, sin fechas futuras).
- ✔ Fija las reglas de la racha y el uso de la fecha local (prohíbe `toISOString()` y `new Date("AAAA-MM-DD")`).
- ✔ Limita la tecnología: sin frameworks, tres archivos, abrir con doble clic.
- ✔ Pide un resumen y las decisiones que el agente tome por su cuenta.

---

## Estación 2 · El AGENTS.md

**Ejercicio:** escribir un AGENTS.md de 20 a 30 líneas con qué es el proyecto, stack y estructura, convenciones, datos, fechas y racha, límites y verificación.

### Mi AGENTS.md

```markdown
# AGENTS.md · Diario de Estudio

## Proyecto
Web para anotar sesiones de estudio (fecha, tema y minutos) y ver la racha de días seguidos.
La usa un estudiante para motivarse; el código debe poder entenderlo un principiante.

## Stack y estructura
- HTML, CSS y JavaScript puro, sin frameworks ni librerías. Se abre con doble clic en index.html.
- Solo tres archivos: index.html (estructura), styles.css (estilos) y app.js (lógica).

## Convenciones
- Variables y funciones en inglés; textos de la interfaz en español.
- Funciones pequeñas con una sola tarea; `const` por defecto, `let` si cambia, nunca `var`.
- Comentarios solo donde la lógica no sea obvia.

## Datos
- Se guardan en localStorage, clave `diario-estudio:sesiones`, como array JSON.
- Cada sesión: `{ id, date: "AAAA-MM-DD", topic, minutes }`; minutes es un entero mayor que 0.
- Si el JSON guardado está dañado, empezar con la lista vacía y avisar; la página nunca se rompe.

## Fechas y racha
- Usar siempre la fecha local del usuario, como texto "AAAA-MM-DD".
- Prohibido `toISOString()` para obtener la fecha: usa UTC y puede cambiar el día.
- Prohibido `new Date("AAAA-MM-DD")`: lo lee como UTC. Usar `new Date(año, mes - 1, día)`.
- Racha: días consecutivos con al menos una sesión, contando desde hoy o, si hoy aún no hay, desde ayer.
- Varias sesiones el mismo día cuentan una vez. No se aceptan fechas futuras.

## Límites
- Siempre: validar los datos del formulario y anotar en MEMORY.md las decisiones tomadas.
- Pregunta antes: añadir una librería o dependencia, crear un archivo o cambiar el formato de los datos.
- Nunca: borrar los datos guardados del usuario, enviar datos fuera del navegador ni dar una tarea por terminada sin verificarla.

## Verificación
- Abrir index.html con doble clic: sin errores en la consola del navegador.
- Racha: 0 sin sesiones; 3 días seguidos = 3; un día sin sesión la reinicia; 2 sesiones el mismo día = 1.
- Una sesión registrada a las 23:30 se guarda con la fecha de hoy, no con la de mañana.
```

### Comparación con la solución

- ✔ Tiene 29 líneas con contenido (36 contando las líneas en blanco): dentro del rango de 20 a 40.
- ✔ Incluye la sección «Fechas y racha», con la prohibición de `toISOString()` y de `new Date("AAAA-MM-DD")`.
- ✔ Los límites distinguen entre «siempre», «pregunta antes» y «nunca».
- ✔ Tiene una sección de verificación con pruebas concretas de la racha y de la fecha local.
- ✔ No incluye credenciales ni listas de archivos obvios: solo lo que el agente no puede deducir leyendo el código.

---

## Estación 3 · La petición de plan

**Ejercicio:** pedir en modo Plan un resumen con los minutos estudiados por semana de las últimas cuatro semanas, indicando qué debe incluir el plan antes de escribir código.

### Mi petición (modo Plan)

```markdown
Modo Plan. Lee AGENTS.md y MEMORY.md antes de responder.

Quiero añadir un resumen «Minutos por semana» que muestre el total de minutos estudiados
en cada una de las últimas 4 semanas (la actual y las 3 anteriores), debajo de la racha.

No escribas código todavía. Primero prepárame un plan que incluya:

1. Cálculo: cómo vas a obtener el inicio de cada semana y agrupar las sesiones usando
   la fecha local, sin `toISOString()` ni `new Date("AAAA-MM-DD")`, como dice AGENTS.md.
   Incluye un ejemplo con fechas concretas.
2. Archivos: qué archivos vas a modificar y qué cambia en cada uno; qué funciones nuevas
   vas a crear y qué reciben y devuelven.
3. Casos límite y cómo los tratas:
   - semanas sin sesiones (deben aparecer con 0 minutos);
   - la semana actual, que todavía está incompleta;
   - sesiones de hace más de 4 semanas (no deben sumarse);
   - fechas futuras;
   - semanas que cruzan un cambio de mes o de año;
   - datos guardados vacíos o dañados.
4. Presentación: las 4 semanas en orden cronológico (de la más antigua a la más reciente)
   y cómo se verán sin usar librerías de gráficos.
5. Dudas que debo decidir yo antes de empezar, por ejemplo: si la semana empieza en lunes
   o en domingo, y cómo se etiqueta cada semana.
6. Verificación: qué pruebas manuales harás para comprobar que funciona.
7. Qué actualizarías en AGENTS.md y en MEMORY.md.

Espera mi aprobación antes de escribir código.
```

### Comparación con la solución

- ✔ Dice qué quiero y dónde se muestra (debajo de la racha).
- ✔ Pide el cálculo por semana con la fecha local, según AGENTS.md.
- ✔ Pide los archivos que se modifican y qué cambia en cada uno.
- ✔ Pide los casos límite: semanas vacías, semana actual incompleta y fechas futuras (y algunos más).
- ✔ Pide las dudas que debo decidir yo (lunes o domingo).
- ✔ Pide qué se actualizaría en AGENTS.md y MEMORY.md.
- ✔ Deja claro que no se escribe código hasta aprobar el plan.

---

## Estación 4 · La revisión

**Ejercicio:** encontrar los problemas de este plan propuesto por el agente.

```
## Plan: minutos por semana

1. Para cada sesión, calculo la semana con new Date(sesion.date).toISOString().
2. Sumo los minutos de cada semana.
3. Instalo la librería Chart.js para mostrar un gráfico de barras.
4. Muestro las semanas en el orden en que aparecen en los datos.
```

### Problemas encontrados

**1. Usa fechas en UTC (paso 1).**
`new Date("AAAA-MM-DD")` interpreta la fecha como medianoche UTC. En Colombia (UTC−5), la sesión del lunes `"2026-10-19"` se convierte en el domingo 18 de octubre a las 7:00 p. m., así que el día y la semana salen mal: la sesión del lunes cae en la semana anterior. `toISOString()` también trabaja en UTC y además solo devuelve un texto con la fecha: no calcula la semana. Por qué es un problema: incumple la sección «Fechas y racha» del AGENTS.md, que prohíbe exactamente esas dos funciones.

**2. Añade una dependencia sin preguntar (paso 3).**
El proyecto es HTML, CSS y JavaScript sin librerías y se abre con doble clic. Chart.js obligaría a descargar un archivo nuevo o a cargarlo desde internet (si no hay conexión, el resumen no se ve). Por qué es un problema: el AGENTS.md pone «añadir una librería o dependencia» en «pregunta antes», y el agente la instala sin consultar. Además, yo pedí un resumen, no un gráfico: se puede mostrar con una lista o con barras hechas con HTML y CSS.

**3. No trata los casos límite (paso 2).**
Solo suma las semanas que aparecen en los datos. Por qué es un problema:
- Una semana sin sesiones no aparecería, cuando debería mostrarse con 0 minutos; a veces se verían menos de 4 semanas.
- No limita el resultado a las últimas 4 semanas: sumaría todas las semanas del historial.
- No dice qué pasa con la semana actual (incompleta) ni con las fechas futuras.

**4. El orden es incorrecto (paso 4).**
El orden de los datos es el orden en que se registraron las sesiones, no el de las fechas: si registro hoy una sesión de hace dos semanas, esa semana aparecería al final. Por qué es un problema: las semanas deben mostrarse en orden cronológico, siempre las mismas 4, de la más antigua a la más reciente.

**Además, le falta lo que pedí:** no dice qué archivos modifica ni qué cambia en cada uno, no plantea las dudas que debo decidir yo (si la semana empieza en lunes o en domingo), no explica cómo lo verificará y no menciona los cambios en AGENTS.md y MEMORY.md.

### Mi respuesta al agente

```markdown
No apruebo el plan. Corrígelo antes de escribir código:

1. Calcula las fechas en hora local, sin toISOString() ni new Date("AAAA-MM-DD")
   (sección «Fechas y racha» de AGENTS.md). Convierte la fecha con new Date(año, mes - 1, día).
2. No instales Chart.js: muestra el resumen con HTML y CSS. Si crees que una librería
   es necesaria, pregúntame primero y explica por qué.
3. Genera siempre las 4 últimas semanas, aunque tengan 0 minutos; no sumes sesiones
   más antiguas ni fechas futuras, y marca la semana actual como «en curso».
4. Ordena las semanas cronológicamente, de la más antigua a la más reciente.
5. Añade los archivos que modificas, las dudas que debo decidir (lunes o domingo),
   las pruebas que harás y los cambios en AGENTS.md y MEMORY.md.
```

---

## Conclusión

Con este paquete un agente tiene lo necesario para construir bien el Diario de Estudio:

- un **prompt claro** que describe la primera versión con sus cinco partes;
- un **AGENTS.md** con las reglas permanentes, sobre todo las de fechas y racha;
- una **petición de plan** que obliga a pensar el cálculo, los archivos, los casos límite y las dudas antes del código;
- una **revisión con criterio** que detecta cuándo un plan incumple esas reglas.
