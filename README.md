# Projeto de BI - Gestão de Jornada e Pendências de RH
Este projeto foi desenvolvido para auxiliar no acompanhamento de indicadores operacionais da gestão de RH de uma empresa.

## Contexto

Na empresa X, havia um problema da gestão de RH/ADM com a jornada de pontos dos seus colaboradores. Enquanto a empresa crescia em número de contratações, a gestão não conseguia acompanhar bem a jornada de trabalho do quadro de colaboradores mesmo a empresa usando o *software* do Tangerino.

Como consequência disso, havia um alto número de pessoas que não cumpriam sua jornada de trabalho corretamente com erros de excesso de horas extras trabalhadas, problemas com o horário de almoço, interjornada e até na quantidade de pontos batidos diariamente. Por conta disso, a empresa estava sofrendo com a falta de advertências devidas e até com processos trabalhistas de seus próprios colaboradores.

Para resolver esse problema, precisei criar uma solução de BI capaz de dar visibilidade à gestão a respeito desses indicadores operacionais e criar automações capazes de detectar inconsistências na jornada de pontos dos colabaradores e enviar alertas individualmente.


## O cenário envolvia:

- Time administrativo pouco familiarizado com acompanhamento de indicadores;
- Falta de controle com as horas extras dos colaboradores;
- Integração com API (Tangerino);
- Processamento de regras complexas de jornada;
- Geração automatizada de alertas individuais.


## Objetivo

Construir uma solução de BI capaz de:

- Monitorar a jornada de trabalho diária;
- Identificar irregularidades automaticamente;
- Automatizar geração de disparos de mensagens de pendências aos colaboradores;
- Fornecer visão analítica para gestão;
- Determinar elegibilidade de trabalho (Pode/Não Trabalhar) --> Nas alterações futuras


## Arquitetura da Solução

 - Fonte de dados
    1. SharePoint (arquivos Excel)
    2. API REST (Tangerino)
       
  - ETL
    1. Power Query (Dataflow Gen1)
    2. Transformações com M

  - Modelagem
    1. Star Schema
    2. Tabelas fato e dimensões

  - Camada analítica
    1. DAX (medidas e regras de negócio)
       
  - Visualização
    1. Power BI Dashboard

  - Automação
    1. Power Automate (geração de arquivos e integração)


## Principais Ferramentas Utilizadas

- Power Query (M) -> Principalmente no tratamento e manipulação das múltiplas fontes de dados (ETL);
- DAX -> Para a criação dos cálculos de regras de jornada e na criação de medidas condicionais
- Power BI Desktop/Service -> Para a criação do dashboard, nas configurações de autenticação de fonte de dados, atualização automática dos relatórios e na criação de relatórios paginados.
- API RESTful -> API da empresa Solides Tangerino usada para testar e validar os *end-points* a serem usados na captura de dados pelo Power Query.
- Power Automate -> Usado na geração de arquivos (.xlsx) para armazenamento das pendências dos colaboradores por dia e no disparo automático de pendências aos colaboradores com problemas na jornada de pontos via email e Whatsapp.



## Dashboard
A seguir, está descrito cada página construída do *dashboard* desenvolvido. Elementos como logo e informações pessoais foram anonimizados a fim de servir como demonstração.

### Capa
Resumo sobre o que se trata o relatório.

<img width="1371" height="747" alt="1" src="https://github.com/user-attachments/assets/b16fd39f-3d75-45ea-bf0f-da19770ce8e0" />


### Menu de Navegação
Botões clicáveis de navegação de página.

<img width="1372" height="742" alt="2" src="https://github.com/user-attachments/assets/726fd96b-2a50-4f32-811e-6cf0b5f5d6c0" />

### Visão Geral
Esta página foi desenvolvida para que o usuário consiga navegar de forma rápida e intuitiva a respeito da situação dos colabores do seu setor ao longo dos dias do mês selecionado. Para isso foram desenvolvidos:
- Botões de navegação de páginas;
- Dois filtros de segmentação de dados para ano e mês;
- Um visual de matriz cujas colunas representam os dias do mês selecionado e o valor numérico correspondente indica a quantidade de colaboradores com irregularidades (A escala de cores do azul para o vermelho indica, visualmente, o quão crítico o dia está em relação ao volume de pessoas com pendências);
- Um visual de matriz em que nas linhas se encontram os setores e os colaboradores daquele setor, enquanto que as colunas exibem as horas extras autorizadas/indevidas, horas de almoço, interjornada e jornada de pontos;
- Um visual de cartão que mostra, em detalhes, os pontos batidos daquele colaborador naquele dia selecionado.

Para esses indicadores operacionais as regras utilizadas são:
- As horas extras autorizadas exibem, no formato *hh:mm*, o quantitativo de horas que o colaborador tem disponível e autorizado para o dia selecionado;
- As horas extras indevidas exibem, no formato *hh:mm*, o quantitativo de horas extras que o colaborador realizou no dia selecionado;
- As horas de almoço tem uma regra de serem feitas em uma hora, com uma tolerância de mais ou menos 5 minutos;
- As horas de interjornada (intervalo de tempo entre o fim de uma jornada de trabalho e início da próxima) devem ser de, no mínimo, uma hora;
- A jornada de pontos deve ser de 4 pontos diários com excessão para estagiário/jovem aprendiz (2 pontos diários).

Caso as regras sejam cumpridas dentro do esperado, é exibido uma sinalização verde, caso contrário é exibido uma sinalização vermelha. Entretanto para os casos que fogem do padrão esperado mas não descumprem totalmente as regras, é exibido uma sinalização amarela para que o usuário investigue melhor o que aconteceu. Confirmada a infração, o usuário pode considerar como pendência do colaborador após a avaliação.

<img width="1371" height="720" alt="3" src="https://github.com/user-attachments/assets/8aa96f40-8e05-45ec-9ab7-1764ce2ddfbc" />


### Métricas

Nesta página, o objetivo é verificar quais o setores precisam de mais atenção em cada um dos indicadores operacionais. Para isso, foi estabelecida uma métrica chamada de *registro de trabalho*. Essa métrica quantifica a somatória dos dias efetivamente trabalhados de todos os colaboradores de cada setor. Estabeleceu-se que em pelo menos 90% das vezes não houvessem pendências na jornada de trabalho dos colaboradores de cada setor. Caso esse índice fosse acima de 90%, o indicador mostraria uma barra azul, caso contrário ficaria vermelha.

Esse visual foi direcionalmente pensado a fim do usuário identificar qual a prioridade de correção do setor que administra.<img width="1331" height="800" alt="4" src="https://github.com/user-attachments/assets/8d358dc7-6309-4229-ae1e-f4ffd54119f9" />


### Pendências Globais

Esta página foi desenvolvida para que o usuário verifique exatamente quais colaboradores e quais pendências estão sendo acionadas para seu setor em um período de tempo. Dessa forma é possível acompanhar mais de perto quem está acionando as pendências, quando isso está acontecendo e qual a gravidade dessas ocorrências.

<img width="1553" height="801" alt="5" src="https://github.com/user-attachments/assets/8b89eb1c-85c4-4d40-b912-657510220868" />





## Disparo Automático de Mensagens

Como forma de auxiliar o time de RH/ADM, foi desenvolvido um fluxo de disparos diários de mensagens por email e Whatsapp aos colaboradores e gestores de cada setor.

A regra consistia em que a cada dia, fossem verificadas as pendências do dia anterior para trás de todos os colaboradores. Essas pendências deveriam ser organizadas por dia. A cada 6 dias consecultivos de envio de mensagens exclusivas ao colaborador e o mesmo não corrigiu, as mensagens deveriam ser disparadas ao seus respectivo gestor em cópia. Toda a automação do disparo automático de mensagens foi desenvolvida usando o Power Automate


<img width="423" height="713" alt="1" src="https://github.com/user-attachments/assets/de2de9a5-ec2a-4d83-b186-b060097e5649" />


Como resultado disso, os colaboradores recebiam uma mensagem via Whastapp e outra por email conforme o exemplo a seguir:

<img width="938" height="775" alt="whats_1" src="https://github.com/user-attachments/assets/37b674a9-4881-4262-8e6c-a21173baeeed" />


<img width="1310" height="706" alt="gmail_1" src="https://github.com/user-attachments/assets/6d344445-a5a0-4ac4-a232-7ed560460fcf" />


## Resultados Obtidos
A partir do projeto de BI desenvolvido, vale apontar os principais resultados obtidos:


1. Redução do tempo do time de RH/ADM para a identificação e tratamento de irregularidades;
2. Redução de análise manual da jornada de pontos;
3. Aumento da confiabilidade dos dados pelos usuários;
4. Menor volume de colaboradores com pendências excessivas;
5. Processo totalmente automatizado.
   


## Observações

Os dados foram anonimizados para fins de portfólio.



