<template>
  <div class="task two position-relative">
    <div class="board_with_the_task" ref="board">
      <h3 class="title">{{ title }}</h3>
      <!--Задание 1-->
      <div class="d-flex flex-column gap-2" v-if="!dragIf ? !tableIf : false" ref="drag">
        <VueDraggable class="d-flex flex-column gap-2 overflow-visible position-static" v-for="container in containers"
                      ghost-class="drag-ghost" fallback-class="ghostDrag" chosen-class="drag-chosen"
                      v-model="container.element" draggable=".locked"
                      :group="container.element[0].locked ? 'locked' : 'shared'" @end="draggableTask"
                      animation="150">
          <div class="stylesDiv" v-for="item in container.element" :class="{'locked': !item?.locked}" v-text="item?.name" :key="item?.id"/>
        </VueDraggable>
      </div>

      <!--Памятка к заданию 1-->
      <div class="tableInf rounded-3 d-flex flex-column align-items-center justify-content-center" ref="table" v-if="!dragIf ? tableIf : false" style="width: 138vh; margin-right: .7vh; opacity: 0">
        <table class="overflow-hidden w-100 h-100">
          <tbody>
          <tr v-for="item in tableHint" :key="item.name">
            <th scope="row" class="p-3 border-white border-2">{{ item.name }}</th>
            <td class="p-3 text-start border-2 border-white">{{ item.description }}</td>
          </tr>
          </tbody>
        </table>
        <div class="but" @click="this.nextGame()" ref="butTable" style="font-size: 2rem; color: white; text-decoration: underline; margin-top: 2vh; opacity: 0">Перейти к следующему заданию</div>
      </div>
      <!--Задание 2-->
      <div v-if="dragIf" ref="doubleTask" class="d-flex w-100 align-content-center" style="opacity: 1">
        <table class="dragTable overflow-hidden w-25 h-100">
          <tbody class="d-flex flex-column justify-content-sm-between h-100">
          <tr v-for="(container, index) in tableTask" class="d-flex align-items-center" style="height: 11.5vh;" :key="index">
            <th scope="row" style="color: rgb(70, 90, 100); border-color: rgb(70, 90, 100);" :class="{'locked': !item?.locked}" class="p-1 w-100 border border-2 border-dark">{{ container.title }}</th>
            <td class="text-center fs-1 w-auto px-2">-</td>
          </tr>
          </tbody>
        </table>

        <table class="dragTable overflow-hidden w-75">
          <tbody class="h-100">
          <tr v-for="(container, index) in tableDrag" :key="index">
            <VueDraggable v-model="container.element" fallback-class="ghostDrag" ghost-class="drag-ghost" animation="200"
                          draggable=".locked" :group="container.element[0].locked ? 'locked' : 'unlocked'" @end="draggableTable">
              <th scope="row" v-for="item in container.element"
                  style="width: 105vh; color: rgb(70, 90, 100); border-color: rgb(70, 90, 100); height: 11.5vh; font-size: 1.2vw;"
                  :class="{'locked': !item?.locked}" class="p-1 border border-2 border-dark">{{ item.name }}</th>
            </VueDraggable>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import {VueDraggable} from 'vue-draggable-plus'

  export default {
    components:{VueDraggable},
    data(){
      return{
        title: '',

        items: [
            1, 2, 3, 4, 5
        ],

        task_two_answer: [
          {title: "Организационный момент, постановка цели и задачи", id: 0},
          {title: "Актуализация знаний", id: 1},
          {title: "Основная часть: усвоение новых знаний, проверка понимания", id: 2},
          {title: "Закрепление изученного материала", id: 3},
          {title: "Домашнее задание и рефлексия", id: 4}
        ],

        containers: [
          {element: [{id: 0, name: '', needId: 0, locked: false}]},
          {element: [{id: 1, name: '', needId: 0, locked: false}]},
          {element: [{id: 2, name: '', needId: 0, locked: false}]},
          {element: [{id: 3, name: '', needId: 0, locked: false}]},
          {element: [{id: 4, name: '', needId: 0, locked: false}]},
        ],

        tableHint: [
          {name: 'Организационный момент: 2 мин', description: 'Этап настраивает школьников на учебу. Сюда входит: приветствие, объявление важных новостей, постановка целей и задач урока.'},
          {name: 'Актуализация предыдущих знаний: 10 мин', description: 'На этом этапе необходимо вспомнить материал предыдущих уроков, чтобы продуктивно построить занятие. С этой целью проводится устный опрос.'},
          {name: 'Основная часть: 15 мин', description: 'На данном этапе объясняется новый материал. От качества объяснения зависит уровень усвоения материала, успеваемость учащихся и результат обучения. ' +
                'Можно воспользоваться различными приемами для объяснения нового материала: рассказ теории и разбор практических заданий вместе с классом, ' +
                'работа с учебником и выполнение упражнений, просмотр учебных видеороликов и т.д..'},

          {name: 'Закрепление изученного. Контроль усвоения: 15 мин', description: 'Основная функция этого этапа – выявление пробелов в знаниях учащихся по изученному материалу и возможность исправления этих недостатков.' +
                ' На этом этапе проводится самостоятельная работа.'},
          {name: 'Домашнее задание. Рефлексия: 3 мин', description: 'На данном этапе необходимо проанализировать – что дал урок, достигнута ли цель, которую вы поставили в начале урока? Записать домашнее задание.'},
        ],

        tableDragDesc: [
          {id: 0, description: '«Доброе утро, ребята! Давайте улыбнёмся друг другу и с хорошим настроением начнем урок. Тема урока написана на доске. Подумайте, чему вы сегодня научитесь?»'},
          {id: 1, description: '«На прошлом уроке мы с вами говорили об однокоренных словах. Кто может сказать, какие слова называются однокоренными?»'},
          {id: 2, description: '«Давайте посмотрим на доску. Вы видите группу однокоренных слов. Прочитайте их и скажите, в написании какого слова вы не сомневаетесь? Почему?»'},
          {id: 3, description: '«Для закрепления пройденного материала предлагаю вам выполнить небольшую самостоятельную работу. Время на выполнение - 15 минут. Я раздам вам листочки с заданием».'},
          {id: 4, description: '«Итак, ребята, мы с вами отлично поработали. Повторили материал прошлого урока. Изучили новую тему. Выполнили самостоятельную работу. Дома вам надо сделать два небольших упражнения для закрепления знаний по теме. Номера упражнений записаны на доске».'},
        ],

        tableTask: [
          {title: "Организационный момент: 2 мин", id: 0},
          {title: "Актуализация предыдущих знаний: 10 мин", id: 1},
          {title: "Основная часть: 15 мин", id: 2},
          {title: "Закрепление изученного. Контроль усвоения: 15 мин", id: 3},
          {title: "Домашнее задание. Рефлексия: 3 мин", id: 4}
        ],

        tableDrag: [
          {element: [{id: 0, needId: 0, locked: false, name: ''}]},
          {element: [{id: 1, needId: 0, locked: false, name: ''}]},
          {element: [{id: 2, needId: 0, locked: false, name: ''}]},
          {element: [{id: 3, needId: 0, locked: false, name: ''}]},
          {element: [{id: 4, needId: 0, locked: false, name: ''}]},
        ],

        taskTwoAnswerEnd: 0,
        item: 0,
        deranged:[],
        tableIf: false,
        dragIf: false,
        buttonShow: false,
      }
    },

    methods:{
      taskTwo(){
        this.title = "Расположите в правильном порядке основные этапы урока";
        this.derangeArray();
        Array.from(this.containers).forEach((item, index)=>{
          item.element[0].name = index + 1 + '. ' + this.deranged[index].title
          item.element[0].needId = this.deranged[index].id
          item.element[0].locked = false
        });

        this.derangeTable();
        Array.from(this.tableDrag).forEach((item, index)=>{
          item.element[0].name = this.deranged[index].description
          item.element[0].needId = this.deranged[index].id
          item.element[0].locked = false
        });
      },

      draggableTask(){
        //Основная логика передвижения элементов между массивами компонентов draggable
        this.containers.forEach((container, index) =>{
          if (container.element.length === 0){
            for (this.item = index; this.item  !== this.containers.length; this.item ++){
              if (!this.containers[this.item + 1].element[0].locked){
                container.element.splice(0, 0 , this.containers[this.item + 1].element[0])
                this.containers[this.item + 1].element.splice(0, 1)
                break;
              }
            }
          }else if(container.element.length === 2){
            for (this.item = index; this.item !== this.containers.length; this.item++){
              if (!this.containers[this.item].element[1].locked){
                this.containers[this.item + 1].element.splice(0, 0 , this.containers[this.item].element[1])
                this.containers[this.item].element.splice(1, 1)
                break;
              }else if(this.containers[this.item].element[1].locked){
                this.containers[this.item + 1].element.splice(0, 0 , this.containers[this.item].element[0])
                this.containers[this.item].element.splice(0, 1)
                break;
              }
            }
          }

          if (container.element[0]){
            container.element[0].id = index;
            container.element[0].name = `${index + 1}. ` + container.element[0].name.slice(2, container.element[0].name.length);
            if (container.element[0].needId === index){
              container.element[0].locked = true;
            }
          }
        });
        this.endTaskTwo()
      },
      endTaskTwo(){
        this.taskTwoAnswerEnd = 0
        this.containers.forEach((container, index)=>{
          if (index === container.element[0]?.needId) this.taskTwoAnswerEnd++
        });

        if (this.taskTwoAnswerEnd === 5){
          this.title = "Вы справились с заданием! Ознакомьтесь подробнее с каждым из этапов.";
          this.$refs.drag.style.transition = '1s';
          this.$refs.drag.style.opacity = '0';
          this.timer = setTimeout(()=>{
            this.tableIf = true;
            this.timer = setTimeout(()=>{
              this.$refs.table.style.transition = '1s';
              this.$refs.table.style.opacity = '1';
              this.timer = setTimeout(()=>{
                this.buttonShow = true;
                this.$refs.butTable.style.opacity='1'
              }, 2000);
            }, 1000)
          }, 1000)
        }
      },
      nextGame(){
        if (this.buttonShow){
          this.buttonShow = false;
          this.$refs.table.style.opacity = '0';
          this.timer = setTimeout(()=>{
            this.title = "К какому этапу урока относятся предложенные ситуации?"
            this.tableIf = false;
            this.dragIf = true;
            this.timer = setTimeout(()=>{
              this.$refs.doubleTask.style.transition = '1s';
              this.$refs.doubleTask.style.opacity = '1';
            }, 2000)
          }, 1000)
        }
      },
      derangeArray() {
        this.deranged = [...this.task_two_answer]; // Копируем исходный массив объектов

        function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            let j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]]; // Меняем местами элементы
          }
        }

        do {
          shuffle(this.deranged); // Перемешиваем массив
        } while (this.deranged.some((val, idx) => val === this.task_two_answer[idx])); // Проверяем, чтобы элементы не стояли на своих местах
      },
      derangeTable(){
        this.deranged = [...this.tableDragDesc]; // Копируем исходный массив объектов

        function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            let j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]]; // Меняем местами элементы
          }
        }

        do {
          shuffle(this.deranged); // Перемешиваем массив
        } while (this.deranged.some((val, idx) => val === this.tableDragDesc[idx])); // Проверяем, чтобы элементы не стояли на своих местах
      },
      draggableTable(evt){
        evt.target.style.opacity = '1';
        this.targetDiv = false;

        this.tableDrag.forEach((container, index) =>{
          if (container.element.length === 0){
            for (this.item = index; this.item  !== this.tableDrag.length; this.item ++){
              if (!this.tableDrag[this.item + 1].element[0].locked){
                container.element.splice(0, 0 , this.tableDrag[this.item + 1].element[0])
                this.tableDrag[this.item + 1].element.splice(0, 1)
                break;
              }
            }
          }else if(container.element.length === 2){
            for (this.item = index; this.item !== this.tableDrag.length; this.item++){
              if (!this.tableDrag[this.item].element[1].locked){
                this.tableDrag[this.item + 1].element.splice(0, 0 , this.tableDrag[this.item].element[1])
                this.tableDrag[this.item].element.splice(1, 1)
                break;
              }else if(this.tableDrag[this.item].element[1].locked){
                this.tableDrag[this.item + 1].element.splice(0, 0 , this.tableDrag[this.item].element[0])
                this.tableDrag[this.item].element.splice(0, 1)
                break;
              }
            }
          }

          if (container.element[0]){
            container.element[0].id = index;
            if (container.element[0].needId === index){
              container.element[0].locked = true;
            }
          }
        });
        this.endTableTwo();
      },
      endTableTwo(){
        this.taskTwoAnswerEnd = 0
        this.tableDrag.forEach((container, index)=>{
          if (index === container.element[0]?.needId) this.taskTwoAnswerEnd++
        });

        if (this.taskTwoAnswerEnd === 5){
          this.title = "Вы успешно справились с заданием!";
          this.timer = setTimeout(()=>{
            this.$emit("exit")
          }, 1000)
        }
      },
    },
  }
</script>

<style scoped>
    .task{
        width: 100%;
        height: 100vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        transition: 1s;
    }

    .task .board_with_the_task{
      padding: 5vh 8vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-evenly;
      position: relative;
      width: 149vh;
      height: 84vh;
      background-image: url("../../../public/images/Board_with_the_task_four.svg");
      background-size: cover;
      background-position-x: -1vh;
    }

    .task.two .board_with_the_task .stylesDiv{
      margin: 0;
      width: 100vh;
      height: 10vh;
      font-family: "Czizh", serif;
      background-color: #75b798;
      font-weight: 600;
      color: rgb(70, 90, 100);
      text-align: center;
      font-size: 4vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .task.two .board_with_the_task h3.title{
      font-family: "Czizh", serif;
      font-style: normal;
      font-size: 4.5vh;
      color: white;
      background: none;
      text-align: center;
      font-weight: 400;
      margin-bottom: 1vh;
    }

    .task.two .board_with_the_task .stylesDiv.locked{
      background-color: rgba(255, 255, 255, 0.8);
    }

    th,
    td{
      font-size: 1.3rem;
      font-weight: 400;
      color: white;
    }

    .dragTable th{
      display: flex;
      flex-direction: column;
      justify-content: center;
      width: 20rem;
      height: 100%;
      background-color: #75b798;
    }

    .dragTable td{
      width: 72rem;
    }

    th.locked{
      background-color: white;
    }

    .dragTable th,
    .dragTable td{
      font-size: 1.5rem;
    }

    .drag-ghost {
      opacity: 0.5; /* Прозрачность */
      background-color: rgba(0, 0, 0, 0.1); /* Фон для призрака */
      border: 3px dashed #333; /* Добавляем рамку */
      pointer-events: none; /* Чтобы нельзя было взаимодействовать */
    }

    .ghostDrag{
      opacity: 0 !important;
    }
</style>