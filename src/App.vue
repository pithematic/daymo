<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>程序员专用-北京避雷查询</h1>
  <v-form>
    <v-container>
      <p>众所周知，我大帝都人口众多，程序员也是其中很大的群体，但是因为互联网行业加班多，大家又多来自五湖四海，刚毕业时还蹦的欢，可工作多年结婚生娃之后，烦恼也就来了。有时迫于无奈，家中老人无法帮助带娃，就需要请月嫂或者育儿嫂，可无奈这个行业鱼龙混杂，中介公司也多是无法约束旗下阿姨们，再加上钱好赚，形形色色的阿姨涌入，难免就会发生各种令人不开心的事情。可孩子又是家中的大事，虽然做着程序员，拿着高工资，可又要还房贷，又要请阿姨，如果碰到一个无良阿姨，那真是肠子都悔青了，家庭不和睦不说，还对辛辛苦苦生下来的孩子造成伤害。p.s. 往往这个时候，都会反思我加班是为了啥。。。
所以我想带个头，先从量化“无良”阿姨开始，做个记录，无论是网上公开的信息，还是各位同行自己的经历，因某些原因不能公开发生，那我们都可以记录下来，我始终相信“无良”阿姨是少数，只要把她们都找出来，避免更多人上当受骗，总是有益的，当然也希望未来政府可以对这块有更好的监管，降低大家养育一胎或者二胎的这些非经济因素的成本。
</p>
<v-divider></v-divider>
    <h3>我们建立了一套数据库查询系统</h3>
    <p>通过输入阿姨的任意信息，我们帮助你进行模糊查找，检查是否在其它妈妈举报过的黑名单上</p>
   <v-divider
  vertical
></v-divider>
      <v-row>
        <v-col
          cols="12"
          sm="6"
        >
          <v-text-field
            v-model="name"
            :rules="rules"
            counter="25"
            hint="不确定的字符用 ？或 * 替代。如 张*雷"
            label="输入育儿嫂姓名"
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          sm="6"
        >
          <v-text-field
            v-model="phone"
            :rules="rules"
            counter
            maxlength="11"
            hint="例如 15500000000"
            label="输入育儿嫂手机号"
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          sm="6"
        >
          <v-text-field
            v-model="id"
            :rules="wordsRules"
            counter="18"
            hint="例如 350100190001010000"
            label="输入育儿嫂身份证号码"
            :counter-value="v => v.trim().split(' ').length"
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          sm="6"
        >
            <v-text-field
              v-model="hometown"
              :rules="wordsRules"
              counter="25"
              hint="This field counts words instead of characters"
              label="输入育儿嫂户籍"
            >       
          </v-text-field>
           
        </v-col>
      </v-row>
       <v-btn
          elevation="2"
          btn
          @click="getData()"
        >获取</v-btn>
         <v-btn
          elevation="2"
          btn
          @click="postData()"
        >提交</v-btn>
      <v-divider
          vertical
        ></v-divider>
       
      <p>{{this.results}}</p>
    </v-container>
  </v-form>

   
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  methods: {
    async getData(){
      const getResponst = await axios.get('https://n7vhhikswi.execute-api.us-west-2.amazonaws.com/test/geoinfo/'
      // ,{headers:{Authorization:`bT0FUFS7PHcF4sA=`}}
      )
      // const getResponst = await axios.post('https://n7vhhikswi.execute-api.us-west-2.amazonaws.com/test/geoinfo/',{name:this.name})

      console.log(getResponst)
      this.results = getResponst.data.body
    },
    async postData(){
      // const getResponst = await axios.get('https://n7vhhikswi.execute-api.us-west-2.amazonaws.com/test/geoinfo/')
      const getResponst = await axios.post('https://n7vhhikswi.execute-api.us-west-2.amazonaws.com/test/daymo/',{
        "name":this.name
        },
        {
          headers:{
            "Content-Type":"application/json",
            "Access-Control-Allow-Origin":"*"}},);
      
      console.log(getResponst)
      this.results = getResponst.data
    },
    sendData: function (event) {
      // `this` inside methods points to the Vue instance
      alert('Hello ' + this.name + '!')
      console.log(this.name,this.phone,this.id)
      // `event` is the native DOM event
      if (event) {
        alert(event.target.tagName)
      }
    }
  },
   data () {
      return {
        
        name: '',
        phone: '',
        id: '',
        hometown: '',
        rules: [v => v.length <= 25 || 'Max 25 characters'],
        wordsRules: [v => v.trim().split(' ').length <= 5 || 'Max 5 words'],
        results:'',
      }
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
