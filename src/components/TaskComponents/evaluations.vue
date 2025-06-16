<template>
    <div class="align-items-center gap-3" style="display: flex; transition: 1s;">
        <h1 v-for="(item, index) in evaluations" :key="index" style="width: 15rem" class="text-center py-3 bg-white rounded-2" @click="(()=>{this.evaluation(index)})">{{ item }}</h1>
    </div>
</template>

<script>
    export default {
      data(){
        return {
          evaluations: [1, 2, 3, 4, 5],
        }
      },

      emits:['evaluated'],

      methods:{
        evaluation(index){
          this.$emit('evaluated', index)
        }
      }
    }
</script>

<style scoped>

</style>