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
📤 Salida del flujo

Eje{
  "hotel": "Hotel O2 Iguazú",
  "price_total": 356000,
  "currency": "ARS",
  "provider": "amadeus",
  "rating": 8.6,
  "link": "https://..."
}
🔍 Lógica de comparación

Normalización a:

Precio total de la estadía

Misma moneda (si aplica conversión)

Comparación considerando (según disponibilidad):

Categoría (stars)

Ubicación

Condiciones similares (cancelación, desayuno, impuestos)

Selección:

Orden por price_total ascendente

Se devuelve el mínimo (y opcionalmente top N)
⚙️ Configuración

Copiá env.example a .env y completá valores.

Nota: en n8n también podés cargar esto desde Settings → Environment Variables o mediante Docker.
▶️ Cómo ejecutar
Opción 1: n8n local (UI)

Levantar n8n

Importar el workflow desde workflows/hotel-price-comparator.json

Configurar credenciales / headers en los nodos HTTP

Activar el workflow

Consumir el webhook

Opción 2: Docker (recomendado)
docker run -it --rm \
  --env-file .env \
  -p 5678:5678 \
  n8nio/n8n
🧪 Probar el webhook (ejemplo)

Reemplazá <WEBHOOK_URL> por la URL real de tu webhook (Test o Production):
curl -X POST "<WEBHOOK_URL>" \
  -H "Content-Type: application/json" \
  -d '{
    "city":"Iguazú",
    "check_in":"2026-03-13",
    "check_out":"2026-03-17",
    "adults":2,
    "rooms":1,
    "stars":4,
    "currency":"ARS"
  }'
📈 Próximas mejoras

Top 5 hoteles más baratos

Filtros avanzados (desayuno, cancelación gratuita)

Historial y tracking de precios

Notificaciones (WhatsApp / Email)

Dashboard de consultas

Integración con frontend (React / Next.js)

👩‍💻 Autora

Yanina Spina
Data Scientist | Data Engineer | Automation & n8n
ETL · APIs · Cloud · Workflows


---

## env.example (listo)

```bash
# -------------------------
# n8n (opcional si usás auth)
# -------------------------
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=your_user
N8N_BASIC_AUTH_PASSWORD=your_password

# URL pública (si estás detrás de proxy / dominio)
# N8N_HOST=n8n.tudominio.com
# N8N_PROTOCOL=https
# N8N_PORT=5678
# WEBHOOK_URL=https://n8n.tudominio.com/

# -------------------------
# Proveedor 1: Amadeus
# -------------------------
AMADEUS_CLIENT_ID=your_amadeus_client_id
AMADEUS_CLIENT_SECRET=your_amadeus_client_secret
# AMADEUS_ENV=test  # test|prod (si lo usás en el workflow)

# -------------------------
# Proveedor 2: SerpApi / SearchAPI (Google Hotels)
# -------------------------
SERPAPI_KEY=your_serpapi_key
# SEARCHAPI_KEY=your_searchapi_key  # si usás SearchAPI en vez de SerpApi

# -------------------------
# Conversión de moneda (opcional)
# -------------------------
FX_API_KEY=your_fx_api_key
FX_BASE=USD
FX_TARGET=ARS

docs/sources.md (plantilla rápida)
# Fuentes de datos

## Proveedor 1: Amadeus
- Tipo: API oficial
- Datos: ofertas, precio total, moneda, hotel, rating (según endpoint)
- Auth: OAuth2 Client Credentials
- Observaciones: normalizar total vs por noche

## Proveedor 2: SerpApi / SearchAPI (Google Hotels)
- Tipo: API de resultados (metabuscador)
- Datos: opciones, links, precios (según respuesta)
- Auth: API key
- Observaciones: puede traer “desde”, validar total

## Reglas de normalización
- Siempre convertir a `price_total`
- Moneda unificada (`currency`)
- Guardar `provider` y `link`

