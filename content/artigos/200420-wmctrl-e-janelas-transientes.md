---
title: "Proposta em Bash para uma implementação independente de janela transiente com o wmctrl"
date: "2020-04-20"
palavras-chave: ["xorg", "controle", "shell", "wmctrl"]
ano: ["2020"]
featured: false
---

Aqui forneceremos uma situação de entendimento um pouco mais exigente, uma vez que vai precisar que o leitor entenda uma série de detalhinhos importantes (que o autor tentará comentar), e uma visão, a visão com que tentou-se pensar ao redigir o que será apresentado mais adiante: componentes independentes, com comportamento e interfaces definidas, trabalhando de forma ordenada, cada um com sua devida responsabilidade.

As janelas transientes são um elemento que _pode_ contribuir com algumas metodologias de uso do computador. Elas ficam vivas, nos bastidores, e podem ser chamadas via atalho de teclado (via de regra), então surgem na tela, faz-se o que quer, então manda-se a janela novamente para o oculto, e num outro momento essa janela é novamente chamada. Usos para esse tipo de recurso é um bloco de notas, uma interface de tocador de músicas, um emulador de terminal _etc_.

É como uma espécie de "faz-tudo", some esporadicamente, mas sempre está por perto.

Este artigo foi construído usando Arch Linux com os seguintes programas, nas respectivas versões:

  * Alacritty : `0.4.2-1`
  * wmctrl : `1.07-5`

## 1. Introdução
Apresenta-se nessa sessão uma breve contextualização e a justificativa para a presente proposta

### 1.1. Contextualização
Em pesquisa acerca da viabilidade de um gerenciador de janelas (ou WM, do inglês: _window manager_) _esotérico_ chamado _spectrwm_, com uma proposta bem interessante, percebeu-se a ausência de um recurso, o que chamamos neste artigo de janela transiente, que pode corresponder à três palavras da língua inglesa: mais próximo da situação deste artigo é _scratchpad_, quando se trata de um emulador de terminal pode se encontrar o termo _drop-down_, ou ainda o termo _top-down_.

Para os ambientes de trabalho como Plasma, Xfce _etc_, existem emuladores de terminal dedicados, ou que também oferecem a funcionalidade _drop-down_. Exemplos de emuladores dedicados são o [Guake](http://guake-project.org/), o [Tilda](https://github.com/lanoxx/tilda) e o [Yakuake](https://kde.org/applications/system/org.kde.yakuake). Entre os emuladores de terminal tradicionais que oferecem o _drop-down_ como um recurso está o [Xfce4-terminal](https://docs.xfce.org/apps/terminal/dropdown).

O termo _scratchpad_ será encontrado já no contexto dos gerenciadores de janelas. Exemplo de fornecedores desse recurso são o i3wm e o Qtile. O Xmonad, conta com esse recurso, mas é necessário descarregar uma biblioteca, que no Arch Linux se encontra no pacote `xmonad-contrib`. O DMW, como em outras diversas questões, não fornece esse recurso, mas tu podes adicioná-lo com a ferramenta `patch`, e o código-fonte para adicionar o recurso se encontra no sítio do projeto _suckless_.

> Não confundir a noção de _janela transiente_, deste artigo, com a propriedade ''WM_TRANSIENT_FOR'' definina no [ICCCM](https://www.x.org/releases/X11R7.6/doc/xorg-docs/specs/ICCCM/icccm.html#client_properties), e estendida na especificação [EWMH](https://specifications.freedesktop.org/wm-spec/wm-spec-latest.html).

### 1.2. Justificativa
No repositório do spectrwm, hospedado no GitHub, há uma [requisição](https://github.com/conformal/spectrwm/issues/222) de outubro de 2018 da funcionalidade de janela transiente (ou _scratchpad_, em inglês), que foi indeferida, mas houve um comentário na sequência, já em 2019, sugerindo a construção de algo com a ferramenta `wmctrl`, e apresentou um pequeno _shell script_ com uma proposta básica.

Essa proposta, porém, apresenta algumas limitações, como não ser projetado para lidar com mais de uma instância.

E um ponto de aprimoramento que pode-se destacar de pronto seria a dependência do título da janela que será controlada. O problema é que, em muitos casos, é um comportamento esperado a modificação do título da janela conforme o uso.

Portanto, aqui será aproveitada a idéia, de construir esse recurso com base no `wmctrl`, mas de uma forma aprimorada, e, espera-se, que permita pensar em uma possibilidade de uso mais amplo que _um só emulador de terminal_. Somando-se ao fato de que alguns gerenciadores de janela não oferecem o recurso das janelas transientes, espera-se que os resultados dessa proposta sirva com contribuição, e ajude no aprimoramento do modo de utilizar a computação em ambientes gráficos.

### 1.3. Apresentando a idéia de janela transiente
A janela transiente é a janela de uma aplicação que o operador quer aparente somente em alguns momentos, e nos outros momentos fica oculta.

Aqui entra o detalhe, essa janela não é destruída, o operador não _fechou_ a aplicação, _como_ na operação de minimizar, em que a janela também não deixa de ser exibida, para que depois seja novamente exibida e o trabalho ali continue. Também pode haver uma comparação com o recurso de algumas aplicação de minimizar para a área de notificações na barra de tarefas, onde continua a aparecer um ícone indicador, onde tu podes clicar e requisitar o retorno da janela.

Mas, a diferença está bem ligada ao contexto. Esse recurso está presente em gerenciadores de janela que trabalham com o particionamento de espaço da tela, como o i3wm. Estes gerenciadores têm um foco grande na idéia de ter várias áreas de trabalho para organizar o trabalho, e geralmente não se usa, ou sequer se oferece, o recurso de minimizar uma dada janela, contando mais com a segmentação em diversas áreas de trabalho, e tendo todas as janelas aparentes, usando todo o espaço da tela, cada janela com um pedaço, ou partição, sem invasão de espaço de uma janela por outra.

Neste contexto, a idéia da janela transiente tem um contorno mais interessante. Essa janela, via de regra, é flutuante, portanto ela vai ficar por cima de quaisquer outras que estejam numa dada área de trabalho. Outro detalhe é que ela pode ser aparecer e sumir em qualquer uma das diferentes áreas de trabalho, sempre contanto com um atalho de teclado para tal.

No meio do processo de redigir um documento, trabalho que pode demandar o uso de mais de uma área de trabalho, para organizar um navegador, um processador de texto, outros documentos que precisem de estar abertos _etc_, de qualquer lugar é possível fazer aparecer, _temporariamente_, uma janelinha que contenha um tocador de músicas, um bloco de notas, um outro navegador _etc_. E com um atalho de teclado.

## 2. Ferramental

### 2.1. wmctrl
O `wmctrl` é uma ferramenta que oferece a possibilita interagir com um gerenciador de janelas sobre o _Xorg_ que forneça certos aspectos conforme a especificação EWMH. Esta especificação padroniza algumas interações entre janelas, aplicações e utilitários em um ambiente gráfico, fornecendo algumas propriedades usadas no _Xorg_.

`wmctrl`, portanto, permite visualizar e manipular alguns aspectos do gerenciador de janelas e das janelas.

### 2.2. Bash
Para implementar a proposta deste artigo, usou-se o interpretador de comandos Bash, que se já não está pré-instalado, está ao menos presente ao menos nos repositórios das mais diversas distribuições, e também pode ser usado em outros sistemas, como os BSDs (FreeBSD, OpenBSD _etc_).

### 2.3. cut
Este comando permitirá extrair partes de uma dada cadeia de caracteres. Para este artigo usou-se o `cut` do _GNU coreutils_.

### 2.4. grep
Este comando permimirá filtrar textos com base em padrões. Para artigo usou-se o _GNU grep_.

## 3. Resultados
Apresenta-se em seguida a implementação da proposta.

### 3.1 Implementação

``` bash
#!/usr/bin/env bash
# This script implements the scratchpad funcionality for WMs that don't have.
# This scratchpad is based on modified instance class, and uses wmctrl.

terminal="$TERMINAL_SOLO"
prefix="transient"
class="$1"

declare -A instance
instance[term]="$terminal --class ${prefix}-term -e tmux new-session -A -s dd"
instance[mpd]="$terminal --class ${prefix}-mpd -e ncmpcpp"

[[ -v instance[$class] ]] || exit 1

readonly state="$(wmctrl -lx)"

extract_field() { echo $1 | cut -d' ' -f"$2" ; }
find_scratchpad() { grep -Fi "${1}." <<< "$state" ; }
wm_hide() { wmctrl -ir "$(extract_field "$1" 1)" -b add,hidden ; }

wm_toggle() {
    local target="$(wmctrl -d | grep -F '*' | cut -d' ' -f1)"
    local position="$(extract_field "$1" 2)"
    local identity="$(extract_field "$1" 1)"
    if [[ $target -eq $position ]] ; then
        wmctrl -ir "$identity" -b toggle,hidden
    else
        wmctrl -iR "$identity"
        wmctrl -ir "$identity" -b remove,hidden
    fi
}

[[ "${#instance[@]}" -gt 1 ]] && for unit in "${!instance[@]}" ; do
    if [[ "$class" != "$unit" ]] ; then
        window="$(find_scratchpad "${prefix}-$unit")" && wm_hide "$window"
    fi
done

if window="$(find_scratchpad "${prefix}-$class")" ; then
    wm_toggle "$window"
else
    ${instance[$class]} &
fi
```

## 4. Discussão
Abordar-se-á a presente proposta partindo dos dois pontos descritos mais acima: suportar mais de uma janela transiente, e não dependência do título da janela, começando pelo último.

### 4.1. Contornando a possível mudança do título da janela
Usando a ferramenta `xprop` é possível verificar uma série de propriedades atreladas à uma dada janela.

É possível verificar algumas propriedades (na documentação do _Xorg_ será encontrado o termo _atom_) para uma dada janela através do `xprop`. Duas são `WM_NAME` e `_NET_WM_NAME`, que podem apresentar o mesmo conteúdo. Sobre a primeira se lê na [documentação do Xorg](https://www.x.org/releases/X11R7.7/doc/man/man3/XSetWMName.3.xhtml). A última é definida na [EWMH](https://specifications.freedesktop.org/wm-spec/wm-spec-1.3.html), e recomenda que o gerenciador de janelas dê a preferência para ela. Essa é a propriedade que será lida quando algum elemento da interface for mostrar o título da janela, por exemplo, na barra de título de uma janela (que em vários casos é construída pelo gerenciador de janelas).

Outras propriedade que cabe destacar é a `WM_CLASS`, sobre a qual podemos ler mais em outra página da [documentação do Xorg](https://www.x.org/releases/X11R7.7/doc/man/man3/XAllocClassHint.3.xhtml). Essa propriedade se divide em duas partes, segundo podemos constatar na estrutura de dados apresentada: o nome da aplicação (`res_name`) e a classe da aplicação (`res_class`). Chamo atenção, seguindo a documentação, que se chama nome nesta estrutura de dados difere do que é apresentado na propriedade `WM_NAME` (e na propriedade `_NET_WM_NAME` também). A propriedade `WM_NAME` é destinada para uso em _exibição_, em uma barra de título, para usar o caso citado acima, e, portanto, pode trazer um conteúdo modificável, como o nome do sítio que tu estás a visitar no navegador, o que não acontece com _ambos_ os elementos da estrutura de dados `WM_CLASS`, eles não são modificados durante o ciclo de vida da janela.

Para a proposta deste artigo decidiu-se lançar mão do primeiro elemento da estrutura de dados `WM_CLASS`, `res_name`, por sua estabilidade.

Nossa proposta apresentará um exemplo com duas janelas do emulador de terminal Alacritty, que suporta a parametrização do conteúdo de `res_name` pelo utilizador na linha de comando que executa a aplicação.

Essa proposta não funciona só com o Alacritty, ou só com emuladores de terminal, funciona com qualquer aplicação em que se consiga parametrizar o conteúdo de `res_name`.

### 4.2. O suporte para mais de uma instância de janela transiente
De pronto se dirá: esse é o grande motivo da reescrita completa da proposta em Bash que foi apresentada, pois a mudança de propriedade para base de controle não exige tanta modificação do que foi apresentado naquele [comentário](https://awesomewm.org/doc/api/libraries/awful.rules.html) à requisição em 2018 junto ao projeto do spectrwm.

Para lidar de uma forma mais organizada com a multiplicidade de instâncias, visando uma facilidade de se adicionar e remover instâncias, usou-se de um recurso que lembra o _dicionário_. No dicionário, cada verbete tem algo atrelado, e tu buscas sempre pelo verbete. Isso tem parte, no Bash, na matéria das chamadas [variáveis indexadas](https://debxp.org/cbpb/aula06), e o nome seria _matriz associativa_, também podendo-se encontrar como _array associativa_. É a idéia do par _chave_ e _valor_, procurando-se através de uma chave, como o verbete, encontra-se um valor.

Usando esse recurso do dicionário, teremos um nome para controle de cada janela transiente associado com o respectivo comando para executar a aplicação que fornecerá essa janela, quando não houver ainda uma instância ativa.

Uma outra vantagem do uso do dicionário é que ele pode ser passado para uma estrutura de repetição (a proposta usa a estrutura do `for`). Essa estrutura de repetição tem como finalidade verificar a existência de cada instância possível diferente da requisitada, e para cada uma que existir, enviar uma instrução ao gerenciador de janelas que a oculte. Somente depois de ocultadas todas as janelas, fazer aparecer a janela requisitada.

Uma outra possibilidade, com o dicionário do Bash, é a [validação](https://git.savannah.gnu.org/cgit/bash.git/tree/NEWS?id=ac50fbac377e32b98d2de396f016ea81e8ee9961#n122) mais simples de existência do nome da janela requisitada no dicionário, através de uma das inúmeras possibilidades oferecidas pelo comando `test`. Também pelo `test`, somente será executada a estrutura de repetição se houver mais de uma entrada no dicionário.

Nas linhas de declaração do dicionário, onde serão definidas também as linhas de comando das janelas transientes, é necessário que a chave seja igual ao texto que segue a sequência `${prefix}-`.

A sequência `${prefix}-` foi sugerida, com `prefix` devolvendo o texto `transient` à título de exemplo (pode ser o que o utilizador preferir), por dois motivos: evitar que exista uma outra janela de mesma instância, assim possibilitando uma parametrização mais precisa de regras para determinar no gerenciador de janelas, por exemplo, que a janela transiente seja sempre flutuante.

### 4.3. Notas sobre o wmctrl
`wmctrl` é o utilitário central nesta proposta. Através dele se interage com o gerenciador de janelas, requisitando informações ou ações.

O comando `wmctrl -lx` devolverá uma lista com todas as janelas existentes. Ele dará o estado das coisas no momento da execução do _script_. `-lx`: `-l` indica a listagem de janelas, `-x` indica que deve ser fornecido também o par contido na propriedade `WM_CLASS`, e ele devolve isto na terceira coluna de cada linha, sendo uma linha por janela, no seguinte formato: `res_name.res_class`, usando a terminologia interna do _Xorg_, ou `instance.class`, usando uma terminologia mais próximo da documentação de alguns gerenciadores de janelas. Na função `find_scratchpad` se efetuará uma busca baseada nesse padrão, procurando sempre pelo nome de instância, e então retornará _toda a linha_ com as informações fornecidas acerca da janela requisitada.

Devolvidos por efeito da listagem de janelas (`-l`) são:

  * O identificador único de cada janela, na coluna 1;
  * O número da área de trabalho onde se encontra, cardinal, na coluna dois;
  * O título do janela, sempre ao final.

O comando `wmctrl -d` devolverá uma lista com as áreas de trabalho virtuais, na coluna 1, e a área ativa no momento da execução do comando será marcada com um `*` na coluna 2, enquanto as demais terão um `-`.

O wmctrl separa as colunas com uma quantidade variável de espaço, logo decidiu-se por uma estratégia para reduzir essa quantidade variável de espaços para um único espaço, assim possibilitando o `cut` extrair com mais precisão o conteúdo de uma coluna trabalhando com o espaço como separador. Isto se verifica na função `extract_field`.

Para executar as ações necessárias, usou-se o `wmctrl -b`. Este, porém, exige a identificação da janela que será alvo da ação, e esta identificação pode ser feita através de parte do título da janela (`-r`), do título completo da janela (`-Fr`) ou do identificador numérico da janela (`-ir`). Optou-se pelo identificador numérico da janela pela sua estabilidade, além de ser muito mais fácil de extrair sem erro do que o título da janela. O uso do `wmctrl -b` pode-se verificar nas funções `wm_hide` e `wm_toggle`.

### 4.4. O limbo
Há basicamente duas formas de fazer a janela “sumir”, de mandá-la para o “limbo”: usar o recurso de minimizar ou mandar a janela para uma área de trabalho que não é acessada em momento algum, que pode ou não ser mostrada no painel.

Alguns gerenciadores de janelas, como o Qtile e o Xmonad, usam a opção da área de trabalho.

Mantendo a opção da proposta original, usou-se do recurso de minimizar a janela. Mas seria perfeitamente possível usar a abordagem de uma área de trabalho “limbo” para isso, e, caso algum leitor prefira essa abordagem, basta adaptar as funções `wm_hide` e `wm_toggle`.

### 4.5. Uma leve parametrização no gerenciador de janelas
Os elementos da propriedade `WM_CLASS` podem aparecer na documentação de diferentes gerenciadores de janelas com os nomes `instance`, para `res_name`, e `class` para `res_class`. Para a propriedade `WM_NAME` frequentemente se encontra a palavra `title`.

Uma vez que, via de regra, se encontra o uso janelas transientes flutuantes, é necessário parametrizar uma diretiva no gerenciador de janelas, de maneira que ele não procure encaixar a janela no esquema de particionamento de tela.

Essa proposta foi construída com o spectrwm em vista, onde foi [julgado que isso está fora do escopo do  gerenciador de janelas](https://github.com/conformal/spectrwm/issues/222#issuecomment-437308460). Portanto, será apresentada a linha de parametrização apropriada para a implementação fornecida acima:

```
quirk[.*:transient-.*] = FLOAT
```

Chama-se a atenção para o texto `trasient-.*`. spectrwm suporta o uso de _RegEx_, e aqui colocou-se o texto que é conteúdo da variável `prefix`, `transient` mais `-`, e `.*`, que significa _qualquer coisa_. Essa linha estabelece: toda janela, independente do conteúdo de `res_class`, que contenha em `res_name` um texto que inicie `transient-` será criada como flutuante. A mudança do conteúdo da variável `prefix` exige a mudança da diretiva na parametrização do gerenciador de janelas, caso o operador deseje que as janelas transientes continuem a flutuar.

Caso o operador não queira um comportamento diferente do gerenciador de janelas em relação á uma janela flutuante, basta não adicionar diretiva alguma.

## 5. Finalização
Espera-se que este artigo traga uma contribuição interessante para os utilizadores do spectrwm, e para os que dispensaram esta opção de gerenciador de janelas pela ausência do recurso de janela transiente. Talvez até para utilizadores de outros gerenciadores de janelas que forneçam o recurso, mas que achem a presente proposta interessante, mesmo que para fins de curiosidade ou instrução.

Independente de se essa proposta terá algum valor, ou se será interessante usá-la, espera-se que este artigo sirva como uma pequena amostra do funcionamento de algumas pequenas coisinhas, e das inúmeras possibilidades oferecidas por um entendimento mais aprofundado de como efetivamente funcionam as coisas.

## Leitura complementar

  * [Infopédia:transiente](https://www.infopedia.pt/dicionarios/lingua-portuguesa/transiente)
  * [man:bash](https://man.archlinux.org/man/core/bash/bash.1.en)
  * [man:cut](https://man.archlinux.org/man/core/coreutils/cut.1.en)
  * [man:grep](https://man.archlinux.org/man/core/grep/grep.1.en)
  * [man:xprop](https://man.archlinux.org/man/extra/xorg-xprop/xprop.1.en)
  * [man:xwininfo](https://man.archlinux.org/man/extra/xorg-xwininfo/xwininfo.1.en)
  * [man:wmctrl](https://man.archlinux.org/man/community/wmctrl/wmctrl.1.en)
  * [wmctrl:Página oficial](http://tripie.sweb.cz/utils/wmctrl/)
  * [Freedesktop:EWMH](https://specifications.freedesktop.org/wm-spec/wm-spec-1.3.html)
  * [Freedesktop:wmctrl](https://www.freedesktop.org/wiki/Software/wmctrl/)
  * [ArchWiki:spectrwm](https://wiki.archlinux.org/index.php/Spectrwm)
  * [GitHub:spectrwm](https://github.com/conformal/spectrwm)
  * [Gustaf Sjöberg:Proposta base para a apresentada neste artigo](https://github.com/conformal/spectrwm/issues/222#issuecomment-457656578)

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
