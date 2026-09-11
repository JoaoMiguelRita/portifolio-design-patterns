# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Composite** é apropriado ou **não** e **justifique em 2–3 frases**.

### 1. Um explorador de arquivos que precisa listar e calcular o tamanho de pastas, que podem conter arquivos **ou outras pastas** (estrutura recursiva).
- Faz sentido usar Composite, é um cenário típico no qual é gerado uma árvore de formas variáveis.

### 2. Um cardápio digital de um restaurante em que **seções** podem conter itens de cardápio **ou outras subseções**, e o total de calorias de uma seção deve somar todos os itens abaixo dela.
- Faz sentido usar Composite, é um cenário típico no qual é gerado uma árvore de formas variáveis.

### 3. Um motor de interface gráfica em que um **painel** pode conter controles simples (botões, rótulos) **ou outros painéis** aninhados, e o sistema precisa desenhar/ocultar qualquer elemento da mesma forma.
- Faz sentido usar Composite, é um cenário típico no qual é gerado uma árvore de formas variáveis, podendo ter "pais" nos "filhos".

### 4. Um cadastro de produtos com uma **lista simples e plana** (nome, preço, estoque) que nunca terá itens compostos ou hierarquia.
- Não faz sentido usar Composite, uma lista simples não exige tamanha necessidade de aplicação do padrão.

### 5. Um sistema de uma rede de lojas em que cada loja pertence a uma região, cada região a um estado e cada estado ao país, e é preciso, a partir de qualquer nível, somar o **faturamento de tudo abaixo** daquele nó.
- R: Faz sentido usar Composite, é válido a criação, visto que em uma região pode ter um monte de estados, e países, ainda mais com a contagem separada do faturamento.


# Exercício 2: Analogia

### Crie uma **analogia própria** para explicar o padrão Composite para alguém que não é da área de TI.

### Na aula, usamos o **organograma de uma empresa**: um departamento contém funcionários ou sub-departamentos, e "quantas pessoas há abaixo de X" soma a árvore recursivamente. Agora crie uma **outra** analogia.
- Divisão de centros de custos dentro de um sistema de saúde. Imagine que eu tenha um centro de custo unidade, e abaixo dele tenho farmácia, almoxarifado, upa, hospital, ubs, e para cada um desses tenha mais alguns vários centros de custos farmacia 1, farmacia 2 ... E para cada um preciso saber o número x de tal produto do seu nó para baixo.

# Exercício 3: Anti-pattern

### Considere o código Java abaixo, usado em um sistema de pedidos de uma loja:

````java
public class Produto {
    String nome;
    double preco;
}

public class Caixa {           // pode conter produtos OU outras caixas
    String nome;
    List<Object> itens = new ArrayList<>();
}

public class Pedido {
    public double calcularTotal(Object item) {
        if (item instanceof Caixa c) {
            double soma = 0;
            for (Object filho : c.itens)
                soma += calcularTotal(filho);   // recursão "externa"
            return soma;
        } else if (item instanceof Produto p) {
            return p.preco;
        }
        return 0;
    }

    public void imprimir(Object item) {
        // ... de novo, um if para Caixa e outro para Produto ...
    }
}
````

### Responda:

### 1. Por que calcular o total **fora** dos objetos, usando `instanceof`, é um problema de design? Onde mora a lógica da árvore e o que acontece se outro trecho do sistema precisar percorrer a mesma estrutura?
- Não consigo acessar os dados especificos do produto filho. Apesar da recursividade funcionar. Além de que, pode acontecer um stack overflow. 

### 2. O que acontece ao adicionar um novo tipo (ex.: um `ProdutoComDesconto` ou um serviço de montagem)? Que bugs ou confusões esse código tende a gerar?
- Contagem incorreta dos valores, pois todos são Object, e não uma classe específica.

### 3. Proponha uma solução usando o padrão **Composite**, explicando os papéis: a **interface comum** (`Component`), a **folha** (`Produto`) e o **composto** (`Caixa`). Onde a recursão passa a morar e por que o cliente passa a chamar **um único método** sem distinguir os tipos?
- Criar uma interface ProdutoObjeto declarando nome e preco. Agora vamos preencher nossa caixa, criando os produtos que vão ter dentro dela. Diagmos que tenha uma outra caixa com bolinhas de gude, e uma outra é um ioio solto. Sendo assim são mais duas classes que precisam ser criadas implementando nossa interface ProdutoObjeto, vamos chmar elas de BolinhasGude e IoIo. Por estar implementando a interface deve ser obrigatoriamente definido seu nome e preço. Agora vamos criar nossa classe Caixa, que também vai implementar ProdutoObjeto, essa classe vai ter nome e uma lista de ProdutoObjeto. Em seguida um construtor para definir que sua criação sempre exiga seu nome ao menos, também um metodo de adicionar que permite inserir um ProdutoObjeto na lista. E para ficar filé, um método getPreco, que vai fazer a recursão da lista de ProdutoObjeto para somar o valores de todos os objetos que tem dentro dela.
- Por fim é só definir certinho no client, ficaria algo como:
````java
    public static void main(String[] args) {
        BolinhasGude bolinhasGude = new BolinhasGude("Bolinhas de Gude", 10.0);
        IoIo ioio = new Ioio("Ioio top", 20.0);

        Caixa caixaDeBolinhasGude = new Combo("Caixa de bolinhas de gude");
        caixaDeBolinhasGude.adicionar(bolinhasGude);

        Caixa caixa = new Combo("Presente");
        caixa.adicionar(caixaDeBolinhasGude);
        caixa.adicionar(ioio);

        System.out.println(caixa.getNome() + ": " + caixa.getPreco()); //Presente: 30.0
        System.out.println(caixaDeBolinhasGude.getNome() + ": " + caixaDeBolinhasGude.getPreco()); //Caixa de bolinhas de gude: 10.0
    }
````

# Exercício 4: Implementação

### Imagine que você foi contratado para criar o sistema de **cardápio e comandas** de um restaurante.

### A casa vende **pratos individuais** e **combos promocionais**. Um combo pode conter pratos individuais **e também outros combos** (por exemplo, um "Combo Família" que inclui o "Combo Duplo" dentro dele). Sem o padrão, calcular o preço de um combo exigiria distinguir, item a item, se é um prato ou um sub-combo.

### Implemente, em Java, um sistema que trate **pratos e combos de forma uniforme**, usando o padrão **Composite**.

#### 1. Crie a interface **`ItemMenu`** (o `Component`):
   - Métodos: `String getNome()` e `double getPreco()`.
#### 2. Crie a **folha** `Prato`:
   - Campos `nome` e `preco` (recebidos no construtor).
   - `getPreco()` retorna o preço do prato.
#### 3. Crie o **composto** `Combo`:
   - Campo `nome` e uma lista de `ItemMenu` (filhos).
   - Métodos `adicionar(ItemMenu item)` e `remover(ItemMenu item)`.
   - `getPreco()` deve **somar recursivamente** o preço dos filhos.
   - `getNome()` devolve o nome do combo.
#### 4. Crie uma classe de teste, por exemplo `Main`, que:
   - Monte **pratos** (ex.: `Hamburguer`, `BatataFrita`, `Refrigerante`) com preços.
   - Monte um **`Combo Duplo`** contendo dois hambúrgueres e um refrigerante.
   - Monte um **`Combo Família`** contendo o `Combo Duplo` **dentro dele**, mais batatas e refrigerantes.
   - Imprima o preço de um **prato isolado**, do **combo duplo** e do **combo família**, sempre chamando **o mesmo método** `getPreco()` sobre a interface `ItemMenu`.
   - Mostre que o preço do combo família **já inclui** o preço do sub-combo (recursão).
   - Explique, em um comentário ou README, como adicionar um novo tipo de item (ex.: `BebidaAlcoolica` com imposto especial) exigiria **apenas** uma nova folha, sem alterar o `Combo` nem a `Main`.
