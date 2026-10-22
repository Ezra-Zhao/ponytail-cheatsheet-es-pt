# Ponytail em uma página · Folha de consulta do Ponytail (cheatsheet)

[![Estrelas do projeto original](https://img.shields.io/github/stars/DietrichGebert/ponytail?style=social)](https://github.com/DietrichGebert/ponytail/stargazers) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE) ![Idiomas](https://img.shields.io/badge/idiomas-ES%20·%20PT-blue)

Support & more: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — templates & workbooks I use](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

> 🌍 [Español](README.md) · Português (esta página)

**Ponytail** é um conjunto de regras (um *skill*/prompt) para agentes de programação — Claude Code, Codex, Copilot, Gemini CLI, Cursor e mais de 20 agentes, segundo o projeto — que obriga o agente a subir uma escada de 7 degraus antes de escrever código novo. A ideia em uma frase: **o melhor código é o que nunca foi escrito**.

Esta folha resume todo o Ponytail em uma página: a escada, os 4 níveis, os comandos e a instalação em 2 passos. Nota honesta: o repositório original já traz o README completo em inglês e espanhol, mas **não tem versão em português**; isto não é uma tradução do README, é a versão de bolso em uma página.

## A escada: 7 degraus antes de escrever código

Antes de escrever, o agente para no **primeiro degrau que se sustenta**:

| # | Degrau | A pergunta que ele faz |
|---|--------|--------------------------|
| 1 | **Isso precisa existir?** | YAGNI: o que ninguém pediu não se escreve; se algo for pulado, ele diz em uma linha |
| 2 | **Já existe neste código?** | Reaproveite o helper, o componente ou o padrão que já existe, do jeito que o código vizinho usa |
| 3 | **A biblioteca padrão faz isso?** | Antes de inventar qualquer coisa, olhe a stdlib da sua linguagem |
| 4 | **A plataforma já traz isso?** | Uma API nativa vale mais que uma biblioteca nova (ex.: `<input type="date">` em vez de um calendário próprio) |
| 5 | **A dependência já está instalada?** | Use-a; nunca adicione uma dependência por causa de poucas linhas |
| 6 | **Cabe em uma linha?** | Se dá para entender de relance, que seja uma linha |
| 7 | **Só então: o mínimo que funciona** | E se a lógica tem desvios, laços, um parser, dinheiro ou segurança, deixe um teste pequeno |

Duas regras que não se negociam:

- A escada vem **depois** de entender o problema — ler o código que a mudança toca e seguir o fluxo real —, não no lugar disso. Preguiçoso com a solução, nunca com a leitura.
- **Nunca se corta**: validação nas fronteiras de confiança, tratamento de erros que evita perda de dados, segurança e acessibilidade.

## Os 4 níveis de intensidade

Troque o nível com `/ponytail lite|full|ultra|off`:

| Nível | Comportamento | Quando usar |
|-------|---------------|-------------|
| `lite` | Constrói o que foi pedido, mas nomeia em uma linha a opção menor e deixa o usuário escolher | Quando você quer manter o controle decisão a decisão |
| `full` | Aplica toda a escada. É o nível **padrão** | Uso diário |
| `ultra` | Além disso, questiona o pedido: antes de construir, discute qualquer parte que a necessidade não justifique | Quando a base de código «te ofendeu pessoalmente», nas palavras do projeto |
| `off` | Ponytail desligado | Quando você precisa do agente sem filtro |

Opcional: você pode fixar o nível padrão com `~/.config/ponytail/config.json` ou a variável de ambiente `PONYTAIL_DEFAULT_MODE`. Nada é obrigatório.

## Comandos em uma linha

| Comando | O que faz |
|---------|-----------|
| `/ponytail-review` | Revisa o diff atual como o dev sênior que é chamado quando algo quebra: bugs, segurança, carga real, código arriscado sem teste, partes lentas e o que sobra. Cada achado diz o que o código faz, o que falha, como corrigir e o que acontece se não corrigir |
| `/ponytail-audit` | A mesma revisão, mas para o repositório inteiro e ordenada por prioridade: o que corrigir primeiro |
| `/ponytail-debt` | Reúne os comentários `shortcut:` que você deixou para depois em um registro, para que «depois» não vire «nunca» |
| `/ponytail-gain` | Mostra o placar de impacto medido do benchmark do projeto: menos código, menos custo, mais velocidade |
| `/ponytail band` | Só no Claude Code: mostra ou oculta a barra acima do prompt com custo, tokens, tempo de agente e percentual de reutilização. `band import` adiciona suas sessões passadas ao painel local |
| `/ponytail-help` | Referência rápida dos comandos |

Os comandos precisam de um host com suporte a skills (Claude Code, Codex, Devin CLI, OpenCode, Gemini, Antigravity CLI, pi, Hermes Agent, Qoder, Grok Build). No Codex, eles são invocados com o prefixo do plugin: `$ponytail:ponytail-review`. Os adaptadores só de instruções (arquivo de regras do Cursor, Windsurf, Cline, Copilot, Kiro…) carregam as regras sempre ativas, mas sem os comandos.

## Instalação em 2 passos

**Claude Code** — dois comandos dentro da sessão:

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

**Codex** — dois comandos no terminal:

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

Depois abra `/hooks` no Codex, confie nos dois hooks de ciclo de vida e comece uma thread nova.

**Qualquer outro agente** (Copilot, Cursor, OpenCode, Gemini CLI e o restante dos 20+): copie o `AGENTS.md` do projeto original para o seu repositório, ou peça ao seu agente para instalar `skills/ponytail/SKILL.md` como skill. O guia passo a passo por agente está no `INSTALL.md` do projeto original.

⚠️ **Instale somente da fonte oficial**: `DietrichGebert/ponytail` no GitHub ou `@dietrichgebert/ponytail` no npm. O projeto afirma que nunca distribui arquivos `.exe` nem `.dll`; uma cópia que os inclua não é dele.

## O exemplo que explica tudo

Pedido: «adicione um seletor de datas ao frontend».

- **Sem Ponytail**: o agente instala uma biblioteca de date picker ou constrói um calendário na mão — **335 linhas**.
- **Com Ponytail**: primeiro ele olha o que já existe. O repo já tem um componente `Input`, e todo navegador traz um seletor de datas. Ele junta as duas coisas — **10 linhas**.

## Os números (benchmark do próprio projeto)

Números do Ponytail 5, medidos pelo próprio projeto no Claude Code comparando o mesmo agente com e sem o skill (39 tarefas, incluindo um repo real de FastAPI + React, 5 execuções por tarefa):

| Métrica | Com Ponytail 5 |
|---------|----------------|
| Código | **−53 %** |
| Tokens de saída | **−45 %** |
| Tempo | **−41 %** |
| Custo | **−26 %** |
| Lógica arriscada que sai com teste | **98 %** (sem o skill: 68 %) |

E nas 6 tarefas de segurança do benchmark (injeção de SQL, path traversal, tokens falsificados, rate limiting, linhas CSV malformadas, cache) ele passou nas 30 execuções: menos código, não menos seguro.

> **Nota de honestidade**: são números do benchmark publicado pelo próprio projeto, não medições independentes. A metodologia, as tabelas por tarefa e as limitações estão no repositório dele (`benchmarks/results/2026-10-07-agentic.md`). Se algo desta folha não coincidir com a documentação original, vale a original.

## Créditos

- Projeto original: [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) (licença MIT) · [ponytail.dev](https://ponytail.dev)
- Esta folha é um resumo independente, escrito com palavras próprias a partir da documentação pública do projeto. Não é uma tradução oficial nem tem afiliação com o projeto.

## 📚 Mais folhas de consulta

Outras folhas de consulta do mesmo autor, também em espanhol e português:

- [Docker 命令速查表 · Docker Cheatsheet](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 comandos de contêineres
- [FFmpeg 命令速查表](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 comandos de áudio e vídeo
- [Git 实战速查表](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 comandos diários e de resgate
- [curl 命令速查表](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 comandos para depurar APIs
- [正则表达式速查表](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 padrões práticos
- [jq 命令速查表](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 filtros JSON

## Licença

MIT © 2026 Guangyi Zhao, ver [LICENSE](LICENSE). O projeto Ponytail também é MIT.
