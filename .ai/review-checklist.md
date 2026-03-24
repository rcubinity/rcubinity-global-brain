# Review checklist

- [ ] Matches **architecture** and layering for this repo (frontend/backend rules above).
- [ ] **API contract** aligned: route catalog / OpenAPI / shared naming with consumers (if the project uses one).
- [ ] **Validation** on inputs; **authz** where required.
- [ ] **Tests** added or updated for non-trivial logic.
- [ ] No **secrets** or tokens committed; env usage correct.
- [ ] **Context doc** (`.context.md` or equivalent) updated if behavior or contracts changed and the file is part of your documentation policy.
- [ ] **Performance**: N+1 queries, large payloads, and hot paths considered.
- [ ] **Logging**: no noisy or sensitive data in production logs.
