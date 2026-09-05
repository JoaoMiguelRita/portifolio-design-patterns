# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Adapter** é apropriado ou **não** e **justifique em 2–3 frases**.

#### 1. Um módulo de previsão do tempo que precisa consultar dois fornecedores externos (SDKs de terceiros) com APIs diferentes e não modificáveis, enquanto o restante da aplicação deve usar uma única interface de consulta.
- Faz sentido usar Adapter, se trata justamente do cenário ideal para utilizar o Adaptar.

#### 2. Uma aplicação que hoje usa uma biblioteca legada de envio de e-mail e pretende trocá-la por outra, sem alterar os serviços que já fazem os envios.
- Faz sentido usar Adapter, pois se trata de aplicações legadas que serão mantidas, e devem ser adaptadas ao modo atual.

#### 3. Um time que decide "preparar o terreno" e já cria adaptadores para todas as classes do sistema, mesmo sem existir hoje nenhuma interface incompatível real.
- Não Faz sentido usar Adapter, ele é criação desnecessária.

### 4. Um `Checkout` que precisa integrar um SDK de pagamento de terceiros para processar os pedidos.
- R: Faz sentido usar Adapter, em virtude das diversas maneiras de pagamento.


# Exercício 2: Analogia

#### Crie uma **analogia própria** para explicar o padrão Adapter para alguém que não é da área de TI.

- Unico caso que consegue pensar foi os fones de ouvido utilizados por diplomatas e presidentes em conferencias. Onde nem todos falam a mesma língua, mas existe uma ferramenta/tradutor que realiza elas em tempo real para se adaptar a cada ouvinte.

# Exercício 3: Anti-pattern

#### Considere o código Java abaixo, usado por um módulo de clima da aplicação:

````java
public class PainelClima {

    public void exibirTemperatura(String cidade, String fornecedor) {
        if (fornecedor.equals("accuweather")) {
            AccuWeatherApi api = new AccuWeatherApi();
            int f = api.getTemperature(cidade);            // retorna °F
            int c = (f - 32) * 5 / 9;                      // conversão manual
            System.out.println("Temperatura: " + c + "°C");
        } else if (fornecedor.equals("openweather")) {
            OpenWeatherApi api = new OpenWeatherApi();
            double c = api.getTempCelsius(cidade);         // outra assinatura
            System.out.println("Temperatura: " + c + "°C");
        }
    }
}
````

#### Além desse painel, **outros dois módulos** da aplicação fazem a mesma consulta repetindo o mesmo código de conversão. Responda:

#### 1. Por que essa forma de **chamar os fornecedores diretamente** é um problema de design?
- Pois o código fica uma massaroca, dificultado expansão, e manutenabilidade.

#### 2. Que tipo de **bugs ou dificuldades** aparecem quando um SDK de terceiros muda (por exemplo, passa a retornar °F) ou quando um novo fornecedor precisa ser adicionado?
- Caso venha a retornar em ºF, a função que realizava a conversão passa a exibir um valor incorreto, assim como a que retorna o valor limpo. E caso precise adicionar um novo fornecedor, seria necessario adicionar essas linhas nesse novo fornecedor. 

#### 3. Proponha uma solução usando o padrão **Adapter**, explicando em linhas gerais: a **interface do cliente**, o(s) **adaptador(es)** por fornecedor e como o `PainelClima` passaria a depender apenas da interface.
- Criar uma centralização, de forma que tenha uma interface com a função de obter a temperatura. Uma classe especificar para a consulta da api externa, assim nos locais aonde forem ser utilizados, devem implementar a interface de obter a temperatura, e a consulta externa busca pela classe especifica. Dessa forma fica centralizado o local de configuração da api externa. Caso ela venha a alterar o retorno, ao corrigir nela, deve corrigir todos os demais pontos.

# Exercício 4: Exemplo real

#### Acesse o seguinte arquivo em um projeto open source:

- Projeto: OpenJDK
- Arquivo: [`InputStreamReader.java`](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/io/InputStreamReader.java)

#### Em Java, um `InputStream` trabalha com **bytes**, enquanto um `Reader` trabalha com **caracteres**. Para ler texto de uma fonte que só fornece bytes (arquivo, socket, etc.), o JDK oferece o `InputStreamReader`: uma classe que **estende `Reader`** (a interface que o cliente usa) e **encapsula um `InputStream`** (recebido no construtor, junto com um charset), convertendo os bytes em caracteres. É exatamente a estrutura do adaptador de objeto vista em aula. Responda:

#### 1. Quais são as duas "interfaces" incompatíveis que o `InputStreamReader` liga? O que a classe **estende** (qual o tipo dela) e o que ela **recebe no construtor**?
- R:

#### 2. Observe os métodos `read()`, `ready()` e `close()`: para onde eles **delegam**? Por que o código cliente que lê texto enxerga **apenas um `Reader`** e nunca precisa conhecer o `InputStream` por baixo?
- R:

#### 3. Onde acontece a conversão de **bytes → caracteres** (dica: observe o campo `StreamDecoder sd`)? Que variação do padrão Adapter essa classe representa e por que ela funciona com **qualquer** `InputStream` (arquivo, socket, memória)?
- R:

# Exercício 5: Implementação

Imagine que você foi contratado para integrar um **módulo de previsão do tempo** a **dois sistemas externos** diferentes.

A sua aplicação precisa consultar a temperatura em **graus Celsius** e não deve depender das classes dos fornecedores. Os fornecedores (SDKs de terceiros, que **não podem ser alterados**) são:

- `OpenWeatherApi` — expõe `public double temperatura(String cidade)` (retorna °C como `double`).
- `AccuWeatherApi` — expõe `public int getTemperature(String cidade)` (retorna °F como `int`).

### Sua missão

Implemente, em Java, um sistema que **adapte os dois fornecedores** a uma interface comum, usando o padrão **Adapter**.

1. Crie a **interface do cliente** `PrevisaoService`:
   - Contrato: `int obterTemperatura(String cidade)` — sempre em **°C**.
2. Crie as classes dos **fornecedores externos** (como se fossem SDKs de terceiros, sem modificar):
   - `OpenWeatherApi` com `public double temperatura(String cidade)`.
   - `AccuWeatherApi` com `public int getTemperature(String cidade)`.
3. Crie os **adaptadores**:
   - `OpenWeatherAdapter implements PrevisaoService`, que encapsula um `OpenWeatherApi` (o valor já vem em °C — converta `double` para `int` com `(int)`).
   - `AccuWeatherAdapter implements PrevisaoService`, que encapsula um `AccuWeatherApi` e **converte de °F para °C** (`(fahrenheit - 32) * 5 / 9`).
4. Crie a classe `PainelClima` (o **cliente**):
   - Recebe uma `PrevisaoService` (por exemplo, no construtor).
   - Método `exibir(String cidade)` que imprime a temperatura usando **apenas a interface**.
5. Crie uma classe de teste, por exemplo `Main`, que:
   - Crie um `PainelClima` com cada fornecedor (passando o adaptador correspondente) e mostre o resultado para uma cidade.
   - Demonstre que o `PainelClima` **não muda** quando o fornecedor é trocado.
   - Explique, em um comentário ou README, como adicionar um terceiro fornecedor exigiria **apenas** uma nova classe de adaptador, sem alterar o `PainelClima`.
