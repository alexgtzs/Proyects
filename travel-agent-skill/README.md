# Travel Agent Skill

Skill experimental para convertir una petición de viaje en una recomendación ejecutable con comparación de opciones, presupuesto completo, itinerario y control de incertidumbre.

## Objetivo

Evitar el patrón típico de "lista de lugares" y actuar más como un agente de viajes:

- entender restricciones reales;
- investigar precios/disponibilidad actual cuando existan herramientas;
- comparar vuelos, hoteles y actividades por costo total;
- construir un presupuesto de principio a fin;
- diseñar itinerarios razonables;
- señalar qué reservar primero;
- mantener alternativas si cambian los precios.

## Archivos

- `SKILL.md` — comportamiento principal del agente.
- `PROVIDERS.md` — estrategia para Expedia, Booking.com, Trip.com, GetYourGuide, web, Calendar y Gmail.
- `schemas/trip-plan.schema.json` — formato estructurado para persistir un viaje.
- `examples/cancun-family-request.json` — ejemplo de entrada para un viaje de fin de año desde Hidalgo.
- `EVALS.md` — escenarios de prueba para validar que la skill toma decisiones útiles.

## Flujo resumido

```text
Solicitud
   ↓
Intake estructurado
   ↓
¿Destino abierto? ── sí ─→ shortlist de destinos
   ↓ no
Vuelos / transporte
   ↓
Hoteles
   ↓
Actividades + logística
   ↓
Clima / riesgos
   ↓
Presupuesto total
   ↓
Itinerario
   ↓
Recomendación + alternativa + prioridad de reserva
```

## Diferencias respecto a un trip planner básico

Un trip planner básico puede recomendar atracciones. Esta skill intenta resolver decisiones de viaje completas.

Ejemplo: si un vuelo desde AIFA cuesta menos que uno desde AICM, no se elige automáticamente AIFA. Se añade el traslado desde el origen, equipaje, horario y duración para comparar el costo efectivo puerta-a-puerta.

## Estado

`v0.1` — primera especificación funcional.

Todavía por añadir:

- scoring calibrado con viajes reales;
- persistencia de preferencias del viajero;
- reconciliación con reservas existentes;
- generación opcional de roadbook/PDF;
- evaluación automática de consistencia del itinerario;
- conectores adicionales para transporte terrestre.

## Inspiración

El diseño toma como referencia el enfoque de skills de planificación de viajes open source, pero esta implementación está orientada a planificación personal con herramientas actuales y separación estricta entre precios confirmados, precios de referencia y estimaciones.
