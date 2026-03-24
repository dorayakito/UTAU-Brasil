# Glossário: Entendendo o "Utaulês"

Se você caiu de paraquedas no mundo do UTAU, deve ter dado de cara com um monte de siglas que parecem grego. Não se preocupa, a gente preparou esse guia pra você não se perder mais nas conversas da comunidade.

---

### Os Termos que Você Precisa Saber

- **VB (Voicebank)**: É a alma do negócio. Basicamente, é a pasta que guarda todos os áudios gravados que dão voz ao seu personagem. Sem um VB, o UTAU é só um programa vazio.
- **UST (UTAU Sequence Text)**: É o arquivo do projeto da música. Se você quer que o UTAU cante uma música específica, você precisa do UST dela. É como se fosse o "DNA" da melodia.
- **USTX**: É o formato de arquivo do OpenUtau. Ele é mais moderno e guarda muito mais informação que o UST antigo.
- **Resampler**: É o motor que processa a voz. Sabe quando você muda a nota no piano roll? O resampler é quem vira a chave pra voz subir ou descer o tom.
- **oto.ini**: É o arquivo de configuração de cada áudio. Ele diz pro UTAU exatamente onde começa e onde termina cada som. Se a oto.ini estiver ruim, o UTAU vai cantar "travado" ou fora do ritmo.
- **Flags**: São pequenos comandos de texto (tipo `g+10`) que você coloca nas notas pra mudar o estilo da voz (deixar mais grossa, mais fina, com sopro, etc).
- **Phonemizer**: Uma ferramenta mágica que converte o texto que você digita (tipo "pão") nos sons que o banco de voz entende. No OpenUtau isso é automático!

---

### Métodos de Voz (A sopa de letrinhas)

- **CV (Consonant-Vowel)**: O método mais básico e antigo. As amostras são sons simples como `ka`, `ki`, `ku`. É fácil de fazer, mas pode soar meio robótico se não for bem trabalhado.
- **VCV (Vowel-Consonant-Vowel)**: O queridinho para um som mais natural. Ele grava as transições entre as vogais, o que deixa o canto muito mais "ligado" e fluído.
- **CVVC**: Um meio termo entre o CV e o VCV. É ótimo para línguas como o português e o inglês, porque foca bastante nas transições entre consoantes e vogais.

---

### Outros Conceitos

- **wavtool**: Trabalha junto com o resampler pra "colar" os pedacinhos de áudio e formar a música final.
- **Rendering**: É o processo de "cozinhar" a música. Quando você aperta o play e o programa gera o som, ele está renderizando.
- **Bank**: Abreviação de Voicebank.
- **Voicer**: A pessoa real que emprestou a voz para gravar o banco.
- **UTAUloid / UTAU**: O personagem virtual em si (tipo a Kasane Teto).


