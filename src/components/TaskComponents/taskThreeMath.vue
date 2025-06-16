<template>
  <div ref="copybook" class="copybook border border-2 border-black rounded-5 overflow-hidden" style="opacity: 1">
    <h1 class="" style="font-family: 'Montserrat', sans-serif; font-size: 2.8rem; margin-top: 1.8rem">
      {{ title }} <span class="fs-4 position-absolute" style="top: 6rem; right: 3rem; font-family: 'Montserrat', sans-serif;">Проверено {{this.currentStudent}} из {{student.length}} тетрадей</span>
    </h1>
    <h3 class="position-absolute fs-1 fw-bold" style="left: 2rem; top: 10rem; rotate: 270deg; font-family: Czizh, sans-serif; color: #1B1464" v-text="derangedStudent[currentStudent]?.name"/>
    <div class="dictation d-flex flex-column" style="margin-top: 1.7rem; gap: 2.25rem">
      <div ref="dictations" :key="index" class="ms-3 d-flex gap-1 align-items-center" v-for="(item, index) in derangedStudent[currentStudent]?.dictation">
        <h1 v-for="word in item" class="fw-bold m-0" style="color: #1B1464; font-family: Czizh, sans-serif" v-text="word"/>
      </div>
    </div>

    <!--      <h1 class="position-absolute" style="bottom: 1rem; left: 40rem; opacity: 1; rotate: 35deg; color: #ff0e0e; font-size: 7rem; font-family: Czizh, sans-serif"></h1>-->
    <h1 ref="evaluationOpacity" class="position-absolute" style="bottom: 0; left: 40rem; opacity: 0; color: #ff0e0e; font-size: 7rem; font-family: Czizh, sans-serif"> {{assessmentStudent.evaluation}} </h1>
  </div>
</template>

<script>
export default {

  data() {
    return {
      title: 'Необходимо проверить самостоятельную работу',
      timer: '',
      dictionary: [
        '2>0.99', '0.307<0.4', '0.5<1', '0.400=0.4', '2.85>2.73', '2.08>1.47', '1.37>1.21',
      ],
      currentStudent: 0,

      student: [
        {id: 6, name: 'Власов В.', dictation: ['2>0.99', '0.307<0.4', '0.5<1', '0.400=0.4', '2.85<2.73', '2.08=1.47', '1.37>1.21'], evaluation: ''},
        {id: 14, name: 'Иванов О.', dictation: ['2<0.99', '0.307=0.4', '0.5<1', '0.400<0.4', '2.85<2.73', '2.08=1.47', '1.37>1.21'], evaluation: ''},
        {id: 17, name: 'Калинина А.', dictation: ['2>0.99', '0.307<0.4', '0.5<1', '0.400=0.4', '2.85<2.73', '2.08=1.47', '1.37<1.21'], evaluation: ''},
        {id: 25, name: 'Павлова И.', dictation: ['2>0.99', '0.307<0.4', '0.5>1', '0.400=0.4', '2.85<2.73', '2.08=1.47', '1.37>1.21'], evaluation: ''},
        {id: 27, name: 'Савин А.', dictation: ['2=0.99', '0.307<0.4', '0.5<1', '0.400=0.4', '2.85>2.73', '2.08>1.47', '1.37>1.21'], evaluation: ''},
        {id: 29, name: 'Щеглов Д.', dictation: ['2>0.99', '0.307<0.4', '0.5<1', '0.400>0.4', '2.85>2.73', '2.08=1.47', '1.37<1.21'], evaluation: ''},
      ],

      derangedStudent: [],
      minLength: '',
      child: '',
      unCurrentIndex: '',
      mistakes: 0,
      countFindMistakes: 0,
      objectMistakes: [],
      assessmentStudent: {id: '', evaluation: ''},
    }
  },

  methods: {
    start() {
      this.derangeArray();
      this.currentStudent = 0;
      this.mistakes = 0;
      this.countFindMistakes = 0;
      this.timer = setTimeout(() => {
        this.checkDictation();
      }, 100);
    },
    checkDictation() {
      this.$refs.dictations?.forEach((item, index) => {
        if (this.derangedStudent[this.currentStudent].dictation[index] !== this.dictionary[index]) {
          this.minLength = Math.min(this.derangedStudent[this.currentStudent].dictation[index].length, this.dictionary[index].length);
          for (let id = 0; id < this.minLength; id++) {
            if (this.derangedStudent[this.currentStudent].dictation[index][id] !== this.dictionary[index][id]) {
              this.unCurrentIndex = id;
              this.mistakes++;
            }
            console.log(this.derangedStudent[this.currentStudent].dictation[index]);
            //Если индекс проблемной буквы есть
            if (this.unCurrentIndex !== '') {
              //Создание чёрточки, как дочернего элемента родителю
              this.child = document.createElement('img');
              this.child.src = 'public/images/Strikethrough_task.svg';
              this.child.style.position = 'absolute';
              this.child.style.opacity = '0';
              this.child.style.top = '0';
              this.child.style.left = '-.5rem';
              this.child.style.width = '2rem'

              item.children[this.unCurrentIndex].style.position = 'relative';
              item.children[this.unCurrentIndex].appendChild(this.child);
              item.children[this.unCurrentIndex].addEventListener('click', ((evt) => {
                if (evt.target.style.opacity === '0') {
                  evt.target.style.opacity = '1';
                  this.objectMistakes.push(evt.target)
                  this.countFindMistakes++;
                }
              }));
              this.unCurrentIndex = '';
            }
          }
        }
      });

      //Присвоение оценки в зависимости от ошибки
      if (this.mistakes < 2) {
        this.assessmentStudent.evaluation = 5
      } else if (this.mistakes < 3) {
        this.assessmentStudent.evaluation = 4
      } else if (this.mistakes < 5) {
        this.assessmentStudent.evaluation = 3
      } else {
        this.assessmentStudent.evaluation = 2
      }
      this.assessmentStudent.id = this.derangedStudent[this.currentStudent]?.id;
    },
    assessmentWork(index) {
      console.log(this.countFindMistakes, this.mistakes)
      if (this.countFindMistakes !== this.mistakes) {
        this.$emit('message', 'Ещё не все ошибки найдены')
      } else {
        //Если совпадает индекс + 1 и нужная оценка для студента
        if (index + 1 === this.assessmentStudent.evaluation) {
          this.$refs.evaluationOpacity.style.opacity = '1';
          //Если у нас не последняя проверяемая тетрадь
          if (this.currentStudent + 1 !== this.student.length) {
            this.mistakes = 0;
            this.countFindMistakes = 0;

            this.timer = setTimeout(() => {
              this.$refs.copybook.style.opacity = '0';
              this.objectMistakes.forEach((item) => item.remove())
              this.timer = setTimeout(() => {
                //Вызов задания с заполнением журнала
                this.$emit('assessmentStudents', this.assessmentStudent);
                this.$refs.evaluationOpacity.style.opacity = '0';
                this.currentStudent++;
              }, 1000);
            }, 1000)

          } else {
            this.title = 'Все тетрадки проверены!';
            this.timer = setTimeout(() => {
              this.$refs.copybook.style.opacity = '0';
              this.objectMistakes.forEach((item) => item.remove())
              this.timer = setTimeout(() => {
                //Вызов задания с заполнением журнала
                this.$emit('assessmentStudents', this.assessmentStudent);
                this.$refs.evaluationOpacity.style.opacity = '0';
                this.$emit('lastWork');
              }, 1000)
            }, 1000)
          }
        } else {
          this.$emit('message', `В работе сделано ${this.mistakes} ошибок, рекомендуется поставить оценку ${this.assessmentStudent.evaluation}.`)
        }
      }
    },
    derangeArray() {
      if (this.student.length > 1) {
        this.derangedStudent = [...this.student]; // Копируем исходный массив объектов

        function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            let j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]]; // Меняем местами элементы
          }
        }

        do {
          shuffle(this.derangedStudent); // Перемешиваем массив
        } while (this.derangedStudent.some((val, idx) => val === this.student[idx]));
      } else {
        this.derangedStudent = [...this.student]
      }
    },
  },
}
</script>

<style scoped>
  .copybook {
    background-image: url("../../../public/images/Work_check_book.jpg");
    background-size: cover;
    padding: 1rem 0 0 12rem;
    display: flex;
    flex-direction: column;
    position: relative;
    width: 131vh;
    height: 77vh;
    margin-top: 4rem;
    animation: none;
  }
</style>