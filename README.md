# Sistema SAS — Grupo 7

> Materia: Diseño de Sistemas Web — Analista Funcional de Sistemas  
> Institución: Terciario Urquiza — Rosario  
> Docente: Pedernera Pablo  
> Cuatrimestre: 2.° 2026

## Integrantes

Ver [integrantes.md](integrantes.md)

## Descripción del proyecto

Sistema SAS es una herramienta de consulta y monitoreo comercial que integra Telegram, n8n y Loyverse mediante API REST sobre HTTPS. El usuario envía comandos por Telegram, cuyos mensajes viajan cifrados (TLS) y son reenviados por webhook a n8n, que actúa como middleware orquestador. n8n procesa la solicitud y consulta la API de Loyverse mediante peticiones HTTP, recibiendo la información en formato JSON. Luego formatea la respuesta y la envía a Telegram mediante su Bot API, que entrega el mensaje al usuario. La comunicación se basa en webhooks (push) en lugar de polling, optimizando el ancho de banda, mientras que la autenticación se realiza con el identificador de Telegram y tokens de API de Loyverse, garantizando un intercambio seguro y en tiempo real.

## Caso de estudio

El sistema no está orientado a una empresa puntual, sino diseñado como solución genérica para pequeños y medianos comercios. El contexto del problema es que estos negocios manejan caja y stock de forma informal, sin trazabilidad clara, sin métricas ni datos precisos, quedando expuestos a pérdidas y robos de dinero o mercadería. Sistema SAS resuelve esto integrando Loyverse (API REST de stock y ventas), n8n (middleware por webhook) y Telegram (interfaz cifrada por TLS) para el dueño o encargado. Al consultar stock, el mensaje viaja por webhook a n8n, que solicita datos a Loyverse vía HTTP y responde en menos de 15 segundos, generando alertas automáticas ante stock bajo, reduciendo pérdidas y agilizando decisiones.

## Entregas

| Entrega | Descripción | Fecha | Estado |
|---------|-------------|-------|--------|
| EP-01 | Presentación preliminar | | |
| EP-02 | | | |
| Final | Versión definitiva | | |

## Estructura del repositorio

```
/
├── README.md
├── integrantes.md
├── RECURSOS.md         ← leer antes de empezar: prerrequisitos, cheatsheet de git, recursos
├── docs/
│   ├── requisitos.md
│   ├── historias-de-usuario.md
│   ├── casos-de-uso.md
│   ├── er-modelo.md
│   ├── diseño-ui.md
│   └── stakeholders.md
├── diagramas/
│   ├── casos-de-uso.puml
│   ├── er.puml
│   └── wireframes/
└── cuestionario/
```

## Instrucciones operativas

- Un integrante del grupo es responsable de subir los cambios al repositorio.
- Completar `integrantes.md` antes de la primera entrega.
- Mantener los archivos en la carpeta correspondiente según la estructura indicada.
- Los diagramas deben entregarse en formato PlantUML (`.puml`). Se pueden visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/).
