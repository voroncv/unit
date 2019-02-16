<template>
  <div class="editor">

    <div class="mock-block" contenteditable>
      <div class="actions">
        <div class="dnd"></div>
      </div>
      <div class="content">
        <h3 class="title">Three Hours With the Late Jim Harrison</h3>
        <p class="descriptions">Madison, Wisconsin State capital and city A view of the skyline of the Madison Isthmus and Lake Mendota from Picnic Point.</p>
      </div>
    </div>

    <div class="unit-block" contenteditable>
      <div class="line-color red"></div>
      <p class="title">Three Hours With the Late Jim Harrison</p>
    </div>
    <div class="unit-block" contenteditable>
      <div class="line-color orange"></div>
      <p class="title">Three Hours With the Late Jim Harrison</p>
    </div>
    <!-- <draggable v-model="content" @start="drag=true" @end="drag=false">
      <div class="mock-block" v-for="elem in content" :key="elem.local_id" v-if="elem.type === 'TEXT'" contenteditable>
        <h3 class="content">{{elem.content}}</h3>
      </div>
    </draggable> -->
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'Editor',
  components: {
    draggable,
  },
  data() {
    return {
      content: [
        {
          "local_id": 1,
          "type": "TEXT",
          "content": "Madison, Wisconsin State capital and city A view of the skyline of the Madison Isthmus and Lake Mendota from Picnic Point."
        },
        {
          "local_id": 2,
          "type": "TEXT",
          "content": "А это просто тестовый элемент"
        },
        {
            "local_id": 2,
            "type": "UNIT",
            "title": "Three Hours With the Late Jim Harrison",
            "color": "RED",
            "content": "Madison, Wisconsin State capital and city."
        },
        {
            "local_id": 3,
            "type": "UNIT",
            "title": "Three Hours With the Late Jim Harrison",
            "color": "GREEN",
        }
      ]
    }
  },
};
</script>

<style>
.editor {
  width: 900px;
  margin: 0 auto;
  margin-top: 96px;
}
.mock-block {
  margin: 50px 10px;
  outline: none;
  display: flex;
  flex-direction: row;
}
.mock-block .actions {
  width: 40px;
  margin-left: -40px;
}
.mock-block .actions .dnd {
  width: 20px;
  height: 20px;
  border-radius: 100%;
  background-color: cadetblue;
}
.mock-block .title {
  font-family: Tahoma;
  font-weight: bold;
  margin: 0;
}
.mock-block .descriptions {
  font-family: Tahoma;
  font-weight: 300;
}
.unit-block {
  display: flex;
  flex-direction: row;
  outline: none;
}
.unit-block .line-color {
  width: 4px;
}
.unit-block .line-color.red {
  background-color: #ff7366;
}
.unit-block .line-color.orange {
  background-color: #fdba49;
}
.unit-block .line-color.yellow {
  background-color: #ffedaf;
}
.unit-block .line-color.green {
  background-color: #16b280;
}
.unit-block .title {
  font-family: Tahoma;
  font-size: 16px;
  margin-left: 15px;
}
</style>
