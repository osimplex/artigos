---
title: "spectrwm e população do painel: para além do baraction.sh"
date: "2020-07-22"
palavras-chave: ["painel", "spectrwm", "liguagem C"]
ano: ["2020"]
featured: false
---

Aqui será explorada, de maneira brevíssima, a possibilidade de uso de um programa em C para população do painel do gerenciador de janelas _spectrwm_, que, segundo o manual, suporta o uso de executáveis externos. O manual, ao descrever o elemento `+A` na parametrização de `bar_format`, diz que se trata de `output of the external script`, e, ao descrever o parâmetro `bar_action`: `external script that populates additional information in the status bar, such as battery life`.

Apesar do uso da palavra `script`, qualquer executável que envie texto para a saída padrão pode ser definido na parametrização de `bar_action`, um _script_ de qualquer interpretador, ou um binário.

Será apresentada, de maneira muito superficial, a possibilidade de uso do `dwmblocks`, que seria algo próximo ao [i3blocks](https://github.com/vivien/i3blocks), só que mais simples, no sentido _suckless_ da palavra.

## Introdução
Este artigo não será tão extenso, simplesmente apresentará a possibilidade, e apresentará uma sugestão de exercício.

O programinha que será explorado é chamado `dwmblocks`, porque foi inicialmente pesado para o gerenciador de janelas do projeto _Suckless_ `dwm`. Este programa possui menos de 200 linhas de código, é de compreensão simples, e, como todo recurso ligado ao _suckless_, carrega a proposta de que o utilizador lide com o código fonte e compile o programa já adequado para às próprias necessidades. Aí reside o exercício, bem pertinente, uma vez que o canal debxp está a fornecer material acerca de linguagem C, [sistemas lógicos](https://www.youtube.com/playlist?list=PLXoSGejyuQGoGC6SqZWP7Bc0mWVFzpGqr) e shell (como uma atenção para o interpretador de comandos Bash).

## Especificação
As explicações serão feitas sobre o `dwmblocks` de "torrinfail".

Apesar do `dwmblocks`, por ser criado para uso com o `dwm`, normalmente enviar a saída de texto para a o campo de nome para a janela raiz do X, de onde o `dwm` recolhe o texto para o painel, ele também pode ser usado para enviar o texto para saída padrão, e inclusive implementa esta capacidade:

```c
void pstdout()
{
	if (!getstatus(statusstr[0], statusstr[1]))//Only write out if text has changed.
		return;
	printf("%s\n",statusstr[0]);
	fflush(stdout);
}
```

Para usar este recurso é necessário executar o binário com `$ dwmblocks -p`, supondo que o binário esteja em um caminho do `$PATH`, conforme se verifica na implementação da função `main`:

```c
int main(int argc, char** argv)
{
	for(int i = 0; i < argc; i++)
	{
		if (!strcmp("-d",argv[i]))
			delim = argv[++i][0];
		else if(!strcmp("-p",argv[i]))
			writestatus = pstdout;
	}
	signal(SIGTERM, termhandler);
	signal(SIGINT, termhandler);
	statusloop();
}
```

Mas, caso o utilizador prefira definir o comportamento no código fonte, basta alterar a linha que define a função que realiza a escrita do texto, que no original consta como:

```c
static void (*writestatus) () = setroot;
```

## Mais particularidades no uso com spectrwm
Por causa da metodologia de recolha da saída de texto do executável, basicamente monitoramento da saída padrão atrelada ao processo, não é possível utilizar uma metodologia para reiniciar o programa de população da barra de forma independente do gerenciador de janelas, é necessário que seja reiniciado o gerenciador de janelas para que ele finalize o processo corrente do `dwmblocks`, e então inicie um outro. No caso do `dwm`, por usar do nome da janela raiz do X, há uma independência nesse aspecto, e é possível reiniciar o binário sem que se tenha que reiniciar o gerenciador de janelas também.

A parametrização do `spectrwm` deve contar com:

 * `bar_action = dwmblocks`, caso do binário esteja no `$PATH`, do contrário é necessário especificar o caminho. Como `spectrwm` realiza expansão de til, um exemplo: `bar_action = ~/bin/dwmblocks`.

 * `bar_format` deve conter `+A` em alguma posição, para que o texto seja apresentado no painel

 * `bar_action_expand` igual a 1 se for utilizado algum recurso de sintaxe da barra dentro da população externa do painel, do contrário, igual a 0.

## Finalização
Este artigo apresenta muito mais uma sugestão de exercício e estudo do que qualquer outra coisa. Mas espera-se que, mesmo assim, constitua uma sugestão útil para utilizadores do `spectrwm` que desejem algo mais elaborado para população do painel, como o suporte para tratamento de sinais (SIGRTMIN) e definição de intervalo para cada elemento que constitui o painel. E não só o `spectrwm`, mas qualquer gerenciador de janelas ou painel que suporte o acoplemento de um executável externo que popule o painel.

Como sugestão extra, a leitura das requisições de modificação do `dwmblocks` pode fornecer alugumas sugestçoes interessantes para aplicar na própria versão.

## Leitura complementar

  * [man:spectrwm](https://man.archlinux.org/man/community/spectrwm/spectrwm.1.en)
  * [ArchWiki:spectrwm](https://wiki.archlinux.org/index.php/Spectrwm)
  * [GitHub:spectrwm](https://github.com/conformal/spectrwm)
  * [GitHub:dwmblocks](https://github.com/torrinfail/dwmblocks)

## Exposições em vídeo

  * [Luke Smith:Wrap Your Brain around 'Patch' and 'Diff' on Linux](https://www.youtube.com/watch?v=-CiLU9-RAGk)

## Instrução sobre Bash

  * [Texto:Curso básico de programação em Bash](https://debxp.org/cbpb)
  * [Vídeo:Curso básico de programação em Bash](https://www.youtube.com/playlist?list=PLXoSGejyuQGpf4X-NdGjvSlEFZhn2f2H7)
  * [Texto:Além do Bash](https://debxp.org/alem_do_bash)
  * [Vídeo:Além do Bash](https://www.youtube.com/playlist?list=PLXoSGejyuQGpen1lAlhngkpuldmot8DV0)

## Instrução sobre a linguagem C

  * [Texto:Fundamentos da Linguagem C](https://debxp.org/fund-c)
  * [Repositório:curso-fundc](https://gitlab.com/blau_araujo/curso-fundc)
