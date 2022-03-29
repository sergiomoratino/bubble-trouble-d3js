<template>
  <h1>Colores Utilizados: {{ countColors() }}</h1>
  <div id="my_dataviz"></div>
  <ModalPop
    :colorCircleSave="colorCircleSave"
    :idCircleSave="idCircleSave"
    :arrayColors="arrayColors"
    :key="colorCircleSave"
    v-show="isModalVisible"
    @close="closeModal"
    @selectColor="selectColor($event)"
    @changeC="changeC($event)"
  />
</template>

<script>
import * as d3 from "d3";
import dataset from "../../public/input.json";
import colours from "../../public/output.json";
import ModalPop from "./Modal.vue";
export default {
  name: "PackChart",
  components: {
    ModalPop,
  },
  props: ["bubbleData"],
  data() {
    return {
      msg: "From the Chart Component",
      dataset,
      colours,
      isModalVisible: false,
      colorCircleSave: "#000000",
      idCircleSave: 0,
      arrayColors: "#000000",
      newColor: "#000000",
      nodes: [],
    };
  },

  mounted() {
    this.packChart();
  },
  /* watch: {
      this.$refs.modal.close()
  },*/
  methods: {
    packChart() {
      const pairsBubbles = dataset.pairs;
      let nodes = [];
      let edges = [];
      let colorNodes = this.declareNumberColor();
      for (let i = 0; i < pairsBubbles.length; i++) {
        nodes.push(pairsBubbles[i].n1);
        nodes.push(pairsBubbles[i].n2);
        edges.push({ source: pairsBubbles[i].n1, target: pairsBubbles[i].n2 });
      }
      nodes = [...new Set(nodes)];
      for (let i = 0; i < nodes.length; i++) {
        nodes[i] = { id: nodes[i], name: nodes[i], color: colorNodes[i].color };
      }

      this.nodes = nodes;

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
          this.colorCircleSave = dataCircle.color;
          this.idCircleSave = dataCircle.id;
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
          })
          .attr("id", function (d) {
            return d.id;
          })
          .style("fill", function (d) {
            return d.color;
          });
      }
      return simulation;
    },

    selectColor() {
      this.showModal();
    },
    changeC(color) {
      this.newColor = color;
      let newColorPass = JSON.parse(JSON.stringify(this.newColor));
      let idCircleChange = newColorPass.idCircletoChange;
      let colorCircleChange = newColorPass.newColor;
      if (this.checkIfIsDiferentColorAround(idCircleChange, colorCircleChange)) {
        d3.selectAll("circle").each(function (d) {
          if (d.id === idCircleChange) {
            d3.select(this).style("fill", colorCircleChange);
            d.color = colorCircleChange;
          }
        });
        return this.countColors();
      }
    },

    // Count total of colors Used
    countColors() {
      let numberOfColors = [];
      d3.selectAll("circle").each(function (d, i) {
        numberOfColors[i] = d.color;
      });
      numberOfColors = [...new Set(numberOfColors)];
      numberOfColors = numberOfColors.length;
      return numberOfColors;
    },
    // Check if color that Bubble around is different
    checkIfIsDiferentColorAround(idCircle, newColor) {
      let isDiferent = true;
      d3.selectAll("circle").each(function (d) {
        if (d.id === idCircle) {
          d3.selectAll("line").each(function (e) {
            if (d.id == e.source.index) {
              if (newColor === e.target.color) {
                alert("Este color está utilizado en una conexión");
                isDiferent = false;
              }
            }
            if (d.id == e.target.index) {
              if (newColor === e.source.color) {
                isDiferent = false;
              }
            }
          });
        }
      });
      return isDiferent;
    },
    // Insert Color Code to number
    declareNumberColor() {
      const dataColours = colours.assignment;
      let nodeColor = [];
      for (let i = 0; i < dataColours.length; i++) {
        nodeColor.push(dataColours[i]);
      }
      nodeColor.forEach((d) => {
        switch (d.color) {
          case 0:
            d.color = "#FF5733";
            break;
          case 1:
            d.color = "#FFC433";
            break;
          case 2:
            d.color = "#FFFE33";
            break;
          case 3:
            d.color = "#B6FF33";
            break;
          case 4:
            d.color = "#33FFEE";
            break;
          case 5:
            d.color = "3369FF";
            break;
          case 6:
            d.color = "#A033FF";
            break;
        }
      });
      this.arrayColors = nodeColor;
      return nodeColor;
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    output() {
      return this.packChart();
    },
  },
};
</script>
