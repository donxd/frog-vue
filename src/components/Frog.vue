<template>
  <div id="frog">
    Hi frog - {{boxesComponent}}!
    <div v-if="configComponent" class='table'>
      <div class='row'>
        <div class='cell'>
          <label for="nboxes">Número de cajas: &nbsp;</label>
        </div>
        <div class='cell'>
          <input type="number" min="1" 
            @change="changeInputBoxes()"
            v-model=boxesComponent>
        </div>
      </div>
      <div class='row'>
        <div class='cell'>
          <label for="pfrog">Posición rana: </label>
        </div>
        <div class='cell'>
          <input type="number" min="1" 
            @change="changeInputSelection()"
            v-model=selectedComponent>
        </div>
      </div>
    </div>
    <table id="elements">
      <tr>
        <td 
          v-for="(box,index) in boxesElements" 
          @click="jumpFrogAdv(box)"
          :class="getBoxClass(box)"
          :key="`${index}`"
          >{{getIndex(box)}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
  // import Vue from 'vue';

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
      config: {
        type: Boolean,
        required: false,
        default: DEFAULT_CONFIG
      }
    },
    data() {
      return {
        boxesComponent: 0,
        boxesElements: []
      };
    },
    created() {
      this.boxesComponent = this.boxes;
      this.selectedComponent = this.selected;
      this.configComponent = this.config;
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
      changeInputBoxes () {
        this.changeBoxes(this.boxesComponent, this.boxesElements.length);
      },
      changeBoxes (newNumberBoxes, oldNumberBoxes=DEFAULT_SELECTED) {
        this.changeBoxesByObjects(newNumberBoxes, oldNumberBoxes);
      },
      changeBoxesByObjects (newNumberBoxes, oldNumberBoxes) {
        if (newNumberBoxes > 0) {
          if (newNumberBoxes > oldNumberBoxes) {
            this.boxesComponent = newNumberBoxes;
            this.addBoxesByObjects();
          } else if (newNumberBoxes < oldNumberBoxes) {
            this.boxesComponent = newNumberBoxes;
            this.removeBoxesByObjects();
          }
        }
      },
      addBoxesByObjects () {
        const numberCreatedBoxes = this.boxesElements.length;
        const numberNewBoxes = this.boxesComponent - numberCreatedBoxes;

        for (let i = 0; i < numberNewBoxes; i++) {
          const newBox = this.getBoxObject(numberCreatedBoxes + 1, false);

          this.boxesElements.push(newBox);
        }
      },
      removeBoxesByObjects () {
        let positionFrog = this.getSelectionByBoxes();
        const numberCreatedBoxes = this.boxesElements.length;
        const numberBoxes = this.boxesComponent;

        if (positionFrog >= numberBoxes) {
          positionFrog = numberBoxes;
          this.selectedComponent = positionFrog;
          this.boxesElements[positionFrog-1].selected = true;
        }

        for (let i = numberCreatedBoxes-1; i >= numberBoxes; i--) {
          this.boxesElements.pop();
        }
      },
      changeInputSelection() {
        // console.log('changeInputSelection # selectedComponent new : ', this.selectedComponent);
        // console.log('changeInputSelection # selectedComponent prev : ', this.getSelectionByBoxes());
        this.changeSelected(this.selectedComponent, this.getSelectionByBoxes());
      },
      changeSelected (newPosition, oldPosition=DEFAULT_BOXES) {
        this.changeSelectedByObjects(newPosition, oldPosition);
      },
      changeSelectedByObjects (newPosition, oldPosition) {
        if (this.boxesElements && this.boxesElements.length && oldPosition > -1){
          // const positionFrog = this.getPositionFrogAdvanced();
          const positionFrog = oldPosition;
          const newPositionSelection = newPosition <= this.boxesElements.length ? newPosition : 1;

          this.selectedComponent = newPositionSelection;
          this.boxesElements[positionFrog-1].selected = false;
          this.boxesElements[newPositionSelection-1].selected = true;
        }
      },
      getSelectionByBoxes() {
        for (let box of this.boxesElements) if (box.selected) return box.index;
      },
      jumpFrogAdv (boxSelected) {
        if (boxSelected.selected) {
          const newBoxSelection = boxSelected.index < this.boxesElements.length ?
            this.boxesElements[ boxSelected.index ] : this.boxesElements[ 0 ];

          this.boxesElements[boxSelected.index-1].selected = false;
          this.boxesElements[newBoxSelection.index-1].selected = true;

          this.selectedComponent = newBoxSelection.index;
        }
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