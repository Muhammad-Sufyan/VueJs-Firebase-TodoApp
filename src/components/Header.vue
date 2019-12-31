<template>
  <div>
    <header>
      <h1 v-on:click="changeTitle">{{ title }}</h1>
    </header>
  
  </div>
</template>

<script>

// import { bus } from '../main';

export default {
   props:{
        title:{
            type:String
        }
    },
//   name: 'HelloWorld',
  // props: {
  //   msg: String
  // }
  data: function(){
    return {
      // title:'Vue Todo App'
    }
  },
  methods:{
    changeTitle:function(){
      // this.title="Vue Todo App"
      // this.$emit('changeTitle','Vue Todo App')
      this.title = 'Vue Todo App';
    //   bus.$emit('changeTitle','Vue Todo App');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header{
  background: lightgreen;
  padding: 10px
}
h1{
  color: #222;
  text-align: center;
}
</style>
