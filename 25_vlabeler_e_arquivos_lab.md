# vLabeler e Arquivos LAB: Organizando a IA

Se você já gravou seu dataset pra criar sua voz em IA, agora vem a parte de "rotulagem" (labeling). É aqui que a gente diz pra IA em qual milissegundo começa e termina cada fonema usando arquivos `.lab`. E a melhor ferramenta pra isso é o **vLabeler**.

---

### O Que Diabos é um Arquivo LAB?

Pense nele como o substituto moderno da `oto.ini`.
- Cada arquivo de áudio seu vai ter um arquivo `.lab` correspondente.
- Dentro dele, existem linhas de tempo que dizem: "Do milissegundo X ao Y, o cantor está fazendo o som de 'A'".
- É isso que permite que a IA faça o alinhamento perfeito entre a melodia e as letras depois.

---

### Por Que Usar o vLabeler?

Antigamente isso era feito em programas chatos e difíceis. O vLabeler é visual, moderno e super rápido.
- Ele mostra o **espectrograma** (um mapa de cores do som), o que ajuda você a ver exatamente onde termina um som de `S` (que parece um chiado) e começa uma vogal.
- Ele tem atalhos de teclado que agilizam demais o processo.

---

### Autolabeling (A Mágica do Alinhamento Automático)

Ninguém merece rotular 2 horas de áudio na mão, né?
- Existem ferramentas (como o MFA - Montreal Forced Aligner) que tentam fazer esse trabalho sozinhas usando uma IA auxiliar.
- O seu trabalho no vLabeler depois é só abrir os arquivos e conferir se o alinhamento automático acertou tudo. Se errou, você só arrasta a linha pro lado e pronto!

---

### Dicas de Fluxo de Trabalho Pro

1. **Foque na Precisão**: Especialmente em sons rápidos como `T`, `P` e `K`. Se a marcação estiver errada, a voz final vai soar "atrapalhada".
2. **Consistência Fonética**: Use sempre o mesmo dicionário de fonemas (ex: se usar `nh`, use sempre `nh`, não mude pra `nj` no meio do projeto).
3. **Salve Sempre**: Perder o trabalho de rotulagem é frustrante demais. Crie backups constantes das suas pastas de labels.


