# Exercício 1: Aplicações

Para cada cenário abaixo, indique se o **padrão Iterator** é apropriado ou **não** e **justifique em 2–3 frases**.

#### 1. Um app de música precisa percorrer uma **playlist** por várias ordens (sequencial, embaralhada, só favoritas), sem que o restante do código conheça a estrutura interna da coleção.
- Faz sentido usar Iterator, esse é um ótimo exemplo.

#### 2. Um explorador de arquivos precisa **percorrer uma árvore de pastas e arquivos** sem que a interface exponha como os nós estão ligados.
- Faz sentido usar Iterator.

#### 3. Um sistema precisa ler um **arquivo gigante por partes** (streaming), entregando um registro por vez em vez de carregar tudo na memória.
- Faz sentido usar Iterator

#### 4. Um array fixo de tamanho conhecido, percorrido sempre por um **único laço `for` simples** dentro da própria classe dona do array.
- Não faz sentido usar Iterator, não há necessidade em virtude da abrangencia do interator

#### 5. Uma conexão de banco de dados que precisa expor as linhas de um `ResultSet` **uma a uma**, escondendo o cursor e a estrutura da tabela.
- Faz sentido usar Iterator

# Exercício 2: Rastreando o anti-pattern em um projeto real

### Baixe o projeto, importe na IDE e **rode o `Main`**:

#### [Baixar `projeto_iterator_antipattern.zip`](/exemplos/projeto_iterator_antipattern.zip)

É um app de música (Maven, Java 17) que percorre uma **playlist** de várias formas
(tocar tudo, embaralhar, sugerir favoritas). O código **funciona**, mas expõe a
estrutura interna da coleção e espalha a travessia pelos clientes.

```bash
mvn compile
java -cp target/classes br.venson.net.designpatterns.iterator.Main
```

> No Eclipse/IntelliJ, basta importar a pasta do projeto e executar a classe `Main`.

Analise o código e responda:

1. `Playlist.getFaixas()` devolve a **própria lista interna**. Quais são as
   consequências de encapsulamento disso? Que clientes passam a depender da
   estrutura da playlist?
2. Ao rodar o `Main`, observe a ordem da playlist antes e depois de
   `tocarEmbaralhado`. Explique **por que** a ordem original mudou.
3. A travessia por índice se repete em três clientes (`Player`, `Recomendador`,
   `RelatorioPlaylist`). O que acontece se a `Playlist` trocar sua estrutura interna
   (array, lista ligada, páginas)? Quantos pontos quebram?
4. Proponha uma refatoração com o padrão **Iterator**, deixando explícitos:
   - a **interface de iterador** (`temProxima()`/`proxima()`);
   - o **iterador concreto** que guarda a posição;
   - o **agregado** `Playlist`, que **cria** o iterador.
5. Como a solução permite **novas formas de travessia** (embaralhada, só favoritas)
   sem duplicar laços e sem expor a estrutura interna?

**Entrega:** um diagrama de classes (antes/depois), o código refatorado e um
parágrafo justificando cada decisão de design.
