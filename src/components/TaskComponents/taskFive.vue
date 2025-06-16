<template>
  <div class="task flex-row align-items-center pe-3 ps-3 pt-5">
    <div v-if="!taskFiveIf" ref="instructionTaskFive" class="d-flex flex-column w-50 align-items-center bg-white p-5 rounded-3">
      <h1 class="text-center fw-bold">Недельный план учителя начальных классов</h1>
      <p class="text-center mt-4 fs-3" style="font-family: 'Montserrat', sans-serif">
        Перед вами расписание учителя начальных классов на неделю.
        Оно помогает планировать своё время и учебную нагрузку. Заполните пропуски в расписании.
        Перетащите предложенные мероприятия в свободные «окошки».
      </p>
      <div class="bg-success rounded-1 px-3 py-1 fs-2 mt-4" @click="startFive()">Начать</div>
    </div>
    <div v-if="taskFiveIf" ref="dragDiv" style="opacity: 0; transition: 1s"
         class="d-flex flex-column justify-content-evenly align-items-center h-75 mt-2">
      <div style="width: 17rem;" class="me-4" ref="drag">
        <VueDraggable v-model="this.plans" fallback-class="ghostDrag" animation="200" ghost-class="drag-ghost"
                      draggable=".locked" group="table" @end="dragTable" class="home d-flex flex-column gap-4"
        >
          <div v-for="plan in plans" v-text="plan?.title" :key="plan?.id"
               @touchstart.passive="onStart" @touchmove.passive="touchMove" @touchend.passive="onEnd"
               :class="{'locked': !plan?.locked}" class="border-black border-2 fw-bold card p-2 ps-3 pe-3 fs-5"
               />
        </VueDraggable>
      </div>
    </div>

    <table v-if="taskFiveIf" ref="dragTable" style="opacity: 0; transition: 1s" class="overflow-hidden w-100 h-75">
      <caption class="fs-4">Расписание уроков 4"Б"</caption>
      <thead>
      <tr>
        <th scope="col" class="border-black border-0 border-bottom border-end border-2 text-center"></th>
        <th scope="col" class="border-black border-0 border-bottom border-end border-2 text-center">Понедельник</th>
        <th scope="col" class="border-black border-0 border-bottom border-end border-2 text-center">Вторник</th>
        <th scope="col" class="border-black border-0 border-bottom border-end border-2 text-center">Среда</th>
        <th scope="col" class="border-black border-0 border-bottom border-end border-2 text-center">Четверг</th>
        <th scope="col" class="border-black border-0 border-bottom border-2 text-center">Пятница</th>
      </tr>
      </thead>
      <tbody>
      <tr>
        <th scope="row" class="border-black border-0 border-bottom border-end border-2">1 урок</th>
        <td class="text-start border-2 border-black">
          <VueDraggable v-model="this.monday" :group="this.monday[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="monday d-flex flex-column justify-content-center">
              <p class="m-0" v-for="item in this.monday" :key="item.id" :class="{'locked': !item?.locked}" v-text="item.title"/>
          </VueDraggable>
        </td>
        <td class="text-start border-2 border-black">
          <VueDraggable v-model="this.tuesdayOne" :group="this.tuesdayOne[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="tuesdayOne h-100 d-flex flex-column justify-content-center">
              <p class="m-0" v-for="item in tuesdayOne" :key="item.id" :class="{'locked': !item?.locked}" v-text="item?.title"/>
          </VueDraggable>
        </td>
        <td class="text-start border-2 border-black">Русский язык</td>
        <td class="text-start border-2 border-black">Русский язык</td>
        <td class="text-start border-2 border-black border-end-0">Литературное чтение</td>
      </tr>
      <tr>
        <th scope="row" class="border-black border-0 border-bottom border-end border-2">2 урок</th>
        <td class="text-start border-2 border-black">Русский язык</td>
        <td class="text-start border-2 border-black">Русский язык</td>
        <td class="text-start border-2 border-black">Математика</td>
        <td class="text-start border-2 border-black">Окружающий мир</td>
        <td class="text-start border-2 border-black border-end-0">Математика</td>
      </tr>
      <tr>
        <th scope="row" class="border-black border-0 border-end border-bottom border-2">3 урок</th>
        <td class="text-start border-2 border-black">Математика</td>
        <td class="text-start border-2 border-black">Окружающий мир</td>
        <td class="text-start border-2 border-black">Литературное чтение</td>
        <td class="text-start border-2 border-black">ИЗО</td>
        <td class="text-start border-2 border-black border-end-0">Технология</td>
      </tr>
      <tr>
        <th scope="row" class="border-black border-0 border-end border-bottom border-2">4 урок</th>
        <td class="text-start border-2 border-black">Литературное чтение</td>
        <td class="text-start border-2 border-black">Литературное чтение</td>
        <td class="text-start border-2 border-black">Английский язык</td>
        <td class="text-start border-2 border-black">ОРКСЭ</td>
        <td class="text-start border-2 border-black border-end-0">Технология</td>
      </tr>
      <tr>
        <th scope="row" class="border-black border-0 border-end border-bottom border-2">5 урок</th>
        <td class="text-start border-2 border-black">Английский язык</td>
        <td class="text-start border-2 border-black text-center fw-bold fs-3" style="font-family: 'Roboto', sans-serif;">-</td>
        <td class="text-start border-2 border-black">Физическая культура</td>
        <td class="text-start border-2 border-black">Музыка</td>
        <td class="text-start border-2 border-black border-end-0">
          <VueDraggable v-model="this.fridayOne" :group="this.fridayOne[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="fridayOne h-100 d-flex flex-column justify-content-center">
            <p class="m-0" v-for="item in fridayOne" :key="item.id" :class="{'locked': !item?.locked}" v-text="item?.title"/>
          </VueDraggable>
        </td>
      </tr>
      <tr>
        <th scope="row" class="border-black border-0 border-end border-bottom border-2">6 урок</th>
        <td class="text-start border-2 border-black">Физическая культура</td>
        <td class="text-start border-2 border-black text-center fw-bold fs-3" style="font-family: 'Roboto', sans-serif;">-</td>
        <td class="text-start border-2 border-black text-center fw-bold fs-3" style="font-family: 'Roboto', sans-serif;">-</td>
        <td class="text-start border-2 border-black text-center fw-bold fs-3" style="font-family: 'Roboto', sans-serif;">-</td>
        <td class="text-start border-2 border-black border-end-0 text-center fw-bold fs-3" style="font-family: 'Roboto', sans-serif;">-</td>
      </tr>
      <tr>
        <th scope="row" class="border-black border-0 border-end border-2"></th>
        <td class="text-start border-2 border-black border-bottom-0">Педагогический совет</td>
        <td class="text-start border-2 border-black border-bottom-0 border-end-0">
          <VueDraggable v-model="this.tuesdayTwo" :group="this.tuesdayTwo[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="tuesdayTwo h-100 d-flex flex-column justify-content-center">
            <p class="m-0" v-for="item in tuesdayTwo" :key="item.id" :class="{'locked': !item?.locked}" v-text="item?.title"/>
          </VueDraggable>
        </td>
        <td class="text-start border-2 border-black border-bottom-0 border-end-0">
          <VueDraggable v-model="this.wednesday" :group="this.wednesday[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="wednesday h-100 d-flex flex-column justify-content-center">
              <p class="m-0" v-for="item in this.wednesday" :key="item.id" :class="{'locked': !item?.locked}" v-text="item.title"/>
          </VueDraggable>
        </td>
        <td class="text-start border-2 border-black border-bottom-0 border-end-0">
          <VueDraggable v-model="this.thursday" :group="this.thursday[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="thursday h-100 d-flex flex-column justify-content-center">
              <p class="m-0" v-for="item in this.thursday" :key="item.id" :class="{'locked': !item?.locked}" v-text="item.title"/>
          </VueDraggable>
        </td>
        <td class="text-start border-2 border-black border-bottom-0 border-end-0">
          <VueDraggable v-model="this.fridayTwo" :group="this.fridayTwo[0]?.locked ? 'notable' : 'table'" animation="200"
                        draggable=".locked" @end="dragTable" class="fridayTwo h-100 d-flex flex-column justify-content-center">
            <p class="m-0" v-for="item in fridayTwo" :key="item.id" :class="{'locked': !item?.locked}" v-text="item?.title"/>
          </VueDraggable>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import {VueDraggable} from "vue-draggable-plus";

  export default {
    components:{
      VueDraggable
    },

    data(){
      return {
        timer: '',
        interval: '',
        random_string: '',
        get_answer_one: '',
        type:'',

        plans: [],

        monday: [],
        tuesdayOne: [],
        tuesdayTwo: [],
        wednesday: [],
        thursday: [],
        fridayOne: [],
        fridayTwo: [],
        targetDiv: false,
        taskFiveIf: false,
        item: '',
        lastCall: 0,
        delay: 100,
      }
    },

    methods:{
      startFive(){
        this.$refs.instructionTaskFive.style.opacity = '0';
        this.timer = setTimeout(()=>{
          this.taskFiveIf = true;
          this.timer = setTimeout(()=>{
            this.$refs.dragDiv.style.opacity = '1';
            this.$refs.dragTable.style.opacity = '1';
          },500)
        }, 300)
      },
      taskFive(){
        this.taskFiveIf = false;
        this.monday = [];
        this.tuesdayOne = [];
        this.tuesdayTwo = [];
        this.wednesday = [];
        this.thursday = [];
        this.fridayOne = [];
        this.fridayTwo = [];
        this.plans = [
          {title: 'Разговоры о важном', id: 0, locked: false, day: ['monday'], text: 'Рекомендованное время для этой дисциплины – первый урок в понедельник'},
          {title: 'Внеклассное мероприятие', id: 1, locked: false, day: ['tuesdayTwo', 'wednesday', 'thursday'], text: 'В этот день лучше планировать другие мероприятия'},
          {title: 'Родительское собрание', id: 2, locked: false, day: ['tuesdayTwo', 'wednesday', 'thursday'], text: 'В этот день лучше планировать другие мероприятия'},
          {title: 'Физическая культура', id: 3, locked: false, day: ['fridayOne'], text: 'Лучше запланировать физкультуру – последним уроком'},
          {title: 'Математика', id: 4, locked: false, day: ['tuesdayOne'], text: 'Рекомендованное время для этой дисциплины – первая половина дня'}
        ];
      },
      dragTable(evt){
        if (evt.to.classList[0] !== 'home'){
          if (evt.data.day.includes(evt.to.classList[0])){
            if (evt.to.classList[0] === 'monday'){
              this.monday[0].locked = true;
            }else if (evt.to.classList[0] === 'tuesdayOne'){
              this.tuesdayOne[0].locked = true;
            }else if (evt.to.classList[0] === 'tuesdayTwo'){
              this.tuesdayTwo[0].locked = true;
            }else if (evt.to.classList[0] === 'wednesday'){
              this.wednesday[0].locked = true;
            }else if (evt.to.classList[0] === 'thursday'){
              this.thursday[0].locked = true;
            }else if (evt.to.classList[0] === 'fridayOne'){
              this.fridayOne[0].locked = true;
            }
          }else{
            if (evt.to.classList[0] === 'monday'){
              this.monday.splice(0, 1);
              this.$emit('message', evt.data.text);
            }else if (evt.to.classList[0] === 'tuesdayOne'){
              this.tuesdayOne.splice(0, 1);
              this.$emit('message', evt.data.text);
            }else if (evt.to.classList[0] === 'tuesdayTwo'){
              this.tuesdayTwo.splice(0, 1);
              this.$emit('message', evt.data.text);
            }else if (evt.to.classList[0] === 'wednesday'){
              this.wednesday.splice(0, 1);
              this.$emit('message', evt.data.text);
            }else if (evt.to.classList[0] === 'thursday'){
              this.thursday.splice(0, 1);
              this.$emit('message', evt.data.text);
            }else if (evt.to.classList[0] === 'fridayOne'){
              this.fridayOne.splice(0, 1);
              this.$emit('message', evt.data.text);
            }else if (evt.to.classList[0] === 'fridayTwo'){
              this.fridayTwo.splice(0, 1);
              this.$emit('message', 'Лучше не планировать в этот день важных мероприятий');
            }
            this.plans.splice(evt.data.id, 0, evt.data)
          }
          this.endGame();
        }
      },
      endGame(){
        if(this.plans.length === 0){
          this.timer = setTimeout(()=>{
            this.$emit("exit")
          }, 1500)
        }
      },

      onStart(evt){
        if (!this.item){
          this.item = document.createElement('div')
          this.item.innerHTML = evt.target.innerHTML
          this.item.style.position = 'absolute'
          this.item.touchAction = 'none'
          this.item.style.zIndex = '10'
          this.item.style.width = '17rem'
          this.item.style.transition = '.1s ease-out'
          this.item.style.left = String(evt.touches[0].pageX - 272/2) + 'px';
          this.item.style.top = String(evt.touches[0].pageY - 30) + 'px';
          this.item.style.opacity = '.5'
          this.item.classList.add('border-black');
          this.item.classList.add('border-2');
          this.item.classList.add('fw-bold');
          this.item.classList.add('card');
          this.item.classList.add('p-2');
          this.item.classList.add('ps-3');
          this.item.classList.add('pe-3');
          this.item.classList.add('fs-5');
          document.body.appendChild(this.item)
        }
      },

      touchMove(event){
        const now = Date.now();
        if (now - this.lastCall < this.delay) return;

        this.lastCall = now;

        this.item.style.left = event.touches[0]?.pageX - this.item.offsetWidth / 2 + 'px';
        this.item.style.top = event.touches[0]?.pageY - this.item.offsetHeight / 2 + 'px';

        // --- ПРОВЕРЯЕМ, НЕ ВЫХОДИТ ЛИ НАШ ОБЪЕКТ ЗА ГРАНИЦЫ ЭКРАНА ---
        this.item_document(event, this.item)
      },

      onEnd(){
        if (this.item){
          this.item?.remove();
          this.item = '';
        }
      },

      item_document(event, item){
        if (event.touches[0].pageX < 40) {
          item.style.left = event.touches[0].pageX - item.offsetWidth / 2 + 50 + 'px';
        } else if (event.touches[0].pageX > window.screen.width - 30) {
          item.style.left = event.touches[0].pageX - item.offsetWidth / 2 - 50 + 'px';
        }
        if (event.touches[0].pageY < 40) {
          item.style.top = event.touches[0].pageY - item.offsetHeight / 2 + 50 + 'px';
        } else if (event.touches[0].pageY > window.screen.height - 30) {
          item.style.top = event.touches[0].pageY - item.offsetHeight / 2 - 50 + 'px';
        }
      },
    },
  }
</script>

<style scoped>
    .task{
      background-color: #eacdaa;
      width: 100%;
      height: 100vh;
      transition: 1s;
      position: fixed;
      z-index: 1;

      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-around;
    }

    table{
      background-color: white;
      border-collapse: collapse;
      border: 0 transparent;
      border-radius: 1rem;
      font-size: 1.3rem;
      letter-spacing: 1px;
    }

    td{
      height: 7rem;
      width: 13rem;
    }

    th{
      width: 7rem;
    }

    caption {
      caption-side: top;
      padding: 10px;
      font-weight: bold;
      color: black;
    }

    th,
    td {
      padding: 8px 10px;
    }

    td:last-of-type {
      text-align: center;
    }

    tfoot th {
      text-align: right;
    }

    tfoot td {
      font-weight: bold;
    }

    .drag-ghost {
      opacity: 0.5; /* Прозрачность */
      transform: scale(1.05); /* Увеличенный размер */
      border: 2px dashed #333; /* Добавляем рамку */
      z-index: 1000; /* Поверх всех элементов */
      pointer-events: none; /* Чтобы нельзя было взаимодействовать */
    }

    .ghostDrag{
      opacity: 0 !important;
    }
</style>