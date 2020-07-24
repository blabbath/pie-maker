<template>
    <section class="sidenav-content">
        <div>
            <input type="text" class="clr-input input-title"
                v-on:input="updateTitle()" v-model="chartTitle"
                placeholder="Please enter a title"
            >
            <div class="chart-input">
                <form class="clr-form">
                    <div class="clr-input-container">
                        <label>Name: </label>
                        <input type="text" placeholder="Name" class="clr-input" v-model="name">
                    </div>
                    <div class="clr-input-container">
                        <label>Value: </label>
                        <input type="number" placeholder="Value" class="clr-input" v-model.number="value">
                    </div>
                    <div class="btn-holder">
                        <button type="button" v-on:click="updateChart()" class="btn btn-primary">
                            Add Data
                        </button>
                        <button type="button" v-on:click="clearData()" class="btn btn-warning">
                            Clear Data
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </section>
</template>

<script>
import { TitleBus } from '../event-busses/title-bus';
import { ChartDataBus } from '../event-busses/chart-data-bus';

export default {
    data () {
        return {
            chartTitle: '',
            name: '',
            value: '',
            data: []
        }
    },
    methods: {
        updateTitle() {
            TitleBus.$emit('titleChanged', this.chartTitle)
        },
        updateChart() {
            if(this.name && this.value) {
                this.data.push({
                    name: this.name,
                    value: this.value
                })
                ChartDataBus.$emit('chartDataChanged', [
                    this.name,
                    this.value,
                ]
                )
            }
        },
        clearData() {
            this.chartTitle = '';
            this.updateTitle();
            this.tableData = [];
            this.updateChart;
            ChartDataBus.$emit('chartDataChanged', [
                this.name = '',
                this.value = ''
            ])
        }
    }
}
</script>

<style scoped>
    nav {
        max-width: 20rem;
        min-width: 20rem;
    }

    .sidenav-content {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        padding: 3rem 2rem;
        max-width: 20rem;
        min-width: 20rem;
    }

    .input-title {
        width: 100%;
    }

    .chart-input {
        padding: 2rem 0 0rem 0;
    }

    .clr-input-container {
        padding: .4rem 0
    }

    .btn-holder {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 3rem 0 0 0;
    }
</style>
