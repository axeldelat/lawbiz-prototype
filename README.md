# Lawbiz — Plataforma de Seguimiento de Trámites

Prototipo interactivo que demuestra cómo se vería un sistema de seguimiento de trámites de **amparo de autoconsumo** para [Lawbiz](https://github.com/axeldelat/lawbiz-prototype) (despacho Varante Legal).

> **Esto no es un producto funcional** — es una herramienta de venta para demostrar entendimiento del negocio y la visión de lo que se puede construir.

## Demo

**[Abrir prototipo](https://claude.ai/code/artifact/6fc21475-abbb-492b-8ec1-fe5a9abd0b4c)** — se abre directo en el navegador, sin instalación.

También puedes abrir `prototype.html` directamente como archivo local en cualquier navegador.

## Qué hace

El prototipo muestra una demo guiada de 9 pantallas con layout split-screen:

- **Panel izquierdo (2/3):** Vista del gestor legal — dashboard, formularios, documentos generados, estados de caso
- **Panel derecho (1/3):** Vista del cliente en un mockup de iPhone — timeline de seguimiento, notificaciones, pagos

### Las 9 pantallas

| # | Pantalla | Interacciones |
|---|----------|---------------|
| 1 | Dashboard de casos | — |
| 2 | Alta de nuevo caso + pago | Crear expediente |
| 3 | Generación y envío de Solicitud | Generar Solicitud, Enviar requisición |
| 4 | Recepción de documentos firmados | Confirmar recepción |
| 5 | Ingreso ante COFEPRIS | Registrar ingreso |
| 6 | Respuesta de COFEPRIS + pago | Notificar al cliente |
| 7 | Generación de Denuncia (DIDGI) | Generar Denuncia, Enviar requisición |
| 8 | Espera final | — |
| 9 | CTA de cierre | — |

Las **interacciones** son el diferenciador: haces clic en botones del panel del gestor y ves el efecto reflejado en el móvil del cliente con animaciones.

## Cómo navegar

- Usa los botones **Atrás** / **Siguiente** en la barra inferior
- O haz clic en los círculos del stepper en el header
- Lee el texto narrativo en la barra inferior — explica cada paso como un pitch
- En pantallas con interacciones, haz clic en los botones verdes resaltados

## Stack

- HTML5 + CSS3 + JavaScript vanilla (archivo único)
- [Tailwind CSS](https://tailwindcss.com/) via CDN
- [Google Fonts](https://fonts.google.com/) — Inter (body) + Oswald (headings)
- Sin backend, sin APIs, sin frameworks — todo es fake/mocked

## Estructura del repo

```
prototype.html                          # El prototipo (todo en un archivo)
docs/superpowers/specs/                 # Spec de diseño
docs/superpowers/plans/                 # Plan de implementación
```

## Notas

- Optimizado para desktop (1280px+)
- Todos los datos son ficticios — ningún dato real de clientes
- Los pagos con Stripe son visuales/mock, no procesan nada
- Los documentos legales (Solicitud, DIDGI, respuesta COFEPRIS) son previews estilizados
