Hotel-Price-Comparator-n8n

Proyecto de automatización construido con n8n que permite buscar, comparar precios de hoteles y devolver la opción más económica según destino, fechas y criterios definidos.

El objetivo es funcionar como un metabuscador automatizado, integrando múltiples fuentes de datos mediante APIs y normalizando la información para tomar la mejor decisión por precio.

🚀 Funcionalidad principal

Recibe parámetros de búsqueda:

Destino

Fecha de check-in / check-out

Cantidad de huéspedes

Cantidad de habitaciones

(Opcional) categoría de estrellas

El flujo:

Consulta múltiples proveedores de hoteles

Normaliza precios y condiciones

Compara resultados

Devuelve:

🥇 Hotel más barato

💲 Precio total de la estadía

🏷️ Proveedor

🔗 Link de reserva

(Opcional) Guarda historial de búsquedas

🧱 Arquitectura del flujo (n8n)

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

🛠️ Tecnologías utilizadas

n8n – Orquestación y automatización

HTTP Request – Consumo de APIs

JavaScript (Function Node) – Normalización y lógica de comparación

APIs de hoteles (ej: Amadeus / RapidAPI / SerpApi / etc.)

Docker (opcional, recomendado para producción)

🔌 Fuentes de datos (proveedores)

Definir acá tus fuentes reales para que el repo quede “cerrado”.

Ejemplo:

Proveedor 1: Amadeus Hotel Search / Hotel Offers API

Proveedor 2: SerpApi / SearchAPI (Google Hotels)

(Opcional) Proveedor 3: Booking / Expedia Rapid (si tenés credenciales partner)

📥 Entrada esperada (Webhook)

Ejemplo de payload JSON:

{
  "city": "Iguazú",
  "check_in": "2026-03-13",
  "check_out": "2026-03-17",
  "adults": 2,
  "rooms": 1,
  "stars": 4
}

📤 Salida del flujo

Ejemplo de respuesta:

{
  "hotel": "Hotel O2 Iguazú",
  "price_total": 356000,
  "currency": "ARS",
  "provider": "Booking API",
  "rating": 8.6,
  "link": "https://..."
}

🔍 Lógica de comparación

Normalización a:

Precio total de la estadía

Misma moneda (si aplica conversión)

Comparación considerando (según disponibilidad de datos):

Categoría (estrellas)

Ubicación

Condiciones similares (ej: cancelación, desayuno, impuestos)

Selección:

Orden por price_total ascendente

Se devuelve el mínimo (y opcionalmente top N)

⚙️ Configuración

Variables de entorno (ejemplo):

HOTEL_API_KEY=your_api_key_here
HOTEL_API_HOST=api_provider_host


En n8n:

Settings → Environment Variables
o .env si se usa Docker.

▶️ Cómo ejecutar el proyecto
Opción 1: n8n local

Importar el workflow (.json)

Configurar credenciales / headers en los nodos HTTP

Activar el workflow

Consumir el webhook

Opción 2: Docker (recomendado)
docker run -it --rm \
  -p 5678:5678 \
  -e N8N_BASIC_AUTH_ACTIVE=true \
  -e N8N_BASIC_AUTH_USER=user \
  -e N8N_BASIC_AUTH_PASSWORD=password \
  n8nio/n8n

📈 Próximas mejoras

Top 5 hoteles más baratos

Filtros avanzados (desayuno, cancelación gratuita)

Comparación histórica de precios

Notificaciones por WhatsApp / Email

Dashboard de consultas

Integración con frontend (React / Next.js)

👩‍💻 Autora

Yanina Spina
Data Scientist | Data Engineer | Automation & n8n
ETL · APIs · Cloud · Workflows
