---
title: "Tmux"
date: "2020-02-05"
palavras-chave: ["tmux", "terminal", "multiplexador"]
ano: ["2020"]
featured: false
---

O tmux oferece uma proposta de algo no estilo arquitetural de componentes independentes para o terminal, somado similar ao gerenciamento de janelas para _GUI_, isso através da divisão da tela em multiplas insâncias independentes do shell.

## Linhas gerais de funcionamento
A proposta arquitetural conta com um processo do tmux que vai suportar todas as sessões, um processo servidor, sendo estas os concentradores de janelas, estas por sua vez podem disponibilizar um ou mais pseudoterminais através do particionamento do espaço disponível na janela. O processo servidor é iniciado tão logo seja solicitada uma sessão via comando `tmux`.

As janelas vão operar como se fossem as abas do Xfce4-terminal, do Konsole, do Gnome Terminal. As divisões da tela são como as obtidas num Terminator ou num Tilix. Como pode-se constatar na imagem apresentada acima, há dois pseudoterminais acessíveis em uma janela do Tmux, que pode estar sendo acessado à partir que qualquer emulador de terminal.

Havendo um processo servidor, há também um processo cliente para o Tmux, executado por um emulador de terminal de preferência do usuário, que disponibilizará através de uma interface de texto o acesso aos pseudoterminais servidos em uma sessão.

Desta forma os processos do shell não são processos filhos do emulador de terminal, mas do multiplexador de terminais, do Tmux. Isso abre a possibilidade de, por exemplo, matar o processo do emulador de terminal sem perder _ipso facto_ o processo do shell.

## Potencial para um multiplexador de terminais

### Multitarefa independente de emulador de terminal
Sendo o Tmux um multiplexador, é também possível realizar separação de tarefas, favorecendo um trabalho mais efieciente para quem usa linha de comando e programas com interface de texto, e sem depender do emulador de terminal suportar abas ou particionamento, aumentando o leque de escolhas possíveis para emulador de terminal, podendo o usuário escolher emuladores mais minimalistas, sem perder o recurso de ter vários pseudoterminais acessíveis através de uma mesma janela, mesmo no TTY.

### Indenpendência do emulador de terminal
Sendo cliente e servidor componentes independentes, é possível se desligar de uma sessão e retornar à ela sem perder o estado do terminal, além da saída de comandos já executados, o que normalmente ocorre depois de se fechar o emulador de terminal.

Criar diferentes sessões também é possível (há inclusive programas específicos para lidar com usos mais elaborados desse recurso, como o [tmuxp](https://tmuxp.git-pull.com/en/latest/)) , no caso de separar uma sessão para uso geral do sistema, uma outra para programação (com janelas para o Vim, para o git, para manipulação dos arquivos, para leitura de manual _et cetera_). Havendo o recurso de múltiplas sessões, trocar de sessão dentro de um mesmo emulador de terminal também é possível, pois, novamente, todos os processos estarão ligados à um componente independente do emulador de terminal.

O Tmux também oferece recursos interessantes para trabalhar em máquinas remotas. Exemplifico:
> Em um processo de atualização do sistemas de uma máquina em rede, ou qualquer outra operação com exigência de tempo, há o perigo de a conexão estabelecida entre as máquinas cair. Usando o Tmux na máquina remota, os processos envolvidos na tarefa não estarão ligados ao processo da conexão remota (como o ssh), mas ao processo independente do Tmux, portanto é possível se desconectar da máquina remota, reconectar em outro momento, e encontrar a operação em andamento ou concluída.

### Particularidades
O Tmux é um excelente recurso com suas funcionalidades “básicas”, e oferece outras tantas possibilidades pouco óbvias, mas que podem ser interessantes em cenários específicos, como a possibilidade de enviar o buffer do terminal para o stdout, o que pode ser manipulado num _shell script_ para que se envie o resultado de um comando (útil para tutoriais) ou uma url para a área de transferência (via `xclip` por exemplo).

#### Atalhos de teclado
Para o uso, os atalhos padrão podem se tornar um problema caso não haja alguma leitura, pois são um pouco diferentes do que se está acostumado. O modo de rolar no _buffer_ do terminal também é diferente, pois ao invés de rolar livremente para cima e para baixo, primeiro deve-se entrar no modo de cópia para então rolar. Com o ponteiro habilitado, a rolagem fará entrar automaticamente no modo cópia e rolará para baixo e para cima. Em programas como o `less`, a rolagem se dá normalmente.

No manual há uma lista com os atalhos padrão perto do início do manual (v. [man tmux:atalhos padrão](https://www.mankier.com/1/tmux#Default_Key_Bindings)).

## Propostas de casamento

### ST + Tmux
ST é o _Simple Terminal_ do projeto [Suckless](https://st.suckless.org/). Uma implementação _simples_, segundo o que a [filosofia do projeto Suckless](https://suckless.org/philosophy/) chama de simplicidade, naturalmente. Essa simplicidade implica em algumas questões como configurações aplicadas ao programa em tempo de compilação, além de algumas funcionalidades comuns em outros emuladores de terminal que neste caso serão solenemente ignoradas na implementação base.

Por certo aspecto, por que ter um emulador de terminal que se limita, tanto quanto possível, ao estritamente essencial?

Junto ao Tmux, percebe-se que o emulador não precisa, necessariamente, de implementar certos recursos que, em um fechamento de escopo mais rigoroso, não tem relação com a tarefa de emular um terminal. ST fornece a plataforma que possibilita a entrada de digitação do utilizador e o desenho da matriz de caracteres, enquanto o Tmux lida com outras questões como persistência de sessão, multiplas partições e "abas" (chamadas janelas), histórico de saída de texto... uma relação em que o terminal pode, não ser limitado, mas se limitar em número de tarefas e complexidade interna de funcionamento.

### URxvt + Tmux
URxvt é um emulador de terminal que pode parecer um tanto complicado e estranho, devido sua configuração ser realizada dentro o `~/.Xresources`, mas oferece uma boa proposta de emulador de terminal possuindo um recurso de _servidor de emulador de terminal_, o `urxvtd`, que pode ser iniciado junto com um DE ou WM, por exemplo, para que depois sejam requisitados janelas, que seriam os clientes, do emulador de terminal com o `urxvtc`. Vantagens seriam inicialização do emulador de terminal mais rápida, e diminuição do consumo de memória (quando configurado de maneira adequada).

Junto com o Tmux, o URxvt pode ser configurado de modo que use menos recursos da máquina (como memória), uma vez que esses recursos seriam tratados pelo Tmux, desabilitando o armazenamento de linhas fora da área da janela do emulador de terminal já que o Tmux terá um mecanismo interno, independente portanto, para o chamado _buffer_.

> Also, many people (me included) like large windows and even larger scrollback buffers: Without --enable-unicode3, rxvt-unicode will use 6 bytes per screen cell. For a 160x?? window this amounts to almost a kilobyte per line. A scrollback buffer of 10000 lines will then (if full) use 10 Megabytes of memory. With --enable-unicode3 it gets worse, as rxvt-unicode then uses 8 bytes per screen cell.
>
> -- [man 7 urxvt](https://www.mankier.com/7/urxvt#Rxvt-Unicode/Urxvt_Frequently_Asked_Questions-Meta,_Features_&_Commandline_Issues)

### Termite + Tmux
Quando se trata de aplicações em emulação de terminal com base no VTE, o Termite é um representante mais minimalista comparado com o terminal do Gnome ou o Tilix (que passa por [turbulências](https://github.com/gnunn1/tilix/issues/1700)). Com configuração simples de redigir, sem muitos floreios e rodeios, Termite entrega os recursos de emulação de terminal do VTE.

Tmux neste caso entraria, sem muitos apelos de arquitetura, como no caso do URxvt, para fornecer mais funcionalidades e possibilidades de trabalho na linha de comando, de forma mais notória através das janelas e partições da área de texto, uma vez que o Termite não entrega o popular, entre tantos terminais, das abas.

### Alacritty + Tmux
Alacritty é um emulador de terminal que não funciona no computador de quem quer que queira usá-lo, uma vez que a base de sua principal propaganda, ser o terminal mais rápido existente, é o uso de GPU para suas operações, e há um requisito de versão mínima do _OpenGl_ para que ele funcione, o que [exclui os maquinários de mais idade](https://github.com/alacritty/alacritty/issues/128).

Quem tem uma máquina cuja unidade de processamento gráfico suporta os recursos utilizados pelo Alacritty, e resolver dar ao projeto uma chance, perceberá logo que as diretrizes do projeto pendem para menos floreios e rodeios, lembrando sempre que a propaganda é de um emulador de terminal projetado para desempenho. Como no caso do Termite, o Tmux pode ser um excelente complemento para o trabalho na linha de comando.

Lembrando um aspecto que vale em todos os outros emuladores: quanto menor o armazenamento de linhas pregressas, menor é o uso de memória, conforme citado nas considerações com o URxvt. Sendo o Alacritty um emulador projetado para alto desempenho, mas que não conta com um recurso de servidor de emulador de terminal, pode-se tentar repassar essa potencial causa de aumento de uso de área de memória não compartilhada.

## Outros projetos

  * [GNU Screen](https://www.gnu.org/software/screen/)
  * [abduco](http://www.brain-dump.org/projects/abduco/)
  * [dvtm](http://brain-dump.org/projects/dvtm/)

Mais opções em [Arch Wiki](https://wiki.archlinux.org/index.php/List_of_applications/Utilities#Terminal_multiplexers).

## Leitura complementar
  * [man tmux](https://www.mankier.com/1/tmux)
  * [Arch Wiki:Tmux](https://wiki.archlinux.org/index.php/Tmux)
  * [Livro:The Tao of Tmux](https://leanpub.com/the-tao-of-tmux/read)
  * [Blog:Ham Vocke:Tmux Guide](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)
  * [Blog:Ham Vocke:Tmux Conf](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/)

## Leitura sobre fundamentos
  * [man pty](https://www.mankier.com/7/pty)
  * [man pts](https://www.mankier.com/4/pts)
