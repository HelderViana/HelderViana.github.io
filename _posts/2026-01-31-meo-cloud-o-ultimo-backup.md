---
layout: post
title: O último backup - A minha experiência com o fim do MEO Cloud 
category: blog
tags: meo cloud meocloud descontinuar serviço datacenter covilha dados
comments: true
---

# O último backup
## MEO Cloud
O MEO Cloud chega ao [fim](https://forum.meo.pt/outros-servicos-e-apps-meo-14/o-meo-cloud-vai-ser-descontinuado-165971) a 16 de março de 2026. Como cliente, já fui avisado por email: todos os dados serão apagados e o acesso deixará de ser possível. É o fim de uma era para quem, como eu, confiava neste serviço para guardar ficheiros e backups.

![_config.yml]({{ site.baseurl }}/images/meo_descontinuar.jpg)

## O que era o MEO Cloud?
O MEO Cloud era um serviço de armazenamento na nuvem, com planos gratuitos até 5GB e até 25GB para clientes MEO (Altice PT). Além disso, oferecia soluções pagas com maior capacidade e aplicações para Windows, MacOS, Linux, iOS e Android. O serviço apareceu no mercado Português, ainda com o nome de CloudPT [CloudPT como podememove ver neste artigo do Pplware](https://pplware.sapo.pt/sapo/cloudpt-agora-e-meo-cloud-e-esta-disponivel-para-todos/).

![_config.yml]({{ site.baseurl }}/images/meocloud_2013_thumb.jpg)

## Porque está a terminar?
A decisão surge após a [venda do Datacenter da Altice na Covilhã à Asterion](https://eco.sapo.pt/2025/11/28/asterion-compra-centro-de-dados-da-altice-na-covilha-por-120-milhoes-euros/). O MEO Cloud foi [lançado em 2013](https://kids.pplware.sapo.pt/o-meu-computador/meo-cloud-com-novas-funcionalidades/) , quando o datacenter ainda pertencia à Portugal Telecom. Sempre presumi que a infraestrutura do serviço estava totalmente alojada nesse [datacenter](https://www.datacentermap.com/portugal/covilha/meo-covilha/) . Numa altura em que tanto se fala de soberania digital na Europa (ou a falta dela), é curioso ver um dos maiores ISP portugueses vender o seu principal datacenter, potencialmente para ser adaptado ao uso em IA.

![_config.yml]({{ site.baseurl }}/images/datacenter_altice_covilha.jpeg)

## E agora, alternativas?
Com o fim do MEO Cloud, desaparecem também as alternativas nacionais para alojamento de ficheiros. Para quem necessita de manter dados em território português, as opções são escassas. O MEO recomenda serviços como Google Drive, OneDrive, iCloud e Dropbox — todos fora da Europa.


## O meu desagrado
Confesso que não fiquei satisfeito com esta decisão. Tenho uma conta com 25GB usada principalmente para backups, que agora terão de ser redirecionados para outro local. Já uso outros serviços cloud, mas esta notícia reforça a minha vontade de apostar numa NAS robusta e de grande capacidade, mantendo os meus dados mais próximos de mim e sob controlo direto.


## O que fazer?
Agora é preciso planear os próximos passos:
1. Primeiro, vou fazer um backup dos dados para dois locais distintos: manter os dados locais no computador, descarregar tudo para a minha NAS e migrar os cerca de 20GB para uma cloud pública paga, garantindo assim três cópias — incluindo uma em localização diferente.
2. Depois, vou atualizar os meus processos de backup para que deixem de usar o MEO Cloud e passem a utilizar um novo destino.
3. A longo prazo, como fã de home lab e self-hosting, pretendo criar uma nova NAS capaz de sincronizar dados dentro e fora de casa. O projeto tem estado parado por questões de orçamento e tempo, mas planeio reutilizar equipamento recondicionado em bom estado. Assim que tiver o hardware, darei início ao projeto.


## Qualidade do serviço
Durante 13 anos como cliente do MEO Cloud, nunca tive problemas com o serviço. Não era o melhor do mundo, mas também estava longe de ser o pior. Sempre foi rápido, provavelmente por estar alojado no país, e oferecia aplicações para todas as plataformas principais.
Sempre tive a impressão de que o serviço funcionava sobre uma versão adaptada do Nextcloud, com uma API compatível com Dropbox (link), o que era uma mais-valia para serviços que dependiam desta cloud.
O MEO Cloud sempre foi uma imagem de marca da PT Inovação, depois Altice Labs. É com pena que vejo o serviço terminar, sabendo que a qualidade nunca foi um problema.
Lembro-me até de em determinado momento (algures em 2015, pelo menos) o meo cloud ter suportado a opção de alojar web assets estáticos, tais como ficheiros .html, .css, permitindo usa-los como endpoint de um endereço web. Não consegui encontrar a documentação do processo, mas cheguei mesmo a configurar uma página web estática dessa forma.
