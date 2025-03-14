<template>
    <h3>机构共现网络</h3>
    <select v-model="clusterNum">
        <option v-for="idx of clusterType" :value="idx">{{ idx }}</option>
    </select>
    <button @click="onSubmit">enter</button>
    <div class="instNetBox"></div>
</template>

<script>
import * as d3 from 'd3'
import { onMounted, ref } from 'vue'
// import testGraph from '../../../data/d3TestData2.json'
import testGraph from '../../../output_data/instNet_Draw.json'

export default {


    setup() {
        const GraphData = ref(testGraph)
        const clusterNum = ref('')
        // n.value = 5
        const clusterType = [0, 1, 2, 3, 4, 5, 6]

        onMounted(() => {
            console.log(GraphData.value);
            // initGraph(GraphData.value, n.value)
        })

        function onSubmit() {
            // console.log(clusterNum.value);
            // initGraph(GraphData.value, clusterNum.value)

        }

        function initGraph(data, n) {
            // Specify the dimensions of the chart.
            const width = 600;
            const height = 600;
            // n = 6;
            // Specify the color scale.
            const color = d3.scaleOrdinal()
                .domain(clusterType)
                .range(d3.schemeCategory10);
            // const color = d3.scaleOrdinal(d3.schemeCategory10);
            const rScale = d3.scaleLinear()
                .domain([0, 300])  // 输入范围
                .range([10, 150]);
            const widthScale = d3.scaleLinear()
                .domain([0, 50])
                .range([1, 100]);
            const alpgaScale = d3.scaleLinear()
                .domain([0, 50])
                .range([0, 1]);
            const svgScale = d3.scaleLinear()
                .domain([200, 0])
                .range([0.1, 0.25]);
            // The force simulation mutates links and nodes, so create a copy
            // so that re-evaluating this cell produces the same result.
            const links = data.links.filter(d => d.cluster === n).map(d => ({ ...d }));
            const nodes = data.nodes.filter(d => d.cluster === n).map(d => ({ ...d }));

            console.log(links);
            console.log(nodes);


            // Create a simulation with several forces.
            const simulation = d3.forceSimulation(nodes)
                .force("link", d3.forceLink(links).id(d => d.id).distance(150)
                    .strength((link) => alpgaScale(link.value)))
                .force("charge", d3.forceManyBody().strength(-50))
                // 离心力
                .force("collide", d3.forceCollide().radius(() => 150))
                // 节点间最小半径
                .force("center", d3.forceCenter(width / 2, height / 2))
                .on("tick", ticked);

            // Create the SVG container.
            d3.select('.instNetBox').selectAll("svg").remove();
            const svg = d3.select('.instNetBox')
                .append("svg")
                .attr("width", width)
                .attr("height", height)
                .attr("viewBox", [0, 0, width, height])
                .attr("style", "max-width: 100%; height: auto;");

            const g = svg.append('g')
            const zoom = d3.zoom().on("zoom", function (event) {
                g.attr("transform", event.transform)
            })
            // 设置初始缩放比例——面对过多节点，让视图变小
            const initialTransform = d3.zoomIdentity
                .translate(width / 2, height / 2)
                .scale(svgScale(nodes.length))
                .translate(-width / 2, -height / 2);

            // 为svg添加鼠标缩放功能
            svg.call(zoom)
            svg.call(zoom.transform, initialTransform)

            // Add a line for each link, and a circle for each node.
            const link = g.append("g")
                .attr("stroke", "#fff")
                .selectAll()
                .data(links)
                .join("line")
                .attr("stroke-opacity", d => alpgaScale(d.value))
                .attr("stroke-width", d => widthScale(d.value));

            const node = g.append("g")
                .attr("stroke", "#fff")
                .selectAll()
                .data(nodes)
                .join("circle")
                .attr("stroke-width", d => rScale(d.coop) / 4)
                .attr("r", d => rScale(d.coop))
                .attr("fill", d => color(d.cluster));

            node.append("title")
                .text(d => d.id);
            link.append("title")
                .text(d => d.source.id + " - " + d.target.id + ":" + d.value)

            // // Add a drag behavior.
            // node.call(d3.drag()
            //     .on("start", dragstarted)
            //     .on("drag", dragged)
            //     .on("end", dragended));

            // Set the position attributes of links and nodes each time the simulation ticks.
            function ticked() {
                link
                    .attr("x1", d => d.source.x)
                    .attr("y1", d => d.source.y)
                    .attr("x2", d => d.target.x)
                    .attr("y2", d => d.target.y);

                node
                    .attr("cx", d => d.x)
                    .attr("cy", d => d.y);
            }

            // Reheat the simulation when drag starts, and fix the subject position.
            function dragstarted(event) {
                if (!event.active) simulation.alphaTarget(0.3).restart();
                event.subject.fx = event.subject.x;
                event.subject.fy = event.subject.y;
            }

            // Update the subject (dragged node) position during drag.
            function dragged(event) {
                event.subject.fx = event.x;
                event.subject.fy = event.y;
            }

            // Restore the target alpha so the simulation cools after dragging ends.
            // Unfix the subject position now that it’s no longer being dragged.
            function dragended(event) {
                if (!event.active) simulation.alphaTarget(0);
                event.subject.fx = null;
                event.subject.fy = null;
            }
        }



        return {
            clusterType,
            clusterNum,
            onSubmit
        }
    }
}

</script>


<style scoped>
.instNetBox {
    width: max-content;
    height: max-content;
    border: 1px solid #441907;
    border-radius: 8px;
    margin-top: 40px;
    margin-left: auto;
    margin-right: auto;
    background: #441907 repeating-linear-gradient(30deg,
            hsla(0, 0%, 100%, .1), hsla(0, 0%, 100%, .1) 15px,
            transparent 0, transparent 30px);
}
</style>