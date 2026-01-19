# mvp-medios  
### Monitoreo de medios con transcripción por palabra, análisis narrativo y clipping exacto de audio

`mvp-medios` es un **MVP técnico de monitoreo de medios** diseñado para procesar audio continuo (radio, TV, podcasts) y:

- Transcribir audio con **timestamp por palabra**
- Detectar **noticias completas** dentro de transmisiones largas
- Cortar audio de forma **exacta, reproducible y auditable**
- Generar **resúmenes confiables**, corregidos editorialmente

El sistema prioriza **coherencia narrativa** sobre pureza acústica y está diseñado para escalar.

---

## 🎯 Objetivo del proyecto

Resolver un problema clásico del monitoreo de medios:

> *¿Cómo identificar noticias reales dentro de audio continuo y cortarlas exactamente donde empiezan y terminan, sin perder contexto?*

Este proyecto responde a eso usando:
- **WhisperX** para tiempo real por palabra  
- **LLM** para análisis narrativo (no para tiempo)  
- **Reglas determinísticas** para decisiones finales  
- **Diccionario controlado** para corrección de nombres  

---

## 🧠 Principios de diseño (no negociables)

Estas reglas gobiernan todo el sistema:

1. **La palabra es la unidad mínima de verdad**
2. **WhisperX es la única fuente de tiempo**
3. **El LLM entiende narrativa, no segundos**
4. **El LLM propone, el sistema decide**
5. **No existe lag artificial**
6. **No existe solape entre chunks**
7. **El audio nunca se corta antes del análisis**
8. **La historia es más importante que la pureza del audio**
9. **El diccionario solo corrige, nunca enriquece**

---

## 🔁 Flujo del sistema

1. **Audio crudo** 
2. **Transcripción** (WhisperX, palabra por palabra)  
3. **Chunks de palabras** (texto, sin timestamps)  
4. **Análisis narrativo con LLM** 
5. **Reglas automáticas del sistema** 
6. **Mapeo palabra → tiempo real** 
7. **Clipping exacto de audio** 
8. **Resumen final corregido** 
---

## 📁 Estructura del proyecto

```text
mvp-medios/
│
├── transcribe_audio.py
├── chunk_words.py
├── analyze_narrative_llm.py
├── apply_rules.py
├── map_words_to_time.py
├── clip_audio.py
├── summarize_news.py
│
├── correct_entities.py
├── dictionary.json
│
├── input_audio/
├── output_clips/
├── temp/
│
└── README.md
