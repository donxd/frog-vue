<template>
  <div id="frog">
    Hi frog - {{boxes}}!
    <table id="elements">
      <tr>
          <!-- :class="{CSS_STYLE_FROG: box.selected===true}" -->
        <td 
          v-for="(box, index) in boxesElements" 
          :class="getBoxClass(box)"
          :key="`box-${index}`">{{getIndex(box)}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
  const DEFAULT_BOXES = 5;
  const DEFAULT_SELECTED = 1;
  const DEFAULT_CONFIG = false;
  const CSS_STYLE_FROG = 'frog';

  export default {
    name: 'Frog',
    props: {
      boxes: {
        type: Number,
        required: false,
        default: DEFAULT_BOXES
      },
      selected: {
        type: Number,
        required: false,
        default: DEFAULT_SELECTED
      },
      styleSelected: {
        type: String
      }
    },
    data() {
      return {}
    },
    created() {
      // console.log('initialize Frog!');
      // console.log('boxes : ', this.boxes);
      // console.log('boxes : ', this.selected);
      // this.selectionStyle = 'frog';
      this.generateObjectBoxes();
    },
    methods: {
      generateObjectBoxes () {
        this.boxesElements = [];
        for (let i = 0; i < this.boxes; i++) {
          const box = this.getNewBoxObject(i+1, this.selected);

          this.boxesElements.push(box);
        }
      },
      getNewBoxObject (index, itemSelected) {
        const attrSelected = index === itemSelected;

        return this.getBoxObject(index, attrSelected);
      },
      getBoxObject (index, attrSelected) {
        return {
          selected: attrSelected, 
          index
        };
      },
      getIndex (item) {
        return item.index;
      },
      getBoxClass (item) {
        const resolve = {};
        resolve[CSS_STYLE_FROG]= item.selected;
        return resolve;
      }
    }
  }
</script>

<style scoped>
  #elements td {
    width: 50px;
    height: 50px;
    background-color: orange;
    text-align: center;
    border: 2px solid orange;
  }
  #elements td:hover {
    border: 2px solid red;
  }
  .frog {
    color: rgba(0,0,0,0);
    background-image: url('~@/assets/frog.gif');
    background-size: 100%;
    background-position: center center;
    background-repeat: no-repeat;
  }
  .table {
    display: table;
  }
  .table .row {
    display: table-row;
  }
  .table .row .cell {
    display: table-cell;
  }
  .vcenter {
    vertical-align: middle;
  }
</style>