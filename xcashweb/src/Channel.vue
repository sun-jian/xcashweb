<template>
  <div id="channel">
    <h1>{{ msg }}</h1>
    <ul v-for="channel in channels">
      <li>{{channel.companyName}} ( {{channel.channelName}}  -  {{channel.billType}} ) </li>
     	<ul v-for="storeId in channel.extStoreIds">
	   <li> {{storeId}} </li>
	</ul> 
     </li>
    </ul>
    <ul> 
  	
  </div>
</template>

<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>

<script>
export default {
  name: 'channel',
  data () {
    return {
      baseUrl: 'http://106.14.47.193',
      msg: '通道管理',
      channels: [{"companyName": "上海纳优信息技术有限公司", "channelName": "银商悦单", "billType": "T1", "extStoreIds": ["898319848160219", "898319848160222", "898319848160223", "898319848160224", "898319848160225"]},{"companyName": "上海纳优信息技术有限公司", "channelName": "银商C扫B", "billType": "T1", "extStoreIds": ["898319848160307", "898319848160308", "898319848160309", "898319848160310", "898319848160311", "898319848160312", "898319848160313", "898319848160314", "898319848160315", "898319848160316"]},{"companyName": "上海纳优信息技术有限公司", "channelName": "银商C扫B", "billType": "D0", "extStoreIds": ["898319848160268", "898319848160269", "898319848160270", "898319848160271", "898319848160272", "898319848160273", "898319848160274", "898319848160275", "898319848160276", "898319848160277"]},{"companyName": "上海微分信息网络有限公司", "channelName": "银商C扫B", "billType": "D0", "extStoreIds": ["898319848160297", "898319848160298", "898319848160299", "898319848160300", "898319848160301", "898319848160302", "898319848160303", "898319848160304", "898319848160305", "898319848160306"]},{"companyName": "深圳市欧莱娜服饰有限公司", "channelName": "银商悦单", "billType": "T1", "extStoreIds": ["898319856510089 (仅支付宝)", "898319856510090 (仅支付宝)", "898319856510091 (仅微信)", "898319856510092  (仅微信)", "898319856510093 (仅微信)", "898319856510095 (仅微信)", "898319856510096 (仅微信)", "898319856510097 (仅微信)", "898319856510098 (仅微信)", "898319856510099 (仅微信)"]}],
   }
  },
}
</script>

<style>
#channel {
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
