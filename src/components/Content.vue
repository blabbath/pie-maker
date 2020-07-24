<template>
    <div class="content-area">
        <div class="title-chart">
            <p class="chart-title">{{contentTitle}}</p>
            <app-chart v-bind:colorArr="colorArr"></app-chart> 
        </div>
        <div class="legend-container">
            <app-legend v-bind:colorArr="colorArr"></app-legend>
        </div>
    </div>
</template>

<script>
import Chart from './Chart';
import Legend from './Legend';
import { TitleBus } from '../event-busses/title-bus';

export default {
    components: {
        'app-chart': Chart,
        'app-legend': Legend
    },
    data () {
        return {
            contentTitle: 'Please enter a title',
            //chartData: [],
            colorArr: [
                '#7bd389',
                '#48599f',
                '#842c41',
                '#8fcfda',
                '#d685d6',
                '#bcc043',
                '#246c59',
                '#c249b6',
                '#a8df9e',
                '#c77858',
                '#2e4b8a'
            ]
        }
    },
    created(){
        TitleBus.$on('titleChanged', (data) => {
            if(!data) {
                this.contentTitle = 'Please enter a title'
            } else {
                this.contentTitle = data
            }
        })
    }
}
</script>

<style scoped>
    .content-area {
        display: flex;
        justify-content: space-evenly;
    }

    .title-chart {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .chart-title {
      font-size: 32px;
      text-align: center;
      letter-spacing: .25rem;
      line-height: 3.5rem;
      padding: 1rem 0 2rem 0;
    }
</style>
