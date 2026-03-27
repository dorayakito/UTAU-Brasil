# [!] OTO.INI NÍVEL EXPERT
## Configuração, Abstração, Parâmetros, Interação com Resamplers e Ferramentas

> **Nota 1:** Este documento pressupõe familiaridade com terminologia de síntese de voz, fonética e o ecossistema UTAU. Termos técnicos de T.I. e fonética são utilizados sem simplificação. Aqui o objetivo é ter um espectro mais amplo e também profundidade técnica.

> **Nota 2:** **Ninguém é obrigado a saber de tudo isso**, isso aqui é para vocês entenderam o quão complexo é e como acontece por trás das telas

> **Nota 3:** Caso você não tenha interesse em informações técnicas e complexas, pule este markdown

---

## Índice

1. [O que é o oto.ini](#1-o-que-é-o-otoini)
2. [Anatomia do arquivo oto.ini](#2-anatomia-do-arquivo-otoini)
3. [Os seis parâmetros fundamentais](#3-os-seis-parâmetros-fundamentais)
   - [Filename (arquivo de referência)](#31-filename-arquivo-de-referência)
   - [Alias](#32-alias)
   - [Offset](#33-offset)
   - [Consonant (Consonant Velocity)](#34-consonant-consonant-velocity)
   - [Cutoff](#35-cutoff)
   - [Preutterance](#36-preutterance)
   - [Overlap](#37-overlap)
4. [Interação entre parâmetros -  o pipeline completo](#4-interação-entre-parâmetros--o-pipeline-completo)
5. [Interação com Resamplers](#5-interação-com-resamplers)
   - [resampler.exe (padrão UTAU)](#51-resamplerexe-padrão-utau)
   - [fresamp](#52-fresamp)
   - [moresampler / TIPS](#53-moresampler--tips)
   - [tn_fnds e WORLD-based resamplers](#54-tn_fnds-e-world-based-resamplers)
   - [doppeltler e variantes](#55-doppeltler-e-variantes)
6. [Ferramentas de configuração](#6-ferramentas-de-configuração)
   - [SetParam](#61-setparam)
   - [Copaiba NEO](#62-copaiba-neo)
   - [vLabeler](#63-vlabeler)
   - [oremo](#64-oremo)
   - [UTAU-Synth (macOS)](#65-utau-synth-macos)
   - [Edição manual](#66-edição-manual)
7. [Organização de aliases](#7-organização-de-aliases)
8. [Configuração por tipo de voicebank](#8-configuração-por-tipo-de-voicebank)
   - [CV (Consonant-Vowel)](#81-cv-consonant-vowel)
   - [VCV (Vowel-Consonant-Vowel)](#82-vcv-vowel-consonant-vowel)
   - [CVVC](#83-cvvc)
   - [VCCV (Arpasing)](#84-vccv-arpasing)
   - [CVC](#85-cvc)
   - [Multipitch e Expression banks](#86-multipitch-e-expression-banks)
9. [Otimização avançada](#9-otimização-avançada)
10. [Casos especiais e edge cases](#10-casos-especiais-e-edge-cases)
11. [Depuração e diagnóstico](#11-depuração-e-diagnóstico)
12. [Referência rápida](#12-referência-rápida)

---

## 1. O que é o oto.ini

O `oto.ini` é o arquivo de configuração central de um voicebank UTAU. Ele funciona como um mapeamento entre amostras de áudio raw (arquivos `.wav`) e os parâmetros que instruem o motor de síntese sobre **como** segmentar, temporalizar e interpolar cada fonema dentro de cada arquivo de gravação.

Do ponto de vista de arquitetura de software, o `oto.ini` atua como uma camada de abstração entre o conteúdo fonético bruto das gravações e o pipeline de síntese do UTAU (que compreende: note scheduling → resampler invocation → wavtool concatenation). Sem um `oto.ini` corretamente configurado, o resampler não tem como saber onde começa ou termina a parte útil de um sample, resultando em artefatos sonoros, timing incorreto ou falha completa na renderização.

O arquivo é um texto plano em formato INI sem seções, onde cada linha representa um **sample entry** -  uma tupla de seis campos delimitados por vírgulas, com o nome do arquivo precedendo o `=`.

**Estrutura de diretório típica:**

```
voicebank/
├── oto.ini              ← configuração da raiz
├── あ.wav
├── い.wav
├── ...
├── oto_ini/             ← subdiretórios opcionais
│   ├── extra/
│   │   ├── oto.ini      ← oto.ini de subdiretório (paths relativos ao subdir)
│   │   └── *.wav
```

Cada subdiretório pode conter seu próprio `oto.ini`. O UTAU resolve paths recursivamente a partir da raiz do voicebank, e aliases definidos em subdiretórios coexistem com os da raiz, sem conflito por padrão -  exceto em caso de colisão de alias, onde o arquivo mais profundo na hierarquia geralmente prevalece (comportamento dependente de implementação do plugin/front-end).

---

## 2. Anatomia do arquivo oto.ini

### Formato da linha

```
filename.wav=alias,offset,consonant,cutoff,preutterance,overlap
```

Exemplo concreto:

```
あ.wav=あ,10,120,−400,80,40
か.wav=か,5,80,−350,60,30
sa.wav=- sa,200,90,−380,70,35
```

### Encoding

O `oto.ini` deve ser salvo em **Shift-JIS** (CP932) para compatibilidade com o UTAU original (Windows). Ferramentas modernas como o vLabeler e o OpenUTAU suportam UTF-8 com BOM, mas voicebanks destinados ao UTAU original devem respeitar Shift-JIS para evitar corrupção de aliases contendo caracteres CJK ou diacríticos.

### Comentários

O formato padrão não possui sintaxe oficial para comentários. Algumas ferramentas toleram linhas começando com `;` ou `#`, mas isso **não é portável** -  o UTAU pode tratar essas linhas como entradas mal formadas.

### Múltiplos aliases para o mesmo arquivo

Um único arquivo `.wav` pode ter múltiplas entradas no `oto.ini`:

```
あ.wav=あ,10,120,−400,80,40
あ.wav=- あ,10,120,−400,80,40
あ.wav=R あ,500,50,−200,30,20
```

Isso é a base de voicebanks CVVC e VCV, onde o mesmo arquivo de gravação expõe múltiplos fonemas em diferentes posições temporais.

---

## 3. Os seis parâmetros fundamentais

Todos os valores de tempo são em **milissegundos (ms)**. O cutoff negativo indica posição relativa ao final do arquivo; valores positivos (menos comuns) indicam posição absoluta a partir do offset.

### Diagrama de disposição temporal

```
Arquivo WAV:
|<————————————————————— duração total do arquivo ————————————————————>|
|                                                                      |
|    [offset]                                          [cutoff]        |
|      |                                                   |           |
|      |←— fixed part —→|←—— consonant ——→|←—— vowel ——→|            |
|      |                 |                 |              |            |
|      |←————————————————|———— overlap ————|              |            |
|      |                 |←— preutterance →|              |            |
|      |                                                              |
| silêncio/ruído |       parte utilizável do sample       | silêncio  |
```

---

### 3.1 Filename (arquivo de referência)

O campo antes do `=` é o nome do arquivo `.wav` referenciado. O path é relativo ao diretório onde o `oto.ini` reside.

**Implicações técnicas:**
- O UTAU carrega o arquivo em memória antes de invocar o resampler.
- Arquivos ausentes geram erro silencioso na maioria das implementações -  o note é simplesmente omitido da renderização.
- Nomes de arquivo case-sensitive em sistemas Unix; insensitive no Windows. Isso causa bugs ao portar voicebanks entre plataformas.

---

### 3.2 Alias

O alias é a **chave de lookup** usada pelo UTAU para associar notas no UST (UTAU Sequence Text) ao sample correspondente. Quando o motor de síntese processa uma nota, ele consulta a tabela de aliases do voicebank (construída na inicialização a partir do `oto.ini`) e localiza o entry correspondente.

**Características:**
- O alias é case-sensitive em algumas implementações e insensitive em outras.
- Espaços em branco no início e no final são geralmente ignorados.
- O alias pode ser qualquer string válida em Shift-JIS/UTF-8.
- O prefixo de alias (ex: `- `, `* `, `A3 `) é amplamente utilizado para pitch suffixes em bancos multipitch.

**Alias especiais:**
- `R` -  convencionalmente usado para amostras de respiração (breath).
- `_` -  alias de silêncio/pausa em algumas implementações.
- Sufixos como `↑`, `↓`, `強`, `弱` são usados para expression layers.

**Resolução de alias em cascata:**

Quando o UTAU não encontra um alias exato, ele pode tentar fallback por prefixo de pitch (ex: `A3 か` → `か`) dependendo da configuração do voicebank (`character.txt`, flags de prefix map). O comportamento exato é implementação-dependente.

---

### 3.3 Offset

**O que é:** O offset marca o **início da região utilizável** do arquivo WAV, em milissegundos a partir do início absoluto do arquivo.

**O que representa fisicamente:** Tudo antes do offset é descartado pelo resampler. Isso permite que as gravações incluam silêncio de pré-attack, ruído de fundo do microfone ou transientes indesejados no início.

**Interação com o resampler:**
- O resampler recebe o offset como parâmetro e faz seek direto para essa posição antes de começar a processar amostras PCM.
- Em termos de implementação: `sample_start = (offset_ms / 1000.0) * sample_rate`.
- Offsets muito grandes que ultrapassem a preutterance resultam em underflow, geralmente manifestando como glitch ou silêncio no início do fonema.

**Boas práticas:**
- Apontar logo antes do onset da consoante (para sílabas CV).
- Para VCV, apontar no início da vogal antecedente.
- Evitar offset zero se o arquivo tiver silêncio no início -  isso desperdiça banda de lookup mas não quebra a síntese.
- Um valor de 10–30ms de margem antes do ataque real é aceitável e evita o corte de transientes.

**Configuração no SetParam:** Barra azul à esquerda (borda esquerda da região configurável).

---

### 3.4 Consonant (Consonant Velocity)

**O que é:** O parâmetro `consonant` (também chamado de "fixed region" ou "consonant velocity" em algumas ferramentas) marca o **fim da região fixa** em relação ao offset, em milissegundos.

**Representação:** `offset + consonant` = posição absoluta do fim da região fixa no arquivo.

**O que representa fisicamente:** A região entre o offset e o consonant é a **região fixa** -  uma porção do sample que **não é stretched pelo resampler**, independentemente do tempo de nota requerido. Ela contém tipicamente:
- A consoante de ataque (onset consonant).
- Transientes de ataque da vogal.
- Qualquer material temporal-específico que não deve ser stretching.

**Interação com o resampler -  stretching behaviour:**

O resampler divide o sample em duas regiões:

| Região | Trecho no arquivo | Comportamento |
|--------|-------------------|---------------|
| Fixed region | `[offset, offset + consonant]` | Copiada 1:1 sem pitch/time modification |
| Stretchable region | `[offset + consonant, end_point]` | Sujeita a time-stretching e pitch-shifting |

Isso é análogo ao conceito de **attack preservation** em samplers como o Kontakt (onde o envelope de ataque é separado do sustain para evitar pitch-artifacts em transientes). No UTAU, a fixed region garante que consoantes percussivas (oclusivas, fricativas, africadas) não sofram artefatos de phase vocoder.

**Implicações práticas:**
- Se `consonant` for muito curto: parte da consoante entra na zona de stretching → artefatos em notas longas ou velocidades baixas.
- Se `consonant` for muito longo: o resampler tenta stretch uma região já fora da consoante → a vogal fica com timbre alterado em notas muito curtas.
- Para fonemas puramente vocálicos (bare vowels em VCV), o consonant pode ser muito pequeno (5–20ms) ou mesmo zero em algumas implementações.

**Configuração no SetParam:** Barra verde (define a borda direita da fixed region).

---

### 3.5 Cutoff

**O que é:** O cutoff define o **ponto de término** do sample utilizável. Ele é expresso de forma diferente das demais:

- **Valor positivo:** posição relativa ao **fim do arquivo**. Ex: `−400` significa "400ms antes do final do arquivo".
- **Valor negativo:** posição absoluta a partir do offset (mais usado em VCV japonês clássico).

**Representação:**
```
end_point = file_duration + cutoff          (quando cutoff > 0)
end_point = offset + cutoff                 (quando cutoff < 0)
```

**O que representa fisicamente:** Descarta o silêncio, ruído ou decaimento indesejado ao final do arquivo. A região `[end_point, file_end]` é ignorada.

**Interação com o resampler:**

O resampler usa `end_point` como o limite máximo de `where` ele pode buscar samples para stretch. Se a nota requerida for mais longa do que `end_point - (offset + consonant)`, o resampler loopa ou extende a região stretchable (esticável) -  dependendo do resampler e das flags de loop configuradas.

**Boas práticas:**
- Cortar antes do decaimento natural da vogal (para bancos que fazem crossfade entre notas).
- Cortar antes de qualquer clique, ruído de microfone ou respiração no final do arquivo.
- Em bancos VCV, o cutoff geralmente cai dentro da região de overlap da vogal seguinte.

**Configuração no SetParam:** Barra à direita (limite direito da região utilizável). Valores negativos são mostrados com seta apontando para a esquerda a partir do final.

---

### 3.6 Preutterance

**O que é:** A preutterance define **quanto tempo antes do beat** o sample começa a ser reproduzido. Em outras palavras: é o lookahead do resampler em relação ao timestamp da nota no UST.

**Representação:** Posição absoluta em ms a partir do offset. `preutterance = 70` significa que o resampler começa 70ms antes do ponto de beat da nota.

**O que representa fisicamente:** A preutterance compensa o fato de que a consoante ocorre **antes** do beat percebido. Em fonética, o onset de uma sílaba (consoante + transiente de vogal) antecede a percepção do beat. Se a preutterance for zero, a consoante ocorre no beat, fazendo a vogal parecer atrasada.

**Visualização temporal:**

```
Timeline do UST:
                        |← beat da nota →|
                        ↑
                        0ms (timestamp da nota)

Timeline do sample:
|←— offset —→|←— preutterance —→|←— resto da vogal —→|←— cutoff →|
              ↑                  ↑
          começa aqui         beat da nota (alinhado ao timestamp)
```

**Interação com o resampler e wavtool:**

1. O UTAU calcula `note_start = note_timestamp - preutterance`.
2. O wavtool é invocado para escrever o sample na timeline de output começando em `note_start`.
3. O resampler recebe a duração total necessária: `required_duration = note_duration + preutterance`.

Isso significa que preutterance afeta diretamente a **quantidade de material que o resampler precisa gerar** -  uma preutterance grande em notas curtas pode resultar em stretching extremo da região vocálica.

**Boas práticas:**
- Para consoantes oclusivas (p, b, t, d, k, g): preutterance logo após o burst de release -  geralmente 50–90ms.
- Para consoantes fricativas (s, sh, f): preutterance no meio da fricativa -  60–100ms.
- Para vogais bare (em VCV): muito pequena -  20–50ms.
- Para sons nasais (n, m): 40–70ms.

**Configuração no SetParam:** Barra vermelha vertical (linha de corte que indica onde o beat cai dentro do sample).

---

### 3.7 Overlap

**O que é:** O overlap define o tamanho da **região de crossfade** entre o sample atual e o sample anterior. É medido em ms a partir do offset.

**Representação:** `overlap` deve sempre ser menor que `preutterance`. O wavtool usa o overlap para fazer um fade linear (ou curva de envelope configurável) entre o tail do sample anterior e o head do sample atual.

**O que representa fisicamente:** Em fala natural, as sílabas não são discretas -  há coarticulação. O overlap simula (imperfeitamente) essa transição suave. Sem overlap, há um corte abrupto entre sílabas, produzindo cliques e transições não naturais.

**Interação com wavtool:**

O wavtool implementa o crossfade como:

```
output[t] = prev_sample[t] * (1 - fade_t) + curr_sample[t] * fade_t
```

onde `fade_t` vai de 0 a 1 ao longo da duração do overlap. O ponto de início do crossfade no sample atual é `offset` (início), e o ponto onde `fade_t = 1` é `offset + overlap`.

**Implicações:**
- Overlap muito pequeno (< 10ms): transição abrupta, especialmente perceptível em vogais.
- Overlap muito grande: transição "lamacenta", onde dois fonemas coexistem por tempo demais -  especialmente problemático em consoantes plosivas.
- O overlap **deve ser menor que a preutterance** -  se `overlap >= preutterance`, o wavtool pode produzir comportamento indefinido (mixagem fora da região prevista).

**Relação overlap/preutterance:**

A regra geral é:
```
overlap ≈ preutterance / 2    (para fonemas vocálicos)
overlap ≈ preutterance / 3    (para consoantes plosivas)
```

**Configuração no SetParam:** Linha verde dentro da região azul de overlap (a área sombreada à esquerda da preutterance).

---

## 4. Interação entre parâmetros -  o pipeline completo

### 4.1 O pipeline de síntese UTAU

O UTAU é um sistema de síntese concatenativa com time-domain e frequency-domain processing. O pipeline completo para uma nota é:

```
UST Note
    ↓
[UTAU Engine]
    ├── Lê parâmetros da nota (pitch, velocity, flags, envelope)
    ├── Consulta oto.ini → obtém (offset, consonant, cutoff, preutterance, overlap)
    ├── Calcula duração total requerida = note_length + preutterance
    ↓
[Resampler] (ex: resampler.exe, fresamp, moresampler)
    ├── Recebe: input.wav, pitch_target, flags, start_ms, consonant_ms, end_ms, vel%, volume%
    ├── Lê PCM do input.wav a partir de start_ms (= offset)
    ├── Mantém [offset, offset+consonant] intacto (fixed region)
    ├── Time-stretches/pitch-shifts [offset+consonant, end_ms] para atingir duração requerida
    └── Escreve output em arquivo temporário
         ↓
[wavtool] (ex: wavtool.exe, wavtool4vcv, mf)
    ├── Recebe: output.wav, destino_na_timeline, preutterance, overlap
    ├── Faz seek na timeline de output para (timestamp - preutterance)
    ├── Aplica crossfade com sample anterior por (overlap) ms
    └── Escreve sample na timeline de render final
         ↓
[Output WAV final]
```

### 4.2 Como velocity interage com o oto.ini

O parâmetro de **velocity** de uma nota no UTAU modifica implicitamente como o resampler trata a preutterance e a consonant region. Alta velocity = mais tempo de consoante relativo ao beat; baixa velocity = consoante comprimida.

Matematicamente, o UTAU implementa velocity como um modificador de preutterance:

```
effective_preutterance = preutterance * (velocity / 100.0)
```

Isso significa que com velocity 50%, a preutterance efetiva é metade do valor configurado no oto.ini. Em samples de consoante longa, baixa velocity pode cortar parte da consoante, produzindo artefatos.

### 4.3 O envelope de nota e sua relação com cutoff

O UTAU suporta um envelope de amplitude por nota (attack, decay, sustain, release). O release do envelope interage diretamente com o cutoff:

- Se o release for mais longo do que o material disponível após a preutterance, o resampler precisará loop ou extender o sample até o cutoff.
- Se o cutoff for muito conservador (muita margem), o resampler tem mais material para release.

---

## 5. Interação com Resamplers

### 5.1 resampler.exe (padrão UTAU)

O resampler padrão do UTAU usa **time-domain resampling** com interpolação cúbica para pitch e um algoritmo de stretch simples (próximo de OLA -  Overlap-Add). É adequado para bancos simples mas produz artefatos em stretching extremo.

**Parâmetros do oto.ini que mais importam:**
- `consonant`: deve ser preciso -  o resampler não tem sofisticação suficiente para compensar erros grandes.
- `cutoff`: deve deixar material suficiente para notas longas, pois não há looping inteligente.

**Flags suportadas relevantes:**
- `g±N` -  gender factor (formant shift)
- `t±N` -  tune correction
- `A` -  BRE (breath noise addition)
- `H` -  harmonics enhancement

### 5.2 fresamp

fresamp usa **frequency-domain processing** (FFT-based phase vocoder) para time-stretching, com melhor preservação de formante do que o resampler padrão.

**Interação com oto.ini:**
- A fixed region (consonant) é **especialmente crítica** no fresamp: se ela incluir material periódico (vogal), o phase vocoder pode introduzir phase artifacts na junção entre fixed e stretchable region.
- O fresamp implementa seu próprio algoritmo de lookahead, onde a preutterance influencia a janela de análise FFT inicial.

**Parâmetros críticos:**
- `consonant`: deve terminar **precisamente** na junção consoante/vogal. Erros de 10–15ms já são audíveis.
- `overlap`: deve ser grande o suficiente para o phase vocoder ter uma janela de análise adequada -  mínimo de 30ms recomendado.

**Flags específicas:**
- `B` -  breathiness injection via noise mixing
- `P` -  peak normalization
- `e` -  envelope follow mode

### 5.3 moresampler / TIPS

O moresampler (e sua variante TIPS) usa um pipeline híbrido: análise espectral com WORLD vocoder para amostras vocálicas e tratamento direto para transientes.

**Interação com oto.ini:**

O moresampler lê o `oto.ini` mas também analisa o espectro do arquivo WAV diretamente para refinar automaticamente alguns parâmetros internamente:

- Detecta automaticamente o F0 (fundamental frequency) do sample.
- Ajusta a janela de análise WORLD com base no consonant region.
- Usa o overlap para calcular a janela de crossfade espectral.

**Parâmetros mais críticos:**
- `offset`: deve começar antes de qualquer material vocálico -  o WORLD vocoder precisa de um período de análise de estabilização.
- `consonant`: o moresampler usa como hint para onde começa o material periódico -  configuração ruim pode fazer o vocoder tentar analisar o espectro de uma consoante fricativa como se fosse voz periódica.

**Flags relevantes:**
- `Mb` -  modo base (análise WORLD padrão)
- `Mt` -  mode tentative (análise heurística com fallback)
- `Re` -  render engine selection

### 5.4 tn_fnds e WORLD-based resamplers

tn_fnds é um resampler baseado integralmente no WORLD vocoder (Harvest/CheapTrick/D4C). O pipeline de analysis/synthesis é completamente separado em:

1. **F0 estimation** (Harvest algorithm): estima a curva de pitch frame a frame.
2. **Spectral envelope estimation** (CheapTrick): estima o espectro vocal suavizado.
3. **Aperiodicity estimation** (D4C): estima a componente não-periódica (breathy/noisy).
4. **Resynthesis**: gera saída com novo F0, preservando envelope espectral e aperiodicidade.

**Interação com oto.ini:**

O tn_fnds tem comportamento diferente na fixed region:
- A fixed region **não é simplesmente copiada** -  ela ainda passa por WORLD analysis, mas com F0 preservation (sem pitch shift dentro dela).
- O boundary entre fixed e stretchable region deve ser em um **frame zero-crossing** do F0 -  caso contrário, há descontinuidade de fase espectral na junção.

**Recomendações de oto.ini para WORLD resamplers:**
- `consonant`: terminar em um ponto de relativa estabilidade espectral.
- `overlap`: deve ser múltiplo do período fundamental aproximado (ex: para F3 = ~220Hz, período ≈ 4.5ms, overlap de ≈ 27ms = 6 períodos).
- `preutterance`: mais crítica do que em resamplers OLA -  o WORLD precisa de material de análise suficiente.

### 5.5 doppeltler e variantes

doppeltler e variantes similares usam **PSOLA (Pitch-Synchronous Overlap-Add)**, que opera em epochs (marcadores de pitch period) em vez de janelas fixas de FFT.

**Interação com oto.ini:**
- A boundary da consonant region afeta onde o PSOLA começa a marcar epochs -  se cair no meio de um transiente não-periódico, o algoritmo de detecção de epoch pode falhar.
- O overlap afeta o tamanho de janela de cada epoch synthesis.

---

## 6. Ferramentas de configuração

### 6.1 SetParam

SetParam é a ferramenta de configuração de oto.ini mais amplamente utilizada no ecossistema UTAU. Desenvolvida originalmente por Nanahira, com versões aprimoradas por vários contribuidores da comunidade.

**Interface e fluxo de trabalho:**

```
Painel de waveform:
[offset] ←barra azul→ [consonant] ←área verde→ [preutterance] ←linha vermelha→ [cutoff] ←barra vermelha→
                        ↑                         ↑
                    barra verde               barra laranja (overlap)
```

**Funcionalidades principais:**

- **Batch processing:** Permite aplicar um template de parâmetros a múltiplos samples selecionados. Útil para ajustar offset de todo um banco de uma vez.
- **Copy parameters:** Copiar os parâmetros de uma entrada para outras (por grupo de fonema).
- **Auto-oto:** SetParam possui uma função de geração automática de oto.ini que tenta detectar onsets de consoante por análise de energia. A qualidade varia significativamente por tipo de fonema.
- **Playback:** Permite ouvir o sample com os parâmetros aplicados simulando o comportamento do resampler.
- **Zoom:** Zoom temporal para ajuste fino de milissegundos.

**Workflow recomendado no SetParam:**

1. Abrir o voicebank (File → Open Voicebank Directory).
2. Se o banco não tiver `oto.ini`, gerar um template com Auto-oto ou criar entries manualmente.
3. Ordenar entries por alias ou filename.
4. Para cada entry:
   a. Ouvir o sample raw.
   b. Ajustar offset para excluir silêncio/ruído inicial.
   c. Ajustar consonant para englobar apenas a consoante (onset).
   d. Ajustar cutoff para excluir silêncio/decaimento final.
   e. Ajustar preutterance para alinhar o beat ao centro da vogal.
   f. Ajustar overlap para ~50% da preutterance.
5. Ouvir com "Play Wav" e testar com UST de referência.
6. Salvar.

**Atalhos de teclado relevantes:**

| Ação | Atalho |
|------|--------|
| Próximo entry | `→` ou `Page Down` |
| Entry anterior | `←` ou `Page Up` |
| Play | `Space` |
| Copiar parâmetros | `Ctrl+C` |
| Colar parâmetros | `Ctrl+V` |
| Auto-oto selecionado | `F5` |

**SetParam Extended / SetParam Beta:**

Versões extendidas adicionam:
- Suporte a visualização de espectrograma.
- Configuração de prefix map integrada.
- Export para formatos alternativos (vLabeler project, etc.).

### 6.2 Copaiba NEO

Copaiba NEO (também referenciado no repositório como *Copaiba Mini*) é um editor de `oto.ini` multiplataforma, open-source, escrito em **Rust**. Desenvolvido por studiopomar/dorayakito, o projeto foca em performance, precisão sub-milissegundo e uma experiência de usuário moderna comparável a ferramentas de DAW profissional.

**Repositório:** [https://github.com/studiopomar/Copaiba-NEO](https://github.com/studiopomar/Copaiba-NEO)

**Stack tecnológico:**

| Componente | Biblioteca |
|-----------|-----------|
| UI | egui + eframe |
| Áudio | rodio + hound |
| DSP | realfft |
| Linguagem | Rust |

A escolha de Rust proporciona latência mínima na renderização de waveform/espectrograma e ausência de GC pauses -  características relevantes ao navegar rapidamente por bancos com centenas de entries.

**Como executar:**

```bash
# Requer Rust toolchain (https://rustup.rs/)
git clone https://github.com/studiopomar/Copaiba-NEO
cd Copaiba-NEO
cargo run
```

Binários pré-compilados estão disponíveis na aba Releases do repositório.

**Visualização de áudio:**

- **Espectrograma** com escala de frequência logarítmica e interpolação bicúbica -  permite visualizar diretamente a estrutura formântica e identificar com precisão o onset de consoantes fricativas e a junção consoante/vogal.
- **Waveform** com renderização por spline contínuo e envelopes de pico -  mais precisa visualmente do que a exibição de waveform retangular do SetParam clássico.
- **Minimap interativo** para navegação grossa por bancos extensos, com marcadores que fazem snap ao peak de amplitude mais próximo.

**Edição de precisão:**

- Edição de parâmetros com precisão **sub-milissegundo**, diretamente por drag ou por input numérico.
- Suporte a três perfis de teclado:
  - **Copaiba (QWERT):** layout de atalhos nativo da ferramenta.
  - **SetParam (F1–F5):** compatibilidade com usuários migrados do SetParam.
  - **Custom:** mapeamento livre.
- **SRP (Snap Relative to Preutterance):** ao mover o offset, o snap mantém a relação relativa com a preutterance -  útil para ajustar o início do sample sem desalinhar o beat.
- **SRnA (Snap Relative to Nothing):** movimento livre sem snap automático.
- **Grid estilo Excel** para edição em massa: multi-seleção com `Ctrl`/`Shift`, navegação por teclado, edição de campo diretamente na célula.

**Plugins e automação integrados:**

| Plugin | Função |
|--------|--------|
| **Alias Sorter** | Ordena entries por alfabeto, arquivo, tipo fonético, etc. |
| **Consistency Checker** | Detecta e navega para erros: arquivos ausentes, timing inválido (`overlap >= preutterance`, `end_point < preutterance`), etc. |
| **Duplicate Detector** | Localiza e remove entries duplicados -  tanto duplicatas exatas quanto duplicatas funcionais (mesmo arquivo, mesmos parâmetros, alias diferente). |
| **Pitch Analyzer (Colheita)** | Detecção de F0 de alta precisão (Hz + nota musical) via interpolação parabólica -  útil para validar que samples de um banco multipitch estão dentro do range esperado. |
| **Enxertia (Batch Rename)** | Search & replace com suporte a **regex** para aliases -  permite refactoring em massa de convenções de nomenclatura (ex: migrar de sufixo `_A3` para prefixo `A3_` em todo o banco). |
| **Batch Numeric Edit** | Aplica um valor ou delta numérico a um parâmetro específico em múltiplos entries simultaneamente. |

**Workflow:**

- **Undo/Redo** com histórico completo (`Ctrl+Z` / `Ctrl+Y`).
- **Live Preview:** feedback visual em tempo real ao arrastar marcadores -  o espectrograma e a waveform atualizam instantaneamente sem necessidade de confirmar a edição.
- **Playback integrado** com playhead em tempo real (via rodio) -  permite ouvir o sample com os parâmetros atuais sem sair da ferramenta.
- **Customização visual:** toggle de espectrograma, grid, zero-line, ajuste de cores e espessura de marcadores.

**Diferenças em relação ao SetParam:**

| Característica | SetParam | Copaiba NEO |
|----------------|----------|-------------|
| Plataforma | Windows (WINE em Linux/macOS) | Multiplataforma nativa |
| Linguagem | Delphi/Pascal | Rust |
| Espectrograma | Sim, mas simplista | Sim, log-scale bicúbico |
| Edição em massa | Grid, um pouco limitada | Grid Excel + batch plugins |
| Precisão de edição | ~1ms (visual) | Sub-milissegundo |
| Detecção de F0 | Não integrada | Sim (Colheita, parabolic interp.) |
| Regex em aliases | Sim | Sim |
| Validação automática | Não | Sim (Consistency Checker) |
| Perfis de teclado | Fixo | Copaiba / SetParam / Custom |

**Casos de uso onde Copaiba NEO se destaca:**

- Bancos VCCV/Arpasing com centenas de entries -  o Alias Sorter e o Duplicate Detector aceleram significativamente a organização.
- Reconfiguração de bancos multipitch -  Batch Numeric Edit + Enxertia permitem ajustes sistêmicos de offset e renomeação de sufixos de pitch.
- Debugging de timing -  o Consistency Checker identifica automaticamente `overlap >= preutterance` e outras invariantes violadas.
- Validação de pitch de amostras -  o Colheita confirma se o F0 detectado está dentro do range esperado para cada subdiretório de pitch.

### 6.3 vLabeler

vLabeler é uma ferramenta de labelamento de áudio multi-formato, open-source (Kotlin/Compose Multiplatform), que suporta `oto.ini` nativamente além de outros formatos (NNSVS, DiffSinger, etc.).

**Vantagens sobre SetParam:**
- Cross-platform (Windows, macOS, Linux).
- Suporte nativo a UTF-8.
- Múltiplos templates de labeling configuráveis via JSON.
- Interface de waveform com spectrograma integrado.
- Plugin system para templates customizados.

**Templates relevantes para UTAU:**
- `utau-oto` -  template padrão para oto.ini japonês.
- `arpasing` -  template específico para bancos VCCV (Arpasing).
- `cvvc-chinese` -  template para bancos CVVC em mandarim.

**Configuração de template no vLabeler:**

Os templates são arquivos `.labeler.json` que definem:
- Quais parâmetros existem, seus nomes, tipos e limites.
- Como os parâmetros mapeiam para o formato do arquivo (oto.ini).
- Regras de validação (ex: `overlap < preutterance`).
- Cor e posição visual de cada marcador.

### 6.4 oremo

oremo é uma ferramenta de **gravação** de voicebanks UTAU, mas também inclui funcionalidade de geração automática de oto.ini baseada no reclist (lista de gravação) configurado.

**Geração de oto.ini pelo oremo:**

O oremo gera um oto.ini template onde:
- O offset é definido com base no silêncio configurado no início de cada gravação.
- O consonant é estimado por tipo de fonema (tabela interna).
- Os demais parâmetros são defaults por tipo de banco (CV/VCV).

A geração automática do oremo serve como **ponto de partida** -  refinamento manual posterior é sempre necessário.

### 6.5 UTAU-Synth (macOS)

UTAU-Synth é o port do UTAU para macOS, com algumas diferenças de comportamento:
- Lê `oto.ini` em UTF-8 por padrão.
- Algumas implementações de resampler produzem resultados ligeiramente diferentes do UTAU Windows para os mesmos parâmetros de oto.ini.

### 6.6 Edição manual

Para automação, scripting ou correção em massa, o `oto.ini` pode ser editado diretamente como texto:

**Python -  ler e modificar oto.ini:**

```python
import codecs
from dataclasses import dataclass
from typing import Optional

@dataclass
class OtoEntry:
    filename: str
    alias: str
    offset: float
    consonant: float
    cutoff: float
    preutterance: float
    overlap: float

def parse_oto(filepath: str, encoding: str = 'shift-jis') -> list[OtoEntry]:
    entries = []
    with codecs.open(filepath, 'r', encoding=encoding) as f:
        for line in f:
            line = line.strip()
            if not line or '=' not in line:
                continue
            filename, params_str = line.split('=', 1)
            parts = params_str.split(',')
            if len(parts) < 6:
                continue
            alias, offset, consonant, cutoff, preutterance, overlap = parts[:6]
            entries.append(OtoEntry(
                filename=filename,
                alias=alias,
                offset=float(offset),
                consonant=float(consonant),
                cutoff=float(cutoff),
                preutterance=float(preutterance),
                overlap=float(overlap)
            ))
    return entries

def write_oto(entries: list[OtoEntry], filepath: str, encoding: str = 'shift-jis'):
    with codecs.open(filepath, 'w', encoding=encoding) as f:
        for e in entries:
            line = f"{e.filename}={e.alias},{e.offset},{e.consonant},{e.cutoff},{e.preutterance},{e.overlap}\n"
            f.write(line)

# Exemplo: aumentar offset de todos os entries em 10ms
entries = parse_oto('oto.ini')
for e in entries:
    e.offset += 10.0
write_oto(entries, 'oto_modified.ini')
```

**Casos de uso para edição programática:**
- Normalizar todos os overlaps para 50% da preutterance.
- Aplicar um shift global de offset após re-gravação com diferente silêncio inicial.
- Validar que nenhum entry tem `overlap >= preutterance`.
- Converter encoding de Shift-JIS para UTF-8 em massa.
- Gerar entries de pitch-suffixed automaticamente a partir de um banco mono.

---

## 7. Organização de aliases

### 7.1 Convenções de nomenclatura

A nomenclatura de aliases é crítica para um lookup correto por parte dos plugins de UST e front-ends como OpenUTAU. As convenções variam por tipo de banco e língua alvo.

**Japonês (hiragana/katakana):**
- CV japonês: aliases em hiragana -  `あ`, `い`, `か`, `き`, etc.
- VCV japonês: `- あ` (início de frase com silêncio), `a あ`, `a か`, etc.
- R (respiração): `x R`, `br`, `breath`

**Inglês (Arpabet):**
- Fonemas Arpabet: `ae`, `ah`, `b`, `ch`, `d`, etc.
- CVVC: `b ae`, `ae n`, `n t`, `t s` -  a convenção de Arpasing.
- VCCV: `- b`, `b ae`, `ae n d`, `d -` -  convenção VCCV.

**Português:**
```
Vogais:    a, e, i, o, u
Ditongos:  ai, au, ei, eu, oi, ou, ui
CV:        ba, be, bi, bo, bu, pa, pe, ...
Nasais:    an, en, in, on, un, am, em, im, om, um
```

**Prefixos de pitch (multipitch):**
- Convenção `A3_`, `B3_`, `C4_`: prefixo precedendo o alias -  `A3_あ`, `B3_か`.
- Convenção de sufixo: `あ_A3`, `か_B3`.
- A convenção mais comum no ecossistema UTAU atual é sufixo após underscore.

### 7.2 Prefix Map

O `prefix.map` é um arquivo separado que instrui o UTAU a mapear ranges de pitch para prefixos de alias. Sua estrutura:

```
C1	_C1
D1	_D1
...
C4	_A3
D4	_A3
E4	_A3
F4	_B3
...
```

Isso permite que o UTAU, ao encontrar uma nota em C4, busque automaticamente o alias com sufixo `_A3` em vez do alias base. A geração e manutenção do prefix.map é tão crítica quanto o oto.ini em bancos multipitch.

### 7.3 Aliases de respiração e pausa

Aliases convencionais para elementos não-fonéticos:

| Alias | Uso |
|-------|-----|
| `R` | Respiração (breath sample) |
| `息` | Respiração (japonês) |
| `_` | Pausa explícita |
| `pau` | Pause (Sinsy/Enunu) |
| `AP` | Aspiration pause |
| `SP` | Silence pause |
| `breath` | Respiração genérica |

---

## 8. Configuração por tipo de voicebank

### 8.1 CV (Consonant-Vowel)

O formato CV é o mais simples: um arquivo por sílaba, com a consoante seguida da vogal. É o formato padrão do UTAU japonês.

**Estrutura de arquivo:**
```
か.wav → alias: か
```

**Configuração típica de parâmetros:**

```
filename.wav = alias, offset, consonant, cutoff, preutterance, overlap

か.wav=ka,10,70,-350,80,40
さ.wav=sa,10,90,-350,100,50
た.wav=ta,10,60,-350,75,35
な.wav=na,10,50,-350,60,30
は.wav=ha,10,80,-350,90,45
あ.wav=a,10,20,-400,30,15
```

**Regras de configuração por categoria de fonema:**

| Tipo de fonema | Offset | Consonant | Preutterance | Overlap |
|----------------|--------|-----------|--------------|---------|
| Oclusivas (k, t, p) | Logo antes do burst | 50–80ms | 70–90ms | 30–45ms |
| Fricativas (s, sh, f, h) | No início da fricação | 70–100ms | 90–110ms | 45–55ms |
| Africadas (ch, ts) | Antes do closure | 80–100ms | 90–110ms | 40–55ms |
| Nasais (n, m) | No início do nasal | 40–60ms | 60–80ms | 30–40ms |
| Líquidas/glides (r, y, w) | No início da aproximante | 30–50ms | 50–70ms | 25–35ms |
| Vogais bare | No início da vogal | 10–20ms | 25–40ms | 10–20ms |

**Limitação do CV:** Sem coarticulação entre sílabas -  a transição V→C é sempre feita pelo wavtool via crossfade, sem informação fonética da transição real. Isso resulta em síntese menos natural do que VCV ou CVVC em velocidades normais.

**Organização do oto.ini CV:**

Convenção por ordem fonética:
```
あ行: あ, い, う, え, お
か行: か, き, く, け, こ
さ行: さ, し, す, せ, そ
...
拗音: きゃ, きゅ, きょ, ...
促音: っ
長音: ー (ou usar sample de vogal sustentada)
```

### 8.2 VCV (Vowel-Consonant-Vowel)

O formato VCV captura a coarticulação entre sílabas. Um único arquivo de gravação contém a vogal anterior → consoante → vogal seguinte, e o `oto.ini` expõe tanto a combinação VCV completa quanto, opcionalmente, a vogal inicial isolada.

**Estrutura de arquivo:**
```
a ka.wav → aliases: [a か], [- か], [か]
```

**Múltiplos aliases por arquivo:**

```
a ka.wav=a か,0,90,-350,100,50
a ka.wav=- か,0,90,-350,100,50
a ka.wav=か,0,90,-350,100,50
```

- `a か` -  usado quando a sílaba anterior termina em /a/.
- `- か` -  usado no início de frase (após pausa).
- `か` -  fallback CV (para compatibilidade e início de frase sem sample dedicado).

**Configuração de parâmetros VCV:**

```
a ka.wav=a か,10,80,-400,110,60
```

No VCV, o offset aponta para o **início da vogal antecedente** (o /a/ em `a ka`). A preutterance aponta para o beat, que cai no início da vogal seguinte (/a/ de /ka/). A consonant region engloba a consoante /k/.

```
[offset]  [←— vogal "a" —→]  [←— consoante "k" —→]  [←— vogal "a" final —→]  [cutoff]
                              ↑ consonant boundary      ↑ preutterance (beat)
```

**VCV completo para japonês:**

Um banco VCV completo requer entries para todas as combinações V→CV relevantes. Para o japonês, as vogais são /a, i, u, e, o/ (5), e as consoantes geram sílabas para cada. Um banco mínimo inclui:

- Para cada consoante inicial C: `- C[v]` (início de frase) + `[v1] C[v2]` para cada transição vocálica relevante.
- Entradas de vogal bare: `- あ`, `- い`, `- う`, `- え`, `- お`.
- Entradas de V→V: `a あ`, `a い`, `a う`, etc. (para transições vogal-vogal sem consoante intermediária).

**Reclists populares de VCV:**
- Reclists de 5-mora: mínimo prático, 5 vogais × 5 consoantes.
- Reclists de 7-mora: com glottal stop, nasal mora.
- Mega reclists: incluindo todas as transições, frequentemente 300+ amostras.

### 8.3 CVVC

CVVC (Consonant-Vowel → Vowel-Consonant) é um formato híbrido que captura coarticulação VC usando samples separados para as transições. É o formato dominante para línguas europeias e mandarim no UTAU.

**Anatomia de um banco CVVC:**

Para a sílaba /ban/, três samples são necessários no oto.ini:
1. `b a` -  onset CV (consoante + vogal)
2. `a n` -  coda VC (vogal → consoante final)
3. `n` -  consoante final standalone (opcionalmente)

**Exemplo de oto.ini CVVC em inglês:**

```
ba.wav=b a,10,80,-400,90,45
ba.wav=a,90,20,-200,30,15
an.wav=a n,10,20,-300,30,15
an.wav=n,300,50,-100,60,30
```

**Parâmetros específicos para CVVC:**

A configuração do CVVC é mais complexa porque cada arquivo expõe múltiplas regiões fonéticas:

Para `ba.wav` com conteúdo `/b/ /a/`:
- Entry `b a`: offset antes do /b/, consonant engloba /b/, preutterance no início do /a/.
- Entry `a` (bare vowel para continuação): offset no meio do /a/, consonant pequeno, preutterance pequena.

**Overlap no CVVC:**

O overlap em entries VC deve ser suficiente para cobrir a transição suave da vogal para a consoante final. Se muito pequeno, a consoante final aparece abruptamente; se muito grande, há sobreposição excessiva da vogal sustentada com o onset da consoante final.

**CVVC Chinês (Mandarim):**

O CVVC para mandarim tem uma estrutura específica que segue a divisão inicial (聲母) + final (韻母):

```
ba.wav=b a,10,80,-350,90,45
ba.wav=a,120,20,-200,25,12
a_end.wav=a -,10,20,-150,25,12
```

O `-` no alias `a -` indica o fim da sílaba com apagamento de consoante final (para sílabas abertas).

**Validação de CVVC:**

Um banco CVVC deve garantir:
- Toda transição VC necessária para a língua está presente.
- O alias fallback (CV básico) existe para todas as consoantes iniciais.
- Não há gaps na cobertura fonética que forcem o motor a usar um alias errado.

### 8.4 VCCV (Arpasing)

VCCV (também conhecido como Arpasing quando baseado em Arpabet para inglês) é o formato mais avançado de voicebanks UTAU para inglês. Desenvolvido por Myst/Delta, o sistema captura clusters de consoantes e transições multi-fonema.

**Estrutura de aliases Arpasing:**

| Tipo | Exemplo | Descrição |
|------|---------|-----------|
| `- CV` | `- b ae` | Início de frase + sílaba |
| `VC` | `ae n` | Vogal → consoante simples |
| `VCC` | `ae n d` | Vogal → cluster de consoantes |
| `CC` | `n d` | Consoante → consoante (cluster medial) |
| `CV` | `b ae` | Consoante → vogal |
| `V -` | `ae -` | Vogal → fim de frase |
| `VC -` | `ae n -` | Vogal → consoante → fim |

**Oto.ini VCCV -  exemplos:**

```
band.wav=- b ae,10,80,-600,90,45
band.wav=b ae,10,80,-600,90,45
band.wav=ae n,90,30,-450,40,20
band.wav=ae n d,90,40,-200,50,25
band.wav=n d,200,50,-150,60,30
band.wav=d -,350,30,-100,40,20
```

**Diferença fundamental do VCCV vs CVVC:**

No CVVC, os clusters de consoantes são tratados como sequências de entries independentes. No VCCV, um único entry `ae n d` captura a transição completa vogal → cluster de consoantes em um único sample, preservando a coarticulação multi-fonema.

**Consonant region no VCCV:**

Para entries como `ae n d`, a consonant region deve englobar **apenas o material que precede a zona stretchable** -  normalmente a vogal /ae/ e a nasal /n/, com a plosiva /d/ ficando na zona stretchable (ou no cutoff dependendo da posição na sílaba).

**Ferramentas específicas:**

- **Arpasing Reclist Generator:** gera reclists baseadas no conjunto fonético Arpabet, com cobertura configurable de clusters.
- **vLabeler Arpasing template:** template específico para labelamento VCCV.
- **Presamp plugin:** plugin para OpenUTAU/UTAU que converte automaticamente USTs padrão para usar aliases VCCV.

**Phonetic coverage para Arpasing:**

Um banco Arpasing completo cobre:
- 15 vogais Arpabet: `aa`, `ae`, `ah`, `ao`, `aw`, `ax`, `ay`, `eh`, `er`, `ey`, `ih`, `iy`, `ow`, `oy`, `uh`, `uw`
- 24 consoantes: `b`, `ch`, `d`, `dh`, `f`, `g`, `hh`, `jh`, `k`, `l`, `m`, `n`, `ng`, `p`, `r`, `s`, `sh`, `t`, `th`, `v`, `w`, `y`, `z`, `zh`
- Clusters prioritários: ~200–400 combinações VC/VCC/CC

### 8.5 CVC

CVC é um formato intermediário entre CV e CVVC, usado principalmente para línguas com coda consonantal simples. Cada sample captura C+V+C:

```
cat.wav=c ae t,10,70,-200,80,40
```

O CVC é menos flexível que CVVC para clusters mas mais natural que CV puro para línguas com coda.

### 8.6 Multipitch e Expression banks

**Multipitch:**

Bancos multipitch contêm o mesmo conjunto fonético gravado em múltiplos pitches base (ex: C3, F3, A3, C4, F4). O `oto.ini` de cada pitch está em seu próprio subdiretório:

```
voicebank/
├── C3/
│   ├── oto.ini
│   └── *.wav
├── F3/
│   ├── oto.ini
│   └── *.wav
├── A3/
│   ├── oto.ini
│   └── *.wav
├── prefix.map
└── character.txt
```

Cada `oto.ini` é independente. O prefix.map mapeia ranges de nota para cada pitch:

```
C2	_C3
...
B3	_C3
C4	_F3
...
E4	_F3
はF4	_A3
...
```

**Expression/Append banks:**

Banks de expressão adicionam camadas de articulação (forte, piano, vibrato, falset, etc.). A organização típica:

```
voicebank_power/
├── oto.ini    ← aliases com sufixo de expressão ou em diretório separado
├── character.txt
```

Ou como append do banco principal:

```
character.txt:
name=VoiceName Power
image=...
author=...
```

Aliases de expressão comuns:
- `_強` (forte/power)
- `_弱` (soft/whisper)
- `_息` (breathy)
- `_F` (falsetto)
- `_Vib` (vibrato pre-applied)

---

## 9. Otimização avançada

### 9.1 Calibração de overlap para resamplers específicos

Diferentes resamplers têm sensibilidades diferentes ao overlap. Um workflow de calibração:

1. Criar um UST de teste com uma sequência de sílabas em tempo moderado (BPM 120, notas de 1 beat).
2. Renderizar com o resampler alvo.
3. Analisar o output no DAW para detectar glitches na junção entre samples.
4. Se glitch no onset: overlap muito pequeno → aumentar.
5. Se "flamming" (dois sons simultâneos): overlap muito grande → diminuir.
6. Iterir até transição limpa.

### 9.2 Fine-tuning de consonant para preservação de burst em plosivas

Para plosivas (p, b, t, d, k, g), o burst transiente é o elemento acústico mais crítico. O consonant deve terminar logo após o burst:

```
Waveform de /ka/:
________|spike|←— aspiration —→|←— /a/ periódico —→
        ↑ burst
        consonant boundary aqui → ótimo
```

Se o consonant termina antes do burst: o burst entra na zona de stretching → distorção em velocidades não-100%.
Se o consonant termina depois do aspiration: a aspiration fica na zona fixa → timbre alterado em velocidades não-100%.

### 9.3 Gerenciamento de silêncio e crossfade em final de frase

Entries especiais para final de frase devem ter cutoff configurado para capturar o decaimento natural:

```
あ.wav=あ R,500,20,-50,30,15
```

O alias `あ R` captura a vogal /a/ em posição final, com cutoff próximo do final do arquivo para incluir o decaimento natural. A preutterance pequena e overlap pequeno indicam que este entry é usado em posição de término.

### 9.4 Multi-format entries no mesmo oto.ini

Em bancos avançados, o mesmo `oto.ini` pode conter entries de múltiplos formatos simultaneamente:

```
ka.wav=か,10,80,-350,90,45         ← CV entry
ka.wav=a か,0,80,-350,110,60       ← VCV entry (se o arquivo contém vogal anterior)
ka.wav=a k,0,20,-280,30,15         ← CVVC VC entry
```

Isso requer que os arquivos de gravação sejam suficientemente longos e contenham o contexto vocálico adequado.

### 9.5 Validação de integridade do oto.ini

Uma série de invariantes devem ser verificados:

```python
def validate_entry(e: OtoEntry, wav_duration_ms: float) -> list[str]:
    errors = []
    
    # Overlap deve ser menor que preutterance
    if e.overlap >= e.preutterance:
        errors.append(f"overlap ({e.overlap}) >= preutterance ({e.preutterance})")
    
    # Preutterance deve estar dentro da região utilizável
    if e.preutterance > e.consonant:
        # Preutterance pode ser maior que consonant -  é válido para vogais
        pass
    
    # Cutoff negativo: end_point deve ser maior que preutterance
    if e.cutoff < 0:
        end_point = wav_duration_ms + e.cutoff
        if end_point <= e.offset + e.preutterance:
            errors.append(f"end_point ({end_point}) <= offset + preutterance")
    
    # Offset deve ser menor que a duração do arquivo
    if e.offset >= wav_duration_ms:
        errors.append(f"offset ({e.offset}) >= wav_duration ({wav_duration_ms})")
    
    # Consonant deve ser maior que zero
    if e.consonant < 0:
        errors.append(f"consonant negativo ({e.consonant})")
    
    return errors
```

---

## 10. Casos especiais e edge cases

### 10.1 Samples muito curtos

Para samples curtos (< 300ms), o cutoff deve ser calibrado com muito cuidado:

- Se o cutoff negativo for maior em magnitude do que o material disponível após a preutterance, o end_point cai antes da preutterance → o resampler não tem material para stretching → comportamento indefinido (geralmente silêncio ou glitch).
- Solução: usar cutoff positivo ou reduzir a magnitude do cutoff negativo.

### 10.2 Banco sem extensão de arquivo no alias

Em alguns front-ends (OpenUTAU principalmente), o alias pode resolver sem a extensão `.wav`. Isso é gerenciado internamente -  o `oto.ini` ainda referencia o filename com extensão.

### 10.3 Caracteres especiais em aliases

- Vírgulas em aliases: **não suportado** -  o parser do UTAU usa vírgula como delimitador.
- Aspas em aliases: dependente de implementação.
- Caracteres de controle: evitar.
- Emojis/caracteres Unicode acima do BMP: não suportado em Shift-JIS, possível em UTF-8 com front-ends modernos.

### 10.4 Colisão de aliases entre subdiretórios

Se o mesmo alias existe em `oto.ini` da raiz e em `subdir/oto.ini`, o comportamento é:
- UTAU original: usa o entry da raiz.
- OpenUTAU: usa o entry do subdiretório (last-write wins em alguns casos).
- Solução: usar prefixos de subdiretório no alias, ex: `[EXTRA]か`.

### 10.5 Samples de ruído e respiração

Samples de respiração têm características especiais:
- Sem periodicidade → F0 estimation de resamplers WORLD falha.
- A consonant region deve ser 0 ou muito pequena (o "conteúdo" é todo stretchable de forma homogênea).
- O moresampler tem um modo específico para ruído (`Mn` flag) que desabilita a análise de F0.

```
breath.wav=R,10,5,-200,30,15
```

### 10.6 Alias lookup com prefix map e fallback

O pipeline de resolução de alias no UTAU é aproximadamente:

```
1. Tenta alias exato com prefixo de pitch: "A3 か"
2. Tenta alias exato sem prefixo: "か"
3. Tenta alias com sufixo de pitch removido: depende do implementation
4. Usa sample de silêncio/erro
```

Um oto.ini bem configurado deve garantir que o fallback nunca seja necessário para os fonemas suportados.

---

## 11. Depuração e diagnóstico

### 11.1 Artefatos comuns e causas

| Artefato | Causa provável | Solução |
|----------|----------------|---------|
| Clique no início da nota | Offset cortando no meio de um sample | Aumentar offset ou verificar se está em zero-crossing |
| Consoante distorcida em velocidades baixas | Consonant muito curto -  consoante na zona stretchable | Aumentar consonant |
| Vogal "robótica" em notas longas | Cutoff muito restritivo -  stretching excessivo | Ampliar cutoff (reduzir magnitude do negativo) |
| Transição abrupta entre sílabas | Overlap muito pequeno | Aumentar overlap |
| Duas sílabas soando simultaneamente | Overlap muito grande | Diminuir overlap |
| Nota parece atrasada ritmicamente | Preutterance muito pequena | Aumentar preutterance |
| Nota parece adiantada ritmicamente | Preutterance muito grande | Diminuir preutterance |
| Silêncio no início de cada nota | Offset maior que preutterance, sem material antes | Verificar alinhamento offset/preutterance |
| Sample não encontrado | Alias incorreto ou encoding errado | Verificar encoding do oto.ini e alias |

### 11.2 Ferramentas de diagnóstico

**Análise de waveform:**
- Audacity: visualizar waveform e espectrograma dos samples.
- SoX: inspeção de linha de comando, útil para scripting de validação.

**Análise de output renderizado:**
- Renderizar um UST de teste e abrir no Audacity.
- Verificar transições usando zoom no zero-crossing entre samples.
- Usar análise espectral para detectar artefatos de phase vocoder.

**Logging de resampler:**
- Alguns resamplers (moresampler) produzem logs detalhados do processamento.
- Analisar logs para detectar F0 estimation failures, boundary issues, etc.

### 11.3 Teste de cobertura de alias

```python
# Verificar se todos os aliases necessários estão presentes
required_aliases_cv_japanese = [
    'あ', 'い', 'う', 'え', 'お',
    'か', 'き', 'く', 'け', 'こ',
    'さ', 'し', 'す', 'せ', 'そ',
    # ... etc
]

def check_coverage(entries: list[OtoEntry], required: list[str]) -> list[str]:
    available = {e.alias for e in entries}
    missing = [alias for alias in required if alias not in available]
    return missing

entries = parse_oto('oto.ini')
missing = check_coverage(entries, required_aliases_cv_japanese)
if missing:
    print(f"Aliases ausentes: {missing}")
```

---

## 12. Referência rápida

### 12.1 Sintaxe da linha oto.ini

```
filename.wav=alias,offset,consonant,cutoff,preutterance,overlap
```

Todos os valores de tempo em **milissegundos**. Cutoff negativo = relativo ao final do arquivo.

### 12.2 Valores de referência por tipo de fonema (CV japonês)

| Fonema | Offset | Consonant | Cutoff | Preutterance | Overlap |
|--------|--------|-----------|--------|--------------|---------|
| あ/い/う/え/お | 10 | 15 | −400 | 30 | 15 |
| か/き/く/け/こ | 10 | 70 | −380 | 85 | 40 |
| さ/す/せ/そ | 10 | 100 | −380 | 110 | 55 |
| し | 10 | 110 | −380 | 120 | 60 |
| た/て/と | 10 | 60 | −380 | 75 | 35 |
| ち | 10 | 90 | −380 | 100 | 50 |
| つ | 10 | 80 | −380 | 90 | 45 |
| な/に/ぬ/ね/の | 10 | 50 | −400 | 65 | 30 |
| は/ひ/ふ/へ/ほ | 10 | 80 | −400 | 90 | 45 |
| ま/み/む/め/も | 10 | 50 | −400 | 60 | 30 |
| や/ゆ/よ | 10 | 40 | −400 | 50 | 25 |
| ら/り/る/れ/ろ | 10 | 50 | −400 | 60 | 30 |
| わ/を | 10 | 40 | −400 | 50 | 25 |
| ん | 10 | 40 | −400 | 50 | 25 |
| R (breath) | 10 | 5 | −200 | 30 | 15 |

> **Atenção:** Estes são valores de referência iniciais. Ajuste fino com base no sample real é **sempre necessário**.

### 12.3 Checklist de configuração de banco

- [ ] Todos os aliases necessários para o tipo de banco estão presentes.
- [ ] Encoding do oto.ini está correto (Shift-JIS para UTAU original, UTF-8 para OpenUTAU/vLabeler).
- [ ] `overlap < preutterance` para todos os entries.
- [ ] Nenhum `offset + preutterance > end_point` (onde `end_point = file_duration + cutoff`).
- [ ] Consonant termina antes do onset da vogal (para bancos CV/VCV).
- [ ] Entries de início de frase (`- phoneme`) existem para todos os fonemas iniciais.
- [ ] Prefix map configurado corretamente (para bancos multipitch).
- [ ] Validação de áudio com UST de teste renderizado.
- [ ] Teste em pelo menos dois resamplers diferentes (comportamento pode divergir).

### 12.4 Fórmulas de referência

```
# Ponto de fim utilizável do sample:
end_point_ms = (cutoff < 0) ? (file_duration_ms + cutoff) : (offset + cutoff)

# Duração da região stretchable:
stretchable_duration = end_point_ms - (offset + consonant)

# Duração da nota que força stretching (além do natural):
requires_stretch_if_note > stretchable_duration - preutterance

# Effective preutterance com velocity:
effective_preutterance = preutterance * (velocity / 100.0)

# Tamanho da região de crossfade:
crossfade_region = [offset, offset + overlap]
```

---
