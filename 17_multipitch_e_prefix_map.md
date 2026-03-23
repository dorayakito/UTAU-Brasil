# Multipitch e Prefix Map: Alcançando as Estrelas

Você comprou ou baixou um banco de voz, mas quando ele chega nas notas agudas, ele começa a soar como um esquilo? Isso acontece porque ele provavelmente é "Single Pitch" (gravado em apenas uma nota). O segredo pra uma voz que canta de tudo sem perder a qualidade é o **Multipitch**.

---

### O Que é o Multipitch?

É quando o voicer grava o mesmo banco de voz em várias alturas diferentes (oitavas).
- Por exemplo: Gravar em `A3` (grave), `D4` (médio) e `G4` (agudo).
- O UTAU vai selecionar automaticamente o áudio que estiver mais perto da nota que você desenhou no piano roll. Isso evita que o resampler precise esticar demais o áudio e distorcer o timbre.

---

### O Famoso Prefix Map

O **Prefix Map** é o "mapa da mina". É um arquivo dentro do banco de voz que diz pro programa qual oitava usar em qual trecho do piano roll.
- Se a nota no piano roll for um `C5`, o Prefix Map diz: "Ei, use o áudio da pasta `G4`!".
- Se for um `C3`, ele diz: "Use o áudio da pasta `A3`!".

---

### Como Configurar o Prefix Map

No UTAU clássico, você vai no menu `Project -> Prefix Map`.
- Lá você vê uma lista de todas as notas possíveis e escreve o sufixo da pasta (ex: `_G4`) no campo correspondente a cada oitava.
- No **OpenUtau**, isso é ainda mais fácil: o programa lê o Prefix Map original do banco, mas você pode sobrescrever as escolhas dele direto no editor de expressões.

---

### Dicas de Ouro pra Multipitch

1. **Evite Saltos Bruscos**: Se o timbre da pasta `A3` for muito diferente da `G4`, a voz vai parecer que mudou de pessoa no meio da música. Tente gravar com a mesma intensidade em todos os tons.
2. **Sufixos**: Sempre use nomes claros nas suas pastas (ex: `Normal_A3`, `Power_G4`). Isso ajuda o Prefix Map a não se perder.
3. **Crossfade**: No OpenUtau, você pode fazer o "blend" (mistura) entre duas oitavas diferentes pra transição ficar invisible. É o que a gente chama de "Pitch Scaling".

Bancos multipitch são maiores e pesados, mas a qualidade e o realismo que eles entregam são imbatíveis. Depois que você usa um banco multipitch bem feito, é difícil voltar pros bancos simples!
