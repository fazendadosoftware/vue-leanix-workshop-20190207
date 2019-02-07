<template>
  <div id="app">
    <fact-sheet-types></fact-sheet-types>
    <!--<fact-sheet-types></fact-sheet-types>-->
    <!--
    <h1 class="example" @click="clickHandler">{{msg}}</h1>
    <div v-for="(node, idx) in response" :key="idx">
      <div style="padding: 1rem">{{node.name}}</div>
    </div>
    -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import FactSheetTypes from './components/FactSheetTypes'

export default {
  name: 'app',
  components: {
    // HelloWorld
    FactSheetTypes
  },
  data () {
    return {
      msg: 'Hello world!',
      response: []
    }
  },
  methods: {
    clickHandler (evt) {
      this.counter++
    },
    queryGraphQL () {
      this.$lx.executeGraphQL(`{
        allFactSheets {
          edges {
            node {
              id
              name
              type
              description
            }
          }
        }
      }`)
        .then(res => {
          this.response = res['allFactSheets']['edges']
            .map(node => node.node)
          console.log('response', this.response)
        })
        .catch(err => {
          console.log('something went wrong', err)
        })
    }
  },
  created () {
    this.$lx.init()
      .then(setupInfo => {
        // Process setupInfo and create config object
        const config = {}
        this.$lx.ready(config)
        this.queryGraphQL()
      })
  },
  mounted () {
    console.log('im mounted')
  },
  beforeDestroy () {
    console.log('im being destroyed')
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.example {
  cursor: pointer;
}

</style>
