<template>
  <div class="tableWr">
    <div class="tableTitle">
      <p>Имя</p>
      <p>Тип</p>
      <p>Цвет</p>
    </div>
    <ul class="style-none">
      <li v-for="(item,index) of arrComp" v-bind:key="item.name" class="list-item">
        <button class="btn btn-top" @click="top(index)"></button>
        <button class="btn btn-bot" @click="bot(index)"></button>
        <input class="inp" type="text" :placeholder="item.name" v-model="arr[index][name]">
        <input class="inp inp-type" type="text" :placeholder="item.type" v-model="arr[index].type">
        <input class="inp visible" type="text" :placeholder="item.color" v-model="arr[index][color]">
        <verte picker="square" model="rgb" v-model="item.color"></verte>

        <button class="close" @click="clearItem(index)"></button>
      </li>
    </ul>
    <button class="btn saveBtn" v-if="saveNewItem" @click="saveItem">Сохранить изменения</button>
    <div class="addWr">
      <input class="nameIn inp" type="text" v-model="name" placeholder="name">
      <input class="typeIn inp" type="text" v-model="type" placeholder="type">
      <verte picker="square" model="rgb" v-model="color"></verte>

      <button class="addBtn btn" @click="addString">Добавить</button>
    <div v-if="showAlert" class="alert">

      <p>Изменения добавлены в localStorage</p>
    </div>
    </div>
  </div>
</template>

<script>
import Verte from 'verte';
import 'verte/dist/verte.css';

export default {
  components: {
    Verte
  },
  data () {
    return {
      showAlert: false,
      table: [{name: '', type: '', coloe: ''}],
      name: '',
      color: '#603C3C',
      type: '',
      tableName: '',
      saveNewItem: true,
      arr: [
        {name: 'name1', type : 'main', color: '#603C3C'},
        {name: 'name2', type : 'side', color: '#603fff'},
        ]
    }
  },

  computed: {
    arrComp () {
      return this.arr
    }
  },

  methods: {
    // test () {
    //   console.log('color=' + this.color + ' ' + 'colour=' + this.colour)
    //   this.colour = '#603C3C'
    // },
    addString () {
      if (this.name && this.type && this.color) {
        this.arr.push({name: this.name, type: this.type, color: this.color})
      }
      console.log(this.table)
    },
    clearItem (index) {
      this.arr.splice(index, 1)
    },
    top(index) {
      let newIndex = this.arr.splice(index, 1)
      this.arr.splice(index - 1, 0, newIndex[0])
    },
    bot(index) {
      let newIndex = this.arr.splice(index, 1)
      this.arr.splice(index + 1, 0, newIndex[0])
    },
    saveItem() {
      localStorage.setItem ("object", JSON.stringify(this.arr))
      this.showAlert = true
      setTimeout(()=>{ this.showAlert = false }, 2000)
    }
  },

  watch: {  
    arr () {
      this.saveNewItem = true
    }
  },
  mounted () {
      // localStorage.setItem ("object", JSON.stringify(this.arr))
      if (localStorage.getItem('object'))
      this.arr = JSON.parse(localStorage.getItem('object'));
  }
}
</script>

<style scoped>
  .tableTitle {
    display: flex;
    justify-content: space-between;
    margin-left: auto;
    padding-left: 20px;
    padding-right: 30px;
    margin-bottom: 10px;
  }

  .tableTitle p {
    font-weight: 700;
  }

  .tableWr {
    background: #0b1116;
    background: radial-gradient(
          circle at top left,
          #2b3036,
          #0e1317
        );;
    width: 320px;
    margin: 0 auto;
    padding: 20px;
    box-sizing: border-box;
    box-shadow: 10px 30px 55px rgba(0,0,0,.4);
    transition: 1s ease;

  }

  .style-none {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .visible {
    display: none;
  }

  .list-item {
    display: flex;
    /* flex-wrap: wrap; */
    justify-content: space-between;
    position: relative;
    padding-left: 20px;
    margin-bottom: 15px;
  }

  .list-item input {
    width: 60px;
  }

  .addWr {
    display: flex;
    
  }

  .btn {
    background-color: transparent;
    border: none;
    outline: none;
    font-family: 'Roboto', sans-serif;
  }

  .close {
    background-color: transparent;
    border: none;
    outline: none;
    background-image: url('../assets/urn.svg');
    width: 20px;
    height: 20px;
    background-size:auto;
    background-repeat: no-repeat;
    background-position: center;
    cursor: pointer;
  }

  .close:hover {
    opacity: 0.6;
  }

  .addBtn {
    color: #2c3e50;
    transition: 0.5s ease;
    cursor: pointer;
  }

  .addBtn:hover {
    color: #00bff2;
  }

  .saveBtn {
    color: #2c3e50;
    transition: 0.5s ease;
    margin-bottom: 20px;
    cursor: pointer;
  }

  .saveBtn:hover {
    color: #00bff2;
  }

  .btn-wr {
    display: block;
    position: relative;
  }

  .btn-top {
    position: absolute;
    display: block;
    background-image: url('../assets/top.svg');
    background-repeat: no-repeat;
    width: 15px;
    height: 20px;
    cursor: pointer;
    left: 0;
    top: 5px;
  }
  .btn-bot {
    position: absolute;
    display: block;
    background-image: url('../assets/bot.svg');
    background-repeat: no-repeat;
    width: 15px;
    height: 20px;
    cursor: pointer;
    top: 22px;
    left: 0;
  }

  .btn-bot:hover {
    opacity: 0.5;
  }

  .btn-top:hover {
    opacity: 0.5;
  }

  .btn-bot:active {
    opacity: 1;
  }

  .btn-top:active {
    opacity: 1;
  }

  .nameIn {
    width: 80px;
  }

  .typeIn {
    width: 80px;
  }

  .inp {
    height: 30px;
    margin-right: 10px;
    background-color: transparent;
    outline: none;
    border: none;
    transition: 0.5s ease;
    padding-left: 5px;
    color: #2c3e50;
  }
  
  .inp:hover {
    outline: 1px solid #00bff2;
  }

  .inp:active {
    outline: 1px solid #00bff2;
  }

  .inp-type {
    width: 60px;
  }

  .alert {
    /* position: relative; */
    transition: 1s ease;
  }

  .alert p { 
    display: block;
    position: absolute;
    font-family: 'Roboto', sans-serif !important;
    font-weight: 200; 
    font-size: 13px;
    -webkit-animation: alert-txt 3.2s cubic-bezier(.02,.54,.2,1.01);
    animation: alert-txt 3.2s cubic-bezier(.02,.54,.2,1.01);
    -webkit-animation-fill-mode: both;
    animation-fill-mode: both;
    top: -8px;
    left: 0;
    width: 100%; height: 30px;
    background:radial-gradient(ellipse at top, rgba(128,255,26,.5) 0%, transparent 70%) no-repeat;
    /* background-position-y: -15px; */
    transition: 1s ease;
  }

  .alert p { color: #80ff1a; }

  .alert {
  }


  @media (min-width: 768px) {
    .tableWr {
    width: 768px;
    padding: 50px;
    }
    .tableTitle {
    padding-left: 80px;
    padding-right: 280px;
    }
    .inp {
      width: 100px !important;
    }
    .list-item {
      margin-left: 55px;
      margin-right: 120px;
    }

    .saveBtn {
      margin-right: 190px;
    }

    .addWr {
      margin-left: 120px;
      width: 200px;
    }

    .addBtn {
      margin-left: 20px;
    }

    .visible {
      display: block;
    }
     .nameIn {
      width: 80px;
    }

    .typeIn {
      width: 80px;
    }

    .close {
      width: 30px;
      height: 25px;
    }
  }

</style>