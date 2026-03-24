# Tutorial: O Legado do UTAU Classic

Muitos de nós começamos pelo UTAU clássico. Ele tem aquela cara de programa do Windows XP, mas não se engane: ele ainda é uma ferramenta poderosíssima se você souber como domar a fera.

---

### O Primeiro Grande Passo: Japanese Locale

Eu sei que a gente já falou disso, mas é sério: **Não tente abrir o UTAU sem mudar o Locale do Windows pra Japonês antes.**
- Vá em Painel de Controle -> Região -> Administrativo -> Alterar localidade do sistema.
- Escolha **Japonês (Japão)**.
- Reinicie o computador.
Isso é o que garante que o programa entenda os arquivos de voz.

---

### Conhecendo a Interface

- **Piano Roll**: No centro, onde você desenha as notas.
- **Menu de Propriedades**: Clique com o botão direito na nota pra mudar as flags, o volume e a modulação.
- **Botões do Topo**: Lá você encontra as ferramentas de seleção e os comandos de renderização.

---

### Como Importar um Banco de Voz

No UTAU clássico, você coloca as pastas das vozes dentro da pasta `voice` (que fica onde você instalou o programa).
- Depois de colocar a pasta lá, vá no menu do programa e selecione o banco.
- Se aparecerem símbolos estranhos no nome (Mojibake), é porque o Locale não foi configurado direito.

---

### O Segredo dos Plugins

O que mantém o UTAU antigo vivo são os plugins.
- **Iroiro**: Essencial pra converter letras e organizar o projeto.
- **AutoPitch**: Ajuda a criar aquelas curvas de afinação automáticas pra voz não soar tão reta.
- **Dica**: Coloque os arquivos do plugin na pasta `plugins` e reinicie o programa. Eles vão aparecer no menu `Tools -> Plugins`.

---

### Renderizando seu Trabalho

Pra ouvir a música inteira, você precisa selecionar todas as notas e apertar o botão de Play. O UTAU vai gerar pequenos arquivos de áudio temporários e depois tocar tudo junto.
- **Dica**: Se a renderização demorar muito, dê uma olhada no Resampler que você está usando. O `moresampler` costuma ser bem mais rápido que o padrão.


