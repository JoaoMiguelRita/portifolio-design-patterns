# Exercício 1: Aplicações

### 1. Um serviço de **notificações** que precisa enviar mensagens por **email**, **SMS** e **push**. O fluxo de envio é sempre o mesmo (montar a mensagem, enviar, registrar log), mas cada canal entrega de um jeito diferente, e novos canais entram frequentemente.
- Faz sentido usar Factory Method, cada forma de mandar mensagem tem sua particularidade, e os métodos tem o mesmo principio, enviar mensagem.

### 2. Uma classe simples `Ponto` com apenas **dois campos obrigatórios** (`x`, `y`), utilizada em um sistema de CAD e instanciada dezenas de vezes por segundo através do construtor tradicional.
- Não faz sentido usar Factory Method, não adianta criar um canhão pra mantar a formiga, o Factory Method é indicado para diversas classes que realizam metodos semelhantes.

### 3. Um framework de **exportação de relatórios** (PDF, CSV, Excel). O módulo principal **não deve conhecer** as classes concretas de exportação, e novas exportações devem entrar apenas com novas subclasses, sem alterar o fluxo de geração.
- Faz sentido usar Factory Method, permite evoluir para novos métodos de exportação, o módulo principal não conhece as classes concretas.

### 4. Uma aplicação que precisa criar um **cliente HTTP** diferente conforme o ambiente: uma implementação **mock** nos testes e uma **real** (OkHttp) em produção, sem que o restante do código mude.
- Faz sentido usar Factory Method, permite diferencia as mesmas funções com cenários diferentes.

### 5. Uma classe `Produto` com três campos obrigatórios (`nome`, `preco`, `quantidadeEstoque`), criada em um único ponto do sistema através do construtor tradicional e sem variações.
- Não faz sentido usar Factory Method, muito simples para se aplicar.

# Exercício 2: Analogia

### Crie uma **analogia própria** para explicar o padrão Factory Method para alguém que não é da área de TI.

- Imagine que você pode fazer um buquê, existem diferentes formas de montar esse buquê, você pode utilizar um papel especifico, flores especificas, cores especificas, ramos e etc. Eu não preciso saber exatamente como é produzido o buquê. Mas ele pode se utilizar de um mesmo papel de outro buquê, um ramo comum, e ainda assim serão produtos diferentes, porém com um mesmo padrão

---

# Exercício 3: Anti-pattern

Considere o código Java abaixo, usado para enviar notificações aos usuários de um sistema:

```java
public class NotificadorService {

    public void enviar(String canal, String destinatario, String mensagem) {
        if (canal.equals("email")) {
            new EmailNotificador().enviar(destinatario, mensagem);
        } else if (canal.equals("sms")) {
            new SmsNotificador().enviar(destinatario, mensagem);
        } else if (canal.equals("push")) {
            new PushNotificador().enviar(destinatario, mensagem);
        }
    }
}

public class EmailNotificador {
    public void enviar(String destinatario, String mensagem) { /* ... */ }
}

public class SmsNotificador {
    public void enviar(String destinatario, String mensagem) { /* ... */ }
}

public class PushNotificador {
    public void enviar(String destinatario, String mensagem) { /* ... */ }
}
```

Responda:

### 1. Por que essa forma de **escolher o canal com `if/else`** é um problema de design?
- Caso venha a conter novas formas de enviar mensagem, precisarei aplicar ela ao else ifs, dando mais trabalho.

### 2. O que precisa ser alterado para adicionar um novo canal (por exemplo, **WhatsApp**)? Que riscos essa mudança traz para o código que já foi testado?
- Precisa testar todos os envios anteriores para certificar que eles se mantiveram corretos, visto que a classe afetada diretamente é a mesma que realiza todos os canais de envios.

### 3. Proponha uma solução usando o padrão **Factory Method**, explicando em linhas gerais: a interface do produto (`Notificador`), o criador abstrato com o **método fábrica** e os **criadores concretos** por canal.
- Centralizar as notificações em uma interface que possui o método enviarMensagem(). Em seguida preciso criar uma classe abstrata Comunicador que conhece a função enviarMensagem, pois ele pega da abstração de Notificador, notificador esse que é implementado em suas formas de envio de mensagem. Agora a parte mais importante, precisa criar os criadores concretos. Que serão mais classes que unirão alguns padrões. Então poderia ter um AlertaDefesaCivel, que implementa os 3 canais, ou NotificacaoMensagem que implementa dois, aqui é onde se define talvez uma parte utilizavél, ou permite que formas futuras possam se utilizar, ou simplesmente serem criadas novas formas de notificar sem se preocupar em quebrar as anteriores.

---

# Exercício 4: Exemplo real

Acesse os seguintes arquivos em um projeto open source:

- Projeto: [`iluwatar/java-design-patterns`](https://github.com/iluwatar/java-design-patterns) (um dos repositórios de padrões de projeto mais populares do GitHub)
- Arquivos:
  - [`Blacksmith.java`](https://github.com/iluwatar/java-design-patterns/blob/master/factory-method/src/main/java/com/iluwatar/factory/method/Blacksmith.java)
  - [`ElfBlacksmith.java`](https://github.com/iluwatar/java-design-patterns/blob/master/factory-method/src/main/java/com/iluwatar/factory/method/ElfBlacksmith.java)

Esse exemplo implementa o Factory Method com **ferreiros** (*blacksmiths*): a interface `Blacksmith` declara o **método fábrica** `manufactureWeapon(...)`, e cada ferreiro concreto (como o `ElfBlacksmith`, ou o `OrcBlacksmith` do mesmo pacote) **decide** qual arma criar. É a mesma estrutura vista em aula com a `Logistica` e o método `criarTransporte()`.

Responda:

### 1. Que papel a interface `Blacksmith` exerce no padrão? Qual é o **método fábrica** declarado nela e qual o seu **tipo de retorno**?
- Ela tem o papel de evidenciar qual método será implementado outras classes para produzir objetos, e tem o nome manufactureWeapon() e retorna Weapon.

### 2. No `ElfBlacksmith`, o que o método `manufactureWeapon(...)` retorna? Por que o tipo de retorno é a **interface** `Weapon` e não a classe concreta (`ElfWeapon`)?
- Weapon, pois ela permite que seja criada a Weapon, e quem define como será usada é o Client, ampliando sua gama de utilização.

### 3. O cliente do exemplo (a classe `App` do mesmo pacote) trabalha com `Blacksmith` e `Weapon` — **abstrações** — e troca de ferreiro (elfo ou orc). O que seria necessário para adicionar um novo ferreiro (por exemplo, um **anão**) **sem alterar** o código que usa as armas? Relacione com o princípio **OCP**.
R: Bastaria criar uma nova classe para a criação do anão, com essa classe implementado Blacksmith. E a forma como usa a arma seria definida dentro da classe do

---

# Exercício 5: Implementação

Imagine que você foi contratado para criar o **Sistema de Notificações** de um portal acadêmico.

O sistema precisa notificar os alunos por diferentes canais (**email**, **SMS** e **push**) sempre seguindo o mesmo fluxo:

- montar a mensagem,
- escolher o canal,
- enviar,
- registrar no log.

Como o fluxo é sempre o mesmo e só muda **qual canal** é criado, repetir o envio com `new` para cada canal deixaria o código acoplado às classes concretas.

### Sua missão

Implemente, em Java, um sistema que **centralize a criação do notificador** em um **método fábrica**, usando o padrão **Factory Method**.

1. Crie a interface `Notificador`:
   - Contrato: `void enviar(String destinatario, String mensagem)`.
2. Crie pelo menos **três produtos concretos**:
   - `EmailNotificador`, `SmsNotificador` e `PushNotificador`, cada um implementando `Notificador` e imprimindo (ou registrando) uma mensagem que identifique o canal usado.
3. Crie a classe abstrata `NotificacaoService` (o **Creator**):
   - Um método público `notificar(String destinatario, String mensagem)` que representa o **fluxo comum** (montar mensagem, chamar o método fábrica, enviar, registrar log).
   - Um **método fábrica** abstrato `protected abstract Notificador criarNotificador();`.
4. Crie os **criadores concretos**:
   - `EmailService`, `SmsService` e `PushService`, que estendem `NotificacaoService` e sobrescrevem `criarNotificador()` devolvendo o notificador correspondente.
5. Crie uma classe de teste, por exemplo `Main`, que:
   - Instancie `EmailService`, `SmsService` e `PushService`.
   - Envie a mesma mensagem pelos três canais usando o método `notificar(...)`.
   - Mostre que o **fluxo é o mesmo** e apenas o **canal criado** muda (ex.: a saída identifica o canal).
   - Explique, em um comentário ou README, como adicionar um novo canal (ex.: `WhatsApp`) exigiria **apenas** uma nova subclasse de `Notificador` e uma nova subclasse de `NotificacaoService`, sem alterar o fluxo existente.

- R: