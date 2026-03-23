# ✦ Conceitos Técnicos: Resamplers e Wavtools

Para entender como o UTAU gera o áudio final, precisamos entender o papel do motor de renderização.

---

### O que é um Resampler?

O **Resampler** é o motor que converte as taxas de amostragem dos arquivos dentro dos voicebanks. Ele vasculha o voicebank a procura das notas representadas na tela, baseando-se nas instruções da `oto.ini`.

**Funções principais:**
- Alinhamento e reajuste de pitch (tom).
- Alteração da velocidade de reprodução.
- Correção de tempo e redimensionamento dos arquivos de áudio.

Existem dois tipos principais de resamplers:
1. **Interpolação (Stretch):** Esticam a onda sonora (ex: TIPS, fresamp, moresampler).
2. **Reamostragem (Loop):** Repetem pedaços da vogal para preencher o tempo (ex: tn_fnds, WARP).

---

### O que é um Wavtool?

O **Wavtool** é o motor secundário. Ele é encarregado de "colar" os arquivos de áudio gerados pelo resampler. Ele analisa espectros, sequências, durações e amplitudes para entregar o produto final: o arquivo `.wav` que escutamos.

Em resumo, enquanto o Resampler "canta" a nota, o Wavtool "compõe" a música final.

---

### Dicas de Uso

- Cada voicebank pode soar melhor com um resampler específico.
- **TIPS** é ótimo para vozes suaves.
- **fresamp** é versátil para a maioria dos bancos.
- **tn_fnds** é excelente para evitar distorção em notas muito longas.
