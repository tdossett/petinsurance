<template>
  <v-layout>
    <v-flex class="justify-center">
        <v-card class="pa-2" id="plot0"></v-card>
        <v-card class="pa-5" id="plot1"></v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import * as tf from '@tensorflow/tfjs'
import * as tfvis from '@tensorflow/tfjs-vis'

export default {
  metaInfo: {
    title: 'Home'
  },
  data () {
    return {
      plot: null
    }
  },
  mounted() {
    this.run()
  },
  methods: {
    async getData() {
      /**
       * Get the car data reduced to just the variables we are intersted
       * and cleaned of missing data
       */
      const carsDataReq = await fetch('https://storage.googleapis.com/tfjs-tutorials/carsData.json')
      const carsData = await carsDataReq.json()
      const cleaned = carsData.map(car => ({
        mpg: car.Miles_per_Gallon,
        horsepower: car.Horsepower,
      })).filter(car => (car.mpg != null && car.horsepower != null))

      return cleaned
    },
    async run() {
      // Load and plot the original inout data that we are going to train on.
      const data = await this.getData()
      const values = data.map(d => ({
        x: d.horsepower,
        y: d.mpg
      }))

      /* const series = ['Horsepower v MPG']
      tfvis.render.scatterplot(document.getElementById('plot1'), {values, series}, {
        height: 300,
        width: 550,
        xLabel: 'Horsepower',
        yLabel: 'MPG'
      }) */

      tfvis.render.scatterplot(
        { name: 'Horsepower v MPG' },
        {values},
        {
          xLabel: 'Horsepower',
          yLabel: 'MPG',
          height: 300
        }
      )

      // Create the model
      const model = this.createModel()
      tfvis.show.modelSummary({ name: 'Model Summary'}, model)

      // Convert the data to a form we can use for training
      const tensorData = this.convertToTensor(data)
      const {inputs, labels} = tensorData

      // Train the model
      await this.trainModel(model, inputs, labels)
      console.log('Done Training')

      // Make some predictions using the model and compare them to the
      // original data
      this.testModel(model, data, tensorData)

    },
    createModel() {
      // Create a sequential model
      const model = tf.sequential()

      // Add a single input layer
      model.add(tf.layers.dense({inputShape: [1], units: 1}))

      return model
    },
    convertToTensor(data) {
      /**
       * Convert the input to tensor that we can use for machine learning. 
       * We willdo our best pratices of _shuffling_ the data and
       * _normalizing_ the data MPG on te y-axis.
       */

      // Wrapping these calculations in a tidy will dispose any
      // intermediate tensors.
      return tf.tidy(() => {
        // Step 1. Shuffle the data
        tf.util.shuffle(data)

        // Step 2. Convert data to Tensor
        const inputs = data.map(d => d.horsepower)
        const labels = data.map(d => d.mpg)

        const inputTensor = tf.tensor2d(inputs, [inputs.length, 1])
        const labelTensor = tf.tensor2d(labels, [labels.length, 1])

        // Step 3. Normalize the data to the range 0 -1 using min-max scaling
        const inputMax = inputTensor.max()
        const inputMin = inputTensor.min()
        const labelMax = labelTensor.max()
        const labelMin = labelTensor.min()

        const normalizedInputs = inputTensor.sub(inputMin).div(inputMax.sub(inputMin))
        const normalizedLabes = labelTensor.sub(labelMin).div(labelMax.sub(labelMin))

        return {
          inputs: normalizedInputs,
          labels: normalizedLabes,
          // Return the min/max bounds so we can use them later
          inputMax,
          inputMin,
          labelMax,
          labelMin
        }
      })
    },
    async trainModel(model, inputs, labels) {
      // Prepare the model for training
      model.compile({
        optimizer: tf.train.adam(),
        loss: tf.losses.meanSquaredError,
        metrics: ['mse']
      })

      const batchSize = 32
      const epochs = 50

      return await model.fit(inputs, labels, {
        batchSize,
        epochs,
        shuffle: true,
        callbacks: tfvis.show.fitCallbacks(
          { name: 'Training Performance'},
          ['loss', 'mse'],
          { height: 200, callbacks: ['onEpochEnd'] }
        )
      })
    },
    testModel(model, inputData, normilizationData) {
      const { inputMax, inputMin, labelMin, labelMax } = normilizationData

      // Generate predictions for a uniform reange of numbers between 0 and 1
      // We un-normalize the data by doing the reverse of min-max scaling
      // that we did earlier
      const [xs, preds] = tf.tidy(() => {

          const xs = tf.linspace(0, 1, 100)
          const preds = model.predict(xs.reshape([100, 1]))

          const unNormXs = xs
            .mul(inputMax.sub(inputMin))
            .add(inputMin)

          const unNormPreds = preds
            .mul(labelMax.sub(labelMin))
            .add(labelMin)

          // Un-normalize the data
          return [unNormXs.dataSync(), unNormPreds.dataSync()]
        })

        const pretictedPoints = Array.from(xs).map((val, i) => {
          return {x: val, y: preds[i]}
        })

        const originalPoints = inputData.map(d => ({
          x: d.horsepower, y: d.mpg
        }))

        tfvis.render.scatterplot(
          {name: 'Model Predictions vs Original Data'},
          { values: [originalPoints, pretictedPoints], series: ['original', 'predicted']},
          {
            xLabel: 'Horsepower',
            yLabel: 'MPG',
            height: 300
          })
     
    }
  },
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>
