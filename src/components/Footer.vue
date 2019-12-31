<template>
  <div>
   <footer>
       <p>{{copyright}}</p>
   </footer>
  </div>
</template>

<script>

// import {bus} from '../main'

export default {
     props:{
        title:{
            type:String
        }
    },
    data:function(){
        return{
            copyright:'Muhammad Sufyan @copyright-2019'
        }
    },
    // created(){
    //     bus.$on('changeTitle',(data)=>{
    //         this.title = data;
    //     })
    // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
footer{
    background: #222;
    padding: 6px;
    margin-top: 6vh
}
p{
    color: aliceblue;
    text-align: center;
}
</style>
