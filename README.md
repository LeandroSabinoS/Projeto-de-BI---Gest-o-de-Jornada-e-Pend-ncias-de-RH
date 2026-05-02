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


## Principais Ferramentas Utilizadas

- Power Query (M) -> Usado principalmente no tratamento e manipulação das múltiplas fontes de dados;
- DAX -> Usado na criação dos cálculos de regras de jornada e na criação de medidas condicionais
- Power BI Desktop/Service -> Usado na criação do dashboard, nas configurações de autenticação de fonte de dados, atualização automática dos relatórios e na criação de relatórios paginados.
- Power Automate -> Usado na geração de arquivos (.xlsx) para armazenamento das pendências dos colaboradores por dia e no disparo automático de pendências aos colaboradores com problemas na jornada de pontos via email e Whatsapp.
