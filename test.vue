<template>
  <div class="demo">
    <div class="weightbox" v-show="weight.isShow">
      <span
        v-for="(item, index) in weight.data"
        :key="index"
        @click="chooseWeight(index, weight.index)"
      >
        <label
          >{{ item.name }}<input type="radio" name="a" @click="stopPropa()"
        /></label>
      </span>
    </div>
    <table>
      <tr>
        <td>姓名</td>
        <td>身高</td>
        <td>体重</td>
        <td>等级</td>
        <td style="width: 350px">操作</td>
      </tr>
      <tr v-for="(item, index) in items" :key="index">
        <td>
          <p v-show="!item.isClone">{{ item.name }}</p>
          <input type="text" v-model="item.name" v-show="item.isClone" />
        </td>
        <td>
          <p v-show="!item.isClone">{{ item.hight }}</p>
          <input type="text" v-model="item.hight" v-show="item.isClone" />
        </td>
        <td>
          <p v-show="!item.isClone">{{ item.weight.name }}</p>
          <input
            type="text"
            v-model="item.weight.name"
            v-show="item.isClone"
            @click="showWeight(item.weight.id, index)"
            readonly
            class="ds-select"
            style="cursor: pointer"
          />
        </td>
        <td>
          <p v-show="!item.isClone">{{ item.rank.name }}</p>
          <select
            v-model="item.rank.id"
            v-show="item.isClone"
            class="ds-select"
          >
            <option value="1">一级</option>
            <option value="2">二级</option>
            <option value="3">三级</option>
          </select>
        </td>
        <td>
          <p v-show="!item.isClone">
            <span>查看</span>
            <span @click="del(index, item.id)">删除</span>
            <span>修改</span>
            <span @click="useclone(item.id, index)">复制</span>
          </p>
          <p v-show="item.isClone">
            <span @click="save(index)">保存</span>
            <span>刷新</span>
            <span @click="del(index, item.id)">删除</span>
          </p>
        </td>
      </tr>
    </table>
  </div>
</template>
<script type="text/javascript">
export default {
  name: 'testBase',
  data () {
    return {
      items: [
        {
          id: 1,
          name: 'zhansgan1',
          hight: '168cm',
          weight: { id: 1, name: '168kg' },
          rank: { id: 1, name: '一级1' },
          isClone: false
        },
        {
          id: 2,
          name: 'zhansgan2',
          hight: '168cm',
          weight: { id: 2, name: '167kg' },
          rank: { id: 2, name: '一级2' },
          isClone: false
        },
        {
          id: 3,
          name: 'zhansgan3',
          hight: '168cm',
          weight: { id: 3, name: '166kg' },
          rank: { id: 3, name: '一级3' },
          isClone: false
        }
      ],
      rank: [],
      weight: {
        data: [
          {
            id: 1,
            name: '168kg'
          },
          {
            id: 2,
            name: '167kg'
          },
          {
            id: 3,
            name: '166kg'
          }
        ],
        index: -100,
        isShow: false
      }
    }
  },
  methods: {
    // 复制用户
    useclone (id, index) {
      const arr = this.items.slice(index, index + 1)[0]
      // 注意：转成json再反转回来，不能直接使用赋值，不然会和复制的一层公用同一个内存单元地址，会得到一样的结果
      const obj = JSON.stringify(arr)
      const newObj = JSON.parse(obj)
      console.log(newObj)
      /** 这里可以对复制的数据进行操作 ***/
      newObj.name = '' // 清空名字
      newObj.isClone = true // 显示操作按钮
      /** 这里可以对复制的数据进行操作 ***/
      this.items.splice(index + 1, 0, newObj)
    },
    // 选择体重弹窗
    showWeight (id, index) {
      this.weight.isShow = true
      this.weight.index = index
      console.log(this.weight)
    },
    // 体重弹窗选择对应体重
    chooseWeight (index, dbIndex) {
      console.log(this.items[dbIndex])
      console.log(this.weight.data[index])
      this.items[dbIndex].weight = this.weight.data[index]
    },
    // 保存用户信息
    save (index) {
      alert(JSON.stringify(this.items[index]))
      console.log(this.items[index])
    },
    // 删除用户信息
    del (index, userId) {
      this.items.splice(index, 1)
      console.log(userId)
    },
    // 阻止冒泡
    stopPropa () {
      const e = event || window.event
      e.stopPropagation()
      return false
    }
  },
  created () {
    // 初始化rank
    this.rank = [
      {
        id: 1,
        name: '一级1'
      },
      {
        id: 2,
        name: '一级2'
      },
      {
        id: 3,
        name: '一级3'
      }
    ]
  }
}
</script>
<style lang="less">
#layout {
  display: none;
}
table {
  border: 1px solid #ccc;
  width: 1000px;
  margin: 50px auto;
  color: #333;
}
.weightbox {
  height: 100px;
  width: 400px;
  margin: 50px auto;
}
.weightbox span {
  padding: 10px;
  border: 1px solid #ccc;
  color: #333;
}
.weightbox span input[type="radio"] {
  -webkit-appearance: radio;
}
.ds-select {
  -webkit-appearance: menulist;
}
table td {
  height: 50px;
  border: 1px solid #ccc;
  text-align: center;
}
table td span {
  color: blue;
  cursor: pointer;
}
table td input {
  width: 100px;
  height: 30px;
  background: none;
}
table td select {
  width: 100px;
  height: 30px;
  background: none;
}
</style>
