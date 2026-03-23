# Tutorial Completo: Criando um Banco de Voz UTAU

## Índice
1. [Conceitos Básicos](#conceitos)
2. [Equipamento Necessário](#equipamento)
3. [Preparação para Gravação](#preparacao)
4. [Gravação das Amostras](#gravacao)
5. [Edição e Processamento](#edicao)
6. [Configuração no UTAU (oto.ini)](#configuracao)
7. [Testes e Ajustes Finais](#testes)

---

## 1. Conceitos Básicos {#conceitos}

### O que é UTAU?
UTAU é um software de síntese de voz desenvolvido por Ameya/Ayame onde você usa amostras de voz humanas para criar um cantor virtual. Cada som gravado é ativado pelas notas MIDI na interface do programa.

### Estrutura de um Banco de Voz
- **Pasta raiz**: Nome do seu UTAU (ex: `MeuUTAU/`)
- **Subpastas**: Organizadas por tipo de fonema (CV, VCV, CVVC, etc.)
- **Arquivo character.txt**: Metadados (nome, autor, imagem)
- **Arquivo oto.ini**: Configuração de timing de cada amostra

---

## 2. Equipamento Necessário {#equipamento}

### Mínimo Viável
- **Microfone**: Qualquer microfone que capte sua voz ou headset decente
- **Ambiente**: Quarto com tratamento acústico improvisado (cobertas, travesseiros) [TOTALMENTE OPCIONAL MAS RECOMENDADO]
- **Software**: Audacity (gratuito) ou Reaper (pago, mas livre), OREMO (facilita), Recstar (Tem para PC e celular)

### Setup Profissional
- Microfone condensador XLR (Neumann TLM 103, Audio-Technica AT2020)
- Interface de áudio (Focusrite Scarlett, Universal Audio)
- Isolamento acústico (placas de espuma, reflex filter)
- Pop filter (filtro anti-puff)

### Software Essencial
- **Gravação/Edição**: Audacity, Reaper, Adobe Audition, Cubase
- **Editor de oto**: SetParam (Japonês), Copaiba Lexicon PC (Brasileiro), COPAIBA TOUCH (Brasileiro, para celular), Laberu (Japonês) ou o próprio UTAU
- **Conversores**: fre:ac, foobar2000 (para formatar arquivos)

---

## 3. Preparação para Gravação

### Escolhendo o Método de Gravação

| Método | Descrição | Dificuldade | Qualidade |
|--------|-----------|-------------|-----------|
| **CV** (Consoante+Vogal) | "ka", "ki", "ku", "ke", "ko" | ⭐ Fácil | ⭐⭐ Básica |
| **VCV** (Vogal+Consoante+Vogal) | "a ka", "i ki", "u ku" | ⭐⭐⭐ Difícil | ⭐⭐⭐⭐⭐ Excelente |
| **CVVC** | Híbrido: "ka", "a k" | ⭐⭐ Média | ⭐⭐⭐⭐ Muito boa |

**Recomendação para iniciantes**: Comece com **CV** ou **CVVC Japonês** (só 46 fonemas básicos).

Vídeo recomendado:
[【UTAU / MÉTODOS】 - MEU UTAU PRECISA SER CV, VCV, CVVC OU VCCV? 【UTAU BRASIL】](https://www.youtube.com/watch?v=G8nvyPG3H74)

### Lista de Fonemas (Japonês CV)

Vogais: a, i, u, e, o

K: ka, ki, ku, ke, ko

S: sa, shi, su, se, so  

T: ta, chi, tsu, te, to

N: na, ni, nu, ne, no

H: ha, hi, fu, he, ho

M: ma, mi, mu, me, mo

Y: ya, yu, yo (yi e ye não existem)

R: ra, ri, ru, re, ro

W: wa, wo (wi e we arcaicos)

N (ん): n

Sites ótimos para pegar listas:

[JP CVVC Reclists](https://utau.felinewasteland.com/jp/cvvc)

[UTAUrials Downloads](https://utaurials.wordpress.com/downloads/)

### Script mental de Gravação:
Tenha em mente de manter constância durante a gravação, exemplo abaixo:

1. a (2 segundos)
2. i (2 segundos)
3. u (2 segundos)
4. e (2 segundos)
5. o (2 segundos)
6. ka (1 segundo)
7. ki (1 segundo)
...

---

## 4. Gravação das Amostras

### Configurações Técnicas
- **Sample Rate**: 44.1kHz ou 48kHz (consistente!)
- **Bitrate**: 16-bit ou 24-bit
- **Formato final**: .wav (.wav) - **mono** (fundamental!)
- **Volume**: -12dB a -6dB (evite clipping/distortion)

### Técnica de Performance (Opcional mas recomendado)

#### 1. Postura e Respiração
- Fique em pé se possível (melhor projeção)
- Microfone a 15-20cm da boca
- Use pop filter
- Respire fundo antes de cada fonema

#### 2. Articulação
- **Vogais**: Mantenha estável por 1-2 segundos (ex: "aaaaaa")
- **Consoantes**: Curtas e precisas
- **Transições**: CV precisa ter consoante audível (não "aaa" mas "kaaa")

#### 3. Consistência é a Chave
- Mantenha o mesmo tom (use afinador visual como o Melda MAnalyzer)
- Volume consistente entre todas as amostras
- Mesma distância do microfone
- Mesma energia/emotação em todas

### Checklist Durante Gravação
- [ ] Gravar em ambiente silencioso (sem vento, notebook fora da tomada, etc.)
- [ ] Fazer aquecimento vocal (escalas simples)
- [ ] Marcar erros imediatamente e regrava-los(evita retrabalho)

---

## 5. Edição e Processamento

### Passo 1: Limpeza de Ruído
1. **Capturar perfil de ruído**: Selecione trecho de silêncio → Efeito → Remover Ruído (Audacity)
2. **Aplicar moderadamente** (não exagere - preserva qualidade natural)
3. **Normalize**: Efeito → Normalizar para -3dB (pico)

### Passo 2: Corte Preciso
Para cada amostra:
- **Início**: Corte no silêncio absoluto (zero crossing)
- **Fim**: Deixe 200-500ms de silêncio após o som
- **Fade**: Aplique fade-in/fade-out suave (5-10ms) para evitar clicks

### Passo 3: Nomeação dos Arquivos (CRÍTICO!)
Use padrão consistente. Para CV Japonês:

a.wav
i.wav  
u.wav
e.wav
o.wav
ka.wav
ki.wav
ku.wav
ke.wav
ko.wav
...

**IMPORTANTE**: 
- Use apenas letras minúsculas
- Sem espaços (use underscore: `shi.wav`, não `shi .wav`)
- Formato: .wav (16-bit, 44100Hz, MONO)

### Passo 4: Organização de Pastas
MeuUTAU/
├── character.txt
├── readme.txt
├── oto.ini (será criado depois)
├── wav/
│   ├── a.wav
│   ├── i.wav
│   ├── ka.wav
│   └── ...
└── prefix.map (opcional)

---

## 6. Configuração no UTAU (oto.ini)

O `oto.ini` é o arquivo mais importante - ele diz ao UTAU onde começa e termina cada som.

### Estrutura do oto.ini
nome_arquivo.wav=alias,deslocamento,consonante,blank,pré-utição,velocidade

### Parâmetros Explicados

| Parâmetro | Descrição | Onde Ajustar |
|-----------|-----------|--------------|
| **Offset** (Deslocamento) | Início do som até o início da vogal | Início do arquivo |
| **Consonant** | Duração fixa da consoante | Área cinza (não estica) |
| **Cutoff/Blank** | Fim do som útil | Final do arquivo |
| **Preutterance** | Tempo antes do ataque da nota | Linha verde |
| **Overlap** | Sobreposição com nota anterior | Linha azul |

### Configuração por Tipo

#### Para CV Simples:
ka.wav=ka,100,40,-500,80,30

- Offset: 100ms (silêncio inicial)
- Consonant: 40ms (duração do "k")
- Blank: -500ms (corte 500ms antes do fim)
- Preutterance: 80ms (quando começa o som)
- Overlap: 30ms (transição suave)

#### Para VCV (mais complexo):
ka.wav=- ka,120,50,-600,90,40

Nota o espaço antes do "ka" - indica transição da vogal anterior.

### Ferramentas de Edição de oto

1. **SetParam** (Recomendado - Japonês)
   - Interface visual intuitiva
   - Atalhos de teclado eficientes
   - Preview em tempo real

2. **VLabeler** (Moderno, Inglês)
   - Open source
   - Suporte a múltiplos formatos
   - Visualização espectrográfica

3. **UTAU Nativo**
   - Menu: Tools → Voice Bank Settings
   - Mais lento, mas funcional para ajustes finos

### Dicas para oto
- **Regra dos 20ms**: Overlap geralmente é 40-50% do preutterance
- **Consonantes curtas**: "k", "t", "p" = 30-50ms
- **Consonantes longas**: "s", "sh", "f" = 60-100ms
- **Teste musical**: Configure alguns fonemas, teste em uma música, ajuste, repita

---

## 7. Testes e Ajustes Finais {#testes}

### Criação do character.txt
name=MeuUTAU
author=SeuNome
image=icon.bmp (100x100px recomendado)
sample=sample.wav (demo voice)

### Testes no UTAU
1. **Teste de escala**: Crie notas C4, D4, E4, F4, G4, A4, B4, C5 com "a", "i", "u", "e", "o"
2. **Teste de transição**: "ka-ki-ku-ke-ko" para verificar conectividade
3. **Teste de frase**: Uma frase simples como "konnichiwa" ou "sakura"

### Problemas Comuns e Soluções

| Problema | Causa | Solução |
|----------|-------|---------|
| **Clicks/Pops** | Cortes no meio da onda | Editar em zero-crossing |
| **Volume irregular** | Gravação inconsistente | Normalizar todos os arquivos |
| **Notas curtadas** | Blank muito agressivo | Aumentar valor do cutoff (menos negativo) |
| **Consoantes estridentes** | Consonant muito longo | Reduzir valor da consoante |
| **Voz robótica** | Overlap muito curto | Aumentar overlap |

### Renderização Final
- Use **resamplers externos**: Doppeltler (recomendado), TIPS, Moresampler, etc
- **Flags comuns**: `g-5` (ajusta tom), `B0` (limpa breath).

[Lista de flags (Tutorial Brasileiro)](https://utaurials.wordpress.com/2017/10/21/raise-the-flags/)

[Como cada resampler soa](https://utau.us/resample.html)

[Baixe este pacotes com todos os resamplers](https://archive.org/details/utau-resampler)


---

## Dicas Avançadas

### Gravação VCV
Para VCV japonês completo, você precisa gravar:
- 5 vogais isoladas
- 5 vogais de entrada (para início de frase)
- 46 sílabas CV × 5 contextos = 230 amostras aproximadamente

Use o método **Harus**: grave tudo num único arquivo .wav longo e use ferramentas como **AutoCVVC** ou **Phonemizer VCV & CVVC JA** no OpenUtau

### Multipitch (Vários tons)
Grave em 3 tons diferentes (ex: C3, F3, B3):

MeuUtau/
├── C3/
├── F3/
└── B3/

Configure `prefix.map` para alternar automaticamente baseado na nota MIDI.

### Aliases
Crie aliases para fonemas alternativos:

ka.wav=ka,100,40,-500,80,30
ka.wav=ca,100,40,-500,80,30  (alias para "ca")

---

## Recursos Úteis

### Downloads
- **UTAU**: http://utau2008.xrea.jp/ (Japonês)
- **SetParam**: Disponível em fóruns UTAU
- **VLabeler**: GitHub (sdercolin/vlabeler)

### Comunidades (para tirar dúvidas)
- Reddit: r/UTAU
- Discord: [Mansão UTAU Brasil](https://disboard.org/pt-br/server/1386444730652688524)
- Forums: UTAU Wiki (em inglês)

### Vídeos Tutoriais Recomendados
- Canal **CZloid** (tutoriais detalhados em inglês)
- Canal **GHPZ** (técnicas avançadas de oto)

---

## Checklist Final

Antes de publicar seu UTAU:
- [ ] Todos os arquivos em MONO, 44.1kHz, 16-bit .wav
- [ ] Oto.ini configurado para todos os fonemas
- [ ] Testado em uma música completa
- [ ] character.txt com nome e autor
- [ ] Ícone em 100x100px (formato BMP)
- [ ] Arquivo sample.wav (demo curta)
- [ ] readme.txt com termos de uso
- [ ] Backup em nuvem dos arquivos originais (não editados)

---

