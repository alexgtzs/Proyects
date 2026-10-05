# Travel Agent — Provider & Tool Strategy

La skill es independiente del proveedor. El agente debe usar las mejores herramientas disponibles en el entorno y degradar de forma segura cuando alguna no exista.

## Matriz de capacidades

| Capacidad | Preferencia | Fallback |
|---|---|---|
| Vuelos | Expedia / Trip.com / proveedor de vuelos con precio actual | búsqueda web reciente + sitio oficial de aerolínea |
| Hoteles | Booking.com / Expedia / Trip.com | búsqueda web reciente + sitio oficial del hotel |
| Actividades | GetYourGuide / Trip.com / proveedor de actividades | web + sitio oficial de la atracción |
| Clima | herramienta meteorológica actual | fuente meteorológica oficial/confiable |
| Distancias y logística local | mapas/búsqueda local disponible | web + estimación claramente marcada |
| Compromisos del viajero | Google Calendar | preguntar solo si el conflicto realmente bloquea el plan |
| Confirmaciones de reservas | Gmail, cuando el usuario lo autorice | datos proporcionados por el usuario |
| Investigación general | búsqueda web | fuentes oficiales primero |

## Reglas de selección de herramientas

1. Si existe una herramienta que devuelve disponibilidad o precio actual, úsala antes de una búsqueda genérica.
2. Nunca combines precios de fechas distintas como si fueran comparables.
3. Conserva moneda original y registra cualquier conversión aplicada.
4. Para vuelos, distingue tarifa base de costo efectivo.
5. Para hoteles, compara costo total de la estancia, no precio por noche aislado.
6. Para actividades, verifica que el horario sea compatible con el itinerario.
7. Si dos proveedores muestran resultados diferentes, no ocultes la discrepancia.
8. Nunca digas `reservado`, `comprado` o `confirmado` hasta recibir confirmación de la acción correspondiente.

## Adaptador conceptual de vuelos

Normaliza cada opción a:

```json
{
  "provider": "expedia",
  "observed_at": "ISO-8601",
  "origin_airport": "NLU",
  "destination_airport": "CUN",
  "departure": "ISO-8601",
  "arrival": "ISO-8601",
  "stops": 0,
  "duration_minutes": 0,
  "fare": {"amount": 0, "currency": "MXN"},
  "baggage_cost": {"amount": 0, "currency": "MXN", "certainty": "NEEDS_CONFIRMATION"},
  "airport_ground_cost": {"amount": 0, "currency": "MXN", "certainty": "ESTIMATED"},
  "effective_total": {"amount": 0, "currency": "MXN"},
  "booking_url_or_reference": null
}
```

## Adaptador conceptual de hotel

```json
{
  "provider": "booking.com",
  "observed_at": "ISO-8601",
  "name": "Hotel",
  "area": "Zona",
  "check_in": "YYYY-MM-DD",
  "check_out": "YYYY-MM-DD",
  "rooms": 1,
  "total_stay": {"amount": 0, "currency": "MXN"},
  "taxes_included": null,
  "rating": null,
  "review_count": null,
  "cancellation": "unknown",
  "meal_plan": "unknown",
  "certainty": "REFERENCE_CURRENT"
}
```

## Calendario

Si Google Calendar está disponible y el usuario pide ayuda para elegir fechas, el agente puede revisar conflictos antes de buscar opciones. No debe crear eventos sin autorización explícita.

Después de que el viaje sea reservado, puede ofrecer crear eventos para:

- salida hacia aeropuerto;
- vuelos;
- check-in/check-out;
- actividades con hora fija;
- recordatorios de documentación.

## Email

Si Gmail está disponible y el usuario lo solicita, el agente puede buscar confirmaciones de vuelo/hotel para reconciliar el itinerario con reservas reales. No debe inferir una reserva a partir de correos promocionales.

## Monitoreo posterior

Cuando el entorno soporte tareas programadas, un viaje con reservas pendientes puede beneficiarse de monitores específicos, por ejemplo:

- caída de precio de un vuelo concreto;
- apertura de disponibilidad de un hotel concreto;
- cambio importante en un vuelo ya reservado;
- apertura de reservas para una actividad.

El monitor debe ser específico al viaje; no crear vigilancia genérica sin necesidad.
