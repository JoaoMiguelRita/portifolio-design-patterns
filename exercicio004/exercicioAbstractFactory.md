# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Abstract Factory** é apropriado ou **não** e **justifique em 2–3 frases**.

### 1. Uma aplicação de interface gráfica que precisa **trocar o tema/plataforma** (Windows, macOS, Linux). Cada plataforma exige uma família coerente de componentes (botão, checkbox, janela), e misturar componentes de plataformas diferentes quebra a interface.
- Faz sentido usar Abstract Factory. Para cada plataforma, terão os requisitos de bibliotecas especificas, apesar de terem a mesma finalidade, com isso é valido aplicar asse design pattern.

### 2. Uma classe simples `Ponto` com apenas **dois campos obrigatórios** (`x`, `y`), utilizada em um sistema de CAD e instanciada dezenas de vezes por segundo através do construtor tradicional.
- Não faz sentido usar Factory Method, não adianta criar um canhão pra mantar a formiga, o Factory Method é indicado para diversas classes que realizam metodos semelhantes.

### 3. Um módulo de acesso a bancos de dados que precisa manter **famílias consistentes por fornecedor**: para MySQL existem `ConexaoMySQL`, `ComandoMySQL` e `TransacaoMySQL`; para PostgreSQL existem as versões `PostgreSQL`. Todos os objetos usados juntos devem vir **do mesmo fornecedor**.
- Faz sentido usar Abstract Factory. Para cada banco de dados, terão os requisitos de configurações especificas, apesar de terem a mesma finalidade, com isso é valido aplicar asse design pattern.

### 4. Uma loja que vende **kits de móveis por estilo** (moderno, vitoriano, art déco). Cada kit é composto por cadeira + sofá + mesa de centro e o cliente espera que os três **combinem entre si**.
- Faz sentido usar Abstract Factory. Por se relacionarem entre si, e fazerem parte da mesma familia, é valido aplicar.


### 5. Uma classe `Produto` com três campos obrigatórios (`nome`, `preco`, `quantidadeEstoque`), criada em um único ponto do sistema através do construtor tradicional e sem variações.
- Não faz sentido usar Factory Method, não adianta criar um canhão pra mantar a formiga, o Factory Method é indicado para diversas classes que realizam metodos semelhantes.

---

# Exercício 2: Analogia

Crie uma **analogia própria** para explicar o padrão Abstract Factory para alguém que não é da área de TI.

- Uma pentiadeira, na qual contém uma familia de maquiagens, acessórios, produtos, ferramentas, perfumes, na qual diverentes coisas podem combinar entre si.

---

# Exercício 3: Anti-pattern

Considere o código Java abaixo, usado para montar a interface gráfica de uma aplicação:

```java
public class Aplicacao {

    private Botao botao;
    private Checkbox checkbox;

    public Aplicacao(String sistemaOperacional) {
        if (sistemaOperacional.equals("windows")) {
            botao = new BotaoWindows();
            checkbox = new CheckboxWindows();
        } else if (sistemaOperacional.equals("linux")) {
            botao = new BotaoLinux();
            checkbox = new CheckboxWindows();
        }
    }

    public void exibir() {
        botao.renderizar();
        checkbox.alternar();
    }
}
```

Responda:

### 1. Por que essa forma de **criar os componentes com `new`** é um problema de design?
- Exige que a aplicação recompile cada objeto novo.

### 2. Que tipo de **bug ou comportamento estranho** pode acontecer quando o código mistura componentes de famílias diferentes (como no `linux`)? E o que aconteceria ao adicionar um novo sistema operacional (ex.: `mac`)?
- Poderia causar falhas ao capturar bibliotecas, e o mac exigiria de um novo else if, e novos testes nos ifs que já existiam para garantir que não quebre.

### 3. Proponha uma solução usando o padrão **Abstract Factory**, explicando em linhas gerais: os **produtos abstratos**, a **fábrica abstrata**, as **fábricas concretas** por SO e como o cliente (a `Aplicacao`) passaria a receber a fábrica.
- Os produtos concontreos pode se entende com as diferentes classes já combinadas entre si, e elas extendem do objeto cru, então no exemplo Mac seria uma classe extendida de Aplicacao, e possuiria dentro dela somente os componentes para exibir no Mac. A fábrica abstrata juntaria além de componentes do Mac, protocolos padrão também compartilhados sobre outros SO, nesse tudo deve ser abstrato. Por fim, as fábricas concretas que devem implementar a fabrica abstrata, e dentro dela junta tudo o que é necessário para que o Mac fique pronto, assim fica subentendido pela aplicação que ao chamar um BotaoMac, o objeto de referencia se trata de um mac.

---

# Exercício 4: Exemplo real

Acesse os seguintes arquivos em um projeto open source:

- Projeto: [`iluwatar/java-design-patterns`](https://github.com/iluwatar/java-design-patterns) (um dos repositórios de padrões de projeto mais populares do GitHub)
- Arquivos:
  - [`KingdomFactory.java`](https://github.com/iluwatar/java-design-patterns/blob/master/abstract-factory/src/main/java/com/iluwatar/abstractfactory/KingdomFactory.java)
  - [`ElfKingdomFactory.java`](https://github.com/iluwatar/java-design-patterns/blob/master/abstract-factory/src/main/java/com/iluwatar/abstractfactory/ElfKingdomFactory.java)

Esse exemplo implementa o Abstract Factory com **reinos** (*kingdoms*): a interface `KingdomFactory` declara os métodos de criação de uma **família de produtos** (`createCastle`, `createKing`, `createArmy`), e cada fábrica concreta (como o `ElfKingdomFactory`, ou o `OrcKingdomFactory` do mesmo pacote) cria uma família **coerente**. É a mesma estrutura vista em aula com a `FabricaMobilia`.

Responda:

### 1. Que **família de produtos** a interface `KingdomFactory` produz? Liste os métodos de criação.
R: 

### 2. No `ElfKingdomFactory`, que **objetos concretos** são criados por cada método? O que aconteceria se um desses métodos retornasse, por engano, um produto **orc** (por exemplo, `OrcCastle`)? (coerência da família)
R: 

### 3. O cliente do exemplo (a classe `App` do mesmo pacote) monta o reino recebendo uma fábrica. O que precisaria mudar no código do cliente para trocar de reino **elfo** para **orc**? Relacione com o princípio **OCP**.
R: 

---

# Exercício 5: Implementação

Imagine que você foi contratado para criar o **sistema de vendas de móveis** de uma loja.

A loja vende **kits de sala de estar** em dois estilos — **moderno** e **vitoriano** (opcionalmente, adicione um terceiro: **art déco**). Cada kit é composto por:

- uma **cadeira**,
- um **sofá**,
- uma **mesa de centro**.

O cliente espera que os três móveis do kit **combinem entre si**. Se o código criar cada peça com `new` espalhado, nada impede de misturar estilos.

### Sua missão

Implemente, em Java, um sistema que **produza famílias coerentes de móveis**, usando o padrão **Abstract Factory**.

1. Crie as **interfaces de produto**:
   - `Cadeira` com método `assentar()`.
   - `Sofa` com método `deitar()`.
   - `MesaDeCentro` com método `apoiar()`.
   - Cada método deve imprimir uma mensagem identificando o **produto e o estilo**.
2. Crie os **produtos concretos** para cada família, por exemplo:
   - `CadeiraModerna`, `SofaModerno`, `MesaDeCentroModerna`
   - `CadeiraVitoriana`, `SofaVitoriano`, `MesaDeCentroVitoriana`
   - (opcional) `CadeiraArtDeco`, `SofaArtDeco`, `MesaDeCentroArtDeco`
3. Crie a **fábrica abstrata** `FabricaMobilia`:
   - Métodos: `criarCadeira()`, `criarSofa()`, `criarMesaDeCentro()`.
4. Crie as **fábricas concretas**:
   - `FabricaMobiliaModerna`, `FabricaMobiliaVitoriana` (e `FabricaMobiliaArtDeco` se quiser), cada uma criando apenas os produtos do seu estilo.
5. Crie a classe `ConfiguradorDeSala` (o **cliente**):
   - Recebe uma `FabricaMobilia` (por exemplo, no construtor).
   - Monta a sala usando apenas os métodos da fábrica e um método `exibir()` que mostra os três móveis.
6. Crie uma classe de teste, por exemplo `Main`, que:
   - Crie salas com **cada fábrica** e mostre que todas ficam **coerentes** (todos os móveis do mesmo estilo).
   - Troque a fábrica (ex.: de moderna para vitoriana) com **uma única mudança** na inicialização.
   - Explique, em um comentário ou README, o que aconteceria se alguém tentasse misturar móveis de famílias diferentes e por que o **Abstract Factory** evita isso.

- R: