<template>
  <task-one v-if="taskOneIf" ref="taskOne" :class="classListOne" @exit="exit"></task-one>
  <task-two v-if="taskTwoIf" ref="taskTwo" :class="classListTwo" @exit="exit"></task-two>
  <task-three v-if="taskThreeIf" @inActiveThree="inActiveThree" ref="taskThree" :class="classListThree" @message="message" @exit="exit"></task-three>
  <task-four v-if="taskFourIf" ref="taskFour" :class="classListFour" @exit="exit" @message="message"></task-four>
  <task-five v-if="taskFiveIf" ref="taskFive" :class="classListFive" @exit="exit" @message="message"></task-five>
  <hint ref="hint"></hint>

  <div class="exit" ref="exit" :class="classListExit" @click="this.exit">Выйти</div>
</template>

<script>
    import TaskOne from "@/components/TaskComponents/taskOne.vue";
    import TaskTwo from "@/components/TaskComponents/taskTwo.vue";
    import TaskThree from "@/components/TaskComponents/taskThree.vue";
    import TaskFour from "@/components/TaskComponents/taskFour.vue";
    import TaskFive from '@/components/TaskComponents/taskFive.vue';
    import Hint from '@/components/TaskComponents/hint.vue';

    export default {
      components:{
          TaskOne,
          TaskTwo,
          TaskThree,
          TaskFour,
          TaskFive,
          Hint
      },

      emits:['blur', 'unBlur', 'inActive'],

      data(){
        return {
          classListOne: "z-0 opacity-0",
          classListTwo: "z-0 opacity-0",
          classListThree: "z-0 opacity-0",
          classListFour: "z-0 opacity-0",
          classListFive: "z-0 opacity-0",
          classListExit: "z-0 opacity-0",

          taskOneIf: false,
          taskTwoIf: false,
          taskThreeIf: false,
          taskFourIf: false,
          taskFiveIf: false,
          exitIf: false,

          element: '',
          elemBelow: '',
        }
      },

      methods:{
        taskOne(bool){
          this.exitIf = true;
          this.taskOneIf = true;
          this.timer = setTimeout(()=>{
            this.classListOne = "z-1 opacity-1"
            this.classListExit = "z-1 opacity-1"
            this.$refs.taskOne?.taskOne(bool)
          }, 100)
        },
        taskTwo(bool){
          this.exitIf = true;
          this.taskTwoIf = true;
          this.timer = setTimeout(()=>{
            this.classListTwo = "z-1 opacity-1"
            this.classListExit = "z-1 opacity-1"
            this.$refs.taskTwo?.taskTwo(bool)
          }, 100)
        },
        taskThree(bool){
          this.exitIf = true;
          this.taskThreeIf = true;
          this.timer = setTimeout(()=>{
            this.classListThree = "z-1 opacity-1"
            this.classListExit = "z-1 opacity-1"
            this.$emit('blur');
          }, 100)
        },
        taskFour(bool){
          this.exitIf = true;
          this.taskFourIf = true;
          this.timer = setTimeout(()=>{
            this.classListFour = "z-1 opacity-1"
            this.classListExit = "z-1 opacity-1"
          }, 100);
        },
        taskFive(bool){
          this.exitIf = true;
          this.taskFiveIf = true;
          this.timer = setTimeout(()=>{
            this.classListFive = "z-1 opacity-1"
            this.classListExit = "z-1 opacity-1"
            this.$refs.taskFive?.taskFive(bool)
          }, 100)
        },

        inActiveThree(){
          this.$emit('inActive', 'table_do')
        },

        exit(){
          this.classListExit = "z-0 opacity-0"
          this.exitIf = false
          this.$emit('unBlur');

          //------------------- #1
          if (this.taskOneIf){
            this.classListOne = "z-0 opacity-0"
            this.timer = setTimeout(()=>{
              this.taskOneIf = false;
            }, 1000)
            this.$emit('inActive', 'square')
          }else if (this.taskTwoIf){
            this.classListTwo = "z-0 opacity-0"
            this.timer = setTimeout(()=>{
              this.taskTwoIf = false;
            }, 1000)
            this.$emit('inActive', 'ads')
          }else if (this.taskThreeIf){
            this.classListThree = "z-0 opacity-0"
            this.timer = setTimeout(()=>{
              this.taskThreeIf = false;
            }, 1000)
          }else if (this.taskFourIf){
            this.classListFour = "z-0 opacity-0"
            this.timer = setTimeout(()=>{
              this.taskFourIf = false;
            }, 1000)
            this.$emit('inActive', 'part_do.two')
          }else if (this.taskFiveIf){
            this.classListFive = "z-0 opacity-0"
            this.timer = setTimeout(()=>{
              this.taskFiveIf = false;
            }, 1000)
            this.$emit('inActive', 'clock')
          }
        },

        message(text){
          this.$refs.hint.messagesShow(text)
        }
      }
    }
</script>

<style scoped>
    .exit{
      width: 18vh;
      height: 9vh;
      background-color: rgb(69,90,100);
      color: beige;
      position: absolute;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;
      top: 90vh;
      left: 1vh;
      font-size: 3vh;
      border: .8vh beige solid;
    }
</style>