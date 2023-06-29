<template>
    <div class="stats shadow">
  
  <div class="stat">
    <div class="stat-title">Nome: {{ stateName }}</div>
    <div class="stat-value text-success">{{ stateTotal }}</div>
    <div class="stat-desc">Habitantes com esse nome</div>
  </div>
  
</div>
</template>
<script>
export default{
    name: 'StateComponent',
    props: ["dataChart"],
    data(){
        return{
            stateName: null,
            stateTotal: null,
        }
    },

    beforeUpdate(){
        let newStateName = this.dataChart[0].nome;
        this.stateName = newStateName
   
        let newStateTotal = this.dataChart[0].res.reduce(( accumulator, object) => {
            return accumulator + object.frequencia
        },0)
        this.stateTotal = newStateTotal
    }



}
</script>