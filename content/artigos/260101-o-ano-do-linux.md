---
title: "O ano do GNU/Linux no meu desktop"
date: "2026-01-01"
palavras-chave: ["computacao", "linux", "construcao", "controle"]
ano: ["2026"]
featured: false
---

Entre as diversas realidades com que se tem contato no meio de entusiastas do sistema operacional _GNU/Linux_ está o folclore do "ano do Linux no _desktop_". Este artigo não se propõe a dissecar os elementos do dizer, mas colocá-lo em perspectiva e ver até onde o texto correrá nos termos do título.

## Primeiro contato

A minha história de descoberta do "Linux", que é o nome que me apresentaram, tem capítulos difusos. Mas o maior momento foi o primeiro teste no ano de 2012, em que tive contato por curiosidade com uma distribuição chamada _BackTrack_. Instalada em _dual boot_.

Apesar do apelo de "instalação ferramenta" do projeto, basicamente focado em testes de segurança de sistemas computacionais, o que me chamou atenção no projeto foi a estética, que era sensivelmente diferente do _Windows Vista_, sistema operacional que usava na altura. Gnome 2, o tema com cores mais escuras, e um funcionamento adequado.

A experiência durou pouco, logo desinstalei o sistema operacional. Entre as causas está o fato de que o computador que utilizava não era meu, e o menu do _GRUB_ (na altura nem sabia o que era _GRUB_) era desconfortável aos demais utilizadores. Além do menu do GRUB, o _Windows_ apresentava um comportamento de aumento de uso de armazenamento no decorrer do tempo, por atualizações especialmente, o que chegava até a gerar a "necessidade" de "formatar o PC" periodicamente, então ficar com o HD dividido entre dois sistemas operacionais não era viável.

Definitivamente, 2012 não foi "o ano do Linux no meu _desktop_".

## Acúmulo de tensões

Muita água passou debaixo da ponte. Passei do _Windows Vista_ para o _Seven_, e depois para a versão 8.1 (que foi muito mal falada, mas me servia). A situação mudou quando veio o tal do _Windows 10_, assim que foi instalado pela primeira vez.

Faz parte do processo normal de "formatação do PC", como se diz no dialeto janeleiro, algum trabalho de ajuste. Instalação de _drivers_, configuração de "issos e aquilos". E é aí que percebi uma mudança: o início da desidratação do chamado "Painel de Controle", que é uma espécie de central de configurações do _Windows_, em favor da nova tela de configurações perfumada e, na visão que tive na época, limitante. Especialmente em um detalhe: controle de atualizações, que era importante para reduzir a necessidade de "formatação".

Esse incômodo rebocou outros, que já existiam, mas não se faziam sensíveis. Foi a gota d'água, como se diz. Os esquemas de "sugestão" de criação de conta _Microsoft_, por exemplo, que incomodou primeiro pelo inconveniente, e cada vez mais conforme era escondida a opção de criar um usuário local no sistema operacional.

A "formatação" periódica que acabava se fazendo necessária por causa da degradação de desempenho do sistema operacional, o trabalho que tinha para "otimizar o sistema", o problema do canal confiável para obtenção de _software_, a desgraça que era para desinstalar algo e tentar apagar todos os resquícios para poupar armazenamento.

Talvez possa ser dito que umas tantas questões derivam não do sistema operacional, mas da minha realidade. Não era o sistema operacional o problema, a culpa do HD lotar em poucos meses (evitando ao máximo salvar arquivos no HD, usava _pen drive_ para arquivo pessoal) era minha, por não ter dinheiro para comprar um HD enorme. Essa questão deixo para meu caro leitor.

## O momento de ruptura

O processo todo parece bastante, para quem conhece a terminologia, com aquilo que se conceitua como _salto qualitativo_.

Num dado momento desse processo de manifestação de insatisfações latentes a memória pescou uma palavra mágica no fundo do baú: _Linux_. De repente aquele sistema operacional da experiência em 2012 poderia ser melhor para as minhas necessidades. E aqui estamos no ano de 2017.

Também foi no ano de 2017 que consegui comprar o primeiro computador próprio, por 530 BRL, um _notebook_ Dell com processador i5 de primeira geração que, não sabia na altura em que comprei, era de um modelo com defeito crônico de arrefecimento (por isso, além de já ser velho na época, que foi barato). Isso tornou a exploração mais viável.

Aquele ano pesquisei até onde dei conta, com o pouco que sabia. Descobri as distribuições de _GNU/Linux_, tive os primeiros contatos com os conceitos de _software livre_ e _código aberto_ etc. Fiz um mapeamento do que se dizia sobre o assunto nos canais de divulgação da época, e decidi que miraria alguma distribuição fundamental (_Debian_, _Arch_, _OpenSUSE_ ou _Fedora_), ou adotaria alguma derivada para aclimatação e poder chegar na fundamental.

Passada a pesquisa, fiz os primeiros testes. _OpenSUSE Tumbleweed_, _Fedora_ e _Manjaro_, exatamente nesta ordem. Testes de alguns dias com cada uma, experimentando o que era específico e julgava mais sensível na administração do sistema operacional: gestão de pacotes. Os testes deixaram de ser testes no _Manjaro_, porque decidi pelo `pacman`, portanto pelo _Arch Linux_. Não acreditava que tinha condições de assumir um _Arch Linux_ de saída.

Importante: essa migração foi feita de forma radical, sobrescrevi o _Windows_ e dediquei todos os recursos da máquina para o _Manjaro_. Não tinha "plano B", a única opção era aprender e fazer dar certo.

A migração aconteceu nos primeiros dias de 2018, este que pode ser considerado "um ano do Linux no meu _desktop_". Ao menos esse é o termo usado pelos divulgadores: _Linux_.

## O processo de iniciação

Saindo do _Windows_, e diante do que conheci nos canais de divulgação, não estava trocando de sistema operacional, em certo sentido estava trocando de planeta. Só que não era uma questão se teria que me esforçar nessa mudança, mas se as tensões acumuladas fariam parte da nova realidade. Não adianta trocar "seis" por "meia dúzia". Esforço é natural para tudo nessa vida, não é verdade?

Os indícios não poderiam ser melhores. Se pudesse resumir em uma palavra o que buscava nessa jornada, essa palavra seria _controle_. E a administração de um sistema operacional _Unix-like_ era exatamente o que queria, sem saber até então que queria. Mais adiante percebi como essencial _para o propósito que almejava_ o fundamento das [quatro liberdades essenciais que definem o _software_ livre](https://www.gnu.org/philosophy/free-sw.en.html#four-freedoms). 

Mas, além do _controle_, haviam os quesitos de integridade do sistema operacional através do tempo, e a qualidade da administração do sistema.

Dito isso, acho que não é muito difícil entender como acabei interessado em aprender a utilizar o `pacman`, e a operar um gerenciador de pacotes. Esse foi o primeiro sinal visível do controle, experimentado através de duas operações: desinstalação de pacote e atualização. Mais a desinstalação do que a atualização, porque usei por anos o _Windows_ com atualizações desabilitadas para retardar a degradação, e pelo fato de que o _software_ desinstalado é de fato desinstalado (passados os parâmetros adequados para o gerenciador de pacotes). Fora a fonte confiável de _software_, o repositório da distribuição.

O quesito _rolling release_ também era bastante interessante porque sinalizava uma hipótese em que, talvez, a formatação deixaria de ser uma tarefa de rotina, um inconveniente que se impõe. Ou no mínimo dispensava o volume de trabalho que poderia ser necessário nas grandes atualizações em outro modelo de versionamento, tal qual praticado no _Debian Stable_.

A operação de linha de comando foi uma experiência nova, mas nunca foi um incômodo. Só levou algum tempo até pegar os rudimentos, com alguma pesquisa consegui entender o uso de _software_ via CLI, e era assim que queria aprender. Via um potencial na computação baseada em semântica, que se demonstrou na descoberta (um tanto depois) do trabalho do professor Blau Araújo, que foi o responsável pela noção que tenho hoje sobre _shell_.

Resumindo 17 meses: se no _Windows_ o aprendizado era um cabo de guerra com o sistema operacional, uma luta para neutralizar as consequências da natureza do _Windows_, no _GNU/Linux_, obtido através do _Manjaro_, houve um concurso, não estava mais disputando com a máquina.

## O batismo de sangue

O objetivo era o _Arch Linux_.

Uma coisa sabia, que descobri ainda em 2017, que o _Arch_ é algo que se monta, não que se obtém montado. Quem escolhe as partes que constituirão um todo é o utilizador. Pois bem, só escolhe quem conhece, e passei 17 meses lentamente avaliando o _Manjaro_ com base em um raciocínio: se é algo montado, é possível verificar as partes, e conhecendo poderei escolher.

Comecei com uma instalação de _Manjaro_ com _Xfce_ obtida pela mídia de instalação. E esse foi o meu objeto de estudo. Numa certa altura, levou alguns meses até chegar nesse ponto, cheguei a instalar um _WM_ (do inglês: _Window Manager_) chamado _Qtile_. O objetivo era simples: validar a realidade da constituição do todo. Nessa altura já nem pensava mais em _Windows_, porque mesmo conduzindo meus estudos, claro que aplicando metodologia, as coisas simplesmente funcionavam, e hipóteses invalidadas eram perfeitamente reversíveis.

A construção de um ambiente com _WM_, seguida de alguns meses de uso sem qualquer problema, e da desinstalação do _Xfce_ (que não deixa lixo no sistema de arquivos, nem exige buscas místicas num _regedit_) foi um marco de validação da competência adquirida. Conhecia as partes constituintes do que era aderente às minhas necessidades, conhecia a gestão de pacotes, já tinha alguma proficiência na operação de CLI em geral.

O fórum do _Manjaro_ em todo esse processo também foi importante. Não exatamente por dúvidas que tirei, mas pelo ambiente e experiências pessoais que pude verificar. Havia um tópico em que as pessoas diziam quanto tempo tinham suas respectivas instalações, e algumas tinham uns bons anos. Tinha ali uma visão das possibilidades

Então chegou o dia de mudar. Houve a contribuição, mais uma vez, de algo do "sistema". Não o funcionamento da minha instalação, que sempre foi impecável, mas uma mudança de direcionamento do _Manjaro_ que aconteceu em meados de 2019. E então decidi que era o momento de cumprir o propósito do processo. Sem plano B.

Assim iniciou-se uma instalação que dura até hoje:

```log
[2019-09-06 15:00] [PACMAN] Running 'pacman -r /mnt -Sy --cachedir=/mnt/var/cache/pacman/pkg --noconfirm base base-devel vim'
```

2019 foi o verdadeiro ano do _GNU/Linux_ no meu _desktop_. Não porque até ali a experiência foi falsa, mas porque finalmente concluí a migração que foi iniciada em 2018.

## Notas

Ainda faltam alguns meses para esse marco, mas este ano serão completos sete anos desde que concluí meu processo migração. E essa parte da história nem vale a pena contar, porque é simplesmente computação pessoal acontecendo sem intercorrências.

Sobre a base do _GNU/Linux_ montei o sistema que preciso para as minhas necessidades, e simplesmente funciona. Não existe mais cabo de guerra com a máquina, somente a expressão do que é necessário que a máquina execute, e tudo funciona. Até onde entendo, computação é isso. Também mantenho _GNU/Linux_ que instalei para outras pessoas, do mesmo jeito que teria que fazer se essas pessoas fossem utilizar _Windows_, porque não são todos os dispostos ao aprendizado de como lidar com computador, com a diferença de que não dá problema. Chega a ser chato, mas é uma fabulosa chatice.

Vez ou outra avalio alguma ferramenta, pra não dizer que nada aconteceu desde então. Recentemente adotei o _Emacs_, e estou utilizando o _Hugo_ para publicar artigos como este, mas a base é a base.

Dificuldades no caminho? Fundamentalmente uma só: o material disponível. Isso pode parecer contraditório para quem fica no lugar-comum da tal "era da informação", mas a disponibilidade de material não tem correlação com qualidade. Pelo fator _software_ livre a situação é mais favorável, mas há lapsos importantes quando se quer, por exemplo, estabelecer o entendimento das partes e da articulação entre elas. Isso é básico, da base, mas a base é frequentemente negligenciada.

Dito isso, recomendo fortemente que verifiquem o trabalho do professor Blau Araújo. Um trabalho de base (que não pode ser confundido com trivialidades, isso é matéria de canais de divulgação), muito sólido. [Vale a consulta](https://blauaraujo.com/). Os temas são variados, _shell scripting_, programação em C, programação em _Assembly_ e outros, sempre com metodologia e desenvolvendo fundamentação em cada passo.

Um outro professor que também disponibilizou um trabalho de base sólido foi o Paulo Kretcheu, este focado no sistema operacional _GNU/Linux_. Em seu [canal no _YT_](https://www.youtube.com/@kretcheu2001) há um curso completo e diversos laboratórios focados em resolução de problemas reais de alunos.

O trabalho dos dois professores supracitados, ao menos até onde vai meu conhecimento, não tem paralelo nem em inglês. Tanto pela base técnica, como pelo conhecimento da história do _software_ livre, do _open source_, e questões relacionadas.

Neste portal há alguns artigos que documentam o resultado de pequenas pesquisas que realizei, e o leitor sinta-se convidado a verificar caso tenha interesse.

## Conclusão

Cada um tem sua história, e agora o leitor pode conhecer a minha. Espero que o relato seja útil de algum modo, senão ao menos uma satisfação de "curiosidade antropológica" sobre o tipo de pessoa que pode haver do meio de _GNU/Linux_.

E devo dizer que todo ano é ano do _GNU/Linux_ no meu _desktop_.
