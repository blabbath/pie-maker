<template>
    <div class="content-area">
        <p class="chart-title">{{contentTitle}}</p>
        <app-map></app-map> 
    </div>
</template>

<script>
import Map from './Map';
import { TitleBus } from '../event-busses/title-bus';

export default {
    components: {
        'app-map': Map
    },
    data () {
        return {
            contentTitle: 'Please enter a title',
            chartData: []
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
