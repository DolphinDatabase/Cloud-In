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

Para acessar o vídeo[^1] de demonstração da aplicação em uso, clique [aqui](https://youtu.be/AGRvBq9Xq4U):

[<img src="https://user-images.githubusercontent.com/74321890/228991716-687c07f9-3b6a-4cea-b855-677b51b2b20a.svg" width="60%" height="60%">](https://youtu.be/AGRvBq9Xq4U "Cloud-in vídeo Demonstração")

## Epic das Sprints

| Sprint | Epic |
| -------| --------- |
| Sprint 1 | Transação manual de arquivos, Notificações e Autenticação nos drives |
| Sprint 2 | Transação automática e Coleta de metadados |
| Sprint 3 | Dashboard de metadados |
| Sprint 4 | Configurações personalizadas da transação |

## Backlog do Produto

- [X] ![EPIC](Imagens/Epic.svg) **SPRINT 1:**  Funcionalidades Básicas
- [X] ![STORY](Imagens/Story.svg) Autenticação com o S3
- [X] ![STORY](Imagens/Story.svg) Autenticação com o Google drive
- [X] ![STORY](Imagens/Story.svg) Template da aplicação
- [X] ![STORY](Imagens/Story.svg) Operações de arquivos do S3 (download, upload e listagem)
- [X] ![STORY](Imagens/Story.svg) Operações de arquivos do Google drive (download, upload e listagem)
- [X] ![STORY](Imagens/Story.svg) Transferência de arquivos individuais
- [X] ![STORY](Imagens/Story.svg) Alertas e notificações
- [X] ![STORY](Imagens/Story.svg) Histórico de transferências
- [ ] ![EPIC](Imagens/Epic.svg) **SPRINT 2:**  Transação automática de arquivos e coleta de metadados
- [ ] ![STORY](Imagens/Story.svg) Salvar credenciais para transação automática
- [ ] ![STORY](Imagens/Story.svg) Pesquisa recorrente ao drive
- [ ] ![STORY](Imagens/Story.svg) Transação automática
- [ ] ![STORY](Imagens/Story.svg) Coleta de metadados de tamanho e tempo na transferência
- [ ] ![STORY](Imagens/Story.svg) Resposta da transferência automática para o frontend
- [ ] ![STORY](Imagens/Story.svg) Padronização de rotas
- [ ] ![STORY](Imagens/Story.svg) Testes de unidade e de integração nas funções de transação
- [ ] ![EPIC](Imagens/Epic.svg) **SPRINT 3:**  Dashboard de metadados
- [ ] ![STORY](Imagens/Story.svg) Coleta de metadados armazenados no backend
- [ ] ![STORY](Imagens/Story.svg) Armazenamento de metadados na aplicação local
- [ ] ![STORY](Imagens/Story.svg) Dashboard em Power BI para visualização dos dados
- [ ] ![STORY](Imagens/Story.svg) Implementação do power BI no frontend
- [ ] ![EPIC](Imagens/Epic.svg) **SPRINT 4:**  Configurações personalizadas da transação
- [ ] ![STORY](Imagens/Story.svg) Configurar tempo de pesquisa recorrente ao drive de origem
- [ ] ![STORY](Imagens/Story.svg) Configurar banda usada na transação
- [ ] ![STORY](Imagens/Story.svg) Configurar tipo de transferência
- [ ] ![STORY](Imagens/Story.svg) Configurar tipo de transferência
- [ ] ![STORY](Imagens/Story.svg) Setup da aplicação desktop

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
