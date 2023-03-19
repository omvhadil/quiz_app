<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { computed, ref } from 'vue'
import quiz from '../constans/'

const quizNumber = ref(0)
const Quis = computed(() => quiz[quizNumber.value])
const dataAnswer = ref([])
const jmlBenar = ref('')
const mode = ref('quiz')

const saveAnswer = (answer) => {
  let userAnswer = {
    question: quizNumber.value + 1,
    answer: answer
  }
  if (dataAnswer.value.length == 0) {
    dataAnswer.value.push(userAnswer)
  } else {
    let index = dataAnswer.value.findIndex((item) => item.question == userAnswer.question)
    if (index == -1) {
      dataAnswer.value.push(userAnswer)
    } else {
      dataAnswer.value[index].answer = userAnswer.answer
    }
  }
}
const getColor = (index) => {
  let indexAnswer = dataAnswer.value.findIndex((item) => item.question == index + 1)
  // 3 kondisi
  // 1. belum dijawab
  // 2. sedang dihalaman ini
  // 3. dijawab
  if (index == quizNumber.value) {
    return 'bg-warning'
  } else if (indexAnswer == -1) {
    return 'bg-light'
  } else {
    return 'bg-success'
  }
}
const getSubmit = () => {
  mode.value = 'result'
  for (let i = 0; i < dataAnswer.value.length; i++) {
    if (dataAnswer.value[i].answer == quiz[i].rightAnswer) {
      jmlBenar.value++
    }
  }
}

const jmlSoal = quiz.length
</script>
<template>
  {{ jmlSoal }}
  <div class="container-fluid bg-success p-3">
    <div class="content-quis bg-white p-3 rounded-4 h-100">
      <div v-if="mode == 'quiz'">
        <h4 class="text-center text-dark mb-3">Wellcome in Quis Application UI</h4>
        <div class="row">
          <div class="col-xl-8">
            <div>
              <div class="content-soal border border-3 border-success p-3 rounded">
                <h6>Question {{ quizNumber + 1 }}</h6>
                <p>
                  {{ Quis.question }}
                </p>
              </div>
              <div class="row mt-3 row-gap-3">
                <div v-for="answer in Quis.answers" :key="answer" class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    name="exampleRadios"
                    :id="answer"
                    :value="answer"
                    :checked="dataAnswer[quizNumber]?.answer == answer"
                    @click="saveAnswer(answer)"
                  />
                  <label
                    class="form-check-label w-100 rounded text-start shadow-sm border p-3"
                    :for="answer"
                    >{{ answer }}</label
                  >
                </div>
              </div>
            </div>
            <div class="text-center mt-3">
              <button
                @click="quizNumber--"
                :disabled="quizNumber == 0"
                class="btn shadow-sm border me-5"
              >
                Prev
              </button>
              <button
                v-if="quizNumber !== quiz.length - 1"
                @click="quizNumber++"
                class="btn shadow-sm border"
              >
                Next
              </button>
              <button v-else @click="getSubmit()" class="btn shadow-sm border">Finish</button>
            </div>
          </div>
          <div class="col-xl-4">
            <div class="shadow-sm border rounded p-3 mt-3 mt-lg-0">
              <div class="row">
                <div class="col-6 col-xl-6">
                  <p>Quetion {{ quizNumber + 1 }} / {{ jmlSoal }}</p>
                </div>
                <div class="col-6 col-xl-6">
                  <div class="text-end">
                    <a href="#" class="text-decoration-none text-dark">Need Help?</a>
                  </div>
                </div>
              </div>
              <div class="row row-gap-3">
                <div
                  v-for="(items, indx) in quiz"
                  :key="indx"
                  class="col-xl-2 col-2 p-0 d-flex justify-content-center"
                >
                  <button
                    class="border rounded-circle p-2"
                    style="width: 50px; height: 50px"
                    :class="getColor(indx)"
                    @click="quizNumber = indx"
                  >
                    {{ indx + 1 }}
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="mode == 'result'">
        <h1>Hasil {{ jmlBenar }} dari {{ jmlSoal }}</h1>
      </div>
    </div>
  </div>
</template>
<style scoped></style>
