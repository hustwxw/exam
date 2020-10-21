<template>
  <el-row>
    <el-col :span="3" style="margin-right: 20px">
      <el-select v-model="type" placeholder="请选择" @change="onSearch(true)">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
    </el-col>
    <el-col :span="8">
      <el-input v-model="keyword" placeholder="请输入题干搜索" clearable @keyup.native="onSearch" @clear="onSearch()"></el-input>
    </el-col>
    <el-col :span="4" style="margin-left: 20px">
      <el-button type="primary" @click="onSearch">查询</el-button>
    </el-col>
    <el-col :span="24" style="margin-top: 20px;width: 100%">
      <el-table
        border 
        :data="result"
        style="width: 100%"
        v-loading="loading"
      >
        <el-table-column
          label="序号"
          type="index"
        >          
        </el-table-column>
        <el-table-column
          prop="ques"
          label="题干"
          width="500px"
          fit
        >
        </el-table-column>
        <el-table-column
          label="答案"
          fit
        >
          <template slot-scope="scope">
            <div v-for="(ele, index) in scope.row.answer" :key="ele" style="margin: 0 10px 10px 0">
              <el-tag v-if="ele.indexOf('注意') === -1">
                {{type === 0 ? `${index + 1}、` : null}}{{ele}}
              </el-tag>
              <el-tag v-else type="danger">
                {{ele}}
              </el-tag>
            </div>
          </template>
        </el-table-column>
      </el-table>
    </el-col>
  </el-row>
</template>

<script>
import mutiple from '../data/mutiple'
import radio from '../data/radio'
import tf from '../data/tf'
export default {
  name: 'HelloWorld',
  data () {
    return {
      keyword: '',
      result: [],
      loading: false,
      type: 0,
      options: [
        {
          value: 0,
          label: '多选'
        },
        {
          value: 1,
          label: '单选'
        },
        {
          value: 2,
          label: '判断'
        }
      ]
    }
  },
  methods: {
    onSearch (flag) {
      if (flag) {
        this.result = []
      }
      this.loading = true
      setTimeout(() => {
        this.result = [mutiple, radio, tf][this.type].filter(e => (e.ques || '').indexOf(this.keyword) > -1)
        this.loading = false
      }, 0)
    }
  },
  mounted () {
    this.result = mutiple
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
