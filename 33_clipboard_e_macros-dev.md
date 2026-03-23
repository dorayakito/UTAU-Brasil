# Clipboard e Macros: Eficiência no Tuning

O tuning no UTAU pode ser um processo repetitivo. O uso de plugins de Clipboard (Área de Transferência) e Macros permite copiar e colar parâmetros complexos entre diferentes notas e projetos.

---

### O Plugin Clipboard (PasteSpecial)

Diferente do Ctrl+C padrão do Windows, os plugins de [Clipboard do UTAU](https://overseasutau.forumotion.net/t1859-utau-copy-paste-plugin-srsly-we-all-need-this) (como o `clipboard.zip` e `pastespecial.zip`) capturam metadados específicos:
- **Pitch Curves**: Copie apenas o desenho da curva de tom de uma nota e aplique em outra, mesmo que a nota seja diferente.
- **Envelopes**: Transfira a dinâmica de volume entre frases inteiras.
- **Flags**: Aplique um conjunto complexo de flags a várias notas de uma vez sem precisar digitá-las novamente.

---

### Macros no OpenUtau

O OpenUtau elevou a automação a um novo nível com o sistema de `Scripts` e `Macros`.
- **Scripts em IronPython**: Usuários avançados podem escrever scripts para automatizar tarefas como "Humanizar Pitch" ou "Ajustar Velocity com base no BPM".
- **Atalhos**: Você pode vincular esses scripts a teclas de atalho (Hotkeys) para um fluxo de trabalho extremamente rápido.

---

### Plugins de Legato e Portamento

Muitos macros auxiliam na criação de transições suaves:
- **Portamento Auto**: Calcula automaticamente o tempo necessário para deslizar de uma nota a outra sem que o som fique artificial.
- **Crossfade Automático**: No UTAU clássico, o botão "Crossfade" (do canto superior direito) é tecnicamente uma macro que ajusta os envelopes das notas selecionadas para evitar clicks.

---

### Como Aumentar sua Produtividade

1. **Templates de Tuning**: Se você encontrar um estilo de vibrato que gosta muito, salve uma nota "modelo" em um arquivo UST de rascunho e use o Clipboard para copiá-la para seus projetos principais.
2. **Keybinds**: Decore os atalhos. `Ctrl+G` no UTAU abre as propriedades da nota; no OpenUtau, você pode customizar quase tudo no menu de configurações.
3. **Mantenha os Plugins Atualizados**: Plugins antigos podem parar de funcionar em versões novas do Windows. Sempre verifique o fórum UtaForum para as versões mais estáveis.
