<template>
  <div>
    <div contenteditable="true" class="edit-code" @keydown="keydown">
      <div class="min-line"></div>
    </div>
    <!-- <div class="cursor" :style="cursorStyle"></div> -->
  </div>
</template>

<script>
export default {
  name: 'Code',
  data () {
    return {
      origin: {
        X: 0,
        Y: 0
      },
      cursorPoint: {
        X: 203.5,
        Y: 60
      },
      content: '',
      currentWord: '',
      wordStart: 0,
      wordEnd: 0
    }
  },
  computed: {
    cursorStyle () {
      return {
        left: this.cursorPoint.X + 'px',
        top: this.cursorPoint.Y + 'px'
      }
    }
  },
  watch: {
    content () {
      this.changeCursorPos()
    },
    currentWord () {

    }
  },
  mounted () {
    this.cursorPoint.X = this.origin.X = document.querySelector('.edit-code').offsetLeft + 10
    this.cursorPoint.Y = this.origin.Y = document.querySelector('.edit-code').offsetTop + 10
  },
  methods: {
    changeCursorPos () {
      // setTimeout(() => {
      var range = window.getSelection().getRangeAt(0)
      console.log(window.getSelection().focusOffset)
      this.cursorPoint.X = range.getBoundingClientRect().x === 0 ? 203.5 : range.getBoundingClientRect().x
      this.cursorPoint.Y = range.getBoundingClientRect().y === 0 ? 60 : range.getBoundingClientRect().y
      console.log(range.getBoundingClientRect())
      // }, 100)
    },
    keydown (key) {
      var range = window.getSelection().getRangeAt(0)
      var node = document.createElement('span')
      node.style.color = '#c678dd'
      // // 选中区域文本
      // node.innerHTML = this.currentWord
      // // 删除选中区
      // range.deleteContents()
      // // 在光标处插入新节点
      range.insertNode(node)
      this.$nextTick(() => {
        node.focus()
      })
      // importa
      // console.log('key', key)
      setTimeout(() => {
        this.currentWord = this.checkCursorPrevString()
        console.log(this.currentWord)

        // if (this.currentWord !== 'import') return

        // 获取文档中选中区域
        // var range = window.getSelection().getRangeAt(0)
        // var node = document.createElement('span')
        // node.style.color = '#c678dd'
        // // // 选中区域文本
        // node.innerHTML = this.currentWord
        // // // 删除选中区
        // // range.deleteContents()
        // // // 在光标处插入新节点
        // range.insertNode(node)
      }, 0)
      // switch (key.code) {

      // }
      // if (key.code === 'Enter') {
      //   this.cursorPoint.Y += 18
      //   this.cursorPoint.X = this.origin.X
      // } else if (key.code === 'Tab') {
      //   this.range.insertNode('  ')
      // } else {
      //   setTimeout(() => {
      //     this.changeCursorPos()
      //   }, 0)
      // }
    },
    click () {
      this.changeCursorPos()
    },
    /** 检查光标所在的有效字符串 */
    checkCursorPrevString () {
      var range = window.getSelection().getRangeAt(0)
      let str = range.startContainer.data // 获取整行的字符串

      if (!str) return ''

      /** 获取光标的位置 range.endOffset
       * 根据当前位置分别从两边去匹配符合条件的字符
       * 如果碰到不符合条件的字符则停下
       * 直到两端都有匹配不了的字符则返回
       * 从原始字符串中截取出光标所在的字符串
       * 【注意】前面i 在截取时候需要+1  str.substr(i + 1, j)
       */

      let i = range.endOffset - 1
      let j = range.endOffset

      while (true) {
        if (/^[a-z0-9]$/i.test(str[i])) i--

        if (/^[a-z0-9]$/i.test(str[j])) j++

        if (!/^[a-z0-9]$/i.test(str[i]) && !/^[a-z0-9]$/i.test(str[j])) break
      }

      // 当前字符串的起始和结束位置
      this.wordStart = i + 1
      this.wordEnd = j

      return str.substr(i + 1, j)
    }
  }
}
</script>

<style scoped>
  .idendifier {
    color: #e06c75;
  }
  .keyword {
    color: #c678dd;
  }
  .edit-code {
    padding: 10px;
    cursor: text;
    width: 700px;
    height: 400px;
    margin: 50px auto;
    outline: none;
    color: rgb(187, 187, 187);
    caret-color: #4ba2ff;
    /* color: transparent;
    text-shadow: 0 0 0 rgb(187, 187, 187); */
    background-color: rgb(40, 44, 52);
    /* font-family: Consolas, "Courier New", monospace; */
    font-family: Menlo, Monaco, "Courier New", monospace;
    font-size: 18px;
    line-height: 27px;
    white-space: pre;
  }

  .cursor {
    width: 2px;
    height: 22px;
    background-color: #386bf7;
    position: absolute;
  }

  .min-line {
    height: 18px;
  }
</style>
