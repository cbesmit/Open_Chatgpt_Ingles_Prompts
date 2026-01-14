{
    "schema_version": 1, // integer — versión del esquema
    "version": 1, // integer — aumenta en cada actualización
    "updated_at": "YYYY-MM-DD", // string ISO — fecha de última actualización
    "autor": "usuario|IA", // string
    "perfil": { // estado actual del alumno
        "nivel_actual": null, // "A1"|"A2"|"B1"|"B2"|"C1"|"C2"|null
        "nivel_objetivo": "B2", // objetivo CEFR
        "ultima_decision": null // "diagnostico"|"propuesta_nivel"|"actualizacion_nivel"|"examen"|null
    },
    "diagnostico_inicial": { // resumen consolidado del diagnóstico
        "fecha": null, // "YYYY-MM-DD"|null
        "score": null, // number 0..100 | null
        "resumen": null, // string | null
        "fortalezas": [], // string[]
        "debilidades": [], // string[]
        "recomendaciones": [] // string[]
    },
    "examenes": [ // histórico de exámenes (más reciente al final)
        {
            "id": "ex_0001", // string único
            "fecha": "YYYY-MM-DD", // ISO
            "nivel_objetivo": "B1", // CEFR del examen aplicado
            "secciones": [
                "Grammar",
                "Vocabulary",
                "Reading",
                "Writing",
                "Speaking/Listening"
            ],
            "resumen_preguntas": "...", // breve descripción de temas/ítems
            "scores": { // desglose + total
                "listening": 0,
                "reading": 0,
                "writing": 0,
                "speaking": 0,
                "total": 0
            },
            "tiempo_min": 0, // duración en minutos
            "cefrestimado": null, // CEFR estimado por el examen (opcional)
            "comentarios": "...", // errores/ aciertos clave
            "recomendaciones": "..." // próximos focos de estudio
        }
    ],
    "metricas": { // métricas derivadas
        "promedio_ult2": null, // number | null
        "promedio_ult3": null, // number | null
        "tendencia_pct": null // number | null — variación % vs. examen anterior
    },
    "reglas": { // umbrales de propuesta de nivel
        "umbral_subir_total": 75, // promedio últimos 2
        "umbral_mejora_pct": 10, // mejora mínima vs. examen anterior
        "umbral_banda_roja": 60 // ninguna habilidad por debajo de este valor
    },
    "historial": [ // log append-only de eventos
        {
            "fecha": "YYYY-MM-DD",
            "version": 1,
            "evento": "init|diagnostico|examen|propuesta_nivel|actualizacion_nivel|rollback",
            "detalle": "texto breve",
            "autor": "usuario|IA"
        }
    ]
}