<template>
    <div ref="hintShow" v-if="hintIf" class="hint">
        <div class="messageError">
          <h4>{{this.message}}</h4>
        </div>
        <h1 @click="closeHint()" class="mark">X</h1>
    </div>
</template>

<script>
  export default {
    data(){
      return {
        message: 'Уберите лишнее',
        timer: '',
        hintIf: false,
      }
    },

    methods:{
      messagesShow(text){
        clearTimeout(this.timer);
        this.hintIf = true;
        this.message = text;
        setTimeout(()=>{
          this.$refs.hintShow.style.opacity = '1';
        }, 50)

        this.timer = setTimeout(()=>{
          this.$refs.hintShow.style.opacity = '0';
        }, 4000)

      },

      closeHint(){
        this.$refs.hintShow.style.opacity = '0'
        setTimeout(()=>{
          this.hintIf = false;
          clearTimeout(this.timer);
        }, 1000)
      }
    }

  }
</script>

<style scoped>
    .hint{
      opacity: 0;
      position: absolute;
      z-index: 10;
      top: 2vh;
      right: 1%;
      box-shadow: 0 .5rem 2rem .1rem rgba(0, 0, 0, 0.2);
      border: .2rem #ff2045 solid;
      background-color: white;
      border-radius: .8rem;
      padding: 1rem;
      text-align: center;
      transition: 1s;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 1rem;
    }

    .hint h1.mark{
      background-color: #de234b;
      width: 2rem;
      height: 2rem;
      border-radius: 10%;
      text-align: center;
      margin: 0;
      font-size: 1.5rem;
      display: flex;
      flex-direction: column;
      justify-content: center;
      font-family: 'Roboto', sans-serif;
      color: white;
      font-weight: 100;
    }

    .hint .messageError{
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      width: auto;
      margin-right: auto;
    }

    .hint .messageError h1{
      font-size: 1.8rem;
      margin: 0;
    }

    .hint .messageError h4{
      color: #8d8d8d;
      margin: 0;
      font-size: 1.2rem;
    }

    .hint h2.closeMessage{
        font-family: 'Roboto', sans-serif;
        margin-bottom: 2rem;
        color: #505050;
    }
</style>