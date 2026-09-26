# Identificación de Drivers Arquitectónicos

**Objetivo:** Integrar los elementos identificados anteriormente y determinar cuáles tienen una **influencia significativa en las decisiones de arquitectura**.

| ID | Driver arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- |
| **DA01** | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales.[cite: 8] | AC03 – Escalabilidad[cite: 8] | Puede influir en la estrategia de escalamiento y despliegue.[cite: 8] |
| **DA02** | El sistema debe mantener tiempos de respuesta adecuados durante una alta concurrencia.[cite: 8] | AC01 – Rendimiento[cite: 8] | Puede influir en la comunicación entre componentes, procesamiento y almacenamiento.[cite: 8] |
| **DA03** | El sistema debe proteger los datos de usuarios y operaciones de compra.[cite: 8] | AC04 – Seguridad[cite: 8] | Puede influir en autenticación, autorización y protección de datos.[cite: 8] |
| **DA04** | El sistema debe integrarse con una pasarela de pago externa mediante una API.[cite: 8] | RC04 - Pasarela de pago[cite: 8] | Condiciona la forma de comunicación e integración con servicios externos.[cite: 8] |
| **DA05** | El sistema debe utilizar una API REST para la comunicación entre frontend y backend.[cite: 8] | RC03 – API REST[cite: 8] | Limita las alternativas de comunicación entre las partes del sistema.[cite: 8]