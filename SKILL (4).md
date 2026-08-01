---
name: fi-forja
description: >
  Gera um Framework Injection na forma canônica (8 seções + os 7 instrumentos do
  ofício) a partir de um pedido cru. Versão didática para alunos — mais simples
  que o FIG/forja-pro, sem têmpera adversarial, um caminho só. Produz um RFI
  (Reasoning Framework Injection = FI × Delivery Architecture) com dupla
  legibilidade (FI-máquina denso + FI-humano rotulado). Use quando o aluno disser
  "forja um FI", "cria um framework injection", "transforma esse pedido em FI",
  "/fi-forja". Tripé da aula: fi-forja (gerar) · ds-meter (medir) · fi-enforce (atravessar).
domain: aula
version: 1.0.0
author: Renato Aparecido Gomes
provenance: >
  destilado do promptforge.py (motor) + forma canônica atualizada ao estado da
  arte pós-8-jun-2026 (RFI=FI×DA, SCA, bridge_width rigoroso). Aula Framework
  Injection — Academia Lendária / Hub SP.
---

# fi-forja — Gerador de Framework Injection canônico (versão aluno)

> **O que esta skill faz:** pega um pedido cru ("faça uma análise de risco") e
> devolve um **Framework Injection canônico** — o método de raciocínio transferido
> à IA, não um prompt melhor. É o "Prompt-Forge como skill", simples o bastante
> para um iniciante usar sozinho.
>
> **O que ela NÃO faz** (de propósito, para ficar simples): não tempera
> (martelo adversarial = skill `/forja`, separada); não roteia multi-modelo; não
> faz benchmark. Só forja o FI canônico, limpo.

## Princípio inegociável: a canonicidade não se degrada

A interface é simples; **a forma canônica é integral.** Todo FI gerado respeita os
**7 instrumentos do ofício** e as **8 seções**. Simplicidade é da porta de entrada,
nunca do método.

## Como executar (você é o motor — siga este pipeline)

Ao receber `/fi-forja "<pedido cru>"` (com flags opcionais `--dominio` e `--executor`):

### Passo 1 · PODA SINTÁTICA (instrumento 2)
Limpe o ruído do pedido cru — fillers ("tipo", "né", "sei lá"), hesitações, pidgin
redundante. **Podar ≠ resumir:** remova só o ruído, preserve todo o sinal.

### Passo 2 · CLASSIFICAR domínio + modalidade
- **Domínio:** jurídico · código · financeiro · pesquisa · geral (use `--dominio` se dado).
- **Modalidade** (governa o que ancorar):
  - Declarativo (*o que saber*) · Procedimental (*como raciocinar*) · Avaliativo
    (*como julgar*) · Ético (*o que recusar*) · Composicional (*como combinar*).

### Passo 3 · DENSIDADE SEMIÓTICA (instrumento 1 — SDE)
Suba a densidade do pedido: troque termos genéricos por termos de **alta densidade
distribucional (dsd) no domínio-alvo** — termos que ativam o frame técnico na
*cabeça do modelo*, não só na sua. As 5 operações SDE: Densify · Rarefy · Rotate ·
Subtract · Blend. (Poda é instrumento 2, separado — não conte como operação SDE.)
> ⚠️ **Armadilha dsc×dsd:** um jargão que só você conhece (densidade cultural alta,
> distribucional baixa) NÃO ativa o frame no modelo. Prefira o termo que o modelo
> representa rico.

### Passo 4 · GERAR as 8 seções (na ordem canônica, âncoras nas bordas — curva-U)
```
[PAPEL]         o expert que o modelo encarna
[OBJETIVO]      a instrução-âncora (abre — instruction-first)
[MÉTODO]        a sequência de raciocínio induzida (passos numerados)
[CONTEXTO]      pano de fundo não-acionável
[FONTES]        hierarquia de autoridade do saber (do mais forte ao mais fraco)
[OUTPUT]        Delivery Architecture — forma de entrega (ver Passo 5)
[CONSTRAINTS]   limites duros (as red lines)
[VERIFICAÇÃO]   bicéfala: ASSERTS (constrangem) + CHECAGENS (induzem)
```
As 5 âncoras de alta carga (PAPEL, OBJETIVO, MÉTODO, CONSTRAINTS, VERIFICAÇÃO) vão
nas bordas; o miolo (CONTEXTO, FONTES) é conectivo.

### Passo 5 · DELIVERY ARCHITECTURE (instrumento 4 — transversal, não só jusante)
O `[OUTPUT]` não é só o fim: ele já define, **desde a formulação**, o que a saída
deve conter e como (estrutura, profundidade, critério de sucesso). Especifique-o
junto do `[OBJETIVO]`, não no fim. *(Isto torna o FI gerado um **RFI = FI × DA** —
conteúdo cognitivo × eixo de entrega.)*

### Passo 6 · MetaBlock (instrumento 6) — priming puro
Acrescente `[META-RACIOCÍNIO]`: identificar a suposição crítica que, se falsa,
invalida a resposta; sinalizar inferência vs afirmação; perguntar sob ambiguidade;
calibrar confiança.

### Passo 7 · REATUALIZAÇÃO DO TERMO DENSO — a coda (instrumento 7, CONDICIONAL)
- **Regra geral:** o MetaBlock é penúltimo e a **coda densa** encerra,
  re-ancorando o termo de domínio da abertura (recency, fecha o arco).
- **Exceção:** se a modalidade é **Ético / metacognitivo**, o **MetaBlock encerra**
  — a metacognição é o termo de maior carga.

## Saída (sempre os 3 blocos)

```
━━━ 1 · FI-MÁQUINA (cole isto na IA) ━━━
<prosa densa, instruction-first, sem rótulos>

━━━ 2 · FI-HUMANO (para você entender) ━━━
[PAPEL] ... [OBJETIVO] ... [MÉTODO] ... (as 8 seções rotuladas)

━━━ 3 · MEDIDA + PRÓXIMO PASSO ━━━
Densidade: ~X.X → ~Y.Y   (para o número detalhado e justificado, rode /ds-meter)
A [VERIFICAÇÃO] está como PRIMING (sugere, não obriga).
→ Para escalá-la a enforcement, rode /fi-enforce neste FI.
```

> **Dual-legibilidade (anti-drift):** o FI-humano é *projeção determinística* do
> FI-máquina — mesma gramática, nunca uma segunda geração. Um é otimizado para
> eficácia; o outro, para compreensão.

## Regras de honestidade (G6 — herdadas do programa de pesquisa)
- **Nunca crave número de comparação não-verificado.** Dê a densidade do prompt do
  aluno, medida agora. NÃO afirme "N× melhor que o controle" nem cite números de
  benchmark de cabeça.
- **Densidade ≠ eficácia.** A DS mede o *input* (o que o aluno diz), não garante o
  *output*. Diga isso se o aluno confundir.
- Marque inferência vs afirmação quando preencher seções com dados que o aluno não deu.

## Backend opcional (para quem tem Python)
Quem tiver o `promptforge.py` pode rodar o motor real para o número exato e
reproduzível:
```
python promptforge.py --domain business "faça uma análise de risco de crédito"
```
Domínios aceitos: `juridico · business · engenharia · pesquisa · marketing · medico`
(auto-detecta se omitir). Flags: `--format both|yaml|human` · `--json` · `--verbose`.
> Testado (2026-06-27): "faça uma análise de risco de crédito" → DS 1.5 → 4.5
> (+200%), determinístico. A skill funciona 100% sem o Python (você é o motor); o
> backend só fecha o número.

## Exemplo (caso-trilho de risco de crédito)
- Input: `/fi-forja "faça uma análise de risco de crédito"`
- Output: FI de 8 seções com `[PAPEL] analista de risco sênior`, `[MÉTODO]
  1.liquidez 2.alavancagem 3.stress-test 3 cenários`, `[FONTES] balanço auditado >
  benchmark > estimativa`, `[VERIFICAÇÃO] ASSERT ≥2 fontes citadas`. Densidade
  ~1.3 → ~4.4. Aviso para rodar `/fi-enforce`.
