# DevOps

## Tabela de Conteúdos

 * [GitFlow](#gitflow)
 * [Track Issue](#track-issue)
 
## GitFlow
<p align="justify">
Git Flow é um modelo de fluxo de trabalho que simplifica e organizar o versionamento das branches de um projeto de desenvolvimento no Git.
O Git Flow pode ser implementado em qualquer projeto de software e está alinhado diretamente à prática de DevOps de entrega contínua. o Git Flow atribui regras bem específicas para diferentes tipos de ramificações e define quando elas devem interagir. Além disso, conta com ramificações individuais para preparar, manter e registrar lançamentos. O Git Flow funciona através do uso de ramificações (branches) do projeto,

No nosso fluxo de trabalho, as branches estão sendo divididas em quatro tipos:
- Main
- Dev
- Feature
- Bugfix

**Main** é a branch principal que contém o código-fonte em produção que foi completamente testado e aprovado. Não é permitido realizar alterações (commit) diretamente na main, ela representa a linha de produção estável do nosso projeto. Os Commits nesta branch são feitos por meio de merges vindos da branch Dev e raramente da branch Bugfix.

**Dev** é a branch de desenvolvimento principal do nosso projeto, é criada a partir da Main. Ela deve sempre estar organizada, com os códigos testados, porém não necessariamente estável o suficiente para implantação em produção. Commits diretos na branch dev são evitados, e o trabalho é feito principalmente em branches de features geradas a partir desta.
Toda vez que realizar um pull request da feature para Dev, é testado o código a partir da pipeline de testes no Github Actions,  para que na main, seja armazenado o código testado e funcional. 

**Feature** é uma branch temporária criada a partir da branch Dev que carrega uma nova funcionalidade para o projeto, ela sempre acabará sendo implementada à própria Dev, através de merge e descartada ao cumprir seu propósito.

Caso um bug seja encontrado depois do merge, realizamos a aplicação da branch **Bugfix**, onde ele pode ser gerado tanto a partir da dev como da main. É investigado a causa do problema, aplicado as correções no código e posteriormente atualizando as branches principais.
<img src="https://github.com/DolphinDatabase/MCS/assets/74321890/6e4c8f8f-c1c9-4a12-b661-e4de9ff2a98a"/>
  
## Track Issue
