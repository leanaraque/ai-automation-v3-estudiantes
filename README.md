# AI Automation · Módulos 5 a 8

Material de las Clases 5 a 8 del curso AI Automation de Coderhouse: Google, AI Agent, IA local con Ollama y WhatsApp.

Todo gira alrededor de un solo caso, la **Clínica Dental del Parque**: un consultorio que recibe 60 mensajes de WhatsApp por día y quiere un asistente que conteste, anote turnos y avise. Vos aplicás el mismo método a tu propio proceso en las preentregas y el proyecto final.

## Antes de empezar

Seguí [antes-de-la-clase-5.md](antes-de-la-clase-5.md): cuenta de Google personal, clave de Groq y cuenta de n8n Cloud.

## Clases

| Clase | Fecha | Tema | Carpeta |
|---|---|---|---|
| 5 | lunes 26/10 | Planificar y conectar Google | [clase-05](clase-05/) |
| 6 | miércoles 28/10 | AI Agent | (próximamente) |
| 7 | lunes 2/11 | IA local con Ollama | (próximamente) |
| 8 | miércoles 4/11 | WhatsApp Business | (próximamente) |

## Cómo usar cada carpeta de clase

- `README.md`: qué vimos, cómo probarlo y la tarea.
- `inicio/`: el workflow con el que arranca la clase.
- `pasos/`: cada paso de la clase como workflow listo para importar. Si te perdiste en clase, importá el paso donde te quedaste y seguí.
- `final/`: el workflow terminado.

**Cómo importar un workflow en n8n:** abrí un workflow nuevo, menú de tres puntos (arriba, a la derecha del nombre) > **Import** > **From URL**, y pegá la dirección "raw" del archivo. Cada README de clase trae las direcciones listas para copiar.

## El sistema de fichas de la clínica

La carpeta `api/pacientes/` simula el sistema de fichas de la clínica: una API de solo lectura que devuelve la ficha de un paciente por su ID.

```text
https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/api/pacientes/101.json
```

Existen los ID 101 a 105. El 106 no existe a propósito: devuelve un error 404. Todos los datos son ficticios.

## Problemas frecuentes

| Síntoma | Solución |
|---|---|
| **Import > From URL** da error | Usá la dirección que empieza con `https://raw.githubusercontent.com/`, no la de la página de GitHub |
| Groq responde 401 | En la credencial, el valor tiene que ser `Bearer ` (con un espacio) seguido de tu clave |
| Groq responde 429 | Hiciste muchos pedidos seguidos (límite gratuito: 30 por minuto). Esperá un minuto |
| En n8n no aparece tu planilla de Google | La planilla está en otra cuenta de Google. Usá la misma cuenta para todo |
