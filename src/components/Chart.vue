<template>
    <div id="pie">
    </div>
</template>

<script>
import * as d3 from 'd3';
import { ChartDataBus } from '../event-busses/chart-data-bus';

export default { 
    props: ['colorArr'],
    data() {
        return {
            chartData: [],
            width: 500,
            height: 600,
            color: d3.scaleOrdinal().range(this.colorArr),
            svg: {},
            g: {},
            path: {},
            tooltip: {}
        }
    },
    
    methods: {
        initPie() {
            this.svg = d3.select('#pie')
                .append('svg')
                .attr('width', this.width)
                .attr('height', this.height)

            this.g = this.svg.append('g')
                .attr('transform', 'translate(250,300)');

            this.tooltip = d3.select("body")
                .append("div")
                .style("opacity", 0)
                .attr("class", "tooltip")
        },

        renderPie(data){
            let pie = d3.pie()
                .value(d => d.value)
                .sort(null)

            let arc = d3.arc()
                .innerRadius(0)
                .outerRadius(Math.min(this.width, this.height) / 2 - 1)

            this.path = this.g.datum(data).selectAll('path')
                .data(pie)
                .enter().append('path')
                    .attr('class','piechart')
                    .attr('fill', (d, i) => this.color(i))
                    .attr('opacity', 0.8)
                    .attr('d', arc)
                    .on("mouseover", this.mouseover)
                    .on("mousemove", this.mousemove)
                    .on("mouseleave", this.mouseleave)
                    .each(function(d){ this._current = d; })

            function arcTween(d) {
                let i = d3.interpolate(d.startAngle+0.1, d.endAngle);
                return function(t) {
                    d.endAngle = i(t)
                    return arc(d)
                }
            }
            // add transition to new path
            this.g.datum(data).selectAll('path')
                .data(pie).transition().duration(800).attrTween('d', arcTween)
            // remove data not being used
            this.g.datum(data).selectAll('path')
                .data(pie).exit().remove();
        },
        
        mouseover(d) {
            this.tooltip
                .style("opacity", 1)
                .html(d.data.name + ': ' + d.data.value)
                .attr('style', 'position: absolute')
/*                 .style("background-color", "rgba(51, 51, 51, .8)")
                .style("padding", "8px 15px")
                .style("color", "white")
                .style("border-radius", ".15rem")
                .style("border-style", "none") */
        },

        mousemove(d) {
            this.tooltip
                .style("top", (d3.event.pageY-30)+"px") 
                .style("left",(d3.event.pageX+40)+"px"); 
        },

        mouseleave(d) {
            this.tooltip.style("opacity", 0)
        }
    },

    mounted() {
        this.initPie()
        ChartDataBus.$on('chartDataChanged', (data) => {
            if (!data[0]) {
                this.chartData.length = 0
            } else {
                this.chartData.push({
                    name: data[0],
                    value: data[1]
                })
                
            }
            this.renderPie(this.chartData)
        })
    }  
}
</script>

<style >
    .tooltip {
        background-color: rgba(51, 51, 51, .8);
        padding: 8px 15px;
        color: white;
        border-radius: .15rem;
        border-style: none;
    }
</style>