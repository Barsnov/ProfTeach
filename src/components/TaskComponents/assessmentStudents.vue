<template>
    <div class="justify-content-center align-items-center gap-5" style="display: flex; transition: 1s;">
      <div class="bg-white px-5 py-4 rounded-4" style="width: 87rem">
        <h1 v-text="title" style="font-family: Montserrat, sans-serif"/>
        <table class="table mt-3 table-bordered border-black" style="font-size: 1rem; border-collapse: collapse;">
          <thead>
          <tr class="border border-black border-end-1">
            <th scope="row" class="text-center">#</th>
            <th class="text-start" scope="row" style="width: 25rem">Ученик</th>
            <th scope="row" class="border-end-0" style="max-width: 1rem;">Оценки</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(student, index) in students" :key="student.id">
            <td :class="{'fw-bold': this.studentAssessment.id === student.id}"> {{index + 1}}</td>
            <td :class="{'fw-bold': this.studentAssessment.id === student.id}" class="text-start ps-2" v-text="student.name"/>
            <td v-for="evaluation in student.evaluations" :key="evaluation">
              <VueDraggable draggable=".locked" class="6" :group="{name: 'assessment', pull: false, put: !this.cells[0].cell?.locked}" v-model="cells[0].cell"
                            v-if="student.id === 6 && evaluation === ''">
                  <div v-for="evaluation in cells[0].cell" :class="{'locked': !evaluation?.locked}">
                      {{evaluation.title}}
                  </div>
              </VueDraggable>
              <VueDraggable draggable=".locked" class="14" :group="{name: 'assessment', pull: false, put: !this.cells[1].cell?.locked}" v-model="cells[1].cell"
                            v-if="student.id === 14 && evaluation === ''">
                <div v-for="evaluation in cells[1].cell" :class="{'locked': !evaluation?.locked}">
                  {{evaluation.title}}
                </div>
              </VueDraggable>
              <VueDraggable draggable=".locked" class="17" :group="{name: 'assessment', pull: false, put: !this.cells[2].cell?.locked}" v-model="cells[2].cell"
                            v-if="student.id === 17 && evaluation === ''">
                <div v-for="evaluation in cells[2].cell" :class="{'locked': !evaluation?.locked}">
                  {{evaluation.title}}
                </div>
              </VueDraggable>
              <VueDraggable draggable=".locked" class="25" :group="{name: 'assessment', pull: false, put: !this.cells[3].cell?.locked}" v-model="cells[3].cell"
                            v-if="student.id === 25 && evaluation === ''">
                <div v-for="evaluation in cells[3].cell" :class="{'locked': !evaluation?.locked}">
                  {{evaluation.title}}
                </div>
              </VueDraggable>
              <VueDraggable draggable=".locked" class="27" :group="{name: 'assessment', pull: false, put: !this.cells[4].cell?.locked}" v-model="cells[4].cell"
                            v-if="student.id === 27 && evaluation === ''">
                <div v-for="evaluation in cells[4].cell" :class="{'locked': !evaluation?.locked}">
                  {{evaluation.title}}
                </div>
              </VueDraggable>
              <VueDraggable draggable=".locked" class="29" :group="{name: 'assessment', pull: false, put: !this.cells[5].cell?.locked}" v-model="cells[5].cell"
                            v-if="student.id === 29 && evaluation === ''">
                <div v-for="evaluation in cells[5].cell" :class="{'locked': !evaluation?.locked}">
                  {{evaluation.title}}
                </div>
              </VueDraggable>
              <p :class="{'fw-bold': this.studentAssessment.id === student.id}" v-else class="m-0"> {{evaluation}} </p>
            </td>
          </tr>
          </tbody>
        </table>
      </div>

      <div class="d-flex flex-column gap-2">
        <VueDraggable class="rounded-2 bg-white p-2" fallback-class="ghostDrag" v-for="evaluation in evaluationsAssessment" v-model="evaluationsAssessment"
                      :group="{name: evaluation.group, pull: 'clone', put: false}"
                      @end="assessmentStudent" :clone="()=>cloneAssessment(evaluation)"
                      :key="evaluation.id">
            <div @touchstart.passive="onStart" @touchmove.passive="touchMove" @touchend.passive="onEnd"
                 style="font-family: Montserrat, sans-serif" class="fw-bold" v-text="evaluation.title"/>
        </VueDraggable>
      </div>
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
        title: 'Выставление оценок в журнал',
        timer: '',
        studentAssessment: {id: '', evaluation: ''},

        evaluationsAssessment:[
          {id: 1, title: 2, group: 'two', locked: false},
          {id: 2, title: 3, group: 'three', locked: false},
          {id: 3, title: 4, group: 'four', locked: false},
          {id: 4, title: 5, group: 'five', locked: false}
        ],

        students: [
          {id: 1, name:	'Александров Алексей', evaluations:	[5,	4, 5,	5, 3, 3, 3, 5,]},
          {id: 2, name:	'Ананьев Дмитрий', evaluations:	[4,	4, 4,	4, 4, 4, 5, 5,]},
          {id: 3, name:	'Андрейчик Анастасия', evaluations:	[3,	3, 3, 2, 4,	3, 3,	3,]},
          {id: 4, name:	'Васильев Александр', evaluations:	[5,	4, 4, 4, 3,	3, 3,	5,]},
          {id: 5, name:	'Васнецов Владислав', evaluations:	[5,	5, 5, 5, 5,	3, 5,	5,]},

          {id: 6, name:	'Власов Виктор', evaluations:	[3,	3, 5, 5, 4, 3, 4, '']},
          {id: 7, name:	'Врагова Наталья', evaluations:	[4,	4, 4, 4, 4, 4, 4, 4,]},
          {id: 8, name:	'Высоков Алексей', evaluations:	[3,	4, 3, 3, 4, 5, 5, 4,]},
          {id: 9, name:	'Гаврилова Ирина', evaluations:	[5,	'Н', 'Н', 'Н', 'Н', 4, 5, 5,]},
          {id: 10, name:	'Данилова Мария', evaluations:	[3,	3, 4, 3, 3, 3, 4, 4,]},
          {id: 11, name:	'Егоров Владислав', evaluations:	[5,	5, 5, 5, 5, 5, 5, 5,]},
          {id: 12, name:	'Ершов Геннадий', evaluations:	[5,	4, 5, 4, 4, 4, 4, 4,]},
          {id: 13, name:	'Иванов Геннадий', evaluations:	['Н',	'Н', 'Н', 3, 3, 4, 'Н', 'Н',]},

          {id: 14, name:	'Иванов Олег', evaluations:	[4,	5, 4, 4, 5, 5, 5, '']},
          {id: 15, name:	'Иванова Дарья', evaluations:	[4,	3, 4, 3, 3, 3, 3, 3,]},
          {id: 16, name:	'Иванченко Дмитрий', evaluations:	[4,	4, 4, 4, 5, 5, 5, 5,]},

          {id: 17, name:	'Калинина Анастасия', evaluations:	[4,	4, 4, 5, 5, 5, 3, '']},
          {id: 18, name:	'Козлова Мария', evaluations:	[4,	4, 3, 3, 3,	3, 'Н',	'Н',]},
          {id: 19, name:	'Крупинин Илья', evaluations:	[4,	4, 5, 3, 4,	4, 4, 4,]},
          {id: 20, name:	'Кузьмина Анна', evaluations:	[5,	5, 5, 5, 'Н', 'Н', 'Н', 'Н',]},
          {id: 21, name:	'Кузьмин Владимир', evaluations:	[5,	4, 4, 4, 4, 3, 3, 3,]},
          {id: 22, name:	'Леонтьева Наталья', evaluations:	[3,	5, 3, 4, 4, 4, 4, 5,]},
          {id: 23, name:	'Михайлова Марина', evaluations:	[3,	3, 3, 2, 4, 4, 2, 4,]},
          {id: 24, name:	'Николаева Виктория', evaluations:	[4,	5,	5,	5,	4,	3,	3,	3,]},

          {id: 25, name:	'Павлова Ирина', evaluations:	[4,	5, 3, 3, 4, 4, 3, '']},
          {id: 26, name:	'Петров Кирилл', evaluations:	[5,	4, 3, 3, 3, 3, 4, 5,]},

          {id: 27, name:	'Савин Артём', evaluations:	[4,	4, 3, 3, 3,	3, 5, '']},
          {id: 28, name:	'Фёдорова Анастасия', evaluations:	[5,	4, 4, 4, 5, 5, 4, 5,]},

          {id: 29, name:	'Щеглов Дмитрий', evaluations: 	[3,	3, 3, 3, 2,	3, 3, '']},
          {id: 30, name:	'Юрасов Глеб', evaluations:	[4,	3, 3, 5, 5, 5, 4, 4,]},

        ],

        cells: [
          {cell:[]},
          {cell:[]},
          {cell:[]},
          {cell:[]},
          {cell:[]},
          {cell:[]},
        ],
        evaluationStudent: '',

        eventClass: '',
        objectAssessment: '',
        lastWork: false,

        isDragging: false,
        item: '',
        lastCall: 0,
        delay: 10,
      }
    },

    methods:{
      assessment(array, object){
        this.objectAssessment = object;
        this.studentAssessment = array;
        console.log(this.studentAssessment.id)
      },
      cloneAssessment(evaluation){
        this.evaluationStudent = evaluation.title;
        return {...evaluation};
      },
      assessmentStudent(evt){
        Number(evt.to.classList[0]) === 6 ? this.eventClass = 0 : (Number(evt.to.classList[0]) === 14 ? this.eventClass = 1
        : (Number(evt.to.classList[0]) === 17 ? this.eventClass = 2 : (Number(evt.to.classList[0]) === 25 ? this.eventClass = 3
        : (Number(evt.to.classList[0]) === 27 ? this.eventClass = 4 : (Number(evt.to.classList[0]) === 29 ? this.eventClass = 5 : this.eventClass = '')))));

        if(Number(evt.to.classList[0]) === this.studentAssessment.id){
          if(this.studentAssessment.evaluation === this.evaluationStudent){
            if (this.lastWork){
              if(this.objectAssessment === 'math'){
                this.title = 'Вы успешно проверили самостоятельную работу учеников!'
                this.$emit('inActiveButton', this.objectAssessment)
              }else{
                this.title = 'Вы успешно проверили диктант учеников!'
                this.$emit('inActiveButton', this.objectAssessment)
              }
              this.$emit('exitAssessment')
            }else{
              this.timer = setTimeout(()=> this.$emit('putRating', this.objectAssessment), 1000);
              this.cells[this.eventClass].cell.locked = true;
            }
          }else{
            this.cells[this.eventClass]?.cell.splice(0, 1);
            this.$emit('message', 'Оценка не та')
          }
        }else{
          this.cells[this.eventClass]?.cell.splice(0, 1);
          this.$emit('message', 'Не тот ученик')
        }
      },
      lastWorked(){
        console.log('hi')
        this.lastWork = true;
      },
      onStart(evt){
        if (!this.item){
          this.item = document.createElement('div')
          this.item.innerHTML = evt.target.innerHTML
          this.item.style.position = 'absolute'
          this.item.touchAction = 'none'
          this.item.style.zIndex = '10'
          this.item.style.transition = '0s'
          this.item.style.left = String(evt.touches[0].pageX - 13) + 'px';
          this.item.style.top = String(evt.touches[0].pageY - 20) + 'px';
          this.item.style.opacity = '.5'
          this.item.classList.add('border-black');
          this.item.classList.add('border-2');
          this.item.classList.add('fw-bold');
          this.item.classList.add('text-center');
          this.item.classList.add('card');
          this.item.classList.add('p-2');
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
    }
  }
</script>

<style scoped>
    td{
      font-family: Montserrat, sans-serif;
      padding: .1rem;
      text-align: center;
      vertical-align: middle;
    }

    p{
      font-family: Montserrat, sans-serif;
    }

    .locked{
      background: #85ff87;
    }

    .ghostDrag{
      opacity: 0 !important;
    }
</style>