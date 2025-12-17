+++
title = "Gerenciador de senhas Pass"
date = "2020-02-18"
tags = ["pass", "segurança", "senhas", "shell"]
featured = false
+++

Aspectos de segurança apresentam questões de alta complexidade, qualque andar é sobre ovos, e sobre toda bola há muitas mãos e dedos. Quando se trata de protocolos de segurança, um dos aspectos é relativo aos métodos de autenticação, onde um é relativamente popular, que chamaremos para os interesses desta pequena publicação de apresentação da ferramenta `pass`, baseada em senha, ou palavra passe, delimitando isso para o uso da palavra no ano de 2020, onde a palavra passe é definida arbitrariamente por uma parte, e em seguida armazenada (sem o formato em conta) de modo que para cada ação que exija autenticação, deve ser apresentada a palavra passe anteriormente definida com exatidão.

Tu chegas ao portão, e ao pronunciar a palavra passe, ele se abre para ti.

## Uma visão
Em segurança, todos os aspectos contam. Indo desde os programas em uso para as atividades, passando pelo sistema operaciona, o maquinário e assim por diante. Menos citados são aspéctos como processamento realizados em computadores externos não exatamente auditáveis, uma vez que a questão da criptografia homomórfica ainda está andando em ambientes mais restritos.

Esquecendo, temporariamente, de tudo o mais. Ao ligar um computador, tu podes ter uma senha para teu usuário no SO, uma senha para um eventual sistema de arquivos criptografado.

No uso da rede internacional de computadores, com diversos sítios oferecendo algo mediante um cadastro, algumas pessoas podem chegar a possuir uma quantidade brutal de cadastros, para interfaces de comunicação, comércios eletrônicos, serviços do Estado, plataformas educacionais e assim por diante.

## Os vários chaveiros
Chaveiro é o lugar de se guardar as chaves, e o chaveiro é o recurso de quem não tem memória (ou paciência) para lidar com dezenas de senhas que não sejam, ou todas iguais, ou do tipo _senha123_.

As senhas, chaves para abrir o acesso, podem ser armazenadas nos mais diversos tipos de chaveiros. As páginas no miolo de uma caderneta podem ser um chaveiro, ou um arquivo txt no seu dispositivo de armazanamento, exemplos simples.

O navegador se oferece também, ao menos os maiores e mais famosos, para o cargo de chaveiro, até que configures o contrário, inclusive em computadores públicos que serão utilizados por um outro após o término de seu uso.

Existem os programas dedicados para a tarefa de chaveiro, que tem toda uma questão arquivo de armazenamento de senhas criptografado, com algum tipo de interface para o usuário, e uma forma de chamar o chaveiro durante as tarefas que, em algum momento, vão exigir a inserção de senha, em alguns casos existe uma extensão para navegador que facilite o chamar do chaveiro.

Existem aqueles que operam em computadores de terceiro (se usarmos a palavra _nuvem_, vamos lembrar a possibilidade não dita de uma eventual _chuva_). O apelo é oferecer a possibilidade de ter acesso às senhas de qualquer dispositivo (neste mundo onde as pessoas querem usar _n_ dispositivos de computação), e para isso o consumidor tem de fornecer suas senhas de acesso. Será fornecido algum tipo de interface, talvez uma extensão para o navegador e assim por diante.

Existem ainda chaveiros improvisados, onde se usa de algum tipo de recurso para cifrar um arquivo contendo senhas, de modo que em toda necessidade o utilizador dirige-se ao _arquivo chaveiro_, usa a metodologia para ler o conteúdo cifrado, busca a senha desejada e assim por diante.

## Um chaveiro no estilo Unix: Pass
_Pass_ oferece uma forma de lidar com várias senhas fortes usando a abordagem Unix.

A peça constitui-se basicamente de um _script_ escrito em Bash (confiram o curso básico de programação em Bash, fornecido gratuitamente pela [comunidade debxp](https://debxp.org/cbpb). Além do Bash, são dependências:
  * `tree`: para mostrar de forma organizada a relação de senhas
  * `xclip`: para copiar uma senha não cifrada para a área de transferência
  * `gnupg`: todos os arquivos contendo senha são cifrados com chave assimétrica

São dependências opcionais, para extender as funcionalidades fornecidas por padrão:
  * `qrencode`: possibilita mostar um código QR para a senha não cifrada
  * `dmenu`: fornece uma interface para recuperar as senhas fora da linha de comando
  * `git`: permite criar um repositório com os arquivos de senhas (que são cifrados)

As dependências estão descritas tal qual no pacote `pass` do Arch Linux em 20-03-2020.

Outros recursos são utilizados, além dos supracitados, mas eles são presumidos em sistemas operacionais como Linux ou BSD, como os programas `rm`, `cp` e `mv`.

## Considerações que podem ajudar durante o uso
É bom ter uma idéia mínima de como se usa o `gpg`, pois será necessário criar um par de chaves para cada armazenamento de senhas desejado.

É interessante que os pares de chaves utilizados para o _Pass_ não sejam utilizados para qualquer outra coisa, e tenham a melhor senha da qual o utilizador consiga se lembrar, pois serão usadas para recuperar o conteúdo de cada arquivo de senha. Ao invés de lembrar-se de dezenas ou centenas de senhas, lembra-se de uma, ou bem poucas.

O _Pass_ fornece um gerador de senhas. A questão dos geradores de senhas pode ter toda aquela carga relacionada com os geradores de números randômicos, entropia _etc_, mas entre ter diversas senhas diferentes, com vários caracteres aleatórios de tipos diferentes, para cada acesso, e os expedientes a que se recorrem quando a memória não vence (famoso _senha123_).

Cada arquivo contendo senhas (e é interessante ter um arquivo para cada chave de acesso) é criptografado e colocado em `~/.password-store` ou no diretório definido pela variável de ambiente `PASSWORD_STORE_DIR`.

Falando em variáveis de ambiente, `PASSWORD_STORE_CLIP_TIME` define o tempo em segundos que a senha fica disponível na área de transferência (aquela de onde se recupera com `Ctrl-V`). Talvez 10 segundos seja suficiente para _colar_ sua senha na página em seu navegador de preferência.

O trabalho é feito em cima da árvore de diretórios dentro do diretório de trabalho, de modo que o comando `tree` mostra organizados todos os registros, conforme a organização dada pelo prório utilizador. Esse trabalho em cima de estrutura de diretório permite a definição de um par de chaves assimétricas diferente para um dado conjunto de senhas, o que pode ajudar, obviamente se houver um conjunto sensível de senhas que tu desejas proteger de forma separada.

_Pass_ é extensível, portanto suas funções básicas podem ser incorporadas num uso com outras ferramentas, como o próprio `dmenu`, e um atalho de teclado, de modo que se possa recuperar uma dada senha quando, por exemplo, eu atentico neste portal para poder escrever a presente publicação em meu navegador.

O seguinte _script_, que faz uso do `dmenu` é fornecido no pacote `pass` do Arch Linux no dia 20-03-2020:

```bash
#!/usr/bin/env bash

shopt -s nullglob globstar

typeit=1

prefix=${PASSWORD_STORE_DIR-~/.password-store}
password_files=( "$prefix"/**/*.gpg )
password_files=( "${password_files[@]#"$prefix"/}" )
password_files=( "${password_files[@]%.gpg}" )

password=$(printf '%s\n' "${password_files[@]}" | dmenu "$@")

[[ -n $password ]] || exit

if [[ $typeit -eq 0 ]]; then
    pass show -c "$password" 2>/dev/null
else
    pass show "$password" | { IFS= read -r pass; printf %s "$pass"; } |
        xdotool type --clearmodifiers --file -
fi
```

## Finalização
A leitura completa do manual (`man pass`) é _indispensável_ para o melhor uso da ferramenta. O manual fornece uma boa explicação acerca dos detalhes da linha de comando, os arquivos e variáveis de ambiente utilizadas, além de exemplos de uso da ferramenta com as respectivas saídas dos comandos, para que tu saibas o que esperar de cada ação.

_Pass_ é um bom exemplo de aplicação da abordagem Unix, onde não se _reinventa a roda quadrada_, mas se lança mão de recursos independentes, sem os assimilar ou criar uma dependência canhestra, que fornecem _interface de texto_, e assim é construída uma ferramenta com escopo definido, e de modo que pode ter suas funcionalidades incorporadas em outra peça para que o utilizador consiga extrair mais resultados.

Que seja um incentivo para estudar mais o _shell_, já que chaveiro apresentado neste capítulo é escrito em Bash, e uma forma de montar o próprio uso do sistema operacional guiado por um princípio de responsabilidade única, onde cada parte faz o seu trabalho.

## Leitura complementar

  * [man pass](https://git.zx2c4.com/password-store/about/)
  * [Página oficial do projeto](https://www.passwordstore.org/)
  * [Repositório do projeto](https://git.zx2c4.com/password-store/)
  * [Arabesque:GNU/Linux Crypto:Passwords](https://sanctum.geek.nz/arabesque/gnu-linux-crypto-passwords/)
  * [Arch Wiki:Pass](https://wiki.archlinux.org/index.php/Pass)
  * [Arch Wiki:Security:Passwords](https://wiki.archlinux.org/index.php/Security#Passwords)
  * [Les mille et une nuits](https://gallica.bnf.fr/ark:/12148/bpt6k5564679j/f299.image.r=sesame.langEN)
  * [Stanford:Gentry 2009:A Fully Homomorphic Encryption Scheme](https://crypto.stanford.edu/craig/craig-thesis.pdf)
  * [Homomorphic Encryption Security Standard](http://homomorphicencryption.org/wp-content/uploads/2018/11/HomomorphicEncryptionStandardv1.1.pdf)
  * [Lattigo](https://github.com/ldsec/lattigo)
  * [Arch Wiki:Password managers](https://wiki.archlinux.org/index.php/List_of_applications/Security#Password_managers)
  * [Canal Kai Hendry:Gerenciamento de senhas com criptografia do Vim](https://www.youtube.com/watch?v=WFcdan1UD-0)

## Sátiras

Aqui serão referenciadas algumas, pensadas mais para os aspéctos familiares aos implementadores de código e elaboradores de programa de computador, mas que podem ser base para algumas analogias em outros níveis:

  * [Desciplopedia:Padrões de projeto:Mega Zord](https://desciclopedia.org/wiki/Gambi_Design_Patterns#Mega_Zord)
  * [Desciplopédia:Padrões de projeto:Reinventando a roda quadrada](https://desciclopedia.org/wiki/Gambi_Design_Patterns#Reinvented_Square_Wheel_Helper)
