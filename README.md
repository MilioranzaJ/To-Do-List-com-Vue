# To-Do List com Vue.js

Uma aplicação web simples e reativa para gerenciamento de tarefas, desenvolvida com **Vue 3** para praticar conceitos de desenvolvimento front-end e explorar o uso de **VUE**.

Este projeto demonstra a construção de uma interface interativa, manipulação de estado reativo e persistência de dados no navegador.

---

## Funcionalidades

- **Adicionar tarefas**  
  Insira novas tarefas pressionando `Enter` ou clicando no botão de adicionar.

- **Marcar como concluída**  
  Ao marcar uma tarefa como concluída, ela é automaticamente riscada.

- **Remoção automática**  
  Após ser concluída, a tarefa é removida automaticamente após **3 segundos**.

- **Remoção manual**  
  Possibilidade de excluir tarefas individualmente.

- **Paginação**  
  A lista exibe **até 5 tarefas por página**, mantendo a interface organizada.

- **Persistência de dados**  
  As tarefas são salvas no **LocalStorage**, evitando perda de dados ao atualizar a página.

- **Animações suaves**  
  Transições visuais ao adicionar, concluir ou remover tarefas.

---

## Tecnologias Utilizadas

- **Vue 3**
- **Composition API** (`ref`, `computed`, `watch`, `onMounted`, `<script setup>`)
- **Vite**
- **JavaScript (ES6+)**
- **HTML5**
- **CSS3 (Flexbox)**

---

## Objetivo do Projeto

Este projeto foi desenvolvido com o objetivo de praticar conceitos fundamentais do ecossistema **Vue**, incluindo:

- gerenciamento de estado reativo
- criação de componentes
- manipulação de eventos
- persistência de dados no navegador
- organização de interfaces

---

## Como executar o projeto

Certifique-se de ter o **Node.js** instalado.

### Clonar o repositório

git clone https://github.com/seu-usuario/nome-do-repositorio.git

Entrar na pasta do projeto

cd nome-do-repositorio

Instalar dependências

npm install

Rodar o servidor de desenvolvimento

npm run dev

Depois disso, abra no navegador:

http://localhost:5173


📌 Melhorias Futuras

Edição de tarefas

Filtro de tarefas (pendentes / concluídas)

Melhor responsividade para mobile

Integração com backend

Sistema de autenticação de usuários

📄 Licença

Este projeto foi desenvolvido para fins de estudo e aprendizado.
