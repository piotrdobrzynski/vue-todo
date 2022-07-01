<template>
  <div class="container">
    <div class="add-item">
      <div class="add-icon" @click="addItem">
        <i class="fas fa-plus"></i>
      </div>
      <form @submit.prevent="addItem">
        <input type="text" placeholder="Add a to-do..." v-model="entry">
      </form>
      <div class="feature-icon" :class="{'gold-bg': entryFavorite}">
        <i v-if="!entryFavorite" class="far fa-star" @click="entryFavorite = !entryFavorite"></i>
        <i v-else class="fas fa-star" @click="entryFavorite = !entryFavorite"></i>
      </div>
    </div>
    <div class="todo-list">
      <div class="item" :class="{'show': item.show}" v-for="(item, index) in todoList" :key="index">
        <div class="item-checkbox">
          <i v-if="!item.complete" class="fas fa-square" @click="completeItem(item)"></i>
          <i v-else class="fas fa-check-square"></i>
        </div>
        <div class="item-title">
          {{ item.title }}
        </div>
        <div class="feature-icon" :class="{'gold-bg': item.favorite}">
          <i v-if="!item.favorite" class="far fa-star" @click="setFavoriteItem(item)"></i>
          <i v-else class="fas fa-star" @click="item.favorite = !item.favorite"></i>
        </div>
      </div>
    </div>
    <div class="show-completed" v-if="completedToDoList.length > 0">
      <div class="button" @click="showCompletedList = !showCompletedList">
        <span v-if="!showCompletedList">show</span><span v-else>hide</span>
        completed to-do's
      </div>
    </div>
    <div class="todo-list complete-list" v-if="showCompletedList">
      <div class="item" :class="{'show': item.show}" v-for="(item, index) in completedToDoList" :key="index">
        <div class="item-checkbox">
          <i v-if="!item.complete" class="fas fa-square"></i>
          <i v-else class="fas fa-check-square" @click="uncompleteItem(item)"></i>
        </div>
        <div class="item-title">
          {{ item.title }}
        </div>
        <div class="feature-icon" :class="{'gold-bg': item.favorite}">
          <i v-if="!item.favorite" class="far fa-star"></i>
          <i v-else class="fas fa-star"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { setTimeout } from 'timers';
  export default {
    data: () => {
      return {
        entry: '',
        entryFavorite: false,
        todoList: [],
        completedToDoList: [],
        showCompletedList: false
      }
    },
    methods: {
      addItem() {
        if(this.entry !== '') {
          let date = new Date;
          let newEntry = {
            id: date.getTime(),
            title: this.entry,
            favorite: this.entryFavorite,
            complete: false,
            show: false
          }

          if(newEntry.favorite) {
            // push new item on first position
            this.todoList.splice(0, 0, newEntry);
          }
          else {
            // push new item on last position
            this.todoList.push(newEntry);
          }

          setTimeout(() => {
            this.todoList.find(element => element.id === newEntry.id).show = true;
          }, 10);
        }

        this.entry = '';
        this.entryFavorite = false;
      },
      setFavoriteItem(item) {
        item.favorite = !item.favorite;
        item.show = false;

        setTimeout(() => {
          let index = this.todoList.findIndex(element => element.id === item.id);
          this.todoList.splice(index, 1);
          this.todoList.splice(0, 0, item);
          item.show = true;
        }, 500);
      },
      completeItem(item) {
        item.complete = !item.complete;
        item.show = false;

        setTimeout(() => {
          this.completedToDoList.push(item);
          let index = this.todoList.findIndex(element => element.id === item.id);
          this.todoList.splice(index, 1);
          item.show = true;
        }, 500);
      },
      uncompleteItem(item) {
        item.complete = !item.complete;
        item.show = false;

        setTimeout(() => {
          if(item.favorite) {
            this.todoList.splice(0, 0, item);
          }
          else {
            this.todoList.push(item);
          }

          let index = this.completedToDoList.findIndex(element => element.id === item.id);
          this.completedToDoList.splice(index, 1);
          item.show = true;
        }, 500);
      }
    }
  }
</script>

<style>

  .container {
       padding: 10px;
       width: calc(100% - 20px);
       min-height: calc(100% - 20px);
  }
   .container .add-item {
       display: grid;
       grid-template-columns: 70px auto 70px;
       grid-template-rows: 60px;
       width: 100%;
       height: 60px;
       background: rgba(0, 0, 0, .4);
       border-radius: 30px;
       overflow: hidden;
  }
   .container .add-item .add-icon {
       grid-column-start: 1;
       grid-column-end: 1;
       display: flex;
       justify-content: center;
       align-items: center;
       color: #fff;
       font-size: 2rem;
       cursor: pointer;
  }
   .container .add-item input {
       grid-column-start: 2;
       grid-column-end: 2;
       background: none;
       border: none;
       width: 100%;
       height: 60px;
       color: #fff;
       font-size: 1.6rem;
  }
   .container .add-item input::placeholder {
       color: #fff;
  }
   .container .add-item input:focus {
       outline: none;
  }
   .container .add-item .feature-icon {
       grid-column-start: 3;
       grid-column-end: 3;
       display: flex;
       justify-content: center;
       align-items: center;
       color: #fff;
       font-size: 1.6rem;
       margin: 0 15px;
       cursor: pointer;
  }
   .container .todo-list {
       padding-top: 40px;
  }
   .container .todo-list .item {
       display: grid;
       grid-template-columns: 70px auto 70px;
       grid-template-rows: 60px;
       width: 100%;
       height: 60px;
       margin-bottom: 10px;
       background: rgba(255, 255, 255, .8);
       border-radius: 30px;
       overflow: hidden;
       opacity: 0;
       transition: all 0.5s linear;
  }
   .container .todo-list .item .item-checkbox {
       grid-column-start: 1;
       grid-column-end: 1;
       display: flex;
       justify-content: center;
       align-items: center;
       font-size: 1.6rem;
       color: #c31432;
       cursor: pointer;
  }
   .container .todo-list .item .item-title {
       grid-column-start: 2;
       grid-column-end: 2;
       display: flex;
       justify-content: flex-start;
       align-items: center;
       font-size: 1.6rem;
  }
   .container .todo-list .item .feature-icon {
       grid-column-start: 3;
       grid-column-end: 3;
       display: flex;
       justify-content: center;
       align-items: center;
       color: #333;
       font-size: 1.6rem;
       margin: 0 15px;
       cursor: pointer;
  }
   .container .todo-list .show {
       opacity: 1;
  }
   .container .complete-list .item {
       position: relative;
       background: rgba(255, 255, 255, .3);
  }
   .container .complete-list .item::before {
       content: '';
       position: absolute;
       top: 30px;
       width: calc(100% - 120px);
       margin: 0 60px;
       border-bottom: 2px solid #555;
  }
   .container .show-completed {
       display: flex;
       justify-content: center;
       align-items: center;
       padding-top: 20px;
  }
   .container .show-completed .button {
       color: #fff;
       padding: 10px;
       font-size: 1.3rem;
       text-transform: uppercase;
       background: #f2b400;
       border-radius: 30px;
       overflow: hidden;
       cursor: pointer;
  }
   .container .gold-bg {
       color: #f2b400 !important;
  }

</style>
