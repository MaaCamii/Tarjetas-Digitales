# Instrucción base · Tarjetas virtuales y perfiles profesionales

**Referencia:** tarjeta de Mabel Vallejo, Abogada (prototipo v8, noviembre 2026)
**Uso:** este documento se le entrega a Claude al iniciar una nueva tarjeta. Define la **estructura y el diseño**. El **contenido** sale siempre de la hoja de vida, los enlaces y los datos que entregue cada persona.

---

## 1. Qué se construye

Una página web de una sola URL, pensada para celular, que cumple dos funciones:

1. **Tarjeta digital (pestaña Inicio):** identidad, contacto inmediato y botón para agendar.
2. **Hoja de vida navegable (pestañas siguientes):** formación, experiencia y ejercicio profesional, cada una en su propia pantalla.

Al compartir el enlace por WhatsApp aparece una **vista previa** (rectángulo) con el logo, el nombre y una frase de llamado a la acción.

Se entregan dos versiones del mismo diseño:

| Versión | Para qué sirve | Dónde vive |
|---|---|---|
| **Prototipo en Claude** (artifact) | Aprobación del cliente desde su celular | Enlace público de Claude |
| **Versión final** | Uso real y compartir por WhatsApp | Vercel (dominio propio opcional) |

---

## 2. Insumos que se piden al cliente

### Obligatorios
- [ ] Nombre completo, tal como debe aparecer
- [ ] Título o profesión (ej. "Abogada", "Psicóloga")
- [ ] Logo oficial: monograma y versión horizontal (PNG o SVG; si llega en JPG con fondo blanco, se convierte a transparente)
- [ ] WhatsApp: número o enlace (`wa.me/...` o `wa.me/qr/...`)
- [ ] Correo de contacto profesional
- [ ] Ciudad o municipio de atención
- [ ] Colores de marca (por defecto: negro, blanco y plata)

### Para la hoja de vida
- [ ] Formación: institución, título y, si existen, especializaciones y años
- [ ] Experiencia: cargos, entidades, lugares y fechas
- [ ] Ejercicio actual: rol, modalidad de atención (virtual, presencial o ambas)
- [ ] Áreas de práctica o servicios, y cuáles son su **experiencia destacada**
- [ ] Fotografía de perfil (rostro sobre fondo claro)

### Opcionales
- [ ] Enlace de agenda (Google Calendar, Calendly o Cal.com)
- [ ] Horario de atención
- [ ] Redes sociales
- [ ] Dominio propio

> **Regla de contenido:** no inventar datos. Lo que no esté en los insumos no se publica. Cuando se agregue una descripción o un ejemplo de servicio para mejorar la redacción, se marca para que la persona lo valide antes de publicar.

---

## 3. Arquitectura de la página

```
┌──────────────────────────────┐
│  Panel activo (ver sección 4) │
│                              │
│                              │
├──────────────────────────────┤
│ Inicio │ Formación │ Experiencia │ Ejercicio │  ← barra de pestañas fija abajo
└──────────────────────────────┘
```

- **Ancho máximo:** 440 px. En escritorio se muestra como una tarjeta enmarcada y centrada.
- **Navegación:** barra inferior tipo app, con 4 pestañas, ícono + texto, y una línea plateada sobre la pestaña activa.
- **Enlaces directos:** cada pestaña se puede abrir con `#formacion`, `#experiencia` o `#ejercicio` al final de la URL.
- **Flujo de lectura:** cada pestaña de contenido termina con un botón "Siguiente → [pestaña]" para recorrer la hoja de vida en orden.
- **Accesibilidad:** pestañas con `role="tab"`, navegación con flechas del teclado y foco visible.

Se pueden cambiar los nombres de las pestañas según la profesión (ej. "Ejercicio" → "Consulta" para psicología), pero se mantiene el máximo de **4 pestañas**.

---

## 4. Contenido de cada pestaña

### 4.1 Inicio — Tarjeta digital (fondo negro)

En este orden:

1. **Encabezado:** marco fino blanco translúcido, abierto abajo
   - Monograma en blanco (≈124 px)
   - Nombre en mayúsculas, con letra serif espaciada
   - Profesión en versalitas muy espaciadas (ej. `A B O G A D A`)
   - Línea corta plateada
   - **Sello de credencial:** una píldora con borde plateado y el dato más fuerte de su formación (ej. "Universidad del Cauca"). Va el nombre de la institución solo, sin "egresada de".
2. **Botón principal (blanco):** "Agendar cita virtual" con subtítulo, ícono de calendario y flecha. Abre el enlace de agenda.
3. **Cuadrícula de contacto 2×2** (fondo gris muy oscuro, borde fino):

   | Botón | Texto pequeño | Acción |
   |---|---|---|
   | WhatsApp | "Escribir por chat" | Abre el enlace de WhatsApp |
   | Teléfono | "Copiar – +57 XXX XXX XXXX" | Copia el número |
   | Correo | El correo completo (con `<wbr>` antes de la @) | Abre el correo o lo copia |
   | Guardar contacto | "Añadir a mi agenda" | Descarga el archivo .vcf |

4. **Enlace de texto:** "Conocer su formación y experiencia →" (lleva a la pestaña Formación).
5. *(Solo en Vercel)* **"Compartir esta tarjeta"**: usa el menú de compartir del celular o copia el enlace.

### 4.2 Formación (encabezado con foto)

- **Encabezado de perfil:** foto circular de 124 px con anillo plateado y borde negro, nombre debajo y la línea `PROFESIÓN · INSTITUCIÓN`.
- **Hoja clara** (papel, esquinas superiores redondeadas):
  - Etiqueta: `FORMACIÓN ACADÉMICA`
  - Título: nombre de la institución
  - Párrafo de 2 a 3 líneas sobre el título obtenido y la institución
  - Sello: círculo negro con ícono, título profesional y descripción
  - **Ficha de datos** (etiqueta arriba, valor abajo): Título · Institución · Carácter · Sede · Año
  - Botón "Siguiente → Experiencia"

### 4.3 Experiencia (encabezado con foto)

- Mismo encabezado de perfil con foto.
- Hoja clara:
  - Etiqueta: `EXPERIENCIA PÚBLICA` o `EXPERIENCIA PROFESIONAL`
  - Título que resuma el ámbito (ej. "Servicio público en Villagarzón, Putumayo")
  - Párrafo sobre lo que aporta esa experiencia
  - **Línea de tiempo vertical:** el cargo más reciente arriba. El actual va con punto negro relleno y los anteriores con punto vacío. Cada cargo lleva entidad, lugar y fechas si las hay.
  - Botón "Siguiente → Ejercicio profesional"

### 4.4 Ejercicio profesional (encabezado con logo)

- **Encabezado compacto:** monograma pequeño, nombre y profesión, con una línea divisoria. Aquí va el logo, no la foto.
- Hoja clara:
  - Etiqueta: `EJERCICIO PROFESIONAL`
  - Título del rol actual (ej. "Abogada litigante")
  - Párrafo de posicionamiento: de dónde viene y qué ofrece, incluyendo la modalidad
  - **"Experiencia en":** fichas negras con 2 a 4 especialidades destacadas
  - **"Áreas de práctica":** una tarjeta por área. Cada tarjeta lleva:
    - Nombre del área (serif, 19 px)
    - Etiqueta "EXPERIENCIA DESTACADA" si aplica (además, la tarjeta lleva borde negro)
    - Descripción de una línea con servicios concretos
    - Referencia normativa o técnica en gris (ver sección 6)
  - **"Atención":** ficha con Modalidad · Ubicación · Teléfono · Correo (y Horario solo si el cliente lo pide)
  - Botón negro "Agendar cita virtual"
  - Pie: © año · Nombre · Profesión

---

## 5. Sistema de diseño

### 5.1 Colores (tokens CSS)

```css
--black:  #0c0c0c;   /* fondo principal */
--panel:  #141415;   /* botones de contacto */
--ink:    #151515;   /* texto sobre papel */
--white:  #ffffff;
--paper:  #f6f6f4;   /* hojas de lectura */
--silver: #b8b9bd;   /* textos secundarios sobre negro */
--silver-2: #8d8e93;
--line:   rgba(255,255,255,.14);  /* bordes sobre negro */
--line-dark: #e1e1df;             /* bordes sobre papel */
--silver-grad: linear-gradient(135deg,#ececee 0%,#a9aaaf 45%,#e2e3e6 70%,#8f9095 100%);
```

- La plata **solo** se usa en detalles: líneas, anillo de la foto, indicador de pestaña y bordes del sello. Nunca en bloques grandes.
- Si la marca del cliente tiene otro color, reemplaza a la plata como acento. El negro y el blanco se mantienen como base.

### 5.2 Tipografía

| Rol | Fuente | Uso |
|---|---|---|
| Display | **Cormorant Garamond** 500/600 | Nombre, títulos de sección, nombres de áreas |
| Texto | **Montserrat** 400/500/600 | Párrafos, botones, etiquetas |

- Nombre: 38 px, mayúsculas, espaciado `.14em`
- Etiquetas de sección: 11 px, mayúsculas, espaciado `.3em`, gris
- Párrafos: 14 px, interlineado 1,65
- Las fuentes se incluyen en el proyecto como `.woff2` (no se cargan de servicios externos).

### 5.3 Formas y espacios

- Radio: 14 px en tarjetas y botones, 22 px en las hojas de papel, píldoras de 999 px.
- Márgenes laterales: entre 22 y 24 px.
- Sombras solo en el botón principal y en la foto.
- Íconos: SVG de línea, grosor 1,5, en el mismo trazo que el texto.

---

## 6. Redacción y marketing profesional

- **Voz:** tercera persona y tono sobrio, sin superlativos vacíos.
- **Credencial primero:** el dato de formación más reconocible va en el sello del Inicio y en la línea del encabezado.
- **De la experiencia al servicio:** el párrafo de Ejercicio conecta la trayectoria con lo que la persona ofrece hoy.
- **Áreas de práctica:** se ordenan con las de experiencia destacada primero y luego las demás. Cada una describe servicios concretos que el cliente reconoce, no categorías abstractas.
- **Referencias normativas** (solo profesiones reguladas, como derecho o salud): se cita la norma vigente que respalda el área, con su número y año exactos. Se verifica que esté vigente antes de publicar (ej. la conciliación se rige hoy por la Ley 2220 de 2022, no por la Ley 640 de 2001).
- **Validación:** antes de publicar, se le entrega al cliente la lista de descripciones o servicios que no salieron literalmente de sus insumos.

---

## 7. Recursos gráficos que se generan

| Archivo | Especificación |
|---|---|
| `monograma-blanco.png` / `-negro.png` | Logo con fondo transparente (el blanco del JPG se convierte en transparencia), recortado al contenido |
| `logo-horizontal-blanco.png` / `-negro.png` | Igual, versión horizontal |
| `foto-perfil.jpg` | Recorte cuadrado de 400×400 del rostro, sin marcos y con aire sobre la cabeza; se muestra en círculo |
| `og-image.jpg` | **1200×630**, menos de 300 KB: fondo negro, logo horizontal en blanco, línea plateada, credencial en versalitas y la frase "Agenda tu cita virtual" |
| `favicon.png`, `apple-touch-icon.png` | Monograma blanco sobre negro |
| `nombre.vcf` | vCard 3.0: nombre, profesión, teléfono, correo, ciudad y una nota con la credencial |

---

## 8. Diferencias entre el prototipo en Claude y la versión en Vercel

El enlace de Claude bloquea algunas funciones. El diseño es el mismo, pero cambian estos botones:

| Elemento | Prototipo en Claude | Versión en Vercel |
|---|---|---|
| Correo | Copia el correo | Abre el correo (`mailto:`) |
| Guardar contacto | Copia los datos | Descarga el `.vcf` |
| Compartir | No se incluye | Menú del celular o copiar enlace |
| Vista previa en WhatsApp | No aparece | Aparece (etiquetas Open Graph) |
| Recursos | Incrustados en el HTML | Carpeta `assets/` |
| Enlaces externos (WhatsApp, agenda) | Funcionan | Funcionan |

**Método recomendado:** se mantiene un solo `index.html` (el de Vercel) con marcas `<!--MAIL-START-->`, `<!--VCF-START-->`, `<!--SHARE-START-->` y un script de construcción que genera la versión de Claude reemplazando esas secciones. Así ningún cambio se hace dos veces.

---

## 9. Publicación en Vercel

1. En `index.html`, reemplazar `SITIO_URL` (aparece 2 veces) por el dominio final. Sin este paso, WhatsApp no muestra la vista previa.
2. Subir la carpeta a Vercel (arrastrar la carpeta o conectar un repositorio de GitHub). No requiere compilación.
3. `vercel.json` asigna el tipo `text/vcard` a los archivos `.vcf`.
4. Probar la vista previa en opengraph.xyz o en el Sharing Debugger de Facebook.
5. WhatsApp guarda en caché las vistas previas. Si cambia la imagen, se comparte el enlace con `?v=2`.

Estructura de la carpeta:

```
index.html
vercel.json
LEEME.md
nombre.vcf
assets/
  og-image.jpg  foto-perfil.jpg  favicon.png  apple-touch-icon.png
  monograma-blanco.png  monograma-negro.png
  logo-horizontal-blanco.png  logo-horizontal-negro.png
  *.woff2 (Cormorant Garamond y Montserrat)
```

---

## 10. Lista de verificación antes de entregar

- [ ] Ningún dato inventado; los añadidos están marcados para validación
- [ ] Revisado en 390 px de ancho: nada se corta y el correo no se parte en un lugar raro
- [ ] Las 4 pestañas cambian bien y los botones "Siguiente" llevan a la pestaña correcta
- [ ] El enlace de WhatsApp y el de la agenda abren desde el celular
- [ ] Copiar teléfono y copiar correo muestran el aviso de confirmación
- [ ] El `.vcf` se importa en iPhone y en Android (versión Vercel)
- [ ] La imagen `og-image.jpg` pesa menos de 300 KB y la vista previa aparece en WhatsApp (versión Vercel)
- [ ] La foto queda centrada en el círculo, sin marcos
- [ ] Las normas citadas están vigentes y bien escritas
- [ ] El prototipo de Claude está compartido como público antes de enviarlo

---

## 11. Instrucción para iniciar una nueva tarjeta en Claude

Copiar, completar y enviar junto con este documento:

```
Crea una tarjeta virtual siguiendo la "Instrucción base · Tarjetas virtuales" adjunta.

Persona: [nombre completo] · [profesión]
Credencial principal: [institución / título]
WhatsApp: [número o enlace]   Correo: [correo]   Ubicación: [ciudad]
Agenda: [enlace o "pendiente"]
Colores: [negro, blanco y plata / otros]
Pestañas: Inicio · Formación · Experiencia · [Ejercicio / otro nombre]
Experiencia destacada: [2 a 4 temas]
Áreas o servicios: [lista]
Modalidad de atención: [virtual / presencial / ambas]

Adjunto: logo (monograma y horizontal), foto de perfil y hoja de vida.
Primero publica el prototipo en Claude para aprobación; después entrega la carpeta para Vercel.
```
