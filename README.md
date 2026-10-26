# 🤖 Asistente Virtual para WhatsApp - Chatbot de Atención al Cliente

Este es un prototipo funcional de un bot de atención al cliente 24/7. El sistema automatiza la atención en WhatsApp, respondiendo consultas frecuentes, verificando el inventario en tiempo real y utilizando Inteligencia Artificial para mantener conversaciones naturales y fluidas.

## ✨ Características principales

- Atención de Preguntas Frecuentes: Responde automáticamente sobre horarios, ubicación, teléfono, envíos y formas de pago.
- Consulta de Inventario en Tiempo Real: Se conecta a Google Sheets para buscar productos y responder con el precio, stock disponible y ubicación exacta.
- Inteligencia Artificial (IA): Utiliza la API de Google Gemini para entender lenguaje natural y responder preguntas abiertas de forma conversacional.
- Disponibilidad 24/7: Funciona todos los días sin intervención humana.
- Filtro Anti-bucles: El código incluye lógica para ignorar los mensajes enviados por el propio bot, evitando respuestas infinitas.

## 🛠️ Tecnologías Utilizadas

- n8n: Para la orquestación del flujo de trabajo y la lógica de automatización.
- Evolution API: Para la conexión, recepción y envío de mensajes a través de WhatsApp.
- Supabase (PostgreSQL): Base de datos en la nube para guardar el estado del servidor de n8n.
- Google Sheets: Utilizado como base de datos ligera para el inventario de productos.
- Google Gemini API: Modelo de lenguaje (LLM) para la generación de respuestas inteligentes.
- Docker: Para el despliegue y contenedorización de los servicios (en el entorno de producción).

## ⚙️ ¿Cómo funciona el flujo?

1. Un cliente envía un mensaje de WhatsApp al número del negocio.
2. Evolution API lo recibe y lo envía a n8n mediante un Webhook.
3. El nodo "Code" procesa el texto (lo convierte a minúsculas, quita tildes y verifica que no sea un mensaje enviado por el bot).
4. Un nodo "Switch" analiza el texto y enruta el flujo:
    - Si es una palabra clave (ej. "horario", "ubicación"), responde con la información fija.
    - Si parece un producto, consulta Google Sheets para devolver precio y stock.
    - Si es una pregunta abierta, envía el texto a Gemini AI para generar una respuesta.
5. La respuesta final se envía de vuelta al cliente por WhatsApp.

## 🚀 Instalación y Despliegue

El flujo de n8n está exportado y disponible en este repositorio en el archivo `bot-whatsapp.json`. Para replicarlo en tu propio entorno:

1. Descarga o clona este repositorio.
2. En tu instancia de n8n, ve al menú principal y selecciona "Import from File".
3. Sube el archivo `bot-whatsapp.json`.
4. Configura tus credenciales:
    - Añade tu propia credencial de Evolution API (necesitarás la URL de tu servidor y la API Key).
    - Añade tu propia credencial de Google Sheets (OAuth2).
5. Reemplaza los marcadores de posición (Placeholders):
    - En el nodo HTTP Request, reemplaza `TU_API_KEY_DE_GEMINI_AQUI` con tu propia clave de API de Google AI Studio.
    - En el nodo Google Sheets, reemplaza `TU_ID_DE_GOOGLE_SHEETS_AQUI` con el ID de tu propia hoja de cálculo.
6. Activa el flujo y copia la URL de Producción del Webhook.
7. Pega esa URL en el panel de configuración de Evolution API (sección Webhooks, evento `MESSAGES_UPSERT`).

## ⚠️ Nota de Seguridad

Este repositorio ha sido sanitizado para su publicación pública. Ninguna API Key, contraseña de base de datos o credencial privada está incluida en el código. Si deseas utilizar este proyecto, deberás proporcionar tus propias claves de acceso.

---
Desarrollado como prototipo de automatización de procesos y atención al cliente.
