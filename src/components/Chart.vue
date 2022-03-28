<template>
  <div id="my_dataviz"></div>
</template>

<script>
import * as d3 from "d3";
import dataset from "../../public/input.json";

export default {
  name: "PackChart",
  props: ["bubbleData"],
  data() {
    return {
      msg: "From the Chart Component",
      dataset,
    };
  },

  mounted() {
    this.packChart();
  },
  methods: {
    packChart() {
      const pairsBubbles = dataset.pairs;
      let nodes = [];
      let edges = [];
      for (let i = 0; i < pairsBubbles.length; i++) {
        nodes.push(pairsBubbles[i].n1);
        nodes.push(pairsBubbles[i].n2);
        edges.push({ source: pairsBubbles[i].n1, target: pairsBubbles[i].n2 });
      }
      nodes = [...new Set(nodes)];
      for (let i = 0; i < nodes.length; i++) {
        nodes[i] = { id: nodes[i], name: nodes[i] };
      }

      console.log("nodes", nodes);
      console.log("edges", edges);

      let margin = { top: 10, right: 30, bottom: 30, left: 40 },
        width = 800 - margin.left - margin.right,
        height = 800 - margin.top - margin.bottom;
      let svg = d3
        .select("#my_dataviz")
        .append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

      let link = svg
        .selectAll("line")
        .data(edges)
        .join("line")
        .style("stroke", "#aaa");

      // Initialize the nodes
      let node = svg
        .selectAll("circle")
        .data(nodes)
        .join("circle")
        .attr("r", 20)
        .style("fill", "#69b3a2")
        .on("click", (d, dataCircle) => {
            console.log(dataCircle)
            this.selectColor();
         });
        

      let simulation = d3
        .forceSimulation(nodes)
        .force(
          "link",
          d3
            .forceLink()
            .id(function (d) {
              return d.id;
            })
            .links(edges)
        )
        .force("charge", d3.forceManyBody().strength(-400)) // This force separate nodes.
        .force("center", d3.forceCenter(width / 2, height / 2)) // This force attracts nodes to the center of the svg area
        .on("end", ticked);
      function ticked() {
        link
          .attr("x1", function (d) {
            return d.source.x;
          })
          .attr("y1", function (d) {
            return d.source.y;
          })
          .attr("x2", function (d) {
            return d.target.x;
          })
          .attr("y2", function (d) {
            return d.target.y;
          });

        node
          .attr("cx", function (d) {
            return d.x + 6;
          })
          .attr("cy", function (d) {
            return d.y - 6;
          });
      }
      return simulation;
    },

    selectColor(){
        console.log("Seleciona el color");
    },

    output() {
      return this.packChart();
    },
  },
};
</script>
