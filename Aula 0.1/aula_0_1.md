**1. Processo** 

Um **processo** é, basicamente, um *programa em execução*. Quando o programa está guardado no seu disco, é apenas um ficheiro inativo. Quando faz duplo clique e ele abre, o sistema operativo transforma-o num processo.
* **A analogia:** Imagine que um processo é uma **fábrica**. A fábrica recebe do governo (o Sistema Operativo) o seu próprio terreno (espaço de memória RAM) e as suas próprias ferramentas. O importante aqui é o **isolamento**: uma fábrica não pode invadir o terreno da outra. 

**2. Threads (Linhas de Execução)**

Uma **thread** é a unidade mais pequena de trabalho dentro de um processo. Todo o processo tem pelo menos uma thread (a principal), mas pode ter várias (multithreading).
* **A analogia:** Se o processo é a fábrica, as threads são os **operários** que lá trabalham dentro. Vários operários (threads) podem trabalhar juntos na mesma fábrica (processo), partilhando o mesmo espaço, materiais e ferramentas (a mesma memória). Como dividem o trabalho, tudo fica mais rápido, mas precisam de ter cuidado para não tentarem usar a mesma ferramenta ao mesmo tempo e estragarem o produto.

**3. Programação Concorrente**

É a capacidade do sistema de lidar com **várias tarefas ao mesmo tempo**. 
* **Como funciona?** Para não o deixar à espera que um programa termine para poder usar outro, o computador alterna rapidamente a atenção do processador entre várias threads/processos (ou executa-os verdadeiramente em simultâneo, se o computador tiver múltiplos núcleos).
* **O problema:** Como a concorrência faz as coisas acontecerem de forma sobreposta, a programação concorrente exige a criação de "leis de trânsito" (sincronização). Por exemplo: se duas threads tentarem alterar o saldo da sua conta bancária exatamente no mesmo milissegundo, pode dar erro. A programação concorrente cria "fechaduras" (locks) para garantir que uma thread termina de mexer nos dados antes que a outra comece.

**4. Gerências (O papel do Sistema Operativo)**

Para que os processos, as threads e a concorrência funcionem sem o computador bloquear, o Sistema Operativo atua como o "gerente geral" da máquina, dividido em departamentos:
* **Gerência de Processos:** É como os Recursos Humanos e o Chefe de Turno. É o sistema operativo que decide qual o processo ou thread que vai usar o processador agora, quanto tempo pode usar, e que coloca em pausa quem já o usou demasiado para dar vez aos outros (Escalonamento).
* **Gerência de Memória:** É o setor imobiliário. Ele distribui os "terrenos" (espaços na memória RAM) aos processos que estão a nascer e recolhe o terreno de volta quando o programa é fechado. Garante que a "fábrica A" nunca acede aos dados da "fábrica B".
* **Gerência de Dispositivos/Entrada e Saída:** Controla quem pode usar o teclado, o rato, o ecrã e o disco rígido, para que os dados fluam sem confusão.

**Resumo da obra:** O Sistema Operacional usa as suas **gerências** para pegar nas suas aplicações e transformá-las em **processos**, que dividem as suas tarefas em **threads**. Tudo isto funciona em harmonia graças à **programação concorrente**, o que lhe permite ouvir música, navegar e descarregar um ficheiro em simultâneo!