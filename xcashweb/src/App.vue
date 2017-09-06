<template>
  <vue-tabs>
   <v-tab title="订单管理">
       <order/>
    </v-tab>
<!--
     <v-tab title="代付管理">
      <cash/>
    </v-tab>
-->
    <v-tab title="商户管理">
       Coming soon...
    </v-tab>

    <v-tab title="通道管理">
      Coming soon...
    </v-tab>
</vue-tabs>
</template>
<script>
import Cash from './Cash.vue'
import Order from './Order.vue'
import {VueTabs, VTab} from 'vue-nav-tabs'

export default {
    name: 'app',
    data: {
    },
    components: {
        Cash,
	Order,
	VueTabs,
	VTab
    }
}
</script>
