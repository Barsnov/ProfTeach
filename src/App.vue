<template>
  <back ref="backMenu" :images="images" @task_one="task_one" @task_two="task_two" @task_three="task_three" @task_four="task_four" @task_five="task_five"></back>
  <task ref="tasked" @inActive="inActive" @blur="blur" @unBlur="unBlur"></task>
</template>


<script>
  import Back from './components/Back.vue'
  import Task from './components/Task.vue'
  export default {
    components: {
      Back,
      Task
    },
    
    data() {
      return {
          images: [],
      }
    },

    methods: {
      async getBackImages(){
        try{
          const response = await fetch('/public/images_back.txt');
          this.images = await response.json();
          if (!response.ok) new Error('Интернет подключи');
        } catch (error){
          console.error('Файл умер при извлечении...', error);
        }
      },

      task_one(value){
          this.$refs.tasked.taskOne(value)
      },
      task_two(value){
          this.$refs.tasked.taskTwo(value)
      },
      task_three(value){
          this.$refs.tasked.taskThree(value)
      },
      task_four(value){
          this.$refs.tasked.taskFour(value)
      },
      task_five(value){
          this.$refs.tasked.taskFive(value)
      },

      inActive(value){
        this.$refs.backMenu.inActive(value);
      },

      blur(){
        this.$refs.backMenu.blurBack();
      },
      unBlur(){
        this.$refs.backMenu.unBlurBack();
      },
    },

    mounted(){
      this.getBackImages()
    }
}
</script>

<style>

</style>
