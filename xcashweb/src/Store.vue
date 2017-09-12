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
		<th>操作</th>
 	</tr>
  	<tr v-for="store in stores" v-bind:key="store.code">
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
		<td>
		   <table>
	 	    <div v-if="!store.codeUrl">		
		     <tr>
			<td>
			<input type="button" v-on:click="getTestLink(store)" value="获取测试二维码"/>
			</td>
		     </tr>
                    </div>
		    <div v-else>
		     <tr>
			<td>
                         <input type="button" v-on:click="clearTestLink(store)" value="清除测试二维码"/>
                        </td>
		     <tr>
		     </tr>
			<td>
			 <img :src="store.qrCodeUrl" width="150" height="150"/>
			</td>
	    	     </tr>
		    </div>
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
	
	getTestLink(store) {
		var vm = this
		var orderUrl = vm.baseUrl + '/xcash/stores/' + store.code + '?appKey='+ store.appKey
		axios.get(orderUrl)
		.then(function (response) {
		    vm.$set(store, 'codeUrl', response.data.codeUrl)
		    vm.$set(store, 'qrCodeUrl', 'http://pan.baidu.com/share/qrcode?w=150&h=150&url='+response.data.codeUrl)
		})
		.catch(function (error) {
                     vm.transactionResponse = '测试失败. ' + error.data
                 })
	},

	clearTestLink(store) {
		var vm = this
		vm.$set(store, 'codeUrl', null)
		vm.$set(store, 'qrCodeUrl', null)
	}

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
