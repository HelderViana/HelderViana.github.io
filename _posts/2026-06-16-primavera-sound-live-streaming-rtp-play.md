---
layout: post
title: Primavera Sound - Live Streaming RTP Play 
category: blog
tags: rtpplay rtp primavera sound primaverasound streaming live ffmpeg yt-dlp hack rip h264
comments: true
---

# Primavera Sound 2026: como gravei as emissões da RTP Play
## O que é o Primavera Sound?
O Primavera Sound Porto é a edição portuguesa do festival Primavera Sound, nascido em Barcelona em 2001. Chegou ao Porto em 2012 — o mesmo ano em que também me mudei para cá. Na primeira edição a que fui, vi nomes como Suede, The Flaming Lips, Rufus Wainwright, Wilco e Yo La Tengo. Alguns já conhecia bem, outros acabaram por ganhar outro peso por os ter visto ali.

![_config.yml]({{ site.baseurl }}/images/Primavera_Sound_2026.png)


## O que é que isto tem a ver comigo?
Quem me conhece sabe que isto tem tudo a ver comigo. Ando em festivais desde, pelo menos, 2003, e durante anos fui presença habitual no festival que acontece nas margens do rio Coura. Desde que vivo no Porto, tenho explorado mais o lado dos festivais urbanos e, dentro desse contexto, o Primavera Sound é claramente o flagship da cidade. Nos últimos anos tenho conseguido ir pelo menos um dia, ano sim, ano não. Este ano foi um dos anos em que não fui — e a alternativa natural passou pelas transmissões em direto da RTP Play. 




## O problema não era ver em direto. Era conseguir guardar.
À partida, a solução parecia simples: abrir a RTP Play e acompanhar os concertos. Na prática, o problema nunca foi “conseguir ver”. O problema era outro: nem sempre consigo estar disponível quando a emissão acontece. Fins de semana têm dinâmica própria, a vida familiar raramente é compatível com a agenda de um festival, e há um padrão quase universal de concertos interessantes a caírem precisamente em horários pouco convenientes — jantar, tarefas domésticas, fim de dia, ou demasiado tarde. 

Foi isso que me fez lembrar uma coisa que, durante a adolescência, era absolutamente banal: se passasse alguma coisa importante na televisão, bastava ter uma cassete VHS disponível e deixar o vídeo a gravar. O processo tinha limitações, mas era simples, tangível e compreensível. Hoje, apesar de toda a evolução tecnológica, fazer algo funcionalmente equivalente acabou por exigir uma infraestrutura bastante mais complexa. 


![_config.yml]({{ site.baseurl }}/images/rtp_play_landpage.png)



## Que alternativas existem hoje?
A primeira pergunta foi esta: como é que se grava, hoje, uma emissão destas? 

A resposta curta é: depende muito mais da plataforma do que do utilizador. E no caso concreto deste festival, percebi rapidamente que as soluções “domésticas” tradicionais não me resolviam o problema. 
1. Gravação na box do operador
Durante anos, as boxes dos operadores em Portugal permitiram gravar emissões de TV. Mas para o meu caso havia dois bloqueios imediatos:

a gravação fica, na prática, fechada no ecossistema da box;
a emissão que me interessava não estava apenas em sinal aberto tradicional, mas sobretudo na RTP Play. 

Ou seja: mesmo que a box conseguisse gravar alguma coisa, eu continuava sem resolver a parte que me interessava mesmo — preservar localmente aquilo que estava em streaming. 
2. Placa de captura
Também considerei a hipótese de usar uma placa de captura AV. Tecnicamente é uma opção válida, mas com uma limitação estrutural para este caso: eu ficaria preso à emissão que estivesse fisicamente a ver naquele momento, naquela máquina. Para uma gravação pontual podia servir, mas não me dava margem para automatizar nem para escalar a captura. 
3. OBS
A mesma lógica se aplicava ao OBS. Sim, podia gravar a sessão local. Mas continuava a ser uma solução centrada naquilo que estivesse a ser reproduzido manualmente, não uma pipeline de captura autónoma. Para um cenário de múltiplos palcos, horários tardios e necessidade de continuidade, isso era demasiado frágil. 
4. Captura direta do stream
Foi aqui que a questão mudou de figura. Em vez de pensar em “gravar o ecrã”, comecei a pensar em capturar o stream diretamente. Essa hipótese parecia de longe a mais interessante, mas também a menos imediata. Nunca tinha feito isso neste contexto, por isso a tarefa deixou de ser “usar uma ferramenta” e passou a ser investigar como o streaming estava montado, onde estavam os bloqueios e o que seria preciso fazer para os contornar tecnicamente. 


![_config.yml]({{ site.baseurl }}/images/vhs_recorder.png)


# Investigação inicial: yt-dlp, ffmpeg e os primeiros limites
Comecei por pedir ajuda a um agente de AI, porque nunca tinha precisado de fazer isto antes e, se o ia fazer agora, queria primeiro perceber como funcionava a distribuição do stream. As duas sugestões que apareceram logo foram previsíveis: yt-dlp e ffmpeg. Eu já conhecia o segundo, mas comecei por experimentar o primeiro. 

Os testes iniciais foram esclarecedores precisamente porque falharam. Consegui capturar o stream da RTP1, mas apenas durante alguns segundos — tipicamente cerca de 120 segundos. Esse detalhe foi importante porque mostrou logo que o problema não era simplesmente “arranjar o URL certo”. Havia algo mais na forma como a RTP expunha o stream, e a solução exigia perceber esse detalhe. 

Ao fim de mais algumas voltas, ficou claro que o streaming da RTP assentava numa playlist master .m3u8, onde o ficheiro principal referencia os vários substreams. Isso por si só já abria uma pista útil, mas rapidamente se tornou óbvio que o acesso estava condicionado por um token de autenticação, e que esse token tinha de ser obtido e renovado de forma compatível com a sessão da RTP Play. Foi aí que o protótipo começou realmente a tomar forma

```bash
# inspeção manual de uma playlist HLS
ffprobe "https://streaming-live.rtp.pt/liverepeater/smil:rtp1HD.smil/playlist.m3u8?tk=1781146800_b75db064616288a913dffd3b9a8de40a82a215f8"

# tentativa inicial de captura
ffmpeg -i "https://www.rtp.pt/play/direto/rtp1" -c copy output.ts

```

## O verdadeiro problema: autenticação e renovação do token
A partir daqui a dificuldade deixou de ser “como gravar um stream” e passou a ser “como continuar a gravar um stream cuja sessão depende de autenticação”. Foi essa a diferença fundamental entre uma solução superficial e uma solução funcional. Não bastava apontar o ffmpeg a um URL estático: era preciso obter o token que a RTP Play usava, passá-lo como cabeçalho correto e garantir que a captura se mantinha até ao fim da emissão ou até interrupção manual. 

Depois de cerca de duas horas de iteração, acabei com um protótipo a usar ffmpeg + Chromium, precisamente para capturar o token de autenticação da RTP Play e assim conseguir manter a captura ativa. Dito desta forma parece linear; não foi. Foi o ponto em que a tarefa deixou definitivamente de ser acessível a um utilizador não técnico e passou a ser um pequeno problema de engenharia aplicada ao consumo de media doméstico. São estes os problemas que estou habituado a resolver, num contexto de trabalho, mas não estava à espera que fosse assim tão complexo para uma tarefa que devia ser rotineira. 

Comecei então a prepar tudo o que precisava para ter uma demo funcional.


```bash

sudo apt update
sudo apt install python3.12-venv ffmpeg chromium-browser

python3 -m venv .venv
source .venv/bin/activate

pip install playwright
playwright install

```


## Ambiente: Windows, WSL, Ubuntu e Python
Como decidi usar um PC Windows, mas queria correr a solução num ambiente Linux mais previsível e robusto, comecei a configurar tudo rm WSL,  estava a usar uma versão bastante mais desatualizada. Pelo caminho foi preciso atualizar o próprio WSL, para a versão 24.04 (não é o mais recente, mas já funcionou bem), tive de rever a versão de Python, configurar python3.12-venv, criar ambientes virtuais e instalar dependências do Chromium. Isto é um dos detalhes que gosto de manter no texto, porque ajuda a mostrar a realidade da solução: não apareceu “porque sim”, e não foi uma sequência de três cliques. Houve trabalho de base para preparar o ambiente antes da captura sequer começar a funcionar

![_config.yml]({{ site.baseurl }}/images/python_ver.png)

## Solução
A arquitetura final ficou montada em cima de um script Python que usa bibliotecas ligadas ao Chromium, Playwright e ffmpeg. A lógica geral era esta: usar o browser para obter contexto e autenticação, extrair o que fosse necessário dos pedidos feitos pela RTP Play e, depois, reutilizar essa informação como cabeçalhos nos pedidos feitos pelo ffmpeg. Foi isso que permitiu manter a captura até ao fim da transmissão.
Em termos conceptuais, o browser deixou de ser “o sítio onde eu vejo o concerto” e passou a ser uma peça da pipeline. Isso, por si só, já diz muito sobre o estado atual destas plataformas: para gravar algo equivalente ao que antes cabia numa VHS, precisei de um browser automatizado a funcionar como intermediário entre autenticação de sessão e ferramenta de captura.


## Captura da emissão e conversão
Uma vez resolvida a parte da autenticação, consegui capturar as emissões no formato .ts. Depois, bastava voltar a usar o ffmpeg para converter a transmissão para .mp4 assim que ela terminasse. Este modelo funcionou bem porque separa a fase de captura da fase de pós-processamento: primeiro garante-se que a emissão fica em disco, depois trata-se do formato final.

## Capturar e preservar
Capturar e preservar não é o mesmo que “fazer download de um ficheiro estático”. Num cenário como este, em que se está a lidar com uma emissão contínua, autenticada, sujeita a início e fim controlados pela plataforma, adicionou uma camada extra de complexidade. Primeiro garantir que não haveriam falhas/interrupções na captura (computador adormecer, adormecimento da placa de rede, espaço em disco, etc) e no fim, criar uma cópia nos standards atuais (H264) para guardar em Aquivo e ver em multiplas plataformas.


```bash
# no final foi só converter o formato ts em mp4 (H.264)
ffmpeg -i rtp_palco3_1781456969.ts -c copy rtp_Xinobi_PrimaveraSound_2026.mp4

```

![_config.yml]({{ site.baseurl }}/images/ffmpeg_convert.png)


## Um detalhe importante: o stream terminava e o processo parava
Um dos pormenores mais úteis foi perceber que, quando a RTP interrompia a transmissão, o ffmpeg também detetava o fim do stream e terminava a captura. Isto é um detalhe pequeno, mas mudou bastante o desenho da solução, porque me permitiu encaixar o processo em agendamentos sem ter de estar a gerir manualmente o momento de fecho da emissão. 

Foi isso que mais tarde me permitiu conciliar a captura com alguns agendamentos em cron e automatizar principalmente as emissões mais tardias. Ou seja: mesmo quando eu não estava disponível para acompanhar o concerto, o sistema podia arrancar sozinho, capturar enquanto o stream existisse e terminar no momento em que a emissão acabasse

![_config.yml]({{ site.baseurl }}/images/ffmpeg_warnings.png)

## Escalar para vários palcos
Como a RTP Play disponibilizava três palcos virtuais para o evento, criei três scripts distintos, um para cada URL. Podia ter simplificado apenas com 1 script e diferentes configurações, uma por palco, mas não quis correr riscos de algo falhar, quando a primeira captura ficou funcional.Isso permitia usar um processo independente por palco e, na prática, capturar mais do que uma emissão em simultâneo. Este ponto é importante porque mostra que a solução passou rapidamente de uma prova de conceito para algo minimamente operacional. Não estava apenas a resolver um caso pontual; estava a construir uma forma repetível de lidar com várias emissões no mesmo evento

## Resultado
No final disto tudo, consegui ficar com uma recordação de mais de 12 GB de concertos que não tive oportunidade de ver em direto. Isso, por si só, já justificava o esforço. Mas o que mais me ficou deste processo não foi apenas o ficheiro final, foi a distância enorme entre o que esta tarefa era antigamente e aquilo em que se transformou hoje. 

Essa distância fez-me lembrar de imediato um momento muito específico da adolescência / início da vida adulta: uma vez gravei em VHS um concerto de Placebo que estava a passar numa emissão da Viva II. Ainda hoje tenho essa cassete. Na altura, apesar das limitações e dos cabos, o processo era radicalmente mais simples: ligar o equipamento à televisão, acertar a entrada, meter a cassete e carregar em Record. Talvez ainda fosse preciso andar um pouco às voltas com um cabo SCART, mas a lógica era direta e acessível.

# O que me impressiona mais nisto tudo
O que mais me impressiona não é o facto de hoje existir streaming em alta qualidade, múltiplos palcos em simultâneo, ou distribuição instantânea. Isso são ganhos óbvios. O que me impressiona é que uma tarefa que já foi banal se tenha tornado tão pouco acessível. E aqui não estou a falar só da barreira técnica abstrata, estou a falar da experiência concreta de implementação. Para chegar a uma solução funcional, foi preciso perceber a estrutura do stream, identificar uma playlist master .m3u8, lidar com autenticação, usar um browser automatizado para capturar contexto de sessão, alimentar o ffmpeg com os cabeçalhos certos, montar um ambiente em WSL, atualizar componentes, tratar dependências e automatizar execuções com cron, tudo isto com a ajuda da IA.

Isto não é uma coisa que eu consiga explicar rapidamente aos meus pais. Não é uma sequência de passos que eu entregue a um amigo que não trabalhe com código, ou em IT, e espere que funcione à primeira. Mesmo para alguém técnico, habituado a resolver problemas de software e infraestrutura, houve investigação, tentativas falhadas, ajustes e algum tempo até estabilizar uma solução robusta. E isso talvez seja a parte mais reveladora de todo o processo: ganhámos imenso em conveniência e distribuição, mas perdemos simplicidade, transparência e controlo. Principalmente controlo!

Antes, gravar era uma funcionalidade do consumo doméstico. Hoje, em muitos casos, é quase um problema de engenharia. E essa transformação diz muito sobre a maneira como passámos de um modelo em que o utilizador tinha posse e controlo local sobre aquilo que consumia para um modelo em que o acesso é temporário, mediado por plataformas e sujeito a restrições cada vez mais opacas. E atenção, não sou critico desta plataforma em particular, muito pelo contrario, tenho orgulho na solução tecnologica da RTP, até porque como contribuinte, tenho de lhe dar uso, não só por ser uma boa solução tecnologica, mas também pela qualidade dos conteudos disponiveis. 

No fundo, foi esta falta de control, no que vemos e ouvimos, que me levou a fazer esta solução: não apenas para guardar concertos, mas para recuperar um pequeno grau de controlo sobre uma experiência que, apesar de tecnologicamente mais avançada, se tornou estranhamente mais efémera e mais distante.

### Links importantes:
[RTP Play](https://www.rtp.pt/play/) 

[yt-dlp](https://github.com/yt-dlp/yt-dlp)

[ffmpeg](https://www.ffmpeg.org/)

[Primavera Sound Porto](https://www.primaverasound.com/pt/porto) 

[my github repo](https://github.com/HelderViana/rtp-dvr)

