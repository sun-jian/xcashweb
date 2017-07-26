<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <ul>
      <li>
        <p>请选择代付通道:</p>
	<select v-model="channel" id="channel" name="channel" >
		<option value="juzhen_test">钜真测试通道</option>
		<option value="juzhen">钜真正式通道</option>
	</select>
     </li>
    </ul>
    <ul>
      <li><input type="text" v-model="accountName" id="accountName" placeholder=" 请输入收款人姓名 " /></li>
    </ul>
    <ul>
      <li><input type="text" v-model="cardNum" id="cardNum" placeholder=" 请输入收款账号 " /></li>
    </ul>
    <ul>
      <li><input type="text" v-model="bankName" id="bankName" placeholder=" 请输入开户银行名称 " /></li>
    </ul> 
   <ul>
      <li><input type="text" v-model="totalFee" id="totalFee" placeholder=" 请输入金额 " /></li>
    </ul>
    <ul>
      <li><input type="text" v-model="orderId" id="orderId" placeholder=" 请输入订单号 " /></li>
    </ul>
    <ul>
      <li><input type="button" v-on:click="postCashing"  value="付款"/></li>
    </ul>
    <ul>
      <li><input type="button" v-on:click="getBalance" value="查看帐户余额"/></li>
    </ul>
    <ul>
      <li><input type="button" v-on:click="getOrderStatus"  value="查看代付交易状态"/></li>
    </ul>
    <ul>
      <li>{{ transactionResponse }}</li>
    </ul>
  </div>
</template>

<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: '代付页面',
      transactionResponse: '',
     }
  },

  methods: {
 	getBalance() {
		this.transactionResponse = '提交中......'
		var vm = this
		axios.post('http://106.14.47.193/xcash/v1/balance', {channel: this.channel})
		.then(function (response) {
		   vm.transactionResponse = response.data
		})
		.catch(function (error) {
            		vm.transactionResponse = '操作失败. ' + error
          	})
	 },

	postCashing() {
                this.transactionResponse = '提交中......'
                var vm = this
                axios.post('http://106.14.47.193/xcash/v1/cashing', {channel: this.channel,cardNum:this.cardNum, accountName:this.accountName, bankName:this.bankName, totalFee:this.totalFee,purpose:"代付"})
                .then(function (response) {
                   vm.transactionResponse = response.data
                })
                .catch(function (error) {
                        vm.transactionResponse = '操作失败. ' + error
                })
         },

	getOrderStatus() {
                this.transactionResponse = '提交中......'
                var vm = this
                axios.post('http://106.14.47.193/xcash/v1/query', {channel: this.channel, orderId:this.orderId})
                .then(function (response) {
                   vm.transactionResponse = response.data
                })
                .catch(function (error) {
                        vm.transactionResponse = '操作失败. ' + error
                })
         }
  } 
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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
