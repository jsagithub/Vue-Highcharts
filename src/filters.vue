<template>
    <div>
        Filter by gender
        <select @change="commit" v-model="gender">
            <option value="">All</option>
            <option value="Male">M</option>
            <option value="Female">F</option>
        </select>
    </div>
</template>
<script>
export default {
    data(){
        return {
            gender: ''
        }
    }, 
    methods:{
        commit(){
            this.$store.commit('SET_FILTER', this.gender);
        }
    }
}
</script>
