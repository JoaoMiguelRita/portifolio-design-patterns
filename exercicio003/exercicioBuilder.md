# Exercício 1: Aplicações

### 1. Uma classe `ConfiguracaoServidor` que representa as configurações de conexão de um serviço, com campos como `host`, `porta`, `timeout`, `quantidadeDeTentativas`, `usarSSL` e `proxy`, onde a maioria dos campos é opcional e varia entre ambiente de desenvolvimento e produção.
- Faz sentido usar Builder, visto que os campos nem sempre são obrigatórios, minimiza diversos constructors, e facilita a criação do objeto nos diferentes cenários.

### 2. Um componente responsável por **montar requisições HTTP** para chamadas a APIs externas, onde o código encadeia URL, método, cabeçalhos e corpo de forma legível (`Request.Builder` do OkHttp).
- Faz sentido usar Builder, pois existem inumeras formas de exibir metodos, cabeçalhos, um builder bem contruido simplificaria a criação.

### 3. Uma classe simples `Ponto` com apenas **dois campos obrigatórios** (`x`, `y`), utilizada em um sistema de CAD e instanciada dezenas de vezes por segundo.
- Não faz sentido usar Builder, visto que sempre será informado os dois valores, simplifica o código pois não há necessidade de criar um canhão para uma formiga.

### 4. Um `RelatorioFinanceiro` com muitas opções de configuração: título, período, filtros, ordenação, formato de saída (PDF, CSV), marca d'água e rodapé, sendo que diferentes módulos geram combinações diferentes sem alterar o código de montagem.
- Faz sentido usar Builder, ainda mais pelos diferentes modulos gerarem combinações diferentes, como é um relatório financeiro é até valido criar um director para padronizar alguns valores como impostos por exemplo.

### 5. Uma classe `Produto` com três campos obrigatórios (`nome`, `preco`, `quantidadeEstoque`), criada em um único ponto do sistema através do construtor tradicional.
- Faz sentido usar Builder, mas depende, a principio não, porém se esse produto se trata de uma empresa que tende a crescer, então sim é valido criar, pois diversos campos ainda vão surgir na classe Produto

# Exercício 2: Analogia

### Crie uma **analogia própria** para explicar o padrão Builder para alguém que não é da área de TI.

#### Descreva uma situação do **mundo real** em que:
   - um objeto seja **montado passo a passo** (etapas bem definidas),
   - existam **partes obrigatórias** e **partes opcionais**,
   - e as **mesmas etapas** possam gerar **variações** diferentes do resultado final.

2. Explique **por que** essa analogia representa bem:
   - a separação entre **o que é montado** (Produto) e **como é montado** (Builder),
   - a ideia de montagem em **passos claros**,
   - e a possibilidade de **variar combinações** sem criar um novo processo do zero.
3. Indique também **uma limitação** da sua analogia (algo que não encaixa perfeitamente com o padrão).

- O cenário de exemplo é a criação de um `Carro`, nele tenho alguns atributos padrões obrigatórios como marca, pneu, modelo, cor, categoria, quantidadePortas, e diversos outros atributos. Imagine que preciso definir um Ford Ka, preciso criar o objeto com marca: Ford, quantidadePortas: 4, cor: prata, pneu: 20m. Agora preciso criar um supercarro, Dodge Charllenger, lá vou eu, cor: preta, marca: Dodge, portas: 2. Percebe que existem diferentes atributos para construir um mesmo objeto, podendo até não existir atributos em um, que vou ter em outro. Agora vou além, eu posso ter padrões para criação de carro, por exemplo criação padrão de carro popular, modelo padrão de carro esportivo, modelo padrão de carro cargueiro, posso ter uma máquina padrão que só controi um tipo de carro, e eu só comando e direciono para qual esteira deve ir a criação do carro. Um ponto um pouco negativo da analogia é que os atributos que eu pensei todo o carro vai ter, porém em quantidades diferentes. Eu teria que especificar em algum filho do carro, por exemplo: modeloTetoSolar. 

# Exercício 3: Anti-pattern

Considere o código Java abaixo, usado em uma aplicação de e-commerce:

````java
public class Pedido {

    private final String cliente;
    private final String endereco;
    private final List<String> itens;
    private final double desconto;
    private final String cupom;
    private final double frete;
    private final String observacoes;

    public Pedido(String cliente) { ... }

    public Pedido(String cliente, String endereco) { ... }

    public Pedido(String cliente, String endereco, List<String> itens) { ... }

    public Pedido(String cliente, String endereco, List<String> itens, double desconto) { ... }

    public Pedido(String cliente, String endereco, List<String> itens, double desconto, String cupom) { ... }

    public Pedido(String cliente, String endereco, List<String> itens, double desconto, String cupom, double frete, String observacoes) { ... }

    // ...outros métodos de regras de negócio...
}
````

Responda:

### 1. Por que esse uso de **construtores sobrecarregados (telescópicos)** é um problema de design?
- Torna dificil manutenção, sempre que eu precisar adicionar um atributo novo preciso criar um novo contructor. Isso cria uma poluição de código feio.

### 2. Que tipo de **bugs ou confusões** podem acontecer quando um desenvolvedor cria um `Pedido` chamando esses construtores?
- Regras de negócio podem complicar, meu novo atributo será obrigatório, tenho que verificar todos os lugares que criam um objeto Pedido, da trabalho ficar caçando essas migalhas.

### 3. O que acontece com esse código a cada **novo campo opcional** adicionado à `Pedido`? Quantos construtores seriam necessários para `N` campos opcionais?
- Teria que ter um construtor para cada atributo, fazendo uma escada de contrutores.

### 4. Sugira **outra abordagem de design** usando o padrão **Builder** e explique, em linhas gerais, como a criação do objeto passaria a funcionar (construtor privado, Builder interno, métodos fluentes e `build()`).
- Passaria a ter somente um constructor, e nisso teria um createBuilder responsavél para criar os builders de cada atributo. Cada atributo no builder passa a retornar a propria instancia, assim, valores que não foram criados nem passam a setar o valor.

# Exercício 4: Exemplo real

#### Acesse o seguinte arquivo em um projeto open source:

- Projeto: OkHttp (biblioteca HTTP para Java/Kotlin)
- Arquivo: [`Request.kt`](https://github.com/square/okhttp/blob/master/okhttp/src/commonJvmAndroid/kotlin/okhttp3/Request.kt)

#### O OkHttp é uma das bibliotecas mais utilizadas para requisições HTTP em Java/Kotlin. A classe `Request` representa uma requisição HTTP **imutável** a partir do momento em que é criada. Responda:

### 1. Procure explicar, em linhas gerais, **por que** a classe `Request` é imutável e **qual o papel** do seu `Builder` nessa garantia.
- Seu proprio constructor chama seu builder, e permite que ele defina se a requisição se trata de um get ou post.

### 2. Observe os métodos de configuração da classe `Builder` (por exemplo, `url(...)`, `header(...)`, `method(...)`, `get()`, `post(body)`). O que esses métodos têm em comum no **tipo de retorno** e por que isso permite o encadeamento fluente visto em aula?
- Acho que todos retornam o proprio tipo do atributo passado para ela, e isso permite que possa ser enviado qualquer tipo de atributo, que ele retorna correto. Além disso, o builder por si só sempre retorna uma instancia, assim permite que eu crie no objeto somente o que foi mandado construir.

### 3. Analise o método `build()` e as verificações feitas nesse exemplo (por exemplo, `checkNotNull(builder.url)`). Relacione esse comportamento com a validação de **campos obrigatórios** do `build()` visto em aula.
- No exemplo dado, o builder garante que url nunca seja null, mesmo que faça com que seja exibido um texto como explicação do motivo.


### 4. O `Builder` desse exemplo é uma classe **interna**. Compare com o que foi apresentado na aula sobre Builder interno `static` e comente se a relação entre o `Builder` e o produto final é a mesma que você aprendeu.
- Sim é o mesmo, porém aplicado em forma diferente do comum, internal referencia a classe que está criando ele.

# Exercício 5: Implementação

### Imagine que você foi contratado para criar o **sistema de montagem de lanches** de uma lanchonete.

### Sua missão

#### Implemente, em Java, um sistema que monte lanches de forma **fluente e passo a passo**, usando o padrão **Builder**.

- Fiquei com preguiça de fazer tudo sor, mas segue o que fiz:

```
public class Hamburguer {
    private String pao;
    private String proteina;
    private String queijo;
    private String vegetais;
    private String molho;
    private Boolean bemPassado;
    private String observacoes;

    private Hamburguer(Builder builder) {
        setPao(builder.pao);
        setProteina(builder.proteina);
        setQueijo(builder.queijo);
        setVegetais(builder.vegetais);
        setMolho(builder.molho);
        setBemPassado(builder.bemPassado);
        setObservacoes(builder.observacoes);
    }

    public String getPao() {
        return pao;
    }

    public void setPao(String pao) {
        this.pao = pao;
    }

    public String getProteina() {
        return proteina;
    }

    public void setProteina(String proteina) {
        this.proteina = proteina;
    }

    public String getQueijo() {
        return queijo;
    }

    public void setQueijo(String queijo) {
        this.queijo = queijo;
    }

    public String getVegetais() {
        return vegetais;
    }

    public void setVegetais(String vegetais) {
        this.vegetais = vegetais;
    }

    public String getMolho() {
        return molho;
    }

    public void setMolho(String molho) {
        this.molho = molho;
    }

    public Boolean getBemPassado() {
        return bemPassado;
    }

    public void setBemPassado(Boolean bemPassado) {
        this.bemPassado = bemPassado;
    }

    public String getObservacoes() {
        return observacoes;
    }

    public void setObservacoes(String observacoes) {
        this.observacoes = observacoes;
    }

    public static final class Builder {
        private String pao;
        private String proteina;
        private String queijo;
        private String vegetais;
        private String molho;
        private Boolean bemPassado;
        private String observacoes;

        public Builder() {
        }

        public Builder pao(String val) {
            pao = val;
            return this;
        }

        public Builder proteina(String val) {
            proteina = val;
            return this;
        }

        public Builder queijo(String val) {
            queijo = val;
            return this;
        }

        public Builder vegetais(String val) {
            vegetais = val;
            return this;
        }

        public Builder molho(String val) {
            molho = val;
            return this;
        }

        public Builder bemPassado(Boolean val) {
            bemPassado = val;
            return this;
        }

        public Builder observacoes(String val) {
            observacoes = val;
            return this;
        }

        public Hamburguer build() {
            return new Hamburguer(this);
        }
    }
}

public class HamburguerDirector{
    public void criarHamburguerTradicional(Hamburguer.Builder hambuerguerBuilder){
        hambuerguerBuilder.pao("Gengre");
        hambuerguerBuilder.bemPassado(true);
        hambuerguerBuilder.proteina("Carne moida");
        hambuerguerBuilder.molho("Maionese");
        hambuerguerBuilder.vegetais("Alface");
        hambuerguerBuilder.queijo("mussarela");
    }

    public void criarHamburguerBacon(Hamburguer.Builder hambuerguerBuilder){
        hambuerguerBuilder.pao("Gergelim");
        hambuerguerBuilder.bemPassado(false);
        hambuerguerBuilder.proteina("Carne moida e bacon");
        hambuerguerBuilder.molho("Maionese");
        hambuerguerBuilder.queijo("Gorgonzola");
    }

    public void criarHamburguerChiken(Hamburguer.Builder hambuerguerBuilder){
        hambuerguerBuilder.pao("Masio");
        hambuerguerBuilder.bemPassado(false);
        hambuerguerBuilder.proteina("Frango empanado");
        hambuerguerBuilder.molho("Maionese");
        hambuerguerBuilder.vegetais("Alface e tomate");
        hambuerguerBuilder.queijo("mussarela");
    }
}
```

