# Monte Seu Caminhão - Brasil

## Case
A Monte seu Caminhão, é uma montadora de caminhões, seus caminhões são fabricados sob-demanda, ou seja, são fabricados somente quando já estão vendidos para seus clientes. Isso significa, que em alguns períodos do ano, a produção é maior do que em outros. Dado esse cenário, A Monte seu Caminhão, tem uma alta demanda de contratação nos períodos de Janeiro e Fevereiro e uma alta demissão em Julho e Dezembro dos seus funcionários CTD(Contrato por tempo determinado)

Com esse cenário, o time de gestão de acessos tem uma dificuldade grande em revogar os acessos desses funcionários, dado que para cada funcionário desligado são geradas requisições de revogação de acessos. No dia-a-dia, são geradas cerca de 100-200 requisições por dia e o sistema de controle de acesso consegue processar todas no mesmo dia, nos períodos de maior volume são geradas cerca de 700 requisições em um só dia, e o sistema processa tudo em cerca de 3 dias. Isso pode gerar uma inconsistência em uma auditoria visto que o processo exige que os acessos sejam revogados no mesmo dia que o funcionário é demitido (D+0)

O sistema local foi desenvolvido em 2006 e renovado pela última vez em 2015 na linguagem .Net, e está on premisses nos servidores da empresa e desacoplar significa ter que conectar diversos outros serviços que hoje já estão integrados no monolito e isso custaria muito dinheiro e tempo, na atual arquitetura, o sistema integra com o ActiveDirectory para fazer a gestão de grupos de AD e com o Oracle para fazer a gestão de grupos de Banco. Toda requisição criada na aplicação conta com uma outra aplicação chamada Scheduler e outra chamada ControlM para organizar as requisições e fazer com que sejam processadas, ambas tecnologias falham em organizar as requisições em FIFO(first-in,first-out), então as requisições adotam ordens aleatórias e é difícil acompanhar as que já foram finalizadas e prever qual a próxima a ser executada.

Olhando para o lado de contratação, não há problemas, já que não importa em quanto tempo a requisição é processada, como são funcionários de linha de produção, não necessitam que os acessos sejam processados instantaneamente e, além disso, a contratação é feita por dois sistemas de mercado, um para RH e outro para gestão de identidades, o sistema local apenas realiza a captura o ID gerado por esse último.

A opção de trocar todas as capacidades para a ferramenta usada na Matriz não é viável, pois na ferramenta inhouse vários processos já estão estabelecidos que a Matriz ainda não usa e estão em desenvolvimento, portanto a opção de manter a ferramenta inhouse ainda é necessária para os próximos 3 anos.
<details>
  <summary>Respostas</summary>
## Respostas
### O que esperamos aprender com esse projeto?
Neste projeto queremos desenvolver nossa capacidade de obtenção de requisitos e análise de requisitos, sejam eles funcionais ou não funcionais.

### Que perguntas precisamos que sejam respondidas?
- Quem são os stakeholders?
- Quais as restrições desse projeto?
- Quais as tecnologias envolvidas?
- Como é o as-is?
- Qual o volume de requisições?
- É um serviço que roda sob demanda ou ele tem um job?
- Quem dará suporte a este serviço?

### Quais são os nossos principais riscos?
- Tempo de projeto
- Diretriz global de mudanças
- Limites de hardware 
- Capacidade da equipe de desenvolvimento
- Não atender a volumetria
- Restrições financeiras
### Crie um plano para aprender o que precisamos para responder a perguntas específicas.
- Buscar mais informações sobre a aplicação as-is.
- Brainstorm com a equipe
- Fazer um desenho da arquitetura atual para validação e entendimento junto aos envolvidos
### Crie um plano para reduzir riscos.
- Observar períodos de freezing.
- Estruturar cronograma de entregas para respeitar o freezing e o tempo de projeto (cascata).
- Levantamento das capacidades do time desenvolvimento para composição da equipe técnica.
- Levantamento dos volumes de requisições históricos.
- Análise de viabilidade técnica (hardware).
### Quem são as partes interessadas?
- Time de acessos 
- Time de segurança 
- Diretoria
- Compliance
### O que eles esperam ganhar?
- Confiabilidade no processo de remoção de acessos;
- Conformidade com diretrizes de auditoria;
- Cumprimento do prazo de revogação previsto em processo.
### Quem são os usuários?
- Time de acessos
### O que eles estão tentando realizar?
- Otimizar tempo de processamento;
- Prestação de contas em auditoria;
- Revogações de acesso dentro do prazo (na data de desligamento do colaborador).
### Qual o pior que pode acontecer?
- Gerar um “falso positivo" e o usuário continuar com os acessos mesmo após o desligamento e esse acesso ser descoberto durante o processo de auditoria.
- Levar ainda mais tempo para processar as requisições.


### Descreva os requisitos que você(s) considera importante e por quê?
- Disponibilidade: A aplicação precisa ficar alwayson, um dia da aplicação fora do ar, já pode causar o discumprimento do prazo.
- Manutenibilidade: A aplicação precisa existir e receber possível alteração nos próximo 3 anos.
- Domínio tecnológico: Garantir que as tecnologias usadas na solução sejam do domínio dos profissionais que irão sustentar a aplicação.
- Performance: Garantir que os acessos tenham sido de fato revogados ao fim do processo.
- Observabilidade: Garantir que o serviço está funcionando e as requisições estão sendo processadas.

### Sobre o que o diagrama ajuda você a raciocinar/pensar?

O diagrama é essencial para trazer uma visão do todo, a construção dele implica também em entender junto ao stakeholder possíveis pontos de dor e requisitos que não haviam sido mapeados no primeiro momento, como aconteceu em nosso caso, pois após montarmos o diagrama, descobrimos que era super importante o sistema ter uma ferramenta de observabilidade acoplada a esse processo, evidenciando a saúde da aplicação. Também foi importante saber a ordem dos acontecimentos e tentar identificar qual seria o ponto de melhoria mais assertivo para o fluxo.

### Quais são os padrões essenciais no diagrama?
Camadas, pub/sub

### Existem padrões ocultos?
Padrão de camadas e Padrão de conexão no ActiveDirectory e ControlM pelo servidor da Suécia.

### Qual é o Metamodelo?
N/A

### Pode ser discernido no diagrama único?
Sim, é uma arquitetura simples e com poucos componentes. Portanto, um único diagrama comporta todos os seus artefatos.

### O diagrama está completo?
Sim, baseado nos conhecimentos que temos do business e da tecnologia, todos os atores e artefatos estão diagramados.

### Poderia ser simplificado e ainda assim ser eficaz?
Entendemos que já está simples o suficiente.

### Houve alguma discussão importante que vocês tiveram como equipe?
Claro, para definir se partiríamos para uma arquitetura de eventos usando pub/sub ou fila.

### Que decisões sua equipe teve dificuldade para tomar?
Se iríamos manter os banco de dados usando CQRS para separar a view do write.

### Que decisões foram tomadas sob incerteza?
partindo de premissas e padrões que já são adotados pelo cliente.

### Houve algum ponto de decisão sem retorno que o forçou a desistir de uma determinada escolha?
Sim, a utilização de serviços em cloud.
</details>
# Diagramas
## Free Form
### As-Is
<p align="center">
  <img alt="Diagrama As-Is" src="root/imagens/Diagramas-Arquitetura AS-IS.drawio.png">
  </p>
  
  ### To-Be
<p align="center">
  <img alt="Diagrama To-Be" src="root/imagens/Diagramas-Arquitetura TO-BE.drawio.png">
  </p>
  
## C4
### Contexto
<p align="center">
  <img alt="Diagrama As-Is" src="root/imagens/Diagrama de Contexto.png">
  </p>

### Container
<p align="center">
  <img alt="Diagrama As-Is" src="root/imagens/Diagrama de Container.png">
  </p>
  
### Componente
<p align="center">
  <img alt="Diagrama As-Is" src="root/imagens/Diagrama de Componente.png">
  </p>

### Link para o Video
<a href="(https://youtu.be/rLqJvXZ4s5o)"<a/>
