# ✦ Tipos de Voicebank e Phonemizers

Cada banco de voz tem seu formato de gravação e configuração. É fundamental entender a diferença entre eles para saber como usá-los corretamente.

---

### Métodos de Gravação

| Sigla | Nome | Como Funciona |
|-------|------|---------------|
| **CV** | Consoante Vogal | Ex: `ko`, `ko`, `ro`. Cada nota é uma sílaba simples. Mais fácil de gravar e configurar, mas pode soar menos natural. |
| **VCV**| Vogal Consoante Vogal | Ex: `- ko`, `o ko`, `o ro`. Onde `-` é o silêncio. Focado na transição suave entre a vogal anterior e a sílaba atual. |
| **CVVC**| CV + VC | Ex: `[ko] [o r] [ro]`. Híbrido que usa conexões de vogal para consoante para suavizar o canto. Fácil de gravar e resultados ótimos. |
| **CVC** | Consoante Vogal Consoante | Usado para línguas complexas como Português ou Russo. Ex: `[-t] [-r a] [a b] [b a] [a lh] [lh a] [a rh-]`. |
| **VCCV**| VCCV | Extensão do CVVC para línguas como Inglês. Foca em preservar a transição natural das consoantes. |
| **Rentan**| Fake VCV | Basicamente um CV com uma "faux-vogal" (falsa vogal) para simular a suavidade do VCV sem precisar gravar todas as combinações. |

---

### Terminologia Adicional

**Mora (モーラ)**
Unidade linguística usada para medir a duração de um som. No UTAU, é representado por um underline (`_`) e indica que os sons foram gravados de forma contínua.

**Multipitch (ou Multitom)**
Bancos gravados em mais de um tom (ex: A3, D4, G4). Permitem que o UTAU alcance notas mais altas ou baixas com maior qualidade.

**Kire (Powerscale)**
Gravação multipitch onde os tons mais baixos são suaves e os mais altos são poderosos/gritantes.

**Append(s)**
Bancos adicionais com tons de voz específicos (Soft, Dark, Power, Whisper).

---

### Phonemizers (Fonemizadores)

Um Phonemizer converte o texto simples em notas separadas com a notação correta do banco de voz.

- **Utilidade:** Facilita muito o uso de bancos complexos (como CVVC ou VCV) ao evitar que você precise digitar cada fonema manualmente.
- **No OpenUtau:** Você pode alterar o Phonemizer clicando no botão abaixo do nome do cantor na track.

**Exemplo:**
Ao usar um banco Português CVC e digitar `pão`, o phonemizer converte para `[p an w]`.
