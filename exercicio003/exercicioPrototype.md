# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Prototype** é apropriado ou **não** e **justifique em 2–3 frases**.

#### 1. Um sistema de relatórios financeiros em que os relatórios mensais nascem de um **template base** com cabeçalho, rodapé e seções padrão, mudando apenas os valores de cada mês.
- Faz sentido usar Prototype, a copia que será retirada desse padrão serve de base para a criação dos novos relatórios.

#### 2. Uma classe simples `Ponto` com apenas **dois campos obrigatórios** (`x`, `y`), utilizada em um sistema de CAD e instanciada dezenas de vezes por segundo.
- Não faz sentido usar Prototype, não tem necessidade, o objeto possuim somente dois campos e eles sempre serão diferentes.

#### 3. Um sistema de jogos em que os inimigos possuem **muita configuração** (estatísticas, equipamentos, habilidades) e várias fases criam variações de um mesmo guerreiro repetindo o mesmo código de construção.
- Faz sentido usar Prototype, repetição de atributos, é valido sim aplicar o prototype, e economizar tempo.

#### 4. Um módulo em que o cliente **não deve conhecer as classes concretas** dos objetos que precisa, podendo apenas pedir uma cópia pelo **nome do modelo** (por exemplo, `"guerreiro"`, `"mago"`) através de um registro.
- Faz sentido usar Prototype, o prototype permite que seja realizado a cópia para consulta do atributo.

#### 5. Uma classe `Produto` com três campos obrigatórios (`nome`, `preco`, `quantidadeEstoque`), criada em um único ponto do sistema através do construtor tradicional e sem variações.
- Não faz sentido usar Prototype, mas talvez o sistema possa evoluir para fazer sentido de usar o prototype, para copiar valores padrões de pedido.

# Exercício 2: Analogia

### Crie uma **analogia própria** para explicar o padrão Prototype para alguém que não é da área de TI.
- Imagine que eu tenho que criar um salgadinho novo, mas sem perder a identidade visual do que já existe. Para isso posso realizar uma cópia para capturar algumas definições padrões dessa minha marca, como cor, logo, tamanho, formato.

# Exercício 3: Anti-pattern

Considere o código Java abaixo, usado em um jogo para criar os inimigos de cada fase:

````java
public class Fase {
    private Inimigo criarCopia(Inimigo antigo) {
        Inimigo copia = new Inimigo();
        copia.setTipo(antigo.getTipo());
        copia.setVida(antigo.getVida());
        copia.setDano(antigo.getDano());
        copia.setArma(antigo.getArma());
        return copia;
    }
}

public class Inimigo {
    private String tipo;
    private Double vida;
    private Double dano;
    private Arma arma; // Arma é um objeto mutável com nome e bônus de dano

    // getters e setters...
}
````

### Responda:

#### 1. Por que essa forma de **copiar campo a campo** é um problema de design?
- A referencia do objeto nesse cenário é o mesmo, assim quando um personagem tomar dano, todos vão tomar o mesmo dado, perdendo a mesma quantia de vida.

#### 2. Que tipo de **bugs ou comportamentos estranhos** podem acontecer se um novo campo for adicionado a `Inimigo` e o `criarCopia` não for atualizado?
- Quando um personagem pegar uma arma, todos os personagem pegam a arma.

#### 3. Observe `copia.setArma(antigo.getArma())`. O que acontece se o clone **compartilhar** a mesma instância de `Arma` do original e o jogo modificar a arma de um deles? Qual conceito visto em aula isso ilustra (cópia rasa × profunda)?
- Cópia rasa, pois ao modificar um atributo de um Inimigo, todos os outros serão afetados mesmo que seja uma cópia, pois todos os objetos compartilham da mesma referencia.

# Exercício 4: Exemplo real

### Acesse os seguintes arquivos no projeto open source do JDK:

- Projeto: OpenJDK
- Arquivos:
  - [`Object.java`](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/lang/Object.java) (método nativo `clone()`)
  - [`Cloneable.java`](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/lang/Cloneable.java)

### Responda:

#### 1. Observe as exigências do `clone()` nativo: retorna `Object` (exigindo cast), pode lançar `CloneNotSupportedException` e depende de `super.clone()`. Compare esse mecanismo com o **contrato de clonagem** visto em aula (interface com método `clonar()` + construtor de cópia). Qual é mais próximo da ideia do GoF de Prototype e por quê?
- Eu não entendi direito o clone do código, mas o clone que vimos em aula, com o construct chega mais próximo da ideia do prototype.

#### 2. Vimos que a cópia rasa faz o clone compartilhar os objetos internos com o original. No `clone()` nativo do Java, o comportamento padrão é a **cópia rasa** ou **cópia profunda**?
- cópia rasa.

# Exercício 5: Implementação (JM A FAZER)

Imagine que você foi contratado para criar o **sistema de inimigos** de um jogo.

O jogo possui vários tipos de inimigo (`guerreiro`, `mago`, `arqueiro`, `chefe`), cada um com **muita configuração** (tipo, vida, dano e uma `Arma`). Como os inimigos nascem de uma **base parecida** e cada fase precisa de várias variações, recriar cada um com `new` repetiria muito código de construção.

### Sua missão

Implemente, em Java, um sistema que crie inimigos **por cópia de protótipos**, usando o padrão **Prototype**.

1. Crie a classe `Arma`:
   - Campos, por exemplo, `nome` e `bonusDano`, com getters e setters (objeto **mutável**).
   - Um método `clonar()` que retorna uma nova `Arma` independente (para a cópia profunda).
2. Crie a interface `InimigoPrototype`:
   - Contrato de clonagem: `InimigoPrototype clonar()`.
3. Crie a classe `Inimigo implements InimigoPrototype`:
   - Campos: `tipo`, `vida`, `dano` e `Arma arma`.
   - Um **construtor de cópia** (`Inimigo(Inimigo base)`) que copia os atributos simples e faz a **cópia profunda** da `Arma` (usando `arma.clonar()`).
   - Um método `clonar()` que retorna `new Inimigo(this)`.
   - Getters e setters para ajustar variações após a cópia.
4. Crie a classe `RegistroDePrototipos`:
   - Um `Map<String, InimigoPrototype>` que guarda os protótipos prontos (`"guerreiro"`, `"mago"`, `"arqueiro"`, `"chefe"`).
   - Um método, por exemplo, `getPrototipo(String nome)`, que **retorna o clone** do protótipo (nunca o próprio protótipo).
   - O cliente pede a cópia **pelo nome**, sem conhecer a classe concreta `Inimigo`.
5. Crie uma classe de teste, por exemplo `Main`, que:
   - Obtenha inimigos via `RegistroDePrototipos` pelos nomes.
   - Crie um **inimigo elite** a partir do `"guerreiro"` (por exemplo, `setVida(...)` e `setDano(...)` maiores) e mostre que o protótipo original **não muda**.
   - Prove que os clones são **objetos distintos** (por exemplo, comparando referências ou `hashCode()`).
   - Demonstre a **cópia profunda**: modifique a `Arma` de um clone e mostre que a `Arma` de outro clone (e do protótipo) permanece intacta.
   - Se quiser, mostre também o que aconteceria se a cópia da `Arma` fosse **rasa** (compartilhando a mesma instância).
