---
title: "Usando soquetes Unix em um protoloco IPC baseado em JSON para controlar \"remotamente\" o mpv"
date: "2020-04-14"
palavras-chave: ["mpv", "controle", "lua", "shell", "soquete unix"]
ano: ["2020"]
featured: false
---

 popular recurso de execução de mídia, o MPV, fornece uma infinidade de recursos pouco conhecidos e explorados tanto nas inúmeras possibilidades de parametrização do funcionamento, como no recurso de extensividade através de _scripts_ escritos em Lua (linguagem de programação criada no Brasil).

Aqui apresentamos um recurso ligeiramente _esotérico_ que objetiva dar um pouco mais de poder aos amantes dos atalhos de teclado, barras baseadas em _shell script_ e relacionados, a possibilidade de usar comandos do shell para enviar comandos ao MPV.

A versão do mpv utilizada para elaboração deste artigo é `0.32.0-3`, distribuição Arch Linux.

## Introdução
O mpv fornece uma parâmetro em sua linha de comando que permite disponibilizar um _soquete Unix_ por instância em execução do mpv, o `--input-ipc-server`. Naturalmanete será necessário usar de uma ferramenta que consiga usar o soquete, e para este artigo será utilizado o `socat`.

Este canal não fornece recursos de segurança, permite execução arbitrária de código, _sendo interessante somente para controlar localmente_ as instâncias do mpv. Um executor de música simples que fornece recursos mais interessantes para controle remoto é o `mpd`, que é um outro projeto, matéria mais interessante de se tratar em outros (porque o mpd fornece bastantes recursos) artigos.

Todo o conteúdo da comunicação através do soquete usa o formato JSON, tanto nas entradas como nas saídas.

## Uso básico, adaptado da documentação oficial
O mpv pode ser invocado da seguinte maneira:

```bash
$ mpv video.mp4 --input-ipc-server=/tmp/mpvsocket
```

Será aberto o programa, e será disponibilizado um soquete em `/tmp/mpvsocket`.

Para usar o soquete:

```bash
$ socat - /tmp/mpvsocket <<< '{ "command": ["get_property", "playback-time"] }'
{"data":190.482000,"error":"success"}
```

Para melhores informações, vide documentação.

## Aplicação com shell script
Tomaremos o cenário do bloqueio de tela. Antes de usar um recurso qualquer para bloquear a tela (`i3lock`, `slock`, `light-locker` _etc_), deseja-se que qualquer execução de mídia seja parada, de modo que não continue enquanto o operador da máquina vai fazer uma refeição ou fala com alguém, e depois possa ser retomada de onde parou.

### A criação de soquetes
Prevendo que podem haver várias instâncias do mpv abertas, por qualquer motivo, é necessário que o caminho de cada soquete criado seja único. Havendo um soquete para cada instância, sendo cada um um arquivo, é necessário lidar com isso também.

A criação de soquetes para cada instãnicia do mpv pode ser feita manualmente, facilitada através de um _alias_ do _shell_.

Uma outra forma de lidar com a criação dos soquetes por instância é usar a extensibilidade do mpv através de um _script_ em Lua.

Os _scripts_ para a extensibilidade do mpv em Lua, como o que será apresentado na sequência, devem ser depositados em `~/.config/mpv/scripts/`. Estes _scripts_ serão executados toda vez que o mpv for executado, e não precisam de permissão de execução.

```lua
-- mpv-socket, one socket per instance, removes socket on exit

local utils = require 'mp.utils'

-- Recuperar o diretório que contém os soquetes
tempDir = os.getenv("XDG_RUNTIME_DIR")

-- Função para concatenação de caminho usando a biblioteca fornecida pelo mpv
function join_paths(...)
    local arg={...}
    path = ""
    for i,v in ipairs(arg) do
        path = utils.join_path(path, tostring(v))
    end
    return path
end

-- Recuperar o PID da instância do mpv
ppid = utils.getpid()
-- Criar um diretório para os soquetes, se não existir, com permissões mais restritivas
os.execute("mkdir -pm 700 " .. join_paths(tempDir, "mpv-socket") .. " &>/dev/null")
-- Parametrizar a criação de um soquete com o PID da instância do mpv como nome
mp.set_property("options/input-ipc-server", join_paths(tempDir, "mpv-socket", ppid))

-- Função para remoção do soquete no encerramento da instânicia do mpv
function shutdown_handler()
    os.remove(join_paths(tempDir, "mpv-socket", ppid))
end
mp.register_event("shutdown", shutdown_handler)
```

### Usando os soquetes
Para interromper a execução em todas as instâncias do mpv abertas, e que disponibilizem um soquete em `/run/user/$(id -u)/mpv-socket`, ou `$XDG_RUNTIME_DIR/mpv-socket`, pode-se usar o _shell script_ abaixo:

```bash
#!/usr/bin/env bash
# Pause all mpv instances though the sockets

# Run through all files in directory
for i in $XDG_RUNTIME_DIR/mpv-socket/* ; do
    # Assert current file is a socket
    [[ ! -S "$i" ]] || {
        socat - $i <<< '{ "command": ["set_property", "pause", true] }' ;
    }
done
```

Este _shell script_ pode ser acionado em um atalho de teclado qualquer, na sequência para bloquear a tela, ou dentro de um outro _shell script_ que lide com a operação de bloqueio de tela.

## Finalização
Os recursos apresentados tem uma dependência: a variável de ambiente `XDG_RUNTIME_DIR`. Esta variável é disponibilizada sem intervenção em um arquivo _profile_ em sistemas com o _SystemD_. Mas nada impede de alterar o _script_ Lua e o _shell script_ para que use um outro diretório caso exista uma situação onde usar esse diretório não seja interessante ou possível (em sistemas que usem um outro _init_).

## Leitura complementar

  * [man:unix](https://man.archlinux.org/man/core/man-pages/unix.7.en)
  * [man:socat](https://man.archlinux.org/man/extra/socat/socat.1.en)
  * [Freedesktop:XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html#variables)
  * [Freedesktop:pam_systemd](https://www.freedesktop.org/software/systemd/man/pam_systemd.html#%24XDG_RUNTIME_DIR)
  * [mpv:Página oficial](https://mpv.io/)
  * [mpv:Documentação sobre o IPC](https://github.com/mpv-player/mpv/blob/master/DOCS/man/ipc.rst)
  * [mpv:Extensibilidade com Lua](https://github.com/mpv-player/mpv/blob/master/DOCS/man/lua.rst)
  * [mpvSockets:Código Lua base para o apresentado neste artigo](https://github.com/wis/mpvSockets)
  * [pauseallmpv:Script Bash base para o apresentado neste artigo](https://gitlab.com/LukeSmithxyz/voidrice/-/blob/master/.local/bin/pauseallmpv)

## Instrução sobre a linguagem Lua

  * [Canal Glider:Curso básico de Lua](https://www.youtube.com/playlist?list=PL61kTUcYddBMmu2L127wl0ZL1P1Vui919)
  * [Canal Glider:Dominando Lua](https://www.youtube.com/playlist?list=PL61kTUcYddBNHsI2gqA5G_6oUDIPOsdx2)

## Instrução sobre Bash e não só

  * [Curso básico de programação em Bash](https://debxp.org/cbpb)
