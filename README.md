<div align="center">
  <img src="https://user-images.githubusercontent.com/74321890/228393527-9bd20785-93b0-4da2-b774-97e81e59e6e4.svg" width="40%">
</div>

![Badge](https://img.shields.io/badge/STATUS-EM_ANDAMENTO-yellow?style=flat-square&logo=)


## Tabela de Conteúdos

- [Tabela de Conteúdos](#tabela-de-conteúdos)
- [Descrição](#descrição)
- [Cliente](#cliente)
- [Ilustração das Funcionalidades](#ilustração-das-funcionalidades)
- [Demonstração](#demonstração)
- [Epic das Sprints](#epic-das-sprints)
- [Backlog do Produto](#backlog-do-produto)
- [Tecnologias](#tecnologias)
- [Integrantes](#integrantes)
- [Cronograma da API](#cronograma-da-api)

## Descrição

<p align="justify">
O software Cloud-In é uma aplicação orquestradora de transferência automática de arquivos entre sistemas de armazenamento online. Por meio de sua interface minimalista e interativa, o usuário consegue cadastrar suas credenciais e configurar transferências conforme sua necessidade, dando início a jornada de download e upload entre os storages.

## Cliente

<p align="justify">
A MidAll nasceu para simplificar a jornada de evolução do seu negócio, visando alcançar qualquer visão estratégica. Nossa missão é preparar negócios para o futuro em uma nova era de disrupções de mercado somadas aos desafios pós pandemia. Acreditamos que Tecnologia, Dados e Inovação orientados para a geração de valor ao cliente são o ambiente de negócios perfeito para promissores resultados.

## Ilustração das Funcionalidades

Para acessar nosso *protótipo*, clique [aqui](https://www.figma.com/proto/HTiqfRS44iny1eBx9loAVv/Cloud-In?page-id=8%3A17&node-id=188-177&viewport=-1283%2C35%2C0.19&scaling=min-zoom&starting-point-node-id=188%3A177).

## Demonstração

Para acessar a playlist do projeto, clique [aqui](https://www.youtube.com/watch?v=AGRvBq9Xq4U&list=PLUOBqJKbljZsvHbaHWKrQ3z0l9l2Uo_f0):

[<img src="https://user-images.githubusercontent.com/74321890/228991716-687c07f9-3b6a-4cea-b855-677b51b2b20a.svg" width="60%" height="60%">](https://www.youtube.com/watch?v=AGRvBq9Xq4U&list=PLUOBqJKbljZsvHbaHWKrQ3z0l9l2Uo_f0 "Cloud-in vídeo Demonstração")

## Epic das Sprints

| Sprint | Epic |
| -------| --------- |
| Sprint 1 | Transação manual de arquivos, Notificações e Autenticação nos drives |
| Sprint 2 | Transação automática e Coleta de metadados |
| Sprint 3 | Dashboard de metadados |
| Sprint 4 | Configurações personalizadas da transação |

## Backlog do Produto

- [X] ![Epic](https://user-images.githubusercontent.com/89356780/229957736-64a40537-3607-421a-afdd-e581db9e55ea.svg) **SPRINT 1:**  Funcionalidades Básicas
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Autenticação com o S3
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Autenticação com o Google drive
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Template da aplicação
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Operações de arquivos do S3 (download, upload e listagem)
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Operações de arquivos do Google drive (download, upload e listagem)
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Transferência de arquivos individuais
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Alertas e notificações
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Histórico de transferências
- [X] ![Epic](https://user-images.githubusercontent.com/89356780/229957736-64a40537-3607-421a-afdd-e581db9e55ea.svg) **SPRINT 2:**  Transação automática de arquivos e coleta de metadados
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Salvar credenciais para transação automática
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Pesquisa recorrente ao drive
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Transação automática
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Coleta de metadados de tamanho e tempo na transferência
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Resposta da transferência automática para o frontend
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Padronização de rotas
- [X] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Testes de unidade e de integração nas funções de transação
- [ ] ![Epic](https://user-images.githubusercontent.com/89356780/229957736-64a40537-3607-421a-afdd-e581db9e55ea.svg) **SPRINT 3:**  Dashboard de metadados
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Coleta de metadados armazenados no backend
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Armazenamento de metadados na aplicação local
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Dashboard em Power BI para visualização dos dados
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Implementação do power BI no frontend
- [ ] ![Epic](https://user-images.githubusercontent.com/89356780/229957736-64a40537-3607-421a-afdd-e581db9e55ea.svg) **SPRINT 4:**  Configurações personalizadas da transação
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Configurar tempo de pesquisa recorrente ao drive de origem
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Configurar banda usada na transação
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Configurar tipo de transferência
- [ ] ![Story](https://user-images.githubusercontent.com/89356780/229957815-ea747c93-b861-40c7-8a2d-bc43c1b2973a.svg) Setup da aplicação desktop

## Tecnologias

<img src="https://img.shields.io/badge/-Vue.js-4FC08D?logo=Vue.js&logoColor=white&style=for-the-badge" alt="badge"> <img src="https://img.shields.io/badge/-Flask-000000?logo=Flask&logoColor=white&style=for-the-badge" alt="badge">  <img src="https://img.shields.io/badge/-MySQL-4479A1?logo=MYSQL&logoColor=white&style=for-the-badge" alt="badge">

## Integrantes

 - Betriz Medeiros (PO)
 - Pedro Motta (SM)
 - Abraão Henrique (DEV)
 - Hamilton Zanini (DEV)
 - Kauã Borgarelli (DEV)
 - Renata Garcia (DEV)
 - Victor Cavichioli (DEV)
 
Para mais informações[^2], clique [aqui](https://github.com/DolphinDatabase/Cloud-In/wiki/Development-Team).

## Cronograma da API

| Data | Evento |
| -------| --------- |
| 13/02 a 03/03 | Kick-off. |
| 23/03 a 02/04 | [Sprint 1](Sprints/SPRINT1.md). |
| 03/04 a 23/04 | Sprint 2 |
| 24/04 a 14/05 | Sprint 3 |
| 15/05 a 04/06 | Sprint 4 |
| 13/06 e 14/06 | Feira de Soluções. |

[^1]: Vídeo produzido e editado pelos integrantes do grupo.
[^2]: Equipe responsável pelo desenvolvimento do Projeto Integrador.
