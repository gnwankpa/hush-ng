<template>
  <div>
    Transactions can be sent </br> from a zaddr or taddr depending on your preference
    <div class="container" >
      <el-row class="caption">
        <el-col :span="16" >Create a transaction</el-col>
        <el-col :span="8" class="balance">Spendable Balance: <span>{{ fakeBalance }}</span></el-col>
      </el-row>
      
      <el-form ref="form" :model="transactionForm" label-width="60px" >
        <el-form-item label="From" >
          <el-select v-model="transactionForm.from" placeholder="Select" style="width:100%;">
            <el-option
              v-for="address in addresses"
              :key="address.address"
              :label="address.address"
              :value="address.address">             
          </el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="To">
          <el-select v-model="transactionForm.destinationAddresses" multiple filterable allow-create placeholder="put address here or choose from your contacts" style="width:100%;">
            <el-option-group v-for="group in groupedDestinationAddresses" :key="group.label" :label="group.label">
              <el-option
                v-for="item in group.addresses"
                :key="item.address"
                :label="item.nickName"
                :value="item.address">
                  <span style="float: left;width:100px;">{{ item.nickName }}</span>
                  <span class="address" style="float: left">{{ item.address }}</span>
                  <span style="float: right; font-size: 13px">{{ item.balance }}</span>
              </el-option>
            </el-option-group>
          </el-select>
        </el-form-item>
        <el-form-item label="Amount">
          <el-col :span="11">
            <el-input placeholder="" v-model="transactionForm.amount" style="width: 95%;"></el-input>
          </el-col>
          <el-col :span="2">Fee</el-col>
          <el-col :span="11">
            <el-input placeholder="" v-model="transactionForm.fee" style="width:100%;"></el-input>
          </el-col>
        </el-form-item>
      </el-form>         
      <el-button type="primary" @click="createTransaction">Create</el-button>
    </div>
    <div class="container" >
      <el-row class="caption">
        <el-col :span="16" >Transaction History</el-col>
      </el-row>
      <el-row class="intro">
        <el-col :span="24" >click on a row to open block explorer</el-col>
      </el-row>

      <el-table :data="transactions" height="280" style="width: 100%" empty-text="None">
        <el-table-column prop="category" label="Category" width="100"> </el-table-column>
        <el-table-column prop="amount" label="Amount" width="180"> </el-table-column>
        <el-table-column prop="address" label="Address" width="*" class-name="address"> </el-table-column>
        <el-table-column prop="confirmations" label="Conf" width="60"> </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
  import { mapState,mapGetters, mapActions } from 'vuex'
  import CloseButton from '../shared/CloseButton'

  const Repeat = require('repeat')
  var store = require('store')

  export default {
    name: 'transactions',
    components: { CloseButton },
    data () {
       return {
         transactionForm : {
          from: null,
          destinationAddresses: [],
          amount: 0.0,
          fee: 0.0001
        },
        fakeBalance: 0.0
      }
    },
    computed:{
      ...mapState([
        'addresses',       
        'transactions',
        'availableBalance',  
        'contacts',
        'groupedDestinationAddresses'
      ]),                 
      ...mapGetters([
        'tAddresses'
      ])           
    },
    methods: {
      createTransaction () {                     
        this.$store.dispatch('sendToMany',this.transactionForm);  
      }
      
    },
    mounted: function() {
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Poppins:300,400,500,700');

  * {
    font-family: 'Poppins', sans-serif;
    color: #2d2d2d;
  }

.address {
    font-family: 'Courier', sans-serif;
    font-size:8pt;  }


  .el-select-dropdown.is-multiple .el-select-dropdown__item.selected::after {
    left: 3px;
  }

</style>
