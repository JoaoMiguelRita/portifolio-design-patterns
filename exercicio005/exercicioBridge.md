# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Bridge** é apropriado ou **não** e **justifique em 2–3 frases**.

#### 1. Um sistema de notificações que precisa variar em duas dimensões: o tipo de conteúdo (informativo, alerta, promoção) e o canal de envio (SMS, e-mail, push). Sem o padrão, cada combinação viraria uma classe nova.
- Faz sentido usar Bridge, duas dimensões na qual podem gerar multiplas classes é exatamente onde o padrão do bridge se aplica.

#### 2. Uma aplicação multiplataforma que precisa rodar a mesma lógica sobre APIs de sistema operacional diferentes (Windows, Linux, macOS), sem multiplicar as classes da interface.
- Faz sentido usar Bridge, duas dimensões na qual podem gerar multiplas classes é exatamente onde o padrão do bridge se aplica.

#### 3. Um módulo de persistência em que a mesma lógica de repositório precisa funcionar com MySQL ou PostgreSQL, podendo até trocar de banco em tempo de execução.
- R: Não faz sentido usar Bridge, isso não gera multiplas classes, logo adicionar o bridge torna código desnecessário.

#### 4. Um framework de testes que precisa executar testes em diferentes ambientes com diferentes configurações de hardware.
- Não faz sentido usar Bridge, para realizar testes não há necessidade de utilizar uma factory que é para coisas mais complexas.

# Exercício 2: Analogia

### Crie uma **analogia própria** para explicar o padrão Bridge para alguém que não é da área de TI.

- Pode se pensar em um computador, ele evoluiem com a troca de suas peças do interior, com suas combinações, caso eu troque de máquina, posso inclusive utilizar o mesmo gabinete e mesmos exteriores.

# Exercício 3: Anti-pattern

#### Considere o código Java abaixo, usado em um editor gráfico:

````java
public abstract class Forma {
    public abstract void desenhar();
}

public class CirculoVermelho extends Forma {
    @Override
    public void desenhar() {
        System.out.println("Desenhando círculo vermelho");
        // lógica de desenhar círculo + lógica da cor vermelho
    }
}

public class CirculoAzul extends Forma {
    @Override
    public void desenhar() {
        System.out.println("Desenhando círculo azul");
        // mesma lógica de círculo, cor azul
    }
}

public class QuadradoVermelho extends Forma {
    @Override
    public void desenhar() {
        System.out.println("Desenhando quadrado vermelho");
    }
}

public class QuadradoAzul extends Forma {
    @Override
    public void desenhar() {
        System.out.println("Desenhando quadrado azul");
    }
}
````

#### 1. Por que essa hierarquia (uma classe por **combinação** de forma × cor) é um problema de design?
- Toda suas funcionalidades são iguais, apenas elas que são diferentes, sendo assim poderia ser simplificado aplicando bridge.

#### 2. O que acontece ao adicionar uma nova forma (ex.: `Triangulo`) ou uma nova cor (ex.: `Verde`)? Quantas classes existiriam para `N` formas e `M` cores?
- Precisaria ser adicionado mais uma classe 5 classes distintas.

#### 3. Proponha uma solução usando o padrão **Bridge**, explicando em linhas gerais: as **duas hierarquias** (forma e cor), como a `Forma` passaria a **referenciar** uma `Cor` e por que cada lado passaria a evoluir de forma **independente**.
- Definir a interface com os metodos em comuns das classes. Para cada aparelho deve ser implementado o interface. Seguindo é criado uma abstração que tem o papel de delegar o trabalho a inteface.

# Exercício 4: Exemplo real

#### Acesse os seguintes arquivos em um projeto open source:

- Projeto: OpenJFX (a biblioteca JavaFX, da família OpenJDK)
- Arquivos:
  - [`Skin.java`](https://github.com/openjdk/jfx/blob/master/modules/javafx.controls/src/main/java/javafx/scene/control/Skin.java)
  - [`Button.java`](https://github.com/openjdk/jfx/blob/master/modules/javafx.controls/src/main/java/javafx/scene/control/Button.java)

#### O JavaFX separa a **lógica** de um controle da sua **aparência** usando o padrão Bridge: o `Control` (a **abstração**) guarda uma referência a uma `Skin` (a **implementação**) e delega a ela a renderização. Cada controle tem a sua própria skin (por exemplo, o `Button` usa um `ButtonSkin`). Quando o `Control` precisa da skin e ela ainda não existe, ele chama `createDefaultSkin()`. É a mesma estrutura do `ControleRemoto` que delega a um `Dispositivo` vista em aula. Responda:

#### 1. Quais são as **duas hierarquias** do padrão nesse exemplo? Onde está a **abstração** e onde está a **implementação**?
- R:

#### 2. Em `Button.java`, localize o método `createDefaultSkin()`. O que ele retorna? Por que o `Button` **delega** a própria aparência a um `ButtonSkin` em vez de embutir todo o desenho no próprio controle?
- R:

# Exercício 5: Implementação

Imagine que você foi contratado para criar o **sistema de veículos** de uma concessionária.

O sistema precisa montar **veículos** combinando dois conceitos que evoluem de forma independente:

- o **tipo de veículo** (por exemplo, `Sedan` e `SUV`), e
- o **tipo de motor** (por exemplo, `MotorGasolina` e `MotorEletrico`).

Sem o padrão, cada combinação viraria uma classe nova (`SedanGasolina`, `SedanEletrico`, `SuvGasolina`...).

### Sua missão

Implemente, em Java, um sistema que **separe as duas hierarquias**, usando o padrão **Bridge**.

1. Crie a interface de **implementação** `Motor`:
   - Métodos: `void ligar()` e `void acelerar()`.
2. Crie as **implementações concretas**:
   - `MotorGasolina` e `MotorEletrico`, cada um imprimindo mensagens que identificam o motor.
3. Crie a classe abstrata **`Veiculo`** (a **abstração**):
   - Um campo `protected Motor motor` (recebido no construtor).
   - Métodos `void dirigir()` (liga o motor) e `void acelerar()` (delega ao motor).
4. Crie as **abstrações refinadas**:
   - `Sedan` e `Suv`, que estendem `Veiculo` (cada uma pode imprimir o próprio tipo ao dirigir).
5. Crie uma classe de teste, por exemplo `Main`, que:
   - Monte as **quatro combinações** (`Sedan` com `MotorGasolina`, `Sedan` com `MotorEletrico`, `Suv` com `MotorGasolina`, `Suv` com `MotorEletrico`) e as execute.
   - Mostre que o **mesmo veículo** funciona com motores diferentes e que o **mesmo motor** funciona em veículos diferentes.
   - Explique, em um comentário ou README, como adicionar um novo motor (ex.: `MotorHibrido`) ou um novo veículo (ex.: `Picape`) exigiria **apenas** uma nova classe, sem alterar o outro lado.
