<template>
    <div id="container_pie">
    </div>
</template>
<script>
import base from './base.vue'
import _ from 'lodash'
import Highcharts from 'highcharts'

export default {
    extends: base,
    methods: {
        dataSource(){
            const languages = this.localList.map(item=> item.language)

            const base = _(languages)
            .countBy()
            .map((y, name) => {
                return {y,name}
            })
            .orderBy(['y'], ['name'])
            .value()

            const categories = base.map(item => item.y)

            const values = base.map(item => item.name)

            const subset = base.slice(0,10)

            const total = subset.reduce((acc,item)=> {
                return acc + item.y
            }, 0)

            const finalData = subset.map(item => {
                item.y = ( item.y / total ) * 100
                return item
            })
            if(this.chart === null){
                this.chart = this.setup({finalData})
            }
            else {
                this.chart.series[0].setData(finalData)
            }
        },
        setup(obj){
            const { finalData } = obj;
            return Highcharts.chart('container_pie', {
                chart: {
                    plotBackgroundColor: null,
                    plotBorderWidth: null,
                    plotShadow: false,
                    type: 'pie'
                },
                title: {
                    text: 'Languages'
                },
                tooltip: {
                    pointFormat: '{series.name}: <b>{point.percentage:.1f}%</b>'
                },
                plotOptions: {
                    pie: {
                        allowPointSelect: true,
                        cursor: 'pointer',
                        dataLabels: {
                            enabled: true,
                            format: '<b>{point.name}</b>: {point.percentage:.1f} %',
                            style: {
                                color: (Highcharts.theme && Highcharts.theme.contrastTextColor) || 'black'
                            }
                        }
                    }
                },
                series: [{
                    name: 'Brands',
                    colorByPoint: true,
                    data: finalData
                }]
            });
        }
    }
}
</script>
