<template>
  <div class="editor">
    <div class="container">
      <div class="row">
        <div class="page-content">
          <draggable v-model="content" @start="drag=true" @end="drag=false" :options="{handle: '.dnd'}" v-if="content">
            <div
              v-for="(elem, elemIndex) in content"
              :key="elemIndex"
              :class="{ 'selectable': elem.type === 'text', 'unit': elem.type === 'unit' }"
            >
              <div
                contenteditable
                spellcheck="true"
                class="selectable-content"
                @keydown.enter="addNewElement($event, elemIndex, elem.type)"
                placeholder="Enter text"
                :id="elem.local_id"
                v-if="elem.type === 'text'"
              >{{elem.content}}</div>

              <div
                v-if="elem.type === 'unit'"
                class="unit_container"
              >
                <div
                  contenteditable="true"
                  spellcheck="true"
                  placeholder="Enter name"
                  class="unit_content"
                  :id="elem.local_id"
                  @keydown.enter="addNewElement($event, elemIndex, elem.type)"
                >{{elem.title}}</div>
              </div>

              <div style="opacity: 1;">
                <div class="dnd" style="position: absolute; top: 0px; left: -20px; width: 18px; height: 24px; pointer-events: auto; cursor: -webkit-grab;">
                  <div class="notion-selectable">
                    <div style="cursor: grab; user-select: none; transition: background 120ms ease-in 0s; display: flex; align-items: center; justify-content: center; width: 18px; height: 24px; border-radius: 3px; fill: rgba(55, 53, 47, 0.3);">
                      <svg viewBox="0 0 10 10" style="width: 14px; height: 14px; display: block; fill: inherit; flex-shrink: 0; backface-visibility: hidden;">
                        <path d="M3,2 C2.44771525,2 2,1.55228475 2,1 C2,0.44771525 2.44771525,0 3,0 C3.55228475,0 4,0.44771525 4,1 C4,1.55228475 3.55228475,2 3,2 Z M3,6 C2.44771525,6 2,5.55228475 2,5 C2,4.44771525 2.44771525,4 3,4 C3.55228475,4 4,4.44771525 4,5 C4,5.55228475 3.55228475,6 3,6 Z M3,10 C2.44771525,10 2,9.55228475 2,9 C2,8.44771525 2.44771525,8 3,8 C3.55228475,8 4,8.44771525 4,9 C4,9.55228475 3.55228475,10 3,10 Z M7,2 C6.44771525,2 6,1.55228475 6,1 C6,0.44771525 6.44771525,0 7,0 C7.55228475,0 8,0.44771525 8,1 C8,1.55228475 7.55228475,2 7,2 Z M7,6 C6.44771525,6 6,5.55228475 6,5 C6,4.44771525 6.44771525,4 7,4 C7.55228475,4 8,4.44771525 8,5 C8,5.55228475 7.55228475,6 7,6 Z M7,10 C6.44771525,10 6,9.55228475 6,9 C6,8.44771525 6.44771525,8 7,8 C7.55228475,8 8,8.44771525 8,9 C8,9.55228475 7.55228475,10 7,10 Z"></path>
                      </svg>
                    </div>
                  </div>
                </div>
                <div @click="addNewElement($event, elemIndex, elem.type)" style="cursor: -webkit-grab; user-select: none; transition: background 120ms ease-in 0s; display: flex; align-items: center; justify-content: center; fill: rgba(55, 53, 47, 0.3); position: absolute; top: 0px; left: -44px; width: 24px; height: 24px; border-radius: 3px; pointer-events: auto;">
                  <svg viewBox="0 0 18 18" style="width: 16px; height: 100%; display: block; fill: inherit; flex-shrink: 0; backface-visibility: hidden;">
                    <polygon points="17,8 10,8 10,1 8,1 8,8 1,8 1,10 8,10 8,17 10,17 10,10 17,10 "></polygon>
                  </svg>
                </div>
              </div>
            </div>
          </draggable>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
import { data } from '../data.json'

export default {
  name: 'Editor',
  components: {
    draggable,
  },
  data() {
    return {
      activeElement: null,
      content: null
    }
  },
  methods: {
    getMaxLocalId() {
      const localIds = this.content.map((elem) => elem.local_id)
      return Math.max(...localIds)+1
    },
    addNewElement(e, index, type) {
      e.preventDefault()
      const id = this.getMaxLocalId()
      this.content.insert(index+1, {
          "local_id": id,
          "type": type,
          "content": ""
        })
      setTimeout(() => {
        document.getElementById(id).focus()
      }, 10)
    },
    initData() {
      const isInit = localStorage.getItem('isInit')
      const actualData = localStorage.getItem('data')
      if (!isInit && actualData) {
        this.content = JSON.parse(actualData)
      } else {
        this.content = data
        localStorage.setItem('isInit', true)
        localStorage.setItem('data', JSON.stringify(data))
      }
    }
  },
  created() {
    this.initData()
  }
};
</script>

<style>
body {
  padding: 0;
  margin: 0;
  font-family: sans-serif;
  font-size: 1.1em;
}

.editor {
  width: 100vw;
  height: 100%;
  position: relative;
  display: flex;
  flex: 1 1 0%;
  background: white;
  cursor: text;
}
.container {
  flex-grow: 1;
  flex-shrink: 1;
  display: flex;
  flex-direction: column;
  background: white;
  z-index: 1;
  width: 100vw;
  height: 100vh;
  max-height: 100%;
}
.row {
  display: flex;
  flex-direction: column;
  z-index: 1;
  flex-grow: 1;
  position: relative;
  align-items: center;
  overflow: auto;
}
.page-content {
  flex-shrink: 0;
  flex-grow: 1;
  width: 900px;
  max-width: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
  font-size: 16px;
  color: rgb(55, 53, 47);
  padding: 50px 96px 30vh;
}
[contenteditable]:empty:before {
  content: attr(placeholder);
  display: block;
  color: rgba(55, 53, 47, 0.4);
}

.unit {
  width: 100%;
  margin: 10px 0;
  position: relative;
}

.unit_container {
  font-size: 1.2em;
  padding: 3px 2px;
  color: rgb(55, 53, 47);
  display: flex;
  justify-content: space-between;
  width: 100%;
  outline: none;
}
.unit_content {
  max-width: 100%;
  width: 100%;
  white-space: pre-wrap;
  word-break: break-word;
  caret-color: rgb(55, 53, 47);
  border-left: 3px solid currentcolor;
  padding-left: 0.9em;
  padding-right: 0.9em;
  -webkit-user-modify: read-write-plaintext-only;
  outline: none;
}
.unit_actions p {
  margin: 0;
}

.selectable {
  width: 100%;
  margin-top: 1px;
  margin-bottom: 1px;
  position: relative;
  color: #37352f;
  display: flex;
  margin: 10px 0;
}
.selectable-content {
  max-width: 100%;
  width: 100%;
  white-space: pre-wrap;
  word-break: break-word;
  caret-color: rgb(55, 53, 47);
  padding: 3px 2px;
  min-height: 1em;
  color: rgb(55, 53, 47);
  -webkit-user-modify: read-write-plaintext-only;
  outline: none;
}
</style>
