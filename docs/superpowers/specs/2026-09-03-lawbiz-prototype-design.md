# Lawbiz — Prototipo de Plataforma de Seguimiento de Trámites

## Objetivo

Construir un prototipo interactivo (Artifact publicado) que demuestre a Lawbiz cómo se vería un sistema de seguimiento de trámites de amparo de autoconsumo. El prototipo reemplaza conceptualmente su proceso actual por WhatsApp con una plataforma donde el gestor legal y el cliente interactúan de forma estructurada.

El objetivo NO es un producto funcional — es vender la idea demostrando entendimiento profundo del negocio.

## Audiencia

El equipo de Lawbiz (despacho Varante Legal). Lo abrirán en desktop desde un link compartido sin explicación previa. Debe ser autoexplicativo.

## Formato y tecnología

- Artifact publicado de Claude (HTML/CSS/JS)
- Se abre en cualquier navegador, sin instalación
- Todo es fake/mocked — no hay backend, no hay datos reales
- Datos ficticios (nunca datos reales del cliente)

## Identidad visual

- Colores: verde oscuro (~#1B3A1B), verde lima (~#7CB342), blanco, grises claros
- Logo de Lawbiz con la hoja de cannabis estilizada como flecha
- Tipografía bold/condensada para títulos
- Estilo general: limpio, profesional, tipo Notion/Linear

## Layout

### Estructura de pantalla (desktop)

```
┌─────────────────────────────────────────────────────────┐
│  Logo Lawbis    │    Stepper: ① ② ③ ④ ⑤ ⑥ ⑦ ⑧        │
├────────────────────────────────┬────────────────────────┤
│                                │                        │
│     PANEL GESTOR (2/3)         │   MOCKUP MÓVIL (1/3)   │
│     Vista desktop              │   Frame de iPhone       │
│     Sistema de gestión         │   Vista del cliente     │
│                                │                        │
│                                │                        │
├────────────────────────────────┴────────────────────────┤
│  [ < Atrás ]     Globo narrativo explicativo  [ Sig > ] │
└─────────────────────────────────────────────────────────┘
```

### Zona superior (fija)
- Barra de navegación con logo Lawbis
- Stepper horizontal mostrando los 8 pasos. Paso activo resaltado en verde lima, completados en verde oscuro, futuros en gris.

### Panel izquierdo — Gestor (2/3)
- Encabezado del caso: nombre del cliente, número de expediente, badge de estado
- Zona de contenido dinámica según el paso: formularios, previews de PDF, estados de espera, documentos recibidos
- Mini-timeline lateral derecho del expediente
- Información de contacto del cliente
- Historial de pagos

### Panel derecho — Cliente móvil (1/3)
- Frame visual de iPhone (bordes redondeados, notch)
- Header: logo Lawbis, saludo, nombre del trámite, paso actual
- Timeline vertical como núcleo de la experiencia:
  - Nodos completados: verde oscuro + palomita
  - Nodo activo: verde lima, expandido, con acciones/instrucciones
  - Nodos futuros: gris, colapsados
- Cada nodo muestra: título, fecha, y al expandir: instrucciones, botones de acción, estado de espera

### Zona inferior (fija)
- Globo narrativo: fondo verde oscuro, texto blanco, bordes redondeados
- Texto pre-escrito por paso, tono de pitch profesional dirigido a Lawbiz
- En pasos interactivos incluye indicación: "Haz clic en X para ver cómo funciona"
- Botones Atrás y Siguiente a los lados
- Atrás deshabilitado en paso 1, Siguiente cambia a "Fin de la demo" en el último paso

## Pantallas de la demo

### Pantalla 1 — Dashboard
- **Gestor:** Tabla con 4-5 casos fake en distintos estados (Nuevo, En trámite, Esperando COFEPRIS, Resuelto). Caso resaltado: "María García López".
- **Móvil:** Pantalla de inicio de la app Lawbiz con logo y mensaje genérico: "Bienvenido a Lawbiz — Seguimiento de tu trámite legal."
- **Narrativa:** "El gestor ve todos sus casos activos de un vistazo. Cada cliente tiene su propio seguimiento en tiempo real."

### Pantalla 2 — Alta de nuevo caso + pago inicial
- **Gestor:** Formulario con datos del cliente (nombre, CURP, domicilio, teléfono, correo). Botón interactivo "Crear expediente".
- **Móvil:** Al hacer clic en crear: animación de bienvenida + paso de pago con formulario tipo Stripe ($1,000 MXN).
- **Narrativa:** "El gestor ingresa los datos y abre un nuevo expediente. El cliente recibe acceso inmediato a su panel y puede realizar su pago directamente en la plataforma."
- **Interacción:** Clic en "Crear expediente" -> móvil se actualiza con animación.

### Pantalla 3 — Generación y envío de la Solicitud
- **Gestor:** Vista del expediente con botón interactivo "Generar Solicitud ante COFEPRIS". Al hacer clic: preview del PDF. Botón interactivo "Enviar requisición de firma al cliente".
- **Móvil:** Al enviar: notificación tipo push entrando. Paso activo: "Firma de Solicitud requerida". Instrucciones: imprimir, firmar, escanear. Botón subir documento o ver dirección de envío.
- **Narrativa:** "El sistema genera automáticamente la solicitud con los datos del expediente. Tu cliente recibe instrucciones claras sin que tengas que escribir un solo mensaje."
- **Interacción:** Clic en "Generar" -> aparece PDF. Clic en "Enviar" -> móvil muestra notificación.

### Pantalla 4 — Cliente envía documentos firmados
- **Gestor:** Notificación en expediente: "El cliente ha enviado documentos firmados". Preview del documento. Botón interactivo "Confirmar recepción".
- **Móvil:** Al confirmar: paso cambia a completado con palomita animada.
- **Narrativa:** "Cuando el cliente sube sus documentos, el gestor los recibe al instante. No más fotos por WhatsApp sin contexto."
- **Interacción:** Clic en "Confirmar recepción" -> palomita animada en móvil.

### Pantalla 5 — Ingreso ante COFEPRIS + espera
- **Gestor:** Botón interactivo "Registrar ingreso ante COFEPRIS". Campos para folio COFEPRIS y guía SEPOMEX. Al hacer clic: estado cambia a "En evaluación — Tiempo estimado: 2-3 meses". Tracking visible.
- **Móvil:** Folio visible, estado "En evaluación", barra de tiempo estimado.
- **Narrativa:** "El gestor registra el ingreso y el cliente ve su folio y estado en tiempo real. Sin necesidad de preguntar por WhatsApp cuánto falta."
- **Interacción:** Clic en "Registrar ingreso" -> móvil muestra folio y barra de progreso.

### Pantalla 6 — Respuesta de COFEPRIS + segundo pago
- **Gestor:** Alerta: "COFEPRIS ha respondido". Preview del oficio. Botón interactivo "Notificar al cliente y solicitar pago".
- **Móvil:** Alerta de respuesta recibida. Detalle de resolución. Formulario de pago Stripe ($1,000 MXN).
- **Narrativa:** "La respuesta de la autoridad se registra y el cliente se entera de inmediato. El pago se gestiona dentro de la plataforma — no más transferencias manuales ni cambios de cuenta."
- **Interacción:** Clic en "Notificar" -> móvil muestra alerta + pago.

### Pantalla 7 — Generación y firma de la Demanda (DIDGI)
- **Gestor:** Botón interactivo "Generar Denuncia por Incumplimiento". Preview del DIDGI. Botón "Enviar requisición de firma". Instrucciones: 1 original + 3 copias.
- **Móvil:** Paso activo: "Firma de Denuncia requerida". Instrucciones detalladas. Botón subir documentos. Opción de guía prepagada con pago integrado ($190 MXN).
- **Narrativa:** "El último escrito se genera con toda la información del caso. El cliente recibe instrucciones paso a paso, incluyendo la opción de pagar una guía prepagada directamente."
- **Interacción:** Clic en "Generar" -> preview. Clic en "Enviar" -> móvil actualizado.

### Pantalla 8 — Demanda ingresada + espera final
- **Gestor:** Estado final: "Demanda ingresada — Esperando resolución judicial. Tiempo estimado: 3-4 meses." Timeline completo visible, todos los pasos anteriores completados. Historial de pagos completo.
- **Móvil:** Timeline completo con todos los pasos en verde excepto el último. "Esperando resolución del juzgado." Mensaje: "Estás en el último paso. Te notificaremos cuando llegue tu autorización."
- **Narrativa:** "El cliente ve todo su proceso de un vistazo: qué pasó, cuándo, y qué falta. Tranquilidad total."

### Pantalla final — Cierre / CTA
- Pantalla tipo landing con mensaje de pitch: "Esto es lo que Lawbiz puede ofrecer a cada cliente. ¿Listo para transformar tu proceso?"
- Call-to-action genérico invitando a dar el siguiente paso

## Interacciones simuladas

Las interacciones son el diferenciador del prototipo. En ciertos pasos, el espectador puede hacer clic en botones del panel del gestor y ver el efecto reflejado en el panel móvil del cliente.

Lista de interacciones:

| Pantalla | Acción del gestor | Efecto en móvil |
|----------|-------------------|-----------------|
| 2 | Clic "Crear expediente" | Aparece bienvenida + paso de pago |
| 3 | Clic "Generar Solicitud" | Preview PDF aparece en gestor |
| 3 | Clic "Enviar requisición" | Push notification + paso activo en móvil |
| 4 | Clic "Confirmar recepción" | Palomita animada en timeline |
| 5 | Clic "Registrar ingreso" | Folio + barra de progreso en móvil |
| 6 | Clic "Notificar al cliente" | Alerta + formulario de pago en móvil |
| 7 | Clic "Generar Denuncia" | Preview DIDGI en gestor |
| 7 | Clic "Enviar requisición" | Paso activo + instrucciones en móvil |

Animaciones: fade-in, slide-from-top para notificaciones, palomita dibujándose. Sutiles, no exageradas.

## Datos ficticios

- Cliente de demo: María García López
- CURP: GALM890512MYNRPR01
- Domicilio: Av. Reforma 234, Col. Centro, Mérida, Yucatán, CP 97000
- Folio COFEPRIS: 263301EL789456
- Guía SEPOMEX: RN208033456MX
- Otros casos en dashboard: nombres mexicanos ficticios con distintos estados

## Navegación

- Botones Atrás/Siguiente en barra inferior
- Stepper superior clickeable (pero la narrativa fluye mejor de forma secuencial)
- Atrás deshabilitado en Pantalla 1
- Siguiente cambia a "Ver cierre" en Pantalla 8
- Pantalla final solo tiene botón Atrás

## Fuera de alcance

- No hay backend ni base de datos
- No hay autenticación real
- No hay procesamiento de pagos real (Stripe es visual/mock)
- No hay generación real de PDFs (se muestra un preview estático)
- No hay envío de notificaciones reales
- No hay responsive — optimizado para desktop (1280px+)
- No hay datos reales del cliente (todo ficticio)
