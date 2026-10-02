# Exercício 1: Aplicações

#### 1. Uma loja virtual calcula o frete por modalidade (Sedex, PAC, Retirada) e precisa permitir **novas modalidades** sem alterar o `Checkout`.
- Faz sentido usar Strategy

#### 2. Um sistema de pagamentos aceita **Pix, cartão e boleto**, escolhidos pelo cliente no momento da compra, cada um com um fluxo próprio.
- Faz sentido usar Strategy

#### 3. Um jogo de batalha permite ao jogador escolher entre **fórmulas de dano** diferentes (bruto, crítico, à distância), combináveis com a mesma lógica de combate.
- Faz sentido usar Strategy

#### 4. Uma classe `Relatorio` que **sempre** gera o mesmo PDF, com um formato fixo que nunca muda e sem variações de comportamento.
- Não faz sentido usar Strategy, não existe mudança em tempo de execução e é sempre um mesmo PDF sem variações.

#### 5. Uma classe `Ponto` com apenas `x` e `y`, que só soma coordenadas e nunca ganha algoritmos alternativos.
- Não faz sentido usar Strategy, não existe mudança em tempo de execução e necessidade.

Para cada item, responda:

# Exercício 2: Rastreando o anti-pattern em um projeto real

### Baixe o projeto, importe na IDE e **rode o `Main`**:

##### [Baixar `projeto_strategy_antipattern.zip`](/exemplos/projeto_strategy_antipattern.zip)

É um sistema de vendas (Maven, Java 17) que calcula **desconto**, **frete** e
**relatório** conforme o `TipoCliente` (`COMUM`, `VIP`, `CORPORATIVO`). O código
**funciona**, mas repete a decisão de comportamento em vários pontos.

```bash
mvn compile
java -cp target/classes br.venson.net.designpatterns.strategy.Main
```

> No Eclipse/IntelliJ, basta importar a pasta do projeto e executar a classe `Main`.

Analise o código e responda:

1. Localize **todas** as decisões de comportamento baseadas em `TipoCliente`. Quantas
   classes repetem o **mesmo** `switch`? O que isso indica sobre o design?
2. O que é preciso alterar para adicionar um novo tipo de cliente (por exemplo,
   `PARCEIRO`)? Liste os arquivos que seriam tocados.
3. Por que esse código é difícil de **testar isoladamente** por regra (desconto, frete,
   formatação)? Como as condicionais atrapalham?
4. Proponha uma refatoração com o padrão **Strategy**, deixando explícitos:
   - a **interface de estratégia**;
   - as **estratégias concretas**;
   - o **contexto** que delega;
   - como o `RelatorioPedido` passa a depender apenas da abstração.
5. Depois de refatorar, como fica a **troca de comportamento em runtime**? Dê um
   exemplo (ex.: aplicar uma regra promocional e depois voltar à padrão).

**Entrega:** um diagrama de classes (antes/depois), o código refatorado e um
parágrafo justificando cada decisão de design.
