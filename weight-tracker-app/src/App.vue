<script setup>
import { ref, shallowRef, watch, nextTick } from 'vue'
import Chart from 'chart.js/auto'
//Initialise an empty array so the inputed weights and date will be pushed into the empty array
const weights = ref([])


//Initialise the weight input to 0.0
  const weightInput = ref(0.0)


//Initialise the weight input submitted to 0.0
  const submittedInput = ref(0.0)


//Initialise the weight chart element to null
  const weightChartEl = ref(null)


//Initialise the weight chart to null
  const weightChart = shallowRef(null)


  //Display current weight
  const addWeight = () => {
 //Update the submitted input value to the weight input value
  submittedInput.value = weightInput.value;


 //Add weight and date to weights, which was initialised as an empty array
  weights.value.push({
    weight: submittedInput.value,
    date: new Date().getTime()
   })
  //Sort the date so the most recent date is above
   weights.value.sort((a,b) => b.date - a.date)
 }

/* Display weight chart
Using the watch function and nextTick function to display the chart. */
watch (weights, newWeights  => {
   const ws = [...newWeights]
 if(weightChart.value){
    weightChart.value.data.labels = ws
        .sort((a,b) => a.date - b.date)
        .map(w  => new Date(w.date).toLocaleDateString())
        .slice(-7)


    weightChart.value.data.datasets[0].data = ws
        .sort((a,b) => a.date - b.date)
        .map(w  => w.weight)
        .slice(-7)


        weightChart.value.update()
       return
       }


  nextTick(() => {
    weightChart.value = new Chart(weightChartEl.value.getContext('2d'),
      {
      type: 'line',
      data: {
        labels: ws
        .sort((a,b) => a.date - b.date)
        .map(w  => new Date(w.date).toLocaleDateString()),
        datasets: [
          {
            label: 'Weight',
            data: ws
            .sort((a,b)=> a.date - b.date)
            .map(w => w.weight),
            backgroundColor: 'rgb(104, 7, 71,0.2)',
            borderColor: 'rgb(104, 7, 71)',
            borderWidth: 1,
            fill: true
          }
        ]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false
      }
    })
  })
 
 }
 , {deep: true})
</script>

<template>
<div>
   <header class='header'>
      <h1>Weight Tracker App</h1>
    </header>
  <main class="main">
      <div class="current-container">
    <div class="current">
      <h3>{{ submittedInput }}</h3>
      <h4>Current weight (kg)</h4>
      </div>
    </div>
    <form @submit.prevent="addWeight" class="form">
  <input type="number" step="0.1" v-model="weightInput" class="input-number"/><br/>
        <input type="submit" value="Add weight" class="input-submit"/>
        </form>


 <div v-if="weights && weights.length > 0">
           <h2>Recent Weights</h2>
    <div class="chart-section">
      <canvas ref="weightChartEl"></canvas>
    </div>
    <h2>Weight History</h2>
    <ul>
   <li v-for="weight in weights" v-bind:key="weight.date" class="weight-list">
     <span>{{ weight.weight }}kg</span>
     <small>{{ new Date(weight.date).toLocaleDateString() }}</small>
                  </li>
                </ul>
    </div>
    </main>
   </div>
</template>

<style>

</style>