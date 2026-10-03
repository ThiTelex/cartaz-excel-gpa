# A7 Gerador de Cartazes — v23

## Correção desta versão
- A caixa preta da dinâmica foi convertida para um fundo SVG inline, evitando dependência da opção do navegador de imprimir fundos/cores.
- O texto da dinâmica permanece branco sobre a caixa preta.
- A borda externa de cada cartaz A7 continua sendo ocultada na impressão.
- A exportação em PDF foi removida na v26.
- O botão “Imprimir todas as páginas” monta temporariamente todas as folhas A4 (8 cartazes por folha) somente durante a impressão; elas não são adicionadas à pré-visualização.

Senha atual permanece no `config.json`.

## v26
- Baseada diretamente na v24 (grade física 4x2).
- Removido completamente o recurso Exportar PDF e as bibliotecas html2canvas/jsPDF.
- Adicionado Imprimir todas as páginas, usando uma área de impressão temporária que é limpa após a impressão.
- A pré-visualização continua exibindo apenas a página A4 selecionada.
