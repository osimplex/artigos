---
title: "Execução periódica de tarefas com o SystemD"
date: "2020-03-17"
palavras-chave: ["systemd", "controle", "tarefas periodicas", "newsboat"]
ano: ["2020"]
featured: false
---

Uma publicação muito ligeira para quem deseja usar de um dos (sim, há outros) recursos disponíveis para um sistema operacional com _Linux_, criado para possibilitar a realização autônoma de tarefas num dado momento cronológico (do relógio).

A versão do _SystemD_ utilizada para elaboração deste artigo é `245-3`, distribuição Arch Linux.

## Introdução
O recurso a ser demonstrado, sem aprofundamentos ou maiores explicações, é uma parte do SystemD que pode ser estudada com maiores detalhes seguindo os recursos indicados na sessão de leituras complementares.

Para as finalidades desta publicação será utilizado o recurso de execução periódicas de tarefas em nível de usuário. O _SystemD_ disponibiliza supervisores de processos para o nível de sistema, e para cada usuário _real_ (v. `man systemctl`, argumentos `--user` e `--system`).

A execução periódica de tarefas é parametrizada através de dois arquivos no caminho `~/.config/systemd/user`, `nome-da-tarefa.service` e `nome-da-tafera.timer`. O primeiro parametriza a tarefa, o segundo os aspectos cronológicos.

## Exemplo prático

### Contexto
O popular, entre quem o conhece, agregador de RSS `newsboat` é uma ferramenta para console que permite acompanhar diversas fontes de publicações (indo desde repositórios em servidores de controle de versão, passando por tudo quanto é _blog_ até canais no _Odysee_ e _subreddits_).

Como toda ferramenta que objetiva te munir do estado evolutivo de várias coisas, é necessária uma ação para que tu tenhas as últimas novidades.

O `newsboat` pode ser configurado para se atualizar assim que aberto, e também para se atualizar periodicamente (10 em 10 minutos, por exemplo), mas essa não será a abordagem desta publicação, inclusive por essas configurações exigirem que o programa esteja aberto, do contrário não se atualiza, o que pode não ser interessante.

O recurso utilizado será a linha de comando do `newsboat`, que fornece uma operação de atualização que não implica a abertura da interface de texto (_TUI_).

### Prática
Para que exista um retorno para o utilizador da existência de atualizações, será necessário averiguar que a configuração `notify-program` tenha como valor, ou o nome, ou o caminho de um executável que lançe notificações. Ex.: `notify-program "notify-send"`.

Para criar a tarefa, basta adicionar os seguintes arquivos em `~/.config/systemd/user`:

```ini
[Unit]
Description=Newsboat feed update

[Timer]
OnCalendar=*:0/15:00
Persistent=true

[Install]
WantedBy=timers.target
```

```ini
[Unit]
Description=Newsboat feed update

[Service]
Type=oneshot
ExecStart=/usr/bin/newsboat -x reload
```

Uma unidade de parametrização cronológica alternativa poderia ser:

```ini
[Unit]
Description=Newsboat feed update

[Timer]
OnStartupSec=1min
OnUnitActiveSec=15min

[Install]
WantedBy=timers.target
```

Adicionados, recarregar as configurações do _SystemD_ com `systemctl --user daemon-reload`, então _habilitar_ e _iniciar_ a operação do recurso criado com  `systemctl enable newsboat.timer --now`.

### Usando um shell script
Uma outra forma de parametrizar a execução da tarefa, é evitando o uso direto de um executável do sistema na unidade do SystemD, preferindo um _shell script_. Dessa maneira pode-se trabalhar com mais liberdade determinados aspectos da execução da tarefa, ainda dispondo dos recursos oferecidos pelo _SystemD_.

```ini
[Unit]
Description=Newsboat feed update

[Service]
Type=oneshot
ExecStart=%h/.local/bin/news-reload
```

```bash
#!/usr/bin/env bash
# This script will be activated by a systemd unit. First check if there is
# network connection, then try to update articles database for newsboat. If the
# main task fail, exit. This is necessary to avoid error log creation on user
# level process supervisor.

ping -c 1 cdn.openbsd.org >/dev/null 2>&- || { echo "Network is down." ; exit 0 ; }

newsboat -x reload >/dev/null 2>&- || echo "Newsboat is open."
```

Para as finalidades desse artigo, adotou-se o diretório `~/.local/bin` para guardar-se o _script_.

O supervisor de processos, parte do _SystemD_, não _conhece_ as variáveis de ambientes definidas através, por exemplo, de _profile_ (`.bash_profile`, `.zprofile`). Existe uma forma própria para ter variáveis de ambiente dentro do supervisor de processos.

De modo a evitar a complexidade de lidar com as variáveis de ambiente, usou-se usou-se de um caminho absouluto para o _script_ na parametrização da tarefa, mas lançando mão do especificador que expande para o diretório do usuário sob o qual se executa o supervisor de processos: `%h` -> `/home/nome_do_usuario`.

### Efeito
O que acontecerá será que de 15 em 15 minutos, conforme parametrizado em `newsboat.timer`, será acionada a unidade de mesmo prefixo, `newsboat`, acrescido do sufixo `.service`, `newsboat.service` portando, e este executa a linha de comando de atualização do `newsboat`, então aguarda o recebimento do código de saída da execução de linha de comando (0 significando sucesso).

A unidade de parametrização cronológica alternativa terá um comportamento umm pouco diferente. A unidade de serviço será acionada como descrito acima, mas o primeiro acionamento ocorrerá somente decorrido 1 minuto da primeira autenticação do usuário, quando é instanciado o supervisor de processos em nível de usuário para o respectivo usuário que foi autenticado. Os acionamentos seguintes serão realizados de 15 em 15 minutos contando do último acionamento da unidade de serviço.

Ao terminar-se toda a operação, o `newsboat` poderá lançar uma notificação informando novas publicações nas diversas interfaces _internet_ afora que tu acompanhas.

Assim não será necessário ter um terminal aberto com o `newsboat` num regime 24/7 para não perder qualquer atulização.

## Unidades agragadas em pacotes
Alguns pacotes trarão conisgo um uso desse recurso de execução periódica de tarefas. Listo, como exemplo (serão utilizados os nomes de pacote conforme os repositórios do Arch Linux na data de 18-03-2020):

  * O pacote `util-linux` fornece `fstrim.timer`, destinado para utilizadores de SSD;
  * O pacote `man-db` fornece `man-db.timer`, para lidar com o registro de manuais;
  * O pacote `pacman-contrib` fornece `paccache.timer`, para eliminar pacotes antigos do cachê;
  * O pacote `shadow` fornece `shadow.timer`, para verificação de senhas e grupos;

## Finalização
Certifique-se da atividade de seu componenete com o comando `systemctl --user list-timers`, que apresentará o momento da próxima execução da tarefa, o momento da execução anterior, a unidate de parametrização cronológica e a unidade de parametrização da tarefa.

## Leitura complementar

  * [Arch wiki:systemd/User](https://wiki.archlinux.org/index.php/Systemd/User)
  * [man:systemctl](https://man.archlinux.org/man/core/systemd/systemctl.1.en)
  * [man:systemd.timer](https://man.archlinux.org/man/core/systemd/systemd.timer.5.en)
  * [man:systemd.time](https://man.archlinux.org/man/core/systemd/systemd.time.7.en)
  * [man:systemd.unit](https://man.archlinux.org/man/core/systemd/systemd.unit.5.en)
  * [man:systemd.service](https://man.archlinux.org/man/systemd.service.5.en)
  * [man:systemd.exec](https://man.archlinux.org/man/systemd.exec.5.en)
  * [YT:Luke Smith:vídeo acerca do newsboat](https://www.youtube.com/watch?v=dUFCRqs822w)
  * [man:newsboat](https://man.archlinux.org/man/community/newsboat/newsboat.1.en)
