# Exercicio 1

### 1- Um serviço de configurações da aplicação que carrega propriedades de um arquivo (application.properties) e é lido por vários módulos de um sistema web.
- Faz sentido usar Singleton, evita de criar o mesmo objetos com diferentes referencias.

### 2- Um componente responsável por gerenciar conexões HTTP para chamadas a APIs externas, onde diferentes partes do sistema podem precisar de configurações de timeout e autenticação distintas.
- Não sei, mas acredito que mão faz sentido usar Singleton, pois cada requisição terá sua particularidade.

### 3- Um logger central que registra eventos da aplicação em arquivo/console, usado por dezenas de classes diferentes.
- Faz sentido usar Singleton, evita de criar o mesmo objetos com diferentes referencias. Evitando perder um log anterior ao criar um novo objeto.

### 4- Uma classe que representa o usuário autenticado atual em um sistema web com múltiplos usuários acessando simultaneamente.
- Não faz sentido usar Singleton, cada usuário pode realizar uma ação diferente do outro, sendo assim não precisam partir de um único objeto.

### 5- Um cache em memória compartilhado entre vários serviços do back-end, que armazena dados frequentemente lidos do banco.
- Faz sentido usar Singleton, evita de criar o mesmo objetos com diferentes referencias. O cache fica mantido para leitura dos diferentes serviços.

# Exercicio 2

### Dê um exemplo real de aplicação
- Imagina que eu sou um usuário, e você é um usuário também, nós juntos somos dois objetos diferentes na lembrança de um computador. Eu e você podemos interagir com uma máquina de ursinho, e essa máquina de ursinho também é um objeto na memória de um computador. Diagamos que eu vá lá e tire a sorte grande de retirar um ursinho de uma máquina, preciso que o computador entenda que se você for tentar tirar um ursinho da máquina logo em seguida, saiba que o ursinho que eu tirei não existe mais. Pra ficar melhor o exemplo, quero a máquina de ursinho não é daquelas tradicionais, ela é daquela que mais de um jogador pode jogar ao mesmo tempo, assim se encaixa bem o exemplo.

# Exercicio 3

### Por que esse uso de Singleton é um problema arquitetural nesse cenário?
-  Pois com multiplos acessos no mesmo objeto carrinho, o carrinho será o mesmo para os usuários, então ao adicionar ou remover um item, remove para todos.

### Que tipo de bugs ou comportamentos estranhos podem acontecer quando vários usuários utilizarem o sistema ao mesmo tempo?
- Adicionar ou remover um item, remove para todos. Baixa performance por atualização repetidas vezes.

### Sugira outra abordagem de design para o carrinho (sem usar Singleton) e explique, em linhas gerais, como as instâncias deveriam ser gerenciadas.
- Remover o singletons, mantendo o contrutor publico, assim cada usuário teria seu objeto carrinho referenciado na memória, sem afetar o objeto do outro.

# Exercicio 4

### Procure explicar, em linhas gerais, quais funcionalidades estão implementadas nesse exemplo.
- Eu tenho um cara que escuta, um que acumula e outra que faz o recebimento das palavras.

### Quais as classes presentes nesse arquivo que podem ser consideradas Singletons, como vimos em aula? Justifique sua resposta.
- JavaWordExcludeList, JavaDroppedWordsCounter usam singleton.

### As soluções apresentadas no código são thread-safe? Explique o porquê.
- Sim, elas possuem synchronized em seus métodos de get instancia

### Porque há duas verificações de if (instance == null) no método getInstance()? Podemos considerar essa estratégia desperdício de recursos? Justifique sua resposta.
- Não, essa modificação garante o thread-safe, garantindo que não crie mais de um objeto desnecessariamente.