### Problemas de Sincronização

* **Condição de corrida e seção crítica:** Quando vários processos acessam e manipulam dados compartilhados concorrentemente, o resultado final pode ser afetado pela ordem de execução, gerando o que chamamos de condição de corrida. Para evitar problemas, a parte do código que faz esse acesso ao recurso compartilhado é identificada como a "seção crítica".
* **Exclusão mútua e bloqueios:** O princípio básico da sincronização é garantir a exclusão mútua: apenas um processo pode executar sua seção crítica por vez. Falhas de planejamento podem causar espera indefinida (starvation), onde um processo nunca consegue acessar o recurso, ou deadlock, uma situação extrema em que processos ficam travados aguardando infinitamente por recursos que estão retidos uns pelos outros.

---

### Mutex e Semáforos

* **Lock Mutex:** É a ferramenta de software mais simples para proteger regiões críticas e garantir a exclusão mútua. Um processo precisa adquirir o lock para entrar na seção e liberá-lo ao sair. Sua principal desvantagem é a "espera em ação" (spinlock), onde o processo gasta ciclos da CPU girando em um loop contínuo até que o lock fique disponível.
* **Semáforos:** São variáveis inteiras controladas através de duas operações atômicas exclusivas: wait() (ou down) e signal() (ou up).
* **Tipos de Semáforos e Otimização:** O semáforo binário varia apenas entre 0 e 1, agindo de forma muito parecida com um Mutex, enquanto o semáforo de contagem não possui restrição de valores, sendo ideal para gerenciar o acesso a um grupo (pool) de vários recursos idênticos. Para evitar o desperdício de CPU (busy waiting), semáforos modernos colocam o processo em uma fila de espera (dormindo) até que seja acordado por outro processo via signal().

---

### O Problema do Produtor-Consumidor

* **O Paradigma do Buffer Limitado:** É o exemplo clássico de processos cooperativos. Um processo "produtor" gera dados e os insere em um buffer (uma região de memória compartilhada), de onde um processo "consumidor" irá retirá-los para leitura.
* **A Necessidade de Sincronização:** É vital impedir que o consumidor tente ler um item de um buffer que ainda está vazio, ou que o produtor tente gravar em um buffer que já está completamente cheio. A solução ideal combina semáforos de contagem (para rastrear espaços vazios e cheios) com um Mutex (para garantir a exclusão mútua no momento exato de gravar ou ler a variável).

---

### Leitores e Escritores

* **Regras de Acesso Concorrente:** Este problema lida com acessos a uma base de dados compartilhada. Processos leitores apenas consultam a base, então vários podem acessá-la simultaneamente sem nenhum perigo. No entanto, os escritores, que atualizam e modificam a base, precisam de acesso totalmente exclusivo, bloqueando tanto novos leitores quanto outros escritores durante a sua operação.
* **Riscos de Starvation (Inanição):** Existem variações de solução que envolvem prioridades e podem gerar problemas. Se a regra diz que nenhum leitor deve esperar enquanto a base estiver apenas sendo lida, os escritores podem sofrer starvation. Por outro lado, se a prioridade for dada ao escritor assim que ele estiver pronto, os leitores é que podem acabar sofrendo de inanição.