<template>
  <div class="task-cards-holder">
    <div class="task-cards-header">
      {{ category }}
    </div>
    <div class="task-cards-list">

      <div v-for="card in inProgressCards" class="card">
        <div class="task-card-control">
          <div class="task-card-control-item toToDo" @click="transferCardToToDo(card.id)">
            <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 400.004 400.004">
              <path d="M382.688,182.686H59.116l77.209-77.214c6.764-6.76,6.764-17.726,0-24.485c-6.764-6.764-17.73-6.764-24.484,0L5.073,187.757
               c-6.764,6.76-6.764,17.727,0,24.485l106.768,106.775c3.381,3.383,7.812,5.072,12.242,5.072c4.43,0,8.861-1.689,12.242-5.072
                c6.764-6.76,6.764-17.726,0-24.484l-77.209-77.218h323.572c9.562,0,17.316-7.753,17.316-17.315
                C400.004,190.438,392.251,182.686,382.688,182.686z"/>
            </svg>
          </div>
          <div class="task-card-control-item delete" @click="deleteCard(card.id), ejectInProgressCardById(card.id)">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path d="M 10 2 L 9 3 L 5 3 C 4.448 3 4 3.448 4 4 C 4 4.552 4.448 5 5 5 L 7 5 L 17 5 L 19
              5 C 19.552 5 20 4.552 20 4 C 20 3.448 19.552 3 19 3 L 15 3 L 14 2 L 10 2 z M 5 7 L 5 20 C
              5 21.105 5.895 22 7 22 L 17 22 C 18.105 22 19 21.105 19 20 L 19 7 L 5 7 z"/>
            </svg>
          </div>
          <div class="task-card-control-item toDone" @click="transferCardToDone(card.id)">
            <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 330 330" xml:space="preserve">
              <path id="XMLID_27_" d="M15,180h263.787l-49.394,49.394c-5.858,5.857-5.858,15.355,0,21.213C232.322,253.535,236.161,255,240,255
              s7.678-1.465,10.606-4.394l75-75c5.858-5.857,5.858-15.355,0-21.213l-75-75c-5.857-5.857-15.355-5.857-21.213,0
              c-5.858,5.857-5.858,15.355,0,21.213L278.787,150H15c-8.284,0-15,6.716-15,15S6.716,180,15,180z"/>
            </svg>
          </div>
        </div>
        <Card :id="card.id" :text="card.text"/>
      </div>

    </div>
  </div>
</template>

<script>
import Card from './Card.vue';
import {mapActions, mapMutations, mapState, mapGetters} from 'vuex';
export default {
  name: "InProgress",
  props: {
    category: String
  },
  computed: {
    ...mapState(['inProgressCards', 'globalCardIdRegistry', 'lastCard'])
  },
  created() {
    this.getInProgressCards();
  },
  data() {
    return {
      loadedCards: {}
    }
  },
  methods: {
    ...mapActions(['changeCardList', 'deleteCard']),
    ...mapGetters(['getInProgressCards']),
    ...mapMutations(['addToDoCard', 'incrementGlobalCardId', 'ejectInProgressCardById', 'addDoneCard']),
    transferCardToToDo(id){
      this.ejectInProgressCardById(id);

      this.changeCardList(this.lastCard, "ToDo"); //to server

      this.addToDoCard(this.lastCard);
    },
    transferCardToDone(id){
      this.ejectInProgressCardById(id);

      this.changeCardList(this.lastCard, "Done"); //to server

      this.addDoneCard(this.lastCard);
    },
  },
  components: {
    Card
  }

}
</script>

<style>

.task-cards-holder {
  width: 400px;
  height: 100%;
  margin: 0 auto;
}

.task-cards-header {
  width: 100%;
  //width: var(--default-card-width);
  margin: 20px auto 0;
}

.task-card-control {

}

.task-cards-list {
  width: 100%;
  overflow: auto;
  max-height: 80%;
}

.card {
  background: var(--default-background-card-color);
  min-height: 50px;
  width: var(--default-card-width);
  margin: 20px auto 0;
  border-radius: 5px;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.25);
}

</style>