## Estrutura Básica

O arquivo é simplesmente um texto com parâmetros em linhas separadas. Coloque na pasta raiz do seu UTAU.

**Exemplo mínimo funcional:**
```
name=MeuUTAU
author=SeuNome
```

Com isso já aparece no UTAU, mas vamos deixar mais arrumado.

---

## Parâmetros Disponíveis

| Parâmetro | Obrigatório | Descrição |
|-----------|-------------|-----------|
| **name** | Sim | Nome do cantor que aparece na lista do UTAU |
| **author** | Recomendado | Seu nome/apelido |
| **image** | Opcional | Ícone visual (100x100px ideal) |
| **sample** | Opcional | Arquivo de demonstração de voz |

---

## Exemplos Práticos

**Para um UTAU básico:**
```
name=VIICTOR
author=xiao
image=icon.bmp
sample=demo.wav
```

**Para um UTAU comercial/serio:**
```
name=VIICTOR CATALINA
author=POMAR LTS
image=character.bmp
sample=sample_v2.wav
```

## Especificações Técnicas

**Formato do arquivo:**
- Encoding: **Shift-JIS** (UTAU original japonês) ou **UTF-8** (versões modernas/inglês)
- Extensão: `.txt` (não .doc, não .rtf)
- Localização: Raiz da pasta do voicebank
- Imagem do ícone em `.bmp`

**Imagem (image):**
- Tamanho recomendado: **100x100 pixels**
- Formatos aceitos: **.png** ou **.bmp** (.bmp é melhor para compatibilidade e retrocompatibilidade)
- Nome simples, sem espaços: `icon.bmp`, `char.bmp`, `img.bmp`

**Sample (sample):**
- Formato: **WAV** (mono, 44.1kHz, 16-bit, mesma regra das amostras)
- Duração: **2-5 segundos** (frase curta característica ou trecho de uma música)

---

## Exemplo Completo Real

Coloque este conteúdo num bloco de notas e salve como `character.txt`:

```
name=Hanako VCV
author=Mariana Santos
image=hanako_icon.bmp
sample=intro.wav
```

Estrutura de pastas correspondente:
```
HanakoVCV/
├── character.txt
├── hanako_icon.bmp      (imagem 100x100)
├── intro.wav            (voz dizendo "Hanako desu!")
├── readme.txt           (instruções opcionais)
└── wav/
    ├── a.wav
    ├── i.wav
    └── ...
```

---

## Dicas Importantes

**Encoding:** Se o UTAU mostrar seu nome com caracteres estranhos (mojibake), converta o arquivo para Shift-JIS usando Notepad++ ou similar. Versões novas do UTAU (como o OpenUTAU) lidam bem com UTF-8.

**Caminhos:** Não use caminhos absolutos tipo `C:/Users/...`. Só o nome do arquivo (`icon.bmp`) porque o UTAU procura na pasta raiz automaticamente.

**Atualização:** Sempre mude o nome se for uma versão nova (ex: `name=MeuUTAU V2`), senão o UTAU sobrescreve o antigo na lista ou confunde os arquivos.

---
