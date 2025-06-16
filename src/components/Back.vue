<template>
    <div ref="background_div" class="background_div"></div>
</template>


<script>
    export default {
        props:{
            images:{
                type: Object,
                required: true,
            }
        },

        data(){
            return{
            }
        },

        methods: {
            // Функции по созданию картинок и управления пальчиками
            startGame(){
                this.images.forEach((item) => {
                    this.element = this.createItem(item);
                    this.$refs.background_div.appendChild(this.element);
                });

                this.giveStart();
            },

            createItem(item){
                this.element = document.createElement("img")
                this.element.src = item.src
                this.element.alt = item.alt
                this.element.className = item.className
                this.element.style.zIndex = item.zIndex
                this.element.item_move = item.item_move
                this.element.style.position = "absolute"
                
                this.element.style.width = item.width
                this.element.style.left = item.style_left + "vh"
                this.element.style.top = item.style_top + "vh"

                if (item.className === 'square' || item.className === 'ads' ||
                    item.className === 'teacher_do' ||item.className === 'table_do' ||
                    item.className === 'part_do two' ||item.className === 'clock'){
                  this.element.classList.add('active')
                }
                
                return this.element
            },

            giveStart(){
                document.querySelector("img.square").addEventListener("click", () => {
                  this.$emit("task_one", true)
                })

                document.querySelector("img.ads").addEventListener("click", () => {
                  this.$emit("task_two", true)
                })

                document.querySelector("img.teacher_do").addEventListener("click", () => {
                  this.$emit("task_three", true)
                })

                document.querySelector("img.table_do").addEventListener("click", () => {
                  this.$emit("task_three", true)
                })

                document.querySelector("img.part_do.two").addEventListener("click", () => {
                  this.$emit("task_four", true)
                })

                document.querySelector("img.clock").addEventListener("click", () => {
                  this.$emit("task_five", true)
                })
            },

            blurBack(){
              this.$refs.background_div.style.filter = 'blur(.3rem)'
            },

            unBlurBack(){
              this.$refs.background_div.style.filter = ''
            },

            inActive(value){
              if(value === 'square'){
                  document.querySelector('.' + value).classList.remove('active')
              }else if(value === 'ads'){
                  document.querySelector('.' + value).classList.remove('active')
              }else if(value === 'table_do'){
                  document.querySelector('.' + value).classList.remove('active')
              }else if(value === 'part_do.two'){
                  document.querySelector('.' + value).classList.remove('active')
              }else if(value === 'clock'){
                  document.querySelector('.' + value).classList.remove('active')
              }
            }
        },

        watch:{
            images(value){
                if (value.length > 0) {
                    this.startGame()
                }
            }
        }
    }
</script>

<style>
    .background_div{
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        z-index: 1;

        transition: 1s;
    }

    img.square.active,
    img.table_do.active,
    img.part_do.two.active,
    img.clock.active,
    img.ads.active{
      filter: drop-shadow(0 0 20px rgb(46, 111, 255));
      border-radius: .2rem;
      animation: pulse 1s infinite alternate-reverse;
    }

    @keyframes pulse{
      0% {
        transform: scale(1);
      }
      100% {
        transform: scale(1.05);
      }
    }
</style>