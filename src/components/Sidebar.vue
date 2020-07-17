<template>
    <nav class="sidenav">
        <section class="sidenav-content">
            <div>
                <input type="text" class="clr-input input-title"
                    v-on:input="changeTitle()" v-model="chartTitle"
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
                        <div class="clr-input-container">
                            <label>Color: </label>
                            <input type="color" class="clr-input" v-model="color" value="#e66465">
                        </div>
                        <div class="btn-holder input">
                            <button type="button" v-on:click="updateChart(); updateTable()" class="btn btn-primary">
                                Add
                            </button>
                        </div>
                    </form>
                </div>
                <table class="table table-compact table-noborder">
                    <thead>
                        <tr>
                            <th>Name</th>
                            <th>Value</th>
                            <th>Color</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in chartData" :key=index>
                            <td>{{item.name}}</td>
                            <td>{{item.value}}</td>
                            <td>{{item.color}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="btn-holder clear">
                <button  class="btn btn-warning" v-on:click="clearData()">Clear Data</button>
            </div>
        </section>
    </nav>
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
            color: '',
            chartData: [],
        }
    },
    methods: {
        changeTitle() {
            TitleBus.$emit('titleChanged', this.chartTitle)
        },
        updateChart() {
            if(this.name && this.value) {
                ChartDataBus.$emit('chartDataChanged', [
                    this.name,
                    this.value
                ])
            }
        },
        updateTable() {
            if(this.name && this.value) {
                this.chartData.push({
                    name: this.name,
                    value: this.value,
                    color: this.color
            })
        }
        this.name = '';
        this.value = '';
        this.color = '';
        },
        clearData() {
            this.chartData = [];
            this.chartTitle = '';
            this.updateChart;
            this.changeTitle();
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
        padding: 3rem 1rem;
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
        justify-content: center;
    }

    .btn-holder.input {
        padding: 2rem 0 0 0;
    }

    .btn-holder.clear {
        padding: 0 0 3rem 0;
    }
</style>
