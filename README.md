# HiveMind — Cursor Plugin (spike)

Instala o **HiveMind** no Cursor como um plugin nativo: conecta o `engram` (memória + identidade
persistente) como MCP server via **Porta B** (bearer, sem mTLS) e traz os comandos `/boot` e
`/end-session`.

Este é o **spike de validação** da via-plugin: prova que um plugin do Cursor consegue empacotar uma
conexão MCP **remota + bearer** com o token injetado por-usuário, sem admin e sem file-drop.

## Segurança

- **Nenhum segredo mora neste repositório.** O `mcp.json` referencia `${HIVEMIND_TOKEN}` — um
  placeholder. O valor real (o seu `fospb_...`) é pedido pelo Cursor no momento do install e fica
  só na sua máquina.
- O endpoint é o ingress público do produto (`https://api.hivemind.ia.br/v1/mcp`); a autenticação
  é o bearer por-device, escopado e revogável.

## Instalar

1. No Cursor: **Customize → Plugins → importar** este repositório (git URL ou pasta local).
2. Em **Configure**, cole o seu token no campo **HiveMind device token** (`HIVEMIND_TOKEN`).
   - Gere o token na dashboard do HiveMind (emissão do `fospb_`).
3. Reinicie/abra uma nova conversa no agente.

## Verificar (o que o spike prova)

- [ ] **Conexão** — o MCP server `engram` aparece conectado e as tools `fos_*` respondem.
- [ ] **Token injection** — funciona com o token no `Configure`, sem nada de secreto no repo.
- [ ] **Frame** — `/boot` dispara `fos_procedure(id:"boot")` e o procedimento roda.

## Estrutura

```
.cursor-plugin/marketplace.json     # marketplace (permite crescer p/ outros plugins)
plugins/hivemind/
  .cursor-plugin/plugin.json        # manifesto: variables + mcpServers + commands
  mcp.json                          # engram: url + Authorization: Bearer ${HIVEMIND_TOKEN}
  commands/boot.md                  # /boot  -> fos_procedure(boot)
  commands/end-session.md           # /end-session -> fos_procedure(end-session)
```

---
_Parte do arco do conector HiveMind. Path A (mTLS) é Enterprise-only; este plugin usa a Porta B._
