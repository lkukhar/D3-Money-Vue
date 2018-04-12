<template>
  <section>
    <div class="container">
      <div class="row" id="budgeting">
        <div class="col-md-12">

          <!-- SUBHEADER -->
          <div class="row">
            <div class="col-md-12">
              <h3 class="text-center">Budgeting</h3>
              <h4 class="text-center">See where your money goes!</h4>
            </div>
          </div>
          <!-- END SUB HEADER -->

          <!-- INPUT AND CHART -->
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

            <!-- CHART -->
            <div class="col-md-6" id="budget-chart">

            </div>
            <!-- END CHART -->

          </div>
          <!-- END INPUT AND CHART -->

        </div>
      </div>
    </div>
  </section>
</template>

<script>
  import * as d3 from 'd3';
  export default {
    name: 'Budgeting',
    data () {
      return {
        moneyItems: [
          {name: 'Credit Card',     amount: ''},
          {name: 'Utilities',       amount: ''},
          {name: 'Rent',            amount: ''},
          {name: 'Car & Insurance', amount: ''},
          {name: 'Groceries',       amount: ''},
          {name: 'Insurance',       amount: ''},
          {name: 'Savings',         amount: ''},
          {name: 'Entertainment',   amount: ''},
          {name: 'Investments',     amount: ''}
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
