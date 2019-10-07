<template>
<div>
     <h1>this is dashboard</h1>
     <b-row>
         <b-col><bar-example></bar-example></b-col>
         <b-col><bar-example></bar-example></b-col>
     </b-row>
    <router-view/>
</div>
   
</template>
<script>
import BarExample from '../sample/charts/BarExample'
export default{
    components : {BarExample}
}
</script>