# VCV Avançado: O Segredo da Fluidez Absoluta

Se você já domina o básico e quer que seu UTAU soe como um cantor real, o VCV (Vowel-Consonant-Vowel) é o caminho. É aqui que a gente para de trabalhar com sons picotados e começa a pensar em frases inteiras.

---

### Por que o VCV soa tão melhor?

O segredo está na transição. No mundo real, a gente não termina uma vogal pra começar outra do zero. Tudo é ligado. O VCV grava exatamente essa "ligação".
- Sabe aquele som `a ka`? No VCV, o programa usa o final do seu `a` anterior pra introduzir o `ka`. Isso acaba com aqueles "clicks" chatos entre as notas.

---

### 5-mora vs 7-mora: Qual a diferença?

Isso se refere ao tamanho da sequência que você grava:
- **5-mora**: Você grava 5 sílabas seguidas (ex: `a ka sa ta na`). É mais rápido de gravar.
- **7-mora**: Você grava 7 sílabas (ex: `a ka sa ta na ha ma`). É mais cansativo, mas dá mais opções pro programa escolher a melhor transição.
- **Dica**: Se você está começando, o 5-mora já entrega um resultado fantástico e poupa sua garganta!

---

### O Segredo da oto.ini no VCV

Aqui a oto.ini muda um pouco de figura:
- O **Overlap** no VCV precisa ser muito maior que no CV. É ele quem faz a "fusão" entre o final da nota anterior e o começo da próxima.
- Uma dica de ouro: No VCV, a linha do **Preutterance** deve ficar exatamente no início da consoante, e o **Overlap** deve cobrir quase todo o silêncio e parte da vogal anterior.

---

### Usando no Piano Roll

No UTAU clássico, usar VCV dá um trabalhinho:
- Você precisa colocar o prefixo da vogal na nota (ex: `a ka`).
- Felizmente, existem plugins como o **Iroiro** ou o **AutoVcv** que fazem isso pra você em um clique.
- No **OpenUtau**, basta selecionar o Phonemizer `JA VCV` e ele faz toda a mágica sozinho enquanto você digita em hiragana.

O VCV exige mais memória do PC e mais paciência do criador, mas quando você ouve seu UTAU cantando suavemente, você sente que cada segundo valeu a pena!
