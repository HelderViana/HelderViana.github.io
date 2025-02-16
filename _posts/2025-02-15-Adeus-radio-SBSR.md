---
layout: post
title: Adeus SBSR.fm
category: blog
tags: radio sbrs futura radar oxigenio fm streaming audio vlc record
comments: true
---

# Adeus SBSR: A Minha Despedida da Rádio Indie no Porto
Desta vez, volto a escrever em português. O tema não é particularmente tecnológico, mas sim cultural—algo muito pessoal para mim.
Hoje partilho um pouco do que era o meu dia-a-dia radiofónico, ou melhor, do que eu ouvia diariamente. Vivo no Porto e tinha o privilégio de sintonizar o auto-rádio em 91.0 MHz, que até 30 de setembro de 2024 foi a frequência da rádio [SBSR](https://arquivo.pt/wayback/20230304022027/https://www.sbsr.fm/) na cidade. Esta era a banda sonora das minhas viagens de carro.
No entanto, a SBSR foi vendida à [Media Livre](https://pt.wikipedia.org/wiki/R%C3%A1dio_SBSR) e deixou de emitir a música indie a que tanto me habituei.

![_config.yml]({{ site.baseurl }}/images/sbsr_arquivio.png)

## O Fim de uma Era
Como uma morte anunciada, pelo menos tive a oportunidade de me despedir desta rádio condignamente. Não foi a primeira vez que passei por isto.
Lembro-me de me ter despedido da Voxx, que, se a memória não me falha, fechou por volta de 2004. Durante algum tempo, continuou a emitir em modo “automático”, como se fosse apenas uma playlist em loop. Mais tarde, a Rádio Nostalgia também encerrou, e as suas frequências foram assumidas pela própria SBSR.
Desta vez, decidi que não queria apenas assistir ao desaparecimento de mais uma rádio. Queria gravar as últimas emissões da SBSR.

## Gravar Rádio: Ontem vs. Hoje
Já tinham passado muitos anos desde a última vez que quis ou precisei de gravar uma emissão de rádio… e já nem me lembrava exatamente de como o fazer. Para piorar, a tecnologia evoluiu imenso desde então.
A última vez que gravei algo do género foi com uma cassete áudio (K7) num rádio com gravador. Ainda tenho algumas cassetes por casa, mas essa opção estava fora de questão.
Se bem me recordo, foi algures entre 1999 e 2000—provavelmente uma emissão da Rádio Nostalgia ou da Voxx. Na altura, não havia muitas alternativas para gravar rádio em casa. O meu primeiro PC já existia nessa altura, mas nem sequer me passou pela cabeça usá-lo para isso. E mesmo que tivesse pensado nisso, o [Sound Recorder do Windows](https://en.wikipedia.org/wiki/Sound_Recorder_(Windows))  não ia ajudar muito...

![_config.yml]({{ site.baseurl }}/images/TDK_D-C60.jpg)

![_config.yml]({{ site.baseurl }}/images/Sound_Recorder_in_Windows_98.png)


## Uma Solução Moderna para um Problema Antigo
Desta vez, o cenário era diferente. O meu conhecimento técnico cresceu desde o ano 2000 (ou pelo menos gosto de acreditar que sim). Também os tempos mudaram: já não tenho nenhum rádio com deck de cassetes funcional, e a gravação analógica estava fora de questão.
Foi então que decidi procurar uma solução digital.



## Primeiras Opções
Comecei por perguntar ao ChatGPT quais seriam as melhores opções para gravar áudio, tanto via streaming como via entrada analógica de um sintonizador ligado ao PC. Tinha algumas máquinas à disposição e sabia que alguma delas serviria para o propósito.
Entre as sugestões que o ChatGPT me deu, estavam:
•	Audacity
•	OBS Studio
•	Streamripper
Pedi apenas opções open source ou freeware. No entanto, nenhuma destas me convenceu completamente.

## O VLC: A Solução Ideal
Queria algo versátil, simples, mas ao mesmo tempo flexível para ser automatizado. Não podia ficar em casa, em frente ao PC, a monitorizar a gravação. Foi então que me lembrei do bom e velho VLC.
Afinal, além de reproduzir quase todos os formatos de áudio e vídeo, o [VLC](https://www.videolan.org/) também permite gravar streams. Perguntei ao ChatGPT se seria possível utilizá-lo para esta tarefa, e a resposta foi afirmativa.
Depois de alguma troca de ideias, chegámos a um script para controlar e automatizar todo o processo:

 ```batch
@echo off
:: Script para gravar stream de rádio com VLC

:: URL do stream
set STREAM_URL=https://centova.radios.pt/proxy/435?mp=/stream/1/

:: pasta para guardar as gravações
set OUTPUT_DIR=C:\temp\sbsr

:: Timestamp no nome dos ficheiros:
for /f "tokens=1-5 delims=/: " %%d in ("%date% %time%") do (
    set DATE=%%d-%%e-%%f_%%g-%%h
)

:: Nome dos ficheiros de ouput:
set OUTPUT_FILE=%OUTPUT_DIR%\stream_%DATE%.mp3

:: Comando VLC para gravar por 1 hora (3600 segundos) e depois parar
"C:\Program Files\VideoLAN\VLC\vlc.exe" %STREAM_URL% --sout file/mp3:%OUTPUT_FILE% --run-time=3600 vlc://quit
 ```

## Automatizar com o Gestor de Tarefas do Windows
Para garantir que a gravação fosse contínua e sem falhas, criei uma tarefa no Gestor de Tarefas do Windows, que era acionada a cada hora. Como o script gravava segmentos de 60 minutos (ou melhor, 3600 segundos), esta configuração assegurava que toda a emissão era capturada de forma estruturada.
O áudio era comprimido e armazenado em ficheiros de 1 hora, com nomes organizados pelo timestamp do início de cada gravação.

![_config.yml]({{ site.baseurl }}/images/task_scheduller.png)

## Obter o Stream Certo
Para encontrar o stream da SBSR, recorri ao site [radios.pt](https://radios.pt/), um agregador de rádios portuguesas. Depois de identificar o link correto, apenas precisei de garantir que tinha espaço em disco suficiente para armazenar todas as horas de emissão.
E assim fiz! Consegui gravar praticamente toda a última semana de setembro de 2024.
________________________________________
## E Agora?
Depois do encerramento da SBSR, fiquei, mais uma vez, órfão de rádio indie.
Como alternativa, podia sempre recorrer à Antena 3—uma excelente rádio pública, com alguns programas de autor. Mas, no geral, a programação em horário nobre não me identificava tanto.
Decidi então explorar outras opções.
Explorar Alternativas: Radar, Oxigénio e… Futura
Comecei a testar o streaming da [Radar](https://radarlisboa.fm/) e da [Oxigénio](https://www.oxigenio.fm/), ambas com emissão FM em Lisboa, mas sem transmissão no Porto. Aqui se nota mais uma vez uma grande assimetria no panorama radiofónico português. Lisboa tem alternativas que simplesmente não existem no resto do país.

Foi então que descobri a [Rádio Futura](https://www.radiofutura.pt/). Uma rádio de autor, com emissão exclusivamente online a partir de Lisboa.
Era exatamente isto que eu procurava!
Além disso, acabei por aprofundar um pouco mais a programação da Radar e da Oxigénio, que também oferecem conteúdos de grande qualidade.
A Falta do FM
Apesar das alternativas em streaming, continuo a sentir falta do FM. Gosto de entrar no carro e simplesmente sintonizar uma estação, sem ter de me preocupar com emparelhar o telemóvel ou configurar a reprodução no auto-rádio.
Hoje em dia, a rádio continua a ser a minha companhia principal nas viagens de carro, mas cada vez mais no formato streaming. Quando ando com as crianças, raramente ponho podcasts—até porque, na maioria das vezes, são em inglês, e elas não iriam apreciar.
No fim, a rádio continua a ser uma parte essencial do meu dia-a-dia. Mas a forma como a consumo mudou completamente.

![_config.yml]({{ site.baseurl }}/images/radio_futura.png)

________________________________________

## Conclusão
A perda da SBSR foi um golpe para quem, como eu, cresceu com rádios indie em FM. Felizmente, ainda existem boas alternativas, mesmo que algumas estejam apenas disponíveis via streaming.
Se também passaste por algo semelhante, qual foi a tua solução? Como consomes rádio hoje em dia?
Deixa o teu comentário! 🚀🎧


