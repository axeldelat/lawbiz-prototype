# Lawbiz Prototype Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build an interactive prototype Artifact demonstrating a case tracking platform for Lawbiz's amparo de autoconsumo legal process.

**Architecture:** Single HTML file published as a Claude Artifact. A JS state machine drives navigation across 9 screens (8 process steps + closing CTA). Each screen renders two panels (desktop admin 2/3, mobile client 1/3) plus a fixed narrative bar. Interactions are simulated via click handlers that trigger CSS animations on the mobile panel.

**Tech Stack:** HTML5, CSS3 (custom properties for Lawbiz brand tokens), vanilla JS, Tailwind CSS via CDN play script, Google Fonts (Inter for body, plus a condensed font for headings).

**Spec:** `docs/superpowers/specs/2026-09-03-lawbiz-prototype-design.md`

## Global Constraints

- Single HTML file, no frameworks beyond Tailwind CDN
- All data is fake/hardcoded — no API calls, no backend
- Optimized for desktop 1280px+ only
- Max file size: 16MB (Artifact limit)
- External scripts only from cdnjs.cloudflare.com or cdn.tailwindcss.com
- External fonts only from fonts.googleapis.com
- Lawbiz brand colors: `--lawbiz-dark: #1a3a1a`, `--lawbiz-lime: #7cb342`, `--lawbiz-white: #ffffff`, `--lawbiz-gray: #f5f5f5`
- No real personal data — all names, CURPs, addresses are fictional
- Logo rendered as inline SVG (stylized cannabis leaf arrow + "Lawbiz" text)

---

### Task 1: Core Shell — Layout, Navigation, and State Machine

Build the foundational layout with all three zones, the step state machine, and the Lawbiz branding. After this task, you should see the full layout skeleton with working Atrás/Siguiente navigation that cycles through 9 steps, the stepper updating, and placeholder content in each panel.

**Files:**
- Create: `prototype.html`

**Produces:**
- Global `app` object with `currentStep`, `goTo(step)`, `next()`, `prev()`, `renderStep()` methods
- CSS custom properties for Lawbiz brand tokens
- Layout grid: header, main (2/3 + 1/3), footer bar
- iPhone mockup frame component (CSS-only)
- Lawbiz logo inline SVG
- `STEPS` array of 9 objects with `{ id, title, narrative, interactions[] }`

- [ ] **Step 1: Create HTML file with Tailwind CDN, Google Fonts, and CSS custom properties**

```html
<title>Lawbiz — Plataforma de Seguimiento</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Oswald:wght@500;600;700&display=swap">
<style>
  :root {
    --lawbiz-dark: #1a3a1a;
    --lawbiz-lime: #7cb342;
    --lawbiz-lime-light: #a5d66b;
    --lawbiz-white: #ffffff;
    --lawbiz-gray: #f5f5f5;
    --lawbiz-gray-mid: #e0e0e0;
    --lawbiz-text: #333333;
    --lawbiz-text-light: #666666;
  }
  body { font-family: 'Inter', sans-serif; }
  h1, h2, h3, .font-display { font-family: 'Oswald', sans-serif; }
</style>
```

Configure Tailwind with Lawbiz colors:

```html
<script>
tailwind.config = {
  theme: {
    extend: {
      colors: {
        lawbiz: {
          dark: '#1a3a1a',
          lime: '#7cb342',
          'lime-light': '#a5d66b',
          gray: '#f5f5f5',
          'gray-mid': '#e0e0e0',
        }
      }
    }
  }
}
</script>
```

- [ ] **Step 2: Build the three-zone layout structure**

```html
<!-- HEADER -->
<header class="fixed top-0 w-full h-16 bg-lawbiz-dark flex items-center px-6 z-50">
  <!-- Logo SVG left, Stepper center-right -->
</header>

<!-- MAIN CONTENT -->
<main class="fixed top-16 bottom-20 w-full flex">
  <!-- Admin panel: 2/3 -->
  <div id="admin-panel" class="w-2/3 bg-white overflow-y-auto p-8">
  </div>
  <!-- Mobile panel: 1/3 -->
  <div id="mobile-wrapper" class="w-1/3 bg-lawbiz-gray flex items-center justify-center">
    <!-- iPhone frame goes here -->
  </div>
</main>

<!-- BOTTOM BAR -->
<footer class="fixed bottom-0 w-full h-20 bg-lawbiz-dark flex items-center px-6">
  <button id="btn-prev">← Atrás</button>
  <div id="narrative" class="flex-1 text-center text-white">...</div>
  <button id="btn-next">Siguiente →</button>
</footer>
```

- [ ] **Step 3: Create the Lawbiz logo as inline SVG**

Design a simplified SVG of the Lawbiz logo: a stylized cannabis leaf shaped like a downward arrow/chevron, followed by "Lawbiz" text. Use white fill for the header. Reference the uploaded images for the shape: three pointed leaves converging downward into a stem/arrow point.

```html
<svg viewBox="0 0 140 40" class="h-8">
  <!-- Leaf/arrow icon -->
  <path d="M12 2 C8 8 2 14 8 20 L16 12 Z" fill="white"/>
  <path d="M20 2 C24 8 30 14 24 20 L16 12 Z" fill="white"/>
  <path d="M16 10 L16 30" stroke="white" stroke-width="2.5"/>
  <!-- "Lawbis" text -->
  <text x="38" y="28" fill="white" font-family="Oswald" font-weight="600" font-size="22">Lawbis</text>
</svg>
```

Refine the SVG paths to match the actual Lawbiz logo shape from the uploaded images. The icon has 3-5 leaf blades converging to a central stem that forms a downward arrow.

- [ ] **Step 4: Build the stepper component**

Render 8 numbered circles connected by lines. The active step is larger with `bg-lawbiz-lime`, completed steps have `bg-lawbiz-lime` with a checkmark, future steps are `bg-gray-500`. Place it in the header to the right of the logo.

```html
<div id="stepper" class="flex items-center gap-1">
  <!-- Generated by JS: for each step, a circle + connecting line -->
</div>
```

```javascript
function renderStepper(currentStep) {
  const stepper = document.getElementById('stepper');
  stepper.innerHTML = '';
  for (let i = 0; i < 8; i++) {
    // Add connecting line (except before first)
    if (i > 0) {
      const line = document.createElement('div');
      line.className = `w-6 h-0.5 ${i <= currentStep ? 'bg-lawbiz-lime' : 'bg-gray-600'}`;
      stepper.appendChild(line);
    }
    // Add circle
    const circle = document.createElement('div');
    const isActive = i === currentStep;
    const isCompleted = i < currentStep;
    circle.className = `w-7 h-7 rounded-full flex items-center justify-center text-xs font-bold cursor-pointer transition-all ${
      isActive ? 'bg-lawbiz-lime text-lawbiz-dark scale-110' :
      isCompleted ? 'bg-lawbiz-lime/70 text-white' :
      'bg-gray-600 text-gray-300'
    }`;
    circle.textContent = isCompleted ? '✓' : i + 1;
    circle.onclick = () => app.goTo(i);
    stepper.appendChild(circle);
  }
}
```

- [ ] **Step 5: Build the iPhone mockup frame**

CSS-only iPhone frame: rounded rectangle with notch at top, positioned centered in the 1/3 panel. Inner content area scrollable.

```css
.iphone-frame {
  width: 280px;
  height: 560px;
  border-radius: 36px;
  border: 4px solid #1a1a1a;
  background: white;
  position: relative;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(0,0,0,0.15);
}
.iphone-notch {
  width: 100px;
  height: 24px;
  background: #1a1a1a;
  border-radius: 0 0 16px 16px;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
}
.iphone-content {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow-y: auto;
  padding-top: 32px;
}
```

```html
<div class="iphone-frame">
  <div class="iphone-notch"></div>
  <div id="mobile-content" class="iphone-content">
    <!-- Mobile screens render here -->
  </div>
</div>
```

- [ ] **Step 6: Build the state machine and navigation**

```javascript
const STEPS = [
  { id: 'dashboard', title: 'Dashboard', narrative: 'El gestor ve todos sus casos activos de un vistazo. Cada cliente tiene su propio seguimiento en tiempo real.', interactions: [] },
  { id: 'alta', title: 'Alta de caso', narrative: 'El gestor ingresa los datos y abre un nuevo expediente. El cliente recibe acceso inmediato a su panel y puede realizar su pago directamente en la plataforma.', interactions: ['crear-expediente'] },
  { id: 'solicitud', title: 'Solicitud', narrative: 'El sistema genera automáticamente la solicitud con los datos del expediente. Tu cliente recibe instrucciones claras sin que tengas que escribir un solo mensaje.', interactions: ['generar-solicitud', 'enviar-requisicion'] },
  { id: 'firma', title: 'Documentos firmados', narrative: 'Cuando el cliente sube sus documentos, el gestor los recibe al instante. No más fotos por WhatsApp sin contexto.', interactions: ['confirmar-recepcion'] },
  { id: 'cofepris', title: 'Ingreso COFEPRIS', narrative: 'El gestor registra el ingreso y el cliente ve su folio y estado en tiempo real. Sin necesidad de preguntar por WhatsApp cuánto falta.', interactions: ['registrar-ingreso'] },
  { id: 'respuesta', title: 'Respuesta COFEPRIS', narrative: 'La respuesta de la autoridad se registra y el cliente se entera de inmediato. El pago se gestiona dentro de la plataforma — no más transferencias manuales.', interactions: ['notificar-cliente'] },
  { id: 'didgi', title: 'Denuncia (DIDGI)', narrative: 'El último escrito se genera con toda la información del caso. El cliente recibe instrucciones paso a paso, incluyendo la opción de pagar una guía prepagada.', interactions: ['generar-didgi', 'enviar-requisicion-2'] },
  { id: 'espera-final', title: 'Espera final', narrative: 'El cliente ve todo su proceso de un vistazo: qué pasó, cuándo, y qué falta. Tranquilidad total.', interactions: [] },
];

const app = {
  currentStep: 0,
  interactionState: {},

  goTo(step) {
    if (step < 0 || step > 8) return;
    this.currentStep = step;
    this.interactionState = {};
    this.render();
  },
  next() { this.goTo(this.currentStep + 1); },
  prev() { this.goTo(this.currentStep - 1); },

  render() {
    renderStepper(this.currentStep);
    renderAdminPanel(this.currentStep, this.interactionState);
    renderMobilePanel(this.currentStep, this.interactionState);
    renderNarrative(this.currentStep);
    renderNavButtons(this.currentStep);
  },

  triggerInteraction(id) {
    this.interactionState[id] = true;
    renderAdminPanel(this.currentStep, this.interactionState);
    renderMobilePanel(this.currentStep, this.interactionState);
    // Update narrative if interaction has custom text
    const step = STEPS[this.currentStep];
    if (step.interactionNarratives && step.interactionNarratives[id]) {
      document.getElementById('narrative-text').textContent = step.interactionNarratives[id];
    }
  }
};
```

Wire up buttons:

```javascript
document.getElementById('btn-prev').onclick = () => app.prev();
document.getElementById('btn-next').onclick = () => app.next();
document.addEventListener('DOMContentLoaded', () => app.render());
```

- [ ] **Step 7: Build the narrative bar and nav button rendering**

```javascript
function renderNarrative(step) {
  const el = document.getElementById('narrative-text');
  if (step >= STEPS.length) {
    el.textContent = '';
    return;
  }
  el.textContent = STEPS[step].narrative;
  // Add interaction hint if step has pending interactions
  const s = STEPS[step];
  if (s.interactions.length > 0 && !Object.keys(app.interactionState).length) {
    el.textContent += ' 👆 Haz clic en los botones resaltados para ver la interacción.';
  }
}

function renderNavButtons(step) {
  const prev = document.getElementById('btn-prev');
  const next = document.getElementById('btn-next');
  prev.disabled = step === 0;
  prev.className = step === 0 ? 'opacity-30 cursor-not-allowed ...' : '...';
  if (step >= 8) {
    next.textContent = '';
    next.disabled = true;
  } else if (step === 7) {
    next.textContent = 'Ver cierre →';
  } else {
    next.textContent = 'Siguiente →';
  }
}
```

- [ ] **Step 8: Add placeholder render functions and verify the shell**

Create stub `renderAdminPanel()` and `renderMobilePanel()` that show the step title and ID as placeholder text. Publish the Artifact and verify:
- Layout splits correctly (2/3 + 1/3)
- Stepper updates on navigation
- iPhone frame renders with notch
- Narrative text changes per step
- Atrás disabled on step 0
- "Ver cierre" on step 7
- Step 8 (index 8) shows the CTA/closing screen
- Logo visible in header

---

### Task 2: Pantallas 1-3 — Dashboard, Alta de Caso, Solicitud

Build the content for the first three screens with their interactions. After this task, screens 1-3 are fully functional with working interactions (crear expediente, generar solicitud, enviar requisición).

**Files:**
- Modify: `prototype.html`

**Consumes:**
- `app.triggerInteraction(id)`, `renderAdminPanel(step, state)`, `renderMobilePanel(step, state)` from Task 1

**Produces:**
- Fully rendered Pantallas 1-3 with admin content, mobile content, and interaction handlers

- [ ] **Step 1: Build Pantalla 1 — Dashboard (admin side)**

Replace the placeholder in `renderAdminPanel` for step 0. Render a table with 5 fake cases:

| Cliente | Expediente | Estado | Fecha inicio |
|---------|-----------|--------|-------------|
| María García López | EXP-2026-001 | En trámite | 15/03/2026 |
| Roberto Méndez Parra | EXP-2026-002 | Esperando COFEPRIS | 28/01/2026 |
| Laura Sánchez Vidal | EXP-2026-003 | Nuevo | 01/09/2026 |
| Carlos Reyes Fuentes | EXP-2026-004 | Resuelto | 10/11/2025 |
| Ana Lucía Torres | EXP-2026-005 | En trámite | 22/06/2026 |

Row for María highlighted with `bg-lawbiz-lime/10` and a left border in `lawbiz-lime`. Status badges colored: Nuevo=blue, En trámite=yellow, Esperando=orange, Resuelto=green.

- [ ] **Step 2: Build Pantalla 1 — Dashboard (mobile side)**

Render the Lawbiz app splash screen inside the iPhone frame: centered logo SVG (larger, in green), text "Bienvenido a Lawbiz", subtitle "Seguimiento de tu trámite legal", and a subtle loading indicator or pulsing dot.

- [ ] **Step 3: Build Pantalla 2 — Alta de caso (admin side)**

Render a case header ("Nuevo expediente") and a form with pre-filled fake data:
- Nombre: María García López
- CURP: GALM890512MYNRPR01
- Domicilio: Av. Reforma 234, Col. Centro, Mérida, Yucatán, CP 97000
- Teléfono: +52 999 123 4567
- Correo: maria.garcia@email.com

Fields are styled as read-only inputs (gray bg) since we're showing them pre-filled. Below: a prominent green button "Crear expediente" with `id="btn-crear-expediente"`.

Wire interaction:
```javascript
document.getElementById('btn-crear-expediente').onclick = () => app.triggerInteraction('crear-expediente');
```

- [ ] **Step 4: Build Pantalla 2 — Alta de caso (mobile side)**

**Before interaction (`crear-expediente` not triggered):** Show a grayed-out empty state: "Esperando datos del gestor..."

**After interaction:** Animate in (fade + slide-up) a welcome screen:
- "¡Hola, María!" in large text
- "Tu expediente ha sido creado"
- Below: a card with "Paso siguiente: Realiza tu pago" showing a mock Stripe payment form: card number field (pre-filled with •••• •••• •••• 4242), expiry, CVC, amount "$1,000 MXN", and a green "Pagar" button (non-functional). Badge "Powered by Stripe" in gray.

- [ ] **Step 5: Build Pantalla 3 — Solicitud (admin side)**

Case header showing "María García López — EXP-2026-001" with badge "En trámite". Below:

**Before `generar-solicitud` interaction:** A card titled "Generar Solicitud ante COFEPRIS" with description text and a green button "Generar Solicitud".

**After `generar-solicitud`:** The card transforms to show a mock PDF preview — a bordered rectangle with the Lawbiz logo header, text "SOLICITUD DE AUTORIZACIÓN SANITARIA", "TITULAR DE LA COMISIÓN FEDERAL...", client name, partial body text. Styled to look like a document page. Below: a new green button "Enviar requisición de firma al cliente".

**After `enviar-requisicion`:** The button changes to a green checkmark badge "✓ Requisición enviada". The admin narrative updates to hint at looking at the mobile panel.

- [ ] **Step 6: Build Pantalla 3 — Solicitud (mobile side)**

**Before interactions:** Timeline with Step 1 (Expediente creado) completed (green + checkmark), Step 2 (Pago inicial) completed, Step 3 (Solicitud) shown as pending/gray.

**After `enviar-requisicion`:** Animate in a push notification at the top of the iPhone: "Lawbiz — Acción requerida". Then the timeline updates: Step 3 becomes active (green lime, expanded) showing:
- "Firma de Solicitud requerida"
- Instructions: "1. Descarga el documento  2. Imprímelo  3. Firma autógrafamente  4. Escanéalo y súbelo aquí"
- A mock "Subir documento" button
- A link "¿Prefieres enviarlo físicamente? Ver dirección"

- [ ] **Step 7: Publish Artifact and verify Pantallas 1-3**

Verify:
- Dashboard table renders with 5 cases, María highlighted
- Mobile splash screen shows on Pantalla 1
- Form with pre-filled data on Pantalla 2
- "Crear expediente" button triggers mobile welcome + payment
- "Generar Solicitud" shows PDF preview on Pantalla 3
- "Enviar requisición" triggers push notification + timeline update on mobile
- Stepper updates correctly across steps 1-3
- Narrative text matches spec for each step
- Animations are smooth (fade-in, slide-up)

---

### Task 3: Pantallas 4-6 — Firma, Ingreso COFEPRIS, Respuesta

Build screens 4-6 with their interactions. These cover the middle of the process: receiving signed documents, registering with COFEPRIS, and handling the response.

**Files:**
- Modify: `prototype.html`

**Consumes:**
- `app.triggerInteraction(id)`, render functions, Lawbiz CSS tokens from Task 1
- Timeline component pattern from Task 2

**Produces:**
- Fully rendered Pantallas 4-6 with admin content, mobile content, and interaction handlers

- [ ] **Step 1: Build Pantalla 4 — Documentos firmados (admin side)**

Case header "María García López — EXP-2026-001", badge "En trámite".

A notification card at the top: amber/yellow background, icon, "El cliente ha enviado sus documentos firmados — hace 2 horas". Below: a mock document preview (a rectangle styled like a scanned page — slightly rotated, with a fake signature scribble at the bottom). Below the preview: green button "Confirmar recepción".

**After `confirmar-recepcion`:** Button changes to "✓ Documentos confirmados" badge. A new info card appears: "Siguiente paso: Ingresar solicitud ante COFEPRIS".

- [ ] **Step 2: Build Pantalla 4 — Documentos firmados (mobile side)**

Timeline with Steps 1-3 completed. Step 4 ("Documentos firmados") starts as active with a "Enviado — En revisión por Lawbiz" status.

**After `confirmar-recepcion`:** Step 4 animates to completed (green checkmark drawing animation via CSS). Step 5 appears as the next pending step.

CSS checkmark animation:
```css
@keyframes drawCheck {
  0% { stroke-dashoffset: 24; }
  100% { stroke-dashoffset: 0; }
}
.check-animate svg path {
  stroke-dasharray: 24;
  animation: drawCheck 0.4s ease-out forwards;
}
```

- [ ] **Step 3: Build Pantalla 5 — Ingreso COFEPRIS (admin side)**

Case header with badge "En trámite". Content:

**Before `registrar-ingreso`:** A form card "Registrar ingreso ante COFEPRIS" with:
- Input field "Folio COFEPRIS" pre-filled: 263301EL789456
- Input field "Guía SEPOMEX" pre-filled: RN208033456MX
- Green button "Registrar ingreso"

**After `registrar-ingreso`:** The form transforms to a status card with green background:
- "✓ Ingresado ante COFEPRIS"
- Folio and tracking number displayed
- Status: "En evaluación — Tiempo estimado: 2-3 meses"
- A mock SEPOMEX tracking timeline (4 rows): Mérida → Centro Distribución → Oficina CDMX → Entregado

- [ ] **Step 4: Build Pantalla 5 — Ingreso COFEPRIS (mobile side)**

Timeline with Steps 1-4 completed.

**Before interaction:** Step 5 shown as active but with text "Pendiente de ingreso ante COFEPRIS".

**After `registrar-ingreso`:** Animate in: Step 5 becomes active with expanded content:
- "Tu solicitud fue ingresada ante COFEPRIS"
- Folio: 263301EL789456 (in a copyable badge)
- A progress bar showing "En evaluación"
- "Tiempo estimado de respuesta: 2-3 meses"
- Small text: "Te notificaremos cuando haya una actualización"

- [ ] **Step 5: Build Pantalla 6 — Respuesta COFEPRIS (admin side)**

Case header with badge "Respuesta recibida" (amber).

An alert card at the top: "COFEPRIS ha emitido respuesta — Oficio 263301EL789456". Below: a mock preview of the official COFEPRIS response (styled like the real document from the screenshots — Secretaría de Salud + COFEPRIS logos in gray, official header, "ASUNTO: RESPUESTA A SOLICITUD", partial body text). Below: green button "Notificar al cliente y solicitar pago".

**After `notificar-cliente`:** Button changes to confirmed badge. A payment status card appears: "Pago solicitado: $1,000 MXN — Pendiente".

- [ ] **Step 6: Build Pantalla 6 — Respuesta COFEPRIS (mobile side)**

Timeline with Steps 1-5 completed.

**Before interaction:** Step 6 shown as active with "La autoridad ha respondido — Revisa los detalles".

**After `notificar-cliente`:** Animate push notification. Step 6 expands to show:
- "COFEPRIS ha emitido su respuesta"
- A collapsible summary of the response
- Divider
- "Pago requerido" card with Stripe mock form: $1,000 MXN, "Pagar" button

- [ ] **Step 7: Verify Pantallas 4-6**

Publish and verify:
- Document preview with fake scan/signature on Pantalla 4
- Checkmark animation when confirming reception
- COFEPRIS form with pre-filled folio on Pantalla 5
- Tracking timeline appears after registering
- Official response preview on Pantalla 6
- Payment form appears in mobile after notifying
- Timeline accumulates completed steps correctly across screens
- All interactions trigger proper state changes

---

### Task 4: Pantallas 7-9 — DIDGI, Espera Final, y CTA de Cierre

Build the final three screens completing the demo experience.

**Files:**
- Modify: `prototype.html`

**Consumes:**
- All patterns from Tasks 1-3

**Produces:**
- Complete 9-screen demo with all interactions working end-to-end

- [ ] **Step 1: Build Pantalla 7 — DIDGI Demanda (admin side)**

Case header "María García López", badge "En trámite".

**Before `generar-didgi`:** Card "Generar Denuncia por Incumplimiento" with description: "Último escrito del proceso. Se presenta ante el Juzgado de Distrito contra la negativa de COFEPRIS." Green button "Generar Denuncia".

**After `generar-didgi`:** Mock PDF preview of the DIDGI document: Lawbiz header, "DENUNCIA POR INCUMPLIMIENTO A LA DECLARATORIA GENERAL DE INCONSTITUCIONALIDAD", "JUEZ DE DISTRITO DEL DECIMOCUARTO CIRCUITO...", partial body text. Below: green button "Enviar requisición de firma al cliente". Info text: "El cliente debe firmar 1 original + 3 copias".

**After `enviar-requisicion-2`:** Button changes to confirmed badge.

- [ ] **Step 2: Build Pantalla 7 — DIDGI Demanda (mobile side)**

Timeline with Steps 1-6 completed.

**Before interactions:** Step 7 pending.

**After `enviar-requisicion-2`:** Push notification animation. Step 7 active with:
- "Firma de Denuncia requerida"
- Instructions: "1. Descarga el documento  2. Imprime 1 original + 3 copias  3. Firma el original  4. Envía todo a la dirección indicada"
- "Subir documentos" button
- Card: "Guía prepagada disponible" with "$190 MXN — Pagar y descargar guía" mock Stripe button

- [ ] **Step 3: Build Pantalla 8 — Espera final (admin side)**

Case header with badge "Demanda ingresada" (blue).

Full case summary view:
- Status card: "Demanda ingresada ante Juzgado de Distrito — Esperando resolución judicial. Tiempo estimado: 3-4 meses"
- Complete case timeline (vertical, all 8 steps with dates)
- Payment summary panel: 3 rows (Pago inicial $1,000 ✓, Segundo pago $1,000 ✓, Guía prepagada $190 ✓, Total: $2,190 MXN)
- Client contact info card

- [ ] **Step 4: Build Pantalla 8 — Espera final (mobile side)**

Complete timeline with all 7 steps in green/completed. Step 8 active:
- "Esperando resolución del juzgado"
- Progress indicator with "Tiempo estimado: 3-4 meses"
- Message card with green border: "Estás en el último paso. Te notificaremos cuando llegue tu autorización."
- Below timeline: "¿Tienes dudas? Contáctanos" link

- [ ] **Step 5: Build Pantalla 9 — CTA de cierre**

This screen replaces the normal layout. Full-screen dark green (`bg-lawbiz-dark`) with centered content:
- Large Lawbiz logo (SVG, white)
- Heading: "Esto es lo que Lawbiz puede ofrecer a cada cliente"
- Three value-prop bullets with icons:
  - "Seguimiento en tiempo real — Tu cliente siempre sabe en qué paso está"
  - "Pagos integrados — Sin transferencias manuales ni confusiones"
  - "Documentos automatizados — Genera escritos en un clic"
- Subheading: "¿Listo para transformar tu proceso?"
- Call-to-action button (lime): "Platiquemos"
- The stepper in the header hides or shows all 8 steps as completed

- [ ] **Step 6: Verify complete flow end-to-end**

Publish and run through the entire demo from Pantalla 1 to 9:
- All 10 interactions work correctly
- Timeline in mobile accumulates properly (more green steps each screen)
- Animations fire correctly on interactions
- Narrative text is accurate per step
- Stepper reflects progress
- CTA screen replaces normal layout
- Atrás from CTA returns to Pantalla 8
- Overall flow tells a coherent story

---

### Task 5: Visual Polish — Animations, Transitions, and Brand Refinement

Final pass to polish interactions, ensure brand consistency, and add the subtle animations that make the prototype feel professional.

**Files:**
- Modify: `prototype.html`

**Consumes:**
- Complete 9-screen prototype from Task 4

**Produces:**
- Polished, production-quality prototype ready for sharing

- [ ] **Step 1: Add screen transition animations**

When navigating between steps, add a subtle fade transition on the admin and mobile panels:

```css
.panel-transition {
  animation: fadeIn 0.3s ease-out;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}
```

Apply `.panel-transition` class to both panels on each render, removing it briefly to retrigger the animation.

- [ ] **Step 2: Add interaction-specific animations**

- Push notification: slide down from top of iPhone frame, slight bounce
- Payment form appearance: fade + scale from 0.95 to 1
- Checkmark: SVG stroke drawing animation (already in Task 3)
- PDF preview: fade in with subtle shadow growing
- Status badge changes: color transition with 0.3s ease

```css
@keyframes slideDown {
  from { transform: translateY(-100%); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
@keyframes scaleIn {
  from { transform: scale(0.95); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}
.push-notification { animation: slideDown 0.4s cubic-bezier(0.34, 1.56, 0.64, 1); }
.scale-in { animation: scaleIn 0.3s ease-out; }
```

- [ ] **Step 3: Refine the mobile timeline component**

Ensure the timeline is visually consistent across all screens:
- Vertical line connecting nodes: solid 2px, gray for future, green for completed
- Node circles: 28px, centered on the line
- Active node: 32px, ring pulse animation
- Consistent spacing between nodes
- Date stamps aligned right of each node

```css
@keyframes pulse-ring {
  0% { box-shadow: 0 0 0 0 rgba(124, 179, 66, 0.4); }
  70% { box-shadow: 0 0 0 8px rgba(124, 179, 66, 0); }
  100% { box-shadow: 0 0 0 0 rgba(124, 179, 66, 0); }
}
.node-active { animation: pulse-ring 2s ease-out infinite; }
```

- [ ] **Step 4: Refine admin panel styling**

- Cards with consistent border-radius (12px), subtle shadow, 1px border `lawbiz-gray-mid`
- Interactive buttons: `bg-lawbiz-lime hover:bg-lawbiz-lime-light` with smooth transition, cursor pointer
- Confirmed badges: green bg with white text and checkmark icon
- Form inputs: clean borders, proper padding, Lawbiz green focus ring
- Document previews: slight paper shadow, page-like aspect ratio
- Alert/notification cards: colored left border (4px)

- [ ] **Step 5: Final Lawbiz brand pass**

- Verify all greens match the Lawbiz brand from uploaded images
- Ensure logo SVG closely resembles the real Lawbiz mark
- Check font hierarchy: Oswald for headings, Inter for body
- Verify dark green header and footer look rich, not muddy
- Check contrast: white text on dark green, dark text on light backgrounds
- Ensure the iPhone frame looks realistic (proper proportions, shadow, notch)

- [ ] **Step 6: Final publish and complete walkthrough**

Publish final Artifact. Walk through the entire demo one last time verifying:
- Professional visual quality appropriate for a client pitch
- All 10 interactions work
- No visual glitches or layout breaks
- Narrative text is error-free and compelling
- Smooth transitions between all screens
- Mobile timeline tells the story clearly
- CTA closing screen feels like a strong finish
- Overall impression: "this team understands our business"
