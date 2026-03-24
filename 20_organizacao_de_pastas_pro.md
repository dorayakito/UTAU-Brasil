# Organização de Pastas: O Segredo dos Pro

Já abriu a pasta de um UTAU e parecia que um furacão passou por lá? Centenas de arquivos soltos, nomes esquisitos... Isso é um pesadelo! Vamos aprender a organizar seu banco de voz como um profissional.

---

### A Estrutura Básica de Ideia

Uma pasta de UTAU organizada deve ter:
1. **Pasta Raiz**: Nome do seu personagem.
2. **Subpastas por Pitch**: Se for multipitch, cada oitava deve estar em sua própria pasta (ex: `_A3`, `_D4`, `_G4`).
3. **Subpastas por Estilo**: Se tiver appends, separe-os também (ex: `Soft`, `Power`).

---

### Arquivos que Não Podem Faltar na Raiz

- `character.txt`: O documento com o nome, autor e imagem do banco.
- `icon.bmp`: A imagem pequenininha que aparece no programa.
- `install.txt` (opcional): Ajuda o UTAU a instalar o banco sozinho.
- `readme.txt`: Seus termos de uso (TOS) e contatos.

---

### O Perigo dos Nomes de Arquivo

Dica de amigo: **Evite acentos e caracteres especiais nos nomes dos arquivos .wav.**
- Use apenas letras normais, números e o sublinhado (`_`).
- Se o seu banco for em japonês, certifique-se de que os nomes estão em Hiragana e que você está usando o Japanese Locale no Windows pra não corromper os nomes.

---

### O Arquivo character.yaml (OpenUtau)

Se você quer que seu banco brilhe no OpenUtau, você pode criar um arquivo `character.yaml`.
- Ele permite configurar ícones personalizados pra cada estilo, definir o autor e até criar expressões exclusivas pro seu banco. É o que dá aquele visual premium e moderno.

---

### Boas Práticas

- **Limpeza**: Delete arquivos temporários que não servem pra nada (tipo arquivos `.frq` de resamplers que você não usa mais) antes de distribuir seu banco.
- **Compressão**: Na hora de enviar pra internet, use `.zip`. Evite `.rar` ou formatos menos comuns, pois o `.zip` é o padrão que o UTAU e o OpenUtau conseguem ler direto.


