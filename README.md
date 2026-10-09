# HiveMind — Cursor Plugin

Instala o **HiveMind** no Cursor como um plugin nativo: conecta o `engram` (memória + identidade
persistente) como MCP server remoto, autenticado pelo seu token de device.

O plugin não carrega nenhum procedimento. O início e o fim de sessão são **servidos pelo engram**
como prompts MCP (`boot` e `end-session`), então você roda sempre a versão atual, sem cópia local.

## Segurança

- **Nenhum segredo mora neste repositório.** O `mcp.json` referencia `${HIVEMIND_TOKEN}` — um
  placeholder. O valor real (o seu `fospb_...`) é pedido pelo Cursor no momento do install e fica
  só na sua máquina.
- O endpoint é o ingress público do produto (`https://api.hivemind.ia.br/v1/mcp`); a autenticação
  é o token por-device, escopado e revogável.

## Instalar

1. No Cursor: **Customize → Plugins → importar** este repositório (git URL ou pasta local).
2. Em **Configure**, cole o seu token no campo **HiveMind device token** (`HIVEMIND_TOKEN`).
   - Gere o token na dashboard do HiveMind.
3. Reinicie/abra uma nova conversa no agente.

## Usar

- **Início de sessão:** digite `/` no chat do agente e escolha o prompt `boot` do servidor `engram`.
- **Fim de sessão:** digite `/` e escolha o prompt `end-session` do servidor `engram`.

Se a sua versão do Cursor não listar os prompts do servidor, peça ao agente: _"rode o procedimento
de boot servido pelo engram"_ (ou _"de end-session"_, ao encerrar). Se o engram não responder, a
sessão não começa — o agente deve parar e avisar, nunca improvisar.

## Verificar

- [ ] **Conexão** — o MCP server `engram` aparece conectado em Settings → MCP.
- [ ] **Token** — funciona com o token no `Configure`, sem nada de secreto no repo.
- [ ] **Prompts** — `/` lista `boot` e `end-session` do `engram`, e o `boot` inicia a sessão.

## Estrutura

```
.cursor-plugin/marketplace.json     # marketplace (permite crescer p/ outros plugins)
plugins/hivemind/
  .cursor-plugin/plugin.json        # manifesto: nome, descrição e a variável HIVEMIND_TOKEN
  mcp.json                          # engram: url + Authorization: Bearer ${HIVEMIND_TOKEN}
```

## Se o token não for pedido no import

O `mcp.json` usa `${HIVEMIND_TOKEN}` (resolvido pela `variable` declarada no manifesto). Se a sua
versão do Cursor não pedir o valor no Configure, edite `plugins/hivemind/mcp.json` na sua cópia local
e troque `${HIVEMIND_TOKEN}` pelo seu `fospb_...` — **sem commitar**.
