# A7 Gerador de Cartazes — v10

Base de referência: **v5**, preservando paginação, setas, pré-visualização, botão geral **Imprimir**, **Exportar PDF**, Result, PBi, Zebrinha, tratamentos Padrão/Percentual/Pack/Parcelamento, arquivo persistido na sessão e Code 39 em SVG.

## Regras das dinâmicas

A tabela abaixo é a regra oficial usada pelo modelo A7:

| COD DINAMICA | DINAMICA | DESC DINAMICA |
|---:|---|---|
| 10 | REGULAR | APROVEITE |
| 12 | OFERTA | OFERTA |
| 13 | PRÓXIMO AO VENCIMENTO | PRÓXIMO AO VENCIMENTO |
| 15 | MARKDOWN | ÚLTIMAS UNIDADES |
| 16 | FORA DE LINHA SEM PROMOÇÃO | APROVEITE |
| 19 | OFERTA 2 UNID | `Mensagem Etiqueta` do Result |
| 20 | FIDELIDADE CLUBE EXTRA | EXCLUSIVO CLUBE EXTRA |
| 23 | A PARTIR DE | `Mensagem Etiqueta` do Result |
| 24 | LEVE / PAGUE | `Mensagem Etiqueta` do Result |
| 25 | OFERTA SEM PROMOÇÃO DE | APROVEITE |

**Importante:** para as dinâmicas **19, 23 e 24**, a terceira informação não é fixa: deve ser lida da coluna **Mensagem Etiqueta** do arquivo `Result`.

Para a dinâmica **15 (MARKDOWN)**, a descrição é sempre **ÚLTIMAS UNIDADES**.

## Composição do cartaz A7

Quando existem dois preços diferentes (DE/POR), o cartaz apresenta:

1. preço **DE:** riscado;
2. caixa com a dinâmica;
3. frase **NESTA PROMOÇÃO, A UN. SAI POR**;
4. preço **POR:** ofertado;
5. PLU;
6. código de barras Code 39.

Os preços **DE** e **POR** usam tamanho de fonte discreto e igual, equivalente ao tamanho que era usado no preço riscado.

Quando não há DE/POR diferentes, permanece somente o preço normal em destaque.

## COD

- Ordem: **DESCRIÇÃO / CÓDIGO DE BARRAS / PLU**.
- Agrupado por **Nome da categoria** e A-Z dentro da categoria.
- Code 39 gerado em SVG, sem depender de fonte instalada.

## Impressão

- Pré-visualização mantém bordas para visualizar os cartazes.
- Impressão não coloca borda adicional no cartaz físico.
- Existe apenas o botão geral **Imprimir**.
- **Exportar PDF** gera todas as páginas.


function calcDeResult(raw,dyn){
  // Regras fixas do Result por NOME DA COLUNA (não por posição do CSV).
  // DE: 12/15 = Preço De; 19/20/23/24 = Preço Venda; demais = sem DE.
  if([12,15].includes(dyn)) return num(val(raw,["Preço De","PREÇO DE","Preco De"]));
  if([19,20,23,24].includes(dyn)) return num(val(raw,["Preço Venda","PREÇO VENDA","Preco Venda"]));
  return 0;
}
function parsePercent(v){
  if(v===null||v===undefined||v==="") return 0;
  let s=String(v).trim().replace(/\s/g,"").replace(",",".");
  if(s.endsWith("%")) return num(s.slice(0,-1))/100;
  const n=Number(s);
  // Msg 1 normalmente vem como percentual (ex.: 20 ou 20%).
  return Number.isFinite(n) ? (Math.abs(n)>1 ? n/100 : n) : 0;
}
function calcPorResult(raw,dyn){
  if([10,12,13,15,16,25].includes(dyn)) return num(val(raw,["Preço Venda","PREÇO VENDA","Preco Venda"]));
  if([20,23,24].includes(dyn)) return num(val(raw,["Preço Fide/Promo","PREÇO FIDE/PROMO","Preco Fide/Promo"]));
  if(dyn===19){
    const unit=num(val(raw,["Preço Unitário","PREÇO UNITÁRIO","Preco Unitario","Preço Venda","PREÇO VENDA","Preco Venda"]));
    const pct=parsePercent(val(raw,["Msg 1","MSG 1","Msg1","MSG1"]));
    const precoComDesconto=unit*(1-pct);
    return +((precoComDesconto+unit)/2).toFixed(2);
  }
  return 0;
}

### Regras de dinâmica
- 10 REGULAR → APROVEITE
- 12 OFERTA → OFERTA
- 13 PRÓXIMO AO VENCIMENTO → PRÓXIMO AO VENCIMENTO
- 15 MARKDOWN → **ÚLTIMAS UNIDADES**
- 16 FORA DE LINHA SEM PROMOÇÃO → APROVEITE
- 19 OFERTA 2 UNID → descrição da `Mensagem Etiqueta`
- 20 FIDELIDADE CLUBE EXTRA → EXCLUSIVO CLUBE EXTRA
- 23 A PARTIR DE → descrição da `Mensagem Etiqueta`
- 24 LEVE / PAGUE → descrição da `Mensagem Etiqueta`
- 25 OFERTA SEM PROMOÇÃO DE → APROVEITE

Quando houver DE/POR, o retângulo preto usa a **descrição da dinâmica** (`dynDesc`), e não o nome da dinâmica. Assim, MARKDOWN exibe **ÚLTIMAS UNIDADES** no retângulo.


## v13 — correções de preço
- A coluna `Preço Venda` é localizada de forma tolerante a diferenças de maiúsculas/minúsculas, acentos, espaços e pequenas variações do cabeçalho.
- Dinâmica 10 (REGULAR) sempre usa `Preço Venda` como POR quando houver valor.
- POR ficou com 16px para leitura melhor; o rótulo `POR:` permanece 12px. DE permanece 12px e riscado.

## v15 — tamanho do DE
Quando o cartaz tiver DE/POR, o valor do **DE** fica em **24 px**, negrito 950, com o mesmo tipo de fonte do preço regular e riscado. O rótulo `DE:` permanece pequeno.


## v16 — validade das ofertas
Para as dinâmicas **12, 13, 15, 19, 20, 23, 24 e 25**, quando houver as datas no `Result`, o cartaz exibe abaixo do preço POR:

**Ofertas válidas de X a Y ou enquanto durar nossos estoques.**

- **X** = `Data Inicio Fide/Promo`
- **Y** = `Data Fim Fide/Promo`
- Fonte: **12px**, sem negrito.
- As datas são apresentadas em formato `dd/mm/aaaa` quando o arquivo vier em formato de data reconhecível.


## Rodapé

© 2026 By Thiago Teles

## v17 — fluxo de acesso e identidade visual
- As etapas **2 · Tratamento, 3 · Cartazes, 4 · Impressão e 5 · COD** ficam bloqueadas até um arquivo ser selecionado e **Processar arquivo** ser concluído.
- Depois do processamento, a navegação entre as etapas ocorre pelos botões **AVANÇAR** e **RETROCEDER** dentro de cada página.
- Ao limpar o arquivo, o fluxo volta a ficar bloqueado e retorna para **1 · Entrada**.
- Barra superior usa o logo Extra Mercado fornecido e identidade em vermelho.
- Botões principais usam azul; botões de retorno usam vermelho.
- Rodapé da aplicação: **© 2026 By Thiago Teles**.


## v18 — acesso por senha
- A aplicação abre primeiro em uma tela de login inspirada no modelo enviado.
- O visual usa fundo escuro com identidade Extra Mercado em vermelho e azul.
- A senha é consultada em `config.json`, pela propriedade `senha`; não fica gravada no JavaScript.
- Para trocar a senha futuramente, basta alterar o valor de `senha` em `config.json`.
- Após autenticar, o usuário entra no fluxo normal da aplicação. A autenticação é mantida na sessão do navegador.

### Observação de segurança
Como este é um aplicativo HTML/JavaScript executado no navegador, `config.json` pode ser acessado por quem tiver acesso ao site. Portanto, essa senha funciona como **trava de acesso da interface**, não como autenticação de segurança de servidor. Para proteger dados ou impedir acesso técnico ao sistema, seria necessário autenticação no servidor.


## v19 — ícones e quebra de página
- Incluído Google Material Symbols Rounded para os ícones da interface, incluindo cadeado do login, upload, impressão, PDF, navegação e ações.
- A impressão individual de cartaz não força `page-break-after: always`; como a pré-visualização imprime uma única página A4 por vez, isso evita a criação de uma folha A4 extra em branco.
- A área A4 da impressão individual fica limitada a 297 × 210 mm, com `break-inside/page-break-inside: avoid` e overflow controlado.


## v20 — correção do login/config.json
- O `config.json` agora é resolvido a partir do endereço do próprio `app.js`, funcionando também quando o projeto é publicado em subpastas, como GitHub Pages.
- A senha lida do JSON é normalizada com `trim()`, evitando falhas por espaços acidentais.
- Erros de carregamento do JSON ficam registrados no console para diagnóstico.
- A senha continua sendo exclusivamente a propriedade `senha` do `config.json`; o valor atual permanece `1833`.


## v21 — impressão e PDF
- O contorno externo de cada cartaz A7 não é impresso e também não aparece no PDF exportado.
- A caixa preta da dinâmica (`promo-box`) continua sendo impressa/exportada.
- `Exportar todos em PDF` gera um único arquivo PDF com **todos os cartazes**, em páginas A4 horizontais, 8 cartazes por página. Não depende da página atualmente selecionada na pré-visualização.

## v22 — Caixa da dinâmica na impressão
- Mantém a borda externa do cartaz A7 somente na pré-visualização.
- Remove a borda externa na impressão física.
- Força a preservação do fundo preto e texto branco da caixa `.promo-box` na impressão com `print-color-adjust: exact` e `-webkit-print-color-adjust: exact`.
- O PDF continua sem a borda externa e com a caixa preta da dinâmica.
