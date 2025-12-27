---
title: "Sxhkd"
date: "2020-02-10"
palavras-chave: ["sxhkd", "atalhos", "xorg"]
ano: ["2020"]
featured: false
---

O Sxhkd (_Simplex X Hot Key Daemon_, ou dæmon simples para teclas de atalho), criado para uso conjunto com o gerenciados de janelas (ou WM, de _Window Manager_) BSPWM, oferece um modo simples de configurar ações para determinadas combinações de teclas em qualquer ambiente que está sobre servidor gráfico X.org.

## Instrução mínima
O processo do Sxhkd, após ser iniciado (via de regra junto com o DE ou WM), aguardará _eventos de teclado_ (o pressionar uma tecla, por exemplo) para então executar algum comando estabelecido se houver algum atalho correspondente descrito no arquivo de configuração. Seu arquivo de configuração fica em `~/.config/sxhkd/sxhkdrc`.

Cada linha no arquivo de configuração é entendida da seguinte forma:

  * Se a linha inicia com `#`, é ignorada (marcação de comentário)
  * Se a linha inicia com um ou mais espaços em branco, é entendida como um comando
  * Senão for comentário ou comando, será entendida como descricação de atalho, o nome das teclas são separados por espaço e/ou `+`.

Antes de iniciar a especificação de atalhos, busque os nomes das teclas no X.org. Isso pode ser feito através do `xev` (v. Em inglês: [Arch Wiki:Entrada via teclado:identificação de teclas](https://wiki.archlinux.org/index.php/Keyboard_input#Identifying_keycodes_in_Xorg)):

```bash
$ xev | awk -F'[ )]+' '/^KeyPress/ { a[NR+2] } NR in a { printf "%-3s %s\n", $5, $8 }'
110 Home
112 Prior
117 Next
115 End
59  comma
60  period
61  semicolon
49  apostrophe
62  Shift_R
49  quotedbl
34  dead_acute
35  bracketleft
48  dead_tilde
51  bracketright
135 Menu
133 Super_L
```

A tecla que é chamada “Janela”, por obra dos contratos da Microsoft com as empresas fabricantes de maquinário em informática e periféricos, por exemplo, é chamada “super”.

Para mais detalhes de sintaxe e afins, consultar manual do Sxhkd.

## Funcionamento
De posse dos nomes das teclas, conhecida a sintaxe básica, a escrita é bastante simples. Segue modelo:

```bash
# Abrir terminal
super + Return
    termite

# Abrir explorador de arquivos
super + f
    nemo --geometry=1000x600

# Abrir cliente youtube de terminal
super + y
    termite -e mpsyt
```

### Detalhamento do funcionamento
O Sxhkd usará um _shell_ para executar os comandos, o que possibilita usar variáveis de ambiente (definidas no `.profile` por exemplo), além do uso de funcionalidades do _shell_ como o direcionamento de saídas de comandos:

```bash
# Abrir navegador de terminal
super + w
    $TERMINAL_CLIENT -e links

# Abir histórico do Clipman
super + F1
    xfce4-popup-clipman 2> /dev/null
```

Será executado pelo Sxhkd `SHELL -c COMMAND`, sendo a variável `SHELL` definida pelo conteúdo de duas variáveis de ambiente na prioridade:

  * `SXHKD_SHELL`, caso queira usar um _shell_ para o Sxhkd, independente do padrão
  * `SHELL`, a variável que aponta para o _shell_  padrão, utilizado se a variável anterior não estiver configurada

Um _shell_ para o Sxhkd pode ser definido no arquivo `~/.profile` apontando, através da variável de ambiente, para o caminho de um _shell_ da seguinte maneira:

```bash
export SXHKD_SHELL="/bin/sh"
```

## Recarregando configurações
Criando um arquivo em `~/.local/bin/` chamado [process-restart](https://paste.rs/I6B.sh "process-restart"), e adicionando o caminho ao `$PATH` no arquivo `~/.profile`:

```bash
export PATH="$PATH:$HOME/.local/bin"
```

Assim é possível criar um atalho para o script específico para reiniciar o processo do Sxhkd, de modo que alterações na configuração tenham efeito:

```bash
# Reiniciar sxhkd
super + Escape
    process-restart sxhkd
```

## Aplicações diversas

### Dmenu
É possível chamar qualquer _shell script_ em um diretório que esteja no `PATH` com permissão de execução, usando tecla de atalho, como já demonstrado acima no atalho para reiniciar o processo do Sxhkd. Isso abre o caminho para _scripts_ com o `dmenu`, por exemplo.

Descarregando o arquivo [mansearch-dmenu](https://raw.githubusercontent.com/debxp/scripts/master/mansearch-dmenu/mansearch-dmenu) para `~/.local/bin/` (clarificando, desde que esse diretório esteja no `PATH`) é possível obter um atalho dessa forma:

```bash
# Páginas de manual
super + F1
    mansearch-dmenu
```

### Múltiplos atalhos por linha
Usando algumas sintaxes mais complexas, é possível configurar múltiplos atalhos por entrada.

Aqui um exemplo com o mpc (que controla o mpd, _Music Player Daemon_, ou Dæmon para execução de música):

```bash
# Controle com o mpc
super + {dead_acute,bracketleft,dead_tilde,bracketright}
    mpc {volume -2,volume +2,toggle,stop}
super + alt + {dead_tilde,bracketright}
    mpc {prev,next}

super + {shift,control} + dead_tilde
    mpc seek -0{0:1,1:0}0
super + {shift,control} + bracketright
    mpc seek +0{0:1,1:0}0
super + {shift,control,alt} + dead_acute
    mpc seek -{05,10,30}:00
super + {shift,control,alt} + bracketleft
    mpc seek +{05,10,30}:00
```

### Comandos até que elaborados
Devido o Sxhkd usar um shell para execução dos comandos, atalhos como o seguinte são possíveis:

```bash
# Captura de área da tela com um gracejo
Print
    maim -s | \
    convert - \( +clone -background black -shadow 80x3+5+5 \) \
    +swap -background none -layers merge +repage \
    ~/Pictures/screenshot/shadow_$(date +%Y-%m-%d-%T).png ; \
    notify-send "Maim" "Selected region shadow screenshot was taken"
```

Para o atalho acima se requer:

  * maim, para a captura de tela;
  * `imagemagick`: `convert`, para o gracejo;
  * `coreutils`: `date`, para nomear o arquivo;
  * Algum servidor de notificações: `notify-send`.

## Considerações finais
Marcando que o Sxhkd, apesar de ter parte no BSPWM, pode ser usado independente de DE ou WM, portanto funcionará com o i3, DWM, Xfce, Openbox ou onde quiseres (não no Wayland, ou no TTY), bastando iniciar o processo do Sxhkd com a sessão do ambiente gráfico.

## Leitura complementar

  * [man sxhkd](https://www.mankier.com/1/sxhkd)
  * [Arch Wiki:Sxhkd](https://wiki.archlinux.org/index.php/Sxhkd)
  * [Repositório no GitHub](https://github.com/baskerville/sxhkd)
  * [Tópico no Fórum do Arch Linux](https://bbs.archlinux.org/viewtopic.php?id=155613)
