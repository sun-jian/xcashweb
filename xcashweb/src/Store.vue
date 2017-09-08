<template>
  <div id="store">
    <h1>{{ msg }}</h1>
    <div id="storeForm">
	<table border="1">
 	 <tr>
   	        <th>客户名称</th>
   	        <th>APP Key</th>
		<th>商户号</th>
		<th>保证金比率(%)</th>
		<th>保证金通道</th>
		<th>日交易额上限(万元)</th>
		<th>通道信息</th>
 	</tr>
  	<tr v-for="store in stores">
    		<td>{{store.name}}</td>
    		<td>{{store.appKey}}</td>
		<td>{{store.code}}</td>
		<td>{{store.bailPercentage}}</td>
		<td>
                  <table>
                     <tr v-for="bailChannel in store.bailStoreChannels">
                        <td>
                               <p v-bind:title="bailChannel.paymentGatewayName + ' ' + bailChannel.billType"> {{bailChannel.extStoreId}} </p>
                        </td>
                    </tr>
                 </table>
                </td>
		<td>{{store.dailyLimit}}</td>
		<td>
		  <table>
		     <tr v-for="channel in store.channels">	
			<td>
				<p v-bind:title="channel.paymentGatewayName + ' ' + channel.billType"> {{channel.extStoreId}} </p>
			</td>
		    </tr>
	         </table>
		</td>
  	</tr>
	</table>
    </div>
  </div>
</template>

<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>

<script>
export default {
  name: 'store',
  data () {
    return {
      baseUrl: 'http://106.14.47.193',
      msg: '商户管理',
      transactionResponse: '',
      preAddStore: false,
      storeName: '',
      storeCode: '',
      appKey: '',
      bailPercentage: '',
      dailyLimit: '',
      channels: [],
      stores: []  
   }
  },

  methods: {
 	isBlank(str) {
           return (!str || /^\s*$/.test(str));
        },
	
	preAddNewStore() {
	   this.preAddStore=true;
	},

	queryAllStores() {
		var vm = this
		var storesUrl = vm.baseUrl + '/xcash/stores'
		axios.get(storesUrl)
		.then(function (response) {
		   vm.stores = response.data
		})
		.catch(function (error) {
            		vm.transactionResponse = '查询失败. ' + error.data
          	})
	 },

  },

  beforeMount(){
    this.queryAllStores()
  },
}
</script>

<style>
#store {
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
