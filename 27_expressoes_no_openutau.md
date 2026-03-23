# Expressões no OpenUtau: O Controle Total

O OpenUtau mandou o sistema antigo de flags pro museu e trouxe as **Expressões**. É um jeito muito mais visual e intuitivo de você dizer pro seu personagem como ele deve cantar cada nota. Vamos ver as mais importantes.

---

### As Expressões que Você Vai Usar Sempre

1. **Velocity (VEL)**: Controla a velocidade da consoante.
   - Quer um rap rápido? Aumente o VEL (ex: 150) pra consoante sair "na lata".
   - Quer que a voz soe mais lenta e preguiçosa? Baixe o VEL (ex: 50).
2. **Breath (BRE)**: O controle da soprosidade.
   - Perfeito pra criar aquele clima triste ou sussurrado. Quanto mais alto, mais "ar" a voz solta.
3. **Gender (GEN)**: Muda a "grossura" da voz.
   - É ótimo pra criar variações. Você pode usar um valor positivo pra deixar a voz mais madura ou negativo pra deixar a voz mais infantil ou feminina.
4. **Volume (VOL)**: Dispensa apresentações, né? É o ganho da nota. Use pra dar ênfase em palavras importantes do refrão.

---

### Automação (As Curvas)

Abaixo do piano roll tem um painel onde você pode desenhar curvas.
- **Dica de Amigo**: Não deixe a dinâmica reta o tempo todo. A voz humana oscila. Desenhe pequenos "crescendos" de volume e variações de `BRE` ao longo da música pra ela ganhar vida.

---

### Expressões Customizadas

Se você criou um banco de voz com pastas especiais (como um `Append Soft`), você pode configurar o OpenUtau pra ter uma expressão chamada "Estilo".
- Isso é feito no arquivo `character.yaml`.
- Depois de configurado, você só troca o valor (ex: 0 pra Normal, 1 pra Soft) e o programa troca a amostra de áudio automaticamente. É prático demais!

---

### Pitch Bend (Vibrato e Afinação)

No OpenUtau, você desenha a curva de afinação direto em cima da nota.
- Quer aquele vibrato de profissional? Use as ferramentas de curva e desenhe as oscilações.
- O segredo de uma boa afinação é nunca deixar ela 100% reta. Pequenas oscilações dão um charme natural.

Explore o painel de expressões sem medo. É ali que você tira o seu UTAU do "modo robô" e coloca ele no "modo artista".
