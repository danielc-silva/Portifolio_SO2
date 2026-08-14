# Condições de Corrida
* Processos podem compartilhar dados na memória ou em arquivos.
* Um exemplo é o spooler de impressão, onde processos colocam arquivos em uma fila para serem impressos.
* Se dois processos acessarem e modificarem os mesmos dados ao mesmo tempo, pode ocorrer um problema.
* No exemplo, os processos A e B leem in = 7 e acreditam que a posição 7 está livre.
* B grava seu arquivo na posição 7 e atualiza in para 8.
* Depois, A grava seu arquivo também na posição 7, apagando o arquivo de B.
* O sistema não percebe o erro, mas o arquivo de B nunca será impresso.
* Esse tipo de situação é chamado de condição de corrida (race condition).
* Elas são difíceis de identificar porque o erro não acontece sempre e se tornam mais comuns com o aumento do número de núcleos.

# Região Crítica
Para evitar problemas que envolvem memória compartilhada, arquivos compartilhados e tudo mais que seja compartilhado é impedir que mais de um processo leia e escreva os dados compartilhados ao mesmo tempo. 
O que precisamos é de exclusão mútua (mutual exclusion), ou seja, uma maneira de garantir que, se um processo estiver usando uma variável ou arquivo compartilhado, os outros processos sejam impedidos de fazer a mesma coisa. 
Durante parte do tempo, um processo está ocupado realizando cálculos internos e outras tarefas que não causam condições de corrida. Porém, às vezes, um processo precisa acessar memória ou arquivos compartilhados, ou realizar outras operações críticas que podem causar essas condições. 
A parte do programa em que a memória compartilhada é acessada é chamada de região crítica (critical region) ou seção crítica (critical section). 
Se conseguirmos organizar as coisas de forma que dois processos nunca estejam em suas regiões críticas ao mesmo tempo, poderemos evitar as condições de corrida. 

Embora esse requisito evite condições de corrida, ele não é suficiente para que processos paralelos cooperem de maneira correta e eficiente usando dados compartilhados. Precisamos que quatro condições sejam satisfeitas para termos uma boa solução:
1. Dois processos não podem estar simultaneamente dentro de suas regiões críticas.
2. Nenhuma suposição pode ser feita sobre as velocidades ou sobre o número de CPUs.
3. Nenhum processo que esteja executando fora de sua região crítica pode bloquear qualquer outro processo.
4. Nenhum processo deve ter que esperar para sempre para entrar em sua região crítica.

# Variáveis de bloqueio 
Como segunda tentativa, vamos procurar uma solução de software. Considere uma única variável compartilhada (lock), inicialmente com valor 0. Quando um processo deseja entrar em sua região crítica, ele primeiro verifica o bloqueio. Se o lock for 0, o processo o define como 1 e entra na região crítica. Se o lock já for 1, o processo simplesmente espera até que ele se torne 0. Assim, 0 significa que nenhum processo está em sua região crítica, enquanto 1 significa que algum processo está em sua região crítica. 

Infelizmente, essa ideia contém exatamente a mesma falha fatal que vimos no diretório do spooler. Suponha que um processo leia o lock e veja que ele está em 0. Antes que ele possa alterá-lo para 1, outro processo é escalonado, executa e define o lock como 1. Quando o primeiro processo voltar a executar, ele também definirá o lock como 1 e, assim, os dois processos estarão em suas regiões críticas ao mesmo tempo. 

Agora você pode pensar que poderíamos contornar esse problema lendo primeiro o valor do lock e, depois, verificando-o novamente imediatamente antes de armazenar um novo valor nele. Porém, isso realmente não ajuda. A condição de corrida simplesmente acontece se o segundo processo modificar o lock logo depois que o primeiro processo terminar sua segunda verificação.

# Solução de Peterson
* T. Dekker foi o primeiro a desenvolver uma solução de software para exclusão mútua sem exigir revezamento rígido.
* Em 1981, G. L. Peterson criou uma solução mais simples, conhecida como Algoritmo de Peterson.
* O algoritmo é usado para controlar o acesso de dois processos às variáveis compartilhadas.
* Antes de entrar na região crítica, o processo chama enter_region, informando seu número (0 ou 1).
* Se necessário, o processo espera até que seja seguro entrar.
* Ao terminar, chama leave_region, indicando que saiu da região crítica e permitindo que o outro processo entre.
* O objetivo principal é garantir a exclusão mútua, evitando que os dois processos acessem a região crítica simultaneamente.