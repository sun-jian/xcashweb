<template>
  <div id="order">
    <h1>{{ msg }}</h1>
    <ul>
	<li>
		<form role="form" class="form" onsubmit="return false;">
		<div v-if="!image">
   		 	<input type="file" @change="onFileChange">
 		 </div>
		<div v-else>
   			 <img :src="image" width="280" height="480"/>
   		 	<button @click="removeImage">换一张</button>
  		</div>
		</form>
	</li>
    </ul>
    <ul>
      <li><input type="text" v-model="targetOrderNo" id="targetOrderNo" placeholder=" 请输入微信／支付宝订单号 " size="40" /></li>
    </ul>
    <ul>
      <li><input type="text" v-model="extOrderNo" id="extOrderNo" placeholder=" 请输入银行订单号 " size="40" /></li>
    </ul>
    <ul>
      <li><input type="text" v-model="orderNo" id="orderNo" placeholder=" 请输入平台订单号 " size="40" /></li>
    </ul> 
   <ul>
      <li><input type="text" v-model="sellerOrderNo" id="sellerOrderNo" placeholder=" 请输入客户订单号 " size="40" /></li>
    </ul>
    <ul>
      <li><input type="button" v-on:click="queryOrder"  value="查询订单"/></li>
    </ul>
    <ul>
      <li><input type="button" v-on:click="refund" value="退款"/></li>
    </ul> 
    <ul>
      <li><input type="button" v-on:click="clear" value="清空"/></li>
    </ul>
    <ul>
      <li>{{ transactionResponse }}</li>
    </ul>
    <div id="orderForm" v-if="!this.isBlank(this.order.status)">
	<table border="1">
 	 <tr>
   	        <th>微信／支付宝订单号</th>
   	        <th>银行订单号</th>
		<th>客户订单号</th>
		<th>应用跳转地址</th>
		<th>金额</th>
		<th>订单状态</th>
		<th>交易时间</th>
		<th>客户名称</th>
		<th>通道来源</th> 
 	</tr>
  	<tr>
    		<td>{{order.targetOrderNo}}</td>
    		<td>{{order.extOrderNo}}</td>
		<td>{{order.sellerOrderNo}}</td>
		<td>{{order.returnUrl}}</td>
		<td>{{order.totalFee}}</td>
		<td>{{order.status}}</td>
		<td>{{order.updateDate}}</td>
		<td>{{order.storeName}}</td>
		<td>{{order.paymentGatewayName}}</td>
  	</tr>
	</table>
    </div>
  </div>
</template>

<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>

<script>

export default {
  name: 'order',
  data () {
    return {
      baseUrl: 'http://106.14.47.193',
      msg: '订单管理',
      transactionResponse: '',
      targetOrderNo: '',
      extOrderNo: '',
      order: {sellerOrderNo: '',returnUrl: '', status: '',targetOrderNo: '',extOrderNo: '', totalFee: '', updateDate: '', storeNmae: '', appKey: '', paymentGateway: '', channelNo: ''},
      image: '', 
    }
  },

  methods: {
 	isBlank(str) {
           return (!str || /^\s*$/.test(str));
        },

	onFileChange(e) {
      		var files = e.target.files || e.dataTransfer.files;
     		if (!files.length)
        		return;
     		this.createImage(files[0]);
		this.uploadImage(files[0]);
    	},
    
	createImage(file) {
      		var image = new Image();
      		var reader = new FileReader();
      		var vm = this;

      		reader.onload = (e) => {
 	      	 	vm.image = e.target.result;
      		};

     		reader.readAsDataURL(file);
	},

	uploadImage(file) {
		var vm = this;

		var data = new FormData();
		data.append('file', file);
		var orderUrl = vm.baseUrl + '/upload';
		axios.post(orderUrl, data)
		.then(function (res) {
			vm.targetOrderNo = res.data.targetOrderNo;
           		vm.transactionResponse = '上传成功';
		})
            	.catch(function (err) {
			alert('FAILED'+err);
              		vm.transactionResponse = '上传失败';
           	 });
	},

	removeImage: function (e) {
     		 this.image = '';
    	},

	

	clear() {
		this.order = {sellerOrderNo: '',returnUrl: '', status: '',targetOrderNo: '',extOrderNo: '', totalFee: '', updateDate: '', storeNmae: '', appKey: '', paymentGateway: '', channelNo: ''}
		this.transactionResponse = ''
		this.targetOrderNo = ''
		this.extOrderNo = ''
		this.orderNo = ''
		this.returnUrl = ''	
		this.image = ''
	},
	
	resetProcessing() {
		this.transactionResponse = '提交中......'
		this.order = {sellerOrderNo: '',returnUrl: '', status: '',targetOrderNo: '',extOrderNo: '', totalFee: '', updateDate: '', storeNmae: '', appKey: '', paymentGateway: '', channelNo: ''}
	},

	queryOrder() {
		this.resetProcessing()
		var vm = this
		var orderUrl = vm.baseUrl + '/xcash/order?'
		if(!this.isBlank(this.targetOrderNo)) {
			orderUrl = orderUrl + 'targetOrderNo=' + this.targetOrderNo
		} else if(!this.isBlank(this.extOrderNo)) {
			orderUrl = orderUrl + 'extOrderNo=' + this.extOrderNo
		} else if(!this.isBlank(this.orderNo)) {
			orderUrl = orderUrl + 'orderNo=' + this.orderNo
		} else if(!this.isBlank(this.sellerOrderNo)) {
			orderUrl = orderUrl + 'sellerOrderNo=' + this.sellerOrderNo
		}
		axios.get(orderUrl)
		.then(function (response) {
		   vm.transactionResponse = '查询成功'
		   vm.order = response.data
		})
		.catch(function (error) {
            		vm.transactionResponse = '查询失败. ' + error.data
          	})
	 },

	refund() {
                this.resetProcessing()
		var vm = this
		var refundUrl = vm.baseUrl + '/xcash/refund?'
                if(!this.isBlank(this.targetOrderNo)) {
                        refundUrl = refundUrl + 'targetOrderNo=' + this.targetOrderNo
                } else if(!this.isBlank(this.extOrderNo)) {
                        refundUrl = refundUrl + 'extOrderNo=' + this.extOrderNo
                } else if(!this.isBlank(this.orderNo)) {
                        refundUrl = refundUrl + 'orderNo=' + this.orderNo
                } else if(!this.isBlank(this.sellerOrderNo)) {
                        refundUrl = refundUrl + 'sellerOrderNo=' + this.sellerOrderNo
                }
                axios.delete(refundUrl)
                .then(function (response) {
		   if(response.data.status === 'REFUND') {
                   	vm.transactionResponse = '退款成功'
                   } else {
			vm.transactionResponse = '退款失败'
	     	   }
		   vm.order = response.data	
		})
                .catch(function (error) {
                        vm.transactionResponse = '退款失败. ' + error.data
                })
         }
  } 
}
</script>

<style>
#order {
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
