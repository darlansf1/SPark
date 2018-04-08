# SPark


**Qual o objetivo do comando cache em Spark?**

A maior parte das operações em um RDD são *lazy*, o que significa que, na prática, resultam apenas em uma abstração para um conjunto de instruções a serem executadas. Essas operações só são realmente executadas através de ações, que não são operações *lazy*, pois só podem ser avaliadas a partir da obtenção de valores do RDD. Isso pode, por exemplo, tornar códigos iterativos mais ineficientes, pois ações executadas repetidamente sobre um mesmo conjunto de dados disparam a ação de todas as operações *lazy* necessárias em cada uma das iterações, mesmo que resultados intermediários sejam iguais, como no caso da leitura de um arquivo. O uso do comando **cache** ajuda a melhorar a eficiência do código nesse tipo de cenário, pois permite que resultados intermediários de operações *lazy* possam ser armazenados e reutilizados repetidamente.


**O mesmo código implementado em Spark é normalmente mais rápido que a implementação equivalente em MapReduce. Por quê?**

Existem alguns fatores no desenho dessas ferramentas que tornam as aplicações desenvolvidas em MapReduce geralmente mais lentas que aquelas que utilizam  Spark. Um desses fatores é o uso de memória. É comum a necessidade de rodar vários *jobs* MapReduce em sequência em vez de um único *job*. Ao usar MapReduce, o resultado de cada *job* é escrito em disco, e precisa ser lido novamente do disco quando passado ao *job* seguinte. Spark, por outro lado, permite que resultados intermediários sejam passados diretamente entre as operações a serem executadas através do *caching* desses dados em memória, ou até mesmo que diversas operações possam ser executadas sobre um mesmo conjunto de dados em *cache*, reduzindo a necessidade de escrita/leitura em disco. Adicionalmente, mesmo em cenários onde ocorre a execução de apenas um *job*, o uso de Spark tende a ter desempenho superior ao MapReduce. Isso ocorre porque *jobs* Spark podem ser iniciados mais rapidamente, pois para cada *job* MapReduce uma nova instância da JVM é iniciada, enquanto Spark mantém a JVM em constantemente em execução em cada nó, precisando apenas iniciar uma nova *thread*, que é um processo extremamente mais rápido.


