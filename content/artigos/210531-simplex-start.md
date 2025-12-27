---
title: "O nascer do sol multicor, o bailar, ou: o início da sessão gráfica"
date: "2021-05-31"
palavras-chave: ["xorg", "startx", "shell"]
ano: ["2021"]
featured: true
---

Este artigo foi construído para demonstrar uma construção realizada no Arch Linux, funcional em 29 de maio de 2021 (caso seja necessário a intervenção com [ALA](https://wiki.archlinux.org/title/Arch_Linux_Archive) no futuro para a reprodução do experimento).

A construção visava a releitura do que era realizado pelo par `startx` e `xinit`.

## 1. Introdução
Apresenta-se nessa sessão uma breve contextualização e a justificativa para a presente proposta.

### 1.1. Contextualização
Desde tempos imemoriais (?), ao apertar de um botão nasce o Sol por trás das telas, explodindo em 16 milhões de cores, a deslumbrar, ou não, os olhos cativos daquele que rejeita desde as vísceras qualquer possibilidade de uma vida sem significado. Ou constituída de elementos sem significado. As nuvens se delineiam claramente no céu, em diversos tamanhos, movimentando-se entre cá e lá, lá e cá...

### 1.2. Justificativa
Para que nos pequenos mundos se possa ver a linha do Sol nascente na abóbada celeste, descreve-se uma trajetória. Descrita, projetada na imagem do céu, a imaginação do homem do campo se torna o raciocínio do relojoeiro da cidade, metódico, preciosista. As pequenas engrenagens devem caber no menor espaço, mas no sistema mais resistente possível ao tempo que há de descrever. Os ponteiros se alinham com o nascer do Sol.

## 2. Resultados
Apresenta-se em seguida a implementação.

### 2.1. $HOME bin/startx

```bash
#!/usr/bin/env bash

tty="$(tty)"
tty="${tty##*tty}"
display=":${tty}"

>> "$XAUTHORITY"

[[ -n $(xauth list "$display") ]] && xauth -q remove "$display"

xauth -q add "$display" MIT-MAGIC-COOKIE-1 \
    "$(od -An -N16 -tx /dev/random | tr -d ' ')"

trap "DISPLAY=$display $XINITRC" USR1
(
    trap '' USR1

    source "$XSERVERRC"
    exec $xorg_cmd "vt${tty}" "$display"
) & wait "$!"
```

### 2.2. $HOME etc/X11/xserverrc

```bash
xorg_cmd="Xorg -noreset -nolisten local -nolisten tcp -auth "$XAUTHORITY""
```

### 2.3. $HOME etc/X11/xinitrc

```bash
# Executed by startx (run your window manager here)

userresources="$XDG_CONFIG_HOME/X11/Xresources"
usermodmap="$XDG_CONFIG_HOME/X11/Xmodmap"
userxkbmap="$XDG_CONFIG_HOME/X11/Xkbmap"
userprofile="$XDG_CONFIG_HOME/X11/xprofile"
sysresources=/etc/X11/xinit/.Xresources
sysmodmap=/etc/X11/xinit/.Xmodmap

# Load some resources and keymaps
[[ -f "$sysresources"  ]] && xrdb -merge "$sysresources"
[[ -f "$sysmodmap"     ]] && xmodmap "$sysmodmap"
[[ -f "$userresources" ]] && xrdb -merge "$userresources"
[[ -f "$userxkbmap"    ]] && xkbcomp "$userxkbmap" $DISPLAY
[[ -f "$usermodmap"    ]] && xmodmap "$usermodmap"

# Run some scripts
if [[ -d /etc/X11/xinit/xinitrc.d ]] ; then
    for f in /etc/X11/xinit/xinitrc.d/?*.sh ; do
        [[ -x "$f" ]] && . "$f"
    done
    unset f
fi

# Start some prorams for the graphical session
[[ -f "$userprofile" ]] && . "$userprofile"

# Start the graphical session
exec "$XDG_CURRENT_DESKTOP"
```

### 2.4. $HOME profile

```bash
# Entre outros elementos do perfil

export XDG_CONFIG_HOME="$HOME/.local/etc"

export XINITRC="$XDG_CONFIG_HOME/X11/xinitrc"
export XSERVERRC="$XDG_CONFIG_HOME/X11/xserverrc"
export XAUTHORITY="$XDG_RUNTIME_DIR/Xauthority"

export XDG_CURRENT_DESKTOP='spectrwm'
```

## 3. Discussão
A abordagem seguirá a ordem dos fatores que importa para o produto.

### 3.1. O despertar
Autenticado o usuário, de `login` parte um processo filho, o _shell de login_.

Sendo _Bash_ o _shell de login_, para as finalidades deste artigo, será [exportada](https://www.gnu.org/software/bash/manual/bash.html#Environment) uma variável demonstrativa `XDG_CONFIG_HOME` que apontará para o diretório de configurações do usuário, que poderia ser o usual `~/.config`, mas se demonstrará com uma transposição do `etc` da hierarquia padrão do sistema de arquivos para o diretório do usuário.

Outras duas variáveis darão o caminho do diretório onde se concentram as configurações do usuário para o Xorg, uma _paralela_ ao início do servidor gráfico (`XINITRC`), outra para determinar a linha de comando de início do servidor gráfico (`XSERVERRC`).

Mais uma para determinar a localização do arquivo que conterá os chamados _dados relativos à autorização_, que terá atenção mais adiante.

A última seria uma “interpretação”, mas para os fins práticos, é simplesmente uma variável exportada que será empregada no processo, `XDG_CURRENT_DESKTOP` define no uso quotidiano um gerenciador de janelas para iniciar com o servidor gráfico, mas para fins de qualquer experimentação que um queira realizar, qualquer _cliente_ para o servidor gráfico.

Esse _cliente_ pode ser um único emulador de terminal, um navegador de _internet_, um tocador de vídeos ou qualquer outro programa de computador que seja capaz de fornecer uma interface gráfica que estabeleça conexão com o servidor gráfico, de preferência tendo algum ciclo de vida, por razões de funcionamento do servidor gráfico que serão apresentadas mais adiante.

### 3.2. A causa “primeira”
Ao usuário será entregue a linha de comando onde pode realizar o imperativo: `startx`.

### 3.3. E o X se fez?
Aqui já perfilam para retrato algumas exigências do servidor gráfico.

#### 3.3.1. O princípio de individuação
O servidor gráfico exige a identificação do que se pode chamar, internamente, de _tela_. Não a tela do computador, o monitor, o dispositivo de saída, pois se assim fosse o atendimento de um sistema com N monitores _exigiria_ um servidor gráfico distinto para cada, o que não é o caso hoje. Será um identificador para a dada instância do servidor gráfico em execução num dado computador (ou hospedeiro na rede, se apelando para o aspecto _network-transparent window system_).

É definido o identificador da tela, que é um número natural, igual ao número do console virtual, posteriormente a tela do servidor gráfico será atrelada ao _buffer_ utilizado pelo dado console virtual.

Ao `TTY1` corresponde a tela `:1`.

#### 3.3.2. Porque as lentes polarizadas bloqueiam alguma claridade
Como pequeno floreio, reproduzindo por outro caminho uma das partes do `startx` original, um processo para lidar com os chamados dados de autorização.

Um dado programa com interface gráfica precisará dos _dados de autorização_, quando o servidor gráfico for iniciado com `-auth [arquivo de dados de autorização]`, para estabelecer comunicação com o servidor gráfico e então transmitir a interface gráfica e receber os dados de entrada (datilografia, movimento do cursor etc).

> Não será matéria aqui o quanto isso é considerado um protocolo “seguro”. Uma vez que em tempo de execução será necessário o acesso, para cada programa, ao estado do servidor gráfico, se torna questionável a invenção de grandes motes de “segurança” onde se trata simplesmente do funcionamento do recurso. Seja para capturas de tela ou o uso de um mecanismo sofisticado de atalhos de teclado como o [Sxhkd](https://odysee.com/@osimplex:f/atalhos-de-teclado-sxhkd:5).

Usando-se da variável de ambiente `XAUTHORITY` para indicar um local para depositar esses dados seguiremos em frente. Seria possível deixar o arquivos em `$HOME/.Xauthority`, mas vamos explorar mais essa possibilidade. O local será, em uma instalação onde o _systemd-logind_ atua, `$XAUTHORITY` -> `/run/user/$(id -u)/Xauthority`.

> Quem for utilizar um gerenciador gráfico de autenticação como o _LightDM_, conferir se a definição, um uma _certa_ definição, dessa variável de ambiente interfere no inicio do ambiente gráfico após a autenticação.

Usando do [redirecionamento](https://www.gnu.org/software/bash/manual/bash.html#Appending-Redirected-Output) `>>` garantimos a existência do arquivo sem perda de eventual conteúdo preexistente, o que é importante no caso da execução simultânea de dois servidores gráficos, caso de duas sessões gráficas no mesmo computador. Havendo perda dos dados de autorização de sessões anteriores ainda em execução, isso afeta o acesso dos programas com interface gráfica ao respectivo servidor gráfico.

Em seguida verifica-se a existência de entradas anteriores para um identificador de tela (da nomenclatura interna do _Xorg_). Essa verificação conta com a coerência entre identificador da tela e do console virtual, de modo que o _script_ é executado sempre em ambiente de texto do console virtual, e não há qualquer servidor gráfico localmente ocupando o identificador. Havendo registros, eles serão de sessões anteriores já terminadas, então serão removidos.

Um exemplo de saída do comando `xauth -f $XAUTHORITY list $DISPLAY` (se executado à partir de uma sessão de ambiente gráfico):

```
MyHostname/unix:1  MIT-MAGIC-COOKIE-1  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Garantida a limpeza, pois a condição de funcionamento nesta proposta é somente uma entrada por tela do servidor gráfico, realiza-se a inserção dos dados de autorização para a sessão que será criada em seguida.

Para a construção da chave hexadecimal serão lidos (`od`) dados randômicos disponibilizados pelo _kernel_ em `/dev/random`. 16 _bytes_ (`-N16`) sem “delimitadores” na saída (`-An`) em algarismos hexadecimais de 2 _bytes_ cada, passando (`|`) para retirar os espaços (`tr -d ' '`). Forma-se então uma chave hexadecimal de 32 caracteres, representando um valor de 128 _bits_.

#### 3.3.3. Então afinam-se os instrumentos
Aqui os convidados são chamados ao salão, os cavalheiros convidam as damas para a valsa.

Aqui apresentamos o recurso _objetivo_, que fica aos cuidados em `XINITRC`. Tudo o que é acessório precisa de terminar ou correr em paralelo, o essencial deve vir por último e sustentar, pela existência continuada, o que foi estabelecido para execução em paralelo.

`XINITRC` é uma variável de ambiente que aponta para um _shell script_, este necessita de permissão de execução (modificar da comum tríade de permissões 644 para 744 já é suficiente).

Neste _shell script_ teremos algumas operações básicas, algumas que contam com a variável `DISPLAY` no [ambiente](https://www.gnu.org/software/bash/manual/bash.html#Command-Execution-Environment), condicionadas à existência de arquivos que indiquem a intenção do utilizador que as operações sejam realizadas:

  * Carga de _recursos_ (`xrdb`) de parametrização de aspectos do _Xorg_ e de aplicações que usem estes _recursos_ como fonte de configurações como _URxvt_ ou _Rofi_. Modificações do mapa de teclado (`xmodmap`). Compilação de um mapa de teclas XKB (`xkbcomp`);
  * Executar o que houver em `/etc/X11/xinit/xinitrc.d`, local utilizado, a depender da distribuição para os diferentes pacotes de aplicações possam deixar preparações para aspectos de funcionamento durante a sessão gráfica;
  * Caso o usuário tenha algo mais que queira executar para preparar e/ou complementar a sessão gráfica, junto com o gerenciador de janelas da escolha, como o dæmon do _Emacs_ ou _URxvt_, um [compositor](https://wiki.archlinux.org/title/Xorg#Composite), um [agente de notificação](https://wiki.archlinux.org/title/Polkit#Authentication_agents) ou simplesmente acionar a equivalência do toque no _touchpad_ ao clique (_tap to click_), caso tenha algo mais definido em `xprofile`, executar o conteúdo do arquivo.

E, _la pièce de résistance_, o cliente para o servidor gráfico. Ele tomará o lugar do processo do _script_ `xinitrc`, e estabelecerá uma comunicação duradoura como servidor gráfico, de modo que essa relação sustentará a sessão gráfica.

Mas para que toda essa movimentação aconteça primeiro precisa-se que o servidor gráfico esteja pronto, e demonstre que irá “conceder a dança”. Ainda assim ela já está ensaiada, tudo [preparado](https://www.gnu.org/software/bash/manual/bash.html#Bourne-Shell-Builtins) para quando vier o sutil [acenar](https://man.archlinux.org/man/signal.7.en#Standard_signals).

```bash
trap "DISPLAY=$display $XINITRC" USR1
```

#### 3.3.4. Ela está pronta para a valsa?
`XSERVERRC` aponta para o arquivo que contém parte da linha de comando para iniciar o servidor gráfico.

> Atenção para `-nolisten`. Fechar o transporte via canal `local` pode impedir o funcionamento em outros ambientes (ex.: OpenBSD).

> Atenção para `vtXX`. O formato de especificação do dispositivo de console virtual pode ser diferente em outro ambiente, tornando incorreta a linha de comando (ex.: OpenBSD).

Cria-se um [agrupamento](https://www.gnu.org/software/bash/manual/bash.html#Command-Grouping) com _subshell_ de onde se realizará a execução dos comandos, e este agrupamento será realizado de forma paralela, o _shell_ condutor então entra em espera.

O _Xorg_, que será iniciado de forma que substitua o _subshell_ sem criar um novo processo, no momento em que está pronto para aceitar a conexão da aplicação cliente para a sessão gráfica é enviado um sinal `USR1`. Então no _subshell_ é definida a captura deste sinal, antes do acionamento do servidor gráfico, mas com argumento vazio, significando que o sinal será ignorado.

Por outro lado, o _Xorg_ tem toda uma questão especial com o sinal `USR1`. Quando o servidor é iniciado, há a verificação se houve a herança de uma _alça_ atrelada ao sinal indicando que o sinal deve ser ignorado, um [SIG_IGN](https://www.gnu.org/software/libc/manual/html_node/Basic-Signal-Handling.html). Neste caso é enviado um sinal `USR1` ao processo pai assim que o servidor gráfico termina de preparar os esquemas de conexão (lembremos do _network-transparent window system_).

Porque elas são tão difíceis? Provavelmente porque essa sutileza é a arte da vida. “Somente concederei a dança àquele que calça as luvas brancas para bailar, pois quem as usa sabe que a necessidade não se manifesta, não, ela se desenrrola gentilmente sob a aparência de acaso.”

Entende-se então a contraparte, pois o que valerá no tratamento do sinal no recebiemento será o herdado pelo _subshell_ no momento em que foi estabelecido o agrupamento de comandos, ainda que tenha se estabelecido um tratamento branco do sinal dentro do _subshell_.

#### 3.3.5. A troca de olhares
Com tudo acertado, o `startx` permanece imóvel enquanto o _Xorg_ não sinaliza que está pronto para receber conexões. Quando finalmente está pronto, acena com `USR1`, ao que o _shell_ prontamente responde conforme detalhado acima.

Ficarão na árvore de processos três processos essenciais, o `startx`, o _Xorg_ e o cliente, seja ele qual for, definido no final do `xinitrc` através da variável `XDG_CURRENT_DESKTOP`. Se qualquer um desses três processos for encerrado, a sessão gráfica é encerrada. Sem o servidor gráfico nada é possível, sem o cliente a operação do servidor gráfico não faz sentido e sem `startx` como fiel da balança a relação não se desenvolve.

### 3.4. A impermanência por fim vencerá
Quando chega o momento de encerrar a sessão gráfica, basta encerrar o gerenciador de janelas, ou o servidor gráfico. Um sem o outro não têm sentido, terminando um, o outro termina também. O `startx`, com o _Xorg_ terminado, realizou a última instrução do _script_ então encerra.

## Finalização
Então, nos dias que se seguiram, tudo teve mais sentido e significado, os dias radiantes, as noites como nunca antes. Marcado estava o caminho das pedras.

## Leitura complementar

  * [man:login](https://man.archlinux.org/man/core/util-linux/login.1.en)
  * [man:X](https://man.archlinux.org/man/X.7)
  * [man:Xorg](https://man.archlinux.org/man/extra/xorg-server/Xorg.1.en)
  * [man:Xserver](https://man.archlinux.org/man/Xserver.1)
  * [man:startx](https://man.archlinux.org/man/startx.1)
  * [man:xauth](https://man.archlinux.org/man/xauth.1.en)
  * [man:Xsecurity](https://man.archlinux.org/man/Xsecurity.7.en)
  * [manXau](https://man.archlinux.org/man/Xau.3.en)
  * [man:xinit](https://man.archlinux.org/man/xinit.1)
  * [man:xrdb](https://man.archlinux.org/man/xrdb.1)
  * [man:xmodmap](https://man.archlinux.org/man/xmodmap.1)
  * [man:xkbcomp](https://man.archlinux.org/man/xkbcomp.1)
  * [man:xkeyboard-config](https://man.archlinux.org/man/xkeyboard-config.7)
  * [man:tty](https://man.archlinux.org/man/tty.1)
  * [man:od](https://man.archlinux.org/man/od.1)
  * [man:tr](https://man.archlinux.org/man/tr.1)
  * [man:hier](https://man.archlinux.org/man/hier.7.en)
  * [man:file-hierarchy](https://man.archlinux.org/man/file-hierarchy.7)
  * [man:signal](https://man.archlinux.org/man/signal.7.en)
  * [GNU:Bash:Manual](https://www.gnu.org/software/bash/manual/)
  * [GNU:coreutils:Manual](https://www.gnu.org/software/coreutils/manual/)
  * [Solaris AUG:MIT-MAGIC-COOKIE-1](https://docs.oracle.com/cd/E19683-01/806-7612/network-15/index.html)
  * [Freedesktop:XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)
  * [Freedesktop:pam_systemd](https://www.freedesktop.org/software/systemd/man/pam_systemd.html#%24XDG_RUNTIME_DIR)
  * [Arch Wiki:Xorg:Rootless Xorg](https://wiki.archlinux.org/title/Xorg#Rootless_Xorg)
  * [GitHub:dylanaraps:bin/x](https://github.com/dylanaraps/bin/blob/dfd9a9ff4555efb1cc966f8473339f37d13698ba/x)
  * [GitHub:Earnestly:sx/sx](https://github.com/Earnestly/sx/blob/master/sx)

## Instrução sobre Bash e não só

  * [debxp:Curso Básico de Programação em Bash](https://debxp.org/cbpb/)
  * [odysee:Blau Araujo](https://odysee.com/@blauaraujo:5)

## Instrução sobre o sistema operacional GNU etc

  * [odysee:Paulo Kretcheu](https://odysee.com/@kretcheu2001:8)
