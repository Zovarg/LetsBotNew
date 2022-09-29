<template>
  <div class="designer__modal-body">
    <symbol fill="none" viewBox="0 0 22 13" id="salesbot--code-icon" xmlns="http://www.w3.org/2000/svg"><mask id="qva" maskUnits="userSpaceOnUse" x="-.129" y="-.039" width="22" height="13" fill="#000"><path fill="#fff" d="M-.129-.039h22v13h-22z"></path><path fill-rule="evenodd" clip-rule="evenodd" d="M12.363.96L7.797 11.647l.922.393 4.566-10.685-.922-.393zM1.617 5.583l5.024-2.914.5.865-4.278 2.482 4.278 2.481-.5.865-5.024-2.914-.746-.432.746-.433zm18.766 0l-5.024-2.914-.5.865 4.278 2.482-4.278 2.481.5.865 5.024-2.914.746-.432-.746-.433z"></path></mask><path fill-rule="evenodd" clip-rule="evenodd" d="M12.363.96L7.797 11.647l.922.393 4.566-10.685-.922-.393zM1.617 5.583l5.024-2.914.5.865-4.278 2.482 4.278 2.481-.5.865-5.024-2.914-.746-.432.746-.433zm18.766 0l-5.024-2.914-.5.865 4.278 2.482-4.278 2.481.5.865 5.024-2.914.746-.432-.746-.433z" fill="#BCBCBC"></path><path d="M7.797 11.646l-.23-.098-.098.23.23.098.098-.23zM12.363.961l.098-.23-.23-.098-.098.23.23.098zM8.72 12.039l-.098.23.23.098.098-.23-.23-.098zm4.566-10.685l.23.098.098-.23-.23-.098-.098.23zM6.641 2.668l.216-.125-.125-.217-.217.126.126.216zM1.617 5.582l.126.217-.126-.217zm5.524-2.049l.125.216.216-.125-.125-.216-.216.125zM2.863 6.015l-.125-.217-.373.217.373.216.125-.216zm4.278 2.481l.216.125.125-.216-.216-.125-.125.216zm-.5.865l-.126.217.217.125.125-.217-.216-.125zM1.617 6.447l.126-.216-.126.216zm-.746-.432l-.125-.217-.373.217.373.216.125-.216zM15.36 2.668l.126-.216-.217-.126-.125.217.216.125zm5.024 2.914l-.126.217.126-.217zm-5.524-2.049l-.216-.125-.125.216.216.125.125-.216zm4.278 2.482l.125.216.373-.216-.373-.217-.125.217zm-4.278 2.481l-.125-.216-.216.125.125.216.216-.125zm.5.865l-.216.125.125.217.217-.125-.126-.217zm5.024-2.914l-.126-.216.126.216zm.746-.432l.125.216.373-.216-.373-.217-.125.217zm-13.102 5.73l4.566-10.686-.46-.196-4.566 10.685.46.197zm.79.064l-.922-.393-.196.46.922.393.196-.46zm4.238-10.553L8.49 11.94l.46.197 4.566-10.686-.46-.196zm-.79-.065l.922.393.196-.46-.922-.393-.196.46zm-5.75 1.261L1.492 5.366l.25.433 5.024-2.915-.25-.432zm.842.956l-.5-.865-.433.25.5.865.433-.25zM2.989 6.23l4.277-2.482-.25-.432-4.278 2.481.25.433zm4.277 2.05L2.99 5.797l-.251.433 4.277 2.482.251-.433zm-.409 1.205l.5-.865-.433-.25-.5.865.433.25zM1.492 6.664l5.023 2.914.251-.433-5.023-2.914-.251.433zM.746 6.23l.746.433.25-.433-.746-.433-.25.433zm.746-.865l-.746.432.25.433.747-.432-.251-.433zm13.742-2.482L20.257 5.8l.251-.433-5.023-2.914-.251.432zm-.158.774l.5-.865-.433-.25-.5.865.433.25zm4.186 2.14l-4.277-2.481-.251.432 4.277 2.482.251-.433zm-4.277 2.915l4.277-2.482-.25-.433-4.278 2.482.25.433zm.59.523l-.5-.865-.432.25.5.865.433-.25zm4.682-3.005l-5.023 2.914.25.433 5.024-2.914-.25-.433zm.746-.433l-.746.433.251.433.746-.433-.25-.433zm-.745 0l.746.433.25-.433-.746-.432-.25.433z" fill="#BCBCBC" mask="url(#qva)"></path></symbol>
    <div class="designer__body" id="scrollArea">
      <div class="designer__workarea" id="workArea">
        <svg class="designer__connections" height="500" width="500" id="paths"></svg>
        <slot v-for="(bot, index) in bots">
          <type-bot :id="bot.id" :type.sync="bot.type" :key="index" :position.sync="bot.position" :data.sync="bot.data"></type-bot>
        </slot>
      </div>
    </div>
    <div class="designer__zoom">
      <button class="designer__button designer__zoom-btn designer__zoom-orig" @click="zoomReset"><span></span></button>
      <button class="designer__button designer__zoom-btn designer__zoom-in" @click="zoomIn"><span></span></button>
      <button class="designer__button designer__zoom-btn designer__zoom-out" @click="zoomOut"><span></span></button>
      <button class="designer__button designer__button_code"><span><svg class="svg-icon svg-salesbot--code-icon-dims"><use xlink:href="#salesbot--code-icon"></use></svg>{{ $t('srccomponentssalesbot.7064') }}</span></button>
    </div>
  </div>
</template>

<script>
import TypeBot from "@/typeBot";
import Panzoom from '@panzoom/panzoom'
export default {
  name: 'HomeView',
  components: {TypeBot},
  data () {
    return {
      element: null,
      panzoom: null,
      paths: null,
      boxs: null,
      padding: 20,
      bots: [
        {
          id: 1,
          position: {x: 70, y: 150, width: 'auto'},
          type: 'start'
        },
        {
          id: 2,
          parent: 1,
          position: {x: 270, y: 150, width: '270px'},
          type: 'next'
        }
      ],
      connections: [{
        boxA: '#box1',
        boxB: '#box2'
      }]
    }
  },

  computed: {
    validStages () {
      var examples = []
      this.bots.forEach((val) => {
        examples.push({
          id: val.id,
          parent: val.parent,
          text: 'Этап ' + val.id
        })
      })
      return examples
    }
  },
  mounted () {
    setTimeout(() => {
      this.elem = document.getElementById('workArea')
      this.panzoom = Panzoom(this.elem, {
        excludeClass: 'box'
      })
      //this.panzoom.pan(10, 10)

      //this.elem.parentElement.addEventListener('wheel', this.panzoom.zoomWithWheel)
      this.boxs = document.querySelectorAll('.box')
      this.paths = document.getElementById('paths')
      var self = this
      jQuery('.box').draggable({
        drag: () => {
          self.coordinates()
        }
      })
      this.coordinates()
    }, 500)
  },
  methods: {
    zoomIn () {
      this.panzoom.zoomIn()
      setTimeout(() => {
        this.coordinates()
      }, 300)
    },
    zoomOut () {
      this.panzoom.zoomOut()
      setTimeout(() => {
        this.coordinates()
      }, 300)
    },
    zoomReset () {
      var t = this.elem.getBoundingClientRect()
      console.table({
        clientX: t.x + t.width / 2,
        clientY: t.y + t.height / 2
      })
      this.panzoom.reset({
        focal: {
          clientX: t.x + t.width / 4,
          clientY: t.y + t.height / 4
        }
      })

      setTimeout(() => {
        this.coordinates()
      }, 300)
    },
    coordinates () {
      let oldPaths = this.paths.children
      for (let a = oldPaths.length - 1; a >= 0; a--) {
        this.paths.removeChild(oldPaths[a])
      }
      let path, boxA, boxB
      var heigth = this.paths.height.animVal.value
      var width = this.paths.width.animVal.value
      for (let a = 0; a < this.connections.length; a++) {
        boxA = $(this.connections[a].boxA)
        boxB = $(this.connections[a].boxB)
        /*
        var Ax = boxA.offset().left
        var Ay = boxA.offset().top
        var Bx = boxB.offset().left
        var By = boxB.offset().top
        */
        if (boxA.hasClass('circle')) {
          var parent = boxA.parents('.box')
          var current = boxA.parents('.btn_grp').position()
          var Ax = parseFloat(parent.css('left')) + parent.width()
          var Ay = parseFloat(parent.css('top'))
          Ay += current.top
        } else {
          var Ax = parseFloat(boxA.css('left')) + boxA.width()
          var Ay = parseFloat(boxA.css('top'))
        }
        var Bx = parseFloat(boxB.css('left'))
        var By = parseFloat(boxB.css('top'))

        var YA = Ay + boxA.height()
        var YB = By + boxB.height()
        if (heigth < YA) heigth = YA + boxA.height()
        if (heigth < YB) heigth = YB + boxB.height()
        var XA = Ax + boxA.width()
        var XB = Bx + boxB.width()
        if (width < XA) width = XA + boxA.width()
        if (width < XB) width = XB + boxB.width()

        /*console.table({
          X1: Ax,
          Y1: Ay,
          X2: Bx,
          Y2: By,
          pad: this.padding,
          a: a
        })*/
        var getStartY = Ay + this.padding // * (a + 1)
        var getEndY = (By || 0) + this.padding
        var e
        var t = Math.floor(Ax + this.padding)
        var n = Math.floor(getStartY)
        var o = Math.floor(getEndY)
        var i = Math.floor(Bx)
        var s = []
        var aa = [1, 2]
        var r = aa.length + 1 - 0
        var l = 10 * r
        s.push('M' + t + ' ' + n)
        if (t < i && i - t > 10) {
          if (Math.abs(n - o) > 3) {
            e = Math.floor(t + (i - t) / (aa.length + 2) * r)
            s.push('L' + e + ' ' + n)
            s.push('L' + e + ' ' + o)
          }
        } else {
          s.push('L' + (t + l) + ' ' + n)
          if (n < o) {
            e = Ay
            e += boxA.height()
            e += l
          } else {
            e = Ay - l
          }
          s.push('L' + (t + l) + ' ' + e)
          s.push('L' + (i - l) + ' ' + e)
          s.push('L' + (i - l) + ' ' + o)
        }
        s.push('L' + i + ' ' + o)
        var getArrowHead = ['', 'L' + (i - 4) + ' ' + (o - 4), 'M' + (i - 4) + ' ' + (o + 4), 'L' + i + ' ' + o].join(' ')

        path = document.createElementNS('http://www.w3.org/2000/svg', 'path')
        path.setAttribute('d', this.roundLines(s.join(' '), 3) + getArrowHead)
        //path.setAttribute('d', s.join(' ') + getArrowHead)
        path.setAttribute('class', 'path')
        this.paths.appendChild(path)
      }
      this.paths.setAttribute('width', width)
      this.paths.setAttribute('height', heigth)
      //this.paths.setAttribute('viewBox', [0, 0, width, heigth].join(' '))
    },
    /* eslint-disable */
    roundLines (e, t, n) {
      function i(e, t, n) {
        var i = t.x - e.x, s = t.y - e.y, a = Math.sqrt(i * i + s * s);
        return o(e, t, Math.min(1, n / a))
      }

      function o(e, t, n) {
        return {x: e.x + (t.x - e.x) * n, y: e.y + (t.y - e.y) * n}
      }

      function s(e, t) {
        2 < e.length && (e[e.length - 2] = t.x, e[e.length - 1] = t.y)
      }

      function a(e) {
        return {x: parseFloat(e[e.length - 2]), y: parseFloat(e[e.length - 1])}
      }

      var r = e.split(/[,\s]/).reduce((function (e, t) {
        var n = t.match("([a-zA-Z])(.+)");
        return n ? (e.push(n[1]), e.push(n[2])) : e.push(t), e
      }), []).reduce((function (e, t) {
        return parseFloat(t) == t && e.length ? e[e.length - 1].push(t) : e.push([t]), e
      }), []), l = [];
      if (1 < r.length) {
        var c = a(r[0]), d = null;
        "Z" == r[r.length - 1][0] && 2 < r[0].length && (d = ["L", c.x, c.y], r[r.length - 1] = d), l.push(r[0]);
        for (var u = 1; u < r.length; u++) {
          var h = l[l.length - 1], p = r[u], g = p == d ? r[1] : r[u + 1];
          if (g && h && 2 < h.length && "L" == p[0] && 2 < g.length && "L" == g[0]) {
            var m, _, f = a(h), b = a(p), v = a(g);
            _ = n ? (m = o(b, h.origPoint || f, t), o(b, g.origPoint || v, t)) : (m = i(b, f, t), i(b, v, t)), s(p, m), p.origPoint = b, l.push(p);
            var y = o(m, b, .5), x = o(b, _, .5), w = ["C", y.x, y.y, x.x, x.y, _.x, _.y];
            w.origPoint = b, l.push(w)
          } else l.push(p)
        }
        if (d) {
          var k = a(l[l.length - 1]);
          l.push(["Z"]), s(l[0], k)
        }
      } else l = r;
      return l.reduce((function (e, t) {
        return e + t.join(" ") + " "
      }), "")
    }
    /* eslint-enable */
  },
  watch: {
    bots: {
      handler: function (after, before) {
        setTimeout(() => {
          var self = this
          jQuery('.box').draggable({
            drag: () => {
              self.coordinates()
            }
          })
          this.coordinates()
        }, 500)
      },
      deep: true
    }
  }
}
</script>

<style>
a, abbr, acronym, address, applet, article, aside, audio, b, big, blockquote, button, canvas, caption, center, cite, code, dd, del, details, dfn, div, dl, dt, em, embed, fieldset, figcaption, figure, footer, form, h1, h2, h3, h4, h5, h6, header, hgroup, i, iframe, img, input, ins, kbd, label, legend, li, mark, menu, nav, object, ol, output, p, pre, q, ruby, s, samp, section, small, span, strike, strong, sub, summary, sup, table, tbody, td, textarea, tfoot, th, thead, time, tr, tt, u, ul, var, video {
  margin: 0;
  padding: 0;
  border: 0;
  font: inherit;
  vertical-align: baseline;
  text-rendering: geometricPrecision;
  -webkit-font-smoothing: antialiased;
}
.path {
  fill: none;
  stroke: #8F9A9D;
  stroke-width: 1;
}
.designer__connections {
  /* left: 0px;
   top: 0px;
   position: absolute;
   z-index: 0;*/
}
.designer__block {
  display: flex;
  flex-direction: column;
  position: absolute !important;
//border: 1px solid #e5e5e5;
//border-radius: 3px;
//padding: 7px 9px 0;
  z-index: 1;
  cursor: default;
  user-select: none;

//background-color: #f5f5f5;
//width: 200px;
//height: 300px;
}
.designer__workarea {
  position: absolute;
  top: 0;
  left: 0;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  font-family: 'PT Sans', Arial, sans-serif;
  line-height: normal;
}
.designer__modal-body * {
  box-sizing: border-box;
}
.designer__modal-body {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  width: 100%;
  height: calc(100vh - 125px);
  position: relative;
}
.designer__body {
  /*
  flex-grow: 1;
  position: relative;
  overflow: hidden;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;*/
  position: relative;
  width: calc(100vw - 240px);
  height: calc(100vh - 125px);
  overflow: hidden;
}
.designer__zoom {
  position: absolute;
  display: -webkit-box;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  flex-direction: column;
  right: 25px;
  top: 50%;
  margin-top: -33px;
  z-index: 2;
}
.designer__zoom {
  position: absolute;
  flex-direction: row;
  top: 58px;
  right: 25px;
}
.designer__button {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  min-width: 31px;
  height: 31px;
  font-size: 13px;
  color: #8f9a9d;
  background: #fff;
  box-shadow: 0 2px 2px rgba(0,0,0,.07);
  border-radius: 36px;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.designer__zoom .designer__button:not(:first-child) {
  margin-left: 5px;
}
.designer__zoom-orig span {
  position: relative;
  width: calc(100% - 18px);
  height: 13px;
  border: 1px solid #bcbcbc;
}
.designer__zoom-orig span::before {
  width: calc(100% + 2px);
  height: 5px;
  left: -1px;
  top: 3px;
}
.designer__zoom-orig span::before, .designer__zoom-orig span::after {
  content: '';
  position: absolute;
  background: #fff;
}
.designer__zoom-orig span::after {
  height: 13px;
  width: 5px;
  top: -1px;
  left: 3px;
}
.designer__zoom-in span, .designer__zoom-out span {
  position: relative;
  background: #bcbcbc;
  height: 1px;
  width: calc(100% - 18px);
}
.designer__zoom-in span::before {
  content: '';
  position: absolute;
  height: 11px;
  width: 1px;
  background: #bcbcbc;
  top: -5px;
}
.designer__zoom .designer__button_code {
  margin-left: 10px !important;
}
.designer__button_code {
  padding: 0 10px;
  z-index: 2;
}
.designer__button_code span, .designer__button_export {
  display: flex;
  align-items: center;
}
.designer__button_code span svg {
  margin-right: 5px;
}
.svg-salesbot--code-icon-dims {
  width: 22px;
  height: 13px;
}
.box .right_menu {
  position: absolute;
  right: 3px;
  top: 1px;
  background: transparent;
}
.box .right_menu button {
  background: transparent;
  line-height: 0px;
  border: none !important;
  outline: none !important;
}
.box.message-input {
  background: #2d7ffb !important;
  border-color: #2d7ffb;
  color: white !important;
}
.box textarea {
  border-radius: 16px;
  padding-top: 8px;
  background: transparent !important;
  border-color: transparent;
  color: white !important;
  resize: none;
}
.box textarea::placeholder {
  color: darkgrey;
}
.box .new_button {
  background: none;
  border: 1px dashed #91baf8;
  border-radius: 12px;
  height: 25px;
  color: #bbd6ff;
  min-width: 110px;
  padding: 0 13px 0 11px;
  cursor: pointer;
}
.box .input_button {
  display: -webkit-box;
  display: flex;
  -webkit-box-align: baseline;
  align-items: baseline;
  position: relative;
  background: #418bfa;
  border: 1px solid #a0c6ff;
  border-radius: 13px;
  overflow: hidden;
  height: 25px;
  width: 195px;
}
.box .trash {
  line-height: 23px;
  color: #ffffff47 !important;
  cursor: pointer;
  margin-left: 5px;
}
.box .circle {
  position: absolute;
  width: 13px;
  height: 13px;
  border-radius: 50%;
  border: 1px solid #8f9a9d;
  background: #fff;
  right: 0px;
  top: 5px;
}
.btn_grp {
  position: relative;
}
</style>
