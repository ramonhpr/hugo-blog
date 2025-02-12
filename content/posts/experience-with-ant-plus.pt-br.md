+++ 
draft = false
date = 2025-02-11T21:14:01-03:00
started_date = 2025-01-21T21:14:01-03:00
title = "Desafios e Aprendizados no Uso do ANT+ para Monitoramento Cardíaco"
description = "my test description"
slug = ""
authors = ["Ramon Ribeiro"]
tags = ["javascript", "React", "Electron"]
categories = ["Estudo de caso"]
externalLink = ""
series = []
+++


![Thumbnail](/images/antplus.png)


Há alguns anos, aceitei um desafio interessante: desenvolver um aplicativo desktop que exigia o uso de uma tecnologia que eu nunca havia experimentado antes. Essa tecnologia era o [ANT+](https://www.thisisant.com/).


Após uma rápida pesquisa no Google, percebi que o ANT+ era bastante semelhante ao Bluetooth, com o qual eu já tinha experiência. Isso me deu confiança para encarar o projeto de frente.


# Sobre o app


O objetivo do aplicativo era criar uma dashboard para uma academia, permitindo o monitoramento da frequência cardíaca dos alunos durante os treinos. Com esses dados, o personal trainer poderia acompanhar o desempenho em tempo real e oferecer suporte imediato aos alunos.


O app seria acessível para os próprios alunos, possibilitando que eles acompanhassem métricas como calorias queimadas e interajam com o instrutor de maneira dinâmica.


# Sobre os dispositivos


Para os dispositivos ANT+ foi utilizados um [dongle USB](https://www.amazon.com.br/CooSpo-extens%C3%A3o-compat%C3%ADvel-TrainerRoad-Sufferfest/dp/B07CB4328P?th=1) para fornecer conectividade ao computador. o dongle suporta até 8 canais de comunicação.


![usbe dongle](https://m.media-amazon.com/images/I/71FJqTisEqL._AC_SX522_.jpg)


O outro dispositivo é a cinta de monitoramento cardiaco da [magene H64](https://www.amazon.com.br/CINTA-CARD%C3%8DACA-MAGENE-H64-BLUETOOTH/dp/B08XK71V4R/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=10DK7IGAA40D9&dib=eyJ2IjoiMSJ9.yL1F_TJyMCi8kNBnfZqSGCoks7ULkuGJyeJw6A78TjgGeDr08bMc4Rh-IRX9ngnWtZ7hHbtW-Tc0q5IZMN9DASfCBe_-oBWQ3W4YhBmp-UAEOejG09mq2BdssWDPFqr4J78_El4z_IdJSxWs3--gEBScwqLvzKNfQnzT6bWAkGMU3mebZaaArVc5dIfOzu6xeMdrxEXtb_9YiFAv-1mWTMkv6DstMkyiUQ1thjJMjCo.H3srSKm5WHeVjLKBwdNY5Wynq7_8A9HU_HU7-OrAuHE&dib_tag=se&keywords=magene+H64&qid=1738501332&sprefix=magene+h64%2Caps%2C408&sr=8-1&ufe=app_do%3Aamzn1.fos.6a09f7ec-d911-4889-ad70-de8dd83c8a74), tem conectividade para bluetooth 4.0 e ANT+.


![magene h64](https://m.media-amazon.com/images/I/61H7YdeTANL._AC_SX679_.jpg)


# Dificuldades enfrentadas


Fora o desafio de aprender sobre uma nova tecnologia, acho que uma das maiores barreiras foi encontrar documentação adequada sobre o que eu deveria utilizar para começar o desenvolvimento.


Então eu pensei primeiro no requisito do sistema se ele deveria rodar em computador desktop ao invés de uma sistema web (que eu tenho mais experiência em desenvolver) logo eu deveria procurar por tecnologias que me forneçam um bom suporte para esse desenvolvimento.


Com isso eu logo pensei no framework [Electron](https://www.electronjs.org/), com ele poderia utilizar minhas habilidades de desenvolvimento como CSS e HTML, que esse framework iria converter isso para o formato desktop.


Ainda não satisfeito com isso eu também queria aproveitar minhas habilidades com framework [React](https://react.dev/),
que me permitiria criar facilmente paginas com sua sintaxe [JSX](https://legacy.reactjs.org/docs/introducing-jsx.html).


Mas eu sabia que toda essa configuração de fazer com que o React e o Electron comunicassem entre si poderia ser muito custoso para se iniciar de cara. Então eu procurei um repositório open-source com um boilerplate.
Para minha surpresa eu encontrei um com várias estrelas no github, o [electron-react-boilerplate](https://github.com/electron-react-boilerplate/electron-react-boilerplate) (nome bem direto por sinal)


No fim eu acabei fechando minha arquitetura de frontend pelos motivos já mencionados.


Agora a parte mais difícil foi encontrar uma biblioteca para receber e enviar comandos do ANT+... Não pude encontrar nenhuma API oficial do ANT+ para javascript que eu pudesse usar no meu projeto.


Tive que utilizar bibliotecas de terceiros, com muito receio pois eu não teria uma comunidade de desenvolvimento forte para poder consultar. Eu tentei a comunicação com várias bibliotecas
que eu procurei no npm. Mas após algumas tentativas eu pude encontrar uma que atendesse minhas expectativas e eu pude receber e enviar dados para a cinta cardiaca.


# Sobre as tecnologias usadas


## ANT+


Como mencionado, o ANT+ é uma tecnologia de comunicação sem fio com o bluetooth porém ele tem seu foco em aplicações fitness e de bem-estar. Diferente do bluetooth que é focado em comunicação geral de dispositivos.


Outro ponto chave para diferenciar as duas tecnologias é que o ANT+ tem uma maior eficiência energética e mesmo com versões mais novas do bluetooth, como BLE (bluetooth low energy), elas ainda podem não ser tão eficientes como o ANT+ em alguns cenários. Fora isso, o ANT+ permite uma maior conectividade entre dispositivos, podendo fazer conexões multiponto.


Como é de se esperar, o bluetooth, por ser um protocolo de comunicação geral, tem uma comunidade de desenvolvimento mais abrangente do que o ANT+ e no ANT+ há também uma falta de documentação que dificulta o desenvolvimento.


Diferente do bluetooth que requer uma fase de requisição de conexão em handshaking, o ANT+ funciona com canais de broadcast. Bastando o receptor das requisições sintonizar o canal correto para receber. Você pode estar se perguntando mas e se dois dispositivos estiverem usando o mesmo canal. A resposta para isso está a cargo do próprio ANT+ que consegue gerenciar essa alternância de canais eficientemente.


### Usando a biblioteca [gd-antplus](https://github.com/gdoumen/ant-plus)


A biblioteca tem duas principais classes que foram usadas: **GarminStick2** e **HeartRateSensor**


- **GarminStick2**: abstrai o uso do dongle USB, verificando se ele está conectado e pronto para escutar os canais.


- **HeartRateSensor**Responsável por interpretar os dados transmitidos pela cinta cardíaca.


Implementei uma interface onde o personal trainer poderia associar cada aluno a um dispositivo de monitoramento. Durante a configuração inicial do treino, o professor faria essa associação, permitindo o cálculo das métricas em tempo real.


### Cálculo do dispêndio energético


Além de mostrar a frenquencia cardiaca atual, calculo do dispendio energético também foi calculado pelos sistema. Foi usado a equação de [Keytel et al (2005)](https://www.researchgate.net/profile/Julia-Goedecke/publication/7777759_Prediction_of_energy_expenditure_from_heart_rate_monitoring_during_submaximal_exercise/links/09e415097a1fb80023000000/Prediction-of-energy-expenditure-from-heart-rate-monitoring-during-submaximal-exercise.pdf) para esse cálculo.


Para homens:


$$
DE (kcal) = {(-55.0969 +
           0.6309 * FC (bpm) +
           0.1988 * peso(kg) +
           0.2017 * idade) \over 4.184 * 60 * tempo (min)}
$$


Para mulheres:
$$
DE (kcal) = {(-20.4022 +
           0.4472 * FC (bpm) +
           0.1263 * peso(kg) +
           0.074 * idade) \over 4.184 * 60 * tempo (min)}
$$


onde:
DE = Dispêndio energético
FC = Frequência cardíaca


# Resultado obtido


![Resultado ant+](/images/resultado-antplus.png)


Após finalizar a implementação do frontend e backend, eu consegui entregar uma prova de conceito funcional para o cliente na academia. O sistema operava como esperado, permitindo o monitoramento cardíaco em tempo real.


Infelizmente, devido a limitações orçamentárias e à infraestrutura da academia, o projeto não seguiu adiante. No entanto, a experiência me proporcionou um aprendizado valioso sobre novas tecnologias, especialmente o ANT+, React e Electron.


Mesmo sem uma implementação comercial, o conhecimento adquirido valeu cada linha de código escrita.



