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
<p align="justify">
Para a ferramenta de gestão decidimos utilizar o Jira, ele é estruturado com Épicos/ Estórias/ Tarefas.
 
Estórias, no nosso caso, são equivalente a issues, onde conseguimos fazer integração com o github utilizando seu id.
 
Definimos que todo commit deve referenciar sua issue com o id da sua respectiva estória.

### pre-commit
<p align="justify">
Para dar o commit utilizamos o pre-commit onde definimos que todo commit deve começar com a sigla padrão das storys (CLD-) e o número da sua story (varia a cada story), assim o usuário passa um id e se esse id for válido, o commit ocorre normalmente.

```
git commit -m "CLD-109 tempo de transferência"
black....................................................................Passed
```

Caso o id não seja válido, recebemos um aviso para corrigir e o commit não é concluído.
```
git commit -m "teste"
[INFO] Initializing environment for https://github.com/ambv/black.
[INFO] Installing environment for https://github.com/ambv/black.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
black....................................................................Failed
```

