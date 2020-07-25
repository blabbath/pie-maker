<template>
    <div id="legend">
    </div>
</template>

<script>
import * as d3 from 'd3';
import { ChartDataBus } from '../event-busses/chart-data-bus';

export default {
    props: ['colorArr'],
    data() {
        return {
            width: 200,
            height: 700,
            svg: {},
            g: {},
            legendData: [],
            color: this.colorArr
        }
    },

    methods: {
        initLegend() {
            this.svg = d3.select('#legend')
                .append('svg')
                .attr('class', 'legend-svg')
                .attr('width', this.width)
                .attr('height', this.height)
            
            this.g = this.svg.append('g')
                .attr('transform', 'translate(20, 200)')
                .attr('id', 'legend-group')
        },

        clearLegend() {
            const legend = document.querySelectorAll("#legend-group");
            legend.forEach(e => e.innerHTML = '')
        },

        createLegend() {
            this.legendData.forEach((item, i) => {
                let legendRow = this.g.append('g')
                    .attr('class', 'legend-row')
                    .attr('transform', 'translate(0, '+ (i * 40) +')');
                legendRow.append('circle')
                    .attr('cx', 10)
                    .attr('cy', 10)
                    .attr('r', 12)
                    .attr('fill', this.color[i])
                legendRow.append('text')
                    .attr('x', 45)
                    .attr('y', 13)
                    .attr('text-anchor', 'end')
                    .text(item.name)
            })
        }
    },

    mounted() {
        this.initLegend();
        ChartDataBus.$on('chartDataChanged', (data) => {
            if (!data[0]) {
                this.legendData.length = 0
            } else {
                this.legendData.push({
                    name: data[0],
                })
            }
            this.clearLegend();
            this.createLegend();
        })
    }
}
</script>

<style>
    .legend-svg {
        display: flex;
        flex-direction: column;
        justify-content: center;
    }
</style>