<template>
  <div class="modal-backdrop">
    <div class="modal">
      <header class="modal-header">
        <slot name="header"> Selecciona un color </slot>
        <button type="button" class="btn-close" @click="close">x</button>
      </header>

      <section class="modal-body">
        <slot name="body">
          <div class="form__input">
            <v-swatches
              v-model="color"
              :swatches="swatches"
              row-length="6"
              shapes="circles"
              show-border
              popover-x="left"
            ></v-swatches>
          </div>

          Escoge un color
        </slot>
      </section>

      <footer class="modal-footer">
        <slot name="footer"> Gracias! </slot>
        <button type="button" class="btn-green" @click="close">Aceptar</button>
      </footer>
    </div>
  </div>
</template>
<script>
import VSwatches from "vue3-swatches";

// Import the styles too, globally
import "vue-swatches/dist/vue-swatches.css";
let colorsToArray = [];
let colors = [];
export default {
  name: "ModalPop",
  components: { VSwatches },
  props: { colorCircleSave: String, idCircleSave: Number, arrayColors: Array },
  watch: {
    colorCircleSave: function () {
    },
    idCircleSave: function () {},
    arrayColors: function (d) {
      colorsToArray = JSON.parse(JSON.stringify(d));
      colorsToArray.forEach((element) => {
        colors.push(element.color);
      });
      // Clean Repeat colors
      colors = [...new Set(colors)];
    },
  },

  data() {
    return {
      color: this.colorCircleSave,
      idCircle: this.idCircleSave,
      swatches: colors,
    };
  },

  methods: {
    close() {
      this.$emit("close");
      //Pass Id Node and New color to chart.vue
      this.$emit("changeC", {
        idCircletoChange: this.idCircleSave,
        newColor: this.color,
      });
    },
  },
};
</script>
<style>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #ffffff;
  width: 80%;
  height: 80%;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}

.modal-header,
.modal-footer {
  padding: 15px;
  display: flex;
}

.modal-header {
  position: relative;
  border-bottom: 1px solid #eeeeee;
  color: #4aae9b;
  justify-content: space-between;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  flex-direction: column;
  justify-content: flex-end;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
}

.btn-close {
  position: absolute;
  top: 0;
  right: 0;
  border: none;
  font-size: 20px;
  padding: 10px;
  cursor: pointer;
  font-weight: bold;
  color: #4aae9b;
  background: transparent;
}

.btn-green {
  color: white;
  background: #4aae9b;
  border: 1px solid #4aae9b;
  border-radius: 2px;
}
</style>
