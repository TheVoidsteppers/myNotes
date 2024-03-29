## ant-design 1.7.x bug 汇总

### tree-select 的下拉已选项不能自动聚焦

> tree-select 偶尔会出现，已选项的值没法滚动到视野中间。

> 通过排查源码，自动聚焦这个功能在 Select.jsx 的 watch '$data._open' 实现

> 其原理就是监听 _open 的值，然后调用 scrollIntoView 方法滚动到视野。scrollIntoView 是 npm 包 dom-scroll-into-view，产生 bug 的原因是这个包计算滚动高度时，有时偏小。

> 解决方法：找到 Select.jsx 对应的实例，模拟 $data._open 的方法，实现滚动

```html
<a-tree-select
  ref="treeSelect"
  v-model="value"
  show-search
  style="width: 100%"
  :dropdown-style="{ maxHeight: '300px', overflow: 'auto' }"
  allow-clear
  tree-default-expand-all
></a-tree-select>
```

```js
export default {
  data () {
    return {
      value: '',
      treeSelectDropdownRef: null,
      treeSelectDropdownData: {}
    }
  },
  watch: {
    'treeSelectDropdownData._open': {
      handler (newVal) {
        if (!newVal) return
        setTimeout(() => {
          // 这里要 $nextTick 是为了保证在组件滚动之后执行
          this.$nextTick(() => {
            const { domTreeNodes } = this.treeSelectDropdownRef.popup.getTree()
            const treeNode = domTreeNodes[this.value]
            if (!treeNode) return
            const domNode = treeNode.$el
            // 调用 dom 原生方法， block: 'center' 代表滚动到视野中间
            domNode.scrollIntoView({ block: 'center' })
          })
        })
      }
    }
  },
  mounted () {
    const treeSelectRef = this.$refs.treeSelect
    if(!treeSelectRef) return
    this.treeSelectDropdownRef = treeSelectRef.$children[0]
    this.treeSelectDropdownData = treeSelectRef.$children[0].data
  }
}
```

### table 集成vue-draggable-resizable实现可伸缩列

> 如果直接按官网的例子写，会编译报错：render: function render (h, h, props, children) { ...

> 其实把 ResizeableTitle 改成 resizeableTitle 小写就可以

> 由于例子还有其他奇奇怪怪的问题，所以我整理了自己的 demo

```js
/**
 * table 的可伸缩列功能抽离成公共方法
 *
 */

import Vue from 'vue'
import VueDraggableResizable from 'vue-draggable-resizable'
Vue.component('vue-draggable-resizable', VueDraggableResizable)

const JS_FIT_FIXED_COLUMN_WIDTH_CLASS = 'js-fit-table_fixed-column-width' // 如果有滚定列空白，在 table 添加此 class 可修复列宽度和固定列宽度不一致的bug
const TABLE_FIXED_LEFT_COLUMN_CLASS = 'ant-design-vue-table__fixed-left-column'
const TABLE_FIXED_RIGHT_COLUMN_CLASS = 'ant-design-vue-table__fixed-right-column'

const numberColWidth = function (col) {
  const { width } = col
  if (typeof width === 'string') {
    if (width.endsWith('px')) {
      col.width = width.slice(0, -2) - 0
      return
    }
  }
  if (typeof (width - 0) === 'number') {
    col.width = width - 0
  }
}

const headeRender = function (columns) {
  const draggingMap = {}
  let existFixedCol = false // 是由存在固定列
  /**
   * width 为 % 不支持拖拽
   * vue-draggable-resizable 的 x 要传入具体数字，此时获取不到table的宽度，无法计算百分比
   * todo 待解决
   */
  const noDragable = columns.some(col => {
    return typeof col.width === 'string' && col.width.endsWith('%')
  })
  if (noDragable) {
    return
  }
  columns.forEach(col => {
    const k = col.dataIndex || col.key
    numberColWidth(col)
    if ('fixed' in col) {
      if (!existFixedCol) existFixedCol = true
      if (col.fixed === true || col.fixed === 'left') {
        if (col.className && col.className.includes(TABLE_FIXED_LEFT_COLUMN_CLASS)) return
        col.className = col.className ? `${col.className} ${TABLE_FIXED_LEFT_COLUMN_CLASS}` : TABLE_FIXED_LEFT_COLUMN_CLASS
      } else if (col.fixed === 'right') {
        if (col.className && col.className.includes(TABLE_FIXED_RIGHT_COLUMN_CLASS)) return
        col.className = col.className ? `${col.className} ${TABLE_FIXED_RIGHT_COLUMN_CLASS}` : TABLE_FIXED_RIGHT_COLUMN_CLASS
      }
    }
    draggingMap[k] = col.width
  })
  const draggingState = Vue.observable(draggingMap)
  const resizeableTitle = (h, props, children) => {
    let thDom = null
    const { key, ...restProps } = props
    let col
    if (key === 'selection-column') {
      col = {}
    } else {
      col = columns.find((item) => {
        const k = item.dataIndex || item.key
        return k === key
      })
    }
    if (!col.width) {
      return (<th {...restProps}>{children}</th>)
    }
    if (existFixedCol) {
      fitFixColumnsWidth()
    }
    const onDrag = x => {
      draggingState[key] = 0
      col.width = Math.max(x, 40)
    }
    const onDragstop = () => {
      draggingState[key] = thDom.getBoundingClientRect().width
    }
    return (
      <th {...restProps} v-ant-ref={r => (thDom = r)} width={draggingState[key]} class="resize-table-th">
        {children}
        <vue-draggable-resizable
          key={col.dataIndex || col.key}
          class='table-draggable-handle'
          w={10}
          x={col.width || draggingState[key]}
          z={1}
          axis="x"
          draggable={true}
          resizable={false}
          onDragging={onDrag}
          onDragstop={onDragstop}
        ></vue-draggable-resizable>
      </th>
    )
  }

  return resizeableTitle
}

// js 手动调整固定列的宽度
const fitFixColumnsWidth = function () {
  const crudTable = document.getElementsByClassName(JS_FIT_FIXED_COLUMN_WIDTH_CLASS)
  for (let i = 0; i < crudTable.length; i++) {
    const ct = crudTable[i]
    // 左侧固定列
    const lf = ct.getElementsByClassName('ant-table-fixed-left')
    if (lf.length > 0) {
      const fixTable = lf[0].getElementsByClassName('ant-table-fixed')
      const lfColumn = lf[0].getElementsByClassName(TABLE_FIXED_LEFT_COLUMN_CLASS)
      const fixedLfCol = ct.getElementsByClassName(`ant-table-fixed-columns-in-body ${TABLE_FIXED_LEFT_COLUMN_CLASS}`)
      let totalWidth = 0
      for (let j = 0; j < lfColumn.length; j++) {
        lfColumn[j].style.width = fixedLfCol[j].offsetWidth + 'px'
        if (fixedLfCol[j].nodeName === 'TH') {
          totalWidth += fixedLfCol[j].offsetWidth
        }
      }
      if (fixTable.length > 0) {
        fixTable[0].style.width = totalWidth + 'px'
      }
    }
    // 右侧固定列
    const rg = ct.getElementsByClassName('ant-table-fixed-right')
    if (rg.length > 0) {
      const fixTable = rg[0].getElementsByClassName('ant-table-fixed')
      const rgColumn = rg[0].getElementsByClassName(TABLE_FIXED_RIGHT_COLUMN_CLASS)
      const fixedRgCol = ct.getElementsByClassName(`ant-table-fixed-columns-in-body ${TABLE_FIXED_RIGHT_COLUMN_CLASS}`)
      let totalWidth = 0
      for (let j = 0; j < rgColumn.length; j++) {
        rgColumn[j].style.width = fixedRgCol[j].offsetWidth + 'px'
        if (fixedRgCol[j].nodeName === 'TH') {
          totalWidth += fixedRgCol[j].offsetWidth
        }
      }
      if (fixTable.length > 0) {
        fixTable[0].style.width = totalWidth + 'px'
      }
    }
  }
}

export default headeRender
```

```js
export default {
  data () {
    return {
      components: {}
    }
  },
  watch: {
    columns: {
      handler (newVal, oldVal) {
        this.components = {
          header: {
            cell: resizeableTitle(newVal)
          }
        }
      },
      immediate: true
    }
  }
}
```
 