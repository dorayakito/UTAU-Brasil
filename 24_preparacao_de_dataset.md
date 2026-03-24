# Preparação de Dataset AI: Do Áudio ao Modelo

Se você quer criar o seu próprio banco de voz de Inteligência Artificial (tipo DiffSinger ou ENUNU), você não vai gravar uma lista de sons picotados. Você vai precisar de um **Dataset**. Vamos ver como preparar esse conjunto de dados pra IA não se perder.

---

### O Que é um Dataset de Voz?

É basicamente uma coleção de áudios de você cantando ou falando, acompanhados pelo texto exato do que você disse. A IA vai "estudar" esses arquivos pra aprender a sua voz.

---

### Dicas de Ouro pra Gravação

1. **Variação é Tudo**: Não grave apenas músicas lentas. Cante rápido, devagar, agudo, grave, alegre, triste... Quanto mais variedade você der pra IA, mais versátil o seu modelo final vai ser.
2. **Qualidade de Estúdio**: Aqui a gente não pode ter erro. Qualquer ruído de fundo (até o som do seu ventilador) vai ser aprendido pela IA e vai aparecer na voz final. Use o melhor microfone que conseguir e trate o som da sua sala com cobertores ou espumas.
3. **Duração**: Pra um banco de IA ficar bom, você vai precisar de pelo menos 1 a 2 horas de áudio limpo. Sim, dá trabalho, mas o resultado compensa!

---

### Organizando seus Arquivos

- **Áudios**: Devem estar em formato `.wav` (44.1kHz ou 48kHz, 16 ou 24 bits).
- **Textos**: Cada áudio precisa de um arquivo `.txt` ou `.lab` com o mesmo nome, contendo a letra do que foi cantado.
- **Fragmentação**: Não envie um arquivo de 10 minutos direto. Corte seus áudios em pedaços pequenos, de 5 a 15 segundos cada. Isso facilita muito o "treinamento" da IA.

---

### Equipamento Recomendado

Se você está levando a sério a criação de uma IA:
- Uma interface de áudio dedicada.
- Um microfone condensador de boa qualidade.
- Um fone de ouvido fechado pra não vazar som do metrônomo pra gravação.


