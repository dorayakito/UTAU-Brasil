# ✦ Tutorial: Configuração oto.ini.INI

A `oto.ini` é o arquivo mais importante do banco de voz. Ele diz ao software como interpretar e cortar as gravações para que o canto soe natural.

---

### Os 6 Parâmetros da oto.ini

1. **Alias:** Nome alternativo. Ex: `ka.wav` pode ter o alias `ka`.
2. **Offset (Deslocamento):** Área inicial em azul. Tudo antes disso é ignorado.
3. **Consonant (Área Rosa):** Área que **não** é esticada pelo motor. Serve para proteger o som da consoante.
4. **Cutoff (Corte):** Área final em azul. Corta o final do áudio. Se o valor for negativo, ele conta a partir do final do arquivo.
5. **Preutterance (Linha Vermelha):** Indica onde a nota musical começa.
6. **Overlap (Linha Verde):** Define o quanto o som se cruza com a nota anterior para uma transição suave.

---

### Como configurar (Método CV)

Siga esta ordem para configurar cada fonema:

1. **Offset:** Corte o silêncio inicial até o comecinho da consoante.
2. **Overlap:**
   - **Plosivas (b, d, g, k, p, t):** Deixe um pequeno espaço antes do pico do som.
   - **Não-plosivas (f, h, m, n, r, s, sh, v, z):** Deixe no meio do som da consoante.
3. **Preutterance:** Posicione exatamente no início da vogal (onde a onda fica mais "cheia").
4. **Consonant (Rosa):** Cubra toda a consoante e uma pequena parte do início da vogal.
5. **Cutoff:** Corte o final do áudio, deixando apenas a parte da vogal que soa bem e está estável.

---

### Dicas Importantes

- **Regra do Volume:** Se a vogal oscila muito de volume, ajuste o loop (parte branca) para uma área mais estável.
- **Transições Suaves:** Um overlap maior ajuda nas não-plosivas, enquanto um overlap menor evita "comer" o som das plosivas.
- **Uso de Ferramentas:** Recomendamos o uso do **vLabeler** ou **SetParam** para editar a `oto.ini` visualmente, ao invés de editar texto manualmente.

---

### Vídeo Tutorial Recomendado

Assista à série:
- [Aprenda a fazer oto.ini FINALMENTE](https://www.youtube.com/watch?v=dQw4w9WgXcQ) (Link ilustrativo, verifique o link real na Megathread original se disponível).
