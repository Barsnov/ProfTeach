<template>
  <div class="task one">
    <div class="board_with_the_task">
      <h3 class="title">Что способствует успешному проведению занятия, а что наоборот - мешает?</h3>
      <h3 class="task_one w-75 mt-2" :class="this.classType">{{this.answerTask}}</h3>

      <div class="table">
        <div class="column one" @click="taskOneAnswer">
          <h4 class="border-bottom border-2 ps-3 m-0" style="height: 6vh">Способствует успеху урока</h4>
          <h4 v-for="(item) in answer" :key="item" ref="answerTrue" class="true">
            {{ item }}
          </h4>
        </div>


        <div class="column two" @click="taskOneAnswer">
          <h4 class="border-bottom border-2 ps-3 m-0" style="height: 6vh">Затрудняет проведение урока</h4>
          <h4 v-for="(item) in answer" :key="item" ref="answerFalse" class="false">
            {{ item }}
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default{
    data(){
      return{
        timer: '',
        random_string: '',
        get_answer_one: '',
        type:'',

        answer: [
            '1)',
            '2)',
            '3)',
            '4)',
            '5)',
        ],

        classType: '',
        answerTask: '',
        task_one_answer: [
          "Монотонность и сухость при изложении нового материала i",
          "Однообразие методов обучения i",
          "Неуверенность в своих знаниях i",
          "Безразличное отношение i",
          "Отсутствие информации об особенностях учеников в классе i",
          //------------------
          "Бодрое самочувствие c",
          "Отличное знание материала урока c",
          "Продуманный план урока c",
          "Разнообразие методов обучения c",
          "Увлекательное изложение материала c",
        ],
        task_one_answer_copy: '',
      }
    },

    methods: {
      taskOne(bool){
        if (bool){
          this.$refs.answerTrue.forEach((item)=>{
            item.innerHTML = item.innerHTML.slice(0,2) + " ...";
          });

          this.$refs.answerFalse.forEach((item)=>{
            item.innerHTML = item.innerHTML.slice(0,2) + " ...";
          });

          this.task_one_answer_copy =  this.task_one_answer.slice()
        }

        this.random_string = Math.floor(Math.random() * (this.task_one_answer_copy.length - 1 + 1))

        if (this.task_one_answer_copy.length === 0){
          this.answerTask = "Вы успешно справились с заданием!!!";

          this.timer = setTimeout(()=>{
            this.$emit("exit")
          }, 1000)
        }else{
          this.get_answer_one = this.task_one_answer_copy[this.random_string]
          this.answerTask = String(this.get_answer_one.slice(0, this.get_answer_one.length - 2));
          this.type = this.get_answer_one.slice(this.get_answer_one.length-1, this.get_answer_one.length)

          if (this.type === "c"){
            this.classType = "correct"
          } else if(this.type === "i"){
            this.classType = "incorrect"
          }

          this.task_one_answer_copy.splice(this.random_string, 1)
        }
      },
      taskOneAnswer(event){
        let number_column = event.target
        if ((/column one/.test(number_column.className) === true) || (/true/.test(number_column.className))){
          if (this.classType === "correct"){
            for (let item of this.$refs.answerTrue){
              if (item.innerHTML.length <= 6){
                item.innerHTML = item.innerHTML.slice(0,3) + this.answerTask
                this.taskOne(false)
                break
              }
            }
          }
        }else {
          if (this.classType === "incorrect"){
            for (let item of this.$refs.answerFalse){
              if (item.innerHTML.length <= 6){
                item.innerHTML = item.innerHTML.slice(0,3) + this.answerTask
                this.taskOne(false)
                break
              }
            }
          }
        }
      },
    }
  }
</script>

<style scoped>
    .task{
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        z-index: 1;
        top: 7vh;
        left: 15vh;
        transition: 1s;
    }

    .task_one{
      height: 10vh;
      font-family: "Czizh", serif;
      font-weight: 600;
      background-color: rgba(255, 255, 255, 0.8);
      color: rgb(70, 90, 100);
      text-align: center;
      font-size: 4vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .task .board_with_the_task h3.title{
      font-family: "Czizh", serif;
      font-style: normal;
      font-size: 4vh;
      color: white;
      background: none;
      text-align: center;
      font-weight: 400;
      margin-bottom: 1vh;
    }

    .task .board_with_the_task{
      padding: 6vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-around;
      position: relative;
      left: 8vh;
      width: 149vh;
      height: 84vh;
      background-image: url("../../../public/images/Board_with_the_task_four.svg");
      background-size: cover;
      background-position-x: -1vh;
    }

    .task .board_with_the_task .table{
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      width: 113vh;
      height: 45vh;
    }

    .task .board_with_the_task .table .column{
      padding: 2vh 4vh;
      width: 49%;
      height: 100%;
      border-radius: 5vh 5vh 0 0;
      border: 0.2vh solid white;
    }

    .task .board_with_the_task .table .column h4{
      font-family: "Czizh", serif;
      font-style: normal;
      font-size: 3vh;
      color: white;
      font-weight: 400;
      display: flex;
      align-items: center;
      margin-top: 2vh;
    }
</style>