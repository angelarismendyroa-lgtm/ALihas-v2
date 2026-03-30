# MODELOS DE REFERENCIA v2 — ALihas-v2 (NemoClaw Architecture)

**Versión:** 2.0
**Fecha:** 2026-03-30
**Propósito:** Referencia central para la selección de modelos y proveedores en ALihas v2.

---

## NOTA OPERATIVA CRÍTICA
- **Motor Exclusivo ALihas v2:** NVIDIA NIM (modelos gratuitos) para TODA la operatividad de OpenClaw.
- **Motor de Seguridad:** NVIDIA NemoClaw (nvidia/llama-3.1-nemotron-safety-guard-8b-v3)
- **Sub-agentes de negocio:** OpenRouter API

---

## 1. Arquitectura de Motores v2

### Motor Operativo (OpenClaw Dashboard)
| Función | Modelo | Proveedor | Notas |
|---------|--------|-----------|-------|
| Motor principal | moonshotai/kimi-k2-thinking | NVIDIA NIM (gratuito) | Multimodal, agéntico nativo |
| Alternativa | Modelos NVIDIA Nemotron | NVIDIA NIM | Enterprise-optimized |

### Motor de Seguridad (NemoClaw)
| Función | Modelo | Proveedor |
|---------|--------|-----------|
| Safety Guard | nvidia/llama-3.1-nemotron-safety-guard-8b-v3 | NVIDIA NIM (gratuito) |

### Telegram (Nivel 0 — Interfaz Multimodal)
| Función | Modelo | Proveedor |
|---------|--------|-----------|
| Asistente multimodal | Motor NVIDIA NIM (multimodal) | NVIDIA NIM |
| Fallback multimodal | Gemini API / OpenRouter | Google / OpenRouter |

### Sub-agentes de Negocio (Expansión)
| Agente | Proveedor | Uso |
|--------|-----------|-----|
| Auditor | OpenRouter API | Análisis de seguridad y compliance |
| Marketing | OpenRouter API | Creación de contenido y estrategia |
| Otros | OpenRouter API | Bajo demanda |

---

## 2. Configuración de Proveedores

### Prioridad 1: NVIDIA NIM (Motor Core)
- **Base URL:** https://integrate.api.nvidia.com/v1
- **Variable:** NVIDIA_API_KEY
- **Uso:** Motor principal, seguridad, operatividad

### Prioridad 2: OpenRouter (Sub-agentes)
- **Base URL:** https://openrouter.ai/api/v1
- **Variable:** OPENROUTER_API_KEY
- **Uso:** Sub-agentes especializados del negocio

### Prioridad 3: Gemini API (Backup multimodal)
- **Base URL:** https://generativelanguage.googleapis.com/v1beta/models/
- **Variable:** GEMINI_API_KEY
- **Uso:** Backup para multimodal en Telegram

---

## 3. Roadmap de Modelos

### Fase Actual (v2.0)
- NVIDIA NIM como motor exclusivo
- Safety Guard NemoClaw activo
- OpenRouter para sub-agentes de negocio

### Fase Futura (v2.1+)
- **Agente Watchdog Local:** Modelo descargable de aprox. 5B parámetros
  - Entrenado para seguridad lógica y análisis de comunicación agéntica
  - Complementa las debilidades lógicas de NemoClaw (seguridad técnica)
  - Actúa como backup si NemoClaw falla
  - Modelo base: Llama o Qwen (aprox. 5B params)
  - Despliegue: Local en el servidor

---

## 4. Tabla de Proveedores (Referencia completa)
| Proveedor | Variable de Entorno | Base URL |
| :--- | :--- | :--- |
| NVIDIA | NVIDIA_API_KEY | https://integrate.api.nvidia.com/v1 |
| OpenRouter | OPENROUTER_API_KEY | https://openrouter.ai/api/v1 |
| Google (Gemini) | GEMINI_API_KEY | https://generativelanguage.googleapis.com/v1beta/models/ |
| Groq | GROQ_API_KEY | https://api.groq.com/openai/v1 |
| DeepSeek | DEEPSEEK_API_KEY | https://api.deepseek.com/v1 |
| Moonshot | MOONSHOT_API_KEY | https://api.moonshot.ai/v1 |
| MiniMax | MINIMAX_API_KEY | https://api.minimax.chat/v1 |

---

## 5. Reglas del Router
- **Default:** Motor NVIDIA NIM (siempre)
- **Seguridad:** NemoClaw Safety Guard (siempre activo)
- **Negocio/Expansión:** OpenRouter para sub-agentes
- **Multimodal Telegram:** NVIDIA si soporta, fallback Gemini/OpenRouter
- **Notificación:** Siempre notificar al cambiar de modelo

---
*ALihas-v2 — Modelos de Referencia v2.0*
*Estrategia NVIDIA NIM Exclusive*
