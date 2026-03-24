# Kire e Powerscale: Dinâmica em Movimento

Se você já ouviu falar de bancos de voz "Kire", você provavelmente ouviu o som mais potente que o UTAU pode oferecer. Esses termos se referem à capacidade do banco de mudar de intensidade de acordo com o volume e a nota.

---

### O Estilo Kire

O termo "Kire" ficou famoso por causa de bancos japoneses de alta performance (como o da Kasane Teto ou da Defoko em versões avançadas).
- Basicamente, são bancos multipitch com intensidades gravadas de forma muito distinta.
- Nas notas baixas, a voz é calma; nas notas altas, o cantor realmente solta o vozeirão.
- Isso cria uma transição natural e emocionante sem que você precise fazer quase nada no tuning.

---

### O Que é Powerscale?

O Powerscale é uma técnica de organização onde o criador do banco usa ferramentas pra equilibrar o volume de todas as oitavas.
- O objetivo é que, ao mudar de uma oitava pra outra, a voz ganhe "corpo" mas não dê um susto no ouvinte com um aumento repentino e estridente de volume.
- É uma questão de equilíbrio técnico na mixagem do próprio banco de voz.

---

### Como Tirar Proveito de um Banco Kire

Se você baixou um banco com essas características:
1. **Confira o Prefix Map**: Veja se ele está configurado pra trocar de oitava nos momentos certos da escala musical.
2. **Modulação Zero (mod0)**: Em bancos Kire, é quase obrigatório usar modulação 0 pra manter a integridade dos harmônicos poderosos que o cantor gravou.
3. **Resamplers Fortes**: Use motores como o `moresampler` ou o `TIPS` pra processar essas vozes, pois eles preservam melhor a força das frequências agudas.

---

### No OpenUtau

O OpenUtau lida com o estilo Kire de braços abertos. Ele consegue visualizar as diferentes ondas e você pode ver exatamente onde a voz "muda de marcha".
- **Dica**: Use a expressão de `Dynamics` (Dinâmica) pra controlar manualmente se você quer a versão potente ou a versão normal de uma nota, independente da altura dela.

Bancos Kire são o "topo de linha" do UTAU clássico. Se você quer que sua música soe como um hino de anime épico, esse é o tipo de voz que você precisa procurar.
