# Flags e Modificadores: O Guia do Clássico

Se você está usando o UTAU clássico, as **Flags** são seus comandos secretos. São letras e números que você digita nas propriedades da nota pra mudar como o som é processado. É um pouco "old school", mas funciona que é uma beleza.

---

### As Flags que Não Podem Faltar

1. **g (Gender)**: Muda o timbre.
   - `g+5` a `g+15`: Deixa a voz mais encorpada e profunda.
   - `g-5` a `g-15`: Deixa a voz mais fina e aguda.
2. **B (Breath)**: Adiciona ar.
   - `B50` ou `B100`: Traz aquele efeito de sussurro que a gente tanto ama.
3. **H e L (Filtros)**:
   - `H`: High-pass. Tira o som de "caixinha de som" e deixa a voz mais brilhante.
   - `L`: Low-pass. Abafa a voz, ótimo pra fazer efeito de rádio ou som distante.

---

### Flags por Motor (Resampler)

Nem todo resampler entende as mesmas flags.
- **Moresampler**: É o rei das flags avançadas. Ele tem o `Mb` (Body - corpo da voz) e `Mt` (Tension - tensão). Também tem a flag `G` que simula um rosnado (Growl) sintético muito legal pro metal.
- **TIPS**: Ele aceita as flags `Mt` e `Mb` de um jeito mais suave e musical.

---

### Onde eu Digito Isso?

- **Pelo Projeto Inteiro**: Em `Project -> Property`, lá tem um campo "Flags".
- **Por Nota**: Clique com o botão direito numa nota, vá em `Note Properties` e digite ali. A flag da nota sempre manda mais que a do projeto.

---

### Dica de Ouro: Modulação Zero (mod0)

Se você quer que a voz saia exatamente como você gravou, sem o UTAU tentar "corrigir" o tom da amostra original, use a flag `mod0`. Isso evita um som robótico e metálico em muitos bancos de voz bem feitos.

As flags parecem complicadas no começo, mas depois de um tempo você já decora seus combos favoritos (tipo `g+5mod0Y0B50`) e o processo vira automático!
