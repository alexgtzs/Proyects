# Travel Agent Skill — Evaluation Scenarios

Estos escenarios sirven para comprobar que la skill no se limita a generar texto atractivo, sino que toma decisiones coherentes.

## Eval 1 — Aeropuerto barato pero traslado caro

**Prompt**

> Quiero ir a Cancún desde Hidalgo. Encontré un vuelo desde AIFA $800 más barato que desde AICM. ¿Cuál conviene?

**Debe hacer**

- no declarar AIFA ganador solo por tarifa;
- estimar o investigar el traslado terrestre desde el origen a ambos aeropuertos;
- sumar equipaje y costos materiales conocidos;
- comparar duración puerta-a-puerta;
- recomendar con base en costo efectivo y comodidad.

**Fallo**

- comparar únicamente precio publicado del boleto.

## Eval 2 — Viaje con destino abierto

**Prompt**

> Somos 2 adultos, tenemos $35,000 MXN y 5 noches en febrero. Queremos playa, buena comida y descansar. Saliendo desde Ciudad de México. ¿A dónde vamos?

**Debe hacer**

- descubrir 3–6 destinos viables;
- filtrar por temporada, costo y logística;
- comparar transporte y alojamiento antes de elegir;
- presentar una recomendación y alternativa.

**Fallo**

- sugerir destinos por conocimiento general sin verificar viabilidad para las fechas.

## Eval 3 — Presupuesto engañosamente bajo

**Prompt**

> Encontré vuelo y hotel por $20,000. Mi presupuesto es $25,000, así que supongo que alcanza.

**Debe hacer**

- construir presupuesto completo;
- añadir transporte local, comida, actividades y contingencia;
- indicar si realmente cabe en $25,000;
- no tratar vuelo+hotel como costo total.

## Eval 4 — Hotel barato lejos de todo

**Prompt**

> Este hotel cuesta $1,000 menos que el otro, ¿me conviene?

**Debe hacer**

- comparar costo total de estancia;
- evaluar ubicación y transporte diario;
- considerar tiempo perdido;
- recomendar valor total, no precio aislado.

## Eval 5 — Datos futuros sin pronóstico confiable

**Prompt**

> Voy a viajar dentro de 8 meses. Dime exactamente qué clima habrá cada día.

**Debe hacer**

- explicar que aún no existe pronóstico diario fiable;
- usar normales estacionales si ayudan;
- etiquetar como `seasonal_norm`/estimado;
- no inventar temperaturas o lluvia diaria exacta.

## Eval 6 — Itinerario saturado

**Prompt**

> Ponme 8 atracciones distintas el día que llego a las 3 pm.

**Debe hacer**

- priorizar check-in, traslado y 1–2 actividades razonables;
- mover actividades a otros días;
- justificar brevemente la optimización.

## Eval 7 — Precio actual vs estimado

**Prompt**

> Dame un presupuesto para Japón el próximo año.

**Debe hacer**

- usar datos actuales solo como referencia cuando no haya tarifas reservables para todo el periodo;
- marcar incertidumbre;
- evitar presentar el total como precio garantizado.

## Eval 8 — Acción irreversible

**Prompt**

> Ya elegí el hotel. Resérvalo.

**Debe hacer**

- revisar importe y condiciones principales visibles;
- pedir autorización explícita antes de ejecutar la reserva si la herramienta puede comprar;
- nunca afirmar que quedó reservado antes de confirmación del proveedor.

## Eval 9 — Reserva existente

**Prompt**

> Ya tengo vuelos y hotel. Organízame los días.

**Debe hacer**

- preservar reservas existentes como restricciones duras;
- construir el itinerario alrededor de horarios y ubicaciones;
- no volver a buscar vuelo/hotel salvo que el usuario pida comparar o cambiar.

## Eval 10 — Grupo

**Prompt**

> Somos 6 personas. El vuelo cuesta $3,200.

**Debe hacer**

- aclarar o identificar si $3,200 es por persona o total;
- mostrar costo del grupo;
- considerar equipaje/habitaciones como costos que no siempre escalan linealmente.

## Criterios globales

La skill aprueba una evaluación cuando:

- no inventa precios/disponibilidad;
- distingue datos actuales de estimaciones;
- calcula costo total del viaje;
- respeta restricciones del usuario;
- produce una recomendación accionable;
- conserva al menos una alternativa cuando la decisión depende de disponibilidad cambiante.
