# A7 Gerador de Cartazes — v23

## Correção desta versão
- A caixa preta da dinâmica foi convertida para um fundo SVG inline, evitando dependência da opção do navegador de imprimir fundos/cores.
- O texto da dinâmica permanece branco sobre a caixa preta.
- A borda externa de cada cartaz A7 continua sendo ocultada na impressão e no PDF.
- O PDF continua sendo exportado como um único arquivo com todas as páginas A4.

Senha atual permanece no `config.json`.


## v26
- Baseada diretamente na v24 (grade física A4 4×2).
- Removida a opção Exportar PDF e as bibliotecas html2canvas/jsPDF.
- Adicionado o botão “Imprimir todas as páginas”.
- A impressão de todas as páginas é montada em um contêiner separado somente no momento da impressão; a pré-visualização continua exibindo apenas a página selecionada.
- Após a impressão, o contêiner de impressão múltipla é limpo para não interferir na pré-visualização seguinte.
