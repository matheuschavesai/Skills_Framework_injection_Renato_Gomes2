---
name: fi-enforce
description: >
  Consultor de travessia priming → enforcement. Analisa um prompt/FI, classifica
  cada cláusula (priming · priming disfarçado · enforcement) pelo teste de 3
  perguntas, e para cada "priming disfarçado" PROPÕE o ASSERT executável que falta —
  o critério externo contextual que faz a cláusula atravessar a ponte. Converte
  prosa em assert verificável e calcula o bridge_width. Use quando o aluno disser
  "como faço isso virar enforcement", "isso obriga ou só sugere", "cria os asserts",
  "/fi-enforce". Tripé da aula: fi-forja (gerar) · ds-meter (medir) · fi-enforce (atravessar).
domain: aula
version: 1.0.0
author: Renato Aparecido Gomes
provenance: >
  FONTE-classificacao-priming-enforcement.md + paper L2 CODEX-DRIVER/bridge_width
  (2026-06-13, SCA/P2E, definição enforceable rigorosa, limited-efficacy veto).
  Aula Framework Injection.
---

# fi-enforce — Da sugestão à obrigação (priming → enforcement)

> **O que esta skill faz:** pega um prompt cheio de "deveria" e ajuda você a achar
> onde ele só **induz** (priming) versus onde de fato **obriga** (enforcement) — e,
> onde só induz mas parecia obrigar, **cria o critério externo** que falta para
> atravessar a ponte.

## A descoberta que tira o achismo

**Enforcement não é uma propriedade da palavra — é uma propriedade de ONDE a regra
é verificada.** Por mais imperativo que um texto pareça (`"NUNCA faça X"`), se nada
fora do modelo reprova a saída, é só priming. O que classifica é a existência de um
**verificador externo executável** atrelado à cláusula.

## O teste de 3 perguntas (aplique a CADA linha — binário, sem achismo)

1. **Tem uma condição que dá pra checar com sim/não?**  Não → **PRIMING**. Pare.
2. **Quem checa é algo FORA do modelo?** (um comando, um teste, um arquivo, uma lei)
   Não → **PRIMING DISFARÇADO** (se a condição era binária). Pare.
3. **Se falhar, a resposta é REPROVADA?**  Sim → **ENFORCEMENT**.

Três perguntas binárias, zero adjetivo. Uma máquina poderia rodar isso — é a prova
de que não é interpretação subjetiva.

## As três classes (e a do meio é onde mora o aprendizado)

| Classe | O que é | Analogia |
|---|---|---|
| **PRIMING** | induz, por design (cláusula cognitiva) | a placa "siga em frente" |
| **PRIMING DISFARÇADO** | parece regra, mas nada reprova | a **catraca de papelão** |
| **ENFORCEMENT** | condição binária + verificador externo que reprova | a catraca de aço |

> **A "catraca de papelão" é o alvo da skill.** `"NUNCA recomende sem os 5 eixos"`
> tem cara de enforcement mas, sozinha no texto, não trava ninguém. A skill troca o
> papelão por aço: anexa o verificador que faltava.

## Como executar

Ao receber `/fi-enforce "<prompt ou FI>"`:

1. **Classifique cada linha** pelo teste de 3 perguntas.
2. Para cada **PRIMING DISFARÇADO**, **gere a hipótese de enforcement contextual** —
   o `cmd:`/`test`/`grep`/schema certo *para aquele prompt* (não um assert genérico).
   - **enforceable** = condicional decidível e verificável que constrange a saída.
   - **cognitive** = orientação interpretativa que prima, não vincula → mantenha como CHECAGEM.
3. **Aplique o veto de eficácia-limitada:** não proponha como assert algo que depende
   de juízo futuro/interpretação aberta ("avalie se é razoável") — isso é cognitivo
   por natureza, não force a virar enforcement (seria assert-teatro).
4. **Calcule o bridge_width** = asserts executáveis / (asserts + checagens), antes e depois.

## Conversão prosa → assert (o gesto central)

| Prosa (priming disfarçado) | Assert executável (enforcement) |
|---|---|
| "garanta que cita as fontes" | `grep -cE "\[(auditado\|benchmark\|estimativa)\]" saida.md` ≥ 2 |
| "a saída deve ter as seções X e Y" | `grep -q "X" saida.md && grep -q "Y" saida.md` |
| "retorne JSON válido" | `jq empty saida.json`  (reprova se malformado) |
| "não deixe campo vazio" | schema/validador que rejeita null nos campos obrigatórios |
| "considere se é razoável" | — **não converta**: é cognitivo, fica como CHECAGEM |

## Saída

```
ANÁLISE DA PONTE  (bridge_width atual: 0.0)

Linha: "NUNCA recomende sem os 5 eixos preenchidos"
  → PRIMING DISFARÇADO (parece regra; nada reprova)
  → HIPÓTESE DE ENFORCEMENT:
     ASSERT: a saída contém os 5 eixos
     cmd: grep -qiE "liquidez|alavancagem|rentabilidade|stress|rating" saida.md
     → agora REPROVA se faltar um eixo. Atravessou para enforcement ✓

Linha: "considere o contexto do litígio"
  → PRIMING (cognitivo, por design — mantém como CHECAGEM)

bridge_width: 0.0 → 0.5   (1 de 2 verificáveis cruzou a ponte)
```

## Regras de honestidade (G6 — inegociáveis)
- **Nunca crave número de comparação não-verificado.** Dê o bridge_width do FI do
  aluno, calculado agora. **NÃO cite "0.33", "N× o controle"** nem médias de
  referência: o achado G6 do programa é que esse controle não tem proveniência
  (mede 0.0 sob o mesmo classificador). Use só o número do artefato em mãos.
- **Bridge_width alto não é meta cega.** Calibre ao solver e ao domínio (vetor E2P):
  domínio HARD (código/jurídico/financeiro) pede mais enforcement; pesquisa/texto
  aberto vive bem com mais priming. Forçar enforcement onde o critério é interpretativo
  gera assert-teatro (reprova).
- Marque inferência vs afirmação ao propor o verificador (o `cmd:` é uma *hipótese*
  contextual, a ser testada — não uma verdade cravada).

## Backend opcional
Onde houver harness, o assert proposto pode ser plugado num gate real (G4) que roda
`curl`/`pytest`/`grep`/`docker` e reprova de fato. A skill propõe o assert; o harness
o executa.

## Exemplo (caso-trilho)
- Input: `/fi-enforce` no FI de risco de crédito.
- Output: identifica que `"NUNCA recomende sem os 5 eixos"` e `"todo número com
  fonte"` são priming disfarçado; propõe `grep -qE "rating" && grep -q "stress"` e
  `grep -cE "\[(auditado|benchmark|estimativa)\]" ≥ 2`; bridge_width 0.0 → 0.66.
