---
title: "Polimorfismo em Bash, ou script orientado a confusão"
date: "2020-06-12"
palavras-chave: ["polimorfismo", "shell", "pog"]
ano: ["2020"]
featured: false
---

O recurso central a ser apresentado, brevemente, tem base na possibilidade de aninhamento de funções em Bash (declarar funções dentro do corpo de uma função). Será apresentada uma comparação, usando de força para caber a comparação, tentando facilitar o entendimento, com as interfaces do paradigma de orientação a objetos. A idéia neste artigo é uma automação doméstica para quem tem situações diferentes em diferentes estações de trabalho, aqui na questão do servidor de áudio e tocador de música.

## Introdução
Como Bash não é uma linguagem com suporte para o paradigma OO, então apelemos ao dicionário, onde podemos constatar como uma das definições de polimorfismo “_qualidade do que é capaz de assumir formas diferentes ou do que ocorre de diferentes formas_”.

## Caso concreto
Antes de tudo, um aviso: existem outras formas, até mais eficientes, de fazer o que o _script_ abaixo se propõe a fazer, portanto tome a demonstração meramente como demonstração.

Direto ao ponto, segue a demonstração:

```bash
#!/usr/bin/env bash

audio_server="pulseaudio"
music_player="mpd"

# _sndio() {
#     toggle() {}
#     down()   {}
#     def()    {}
#     up()     {}
#     "$order"
# }

_pulseaudio() {
    case "$element" in
        speaker)    local id="@DEFAULT_SINK@"   ; local dev="sink" ;;
        microphone) local id="@DEFAULT_SOURCE@" ; local dev="source" ;;
        *)          exit 40 ;;
    esac
    toggle() { pactl set-"$dev"-mute "$id" toggle ; }
    down()   { pactl set-"$dev"-volume "$id" -"$step"% ; }
    def()    { pactl set-"$dev"-volume "$id" "$step"% ; }
    up()     { pactl set-"$dev"-volume "$id" +"$step"% ; }

    "$order"
}

_mpd() {
    stop()     { mpc stop ; }
    toggle()   { mpc toggle ; }
    up()       { mpc volume +"$step" ; }
    down()     { mpc volume -"$step" ; }
    backward() { mpc prev ; }
    forward()  { mpc next ; }
    seek()     { mpc seek "$step" ; }

    "$order"
}

_cmus() {
    stop()     { cmus-remote --stop ; }
    toggle()   { cmus-remote --pause ; }
    up()       { cmus-remote --volume +"$step"% ; }
    down()     { cmus-remote --volume -"$step"% ; }
    backward() { cmus-remote --prev ; }
    forward()  { cmus-remote --next ; }
    seek()     { cmus-remote --seek "$step" ; }

    "$order"
}

while getopts ":o:s:e:am" opt ; do
    case "$opt" in
        a) _audio() { "_$audio_server" ; } ; exec="_audio" ;;
        m) _music() { "_$music_player" ; } ; exec="_music" ;;
        o) order="$OPTARG"   ;;
        s) step="$OPTARG"    ;;
        e) element="$OPTARG" ;;
    esac
done

"$exec"
```

## Resumo da ópera
Para usar este pequeno _script_, alguns cuidados não documentados são necessários em algumas partes, porque o polimorfismo em um cenário de automação vai depender do _mínimo múltiplo comum_ entre as partes, e isso é determinado na leitura cuidadosa dos manuais de cada comando que se deseja abstrair.

A finalidade do _script_ é lidar com recursos de áudio, tocador de música e servidor de áudio num cenário onde se quer usar o mesmo script em duas estações diferentes. Os tocadores contemplados foram `mpd`, que pode ser controlado remotamente através do `mpc`, e o `cmus`, que pode ser controlado remotamente através do `cmus-remote`. O único servidor de áudio contemplado foi `pulseaudio`, controlado através do `pactl`. Há uma entrada comentada para servidor de áudio `sndio`, que poderia ser controlado através do `aucatctl`, simplesmente para ter o exemplo da categoria do `pulseaudio`.

A abordagem do _script_ é análoga à algo do paradigma orientado a objeto. Numa linguagem com suporte para OO, uma das abordagens seria a de um dos chamados _padrões_, onde se define uma interface comum para determinadas operações, e então criar as estruturas que vão herdar dessa interface e implementar cada uma das operações. Bash não suporta o paradigma OO, mas através do recurso de aninhamento de funções se conseguiu um resultado semelhante, apesar de que, pela ausência da _interface_, o polimorfismo deve ser cuidado pelo programador, uma vez que não há compilador ou interpretador para acusar a não implementação de um método abastrato.

Cada função que lida com recurso deve implementar as funções para lidar com o recurso, e então realizar a chamada, o que se verifica com as linha onde figuram `"$order"`

Para exemplificar, as funções que lidam com um servidor de áudio devem implementar, necessariamente, quatro funções: `toggle`, para habilitar e desabilitar o som; `down`, `def` e `up` para diminuir, definir e aumentar o volume, respectivamente. Essas quatro funções devem produzir o mesmo resultado prático para diferentes recursos, dito de outro modo, a função `up` da função `_pulseaudio` deve produzir o mesmo resultado da função `up` da função `_sndio`, de modo que, quando este _script_ possa ser usado _da mesma maneira_ tanto em uma máquina com _Pulseaudio_ como em outra máquina com _sndio_, uma mesma operação, como a de dimunir o volume, realizada de formas diferentes. O mesmo se aplica aos tocadores de música, poque as funções `_mpd` e `_cmus` implementam funções que produzem o mesmo resultado prático, abstraindo as linhas de comando de cada utilitário de controle remoto dos respectivos tocadores de música.

O recurso do `getopts`, comando interno do Bash, não será explicado no presente artigo, mas sua finalidade é basicamente permitir um processamento um pouco mais simples de parâmetros em uma linha de comando como `script -a um -b`. Da forma como está o _script_ apresentado, uma possível linha de comando seria `$ audio-handler -m -o up -s 5`, que faria o tocador de música aumentar o volume em 5%.

## Finalização
O cenário de uso desse _script_ é atalhos de teclado, e faz parte de uma visão maior, onde se procura uma montagem _ideal_, onde se pode realizar troca de componentes com a menor dificuldade possível.

Com a abstração da interface de linha de comando utilizada para certas ações com diferentes recursos áudio, teoricamente não é mais necessário lidar com a parametrização de atalhos de teclado ao migrar de um recurso para outro. Caso surja um novo recurso, ao invés de se aventurar na costura de condicionais, basta criar uma nova função que faça abstração apropriada para este novo recurso, assim as demais implementações continuam funcionando, uma vez que não se tocará nelas, e a parametrização de atalhos de teclado permanece intocada.

Espera-se que essa programação orientada a confusão tenha se mostrado menos confusa, e que se tenha demonstrado, neste artigo, um exemplo de entendimento razoável para funções aninhadas em Bash.

## Leitura complementar

  * [Infopédia:polimorfismo](https://www.infopedia.pt/dicionarios/lingua-portuguesa/polimorfismo)
  * [man:mpc](https://man.archlinux.org/man/extra/mpc/mpc.1.en)
  * [man:cmus](https://man.archlinux.org/man/community/cmus/cmus.1.en)
  * [man:cmus-remote](https://man.archlinux.org/man/community/cmus/cmus-remote.1.en)
  * [man:pactl](https://man.archlinux.org/man/extra/libpulse/pactl.1.en)
  * [man:sndio](https://man.archlinux.org/man/community/sndio/sndio.7.en)
  * [GNU Bash:Bourne Shell Builtins](https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html#Bourne-Shell-Builtins)

## Instrução sobre Bash e não só

  * [Curso básico de programação em Bash](https://debxp.org/cbpb)
  * [Além do Bash](https://debxp.org/alem_do_bash)
