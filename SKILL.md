---
name: ds-meter
description: >
  Mede a densidade semiótica (DS) de um prompt ou de um termo, dá o score (1.0–5.0)
  E justifica os critérios da medida — quais termos são densos, quais genéricos,
  por quê. Distingue densidade cultural (dsc) de distribucional (dsd) e sugere
  operações SDE para subir a densidade. Use quando o aluno disser "mede a densidade",
  "quão denso é esse prompt", "compara antes e depois", "/ds-meter". A justificativa
  é o coração: não dá só o número, explica a régua. Tripé da aula: fi-forja (gerar) ·
  ds-meter (medir) · fi-enforce (atravessar).
domain: aula
version: 1.0.0
author: Renato Aparecido Gomes
provenance: >
  destilado de calculate_ds() (promptforge.py) + paper9 Densidade Semiótica
  (DOI 10.5281/zenodo.19382350) + DSc-DSd-DISTINCTION.md. Aula Framework Injection.
---

# ds-meter — Medidor de densidade semiótica com justificativa

> **O que esta skill faz:** mede quão denso é um prompt (ou um termo) — quanto de
> **método por palavra** ele carrega — num score de 1.0 a 5.0, **e explica os
> critérios da medida.** A justificativa é o ponto: outras ferramentas dão o
> número; esta mostra a régua.

## O que é densidade semiótica (o que estou medindo)

A razão entre o **método implícito** que uma instrução ativa e a **forma de
superfície** que ela ocupa. Termo denso = carrega um campo de prática inteiro
("refatorar", "rating interno"). Termo raso = aponta vagamente ("fazer", "análise").

**Dois eixos (não confundir):**
- **dsc — densidade cultural:** quanto o termo carrega numa comunidade humana.
- **dsd — densidade distribucional:** quanto o termo carrega na representação do
  *modelo* (a riqueza da vizinhança do termo no espaço do modelo). **É a dsd que
  importa para o FI** — é ela que ativa o frame na máquina.

## Como medir (siga estes critérios — e MOSTRE-OS)

Para cada prompt/termo, avalie e **explicite**:

1. **Razão método/superfície** — quanto de método cada palavra carrega. Muitas
   palavras, pouco método → DS baixa.
2. **Termos-âncora de domínio** (dsd alta) — presença de termos que ativam um frame
   técnico ("stress-test", "current ratio", "súmula vinculante"). Premiam o score.
3. **Genéricos** ("fazer", "coisa", "análise" solto, "melhor") — penalizam. Marque-os.
4. **Ativação de frame (Fillmore)** — o termo acende uma cena estruturada? "Garçom"
   acende o restaurante inteiro; "pessoa que serve" não.
5. **Alerta dsc×dsd** — o termo é culturalmente rico mas distribucionalmente raro
   (jargão local, gíria, marca regional)? Então tem dsc alta e dsd baixa → o modelo
   NÃO ativa o frame. Sinalize.

## Escala (1.0–5.0)
- **1.0–2.0** pidgin — telegráfico, genérico ("faça uma análise")
- **2.0–3.0** intermediário — algum método explícito, ainda vago
- **3.0–4.0** registro técnico — termos de domínio presentes
- **4.0–5.0** registro de especialista — método codificado, frame ativado

## Saída (sempre: número + POR QUÊ)

```
DS: 4.4 / 5.0

POR QUÊ (os critérios desta medida):
  ✓ "rating interno"  — termo-âncora financeiro, dsd alta (ativa o frame bancário)
  ✓ "stress-test em 3 cenários" — método explícito e numerável, não vago
  ✓ "cobertura de juros" — termo técnico de domínio
  ✗ "análise" (sozinho) — genérico, dsd baixa → candidato a densificar

dsc × dsd:  "rating interno" tem dsc E dsd altas no domínio financeiro ✓
            (se fosse gíria local, eu alertaria: dsc alta, dsd baixa → frame não ativa)

SUGESTÃO SDE para subir:
  • DENSIFY "avaliar" → "auditar com rating interno"   (~ +0.3)
  • BLEND "risco" + "concessão" → "risco de concessão" (frame mais específico)
```

## Modo comparação (antes/depois — o mais usado na aula)
`/ds-meter --comparar "versão A" "versão B"` →
```
A: "faça uma análise de risco"           DS 1.3 — pidgin (genéricos: faça, análise, risco solto)
B: "aplique rating interno com stress-test"  DS 4.2 — registro técnico (3 termos-âncora)
Δ +2.9 — você não escreveu mais; codificou método.
```

## Regras de honestidade (G6 — inegociáveis)
- **Densidade ≠ eficácia.** Esta skill mede o **input** (o que você diz), NÃO prova
  que o output será melhor. Um número real não é automaticamente um número que
  garante qualidade. Diga isso sempre que o aluno tratar DS como nota de qualidade.
- **Nunca crave comparação não-verificada.** Dê o score do prompt do aluno, medido
  agora. NÃO afirme "N× mais denso que a média/o controle" — esses números de
  referência precisam de proveniência que esta skill não tem em mãos.
- O score é uma **estimativa orientadora**, não uma medida de laboratório. Marque
  como tal. (Para o número reproduzível, use o backend Python.)

## Backend opcional (número exato)
O `promptforge.py` calcula a DS internamente e a devolve no JSON (`ds_original` e
`ds_fi`):
```
python promptforge.py --domain business --json "seu prompt" | python3 -c "import sys,json; d=json.load(sys.stdin); print('DS:', round(d['ds_fi'],2))"
```
> O CLI não tem um modo `--comparar` direto; para o antes/depois, rode o pedido cru
> e o FI gerado e compare os `ds_*`. A skill funciona sem o Python — você estima
> pelos critérios acima e os explicita; o backend fecha o número (testado
> 2026-06-27: cru "análise de risco" = DS 1.5, FI gerado = 4.5).

## Exemplo (termo isolado)
- Input: `/ds-meter "due diligence"`
- Output: `DS ~4.1 — termo-âncora jurídico-financeiro de dsd alta (ativa frame de
  auditoria, risco, compliance). Atenção: se o tokenizer fragmentar "due"+"dil"+
  "igence", a densidade composicional cai — risco de fragmentação (Cao et al. 2025).`
