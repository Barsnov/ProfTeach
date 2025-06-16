<template>
  <div class="task">
    <div class="board_with_the_task d-flex flex-column align-items-center justify-content-between p-5 mt-5" style="width: 85%; height: 85%; margin-left: 10rem;">
      <!--Общение-->
      <div v-show="messages.length !== 0" ref="discussion" class="discussion d-flex flex-column gap-3 mt-4 mb-3 overflow-y-auto px-5 py-2" style="width: 95%; height: 40rem; scroll-behavior: smooth; ">
        <div v-for="(message, index) in messages" key="{{index}}" class="d-flex flex-column gap-3">
            <!--Ученик-->
            <div class="d-flex gap-4 justify-content-start align-items-center">
              <img :alt="message?.studentName + ' ' + index" :src="message?.studentSrc" style="width: 8rem;">
              <div class="d-flex flex-column border border-white p-4" style="border-radius: 1.3rem 1.3rem 1.3rem 0">
                <h3 class="text-white" v-text="message?.studentName"/>
                <p class="text-white m-0 mt-3 fs-5" style="width: 28rem" v-text="message?.text"/>
              </div>
            </div>

            <!--Учитель-->
            <div ref="teacherAnswer" style="opacity: 0" class="d-flex gap-4 justify-content-end align-items-center">
              <div class="d-flex flex-column border border-white p-4" style="border-radius: 1.3rem 1.3rem 0 1.3rem">
                <h3 class="text-white">Учитель:</h3>
                <p class="text-white m-0 mt-3 fs-5" style="width: 30rem" v-text="message?.firstAnswer"/>
              </div>
              <img alt="teacher" src="../../../public/images/Head_teacher.svg" style="width: 8rem;">
            </div>
        </div>
      </div>

      <div ref="twoAnswer" v-show="messages.length !== 0" style="opacity: 0" class="d-flex mb-2 gap-5 align-items-end justify-content-center">
        <h4 ref="answers" @click="teacherAnswer(index)" v-for="(answer, index) in answers"
            class="border bg-white border-white rounded-2 rounded-bottom-0 p-4 text-center d-flex flex-column justify-content-center" style="width: 45%; height: 8rem" v-text="answer.title"/>
      </div>

      <h1 ref="endGame" class="text-white mb-2" style="opacity: 0" v-if="indexAnswer === messages.length">Вы успешно справились с заданием!!!</h1>
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        timer: '',
        interval: '',
        clickAnswer: false,

        answers: [
          {title: ''},
          {title: ''}
        ],

        taskFour: [
          {
            student: "/public/images/Head_Olya.svg",
            studentName: 'Оля:',
            text: 'А для чего нам изучать в школе столько разных предметов?',
            firstAnswer: 'Оля...',
            answers: ['Оля, сама догадайся, это же так очевидно!', 'Оля, интересный вопрос, давай порассуждаем вместе.'],
            trueAnswer: 1,
          },
          {
            student: "/public/images/Head_Sasha.svg",
            studentName: 'Саша:',
            text: 'Я не понял правило про именительный и винительный падежи у существительных. Вы можете объяснить его ещё раз?',
            firstAnswer: '...',
            answers: ['Почитай внимательно учебник! Там все доступно написано.', 'Хорошо, давай разберём его сейчас вместе.'],
            trueAnswer: 1,
          },
          {
            student: "/public/images/Head_Kolya.svg",
            studentName: 'Коля:',
            firstAnswer: '...',
            text: 'Как можно запомнить столько всяких правил? В голове не укладывается!',
            answers: [' Правил действительно много, но есть разные приемы, которые помогают их быстро и легко запомнить. Я вас обязательно им научу.',
              'Придется постараться, иначе не сдадите экзамены и никуда не поступите!'],
            trueAnswer: 0,
          },
          {
            student: "/public/images/Head_Kolya.svg",
            studentName: 'Коля:',
            text: 'Могу ли я улучшить оценки по предмету?',
            firstAnswer: '...',
            answers: ['Коля, хорошо, что у тебя есть стремление улучшить оценки. Приходи после уроков, мы с тобой подтянем твои знания.',
              'Зачем тебе это? Ты все равно не отличник!'],
            trueAnswer: 0,
          },
          {
            student: "/public/images/Head_Olya.svg",
            studentName: 'Оля:',
            text: 'А какой предмет вам нравился в школе?',
            firstAnswer: '...',
            answers: ['В школе мне нравились различные предметы… (поделиться личным опытом)', 'Оля, ты мешаешь вести мне урок своими глупыми вопросами!'],
            trueAnswer: 0,
          },
        ],

        messages:[],
        indexAnswer: 0,
        objectTask: {},
      }
    },

    methods:{
      start(){
        //Запуск игрушки-чата
        setTimeout(()=>{
          if (this.indexAnswer < this.taskFour.length){
            this.objectTask = {
              studentName: this.taskFour[this.indexAnswer].studentName,
              text: '',
              studentSrc: this.taskFour[this.indexAnswer].student,
              firstAnswer: '',
              trueAnswer: this.taskFour[this.indexAnswer].trueAnswer,
            }
            this.messages.push(this.objectTask);
            this.recordingMessageStudent(this.taskFour[this.indexAnswer], this.messages[this.indexAnswer])
          }else{
            this.$refs.endGame.style.opacity = '1';
            setTimeout(()=>this.$emit('exit'), 3000)
          }
        }, 1500)
      },


      //Заполнение чего либо из сообщений
      recordingMessageStudent(arrayOne, arrayTwo){
        let i = 0;
        let interval = setInterval(()=>{
          if (i < arrayOne.text.length){
            arrayTwo.text += arrayOne.text[i];
            i++
            if (this.$refs.discussion) {
              this.$refs.discussion.scrollTop = this.$refs.discussion.scrollHeight;
            }
          }else{
            clearInterval(interval)
            setTimeout(()=>{
              if (this.$refs.teacherAnswer[this.indexAnswer]){
                this.$refs.teacherAnswer[this.indexAnswer].style.opacity = '1'
              }
              setTimeout(()=>this.recordingMessageTeacher(this.taskFour[this.indexAnswer], this.messages[this.indexAnswer]), 500)
            }, 1000)
          }
        }, 50)
      },

      recordingMessageTeacher(arrayOne, arrayTwo){
        let i = 0;
        let interval = setInterval(()=>{
          if (i < arrayOne.firstAnswer.length){
            arrayTwo.firstAnswer += arrayOne.firstAnswer[i];
            i++
            if (this.$refs.discussion) {
              this.$refs.discussion.scrollTop = this.$refs.discussion.scrollHeight;
            }
          }else{
            clearInterval(interval)
            setTimeout(()=>{
              this.answers.forEach((item, index)=>{item.title = this.taskFour[this.indexAnswer].answers[index];})
              setTimeout(()=>{
                if (this.$refs.twoAnswer){
                  this.$refs.twoAnswer.style.opacity = '1';
                  this.clickAnswer = true;
                }
                }, 400)
            }, 1000)
          }
        }, 50)
      },

      recordingMessageTeacherTwo(arrayOne, arrayTwo){
        let i = 0;
        arrayTwo.firstAnswer = ''
        let interval = setInterval(()=>{
          if (i < arrayOne.title.length){
            arrayTwo.firstAnswer += arrayOne.title[i];
            i++
            if (this.$refs.discussion) {
              this.$refs.discussion.scrollTop = this.$refs.discussion.scrollHeight;
            }
          }else{
            clearInterval(interval)
            setTimeout(()=>{
              this.start();
              this.indexAnswer ++;
            }, 1000)
          }
        }, 50)
      },

      //Функция нажатия на кнопочки ответа учителя
      teacherAnswer(index){
        if(this.clickAnswer){
          if(this.messages[this.indexAnswer].trueAnswer === index){
            this.recordingMessageTeacherTwo(this.answers[index], this.messages[this.indexAnswer])
            this.clickAnswer = false;
            setTimeout(()=>{this.$refs.twoAnswer.style.opacity = '0';}, 400)
          }else{
            this.$emit('message', 'Учитель не должен грубить своим ученикам, попробуйте другой вариант ответа..')
          }
        }
      }
    },

    mounted(){
      this.start()
    }
  }
</script>

<style scoped>
    .task{
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 1;
        transition: 1s;
    }

    .task .board_with_the_task{
      background-image: url("../../../public/images/Board_with_the_task_four.svg");
      background-size: cover;
    }
</style>