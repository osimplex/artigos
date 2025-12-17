+++
title = "Proposta em Bash para uma reimplementação de janela transiente com o xdotool"
date = "2022-11-17"
tags = ["xorg", "controle", "shell", "xdotool"]
featured = false
+++

Este artigo é uma resposta para a proposta publicada em 20 de abril de 2020, de implementação de janela transiente em Bash usando o utilitário wmctrl para interação com o gerenciador de janelas através da definições no servidor gráfico, o _Xorg_.

Toda a contextualização com as justificativas e apresentação do conceito de janela transiente foi apresentada no artigo que apresentou a proposta com `wmctrl`.

Este artigo foi construído usando recursos distribuídos pelo projeto Arch Linux no estado do dia 14 de novembro de 2022.

## 1. Introdução
Apresenta-se nessa sessão uma breve contextualização e a justificativa para a presente proposta

### 1.1. Contextualização
Finalizada a proposta de janela transiente com `wmctrl` levantou-se a hipótese de reimplementar o mesmo recurso usando outra ferramenta que permite interagir com o servidor gráfico, determinando comportamentos do gerenciador de janelas nos quesitos de visibilidade da janela. Essa ferramenta é o `xdotool`.

### 1.2. Justificativa
Apesar dos objetivos plenamente atingidos na proposta original, com `wmctrl`, dadas as diferenças do `xdotool`, sem apelos para ganho de desempenho ou alguma melhoria e ganho de recurso, essa proposta faz-se simplesmente no intuito de demonstrar a viabilidade de atingir os mesmos objetivos utilizando outra ferramenta, e realizando interações diferentes com o servidor gráfico.

Desta forma, este artigo visa contribuir com o progresso do entendimento acerca das possibilidades de uso das capacidades do servidor gráfico Xorg, e das diferentes formas e ferramentas de interação com o servidor gráfico.

## 2. Escolhas de projeto para a nova proposta
A nova proposta tem como premissa entregar as mesmas possibilidades da original e o mesmo comportamento percebido na interação.

Para a nova proposta decidiu-se pela utilização de um arquivo para controle das janelas transientes, ao invés de recuperar todas as janelas com a chamada _instância_ com o prefixo determinado, prefixo `transient-`, em todas as vezes que o _script_ é executado. Não foi idealizada qualquer prevenção para situação de execução do _script_ num cenário de dois servidores gráficos em funcionamento para um mesmo usuário na mesma máquina.

Eliminou-se a utilização do utilitário `cut` do GNU _coreutils_ e construiu-se uma função dedicada (`extract_field`) para as necessidades de recuperação de "coluna" que supre os requisitos internos do _script_.

Ao invés da determinação do átomo `_NET_WM_STATE_HIDDEN` via requisição `_NET_WM_STATE` definida na [EWMH](https://specifications.freedesktop.org/wm-spec/wm-spec-1.3.html), realizada na proposta com `wmctrl`, utilizou-se uma operação de modificação de estado da janela em linha com a [ICCCM](https://x.org/releases/X11R7.6/doc/xorg-docs/specs/ICCCM/icccm.html) e uma outra requisição também difinida na EWMH, de modo que o gerenciador de janelas responda de acordo determinando a visibilidade da janela.

Apesar disso não ser exatamente uma escolha de projeto, diferente da proposta anterior a aplicação que foi selecionada para o _script_ exemplo foi o ST, _Simple Terminal_ do projeto _Suckless_, por também suportar a parametrização do conteúdo que será definido para `res_name` na estrutura `WM_CLASS` (para uma discussão em detalhes, conferir o artigo da proposta original) em tempo de invocação, na linha de comando. Onde houver `$TERMINAL_SOLO`, ler `st`.

## 3. Resultados
Apresenta-se em seguida a implementação da proposta.

### 3.1 Implementação

``` bash
#!/usr/bin/env bash
# This script implements the scratchpad funcionality for WMs that don't have.
# This scratchpad is based on modified instance class, and uses xdotool.

spd_file="$XDG_RUNTIME_DIR/scratchpad.reg"
terminal="$TERMINAL_SOLO"
prefix="transient"
class="$1"

declare -A instance
instance[term]="$terminal -n ${prefix}-term -e tmux new-session -A -s dd"
instance[mpd]="$terminal -n ${prefix}-mpd -e ncmpcpp"
[[ -v instance[$class] ]] || exit 1

readonly field_separator="$IFS"
readonly current_desktop="$(xdotool get_desktop)"

extract_field() {
    local field="$2" ;
    IFS=':' ;
    set -- $1 ;
    echo "${!field}" ;
    IFS="$field_separator" ;
}

[[ -f "$spd_file" ]] && {
    IFS= read -d '' -r register < "$spd_file" ;
    register="${register%$'\n'}" ;
    target="$(grep -F "${prefix}-$class" <<< "$register")" ;
    IFS=$'\n' register=(${register/$target/}) ;
    IFS="$field_separator" ;
    : > "$spd_file" ;
} || {
    : > "$spd_file" ;
    chmod 600 "$spd_file"
}

[[ "${#register[@]}" -gt 0 ]] && for unit in "${register[@]}" ; do
    unit_id="$(extract_field "$unit" 1)"
    unit_cl="$(extract_field "$unit" 2)"
    xdotool getwindowname "$unit_id" >/dev/null 2>&- && {
        echo "$unit" >> "$spd_file" ;
        xdotool search --onlyvisible --classname "$unit_cl" >/dev/null || {
            xdotool set_desktop_for_window "$unit_id" "$current_desktop" ;
        }
        xdotool windowunmap "$unit_id"
    }
done

target_id="$(extract_field "$target" 1)"
if xdotool getwindowname "$target_id" >/dev/null 2>&- ; then
    echo "$target" >> "$spd_file"
    window_desktop="$(xdotool get_desktop_for_window "$target_id" 2>/dev/null)"
    xdotool windowunmap "$target_id"
    [[ "$window_desktop" -eq "$current_desktop" ]] || {
        xdotool set_desktop_for_window "$target_id" "$current_desktop" ;
        xdotool windowactivate "$target_id" 2>/dev/null ;
    }
else
    ${instance[$class]} &
    wid="$(xdotool search --sync --onlyvisible --classname ${prefix}-$class)"
    echo "$wid:${prefix}-$class" >> "$spd_file"
fi
```

## 4. Discussão
Abordar-se-á a nova proposta apresentando a idéia geral de cada etapa na implementação apresentada, algumas considerações sobre a função `extract_field` e uma descrição de cada um dos usos do utilitário `xdotool` na implementação apresentada.

### 4.1 Idéia geral da proposta
O método que viabiliza o recurso de janela transiente segue o seguinte esquema:
  * Caso exista o arquivo de controle de janelas transientes, recuperar todas as linhas do arquivo, que espera-se conter texto no formato `<número identificador da janela>:<nome de instância da janela>`, cada linha representando uma janela controlada. Determinar a existência de uma janela no controle que corresponda ao _objetivo_ informado via linha de comando e construir uma matriz indexada com todas as janelas controladas menos a _objetivo_.
  * Caso haja janelas controladas além da _objetivo_, modificar no servidor gráfico o estado destas janelas de modo que o gerenciador de janelas não dê às janelas visibilidade.
  * Caso a _objetivo_ exista no servidor gráfico, modificar no servidor gráfico o estado da janela para que o gerenciador de janelas possa dar a janela visibilidade, estabelecer que a janela esteja na área de trabalho virtual corrente e determinar que à janela seja dado foco.
  * Caso a _objetivo_ não exista no servidor gráfico, executar o comando determinado para iniciar uma aplicação de modo que possa ter o cliente no servidor gráfico controlado, determinando o nome de instância, e registrar a janela no arquivo de controle.

A parametrização do gerenciador de janelas para obter a questão da janela flutuante continua conforme a proposta original.

### 4.2. Notas sobre o extract_field
A função `extract_field` definida no _script_ foi elaborada para substituir o utilitário `cut`.

O objetivo dentro do _script_ é obter uma coluna em uma linha cujo formato foi especificado em _Idéia geral da proposta_, o que poderia ser obtido com um a linha de comando do `cut` especificando o delimitador `:` e indicando um campo com um numeral. Mas decidiu-se por realizar o processamento da linha com instruções em Bash puro da seguinte forma:
  * Definir uma variável local atribuindo o conteúdo de parâmetro posicional em que se informa um texto numeral que indica o _campo_, ou a coluna, desejada.
  * Determinar o _Internal Field Separator_, separador de campos, com a modificação do conteúdo da variável `IFS`.
  * Com o comando interno `set` determinar que cada _campo_ no conteúdo no parâmetro posicional `$1`, que é recebido na linha de comando em que se invoca a função, seja definido como conteúdo de parâmetros posicionais no contexto da função.
  * Escrever na saída padrão o conteúdo do _campo_ definido no parâmetro posicional usando uma expansão indireta de parâmetro, de modo que se determine de qual parâmetro deseja-se recuperar o conteúdo a partir do conteúdo da variável que guarda o campo definido na chamada da função como desejado.

Após a etapa do `set` os parâmetros posicionais começando pelo `$1` têm conteúdo igual a cada _palavra_ definida após `--`. Para que a totalidade do texto informado após `--` não seja encarado como uma única palavra, dado o formato com separador dos elementos sendo `:`, realizou-se a a definição do _Internal Field Separator_ como `:`, tornando `:` o caractere separador de palavras.

Desta forma, com a linha de comando `extract_field '1234:texto' 1`, obtém-se uma saída igual a _1234_, modificando o segundo argumento para 2, obtém-se uma saída igual a _texto_.

### 4.3. Notas sobre o xdotool
`xdotool` é o utilitário central nesta proposta. Através dele se interage com o servidor gráfico, determinando algumas operações do gerenciador de janelas para obter o cumprimento dos requisitos para atingir o conceito de janela transiente.

O comando `xdotool getwindowname`, passando um numeral identificador de janela como segundo argumento, faz uma consulta junto ao servidor gráfico para recuperar o que se considera o título da janela na estrutura de dados associada à janela. Caso não exista a janela o comando encerra devolvendo um status de erro.

O comando `xdotool search`, a depender de quais argumentos mais são especificados, realiza uma busca junto ao servidor gráfico para recuperar os números identificadores das janelas cujas propriedades satisfazem os critérios da pesquisa. Caso nenhuma janela satisfaça as condições da pesquisa, o comando encerra devolvendo um status de erro.

O comando `xdotool set_desktop_for_window` determina uma propriedade para uma dada janela de modo que isso indique ao gerenciador de janelas que ela deve ter visibilidade em uma _área de trabalho virtual_, informada na forma numeral.

O comando `xdotool windowunmap` é utilizado para realizar a manipulação de estado mencionada acima, o que se realiza em última instância com uma chamada da função [XUnmapWindow](https://man.archlinux.org/man/extra/libx11/XUnmapWindow.3.en) da _Xlib_. Em linha com a [ICCCM](https://x.org/releases/X11R7.6/doc/xorg-docs/specs/ICCCM/icccm.html), espera-se que o gerenciador de janelas não dê visibilidade à janela que sofre manipulação no estado em nenhuma área de trabalho virtual.

O comando `xdotool windowactivate` é utilizado para realizar uma requisição `_NET_ACTIVE_WINDOW` definida na [EWMH](https://specifications.freedesktop.org/wm-spec/wm-spec-1.3.html), de modo que o gerenciador de janelas realize a transferência de foco para uma dada janela definida nos argumentos subsequentes. Esta requisição ao servidor gráfico desencadeia uma resposta do gerenciador de janelas, no caso do `spectrwm`, também de “mapeamento” da janela caso ela esteja num estado não mapeado, de modo que o gerenciador de janelas torna a dar visibilidade para a janela alvo da requisição de “ativação”.

## 5. Finalização
Espera-se que este artigo, assim como o que trouxe a proposta original, traga uma contribuição interessante para os utilizadores do spectrwm, e para os que dispensaram esta opção de gerenciador de janelas pela ausência do recurso de janela transiente. Talvez até para utilizadores de outros gerenciadores de janelas que forneçam o recurso, mas que achem a presente proposta interessante, mesmo que para fins de curiosidade ou instrução.

Independente de se essa proposta terá algum valor, ou se será interessante usá-la, espera-se que este artigo sirva como uma pequena amostra do funcionamento de algumas pequenas coisinhas, e das inúmeras possibilidades oferecidas por um entendimento mais aprofundado de como efetivamente funcionam as coisas.

## Leitura complementar

  * [man:bash](https://man.archlinux.org/man/core/bash/bash.1.en)
  * [man:grep](https://man.archlinux.org/man/core/grep/grep.1.en)
  * [man:xdotool](https://man.archlinux.org/man/community/xdotool/xdotool.1.en)
  * [xodotool:Página oficial](https://www.semicomplete.com/projects/xdotool/)
  * [ArchWiki:spectrwm](https://wiki.archlinux.org/index.php/Spectrwm)
  * [GitHub:spectrwm](https://github.com/conformal/spectrwm)
  * [Proposta anterior de janela transiente](https://www.semicomplete.com/projects/xdotool/)

### Documentação sobre janela transiente nos WMs

  * [i3wm:scratchpad](https://i3wm.org/docs/userguide.html#_scratchpad)
  * [Qtile:scratchpad](http://docs.qtile.org/en/latest/manual/config/groups.html#scratchpad-and-dropdown)
  * [Suckless:dwm:scratchpad](https://dwm.suckless.org/patches/scratchpad/)

### Documentação sobre diretivas para janelas específicas nos WMs

  * [man:spectrwm:Parâmetro quirk](https://man.archlinux.org/man/community/spectrwm/spectrwm.1.en#QUIRKS)
  * [Qtile:Classes Match e Rule](http://docs.qtile.org/en/latest/manual/config/groups.html#match)
  * [i3wm:Diretiva for_window](https://i3wm.org/docs/userguide.html#for_window)
  * [dwm:config.def.h vetor rules](https://git.suckless.org/dwm/file/config.def.h.html#l24)
  * [awesomewm:Módulo awful.rules](https://awesomewm.org/doc/api/libraries/awful.rules.html)

## Exposições em vídeo

  * [Luke Smith:Dropdown Terminals and Scratchpads in i3wm!](https://odysee.com/@Luke:7/dropdown-terminals-and-scratchpads-in:f)
  * [Ditrotube:Xmonad and Named Scratchpads](https://odysee.com/@DistroTube:2/xmonad-and-named-scratchpads:e)
  * [DistroTube:Spectrwm Is An Impressive Tiling Window Manager](https://odysee.com/@DistroTube:2/spectrwm-is-an-impressive-tiling-window:f)
  * [DistroTube:Discovered Some Cool Stuff In Spectrwm and Qtile](https://odysee.com/@DistroTube:2/discovered-some-cool-stuff-in-spectrwm:d)

## Instrução sobre Bash e não só

  * [Curso básico de programação em Bash](https://debxp.org/cbpb)
  * [Curso Shell Gnu](https://blauaraujo.com/downloads/curso-shell-gnu.pdf)
