<template>
  <div class="classlist">
      <div class="add">
        <select v-model="currentType">
          <option :value="type" v-for="(type, index) in types" :key="type + index" :checked="currentType == type">{{type}}</option>
        </select>
        <button class="uk-button uk-button-primary" @click="add()">+</button>
      </div>
      <span class="list__header">Classnames</span>
      <ul class="list">
        <li class="list__item" v-for="(classname, index) in model.classnames" :key="'classname' + index"><span>{{classname}}</span><button class="uk-button uk-button-secondary uk-button-small" v-if="classname != 'row'" @click="remove(index)"><i class="uk-icon-trash"></i></button></li>
      </ul>
  </div>
</template>

<script>
const TYPE = {
  row: 'row',
  reverse: 'reverse',
  naturalHeight: 'natural-height',
  flexColumn: 'flex-column',
  start: 'start',
  center: 'center',
  end: 'end',
  middle: 'middle',
  bottom: 'bottom',
  around: 'around',
  between: 'between'
}
export default {
  mixins: [window.Storyblok.plugin],
  data: () => ({
    currentType: TYPE.reverse,
  }),
  methods: {
    initWith() {
      return {
        plugin: 'sassflexboxgrid-row',
        classnames: [TYPE.row]
      }
    },
    pluginCreated() {
      this.types = [TYPE.reverse, TYPE.naturalHeight, TYPE.flexColumn, TYPE.start, TYPE.center, TYPE.end, TYPE.middle, TYPE.bottom, TYPE.around, TYPE.between]
    },
    remove(index) {
      this.model.classnames.splice(index, 1)
    },
    search(classname) {

      let searchTerm = classname
      let position = -1

      this.model.classnames.some((item, index) => {
        const foundMatch = item.includes(searchTerm)
        if(foundMatch) {
          position = index
          return true
        }
        return false
      })

      return position

    },
    add() {
      
      const searchIndex = this.search(this.currentType)

      if(searchIndex == -1) {
        this.model.classnames.push(this.currentType)
      }
    }
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    },
  }
}
</script>

<style>
  .add {
    display: flex;
    align-items: center;
    flex: 1;
    margin-bottom: 10px;
  }
  
  .add span {
    display: inline-block;
    margin: 0 5px;
  }

  .add div {
    display: flex;
    align-items: center;
  }

  .uk-form .add select {
    padding: 0 25px 0 15px !important;
  }

  .uk-form .add button {
    margin-left: 5px;
  }

  .uk-form .list {
    padding: 0;
    margin: 0;
  }

  .uk-form .list .list__item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 10px;
  }
</style>
