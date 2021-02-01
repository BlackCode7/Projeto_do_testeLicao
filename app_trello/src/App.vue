<template>
  <!--
  https://www.youtube.com/watch?v=YmJzRpMFdDY
  https://developer.mozilla.org/pt-BR/docs/DragDrop/Drag_and_Drop
  http://blog.sw9.com.br/2018/11/20/drag-e-drop-com-html5-aula-1-apresentacao-e-feature-detection-sw9/
  https://www.youtube.com/watch?v=6wn8hpUcEcM
  https://www.youtube.com/results?search_query=sistema+de+cadastro+crud+com+java+web

  menu vertical
  https://www.devmedia.com.br/criando-um-menu-vertical-dropdown-com-css-e-html/37207

  Selenium
  https://www.selenium.dev/documentation/pt-br/webdriver/browser_manipulation/

  tutorial do youtube para aprender parada no minuto 33:50
  -->
  <div id="app">
    <Header />
    <!--Construção dos Cards Aqui!-->
    <div class="board">
      <div class="lane">
        <h2 class="lane-title">BackLog</h2>
        <!-- Metodos criado para arrastar e soltar @drag-start="handleDragStart", @drop="handleDrop" -->
        <Container group-name="trello"         
        @drag-start="handleDragStart('backlogs', $event)"
        @drop="handleDrop('backlogs', $event)">
          <Draggable v-for="card in cards.backlogs" :key="card.id">
            <Cards>
              {{ card.text }}
            </Cards>
          </Draggable>
        </Container>
      </div>

      <div class="lane">
        <!-- Metodos criado para arrastar e soltar @drag-start="handleDragStart", @drop="handleDrop" -->
        <Container group-name="trello"
        @drag-start="handleDragStart('desenvolvimento', $event)"
        @drop="handleDrop('desenvolvimento', $event)">
          <h2 class="lane-title">Desenvolvimento</h2>
          <!-- Elemento de arrastar e soltar Draggable -->
          <Draggable v-for="card in cards.desenvolvimento" :key="card.id">
            <Cards>
              {{ card.text }}
            </Cards>
          </Draggable>
        </Container>
      </div>

      <div class="lane">
        <!-- Metodos criado para arrastar e soltar @drag-start="handleDragStart", @drop="handleDrop" -->
        <Container group-name="trello"
        @drag-start="handleDragStart('testes', $event)"
        @drop="handleDrop('testes', $event)">
          <h2 class="lane-title">Testes</h2>
          <!-- Elemento de arrastar e soltar Draggable -->
          <Draggable v-for="card in cards.testes" :key="card.id">
            <Cards>
              {{ card.text }}
            </Cards>
          </Draggable>
        </Container>
      </div>

      <div class="lane">
        <h2 class="lane-title">Concluídos</h2>
        <!-- Elemento de arrastar e soltar Draggable -->
        <!-- Metodos criado para arrastar e soltar @drag-start="handleDragStart", @drop="handleDrop" -->
        <Container group-name="trello"
        @drag-start="handleDragStart('concluidos', $event)"
        @drop="handleDrop('concluidos', $event)"
        :get-child-payload="getChildPayLoad">
          <Draggable v-for="card in cards.concluidos" :key="card.id">
            <Cards>
              {{ card.text }}
            </Cards>
          </Draggable>
        </Container>
      </div>
    </div>
  </div>
</template>

<script>
//importando de header vue
import Header from "./components/Header";
//importado de Cards.vue
import Cards from "./components/Cards";
//importando de initialCards.js o cards não é um component
// entao ele fica dentro do data{}
import initialCards from "./initialCards";
// importando lib para arrastar e soltar Draggable
import { Container, Draggable } from "vue-smooth-dnd";

export default {
  name: "App",
  // Aqui declaramos os componentes que criamos em components
  // e joga o component no html do <template>
  components: {
    Header,
    Cards,
    Container,
    Draggable
  },
  data: () => ({
    cards: {
      // acessando os conteudos dos cards
      backlogs: initialCards.backlogs,
      desenvolvimento: initialCards.desenvolvimento,
      testes: initialCards.testes,
      concluidos: []
    },
    //controle do card que esta sendo arrastado
    draggingCard:{
      lane:"",
      index: -1,
      cardData: {}
    }
  }),
  // criando metodos arrastar e soltar
  methods: {
    //Metodos criado para arrastar e soltar @drag-start="handleDragStart", @drop="handleDrop"
    handleDragStart(lane, dragResult){
      const { payload, isSource } = dragResult;
      if (isSource) {
        this.draggingCard = {
          lane,
          index: payload.index,
          cardData: {
            ...this.cards[lane][payload.index],
          }
        }        
      }      
    },
    //Metodos criado para arrastar e soltar @drag-start="handleDragStart", @drop="handleDrop"
    handleDrop(lane, dropResult){
      const { removedIndex, addedIndex } = dropResult
      console.log(lane, dropResult, removedIndex)
    },
    //Função de recebe os indeces da sua coluna
    getChildPayLoad(index){
      return{
        index,
      }
    },
  },
};
</script>

<style>
/*Estilo dos cards*/
.board {
  display: flex;
  justify-content: flex-start;
  margin: 1.2rem 0 0.8rem;
}
.lane {
  background: var(--color-grey);
  width: 23rem;
  height: 30rem;
  border-radius: 0.8rem;
  box-shadow: 0 0.1rem 0.2rem 0 rgba(33, 33, 33, 0.1);
  margin: 0 0.8rem;
  padding: 0 0.7rem;
}
.lane-title {
  padding: 0.8rem 0.5rem;
  margin-bottom: 0.6rem;
}
</style>
