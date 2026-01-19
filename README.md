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
```

**Cada archivo tiene una sola responsabilidad clara.**

🟦 1. Transcripción de audio
----------------------------

### transcribe\_audio.py

**Responsabilidad:** Convertir audio continuo en una lista ordenada de palabras con timestamps.**Entrada:** Archivo de audio (radio, TV, podcast)**Salida (Ejemplo):**

JSON
**Salida**
```json
[
  { "index": 0, "word": "buenos", "start": 0.52, "end": 0.71 },
  { "index": 1, "word": "días", "start": 0.72, "end": 0.93 }
]
```

**Reglas clave:**

*   Timestamp por palabra.
    
*   Output inmutable.
    
*   WhisperX es la verdad absoluta temporal.
    

🟨 2. Chunking por palabras (NO audio)
--------------------------------------

### chunk\_words.py

**Responsabilidad:** Dividir la transcripción en grupos secuenciales de palabras para el análisis narrativo.**Qué es un chunk:** Un rango de índices de palabras, no segundos ni audio.

**Ejemplo:** Chunk 1 -> palabras 0-499 | Chunk 2 -> palabras 500-999**Reglas:** Sin solape, sin timestamps, solo índice + palabra.

🟧 3. Análisis narrativo con LLM
--------------------------------

### analyze\_narrative\_llm.py

**Rol del LLM:** Identificar dónde empieza y termina cada noticia a nivel narrativo y generar un resumen inicial.**Salida del LLM (Ejemplo):**

JSON
```
[    {      "start_word": 320,      "end_word": 498,      "summary": "Nasri criticó al CNE por el proceso electoral."    }  ]   `
```
**Restricciones:** El LLM entiende historias, no tiempo. No produce timestamps ni decide duración.

⚙️ 4. Decisiones automáticas del sistema
----------------------------------------

### apply\_rules.py

**Responsabilidad:** Convertir propuestas del LLM en decisiones finales mediante lógica de negocio.**Reglas:** Duración mínima/máxima, continuidad entre chunks, marcadores explícitos.**El LLM propone. El sistema decide.**

🟦 5. Mapeo de palabras a tiempo real
-------------------------------------

### map\_words\_to\_time.py

**Responsabilidad:** Traducir índices de palabras a segundos reales usando la data inmutable de WhisperX.**Proceso:** start\_time = words\[start\_word\].start | end\_time = words\[end\_word\].end**Nota:** Paso determinista, reproducible y auditable.

🟥 6. Clipping de audio
-----------------------

### clip\_audio.py

**Responsabilidad:** Cortar audio exactamente donde el sistema lo indica usando FFmpeg. No analiza texto, solo ejecuta cortes técnicos.

🟪 7. Resumen final y corrección editorial
------------------------------------------

### summarize\_news.py / correct\_entities.py

**Responsabilidad:** Entregar el resumen final para consumo humano usando dictionary.json.**Reglas estrictas:**

*   ✅ Corregir errores ortográficos de nombres propios.
    
*   ✅ Normalizar variantes conocidas.
    
*   ❌ No expande nombres ni introduce información nueva.
    

🧠 Manejo de múltiples periodistas
----------------------------------

*   Se aceptan interrupciones y solapamientos.
    
*   El speaker NO define cortes. El tema manda.
    
*   La historia es más importante que la pureza del audio.
    

📤 Output final
---------------

Por cada noticia detectada: Clip exacto de audio, Resumen corregido, Inicio y fin reales (segundos) y Texto legible.

🧪 Estado del proyecto
----------------------

Este repositorio define un MVP técnico sólido: escalable, auditable, determinista y mantenible.

> **"El LLM entiende historias. WhisperX entiende tiempo. El sistema conecta ambos."**



