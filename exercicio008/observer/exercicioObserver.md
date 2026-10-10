# Exercício 1: Aplicações

### Para cada cenário abaixo, indique se o **padrão Observer** é apropriado ou **não** e **justifique em 2–3 frases**.

#### 1. Uma **estação meteorológica** publica novas medições e vários **displays** (temperatura, estatísticas, previsão) precisam se atualizar automaticamente, sem que a estação conheça cada display.
- Faz sentido usar Observer, pois o observer permite que a estação não conheça o display.

#### 2. Uma **interface gráfica** em que um botão pode ter vários *listeners* e o **modelo** precisa avisar as *views* quando muda, permitindo novos interessados sem alterar o modelo.
- Não faz sentido usar Observer, não é indicado utilizar em MCV. 

#### 3. Uma **newsletter** em que um número variável de assinantes recebe o conteúdo novo e pode **assinar/cancelar** a qualquer momento.
- Faz sentido usar Observer, a utilização do observer permite uma fácil decisão de quando assinar ou cancelar de imediato.

#### 4. Uma classe `Configuracao` que guarda valores lidos **uma única vez** por um **único** consumidor fixo, sempre de forma direta e imediata, sem notificações.
- Não faz sentido usar Observer, visto que não há a menor necessidade implementar para valores fixos, dependentes e que conversam de forma imediata.

#### 5. Uma classe `Ponto`, com apenas `x` e `y`, que só guarda coordenadas e nunca avisa ninguém sobre mudanças.
- Não faz sentido usar Observer, ninguém avisa ninguém, sendo assim nem seria possível implementar

# Exercício 2: Rastreando o anti-pattern em um projeto real

Baixe o projeto, importe na IDE e **rode o `Main`**:

[Baixar `projeto_observer_antipattern.zip`](/exemplos/projeto_observer_antipattern.zip)

É uma estação meteorológica (Maven, Java 17) que publica medições para **displays**.
O código **funciona**, mas a própria estação conhece cada display e o atualiza
diretamente, sem registro dinâmico.

```bash
mvn compile
java -cp target/classes br.venson.net.designpatterns.observer.Main
```

> No Eclipse/IntelliJ, basta importar a pasta do projeto e executar a classe `Main`.

Analise o código e responda:

1. A `EstacaoMeteorologica` guarda referências a `DisplayTemperatura` e
   `DisplayEstatisticas`. Por que isso é um **acoplamento forte** entre sujeito e
   observadores?
2. O que é preciso alterar na estação para adicionar um **`DisplayUmidade`**?
   Liste os pontos tocados.
3. Por que **não dá** para registrar/remover um display em tempo de execução?
   O que faltaria para permitir isso?
4. Proponha uma refatoração com o padrão **Observer**, deixando explícitos:
   - a **interface de observador** (`Observador`, com `atualizar(...)`);
   - o **sujeito** (`Sujeito`), que registra, remove e **notifica**;
   - os **observadores concretos** (`DisplayTemperatura`, `DisplayEstatisticas`).
5. Depois de refatorar, como adicionar um display novo fica mais simples? E como a
   estação fica **reutilizável** em outro painel, sem recompilar?

**Entrega:** um diagrama de classes (antes/depois), o código refatorado e um
parágrafo justificando cada decisão de design.
