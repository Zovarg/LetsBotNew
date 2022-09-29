<template>
  <div :class="'box designer__block ' + getAlertStyle()" :id="'box' + id"
       @mouseleave="changePosition" @mousemove="changePosition" :style="{left: position.x+'px', top: position.y+'px', width: position.width}">
    <slot v-if="type !== 'start' && type !== 'next'">
      <div class="btn-group right_menu">
        <button type="button" data-toggle="dropdown">
          ...
        </button>
        <div class="dropdown-menu">
          <a class="dropdown-item" @click="setType('next')" role="button"><i class="fas fa-solid fa-edit" style="display: inline-block"></i> Сменить действие</a>
          <a class="dropdown-item" role="button"><i class="fa fa-solid fa-trash text-danger" style="display: inline-block"></i> Удалить</a>
        </div>
      </div>
    </slot>
    <slot v-if="type ==='start'"><div><i class="fa fa-solid fa-play" style="display: inline-block"></i> Запуск бота</div></slot>
    <slot v-else-if="type === 'next'">
      Следующий шаг
      <button @click="addNew('message')" type="button" class="btn btn-light btn-block mt-3 text-left">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
          <rect width="18" height="18" rx="9" fill="url(#pattern151)"></rect>

          <defs>
            <pattern id="pattern151" patternContentUnits="objectBoundingBox" width="1" height="1">
              <use xlink:href="#image399" transform="translate(-0.097561 -0.365854) scale(0.0243902)"></use>
            </pattern>

            <image id="image399" width="48" height="68" xlink:href="data:image/png;base64,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"></image>
          </defs>
        </svg>
        Отправить сообщение</button>
      <button @click="addNew('pause')" type="button" class="btn btn-light btn-block text-left">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle cx="9" cy="9" r="9" fill="#DF89C2"></circle>
          <path fill-rule="evenodd" clip-rule="evenodd" d="M5 5C4.44772 5 4 5.44772 4 6V11C4 11.5523 4.44772 12 5 12H11L12.3172 13.3172C12.5691 13.5691 13 13.3907 13 13.0343V12C13.5523 12 14 11.5523 14 11V6C14 5.44772 13.5523 5 13 5H5ZM5 7H13V8H5V7ZM10 9H5V10H10V9Z" fill="white"></path>
        </svg>
        Прерыватель</button>
      <button @click="addNew('action')" type="button" class="btn btn-light btn-block text-left">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle cx="9" cy="9" r="9" fill="#39E2A5"></circle>
          <path d="M5 11.2995V13H6.72268L11.6297 8.10467L9.90701 6.40419L5 11.2995ZM12.8825 6.7649C13.0392 6.61031 13.0392 6.30113 12.8825 6.14654L11.8385 5.11594C11.6819 4.96135 11.3687 4.96135 11.2121 5.11594L10.429 5.88889L12.1517 7.58937L12.8825 6.7649Z" fill="white"></path>
        </svg>
        Выполнить действие</button>
      <button @click="addNew('if')" type="button" class="btn btn-light btn-block text-left">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle cx="9" cy="9" r="9" fill="#9B51E0"></circle>
          <path d="M8.1671 15C8.15044 15 8.11713 15 8.10047 14.9813C8.03384 14.9627 8.00052 14.8881 8.00052 14.8134V10.2799L5.10203 5.89552C4.98542 5.70896 4.96877 5.52239 5.05206 5.31716C5.13535 5.11194 5.31858 5 5.50182 5H12.4982C12.6981 5 12.8647 5.1306 12.9479 5.31716C13.0312 5.50373 13.0146 5.72761 12.898 5.89552L9.99948 10.2799V13.3209C9.99948 13.3769 9.96616 13.4328 9.93285 13.4701L8.26705 14.9627C8.23373 14.9813 8.20042 15 8.1671 15Z" fill="white"></path>
        </svg>
        Условие</button>
      <button @click="addNew('validate')" type="button" class="btn btn-light btn-block text-left">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle cx="9" cy="9" r="9" fill="#27AE60"></circle>
          <path fill-rule="evenodd" clip-rule="evenodd" d="M13.7072 6.70706L7.00008 13.4142L3.29297 9.70706L4.70718 8.29285L7.00008 10.5857L12.293 5.29285L13.7072 6.70706Z" fill="white"></path>
        </svg>
        Валидатор</button>

      <div v-if="data && data.selectStage" class="btn-group btn-block" role="group">
        <button id="btnGroupDrop1" type="button" class="btn btn-light btn-block text-left dropdown-toggle" data-toggle="dropdown">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="9" cy="9" r="9" fill="#56CCF2"></circle>
            <path d="M9.73473 8.86532H7.61273V8.40574C7.61273 8.08239 7.22898 7.88946 6.94406 8.06842L4.19133 9.79826C3.93658 9.95815 3.93586 10.3123 4.19133 10.4729L6.94406 12.2024C7.22952 12.3819 7.61273 12.1879 7.61273 11.8651V11.4055H9.30455V13.5946C9.30455 13.8183 9.49714 14 9.73473 14H11.5698C11.8072 14 12 13.8185 12 13.5946V11.0002C12 9.82142 10.9797 8.86532 9.73473 8.86532Z" fill="white"></path>
            <path d="M12.9198 5.74422L10.8949 3.17834C10.7073 2.9404 10.2929 2.94074 10.1051 3.17834L9.48772 3.96106L8.08034 5.74422C7.87023 6.01045 8.09729 6.36746 8.47525 6.36746H9.01328V7.8608H9.48792C10.5313 7.8608 11.4513 8.3143 11.9873 9.00001V6.36746H12.5253C12.9037 6.36746 13.1296 6.00978 12.9198 5.74422Z" fill="white"></path>
          </svg>
          Перейти на другой шаг
        </button>
        <div class="dropdown-menu">
          <a class="dropdown-item" v-for="stage in checkValid" role="button" @click="setParent(stage.id)">{{stage.text}}</a>
        </div>
      </div>
      <button @click="addNew('bot')" type="button" class="btn btn-light btn-block text-left">
        <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="16" height="16" viewBox="0 0 16 16">
          <path fill="#24bc8c" d="M8 0c-4.4 0-8 3.6-8 8s3.6 8 8 8 8-3.6 8-8-3.6-8-8-8zM6 12v-8l6 4-6 4z"></path>
        </svg>
        Запустить бота</button>
      <button @click="addNew('end')" v-if="id !== 2" type="button" class="btn btn-dark btn-block text-left">
        <svg width="9" height="11" viewBox="0 0 9 11" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M2.584 1.59874V9.40126C2.584 10.2842 2.00555 11 1.292 11C0.578448 11 0 10.2842 0 9.40126V1.59874C0 0.715779 0.578448 0 1.292 0C2.00555 0 2.584 0.715779 2.584 1.59874Z" fill="#F26E6E"></path>
          <path d="M8.084 1.59874V9.40126C8.084 10.2842 7.50556 11 6.792 11C6.07845 11 5.5 10.2842 5.5 9.40126V1.59874C5.5 0.715778 6.07845 0 6.792 0C7.50556 0 8.084 0.715778 8.084 1.59874Z" fill="#F26E6E"></path>
        </svg>
        Остановка бота</button>
    </slot>
    <slot v-else-if="type === 'end'"><div><i class="fa fa-solid fa-pause" style="display: inline-block"></i> Остановка бота</div></slot>
    <slot v-else-if="type === 'message'">
      <textarea cols="30" rows="3" class="form-control" placeholder="введите сообщение"></textarea>
      <div v-if="data.buttons" :key="'btn'+key" :data-key="key" class="d-flex mt-2 btn_grp" v-for="(button, key) in data.buttons"><input type="text" class="input_button"><i class="trash fa fa-trash"></i><div class="circle" :id="'box'+id+'button'+key"></div></div>
      <button class="new_button mt-2" @click="addButton()">+ Добавить кнопку</button>
    </slot>
    <slot v-else-if="type === 'pause'"></slot>
    <slot v-else>Неизвестная команда</slot>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  name: 'typeBot',
  props: ['id'],
  data () {
    return {
      type: this.$attrs.type,
      position: this.$attrs.position,
      data: this.$attrs.data || {}
    }
  },
  computed: {
    checkValid () {
      var all = {}
      this.$parent.bots.forEach(val => {
        all[val.id] = val
      })
      var valid = this.$parent.validStages
      var newValid = []
      valid.forEach(v => {
        if (all[v.id].type !== 'start' && v.id !== this.id) {
          var temp = all[this.id].parent.toString().split('-')
          if (temp.length > 1) {
            if (temp[0] !== v.id.toString()) {
              console.table({
                temp: temp,
                v: v
              })
              newValid.push(v)
            }
          } else newValid.push(v)
        }
      })
      return newValid
    }
  },
  methods: {
    setParent (id) {
      var key = (this.$options._parentVnode.key)
      var data = this.$parent.bots[key]
      this.$parent.connections[data.data.connection].boxB = '#box' + id
      Vue.delete(this.$parent.bots, key)
    },
    addButton () {
      if (!this.data.buttons) this.data.buttons = []
      this.data.buttons.push('Кнопка')
      var key = this.data.buttons.length - 1
      this.$forceUpdate()
      setTimeout(() => {
        var newIds = this.$parent.bots.length + 1
        this.$parent.bots.push({
          id: newIds,
          parent: [this.id, key].join('-'),
          position: {x: (this.position.x + $('#box'+this.id).width() + 150), y: $('[data-key="'+key+'"]').offset().top, width: '270px'},
          type: 'next',
          data: {
            selectStage: true,
            connection: this.$parent.connections.length
          }
        })
        this.$parent.connections.push({
          boxA: '#box' + this.id + 'button' + key,
          boxB: '#box' + newIds
        })
      }, 400)

    },
    setType (action) {
      this.type = action
      this.position.width = '270px'
    },
    getAlertStyle () {
      if (this.type === 'start')  return 'alert alert-success'
      else if (this.type === 'end')  return 'alert alert-danger'
      else if (this.type === 'message') return 'alert message-input'
      else return 'alert alert-dark'
    },
    addNew (action) {
      this.type = action
      if (action !== 'end') {
        var newId = this.$parent.bots.length + 1
        this.$parent.bots.push({
          id: newId,
          parent: this.id,
          position: {x: (this.position.x + parseInt(this.position.width) + 150), y: this.position.y, width: '270px'},
          type: 'next',
          data: {
            selectStage: false,
            connection: this.$parent.connections.length
          }
        })
        this.position.width = 'auto'
        this.$parent.connections.push({
          boxA: '#box' + this.id,
          boxB: '#box' + newId
        })
      } else {
        this.position.width = 'auto'
      }
    },
    changePosition: function () {
      this.position.x = parseInt($('#box'+this.id).css('left'))
      this.position.y = parseInt($('#box'+this.id).css('top'))
    }
  }
}
</script>

<style scoped>

</style>
