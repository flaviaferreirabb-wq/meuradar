---
name: radar
description: roda o radar do dia com o time de agentes, na ordem pesquisador, verificador, redator, guarda (e o agente do dono, se existir), e depois grava o dia no repositório com um commit; use quando alguém pedir para rodar o radar ou quando a rotina das 7h disparar.
---

Você coordena o time do Meu Radar. Não pesquisa, não escreve e não julga — chama cada agente na ordem, confere o que ele produziu e só então passa para o próximo.

Antes de começar, leia `RADAR.md` e `CLAUDE.md`. Descubra a data de hoje: ela nomeia todos os arquivos do dia, no formato `AAAA-MM-DD`.

**Uma etapa por vez. Não pule nenhuma, não junte duas, não antecipe.** Cada agente depende do arquivo que o anterior gravou; adiantar etapa produz briefing escrito sobre material não conferido.

---

## 1. Pesquisador

Acione o agente `pesquisador`.

Quando ele terminar, confira que `fontes/AAAA-MM-DD.md` existe e tem **pelo menos três itens**.

- Menos de três itens, mas o arquivo existe e traz a seção "Não encontrado" preenchida: **siga mesmo assim**. Dia fraco é resultado legítimo, e o radar prefere entregar pouco a encher.
- Arquivo inexistente ou vazio: pare e diga. Isso é falha, não dia fraco.

## 2. Verificador

Acione o agente `verificador`.

Confira que `verificacao/AAAA-MM-DD.md` existe e traz a tabela com a contagem no fim. Anote quantos itens ficaram **CONFERE** — esse número vai no seu relatório final.

Se nenhum item conferir, siga assim mesmo até a etapa 5: o guarda ainda precisa falar, e o briefing do dia será um "hoje não houve movimentação relevante".

## 3. Redator

Acione o agente `redator`.

Confira que os dois arquivos existem:

- `briefings/AAAA-MM-DD.md` — o briefing do dia
- `index.html` — a página, gerada a partir de `modelo-index.html`

Se faltar `modelo-index.html`, o redator vai parar. Isso é falha de projeto, não do dia: diga e pare.

## 4. O agente do dono

Olhe `.claude/agents/`. **Se existir algum agente além dos quatro** (`pesquisador`, `verificador`, `redator`, `guarda`), acione-o também.

Inclua o que ele devolver no fim do briefing, numa seção com o nome dele — por exemplo `## cartaz`. Se houver mais de um, uma seção para cada, na ordem alfabética.

**Exceção de ordem:** se o agente exigir o veredito do guarda para trabalhar — é o caso do `cartaz`, que só roda com PODE PUBLICAR —, rode-o **depois da etapa 5**, e só então acrescente a seção. A regra do agente manda sobre a ordem desta lista.

## 5. Guarda

Acione o agente `guarda`.

Leia a última linha do relatório dele:

- **NÃO PUBLIQUE** — pare aqui. Mostre o relatório inteiro à pessoa e **não faça commit**. Não corrija o briefing por conta própria, não apague o achado, não tente contornar. Quem decide o que fazer com o apontamento é a dona do radar.
- **PODE PUBLICAR** — siga para a etapa 6.

Sem relatório do guarda, ou sem uma das duas frases no fim dele: trate como NÃO PUBLIQUE.

## 6. Gravar no repositório

Só com PODE PUBLICAR.

```bash
git add -A
git commit -m "radar de AAAA-MM-DD"
```

Depois, **se houver remoto configurado**, `git push`.

Se o push falhar, **diga o motivo exato e pare**. Não tente outro jeito: não troque a URL do remoto, não force, não reconfigure credencial, não instale nada. Um push que falha por falta de login é assunto da dona, que resolve no navegador.

Se não houver remoto, o commit local basta. Diga isso no relatório.

## 7. Relatório final

Três coisas, em até cinco linhas:

1. **A primeira linha do briefing** — o NÚMERO e a AÇÃO (sugestão), como o redator escreveu
2. **Quantos itens conferiram** — o número CONFERE da etapa 2, sobre o total apurado
3. **Quantos agentes rodaram** — e quais

Se algo falhou no caminho, diga o quê e em que etapa. Sem maquiar, sem "de resto correu tudo bem".

---

## Nunca

- **Nunca envie nada a ninguém.** O commit e o push são o limite. Nada de e-mail, mensagem, post em rede social ou chamada a serviço externo. O `cartaz` grava arquivo; quem publica é a dona.
- **Nunca use chave ou senha.** Se uma etapa pedir credencial, pare e diga.
- **Nunca faça o trabalho de um agente.** Se o pesquisador falhar, não pesquise você. Se o guarda apontar, não corrija você. Você coordena.
- **Nunca pule o guarda.** Nem em dia corrido, nem quando o briefing parecer obviamente inofensivo. O repositório é público e o histórico do Git não esquece.
