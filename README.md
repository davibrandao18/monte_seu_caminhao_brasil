# Monte Seu Caminhao - Brasil

## Case
A Monte seu Caminhão, é uma montadora de caminhões, seus caminhões são fabricados sob-demanda, ou seja, são fabricados somente quando já estão vendidos para seus clientes. Isso significa, que em alguns períodos do ano, a produção é maior do que em outros. Dado esse cenário, A Monte seu Caminhão, tem uma alta demanda de contratação nos períodos de Janeiro e Fevereiro e uma alta demissão em Julho e Dezembro dos seus funcionários CTD(Contrato por tempo determinado)
Com esse cenário, o time de gestão de acessos tem uma dificuldade grande em revogar os acessos desses funcionários, dado que para cada funcionário desligado são geradas requisições de revogação de acessos. No dia-a-dia, são geradas cerca de 100-200 requisições por dia e o sistema de controle de acesso consegue processar todas no mesmo dia, nos períodos de maior volume são geradas cerca de 700 requisições em um só dia, e o sistema processa tudo em cerca de 3 dias. Isso pode gerar uma inconsistência em uma auditoria visto que o processo exige que os acessos sejam revogados no mesmo dia que o funcionário é demitido (D+0)
O sistema local foi desenvolvido em 2006 e renovado pela última vez em 2015 na linguagem .Net, e está on premisses nos servidores da empresa e desacoplar significa ter que conectar diversos outros serviços que hoje já estão integrados no monolito e isso custaria muito dinheiro e tempo, na atual arquitetura, o sistema integra com o ActiveDirectory para fazer a gestão de grupos de AD e com o Oracle para fazer a gestão de grupos de Banco. Toda requisição criada na aplicação conta com uma outra aplicação chamada Scheduler e outra chamada ControlM para organizar as requisições e fazer com que sejam processadas, ambas tecnologias falham em organizar as requisições em FIFO(first-in,first-out), então as requisições adotam ordens aleatórias e é difícil acompanhar as que já foram finalizadas e prever qual a próxima a ser executada.
Olhando para o lado de contratação, não há problemas, já que não importa em quanto tempo a requisição é processada, como são funcionários de linha de produção, não necessitam que os acessos sejam processados instantaneamente e, além disso, a contratação é feita por dois sistemas de mercado, um para RH e outro para gestão de identidades, o sistema local apenas realiza a captura o ID gerado por esse último.
A opção de trocar todas as capacidades para a ferramenta usada na Matriz não é viável, pois na ferramenta inhouse vários processos já estão estabelecidos que a Matriz ainda não usa e estão em desenvolvimento, portanto a opção de manter a ferramenta inhouse ainda é necessária para os próximos 3 anos.

## Missão Visão Valores

**Missão**
Nossa missão é produzir caminhões de alta qualidade que superem as expectativas dos nossos clientes, contribuindo para a eficiência do transporte e logística global, com foco na inovação, sustentabilidade e segurança.
**Visão**
Nossa visão é ser reconhecida mundialmente como a líder em soluções de transporte, fornecendo produtos de vanguarda que impulsionem a mobilidade e o desenvolvimento econômico de maneira sustentável.
**Valores**
**Qualidade:** Comprometemo-nos a entregar produtos de excelência que garantam a satisfação e a confiança dos nossos clientes.
**Inovação:** Valorizamos a inovação contínua para desenvolver tecnologias avançadas que atendam às necessidades dinâmicas do mercado.
**Sustentabilidade:** Priorizamos práticas sustentáveis em todas as etapas da nossa cadeia de produção, reduzindo o impacto ambiental e promovendo a responsabilidade social.
**Segurança:** A segurança é fundamental em todos os nossos produtos e operações, visando proteger nossos colaboradores, clientes e a comunidade.
**Integridade:** Conduzimos nossos negócios com transparência, ética e respeito, construindo relações de confiança com todas as partes interessadas.
**Excelência Operacional:** Buscamos a eficiência e a melhoria contínua em todos os nossos processos para oferecer valor superior aos nossos clientes.
**Colaboração:** Promovemos um ambiente de trabalho colaborativo, valorizando a diversidade e incentivando o desenvolvimento pessoal e profissional dos nossos colaboradores.
