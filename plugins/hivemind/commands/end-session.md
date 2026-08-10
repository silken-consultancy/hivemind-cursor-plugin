---
name: end-session
description: Encerra a sessão HiveMind — consolida memórias e grava o handoff de continuidade (procedimento servido pelo engram).
---

Chame a tool `fos_procedure` com `{ "id": "end-session" }` e **execute o `body` retornado verbatim** — ele consolida as memórias da sessão e fecha com um `next_note` de continuidade garantido, para que o próximo boot retome de onde parou.
