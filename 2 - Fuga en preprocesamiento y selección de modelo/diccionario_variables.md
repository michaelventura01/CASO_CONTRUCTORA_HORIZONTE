# Diccionario de Variables - Constructora Horizonte

## Dataset: Días restantes para completar un frente de trabajo

| Variable | Tipo | Descripción | Rango/Valores | Clasificación |
|----------|------|-------------|---------------|---------------|
| week_date | date | Fecha de registro semanal | 2025-01-19 a 2025-12-11 | Identificador |
| project_id | string | Identificador único del proyecto | 30 proyectos únicos | Identificador |
| workfront_id | string | Identificador único del frente | 150 frentes únicos | Identificador |
| work_type | string | Tipo de frente de trabajo | Excavación, Estructura, Electricidad, Terminaciones, Áreas Exteriores | Predictora |
| province | string | Provincia del proyecto | Santo Domingo, La Romana, Santiago, El Seibo, San Pedro de Macorís, La Altagracia | Predictora |
| planned_progress_pct | float | Avance planificado acumulado (%) | 6.9 - 100.0 | Predictora |
| actual_progress_pct | float | Avance real acumulado (%) | 6.1 - 100.0 | Predictora |
| workers_assigned | int | Trabajadores asignados al frente | 2.0 - 16.0 | Predictora |
| equipment_hours | float | Horas de equipo utilizadas | 4.2 - 66.4 | Predictora |
| material_availability_pct | float | Disponibilidad de materiales (%) | 52.5 - 100.0 | Predictora |
| rain_days_last_week | int | Días de lluvia semana anterior | 0.0 - 8.0 | Predictora |
| change_orders_to_date | int | Órdenes de cambio acumuladas | 0 - 2 | Predictora |
| contractor_id | string | Identificador del contratista | 12 contratistas únicos | Predictora |
| days_elapsed | int | Días transcurridos desde inicio | 7 - 280 | Predictora |
| engineer_estimate_days | int | Estimación del ingeniero (baseline operativo) | 0.0 - 635.2 | Predictora (Baseline) |
| days_remaining | int | Días reales restantes (TARGET) | 0 - 546 | Target |
| final_completion_date | date | **FUGA**: Fecha real de finalización | 5428 | Posterior (Excluir) |
| total_final_cost | float | **FUGA**: Costo final del proyecto | $9,621 - $180,478 | Posterior (Excluir) |
| final_delay_reason | string | **FUGA**: Razón principal del retraso | Clima, Diseño, Mano_de_obra, Órdenes de Cambio, Logística | Posterior (Excluir) |
| final_progress_report | string | **FUGA**: Informe de progreso final | {'En_progreso': 5382, 'Completado 100.0%': 39, 'Completado 99.0%': 7} | Posterior (Excluir) |
