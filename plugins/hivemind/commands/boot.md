---
name: boot
description: Inicia a sessão HiveMind — carrega identidade e contexto do projeto (procedimento servido pelo engram).
---

Chame a tool `fos_procedure` com `{ "id": "boot" }`. Pegue o campo `body` retornado e **execute-o verbatim e por completo** — é o procedimento de boot autoritativo do HiveMind: ele carrega as próprias tools MCP, resolve o slug do projeto e roda a espinha determinística de identidade + projeto.

Se a chamada falhar, vier vazia, ou o engram estiver inalcançável, **pare** e reporte que o boot do HiveMind não pôde ser servido — não improvise identidade ou estado a partir de memória.
