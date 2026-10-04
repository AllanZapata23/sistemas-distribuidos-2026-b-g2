<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       09-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 09

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Document configuration vs secrets vs feature flags; document secure software delivery / progressive delivery practices; and produce visual summaries for both study topics.
<!-- CONFIG-END -->

> **Week 09 deliverable — secure config + progressive delivery (study track).** Study notes and visuals on **configuration / secrets / feature flags** and a **guide to secure software delivery**.
> **Course fork:** https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2
> **Author email:** `azapatat-2021a@corhuila.edu.co`
> **Week window:** Monday **2026-09-28** → Sunday **2026-10-04**.

## 1. User stories worked this week

| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-OPT-075 | Document configuration, secrets and feature flags | done | [`Configuration-Secrets-and Features Flags.md`](./Configuration-Secrets-and%20Features%20Flags.md) |
| HU-OPT-076 | Produce visual summary of configuration, secrets and feature flags | done | [`Configuration-Secrets-and Features Flags.png`](./Configuration-Secrets-and%20Features%20Flags.png) |
| HU-OPT-077 | Document planning for secure configuration and progressive delivery | done | [`Guide to secure software delivery.md`](./Guide%20to%20secure%20software%20delivery.md) |
| HU-OPT-078 | Produce visual summary of secure configuration / progressive delivery | done | [`Guide to secure software delivery.png`](./Guide%20to%20secure%20software%20delivery.png) |

## 2. My individual contribution

> **Author:** Allan Zapata Torres / `AllanZapata23` (`azapatat-2021a@corhuila.edu.co`).  
> **Week 09 scope (Mon–Sun):** study notes + visuals prepared within **2026-09-28 … 2026-10-04**; target course-fork commit date **2026-10-04** (end of week).
> **Scope this week:** study notes + visuals on secure configuration and progressive delivery.  
> **Code note:** I did **not** author `opti-*` scaffold PRs this week; my verified code slice lands in Week 10 on `opti-front`.

### Study notes and visuals

| Artifact | What it covers |
|----------|----------------|
| [`Configuration-Secrets-and Features Flags.md`](./Configuration-Secrets-and%20Features%20Flags.md) | Configuration vs secrets vs feature flags; lifecycles; security posture |
| [`Configuration-Secrets-and Features Flags.png`](./Configuration-Secrets-and%20Features%20Flags.png) | Visual summary of configuration / secrets / feature flags |
| [`Guide to secure software delivery.md`](./Guide%20to%20secure%20software%20delivery.md) | Secure delivery practices, progressive delivery ideas, operational safeguards |
| [`Guide to secure software delivery.png`](./Guide%20to%20secure%20software%20delivery.png) | Visual summary of secure / progressive delivery |

### Mapping to OptiView

| Topic | How I apply it |
|-------|----------------|
| Configuration | Env-specific settings outside source for `opti-*` |
| Secrets | Never commit tokens/keys; inject via environment / secret store |
| Feature flags | Progressive exposure of UI/API capabilities without hard forks |
| Progressive delivery | Promote through `develop` → `qa` → `main` with small reversible slices |

## 3. Blockers and risks

- HU-status template was empty until this fill-in; artifacts must be committed to fork `main`.
- No personal `opti-*` code commits dated in this Mon–Sun window to list here.

## 4. Plan for next week

- Land an `opti-front` dashboard slice (charts clarity + product-family links).
- Write Week 10 persistence (saga/outbox/CQRS) and MVP 2 release notes + visuals.

## 5. Compliance self-check

- [x] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [x] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables

Notes: Week window is Mon–Sun (**2026-09-28 → 2026-10-04**). Course-fork HU-status commits on `main` should use author/committer dates inside that window. Week 09 for me is documentation + visuals. Per-env / tests / hexagonal boxes stay unchecked until my Week 10 front commit.

## 6. Evidence links

- [`Configuration-Secrets-and Features Flags.md`](./Configuration-Secrets-and%20Features%20Flags.md)
- [`Configuration-Secrets-and Features Flags.png`](./Configuration-Secrets-and%20Features%20Flags.png)
- [`Guide to secure software delivery.md`](./Guide%20to%20secure%20software%20delivery.md)
- [`Guide to secure software delivery.png`](./Guide%20to%20secure%20software%20delivery.png)
- Course fork: https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2
- Next week code evidence (Week 10): https://github.com/code-corhuila/opti-front/commit/9c251b839323504383f38bb17b8cb1d79a34e23e
