# A7 Gerador de Cartazes — v23

## Correção desta versão
- A caixa preta da dinâmica foi convertida para um fundo SVG inline, evitando dependência da opção do navegador de imprimir fundos/cores.
- O texto da dinâmica permanece branco sobre a caixa preta.
- A borda externa de cada cartaz A7 continua sendo ocultada na impressão e no PDF.
- O PDF continua sendo exportado como um único arquivo com todas as páginas A4.

Senha atual permanece no `config.json`.

## v25 — ajuste de corte central 4×2
Na impressão A4 horizontal, as quatro colunas usam 73,75 mm cada, com um vão central de 2 mm entre as colunas 2 e 3. As colunas 3 e 4 recebem deslocamento de 2 mm. As margens externas permanecem alinhadas às bordas da folha (73,75 × 4 + 2 = 297 mm). O mesmo ajuste é aplicado ao PDF.
