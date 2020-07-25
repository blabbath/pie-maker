<template>
    <div>
        <div class="clr-row">
            <div class="clr-col-9 title-col">
                <p class="chart-title">{{contentTitle}}</p>
            </div>
            <div class="clr-col-3">
            </div>
        </div>
        <div class="clr-row">
            <div class="clr-col-9 chart-col">
                <div class="clr-row">
                    <app-chart v-bind:colorArr="colorArr"></app-chart> 
                </div>
            </div>
            <div class="clr-col-3 legend-col">
                <app-legend v-bind:colorArr="colorArr"></app-legend>
            </div>
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
    .title-col {
        display: flex;
        justify-content: center;
    }

    .chart-col {
        display: flex;
        justify-content: center;
    }

    .chart-title {
      font-size: 32px;
      text-align: center;
      letter-spacing: .25rem;
      line-height: 3.5rem;
      padding: 1rem 0 2rem 0;
    }
</style>
