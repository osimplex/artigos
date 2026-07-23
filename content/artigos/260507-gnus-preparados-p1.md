---
title: "A arte da preparação de GNUs (I) - Base e Fundamento"
date: "2026-05-07"
palavras-chave: ["preparacao de GNUs", "teoria", "controle", "computacao"]
ano: ["2026"]
featured: true
---

Este é o primeiro artigo de uma série dedicada à arte de preparar um sistema operacional _Unix-like_ para uso pessoal. Será dada ênfase ao _GNU/Linux_ por ser o sistema operacional em que desenvolvi as competências fundamentais que serão descritas, mas é perfeitamente possível aproveitar bastante coisa que será dita num _FreeBSD_ etc. Neste primeiro artigo trataremos da preparação da base.

## Introdução

Esta é a série de artigos que gostaria de ter lido na época em que iniciei a exploração que resultou no sistema operacional que utilizo hoje, ou, sendo mais específico, no sistema operacional que construí com componentes de uma distribuição de GNU com Linux, e que hoje me assiste nas diversas necessidades de computação pessoal.

De saída vou pedir desculpas ao leitor pelo tipo de texto como o do parágrafo anterior. Considere como fruto de uma disciplina que desenvolveu-se em uma circunstância em que nada é dito com clareza e precisão. Como esta é a série que gostaria de ter lido, tentarei articular o que descobri até aqui da melhor maneira possível, dizendo os nomes das coisas e descrevendo as coisas, tentando evitar partir da premissa que o leitor sabe do que estou falando _em matéria de computação_.

O objetivo desta série é ilustrar os elementos da construção de um sistema operacional GNU/Linux e como eles se relacionam, de modo que o leitor tenha um mapa que o permita conduzir investigações específicas para construir com confiança o próprio sistema operacional.

## Só o _kernel_?

Como tratarei de GNU/Linux, uma limpeza de palavreado é necessária. Falar direito ajuda a pensar direito, e para construir um sistema operacional a clareza de pensamento é necessária em todas as etapas.

Comecemos por entrar em acordo sobre como serão utilizadas algumas palavras.

### A idéia de sistema operacional

Vamos começar com a idéia de _sistema operacional_. Um _sistema_ é necessariamente um conjunto de elementos que, juntos, funcionam de maneira ordenada para algum fim. Pense em sistema respiratório ou sistema digestivo, coisas que estudamos nas aulas de anatomia humana na escola, uma série de formações no corpo como boca, esôfago, estômago, intestinos trabalham de forma ordenada em prol da assimilação do alimento ingerido para nutrir todo o organismo. Já temos uma parte do conceito.

O _sistema operacional_ será necessariamente, em sentido amplo, o conjunto de componentes de _software_, programas de computador, que tornam o _hardware_, a máquina, de algum modo operacional e (termo horrível) "operacionalizável". Sem um _sistema operacional_ a utilização da máquina não é impossível, mas se torna mais complicada, pois será necessário escrever programação para lidar com cada componente eletrônico desejado e disponível no computador. O computador é um sistema também, só que eletrônico. Então o _sistema operacional_ gerencia os elementos do sistema eletrônico que é o computador, e fornece meios para utilização deste sistema eletrônico.

Agora vamos avançar na questão dos meios de utilização. Aqui temos a possibilidade de restrições e ampliações:
 1. Em uma definição mais restrita de _sistema operacional_, este seria um sistema que simplesmente realiza o gerenciamento do _hardware_ e disponibiliza uma interface para uso de outros programas de computador, aplicações, que acessarão os recursos do _hardware_ através do _sistema operacional_.
 2. Dando um passo adiante, o _sistema operacional_ pode fornecer meios para a criação e execução de programas.
 3. Mais um passo, o _sistema operacional_ pode fornecer meios para que uma pessoa opere o computador.
 4. Na maior amplitude, o _sistema operacional_ pode englobar toda a coleção de programas de computador que tornam a máquina útil para uma pessoa, incluindo processador de texto, navegador e assim por diante.
 
Para a definição nº 1 temos uma outra palavra específica: _kernel_. Um _kernel_ pode ser encarado como um _sistema operacional_ ou como parte de um _sistema operacional_, depende do que estamos querendo dizer com o termo _sistema operacional_, coisa que pode variar conforme o contexto.

Quem pegou outras épocas da computação conheceu computadores que disponibilizavam uma interface para programar em _BASIC_ ou carregar um programa _BASIC_ na memória. Neste caso temos a interação com sistemas operacionais que se encaixam na definição nº 2.

### O paradigma de sistema operacional

Nesta série trabalharemos com um sistema operacional _Unix-like_, o GNU/Linux. Então falemos rapidamente do _Unix_. _Unix_ é um outro sistema operacional, anterior ao _GNU_ (que vamos comentar mais adiante), que é paradigmático pela forma que foi construído, suas características.

Recorrendo ao "_The Unix Programming Environment_" de Brian Kernighan e Rob Pike, o primeiro capítulo inicia-se com a seguinte pergunta: o que é "_Unix_"? Ao que se seguem algumas definições:
 1. No sentido mais estrito, o _kernel_, um programa que controla os recursos de um computador e os aloca entre seus usuários, de um sistema operacional multitarefa (de tempo compartilhado em tradução literal).
 2. Em sentido mais amplo, "_Unix_" pode incluir não somente o _kernel_, mas também programas essenciais como compiladores, editores, linguagens de comando, programas para copiar e imprimir arquivos etc.
 3. Em sentido ainda mais amplo, "_Unix_" pode incluir programas criados por ti ou outros utilizadores para execução em teu sistema, como ferramentas para preparação de documentos, rotinas para análises estatísticas e programas para geração de gráficos.
 
Em seguida arremata: "qual desses usos do termo _Unix_ é correto depende de que nível de sistema está sendo considerado".

Não é tão diferente da lista anterior. Podemos perceber paralelos entre as definições 1 e 1, 3 e 2, 4 e 3.
 
Adiante, dado o modo como o _Unix_ foi construído, derivou-se uma noção para _sistema operacional_ _Unix-like_: um sistema constituído de quatro elementos que tornam um computador útil e o dão característica:
 * _Kernel_
 * Biblioteca C padrão
 * _Shell_
 * Conjunto de utilitários do sistema

Um descendente "vivo" do _Unix_ que temos disponível para utilizar hoje em dia é o _FreeBSD_, e podemos verificar neste as realidades comentadas. Ao instalar o _FreeBSD_ no computador temos o _kernel FreeBSD_ e demais componentes que completam o _sistema operacional FreeBSD_, aos quais podemos somar outros programas de computador que podemos criar com os recursos do sistema operacional ou obter de terceiros.

### A querela do nome do sistema operacional

De tempos em tempos aparece a questão de se é próprio dizer que um Arch ou um Debian seria um tipo de "_Linux_", ou distribuição de "_Linux_".

Realizada a etapa de definir a [idéia de sistema operacional]({{< relref "260507-gnus-preparados-p1#a-idéia-de-sistema-operacional" >}}), com as quatro definições do que pode ser um sistema operacional, podemos dizer com segurança onde se encaixa o Linux: na primeira definição. Não por acaso o _site_ em que o Linux é disponibilizado se chama [kernel.org](https://kernel.org/), e está escrito em letras garrafais na _homepage_ o seguinte: _The Linux Kernel Archives_.

Desta forma é seguro afirmar que um Debian instalado não _é_ um _Linux_, mas que, se for o caso, _tem_ um _Linux_. Porque pode sequer ter em algumas ocasiões. Existe instalação de Debian com _kernel FreeBSD_, e que não constitui um sistema operacional _FreeBSD_. Da mesma forma que o _Android_ não _é_ um _Linux_, _tem_ um _Linux_, o utiliza como _kernel_ junto de outros diversos componentes específicos do para constituir um sistema operacional.

Aqui introduzimos o sistema operacional GNU. Este sistema operacional é um _Unix-like_ (segue o paradigma do _Unix_) construído com a premissa de ser totalmente constituído de _software_ livre (assunto para outro artigo da série). O detalhe do sistema operacional GNU é que na maior parte das vezes ele não é utilizado com o _kernel_ desenvolvido pelo projeto GNU, o _Hurd_, mas com outros. O Debian fornece o sistema GNU junto ao _kernel_ _Linux_ ou _FreeBSD_. Por causa desse tipo de junção do sistema operacional (pela definição própria para sistemas _Unix-like_) GNU com um _kernel_ de outro projeto apareceu a noção de _GNU/Linux_, _GNU/kFreeBSD_ e assim por diante.

A querela do nome se deve ao detalhe de que, normalmente, o nome do _kernel_ não é destacado em detrimento do sistema operacional. Como acontece com o _Android_ e o _MacOS_ (cujo _kernel_ se chama _Darwin_). Mas entre as distribuições de GNU com Linux frequentemente se deixa de lado o nome do sistema operacional em prol do _kernel_. Isso acontece inclusive com projetos como o _Alpine_, que não utiliza elementos do sistema operacional GNU e oferece um sistema cuja base é a soma do _kernel Linux_ com a biblioteca _musl libc_ e o _Busybox_.

### O devido nome das coisas

Aparte a querela, para os interesses dessa série de artigos não deixo de notar que a insistência no nome do _kernel_ para nomear o sistema operacional é uma imprecisão que pode enevoar o pensamento. Basta frequentar um fórum de entusiastas e aguardar que logo aparece quem se queixe de algo em que "o _Linux_" não funciona como deveria, e, quando tu vais verificar o caso, o _Linux_ está funcionando muito bem, mas a pessoa configurou mal o comportamento de uma aplicação qualquer. 

Ainda que seja possível chamar o _Linux_ de sistema operacional, conforme o que já foi apresentado até aqui, a definição de sistema operacional em que o _Linux_ se encaixa não faz sentido para o contexto desta série de artigos, que não vai focar em minúcias de programação em baixo nível ou produção de sistemas embarcados com _Linux_. 

O mínimo conceito de sistema operacional adequado para o contexto desta série é de _sistema operacional Unix-like_, que adere à 3ª definição descrita na [idéia de sistema operacional]({{< relref "260507-gnus-preparados-p1#a-idéia-de-sistema-operacional" >}}). Assim sendo, _Linux_ é somente o _kernel_ e GNU (porque esta é a escolha de trabalho da série) é o restante do sistema operacional. Por vezes também chamaremos o sistema operacional pelo nome da distribuição, como Arch ou Debian, quando usarmos o termo em sentido ainda mais amplo, avançando para a 4ª definição.

## Um sistema operacional básico 

Estabelecido o conceito de sistema operacional _Unix-like_, passemos aos componentes. Já vimos que são quatro. Agora vamos dar algum detalhe do que seria cada um dos quatro.

Do _kernel_ já falamos, ele cumpre as funções da definição mais estrita de sistema operacional descrita na [idéia de sistema operacional]({{< relref "260507-gnus-preparados-p1#a-idéia-de-sistema-operacional" >}}). Podemos adicionar o detalhe há uma camada de interface para que aplicações interajam com o _kernel_, essa interface se dá pelas _chamadas de sistema_ (do inglês _system calls_).

Biblioteca C padrão, ou _libc_ para usar o termo mais curto, não é algo que se discutirá nesta série, mas é um conjunto de funções de uso comum elaborado sobre a estrutura das chamadas de sistema disponibilizado para o uso de aplicações escritas em C, ou que possam utilizar de bibliotecas em liguagem C. É uma outra forma das aplicações interagirem com o _kernel_, e o programador de cada aplicação pode optar por usar funções da _libc_ ou chamadas de sistema, uma coisa não exclui a outra.

O _shell_ é uma aplicação especial que fornece interface para executar outras aplicações. É através do _shell_ que uma pessoa poderá operar o sistema operacional e o _hardware_, sendo ele a interface padrão entre homem e máquina.

O conjunto de utilitários do sistema é uma série de pequenos programas que o utilizador pode utilizar através do _shell_. Em um sistema operacional _Unix-like_ os utilitários são construídos com finalidades bem específicas e de modo que eles possam ser combinados com recursos do _shell_ para servir diversas necessidades de computação do utilizador sem que para isso seja necessário desenvolver uma nova aplicação.

## Uma distribuição de sistema operacional

Essa é uma realidade bem característica do ramo GNU/Linux. Existe distribuição de sistema operacional no ramo dos BSDs e outros sistemas operacionais _Unix-like_ também, mas as premissas são diferentes.

Distribuição é uma espécie de projeto em que se toma _software_ livre produzido por terceiros para realizar (capitão óbvio na sala) a distribuição deste _software_ devidamente preparado para execução e uso, por vezes com modificações. Um exemplo seria o projeto _Linux libre_, que distribui um _kernel Linux_ modificado para eliminar o carregamento de elementos que não se enquadram como _software_ livre.

Mas quando se fala em distribuição geralmente o assunto não é distribuição de _kernel_, mas de sistemas operacionais.

Uma distribuição de sistema operacional é um projeto em que se toma componentes de sistema operacional produzidos por outras pessoas ou organizações, e se realiza todo um trabalho para distribuir estes componentes de modo que possam ser somados em um computador, ou instalados, e esta soma resulte em um sistema operacional que funcione. Essas poucas palavras estão longe de dizer o quanto isso dá trabalho, porque para isso é necessário alinhar algo na ordem de dezenas de milhares de peças de _software_ de uma forma que funcionem juntas e em harmonia.

Esse fenômeno é bastante característico do ramo GNU/Linux justamente porque veio com as iniciativas de distribuir sistemas constituídos da soma do _kernel Linux_ com os diversos componentes do sistema operacional GNU (_libc_, _shell_ e utilitários). Isso vem desde a década de 90, após a disponibilização do _kernel Linux_ sob uma licença livre, quando passou a fazer sentido somá-lo ao GNU para então produzir um sistema operacional 100% livre.

## Uma instalação de sistema operacional

Se a distribuição é o projeto que fornece diversos componentes de sistema operacional de uma forma que seja possível compor um sistema com eles, a instalação é a composição do sistema operacional "materializada" em um computador.

Uma distribuição tem como base o chamado _empacotamento_ de componentes, que consiste em gerar um arquivo que contém a, ou pode ser desdobrado na, coleção de arquivos necessários e/ou úteis para o funcionamento e uso do _software_ empacotado. Tomando como exemplo o próprio _Linux_, o _kernel_ em forma útil é frequentemente constituído de inúmeros arquivos, e para simplificar a entrega cria-se um arquivo, um _pacote_, que contém todos esses arquivos devidamente organizados para que possam ser _instalados_, isto é: copiados para as devidas posições de modo o conjunto funcione.

Para prosseguirmos o estudo adotaremos o Arch como distribuição por critério de conveniência para o autor da série. Mas todo o entendimento desenvolvido pode ser aplicado ao uso de outra distribuição como o Debian com algumas adaptações.

## A instalação básica

Vimos a definição de _sistema operacional Unix-like_ anteriormente. Ao lidar com distribuições teremos uma outra idéia construída sobre esta que é a _instalação básica_, que consiste no menor conjunto de pacotes que definem a instalação de uma dada distribuição. Pode-se entender que esse conjunto frequentemente vai um pouco além dos quatro elementos de um sistema _Unix-like_, incorporando pacotes que servem à obtenção de outros pacotes da distribuição por exemplo. 

Se retornarmos à [idéia de sistema operacional]({{< relref "260507-gnus-preparados-p1#a-idéia-de-sistema-operacional" >}}), por mais que uma _instalação básica_ tenha relativamente pouca coisa comparada com o sistema montado para "uso civil ordinário", tocar um negócio, fazer trabalhos escolares ou acadêmicos etc, o todo pode ser encarado como um _sistema operacional_ aderente com a 3ª definição apontando para a 4ª.

No Arch, a obtenção do mínimo conjunto de pacotes que define uma instalação básica se dá com a instalação do pacote `base`. Podemos [verificar a relação de componentes](https://archlinux.org/packages/core/any/base/) que define a instalação básica hoje, e são 28 no total:

| Pacotes                    | Finalidade                                      |
|----------------------------|-------------------------------------------------|
| licenses                   | Licenças de _software_                          |
| pacman archlinux-keyring   | Base para gestão de pacotes                     |
| filesystem                 | Arquivos fundamentais                           |
| glibc gcc-libs             | _libc_ e outras bibliotecas                     |
| bash                       | _Shell_                                         |
| coreutils                  | Utilitários fundamentais                        |
| findutils                  | Utilitários de localização de arquivos          |
| grep gawk sed              | Utilitários para processamento de texto         |
| gettext                    | Utilidades para i18n e l10n                     |
| file                       | Utilitário para inspeção de tipo de arquivo     |
| iproute2 iputils           | Utilitários para redes                          |
| tar xz gzip bzip2          | Utilitários para empacotar e compactar arquivos |
| shadow                     | Utilitários para gestão de usuários             |
| procps-ng psmisc           | Utilitários para gestão de processos            |
| systemd systemd-sysvcompat | _init_, mais uma realidade em si mesma          |
| pciutils                   | Utilitários para barramento PCI                 |
| util-linux                 | Utilitários para interação com o Linux          |
| linux (opcional)           | _Kernel_                                        |

Não abordaremos tudo o que é fornecido em uma instalação básica de Arch, mas alguns elementos serão abordados com algum detalhe nos próximos artigos.

## Considerações finais

Este artigo não foi de grandes emoções e experimentos, mas estabeleceu o ponto de partida sem o qual não seria possível continuar.

Em um próximo artigo será abordado o conceito de _software_ livre, que algumas vezes foi citado aqui. Além de significado histórico, esse conceito está na base de realidades que nos afetam até hoje, e esta série de artigos jamais seria escrita sem o _software_ livre. Além disso, traremos uma noção funcional sobre o licenciamento de _software_.

Para iniciar a visão de como o sistema operacional se articula e movimenta, trataremos do conceito de _boot_, _init_ e verificaremos um pouco da forma de interação homem-máquina que se dá através de _shell_.

Tendo a primeira visão de como se dá a interação com o sistema operacional, retomaremos o conceito de _pacotes_ no contexto da _gestão de pacotes_ para entender a idéia de _instalação_ de sistema operacional, e como podemos obter mais recursos para ampliar as possibilidades de utilização do sistema operacional.

Em seguida iniciaremos aquilo que é o objetivo da série: "preparação de GNUs", visando computadores de uso pessoal, ao contrário de servidores. Para tanto verificaremos as pilhas de áudio e de vídeo, como podemos constituí-las, composição de um ambiente gráfico de trabalho e os pormenores associados.

## Referências

 * W. Richard Stevens & Stephen A. Rago - Advanced Programming in the UNIX Environment
 * Brian W. Kernighan & Rob Pike - The Unix Programming Environment
 * Andrew S. Tanenbaum & Herbert Bos - Modern Operating Systems
 * Michael Kerrisk - The Linux Programming Interface - A Linux and UNIX System Programming Handbook

Ver também:
 * Paulo Kretcheu - [Curso GNU](https://www.youtube.com/playlist?list=PLuf64C8sPVT9L452PqdyYCNslctvCMs_n)
 * Blau Araújo - [_Shell Script_ Descomplicado](https://www.youtube.com/playlist?list=PLXoSGejyuQGpoHaptH9mbjNQi16gOLldE)
