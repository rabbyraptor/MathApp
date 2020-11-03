<template>
  <ion-app>
    <ion-page>
      <ion-header :translucent="true">
        <ion-toolbar>
          <ion-title>MathApp</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-content :fullscreen="true">
        <!-- <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header> -->

        <div id="container">
          <div class="timer">
            
            <h2><strong style="font-size:75%;">Time<br></strong>00:12</h2>
            </div>
          <div class="flower">
            <div class="stem"></div>

            <div class="petals">
              <div class="problem">
                <h3 v-if="!gameStarted" @click="startGame()">START</h3>
                <h3 v-else>{{ problemOne }} + {{ problemTwo }}</h3>
              </div>
              <div class="petal" id="p1">
                <h1 @click="checkAnswer(petalOne)">{{ petalOne }}</h1>
              </div>
              <div class="petal" id="p2">
                <h1 @click="checkAnswer(petalTwo)">{{ petalTwo }}</h1>
              </div>
              <div class="petal" id="p3">
                <h1 @click="checkAnswer(petalThree)">{{ petalThree }}</h1>
              </div>
              <div class="petal" id="p4">
                <h1 @click="checkAnswer(petalFour)">{{ petalFour }}</h1>
              </div>
            </div>
          </div>
          <div class="score">
            <h3>Score multiplier: x{{ multiplier }}</h3>
            <h3>Correct answers: {{ score }}</h3>
          </div>
          <p style="color:#aaa; margin-top:32px;">Version 0.1.1</p>
        </div>
      </ion-content>
    </ion-page>
  </ion-app>
</template>

<script>
import { IonApp, IonPage, IonContent, IonHeader, IonToolbar, IonTitle } from "@ionic/vue";
export default {
  data() {
    return {
      gameStarted: false,
      score: 0,
      multiplier: 1,
      problemOne: null,
      problemTwo: null,
      correctAnswer: null,
      petalOne: null,
      petalTwo: null,
      petalThree: null,
      petalFour: null,
    };
  },
  methods: {
    startGame() {
      if (this.gameStarted == false) {
        this.getProblem();
        this.getCorrectAnswer();
        this.assignNumbers();
        this.assignCorrectAnswer();
        this.gameStarted = true;
      }
    },
    getProblem() {
      this.problemOne = this.getRandomInt();
      this.problemTwo = this.getRandomInt();
    },
    getRandomInt() {
      return Math.ceil(Math.random() * 10);
    },
    assignNumbers() {
      const takenNumbers = [];
      this.petalOne = this.getRandomInt();
      takenNumbers.push(this.petalOne);
      this.petalTwo = this.getRandomInt();
      if (!takenNumbers.includes(this.petalTwo)) {
        takenNumbers.push(this.petalTwo);
        this.petalThree = this.getRandomInt();
        if (!takenNumbers.includes(this.petalThree)) {
          takenNumbers.push(this.petalThree);
          this.petalFour = this.getRandomInt();
        }
        if (!takenNumbers.includes(this.petalFour)) {
          takenNumbers.push(this.petalFour);
        }
      }

      this.petalFour = this.getRandomInt();
    },
    getCorrectAnswer() {
      this.correctAnswer = this.problemOne + this.problemTwo;
    },
    assignCorrectAnswer() {
      const number = Math.ceil(Math.random() * 4);
      if (number == 1) {
        this.petalOne = this.correctAnswer;
      } else if (number == 2) {
        this.petalTwo = this.correctAnswer;
      } else if (number == 3) {
        this.petalThree = this.correctAnswer;
      } else if (number == 4) {
        this.petalFour = this.correctAnswer;
      }
    },
    checkAnswer(answer) {
      if (answer == this.correctAnswer) {
        this.score += (1 * this.multiplier);
        if(this.score >= 10){
          this.multiplier = 2;
        }
        this.nextProblem();
      }
    },
    nextProblem() {
      this.getProblem();
      this.getCorrectAnswer();
      this.assignNumbers();
      this.assignCorrectAnswer();
    },
  },
  components: {
    IonPage,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonApp,
    IonContent
  },
};
</script>

<style>
h1,
h2,
h3,
h4 {
  margin: 0 !important;
}
.timer {
  height: 20vw;
  width: 60%;
  padding: 16px;
  box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: center;
  align-items: center;
  margin: auto;
  margin-bottom: 32px;
  border-radius: 5%;
}

.petals {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  position: relative;
}
.problem {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: lightyellow;
  height: 30vw;
  width: 30vw;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color:#333;
}
.petal {
  height: 50vw;
  width: 50vw;
  background-color: lightpink;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.petal h1 {
  font-weight: 800;
}
#p1,
#p2 {
  margin-bottom: -12.5%;
}
#p3,
#p4 {
  margin-top: -12.5%;
}
#p1,
#p3 {
  justify-self: right;
  margin-right: -12.5%;
}
#p2,
#p4 {
  justify-self: left;
  margin-left: -12.5%;
}
.score {
  margin-top: 32px;
}

#container {
  padding-top: 16px;
  padding-bottom: 16px;
  text-align: center;
}
</style>
