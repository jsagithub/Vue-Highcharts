<template>
    <div id="container">
        Countries
    </div>
</template>
<script>
import _ from 'lodash'
import Highcharts from 'highcharts'
import base from './base.vue'
export default {
    extends: base,   
    methods: {
        // data optimization
        dataSource(){
            
            const countries = this.localList.map(item=> item.country)

            const base = _(countries)
            .countBy()
            .map((value, key) => {
                return {key,value}
            })
            .orderBy(['value'], ['desc'])
            .value()

            const categories = base.map(item => item.key)

            const values = base.map(item => item.value)

            if (this.chart === null){
                this.chart = this.setup({ categories, values})
            }
            else {
                this.chart.series[0].setData(values)
            }
        },
        //highcharts code
        setup(obj){
            const { categories, values } = obj

            return Highcharts.chart('container', {
                chart: {
                    type: 'column'
                },
                title: {
                    text: 'Countries'
                },
                subtitle: {
                    text: 'Source: WorldClimate.com'
                },
                xAxis: {
                    categories: categories,
                    crosshair: true
                },
                yAxis: {
                    min: 0,
                    title: {
                        text: 'Countries'
                    }
                },                
                series: [{
                    name: 'Qnt',
                    data: values

                }]
            });
        }
    }
}
</script>
