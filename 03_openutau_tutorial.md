# ✦ Tutorial: OpenUtau - UTAU

O OpenUtau é uma recriação open source do programa UTAU de 2008, uma versão moderna com melhor compatibilidade, suporte a línguas e novas tecnologias. Recomendamos utilizar o OpenUtau nos dias atuais.

---

### Download e Instalação

1. Vá até o site oficial do OpenUtau.
2. Na aba **Home**, escolha o download para seu sistema (Windows, macOS ou Linux).
3. Crie uma pasta em um local de fácil acesso (ex: Área de Trabalho).
4. Extraia o conteúdo do arquivo `.zip` para dentro desta pasta.
5. Clique em `OpenUtau.exe` para abrir.

---

### Configuração Básica

As configurações ficam em **Ferramentas -> Preferências** (*Tools -> Preferences*).

1. **Reprodução (*Playback*):** Escolha o dispositivo de saída (seu fone ou caixa de som).
2. **Aparência (*Appearance*):** Selecione a língua e o tema do programa.
3. **Resamplers e Wavtools:** Na pasta onde o OpenUtau está instalado, existem as subpastas `Resamplers` e `Wavtools`. Coloque os motores de renderização baixados dentro delas para usá-los no programa.

---

### Controles da Interface

A interface é separada em:

1. **UI Principal:** Ferramentas e as faixas (*tracks*).
2. **Faixas (*Tracks*):** Onde você escolhe bancos de voz, phonemizers, resamplers e wavtools.
3. **Partes:** Blocos que guardam as notas que o banco canta.
4. **Piano Roll:** Clique duplo em uma Parte para abrir. Aqui você cria e edita notas musicais.

**Atalhos Úteis:**
- `T`: Abre/Fecha o menu de ajuda rápida.
- `Rolar`: Rolagem vertical.
- `Ctrl + Rolar`: Zoom horizontal.
- `Alt + Rolar`: Zoom vertical.
- `Enter`: Abre o menu de lírica da nota.
- `Ctrl + A`: Selecionar tudo.
- `Ctrl + Seta Cima/Baixo`: Mudar o tom (oitava).

---

### Instalando Voicebanks (Singers)

1. **Método 1 (Arrastar e Soltar):** Arraste o arquivo `.zip` ou `.rar` do banco de voz para qualquer lugar na interface do OpenUtau.
2. **Método 2 (Instalar Voz):** Vá em **Ferramentas -> Instalar voz**.

> [!WARNING]
> No Windows sem locale japonês, descompactar bancos manualmente pode corromper arquivos (mojibake). Instale sempre pela interface do OpenUtau.

---

### Fazendo seu Primeiro Cover

1. **Preparação:** Você precisará de um voicebank, um arquivo de sequência (UST/USTx/VSQx) e a instrumental da música.
2. **Importação:** Arraste o arquivo MIDI/UST/VSQx para a interface. Ou use **Arquivo -> Importar Faixas**.
3. **Configuração:** Clique no botão de voz na faixa e escolha seu cantor.
4. **Phonemizer:** Escolha o fonemizador adequado para o seu banco. Para bancos japoneses comuns, use: `JA VCV & CVVC Japanese presamp Phonemizer`.
5. **Formatação de Lírica:** Se a UST estiver em VCV e você usa um banco CV, selecione todas as notas → **Lírica -> (Japonês) VCV para CV**. Para remover caracteres extras, use **substituição geral da letra -> Remove non-hiragana**.
6. **Tuning Básico:**
   - **Curvas de tom:** Manipule os pontos com o mouse.
   - **Vibrato:** Clique no botão de vibrato na parte inferior da nota.
   - **Desenho de tom:** Use a ferramenta de desenho na barra superior para desenhar curvas livremente.
7. **Exportação:** Vá em **Arquivo -> Exportar arquivos para wav**.

---

### Flags no OpenUtau

1. Abra o **Menu de Expressões**.
2. Adicione uma nova expressão (+).
3. Dê nome e abreviação.
4. Troque o modo para "Opções".
5. Ative "É uma flag" e salve.
