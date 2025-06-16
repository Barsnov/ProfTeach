<template>
  <div class="three position-absolute w-100 h-100 flex-column justify-content-center align-items-center" style="display: flex;">
    <div v-show="taskThreeIf" ref="instruction" style="display: flex" class="bg-white flex-column align-items-center border border-4 rounded-2 w-50 border-black p-5">
      <h1 class="fw-bold">Проверка тетрадей</h1>
      <p class="fs-4 mt-3 text-center" style="font-family: 'Montserrat', sans-serif">Проверка тетрадей является неотъемлемой частью деятельности учителя.
        Записи в тетрадях необходимо выполнять красной ручкой. В качестве отметки может быть использован один из следующих символов: «2», «3», «4», «5».
        Допускается выставление нескольких отметок за каждый вид деятельности (в том числе и через дробь).
      <br>Задание: проверьте тетради учеников и выставите в журнал оценку, соответствующую фамилии ученика</p>
      <div class="d-flex align-items-center justify-content-center gap-3 mt-5">
        <div ref="buttonMath" class="rounded-2 bg-dark text-white text-center d-flex py-2 fs-3 px-4" @click="startMath()">Математика</div>
        <div ref="buttonRus" class="rounded-2 bg-dark text-white text-center d-flex py-2 fs-3 px-4" @click="startRusLang()">Русский язык</div>
      </div>
    </div>

    <task-three-math @lastWork="lastWorked" @assessmentStudents="assessmentStudents" ref="math" v-show="mathIf" @message="message"/>
    <task-three-rus-language @lastWork="lastWorked" @assessmentStudents="assessmentStudents" ref="rusLang" v-show="rusLangIf" @message="message"/>
    <evaluations v-show="evaluationsIf" @evaluated="evaluated" class="mt-3"/>

    <!--Задание с журналом-->
    <assessment-students @inActiveButton="inActiveButton" @inActiveThree="inActiveThree" @exit="exit" @message="message" @putRating="putRating" style="transition: 1s" ref="assessmentStudentsTask" v-if="assessmentStudentsIf"/>
  </div>
</template>

<script>
  import taskThreeMath from "@/components/TaskComponents/taskThreeMath.vue";
  import taskThreeRusLanguage from "@/components/TaskComponents/taskThreeRusLanguage.vue";
  import evaluations from "@/components/TaskComponents/evaluations.vue";
  import assessmentStudents from "@/components/TaskComponents/assessmentStudents.vue"

  export default {
    components:{
      taskThreeMath,
      taskThreeRusLanguage,
      evaluations,
      assessmentStudents,
    },

    data(){
      return{
        mathIf: false,
        rusLangIf  : false,
        evaluationsIf: false,
        taskThreeIf: true,
        assessmentStudentsIf: false,
        lastWork: false,
        object: '',
        timer: '',
        countThree: 0,
      }
    },

    methods:{
      start(){
        this.mathIf = false;
        this.rusLangIf = false;
        this.evaluationsIf = false;
        this.assessmentStudentsIf = false;
        this.taskThreeIf = true;
      },

      startMath(){
        this.$refs.instruction.style.opacity = '0';
        this.timer = setTimeout(()=>{
          this.taskThreeIf = false;
          this.mathIf = true;
          this.evaluationsIf = true;
          this.timer = setTimeout(()=>{
            this.$refs.math.start()
          }, 100);
        }, 300);
      },

      startRusLang(){
        this.$refs.instruction.style.opacity = '0';
        this.timer = setTimeout(()=>{
          this.taskThreeIf = false;
          this.rusLangIf = true;
          this.evaluationsIf = true;
          this.timer = setTimeout(()=>{
            this.$refs.rusLang.start()
          }, 100);
        }, 300)
      },

      evaluated(index){
        if (this.mathIf){
            this.$refs.math.assessmentWork(index)
        }else if (this.rusLangIf){
            this.$refs.rusLang.assessmentWork(index)
        }
      },

      //Вывод сообщения об ошибке
      message(value){
        this.$emit('message', value);
      },

      //Выставили оценку в журнал
      putRating(object){
        this.assessmentStudentsIf = false;
        this.timer = setTimeout(()=>{
          if (object === 'math'){
            this.mathIf = true;
            this.$refs.math.checkDictation();
          }else{
            this.rusLangIf = true;
            this.$refs.rusLang.checkDictation();
          }
          this.evaluationsIf = true;
        }, 500)
      },

      //Появление задания с оцениванием школьников
      assessmentStudents(student){
        this.mathIf ? this.object = 'math' : 'rusLang';
        this.mathIf = false;
        this.rusLangIf = false;
        this.evaluationsIf = false;
        this.timer = setTimeout(()=>{
          this.assessmentStudentsIf = true;
          this.timer = setTimeout(()=> this.$refs.assessmentStudentsTask.assessment(student, this.object), 100);
          if (this.lastWork) this.timer = setTimeout(()=> this.$refs.assessmentStudentsTask.lastWorked(), 100);
        }, 500)
      },

      lastWorked(){
        this.lastWork = true;
      },

      inActiveThree(){
        this.$emit('inActiveThree')
      },

      inActiveButton(value){
        this.timer = setTimeout(()=>{
          if(value === 'math'){
            this.$refs.buttonMath.style.opacity = '0.5'
            setTimeout(()=> this.mathIf = false, 500)
            this.countThree ++
          }else{
            this.$refs.buttonRus.style.opacity = '0.5'
            setTimeout(()=> this.rusLangIf = false, 500)
            this.countThree ++
          }

          this.lastWork = false;
          this.taskThreeIf = true
          this.$refs.instruction.style.opacity = '1';
          this.assessmentStudentsIf = false;

          if (this.countThree === 2){
            setTimeout(()=>this.exit(), 500)
          }
        }, 2000)
      },

      exit(){
          this.inActiveThree()
          this.$emit('exit')
      }
    },

    mounted() {
      this.start();
    }
  }
</script>


<style scoped>
    .three{
      opacity: 1;
      transition: 1s;
    }
</style>