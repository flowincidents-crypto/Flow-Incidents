# Flow-Incidents
Demo for Flow Incidents

____

Prompt Actualizado: Generación de Prototipo "Flow incidents"
Rol del Desarrollador:
"Busco un desarrollador full-stack senior experto en Next.js 14+ (App Router), TypeScript, Tailwind CSS, y la integración de las APIs de OpenAI y Google Generative AI (Gemini). Necesito crear un prototipo para el hackathon 'Flow incidents'."


1. OBJETIVO Y NAVEGACIÓN
Crear una aplicación web con dos vistas principales protegidas por un login (@company.com):

Vista Generador (/dashboard): Transforma logs técnicos desordenados en artículos estandarizados de Knowledge Base (KB) siguiendo el estándar KCS, permitiendo elegir entre el motor de OpenAI (gpt-4o) y Google (Gemini 1.5 Pro) mediante un switch dinámico. Estos logs se registran en un archivo json para el control de incidencias 

Vista Historial (/history): Un panel de control de incidencias basado en un archivo JSON local, con estética de reporte corporativo.

Crear una SPA (Single Page Application) protegida por una pantalla de login 


2. ESPECIFICACIONES TÉCNICAS, ESTILO Y THEME SWITCHING

Framework: Next.js 14+ con TypeScript.
Implementar un Dual-Switch System en el Header:
Toggle IA: OpenAI (Emerald-500) vs. Gemini (Blue-500).
Toggle Tema: * Dark Mode: Estética "Modern Enterprise" (Slate-950, Slate-900, bordes Slate-800).
Light Mode (Office 365 style): Fondo blanco (Slate-50), Header en azul corporativo (#0078d4), tarjetas blancas con bordes sutiles y tipografía clara.

Iconos: Lucide React.
Autenticación: Pantalla /login con validación de dominio @company.com y credenciales hardcoded para la demo. Ruta /dashboard protegida por middleware.

3. PERSISTENCIA (JSON DB)
Usa el módulo fs de Node.js para gestionar la base de datos en un archivo data/incidents.json.

Cada vez que se genere un artículo, se debe guardar un objeto con: idUnique, título, fecha, creador, estado (por defecto "Cerrado"), prioridad (detectada por la IA) y el contenido_markdown.


4. ESTRUCTURA DE LA INTERFAZ

Vista de Historial (/history):
Cards de Métricas: Al igual que la imagen de referencia, mostrar 4 indicadores rápidos: Total Incidencias, Sin Empezar, En Proceso, y Cerrados.

Tabla de Control: Implementar una tabla estilizada con los siguientes campos:

Nº de Incidencia, Título/Problema (resumen del log), Prioridad (Badge de color: Alta/Roja, Media/Amarilla, Baja/Azul), Fecha de apertura, Creador, y Estado (Badge con fondo según estado).

Acciones: Botón para ver el detalle del Markdown generado.

Vista de Generación (/dashboard):
Panel de entrada (Logs) y panel de salida (Markdown viewer con Glassmorphism).
Botón "Guardar en Historial" que dispare la escritura en el JSON.


4. LÓGICA DE IA (Prompt Engineering & System Role)
Configura el endpoint /api/generate para que ambos modelos reciban el siguiente System Prompt estricto:

Rol: Actúa como un Ingeniero de Documentación Técnica Senior.
Reglas de Fidelidad:
No inventes información: Si la causa raíz o un paso no está en el texto original, marca el campo como '[Pendiente de confirmar]'.

Integridad de Comandos: Copia comandos de terminal (grep, systemctl, rm, etc.) exactamente como aparecen. No corrijas sintaxis técnica.

Seguridad: Anonimiza automáticamente IPs, contraseñas y tokens sustituyéndolos por [REDACTED_SECRET].

Tono: Profesional, instructivo y directo.

Estructura del Artículo (Markdown):

[Título Descriptivo]
🔍 Síntomas
🧠 Análisis de Causa Raíz
🛠️ Pasos para la Resolución
✅ Validación
Ejemplo de referencia (Few-Shot):
Input: 'El servidor web tiraba 502. Entré, vi que el disco estaba full. Borré archivos en /tmp y reinicié nginx. Funcionó.'
Output: > # Recuperación de Error 502 en Servidor Web

🔍 Síntomas: El sitio web devuelve un error de puerta de enlace incorrecta (502).
🧠 Análisis de Causa Raíz: Almacenamiento local lleno en el servidor, impidiendo la escritura de logs y el funcionamiento del servicio.
🛠️ Pasos para la Resolución:
Limpieza de archivos temporales: rm -rf /tmp/*.

Reinicio del servicio: systemctl restart nginx.

Tareas: Anonimizar secretos ([REDACTED_SECRET]), no inventar información ([Pendiente de confirmar]), y mantener comandos intactos.

Output JSON Adicional: La API debe devolver el Markdown y un objeto de metadatos (Sugerencia de Prioridad e Impacto) para alimentar la tabla de historial.

6. TAREAS TÉCNICAS
API Routes: Crear /api/generate (IA) y /api/incidents (GET/POST para leer y escribir el archivo JSON).

Middleware: Bloquear acceso si no hay sesión (simulada con cookies o JWT básico).

UI: Usar framer-motion para las transiciones de color entre el modo Dark y Office 365.

Markdown: react-markdown para el visor derecho.

Proporciona el código modularizado, incluyendo el componente de la tabla de historial y la lógica de manejo de archivos fs.

✅ Validación: Verificar disponibilidad del sitio y espacio libre con df -h.

5. TAREAS DE IMPLEMENTACIÓN
Layout & Middleware: Configurar la protección de rutas y el tema oscuro profundo.

API Handler: Crear una ruta que gestione ambas bibliotecas (openai y @google/generative-ai) basándose en el parámetro provider.

UI Effects: Implementar transiciones suaves de color de acento cuando el usuario cambie el switch de IA (de verde a azul).

Markdown Rendering: Usar react-markdown para mostrar la salida de forma elegante.

Proporciona el código completo, modularizado y funcional, listo para conectar las variables de entorno OPENAI_API_KEY y GEMINI_API_KEY.
