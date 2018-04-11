<template>
  <div class="row" id="budgeting">
    <div class="col-md-12">
      <div class="row">
        <div class="col-md-12">
          <h3 class="text-center">Budgeting</h3>
          <h4 class="text-center">See where your money goes!</h4>
        </div>
      </div>
      <div class="row">
        <!-- INPUT -->
        <div class="col-md-4">
          <span>Enter the amount you spend <em>per month</em> for:</span>
          <div v-for="item in moneyItems" class="money-item-fields">
            <div>
              <b>{{item.name}}:</b>
              <input type="number" step=".01" min="0" v-model.number="item.amount" v-bind:placeholder="item.name"/>
            </div>
          </div>
        </div>
        <!-- END INPUT -->

        <!-- GRAPH -->
        <div class="col-md-6">

        </div>
        <!-- END GRAPH -->
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Budgeting',
    data(){
      return {
        moneyItems: [
          {name:'Credit Card',               amount:''},
          {name:'Utilities',                 amount:''},
          {name:'Rent',                      amount:''},
          {name:'Car & Insurance',           amount:''},
          {name:'Groceries',                 amount:''},
          {name:'Insurance',                 amount:''},
          {name:'Savings',                   amount:''},
          {name:'Entertainment',             amount:''},
          {name:'Investments',               amount:''}
        ]
      }
    }
  }
</script>

<style scoped>
  .money-item-fields{
    /* border-top: 1px solid lightgrey; */
    padding:5px 0px 5px 0px;
  }
  input{
    width: 50%;
    float:right;
  }
</style>
