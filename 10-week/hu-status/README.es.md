<!-- PLANTILLA HU-STATUS (traducción al español) - NO borres los marcadores <!-- ... -->
     ni las cabeceras de tabla.
     ATENCIÓN: la nota semanal se lee AUTOMÁTICAMENTE del archivo en inglés:
       10-week/hu-status/README.md  (dentro de TU fork).
     Este archivo es una copia en español para lectura y no se califica. -->

# Estado Semanal - Semana 10

<!-- CONFIG-START - debe coincidir con el CONFIG de tu repo de perfil (username/username) -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Aterrizar mi slice de dashboard en `opti-front` (gráficas más claras y enlaces a todas las familias de producto) sobre el shell de la Semana 09 y promoverlo `develop` → QA → `main`; escribir notas y visuales de persistencia (saga, outbox, CQRS) y shipping MVP 2.
<!-- CONFIG-END -->

> **Ventana:** lunes **2026-10-05** → domingo **2026-10-11**.  
> **Fork:** https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2  
> **Correo:** `azapatat-2021a@corhuila.edu.co`

## 1. Historias de usuario trabajadas esta semana

### A. Esta semana

| HU ID | Título | Estado | Evidencia |
|---|---|---|---|
| HU-OPT-090 | Documentar persistencia (saga, outbox, CQRS) | done | [`persistence_in_distributed_systems.md`](./persistence_in_distributed_systems.md) |
| HU-OPT-091 | Resumen visual saga / outbox / CQRS | done | [`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png) |
| HU-OPT-092 | Documentar release MVP 2 | done | [`release_shipping_mvp_2.md`](./release_shipping_mvp_2.md) |
| HU-OPT-093 | Resumen visual release MVP 2 | done | [`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png) |
| HU-OPT-138 | Clarificar charts del dashboard y enlazar familias de producto en `opti-front` | done | https://github.com/code-corhuila/opti-front/commit/9c251b839323504383f38bb17b8cb1d79a34e23e · #21 · #22 · #23 |
| HU-OPT-139 | Co-autor HU-14 dashboard en `opti-front` | done | #13 · https://github.com/code-corhuila/opti-front/commit/0af50b828c8c8e6780a12aa4ad1388408a2b588b |

### B. Reporte tardío Semana 09

Sin trabajo del domingo 2026-10-04.

## 2. Mi contribución individual

Autor: Allan Zapata Torres / `AllanZapata23`. Notas + visuales de la semana y commit propio de dashboard en `opti-front`. No abrí PRs como autor; compañeros abrieron los PRs de merge/promoción.

## 3. Bloqueadores y riesgos

- Cero PRs con `author:AllanZapata23`.
- Sin tests nuevos en mi commit.
- No reportar trabajo de otros repos/autores como propio.

## 4. Plan próxima semana

- Abrir al menos un PR como `AllanZapata23`.
- Agregar un test enfocado del dashboard.
- Tomar un slice Corte 3 de punta a punta.

## 5. Autoverificación

- [x] Conventional Commits
- [x] PR por ambiente (mi commit fue promovido develop/qa/main)
- [x] Criterios verificables
- [ ] Tests
- [x] Límites DDD / hexagonal (UI shell)
- [x] Sin secretos

## 6. Enlaces de evidencia

- [`persistence_in_distributed_systems.md`](./persistence_in_distributed_systems.md)
- [`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png)
- [`release_shipping_mvp_2.md`](./release_shipping_mvp_2.md)
- [`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png)
- https://github.com/code-corhuila/opti-front/commit/9c251b839323504383f38bb17b8cb1d79a34e23e
- https://github.com/code-corhuila/opti-front/pull/13 · #21 · #22 · #23
