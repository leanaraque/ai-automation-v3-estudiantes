# Clase 5 · Planificar y conectar Google

Lunes 26/10 · Módulo 5 · Creando workflows con aplicaciones y servicios externos

## Qué vimos

1. **Punto de partida**: lo que viste en los Módulos 1 a 4 (prompt, workflow JSON, nodos, HTTP Request con credencial), aplicado a la clínica. Seis mensajes de WhatsApp clasificados con Groq.
2. **Planificar antes de construir**: siete preguntas antes de arrastrar el primer nodo (problema, proceso, accesos, errores, idempotencia, avisos, mantenimiento).
3. **El flujo de fichas**: la planilla de pacientes tiene los ID, pero no el nombre, la edad ni la obra social. El flujo los pide al sistema de fichas, completa la planilla y le avisa a la Dra. Benítez con un solo correo.

```text
Al hacer clic en Probar (Manual Trigger)
  → Leer pacientes (Google Sheets)
  → Solo fichas incompletas (Filter)
  → Pedir la ficha al sistema (HTTP Request)
      Success → Completar la ficha (Google Sheets)
                → Juntar las fichas completadas (Aggregate)
                → Avisar a la Dra. Benítez (Gmail)
      Error   → el ID 106, que no existe en el sistema
```

## La planilla

Creá una planilla nueva en [sheets.new](https://sheets.new), llamala `Clínica Dental del Parque` y llamá `Pacientes` a la hoja. Escribí:

| | A | B | C | D |
|---|---|---|---|---|
| 1 | ID | Nombre | Edad | Obra social |
| 2 | 101 | | | |
| 3 | 102 | | | |
| 4 | 103 | | | |
| 5 | 104 | | | |
| 6 | 105 | | | |
| 7 | 106 | | | |

Los títulos tienen que quedar exactamente así (mayúsculas incluidas): el flujo busca las columnas por su nombre.

## Importar los workflows

En n8n: workflow nuevo > menú de tres puntos > **Import** > **From URL** y pegá una de estas direcciones.

| Archivo | Para qué | Dirección |
|---|---|---|
| Punto de partida | El repaso de M1 a M4 | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/inicio/punto-de-partida.json` |
| Paso 1 | Leer la planilla | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/pasos/paso-1.json` |
| Paso 2 | Pedir la ficha (y el error 404) | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/pasos/paso-2.json` |
| Paso 3 | Salida de error y completar la planilla | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/pasos/paso-3.json` |
| Paso 4 | Gmail (llegan 5 correos) | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/pasos/paso-4.json` |
| Paso 5 | Aggregate (llega 1 correo) | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/pasos/paso-5.json` |
| Paso 6 | Filter (correr dos veces no repite nada) | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/pasos/paso-6.json` |
| Final | El flujo completo | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/final/fichas-pacientes.json` |
| Desafío extra | Avisar qué ficha no se encontró | `https://raw.githubusercontent.com/leanaraque/ai-automation-v3-estudiantes/main/clase-05/final/desafio-extra.json` |

**Después de importar, completá lo que es tuyo** (no viaja en el archivo):

| Nodo | Qué elegir |
|---|---|
| `Clasificar con Groq` (punto de partida) | **Credential for Header Auth**: tu credencial de Groq (Name `Authorization`, Value `Bearer ` + tu clave) |
| `Leer pacientes` y `Completar la ficha` | Tu credencial de Google Sheets, **Document** `Clínica Dental del Parque` y **Sheet** `Pacientes` |
| `Avisar a la Dra. Benítez` | Tu credencial de Gmail y, en **To**, tu propio correo (el archivo trae `tu-correo@gmail.com`) |

## Cómo probarlo

| Prueba | Resultado esperado |
|---|---|
| Ejecutar el flujo final con la planilla recién creada | Filas 101 a 105 completas, la 106 vacía. Llega 1 correo: `Fichas completadas: 5` |
| Ejecutar otra vez, sin tocar nada | No llega ningún correo: el Filter solo deja pasar el 106, que va por la salida de error |
| Borrar B4:D4 (la fila del 103) y ejecutar | Llega 1 correo: `Fichas completadas: 1` con `- 103: Jorge Acosta` |

## Tarea (20 a 30 minutos)

1. Respondé las siete preguntas de la planificación para **tu propio proceso**, en un documento corto. Va a tu Preentrega 3.
2. En tu n8n: un flujo que lea una planilla tuya y te mande **un solo correo** con un resumen. Podés partir del flujo final.

**Desafío extra (opcional):** conectá la salida **Error** de `Pedir la ficha al sistema` a un segundo Gmail que avise qué ficha no se encontró. Resuelto en `final/desafio-extra.json`.

## Para tu Preentrega 3

La Preentrega 3 pide planificación, un servicio de Google, un AI Agent con su system prompt y una reflexión de riesgos. Hoy viste las dos primeras; el agente llega en la Clase 6. Las cuatro preentregas se entregan juntas cuando se habilita el proyecto final.
