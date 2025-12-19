# Hotel-Price-Comparator-n8n

Proyecto de automatización construido con **n8n** que permite **buscar, comparar precios de hoteles** y devolver la **opción más económica** según destino, fechas y criterios definidos.

Funciona como un **metabuscador automatizado**, integrando múltiples fuentes mediante **APIs** y **normalizando** la información para comparar en igualdad de condiciones.

---

## 🚀 Funcionalidad principal

Recibe parámetros de búsqueda:
- Destino (city)
- Fecha de check-in / check-out
- Cantidad de huéspedes (adults)
- Cantidad de habitaciones (rooms)
- (Opcional) categoría de estrellas (stars)

El flujo:
- Consulta múltiples proveedores
- Normaliza precios y condiciones
- Compara resultados

Devuelve:
- 🥇 Hotel más barato
- 💲 Precio total de la estadía
- 🏷️ Proveedor
- 🔗 Link de reserva
- (Opcional) historial de búsquedas

---

## 🧱 Arquitectura del flujo (n8n)

Webhook  
↓  
Set / Function (validación de parámetros)  
↓  
HTTP Request – Proveedor 1  
↓  
HTTP Request – Proveedor 2  
↓  
Merge  
↓  
Function (normalización + comparación)  
↓  
Respond to Webhook  

---

## 🔌 Fuentes de datos (proveedores)

Definilas en `docs/sources.md`.

Ejemplo:
- Proveedor 1: **Amadeus Hotel Search / Hotel Offers API**
- Proveedor 2: **SerpApi / SearchAPI (Google Hotels)**
- (Opcional) Proveedor 3: **Booking / Expedia Rapid** (si contás con credenciales partner)

---

## 🛠️ Tecnologías utilizadas

- **n8n** – Orquestación y automatización
- **HTTP Request** – Consumo de APIs
- **JavaScript (Function Node)** – Normalización y lógica de comparación
- **Docker** (opcional, recomendado para producción)

---

## 📥 Entrada esperada (Webhook)

Ejemplo de payload JSON:
```json
{
  "city": "Iguazú",
  "check_in": "2026-03-13",
  "check_out": "2026-03-17",
  "adults": 2,
  "rooms": 1,
  "stars": 4,
  "currency": "ARS"
}
