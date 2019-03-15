<template>
  <div id="app">
    <nav>
      <ul class="nav-section">
        <li @click="isShow = true">Dodaj +</li>
      </ul>
    </nav>

    <section id="toDoTasks">
      <div class="toDoTask" v-for="task in currData">
        <h3 :style="{'background-color':task.color}">{{ task.name }}<font-awesome-icon :icon="['fas','times']" class="fontTimes" @click = 'removeTask($event, task.id)'/></h3>
        <p class="date" :style="{'background-color':task.color}">{{ task.day }}</p>
        <p class="note" :style="{'border-color':task.color}" v-html="$options.filters.lineWrap(task.note)"></p>
      </div>
    </section>
    
    <section id="content" v-if="currData.length == 0">
      <h6>Aplikacja ToDo służy do zapisywania swoich planów na określony przez Ciebie czas.</h6>
      <h1>Dodaj swoje zadanie</h1>
    </section>

    <div id="add" v-if="isShow">
      <div class="add-field">
        <label for="taskName"><p>Tytuł</p><input v-model="toDoName" type="text" name="taskName" placeholder="Tytuł..."/></label>
        <label>
          <p>Dzień:</p>
          <select v-model="toDoDay">
            <option v-for="day in dayName">{{ day[0] }}</option>
          </select>
        </label>
        <label for="taskNote"><p>Notka</p><textarea rows="7" name="taskNote" v-model="toDoNote"></textarea></label>
        <button class="close-task" @click="isShow = false"><font-awesome-icon :icon="['fas','times']"/></button>
        <button class="add-task" @click="addTask()" v-if="toDoName && toDoNote">Dodaj</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      isShow: false,
      dayName: [
        ['Poniedziałek','#0b4eba'],
        ['Wtorek','#0fbc25'],
        ['Środa','#d12727'],
        ['Czwartek','#d19526'],
        ['Piątek','#d125cb'],
        ['Sobota','#e0d61f'],
        ['Niedziela','#4c4c4c']
      ],
      toDoId: 1,
      toDoDay: 'Poniedziałek',
      toDoNote: '',
      toDoName: '',
      toDoColor: '#0b4eba',
      currData: []
    }
  },
  methods: {
    addTask(){
      for(var key of this.dayName) if(key[0] == this.toDoDay) this.toDoColor = key[1];

      this.currData.push({
        id: this.toDoId,
        name: this.toDoName,
        note: this.toDoNote,
        day: this.toDoDay,
        color: this.toDoColor
      });

      this.toDoId++;
      this.toDoDay= 'Poniedziałek';
      this.toDoNote = '';
      this.toDoName = '';
      
      this.isShow = false;
    },
    removeTask(e, task){
      for(var i=0; i < this.currData.length; i++){
        if(this.currData[i].id == task){
          this.currData.splice(i, 1);
        }
      }
    }
  },
  filters:{
    lineWrap(value){
      var list = value.split("\n");
      var currList = list.join('</br>');

      return currList;
    }
  }
}
</script>

<style scoped lang="scss">
  *{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  #app{
    width: 100%;
    height: 100vh;
    background-color: #ddd;
    position: relative;
  }

  #add{
    height: 100%;
    width: 100%;
    background-color: rgba(68, 68, 68, .5);
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    display: flex;
    justify-content: center;
    align-items: center;

    .add-field{
      width: 500px;
      height: 480px;
      background-color: white;
      border-radius: 10px;
      position: relative;
    }

    .add-field label{
      display: block;
      width: 95%;
      margin: auto;
      
      input,textarea{
        width: 100%;
        padding: 10px;
        border-radius: 10px;
        outline: none;
        border: none;
        box-shadow: 0 0 0 1px #7a7a7a;

        &:focus{
          box-shadow: 0 0 3px 1px #0057c9;
        }
      }

      select{
        width: 100%;
        padding: 10px;
        border-radius: 10px;
        outline: none;
      }

      p{
        padding: 10px 0;
        font-size: 14px;
        font-weight: 500;
        color: #0057c9;
      }
    }

    .close-task{
      position: absolute;
      top: 5px;
      right: 10px;
      background-color: transparent;
      border: none;
      outline: none;
    }

    .add-task{
      display: block;
      margin: 20px auto 0;
      background-color: #0057c9;
      border: none;
      color:white;
      padding: 7px 10px;
      border-radius: 7px;

      &:hover{
        background-color: #216dd1;
      }
    }
  }

  #toDoTasks{
    padding-top: 25px;
    text-align: center;

    .toDoTask{
      width: 250px;
      display: inline-block;
      vertical-align: top;
      margin: 0 10px 10px;
      position: relative;

      .fontTimes{
        font-size: 16px;
        position: absolute;
        top: 5px;
        right: 10px;
        cursor: pointer;
      }

      h3{
        color: white;
        border-radius: 5px 5px 0 0;
        text-align: center;
        word-wrap: break-word;
        padding: 20px 5px 5px;
      }

      .date{
        color: white;
        text-align: center;
      }

      .note{
        text-align: center;
        padding: 10px;
        border: 2px solid;
        min-height: 50px;
        word-wrap: break-word;
      }
    }
  }

  nav{
    height: 40px;
    background-color: #01132d;

    .nav-section li{
      color: #ddd;
      float: left;
      list-style: none;
      height: 40px;
      padding: 0 15px;
      line-height: 2.6;
      cursor: pointer;
      text-align: center;
      font-size: 14px;

      &:hover{
        background-color: #083989;
      }
    }
  }

  #content{
    position: absolute;
    bottom: 100px;
    left: 0;
    right: 0;
    margin: auto;

    h1{
      text-align: center;
      font-size: 34px;
      color: #444;
    }

    h6{
      text-align: center;
      color: #444;
      width: 700px;
      margin: auto;
    }
  }
</style>
