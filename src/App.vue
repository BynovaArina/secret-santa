<template>
  <form v-on:submit="distribute" class="santa">
    <h1 class="santa__title">Тайный санта</h1>
    <div class="santa__container"
      v-for="(name, index) in names" 
      :key="index"
    > 
      <label class="santa__player-label">Игрок</label>
      <input class="santa__player" required v-model="names[index]" placeholder="Имя">
      <label class="santa__gift-label">хочет в подарок</label>
      <textarea class="santa__gift" required v-model="gifts[index]" placeholder="Подарок" />
    </div>
    <div>
      <button type="button" class="santa__button santa__button--add" @click="add">Добавить</button>
      <button type="button" class="santa__button santa__button--remove" @click="remove">Удалить</button>
    </div>
    <button class="santa__button santa__button--submit" type="submit" :disabled="names.length <= 1">Распределить</button>
    
    <div class="santa__results">
      <ul class="santa__list">
        <li class="santa__list-item"
          v-for="(gifter, index) in gifters"
          :key="index"
        >
          {{ gifter }} дарит 👉 {{ names[index] }} в подарок 🎁{{ gifts[index] }}
        </li>
      </ul>
    </div>
  </form>
</template>

<script>
import { shuffle } from 'lodash';

export default {
  name: 'App',
  methods: {
    add() {
      this.names.splice(this.names.length, 0, '');
      this.gifts.splice(this.gifts.length, 0, '');
    },
    remove(){
      this.names.splice(this.names.length - 1, 1);
      this.gifts.splice(this.gifts.length - 1, 1);
    },
    distribute(event) {
      event.preventDefault();
      let arrNames = shuffle(this.names);
      let gifters = [];
      for (let i = 0; i < this.names.length; i++) {
        let index = arrNames.findIndex((el) => el === this.names[i]);
        if (index !== -1) {
          arrNames.splice(index, 1);
        }
        let name = arrNames.splice(0, 1);
        gifters.push(name[0]);
        if (index !== -1) {
          arrNames.push(this.names[i]);
        }
        arrNames = shuffle(arrNames);
      }

      if (gifters[gifters.length - 1] === undefined) {
        gifters[gifters.length - 1] = gifters[gifters.length - 2];
        gifters[gifters.length - 2] = this.names[this.names.length - 1];
      }

      this.gifters = gifters;
    }
  },
  components: {
  },
  data() {
    return {
      names: ['Геннадий', 'Петя', 'Александр'],
      gifts: ['Полотенце', 'Носки', 'Картошку'],
      gifters: []
    }
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-size: 18px;
  color: #2c3e50;
}
#app {
  width: 100%;
  min-height: 100vh;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  flex-direction: column;
}
.santa {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 15vh;
  box-shadow: 0px 3px 3px -2px rgb(0 0 0 / 20%), 0px 3px 4px 0px rgb(0 0 0 / 14%), 0px 1px 8px 0px rgb(0 0 0 / 12%);
  border-radius: 0.5em;
  padding: 2em;
}
.santa__title {
  font-family: inherit;
  font-size: 1.5em;
  font-weight: normal;
  margin: 0;
  margin-bottom: 1em;
  
}
.santa__container {
  display: flex;
  align-items: center;
  margin-bottom: 1em;
}
.santa__player-label {
  margin-right: 1em;
}
.santa__player {
  font: inherit;
  border: none;
  border-bottom: 1px solid;
  padding: 0.5em;
  width: 150px;
}
.santa__gift-label {
  margin: 0px 1em;
}
.santa__gift {
  font: inherit;
  resize: none;
  border: none;
  border-bottom: 1px solid black;
  padding: 0.5em;
}
.santa__button {
  font: inherit;
  background: none;
  border: none;
  border: 1px;
  padding: 0.5em 1em;
  margin: 0.5em;
  color: rgba(0, 0, 0, 0.589);
  cursor: pointer;
}
.santa__button--add {
  background-color: rgba(246, 255, 246, 0.507) ;
  border: 1px solid rgb(205, 230, 205) ;
  border-radius: 1000px;
}
.santa__button--remove {
  background-color: rgba(247, 240, 240, 0.356) ;
  border: 1px solid rgb(248, 221, 221) ;
  border-radius: 1000px;
}
.santa__button--submit {
  background-color: rgba(223, 239, 250, 0.288) ;
  border: 1px solid rgba(191, 221, 245, 0.658) ;
  border-radius: 1000px;
}
.santa__list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.santa__list-item {
  margin-top: 1em;
}
</style>
