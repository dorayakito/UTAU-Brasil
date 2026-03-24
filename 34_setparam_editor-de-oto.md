# SetParam

O SetParam é a ferramenta clássica de edição manual de `oto.ini`. Embora existam alternativas como o vLabeler, o SetParam ainda é amplamente utilizado por sua precisão e por ser o padrão em muitos tutoriais antigos.

Eu gosto do SetParam porque ele é direto ao ponto e te mostra exatamente o que você precisa ver pra voz não soar travada.

---

### A Interface do SetParam

Ao abrir o programa, você vê a "onda" do som e algumas linhas coloridas:
1. **Offset (Linha Branca)**: Marca onde o silêncio acaba e o som começa de verdade.
2. **Consonant (Rosa)**: A parte do áudio que o programa não pode esticar (a consoante rápida).
3. **Preutterance (Vermelha)**: Onde o som da vogal começa a atacar com força. Se isso tiver no lugar errado, a voz vai sair fora do ritmo.
4. **Overlap (Verde)**: Quanto esse som vai se misturar com o som da nota de trás.
5. **Cut-off**: Deixa o final do som limpo, cortando o que não serve.

---

### Dicas

1. **Ajuste Fino**: Use o zoom máximo para garantir que o **Preutterance** não esteja cortando o ataque da consoante (como o "t" ou "p").
2. **Batch Processing**: O SetParam permite pular para a próxima amostra automaticamente. Se você planejar bem suas gravações com o OREMO, configurar a OTO no SetParam vira um processo mecânico e rápido.
3. **Exportação**: Não esqueça de salvar (`Ctrl+S`). O UTAU só vai ver as mudanças depois que você salvar o arquivo `oto.ini` no SetParam.


