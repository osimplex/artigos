---
title: "A arte da preparação de GNUs (II) - Liberdade e Licenciamento"
date: "2026-08-31"
palavras-chave: ["preparação de GNUs", "licenciamento", "computacao"]
ano: ["2026"]
featured: false
---

Este é o segundo artigo de uma série dedicada à arte de preparar um sistema operacional _Unix-like_ para uso pessoal, com ênfase no _GNU/Linux_. Neste artigo se tratará de algo frequentemente relegado a um segundo plano, muitas vezes apresentado sem o devido cuidado, divulgando-se enormes erros em nome de "simplificar" a matéria. O assunto hoje é liberdade de _software_ e licenciamento.

## Introdução

Todo homem nasce, cresce, desenvolve-se e morre em sociedade. Neste pequeno ensaio de filosofia de mesa de bar será apresentado um modelo. 

Este artigo tomará como centro a base que viabilizou, em última instância, até a existência desta série de artigos que escrevo: o conceito de _software_ livre, cunhado na década de 1980 por uma figura bastante pitoresca na história da computação, Richard Stallman. Este conceito, e a subsequente prática, pode-se dizer sem medo, é uma das bases da nossa história recente enquanto "esfera de influência ocidental", para usar uma categoria da não tão finada Guerra Fria. 

Se o leitor imagina que aqui será o artigo de "política" contaminando assunto "técnico", digo que está certo e errado. É e não é o que o leitor imagina. Primeiro porque aqui se tratará de _política_, não de "política", segundo porque o artigo continuará num domínio técnico, uma vez que o domínio técnico não acaba onde encerram os algarismos.

## O vínculo

Já foi citada a situação do homem em sociedade. Mas detalhemos um aspecto esquecido dessa situação começando com a pergunta: qual a menor unidade em uma sociedade humana? Respondendo de forma simplista, proponho que temos dois caminhos:

 1. O critério da menor unidade por limite de divisão nos leva a uma resposta recorrente, que alguns até apontam como "óbvia": o próprio homem, no sentido de ser humano, ou "o indivíduo" como alguns preferem dizer.
 2. O critério da menor unidade por capacidade de reprodução nos aponta para uma limitação do limite de divisão, porque um homem de modo algum gerará outro homem.

Detalhando o segundo critério, como organismo biológico o homem é incapaz de reproduzir-se por divisão, como uma estrela-do-mar ou as próprias células de que é composto. Não fazemos mitose para nos reproduzir, fazemos sexo, ou reprodução sexuada, o que demanda dois provedores de gametas compatíveis para funcionar. Mais ainda, após o nascimento temos um indivíduo que passará bastantes anos dependente nas necessidades mais básicas de outros indivíduos, e em última instância pode-se de dizer que isso jamais mudará, ainda que essa dependência mude no curso da vida e apareçam as contrapartidas.

Feita essa exposição, não pretendo fixar a menor unidade pelo segundo critério, mas ela pode ser compreendida na mesma abordagem com a qual articulamos o conceito de sistema operacional no artigo anterior. A menor unidade pode ser uma "família nuclear", necessariamente pai com mãe e filhos, avançando para uma "família expandida" que inclui os parentes, uma comunidade ou um clã e assim por diante. Pode parecer indevido avançar além da "família nuclear", mas um índio que foi expulso da tribo e morreu no mato não acharia tão indevido assim.

Repetindo: nós nascemos, crescemos, nos desenvolvemos e morremos em sociedade. E nessa condição de viver em sociedade surge a realidade do vínculo, que é a passagem do concreto para o abstrato.

## O vínculo e o direito

Do vínculo nasce o direito.

Quando dois indivíduos, na menor unidade pelo primeiro critério, estabelecem relação, aí aparece a dimensão do direito. Pode não ser textualmente codificado, mas ao verificar que o vínculo implica a expectativa, que podemos entender nos termos de _direitos_ e _obrigações_ (algo que um espera e outro tem que cumprir), percebe-se a dimensão jurídica do vínculo. Diremos que direito e justiça não é algo de instituições e profissionais específicos, mas uma realidade próxima de nós, ainda que também exista em nossa sociedade o direito institucionalizado.

Com outra linguagem: _o direito é uma função do vínculo_, ou uma estrutura abstrata de articulação entre indivíduos.

## O direito e a liberdade

Do direito nasce a liberdade.

Que não se interprete esta etapa como uma defesa do direito positivo em contraposição ao _jus_ naturalismo. Explicando bem grosseiramente ao leitor que não conhece a querela, uma linha dirá que o direito é produzido, ou "positivado", por um contexto social de um dado tempo e lugar simplesmente, e outra que há direito de validade universal que procede da _natureza_, e aqui muito cuidado com o conceito de natureza.

Nesta etapa do raciocínio proponho, apontando para outra querela, sobre idéia de _liberdade_, um conceito de liberdade com inspiração material. Tomando emprestado o que vemos em projetos de estruturas mecânicas, o conceito de "graus de liberdade", verificamos com clareza que _liberdade é necessariamente uma função de estrutura_. E, ao especificar graus de liberdade viabilizados por uma determinada articulação, expressamos uma totalidade do possível nesta estrutura.

Transpondo esse pensamento para nosso raciocínio, se o direito é a estrutura de articulação entre indivíduos, não convém tratar de liberdade em termos de "poder fazer" ou "não ser impedido de fazer", mas da função de estrutura. Porque não verificamos liberdade em um braço mecânico ligado a estrutura nenhuma, mas, se está dada uma estrutura articulada, então podemos verificar os graus de liberdade; também deste modo entenderemos a liberdade humana em sociedade. Daí pode-se dizer que estabelecer e limitar a liberdade é um só e o _mesmo_ ato, ou essencialmente a mesma coisa.

## O direito e a expressão humana

O _software_, que é objeto central desta nossa série, faz parte de uma categoria mais ampla, que é a da expressão humana. Ao menos juridicamente. Nós homens podemos nos expressar através da literatura, da música, das artes plásticas e do _software_ também. O que exige que façamos algumas distinções.

### Contra as efabulações

Iniciemos da seguinte forma: _propriedade intelectual não existe_, é uma fábula, um pseudoconceito. Porém deve-se reconhecer que existe direito de patentes, marcas além do direito autoral e outras situações como topologia de circuito integrado, cultivares etc. Afirmo isso porque a categoria _propriedade intelectual_, ainda que seja recorrente no _status quo_, na doutrina, é um contrassenso, e mistura realidades jurídicas bem distintas por uma característica que não é compartilhada.

Para começarmos a construir algum entendimento, convém afastar o pseudoconceito e entender os verdadeiros conceitos.

### Patentes e marcas

Para dar uma boa noção, comparemos patentes e marcas. 

As patentes foram criadas como uma realidade de contrato social que visa evitar a perda social de desenvolvimento _industrial_ solucionando uma tensão. Todo desenvolvimento tecnológico, como um motor elétrico, se dá sobre o conhecimento social e é levado a cabo por um indivíduo ou conjunto de indivíduos organizados, do que decorre dois interesses legítimos e concorrentes: o interesse de quem realizou o esforço para chegar ao resultado de desenvolvimento, e o da sociedade em geral de incorporar este resultado ao seu patrimônio comum de modo que todos possam aproveitá-lo. Essa passagem do individual ao social necessita de uma etapa de compartilhamento, só que existe um incentivo mercantil para que isso não se dê: exclusividade na exploração comercial. Daí temos o segredo industrial. E uma característica do segredo é que ele pode se perder.

Para incentivar a etapa do compartilhamento de invenções surgiram as patentes. Podemos entender que patente é uma concessão social, ou um título outorgado, de exclusividade na exploração comercial de um resultado de desenvolvimento tecnológico _por período limitado_ que se dá em troca da divulgação deste resultado. Desta forma a sociedade pode incorporar o desenvolvimento e quem desenvolveu obtém uma contrapartida. Temos ainda primos da invenção como o modelo de utilidade e o registro de desenho industrial.

Marcas, por outro lado, são sinais distintivos. Produtos semelhantes são diferenciados pelas _marcas_ que levam, além de suas propriedades. Então aí temos questões de construção de reputação, além de identidade. O direito de marca concede ao detentor exclusividade no uso, de modo que não se confundam seus respectivos produtos, serviços e suas origens.

O gênero mais apropriado para toda a categoria, no meu ver, seria a de bens intangíveis, como se observa num plano de contas em contabilidade, mais especificamente entre as chamadas contas devedoras. A questão não é intelecto, especialmente em direito de marcas, que cobre registros puramente nominativos inclusive, para proteger nomes comerciais que podem não ser originados por grandes investimentos intelectuais, mas o direito de exploração exclusiva de uma realidade imaterial. Daí podemos conceder, no limite, o termo propriedade imaterial, mas sempre retornando às respectivas filosofias de cada direito da categoria, distintas das demais realidades no escopo do _direito das coisas_, bens materiais móveis ou imóveis.

### Direito autoral

O direito autoral é o direito em que se enquadra o _software_.

A questão do direito autoral tem dimensões. A humanidade compõe música, pinta, conta histórias desde que provavelmente o período das cavernas, e embalados pelas diversas formas de expressão nos embalamos enquanto sociedade. E nessa história houve miríades de miríades de criações cujos autores sequer sabemos quem são, seja porque sequer chegou um nome, seja por causa do que chamamos pseudoepígrafe, uma falsa atribuição de autoria. Mas recebemos e transmitimos, fenômeno que chamamos _tradição_, para que continuemos enquanto sociedade.

Mas o direito autoral não nasceu visando esse aspecto da realidade humana. Ou melhor dizendo, o _direito de cópia_, ou _copyright_, como dizem os gringos. Este direito começou, por causa da invenção da imprensa, como uma forma de restringir que literatura poderia ser impressa. Isto é: se tratava de uma regulação do uso das "plataformas de difusão" da época. O _copyright_ se estabeleceu em princípio com a incorporação da Guilda dos Impressores pela Coroa inglesa no ano de 1557, em que se estabelecia um monopólio da _prensa_, ou do direito de copiar, sob a responsabilidade de não imprimir textos _condenáveis_.

Em 1662 se estabeleceu em lei que toda publicação deveria ser registrada junto à Guilda dos Impressores, de modo que ali se dava a permissão para copiar. Passadas algumas décadas, houve um outro movimento para estabelecer os direitos de cópia de outro modo, culminando no Estatuto da Rainha Ana em 1710, que se apresentava como "uma lei para encorajamento do aprendizado". Aí começou a história do "detentor dos direitos", pois o direito de cópia que anteriormente pertencia à Guilda passou a ser concedido, em primeiro, ao autor após o processo de registro e depósito de cópias da obra, e então este direito de cópia poderia ser _licenciado_, concedido a terceiros. E este direito incluía o controle sobre a impressão e reimpressão de livros.

A história conta com outros tantos capítulos, e complexidades. Num dado momento a noção do _copyright_ passou a incluir música com o surgimento da possibilidade de gravar e reproduzir sons, e da indústria fonográfica. Hoje o escopo do _copyright_ alcança diversas formas de expressão humana, e isso inclui o _software_.

Outro desenvolvimento importante foi o estabelecimento nos EUA da Lei de Direitos de Cópia de 1976, em que criação e "fixação" da obra foram estabelecidas como _fato gerador_ do direito, ao invés da _publicação_, mesmo que não haja uma notificação de direitos, ou _copyright notice_. Exemplificando, se este artigo for finalizado em papel impresso, mesmo sem divulgá-lo ou escrever "todos os direitos reservados", desde momento em que estiver finalizado vigorará o meu direito de autor, ou de cópia. Isso depois terá um entendimento atualizado para um contexto com computação e _Internet_ de banda larga.

Em território brasileiro vigora a Lei 9.610/1998, na qual podemos verificar o Art. 7: 

> São obras intelectuais protegidas as criações do espírito, expressas por qualquer meio ou fixadas em qualquer suporte, tangível ou intangível, conhecido ou que se invente no futuro [...].

Para alinhar o entendimento usaremos a seguinte definição: 

> Direito autoral é aquele em virtude do qual o autor de uma obra literária, científica ou artística tem o direito de vincular o seu nome à sua produção, reproduzindo ou transmitindo a obra com exclusividade. Deste modo observam-se dois aspectos: a) o direito moral do autor, de ter o nome vinculado à obra, de tê-la como sua, sem modificações e deturpações; b) o elemento econômico, fundamento da propriedade imaterial, consistente no direito de explorar comercialmente a obra, representá-la, reproduzi-la, cedê-la e imprimi-la.

### Software como expressão humana

Então o _software_ e a programação, que é a atividade humana pela qual se produz o _software_.

Podemos entender a programação de algumas formas, e a abordagem mais tradicional iniciará a incursão pelo conceito de _algoritmo_, que não é uma entidade mística como faz-se parecer nas conversas leigas e tópicos jornalísticos, mas, pela escolha de palavras mais comum, "uma sequência de etapas para resolução de um problema". Esta apresentação aponta para a dimensão científica da atividade de elaborar um programa de computador.

Pessoalmente, como alguém que já escreveu um e outro programa de computador, encaro a tarefa de programação como um esforço narrativo, de escritor mesmo, com a diferença de que minha história terá força de realidade e constituirá um tipo de [_máquina sutil_]({{< relref "251201-inimiga-da-pratica#o-milagre-da-ciência-e-da-computação" >}}) partindo da _palavra_. Porque toda palavra num programa de computador é roda e alavanca, elemento funcional, considero que "programa de computador é uma história de como as coisas acontecerão". Pode parecer forçado para o leitor, mas também percebo um aspecto literário no processo de redação de _software_.

Seja como for, no _software_ depositamos algo de nós, não diria que "o conhecimento", como alguns dizem, mas realmente algo de nós. Assimilamos uma realidade, consideramos a demanda de um outro ou de nós mesmos, e dessa reflexão emerge uma articulação ideal. Então nos esforçamos para traduzir em palavras o que temos em mente, estabelecendo uma simulação de algo de nossa dinâmica interior, um pensamento artificial.

Em sociedade, essa produção humana está, com algumas particularidades que derivam da natureza da coisa, sob o guarda-chuva do direito autoral.

## _Hacker_ e cultura _hacker_

Este artigo é escrito em 2026, o lastro do tema vai até a década de 1950. Um resumo interessante em língua portuguesa temos na dissertação de mestrado em história social de Aracele Torres do ano de 2013, "A Tecnoutopia do Software Livre: Uma história do projeto técnico e político do GNU", na segunda seção do primeiro capítulo de título: "A invenção de uma cultura _hacker_". Também vale a leitura do livro "_Hackers: Heroes of the Computer Revolution_" de Steven Levy.

Sem pretender muito neste resumo do resumo do resumo, trarei a proposta de entendimento do termo _hacker_ apresentada por _Richard Stallman_, uma personagem célebre na história _hacker_: 

> É difícil escrever uma definição simples de algo tão variado como _hackear_, mas penso que estas atividades têm em comum o deleite, a engenhosidade e a exploração. Deste modo, _hackear_ significa explorar os limites do possível com espírito de engenho e deleite. Atividades que demonstram um deleite no engenho têm "valor _hacker_". (tradução nossa)  
>  
> -- [_On Hacking_](https://web.archive.org/web/20260826084937/https://stallman.org/articles/on-hacking.html). Richard Stallman

No artigo do qual foi extraída a citação acima ainda há desenvolvimentos adicionais relevantes. O _hack_ para Stallman não é algo necessariamente útil, nem necessariamente inútil. Mais ainda, não é necessariamente algo relacionado com computação, e ele cita nominalmente uma peça musical de Guillaume de Machaut, "_Ma Fin Est Mon Commencement_". Em outra ocasião ele referencia uma peça de J. S. Bach (provavelmente o [Canon do Caranguejo](https://www.youtube.com/watch?v=miGuET40U7I), que pode ser tocado de trás para frente). Dois exemplos de _hack_ na música.

Do livro de Steven Levy também podemos extrair algumas sentenças, preceitos que se tomaram para apresentar o que poderia ser uma "ética _hacker_":

> Acesso a computadores --- e qualquer coisa que poderia ensinar algo sobre como o mundo funciona --- deveria ser ilimitado e total.  
> Toda informação deve ser livre.  
> Desconfie da autoridade --- Promova a decentralização.  
> _Hackers_ devem ser julgados por seus _hacks_, não falsos critérios como escolaridade, idade, raça ou posição social.  
> Você pode criar arte e beleza em um computador.  
> Computadores podem mudar a sua vida para melhor. (tradução nossa)  
>  
> -- Hackers: Heroes of the Computer Revolution. Steven Levy, 2010

Sem desenvolver muito as citações, e recomendando a leitura das fontes, chamo a atenção para duas realidades que podemos perceber:

1. Desviando um pouco do assunto deste artigo, esse espírito _hacker_ me parece algo que faz parte em alguma medida da nossa "alma nacional", se é que se pode dizer que isso existe. Por um lado, claramente temos uma estrutura de julgamento social baseada nos tais "falsos critérios", mas não podemos desconsiderar o elemento da _gambiarra_, de buscar a solução "tirando leite de pedra", além de outras coisas.
2. O modo de ser associado ao ideal _hacker_ apresenta uma _tensão_ com a questão do direito autoral, não necessariamente uma contradição. E veremos adiante frutos do desenvolvimento desta tensão.

## _Hack_ legal

### Estado de "natureza"

Uma obra autoral "nasce" com _todos os direitos reservados_. É a realidade do direito desde a "fixação" [discutida anteriormente]({{< relref "260831-gnus-preparados-p2.md#direito-autoral" >}}). Um texto, uma gravação musical, imagem, _software_ etc que esteja desprovido de algum termo que estabeleça o contrário não pode ser reproduzido, distribuído, comercializado (entre outras coisas) por terceiros. Esta é a realidade do direito desde a "fixação" da obra. Este é o _estado de "natureza"_.

Estabelecido o direito, há o detentor do direito. E o detentor dos direitos sobre a obra pode permitir que terceiros se beneficiem da obra, e uma das formas de realizar isso é por meio de um instrumento chamado _licença_. Para pôr em termos simples, a licença é uma espécie de contrato em que o detentor dos direitos, ou _copyright holder_ em gringuês, concede direitos a um terceiro. 

Sobre essa base pode haver comércio. Um autor de livro licencia mediante pagamento a obra para que uma editora o imprima e distribua, um cantor licencia a execução de uma música para uma gravadora, um programador licencia seu _software_ para um cliente. Quem já instalou o _Windows_ (além de outros _softwares_ privativos) já teve a oportunidade de não ler e "aceitar" o contrato EULA (_End-User License Agreement_), o licenciamento do _Windows_.

Neste licenciamento o detentor dos direitos, dependendo da posição de negociação, tem poder de estipular quais direitos concede ou não concede para quem licencia a obra, além da compensação por isso, naturalmente.

Essa mecânica não se demonstra como uma dinâmica de "promoção do aprendizado", como poderia fazer parecer o ancestral comentado, Estatuto da Rainha Ana, mas de promoção do comércio.

E da comentada tensão dialética entre cultura _hacker_, com destaque para o acesso livre, o espírito de experimentação, e direito autoral emergiu uma síntese, um "_hack_ legal"

### _Software_ livre

A definição de _software_ livre é mantida pela [_Free Software Foundation_ (FSF)](https://www.gnu.org/philosophy/free-sw.html).

Segundo a FSF, um programa de computador é _software_ livre se o utilizador deste programa (individualmente este que escreve, meu leitor ou qualquer outra pessoa) goza de _quatro liberdades essenciais_:

0. A liberdade de executar este programa conforme o desejado para qualquer propósito.
1. A liberdade de estudar como este programa funciona, e modificá-lo de modo que satisfaça as respectivas demandas de computação.
   - Acesso ao código fonte é uma pré-condição para isto.
2. A liberdade de redistribuir cópias, de modo que se possa ajudar ao próximo.
3. A liberdade de distribuir cópias das próprias versões modificadas para terceiros.
   - Fazendo assim dá-se à toda comunidade a chance de beneficiar-se com estas modificações.
   - Acesso ao código fonte é uma pré-condição para isto.

Um _software_ é livre se concede aos utilizadores adequadamente todas as liberdades enumeradas. De outro modo, é _não-livre_.

Mas aqui neste artigo apresentaremos uma outra definição complementar, de autoria do Prof. Paulo Kretcheu:

> _Software_ livre é todo _software_ cuja licença seja aceita pela _Free Software Foundation_ como respeitante às quatro liberdades essenciais que definem o _software_ livre.

O sistema do direito autoral é a base para o cerceamento de liberdades do utilizador. Já comentamos, o estado de "natureza" da obra autoral é _todos os direitos reservados_, e pelo licenciamento se dá o negócio do _software_ privativo, em que se concede a mínima possibilidade necessária ao _usuário_ para que a obra cumpra alguma função, estabelecendo uma relação de dependência entre fornecedor e _usuário_. E a operacionalização do _software_ livre, isto é, a concessão de liberdades, se dá na mesma base do direito autoral; este é o _hack_.

Recordando o que apresentamos sobre [o direito e a liberdade]({{< relref "260831-gnus-preparados-p2#o-direito-e-a-liberdade" >}}), a liberdade, como função de estrutura, se dá em função do licenciamento. Daí, ao obter uma obra sob licença livre, gozamos de plenitude de liberdade de _software_. Mas a matéria legal não é simples de lidar, então temos uma mediação da própria FSF, que analisou e analisa o texto de diversas licenças de _software_ para verificar se concedem adequadamente as liberdades, além de propor licenças próprias como a família GPL.

Podemos verificar uma lista de licenças comentada pela FSF [aqui](https://www.gnu.org/licenses/license-list.html).

### Projeto GNU

O _hack_ jurídico do _software_ livre tem uma iniciativa irmã, o chamado [Projeto GNU](https://www.gnu.org).

O projeto consistia, e consiste, num [sistema operacional _Unix-like_]({{< relref "260507-gnus-preparados-p1#o-paradigma-de-sistema-operacional" >}}) 100% livre, ou que respeita a liberdade dos utilizadores. Esta questão do respeito às liberdades, especificamente às quatro liberdades essenciais _do utilizador_, que já foram apresentadas, é o ponto nevrálgico. Tudo é feito sob esta premissa que é materialmente estabelecida na forma do direito aplicando o licenciamento GPL.

GNU é um acrônimo recursivo que significa "_GNU's not Unix_" (GNU não é Unix). Unix era um sistema operacional não-livre.

O projeto com o tempo ganhou corpo e cresceu. Até o ponto em que se tinham prontos todos os elementos de um sistema operacional _Unix-like_ com exceção de um componente, o _kernel_. Em certa altura surgiu o _kernel_ Linux, que num primeiro momento não estava disponível sob uma licença de _software_ livre, mas quando foi relicenciado passou a ser frequentemente associado aos componentes GNU, produzindo o que ficou conhecido como GNU/Linux.

Mas é de se notar que a fórmula do projeto GNU e do licenciamento livre se demonstrou efetiva não só no sentido de cumprir os objetivos de produzir um sistema operacional dentro do espírito da cultura _hacker_, e que servisse de plataforma para esta através da instituição legal do respeito às liberdades dos utilizadores, mas de fazer isso com qualidade, gerando um _modo de produção_.

A soma do sistema operacional GNU com o _kernel_ Linux também teve seus reflexos. Um muito notável era o trabalho de junção destes componentes, além de _softwares_ produzidos por terceiros, para compor um todo. Este tipo de trabalho começou a ser conduzido por indivíduos e comunidades, produzindo o que chamamos de _distribuições_, sendo duas das mais antigas o Debian e o Slackware.

### Código _aberto_

A definição de código aberto é mantida pela [_Open Source Initiative_ (OSI)](https://opensource.org/osd).

A OSI surgiu visando promover o _modo de produção_ que se desenvolveu como efeito das iniciativas do _software_ livre e do projeto GNU. Como se pode ler na [missão da OSI](https://opensource.org/about), "[O] código aberto possibilita um método de desenvolvimento que canaliza o poder da revisão de pares distribuída e da transparência do processo. A promessa do código aberto é maior qualidade, melhor confiabilidade, maior flexibilidade, custos menores e um fim para o aprisionamento tecnológico (_vendor lock-in_) predatório." O foco é deslocado, portanto, da liberdade do utilizador para o critério de utilidade do modo de produção viabilizado pelo modelo de licenciamento. É uma proposta de eficiência no uso dos recursos corporativos que se dá pela aceitação de um nível de colaboração.

Sendo a base do modo de produção o modelo de licenciamento, a OSI propõe dez pontos que definem se uma licença é de código aberto. Esses dez pontos foram _inspirados_ (para não dizer que foram praticamente copiados) de uma definição anterior que surgiu no projeto Debian.

O Debian surgiu com o intuito de distribuir um sistema operacional 100% livre, e para estabelecer um alinhamento entre os participantes dessa iniciativa comunitária foi elaborado o [Contrato Social do Debian](https://www.debian.org/social_contract), cuja primeira versão foi ratificada em 1997, um ano antes da fundação da OSI em 1998. Parte deste contrato é a chamada DFSG, _Debian Free Software Guidelines_ (diretrizes de _software_ livre do Debian), que é uma lista de dez pontos _bem semelhante_ à definição de código aberto, que estabelece os critérios do que será considerado _software_ livre no Debian, e portanto o que será distribuído.

Para fazer o arremate, trazemos outra vez a fórmula do Prof. Paulo Kretcheu:

> De código aberto é todo _software_ cuja licença seja aceita pela _Open Source Initiative_ como respeitante aos dez critérios que definem um _software_ como de código aberto.

E temos a mesma situação de mediação, a OSI faz análise de licenças e disponibiliza uma relação de licenças aprovadas [aqui](https://opensource.org/licenses).

### Intersecção e disjunção

A OSI como a FSF trabalham com base em um modelo de licenciamento mais ou menos semelhante, apesar de visões e propósitos completamente distintos. Isto produz uma situação que é a seguinte: em sentido estrito, uma boa fatia dos _softwares_ livres são de código aberto e vice-versa, por estarem disponíveis sob licença aceita por ambas as organizações.

Existem licenças aceitas pela FSF que são rejeitadas pela OSI, e o contrário também. A _Linux Foundation_ mantém uma lista atualizada com diversas licenças e seus respectivos status de aprovação na FSF e na OSI, que pode ser consultada [aqui](https://spdx.org/licenses).

E também existem peças e situações rejeitadas por ambas. Uma delas é a situação do chamado _código disponível_, ou código _meramente_ disponível. _Source available_ em gringuês. Exemplificando, imagine um repositório de código fonte acessível via _Internet_, sem licença porém. Já foi discutido o [estado de "natureza"]({{< relref "260831-gnus-preparados-p2#estado-de-natureza" >}}) da obra autoral, código disponível desta maneira não é livre e nem aberto. Para que seja considerado livre e/ou aberto é necessário o licenciamento sob algo que seja aceito pela FSF e/ou OSI. É uma situação recorrente, porém, ouvir dizer que esse tipo de peça se trata de "código aberto" pelo fato do acesso estar aberto, mas isso não está correto.

Podendo dizer uma coisa ou outra, falar do _software_ livre e do código aberto, é justo aplicar os termos com consciência e fidelidade às linhas de pensamento. Pessoalmente, dificilmente vou classificar uma peça de _software_ que use e aprecie como de código aberto, coincidindo a aceitação de licenciamento aplicado, porque o que me importa não é vender um modo de produção no seguimento corporativo, mas as liberdades, a relação de respeito, a cultura _hacker_. Mas não é como se isso fosse uma _filosofia_ (em sentido bem amplo) e a posição da OSI não fosse, bem pelo contrário, porque o pragmatismo também é uma posição, ou uma escola, filosófica. Cada qual com os próprios valores.

## Tópicos

### _Creative Commons_

Em outro campo do direito autoral, temos a [família de licenças](https://creativecommons.org/cc-licenses) _Creative Commons_ (CC) que foi elaborada visando outras obras autorais como literatura e música. São licenças elaboradas para que o público geral, sem disposição ou capacidade de contratar um advogado, pudesse disponibilizar suas criações sob um licenciamento que julgue conveniente e que ofereça mais àquele que recebe a obra em comparação com o [estado de "natureza"]({{< relref "260831-gnus-preparados-p2#estado-de-natureza" >}}), com [destaque](https://creativecommons.org/public-domain/freeworks) para o que se define como [obra cultural livre](https://freedomdefined.org/Definition), um análogo do _software_ livre.

A família de licenças CC contempla diversas possibilidades baseadas em quatro atributos:

 * BY --- Dar créditos ao autor original ao distribuir a obra original ou derivada;
 * SA --- Distribuir trabalhos derivados sob a mesma licença, dispositivo de _copyleft_;
 * ND --- É vedada a produção de obras derivadas e adaptações;
 * NC --- É permitido somente uso não comercial da obra;

Temos seis licenças CC com diferentes configurações destes atributos, mais a CC0, que é um instrumento de _dedicação ao Domínio Público_ (ato de abrir mão dos direitos autorais) ou licenciamento sob condição similar para jurisdições em que não é possível tal dedicação. As seis licenças podem ser consultadas [aqui](https://creativecommons.org/cc-licenses).

Cabe notar que nem todas as licenças são aderentes à definição de obra cultural livre, somente duas: CC-BY e CC-BY-SA, além da CC0. As quatro restantes deixam de estabelecer alguma liberdade por força dos atributos ND e/ou NC.

Para dar um exemplo, CC-BY-SA 4.0 é a licença sob a qual estão todos os artigos deste portal.

### _Copyleft_, _software_ livre e código aberto

O _copyleft_ é uma espécie de dispositivo que podemos encontrar nas licenças de obras autorais, e consiste numa obrigação a ser observada na distribuição de uma modificação da obra licenciada, a obrigação de distribuir esta modificação sob a mesma licença da obra original. A finalidade do _copyleft_ é a manutenção de intenções do detentor de direitos de uma obra originária no ato de licenciamento. Como isso se dá, e os critérios específicos que geram tal obrigação, variam de licença para licença.

Essa espécie de dispositivo é possível porque as definições do _software_ livre e de código aberto, ambas, admitem ser aceitável estabelecer certas obrigações acerca de como um _software_ pode ser distribuído desde que não contradigam:

 * Alguma das _quatro liberdades_ postuladas pela FSF para licenças de _software_ livre
 * Algum dos _dez critérios_ postulados pela OSI para licenças de código aberto

Exposto isso, clarificamos uma confusão com a qual já tive contato: dizer que uma peça sob licença com _copyleft_ é _ipso facto_ _software_ livre, mas não código aberto. Se trata de um equívoco, pois existem licenças com _copyleft_ aceitas tanto pela FSF como pela OSI, como a GPLv3 e a CDDLv1. O dispositivo _copyleft_ não faz parte nem dos critérios da OSI e nem é requisito para as liberdades tal qual postuladas pela FSF.

### _Copyleft_ é "menos livre"

Essa é uma querela no ramo do _software_ livre e do código aberto.

Já foi exposto que o _copyleft_ não é um requisito e nem um impedimento nas definições da FSF e da OSI. E assim sendo, existem licenças que contam e que não contam com esta espécie de dispositivo, sendo as licenças com _copyleft_ mais famosas as produzidas pela FSF, em especial a GPL (hoje na versão 3).

Entre as licenças livres que há temos um gênero conhecido como de licenças "permissivas", mas podemos chamar de minimalistas. São licenças de construção bem sucinta, que estabelecem as liberdades, poucas ou nenhuma obrigação, apresentam o aviso de garantia, estabelecem a limitação de responsabilidade com o mínimo de palavras necessário. Não contam com _copyleft_. Os exemplos mais famosos seriam ISC, BSD de duas ou três cláusulas, Expat (também conhecida como MIT).

Certa vez ouvi uma pessoa da comunidade dos BSDs comentar que preferia esse tipo de licença pelo minimalismo. Sendo um texto mais simples de ler, sem muita carga de _juriquês_, oferecia o que ele buscava: um meio de simplesmente oferecer a obra dele como um presente para a humanidade, e "livrar a barra" (aviso de garantia e limitação de responsabilidade).

Por outro lado, alguns polemistas, fomentando tensão, opõem licenças com _copyleft_ às "permissivas" preterindo as primeiras. Entre os recursos retóricos que surgem está o de apresentar as licenças com _copyleft_ como se fossem "menos livres", "restritivas", "tirassem" alguma liberdade.

Não vou propor que a cláusula _copyleft_ é uma bala de prata, e que deve ser aplicada em todo e qualquer licenciamento. Mas a primeira questão para a retórica da detração é: como isso pode se dar? Sabemos que toda obra autoral ["nasce"]({{< relref "260831-gnus-preparados-p2#estado-de-natureza" >}}) não-livre, e que o _copyleft_ não é critério para definir se algo é ou não é livre, e nem de código aberto. Antes, qualquer licença _livre_, contando com _copyleft_ ou não, estabelece plenamente a liberdade de _software_.

Também sabemos que podem ser estabelecidas obrigações associadas à distribuição do _software_, de modo que não há restrição alguma, mas _condição_. E _copyleft_ sequer é o único exemplo disso. Na GPLv3 podemos verificar a obrigação de oferecer ao recipiente de um _software_ distribuído na forma de código-objeto (executável compilado) um meio de obter o respectivo código fonte, a obrigação de fornecer todos os avisos de licenciamento e a licença ao recipiente de uma cópia verbatim do código fonte. Mais ainda, em licenças ditas "permissivas" frequentemente encontramos obrigações associadas à distribuição, como a reprodução do aviso de direitos autorais, das cláusulas, aviso de garantia e limitação de responsabilidade que verificamos na BSD de três cláusulas (essa é uma de três, naturalmente). Se o mero estabelecimento de obrigações acessórias fosse problema, constituísse "restrição às liberdades", teria de se admitir que isso afeta licenças minimalistas também.

Retomando o que [já discutimos]({{< relref "260831-gnus-preparados-p2#o-direito-e-a-liberdade" >}}), liberdade é uma função de estrutura. E nos termos desta discussão, toda estrutura de licenciamento define liberdade. Podendo aderir ou não à definição da FSF de _software_ livre, ou de obra cultural livre. De todo modo, sem critérios não se determina quais os nossos requisitos em termos de _graus de liberdade_. E, dentro dos critérios que apresentamos no decorrer do artigo, não cabe ser detrator do _copyleft_.

Mas saindo da retórica dos detratores, há uma motivação material. Eles não aderem a um licenciamento de _software_ livre, ou de código aberto, que requer a manutenção de intenção do autor original nas obras derivadas, a intenção de que haja liberdade de _software_ para todos os que tiverem contato com a obra (pensamento ligado ao _software_ livre), ou a preservação das condições do modo de produção do _software_ (pensamento ligado ao código aberto).

Sem uma obrigação de _copyleft_ é possível (também) se apropriar da obra de um terceiro, construir sobre ela, e distribuir este trabalho derivado sem conceder aos usuários as mesmas liberdades que possibilitaram a construção desta obra derivada. É por isso que a retórica dirá que isso torna o licenciamento "menos livre" e desfavorece "o desenvolvedor", faltando dizer qual.

Adicionando cores à questão do desenvolvedor, essa redistribuição com restrições à liberdade dos usuários é defendida em nome dos negócios. Mas em nome dos negócios também podemos ir em favor das obrigações associadas às liberdades, e aqui cito o caso do Redis. Um banco de dados NoSQL bastante interessante, _software_ livre e de código aberto do ponto de vista de licenciamento, BSD, que acabou tornado não-livre, relicenciado sob SSPL, pela empresa detentora dos direitos para enfrentar o parasitismo comercial de outras empresas, fornecedoras de computação "em nuvem" (vulgo servidores próprios), que ofereciam a inovação sem qualquer contrapartida. Onde fica o desenvolvedor?

A licença SSPL, para dar noção completa, não é aceita pela FSF, e nem pela OSI. Então o Redis deixou de ser projeto de código aberto também.

Esse movimento de licenças manifestou limitações, então [desde o ano passado](https://redis.io/blog/agplv3), mais precisamente em maio de 2025, o Redis passou a ser distribuído com duplo licenciamento, AGPLv3 (aceita pela FSF e pela OSI) e SSPL. A licença AGPLv3 conta com cláusula _copyleft_ e um outro dispositivo, outra obrigação, que estipula a disponibilidade do código fonte sob a mesma licença para quem interagir com a peça de _software_ via rede. Deste modo o detentor de direitos se relaciona com dois perfis, oferecendo liberdade para quem a valoriza, e oferecendo alguma outra condição para quem não a valoriza.

Outro exemplo, no ramo de impressão 3D há um projeto chamado Klipper, um _firmware_ para impressoras 3D de competência tal que é embarcado em diversas impressoras disponíveis comercialmente. Há não muito tempo o licenciamento [foi violado](https://web.archive.org/web/20260607222152/https://freethecode.lol) por um fabricante, mas felizmente com o tempo e alguma pressão o fabricante não só cumpriu os termos da licença, cedendo acesso ao código fonte licenciado a quem tinha direito, mas tornou pública a versão modificada do _firmware_.

Pessoalmente, já testemunhei violações de cláusula de licenças minimalistas até, coisa que não custaria cumprir, por parte de empresa que fatura seus humildes bilhõezinhos. Mas neste departamento, via de regra, ninguém vai atrás de exigir a execução dos termos da licença. Diferente do terreno do _copyleft_, em que há preocupação com a manutenção das intenções do detentor de direitos.

Mais uma vez, _copyleft_ não é bala de prata. Violação de licença acontece, e lidar com isso de forma produtiva não é simples (dizem que o licenciamento é tão forte quanto o time de advogados que se pode contratar para fazê-la valer), mas quem deseja pode aplicar. Nem que seja, no caso da pessoa não ter recursos e/ou disposição para fazer valer a licença, como forma de expressar intenções. Assim como pode ser aplicada uma licença mais minimalista. Ao detentor dos direitos cabe a escolha.

### GPL obriga a publicação do código fonte

O grande equívoco com o qual tenho contato no domínio das obrigações estipuladas pelas licenças GPL é o da "obrigação de publicar o código fonte". Alguns chegam a acreditar que para publicar, sendo o detentor de direitos, uma peça de _software_ sob GPL é necessário disponibilizar o código fonte "no GitHub".

Há uma família de licenças GPL, o correto seria comentar cada uma delas. Mas como guia geral podemos clarificar, lembrando o que já discutimos sobre [o direito nascer do vínculo]({{< relref "260831-gnus-preparados-p2#o-vínculo-e-o-direito" >}}), que os direitos estabelecidos no licenciamento pertencem àquele que recebeu uma cópia licenciada de forma legítima, do vínculo. Que pode ser comercial inclusive, porque é perfeitamente legal comercializar um _software_ sob licença GPL, e não fere qualquer uma das liberdades, antes, faz parte do exercício delas. Então se uma pessoa que recebeu uma cópia licenciada sob GPL de um dado _software_, seja na forma de cópia verbatim ou modificada do código fonte, ou código objeto, esta pessoa que goza das liberdades e direitos concedidos por quem de direito quando licenciou a obra. 

Pode-se argumentar que esta pessoa, gozando de todas as liberdades, pode redistribuir o _software_. E de fato pode, caso não haja condição estipulada por ordem judicial ou algum contrato adicional, que vai além do licenciamento. O que conta até com uma previsão parcial que podemos verificar na 12a seção da GPLv3 de título [_No Surrender of Others' Freedom_](https://www.gnu.org/licenses/gpl-3.0.html).

Esclarecendo o glamouroso equívoco que comentamos inicialmente, o que a GPL estipula não é "publicar o código fonte", torná-lo universalmente acessível, mas fornecer àquele que obteve cópia licenciada, seja porque o detentor de direitos decidiu por liberalidade licenciar a obra original, seja em decorrência da obrigação estabelecida por licenciamento anterior do qual se obteve o benefício de uma obra derivada, fornecer àquele que obteve cópia do _software_ o código fonte nas devidas condições de licenciamento.

### Lavagem de direitos autorais

Esta é uma tendência que veio com o advento dos LLMs, os modelos de linguagem, e mereceria um artigo próprio. A lavagem de direitos autorais consiste em produzir, com base em um código fonte, um outro equivalente, uma obra derivada portanto, e passar-se por detentor de direitos de obra original. Isso vai de encontro ao aspecto moral do direito moral inclusive.

Existe uma forma "canônica" de produzir _software_ equivalente a outro existente com maior segurança de não violar direitos, o processo chamado de _clean room implementation_, em que o conhecimento da implementação original não pode de modo algum "contaminar" a nova implementação. Não é simples, ou barato, de se executar, e nem garante que o afastamento de qualquer dificuldade legal, mas é interessante ao menos uma aproximação deste processo para produzir um resultado que seja melhor defensável juridicamente. Projetos como o [Asahi Linux](https://asahilinux.org/copyright), trabalhando com engenharia reversa de componentes _Apple_, procedem neste caminho.

Com os LLMs ficou mais barato emular esse processo quando o objetivo é driblar os direitos de quem, diferente da _Apple_, não conta com um exército particular de advogados e sabe-se lá o que mais na folha de pagamento. Pode-se requisitar uma implementação parelha com instruções específicas para gerar diferenças, e, dado que o direito autoral é sobre a expressão, não cobre conceitos, métodos matemáticos etc, calculando uma sequência de palavras com diferença suficientemente que dê conta de produzir o mesmo resultado, é possível passar a obra derivada por original, ainda que isso não conte com qualquer garantia de segurança jurídica e nem haja um caminho mais ou menos "canônico", testado em litígio, para esse tipo de coisa. Mas é certo que fazer isso com seres humanos seria bastante trabalhoso, mas com uma super-calculadora à disposição a situação muda. E o sucesso desse tipo de operação, repito, vai muito dos recursos da parte lesada.

Isso sem considerar a questão de código licenciado sob licença livre com cláusula _copyleft_, ou não-livre mesmo, que pode eventualmente fazer parte da base de dados dos LLMs, mas isso é outra situação. Outra entre tantas tensões legais que vieram com os LLMs e, eventualmente, terão de ser resolvidas.

## Conclusão

O artigo foi longo, mas o assunto é muito maior. E tão grande quanto carregado de equívocos com os quais me deparei. Com este artigo espero deixar uma situação melhor do que a que encontrei.

Essa questão de liberdade e licenciamento pode não ser tão valorizada por aí, mas se não fosse pelo que foi exposto neste artigo não haveria esta série. Seja o leitor da linha que for, que valoriza antes o modo de produção de _software_ ou a liberdade na computação, isso foi viabilizado pelo _hack_ legal que nos legou uma fabulosa subversão do direito autoral.

Com o _hack_ legal, diversas pessoas não procuraram instituições de Estado para suspender a exclusividade estabelecida pelo direito autoral, e tampouco se renderam ao fatalismo e submeteram-se ao arbítrio corporativo na posição do avestruz, mas simplesmente se reuniram e se empenharam para juntos parir a própria liberdade. Para si e tantos outros que quisessem, e quiseram, juntar-se. Não nos esqueçamos deste fundamento. Mais, dando valor ao legado daqueles que nos antecederam, juntemo-nos a eles, porque [o vínculo]({{< relref "260831-gnus-preparados-p2/#o-vínculo" >}}) é o fundamento último, e já discutimos que o indivíduo não é a menor unidade de uma sociedade considerando capacidade de reprodução.

Nos próximos artigos continuaremos, no gozo da plenitude das liberdades de _software_, o caminho para nosso domínio intelectual do sistema operacional GNU.
