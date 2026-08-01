---
name: fi-tempera
description: >
  Tempera um Framework Injection: ataca a obra para fortalecê-la, no ciclo
  forjar → martelar → recozer → resfriar. Versão didática para alunos — mais
  simples que a skill /forja completa (sem rotas A/B/C/D, sem AGORA/lapidação
  multi-passe). Separa os papéis: Artesão forja, Auditor martela (ataca), o
  método recoze (só as críticas que sobrevivem), Sentinela resfria (classifica
  OK/alucinação e calcula o Índice de Confiança). Use quando o aluno disser
  "tempera meu FI", "ataca esse framework", "fortalece isso", "/fi-tempera".
  Quarta skill do kit: fi-forja (gerar) · ds-meter (medir) · fi-enforce
  (atravessar) · fi-tempera (fortalecer).
domain: aula
version: 1.0.0
author: Renato Aparecido Gomes
provenance: >
  destilado de forja.md / forja-pro.md (têmpera completa) para versão aluno +
  separação de papéis (Artesão/Auditor/Sentinela) do workshop. Aula Framework
  Injection — Academia Lendária / Hub SP.
---

# fi-tempera — Tempere o seu FI (forjar → martelar → recozer → resfriar)

> **O que esta skill faz:** pega um FI já forjado e o **fortalece pelo ataque.**
> A têmpera não *verifica* o aço — ela o *transforma* pelo teste. Um FI não-temperado
> é rascunho; só o temperado é entregável.
>
> **O que ela NÃO faz** (para ficar simples): sem rotas múltiplas, sem lapidação
> multi-passe, sem AGORA. Um ciclo de têmpera, limpo. (A versão completa é a skill
> `/forja`, para uso avançado.)

## O princípio: separação de papéis

Quem cria não pode ser quem critica — senão você só confirma o que já pensou. A
têmpera separa **quatro papéis**, e você os encarna em sequência:

| Papel | O que faz |
|---|---|
| **Artesão** | forjou o FI denso (já feito — é o input) |
| **Auditor Constitucional** | *martela* — ataca a obra pelos pontos fracos |
| (recozer) | só as críticas que **sobrevivem** ao contra-ataque entram |
| **Sentinela** | *resfria* — classifica cada parte OK / ALUCINAÇÃO, excisa o que é invenção, calcula o Índice de Confiança |

## O ciclo (4 tempos)

### 1 · FORJAR (input)
O FI que o aluno já gerou com `/fi-forja`. Ponto de partida.

### 2 · MARTELAR (o Auditor ataca)
Encarne o **adversário mais hábil** e ataque o FI por cada vetor:
- **Vago:** alguma instrução pode ser interpretada de 2 jeitos? Aponte.
- **Não-verificável:** algum ASSERT não é checável de fato? (priming disfarçado)
- **Furo de fonte:** a hierarquia de FONTES tem buraco? Conflito não tratado?
- **Red line ausente:** falta um CONSTRAINT que o domínio exige?
- **Sobra:** alguma seção dilui em vez de densificar? (candidata a poda)

### 3 · RECOZER (filtrar)
Das críticas do Auditor, mantenha **só as que sobrevivem** a um contra-argumento.
Crítica que cai no primeiro contra-ataque era ruído — descarte. As que resistem
viram correções no FI.

### 4 · RESFRIAR (o Sentinela sela)
Para cada parte do FI corrigido, classifique:
- ✅ **OK** — sustentado, fica.
- ⚠️ **ALUCINAÇÃO** — número/fonte/fato que o FI afirma sem lastro → **excise** (remova
  ou marque "a confirmar"). Nunca deixe invenção passar.
Calcule o **Índice de Confiança** = partes OK / total de partes (quão limpo o aço saiu).

## Saída

```
━━━ FI TEMPERADO ━━━
<o FI corrigido, com as críticas sobreviventes aplicadas>

━━━ RELATÓRIO DE TÊMPERA ━━━
Martelo (Auditor) levantou: <N> ataques
Recozer: <M> sobreviveram (os outros eram ruído)
Resfriar (Sentinela): <K> alucinações excisadas
Índice de Confiança: <OK>/<total> = X.XX

Mudanças aplicadas:
  • <seção> — <o que mudou e por quê>
```

## Regras de honestidade (G6 — inegociáveis)
- **O Sentinela nunca deixa alucinação passar.** Se o FI afirma um número, uma fonte
  ou um fato sem lastro, isso é excisado ou marcado "a confirmar" — não maquiado.
- **Têmpera não infla.** O objetivo é um FI mais forte, não mais longo. Crítica que
  só adiciona volume sem fechar um furo real é descartada no recozer.
- **Índice de Confiança é estimativa orientadora**, não medida de laboratório.
  Marque como tal.

## Quando usar (lugar no fluxo)
```
/fi-forja "pedido"   → nasce o FI
/ds-meter            → mede a densidade
/fi-enforce          → escala a verificação para enforcement
/fi-tempera          → ataca e fortalece  ← VOCÊ ESTÁ AQUI (último passo)
```
Output não-temperado é rascunho; só o temperado é entregável.

## Exemplo (caso-trilho)
- Input: `/fi-tempera` no FI de risco de crédito.
- Auditor martela: *"'analise a solvência' ainda é vago — qual horizonte temporal?"*;
  *"o ASSERT '≥2 fontes' não diz fontes de quê"*.
- Recozer: ambas sobrevivem → vira `[MÉTODO] solvência em horizonte de 12 meses` e
  `ASSERT ≥2 fontes rotuladas por tipo (auditado/benchmark/estimativa)`.
- Sentinela: nenhuma alucinação (o FI não inventou números). Índice de Confiança: 1.0.
