# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Mediator** é apropriado ou **não** e **justifique em 2–3 frases**.

#### 1. Uma **sala de chat** em que dezenas de usuários trocam mensagens e as regras de conversa (quem fala com quem, sons, privacidade) mudam com frequência, sem que cada usuário conheça os demais.
- Faz sentido usar Mediator, é o melhor exemplo, separa perfeitamente os usuários.

#### 2. Uma **tela de diálogo** com muitos widgets que reagem uns aos outros: ao marcar um `Checkbox`, um `Botão` é habilitado e um `Campo de texto` é limpo, formando uma teia de dependências.
- Faz sentido usar Mediator, simplificada, e é justamente recomendado quando se trata de um amarrado em malha como nesse exemplo.

#### 3. Uma **central de despacho** de entregas que precisa coordenar motoristas, pedidos e entregas, decidindo qual pedido vai para qual motorista conforme as regras mudam.
- Faz sentido usar Mediator, desde que implementado da maneira correta, sepando os motoristas corretamente.

#### 4. Uma **biblioteca** em que a classe `Impressora` se comunica com um único `Spooler` fixo, sempre pelo mesmo método simples, e essa interação nunca cresce nem ganha novos objetos.
- Não faz sentido usar Mediator, overengeenir pois usa um único objeto.

#### 5. Uma classe `Ponto`, com apenas `x` e `y`, que só guarda coordenadas e nunca participa de interações com outros objetos.
- Não faz sentido usar Mediator, overengeenir pois usa um único objeto, e sem sentido de utilizar.

# Exercício 2: Rastreando o anti-pattern em um projeto real

Baixe o projeto, importe na IDE e **rode o `Main`**:

[Baixar `projeto_mediator_antipattern.zip`](/exemplos/projeto_mediator_antipattern.zip)

É uma sala de chat (Maven, Java 17) em que cada **usuário** guarda referências
diretas aos demais e envia mensagens diretamente. O código **funciona**, mas as
ligações formam uma malha e são reescritas na mão a cada entrada ou saída.

```bash
mvn compile
java -cp target/classes br.venson.net.designpatterns.mediator.Main
```

> No Eclipse/IntelliJ, basta importar a pasta do projeto e executar a classe `Main`.

Analise o código e responda:

1. `Usuario` guarda uma lista de `contatos` (outros `Usuario`). Para **N** usuários,
   quantas ligações podem existir? O que isso indica sobre o acoplamento?
2. O que é preciso alterar para incluir um novo usuário (por exemplo, o `Diego`)?
   Liste os pontos tocados. E para **remover** alguém, que problemas podem sobrar?
3. As regras de conversa (todos recebem? só o admin fala? mensagem privada?) não
   estão em nenhum lugar claro. Onde elas deveriam morar? Por quê?
4. Proponha uma refatoração com o padrão **Mediator**, deixando explícitos:
   - a **interface de mediador** (`MediadorChat`);
   - o **mediador concreto** que conhece os usuários (`SalaChat`);
   - os **colegas** (`Usuario`), que passam a conhecer apenas o mediador.
5. Depois de refatorar, como incluir/remover usuário **muda**? E como a mesma
   classe `Usuario` passa a servir em **outra** sala ou em um chat privado?

**Entrega:** um diagrama de classes (antes/depois), o código refatorado e um
parágrafo justificando cada decisão de design.
