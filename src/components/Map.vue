<template>
    <div>
        <pie-chart :chart-data="chartData" :options="chartOptions"></pie-chart>
    </div>
</template>

<script>
import { ChartDataBus } from '../event-busses/chart-data-bus'
import PieChart from "../PieChart";

export default {
    components: {
        PieChart
    },
    data() {
        return {
            chartOptions: {
                hoverBorderWidth: 20,
                responsive: true,
            },
            labels: [],
            data: [],
            chartData: {},      
        }
    },  
    methods: {
        updateData() {
            this.chartData = {
                hoverBackgroundColor: "red",
                hoverBorderWidth: 50,
                labels: this.labels,
                datasets: [
                    {
                        label: "Data One",
                        backgroundColor: ["#41B883", "#E46651", "#00D8FF"],
                        data: this.data
                    }
                ]
            }  
        }
    }, 
    mounted() {
        ChartDataBus.$on('chartDataChanged', (data) => {
            this.labels.push(data[0][0])
            this.data.push(data[1][0])
        })
    },
    updated() {
        this.updateData()
    }
}
</script>

<style>
    #doughnut-chart {
        width:200px
    }
</style>