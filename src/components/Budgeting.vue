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
              <span>Enter the amount you spend <em><b>per month</b></em> for:</span>
              <div v-for="item in moneyItems" class="money-item-fields">
                <div>
                  <b>{{item.label}}:</b>
                  <div class="money-item-amount float-right">
                    $<input type="number" step=".01" min="0"  v-model.number="item.count" v-bind:placeholder="item.label"/>
                  </div>
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
  const d3 = require('d3');
  export default {
    name: 'Budgeting',
    data () {
      return {
        moneyItems: [
          {label: 'Credit Card',     count: 0},
          {label: 'Utilities',       count: 0},
          {label: 'Rent',            count: 0},
          {label: 'Car & Insurance', count: 0},
          {label: 'Groceries',       count: 0},
          {label: 'Insurance',       count: 0},
          {label: 'Savings',         count: 0},
          {label: 'Entertainment',   count: 0},
          {label: 'Investments',     count: 0}
        ]
      }
    }
  }

</script>

<style scoped>
  .money-item-fields{
    padding:5px 0px 5px 0px;
  }
  .money-item-amount{
    display: inline-block;
    text-align: right;
  }
  input{
    width: 70%;
    float:right;
  }
</style>
