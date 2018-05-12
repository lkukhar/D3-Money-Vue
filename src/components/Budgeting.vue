<template>
  <section>
    <div class="container">
      <div class="row" id="budgeting">
        <div class="col-md-12">

          <!-- SUBHEADER -->
          <div class="row">
            <div class="col-md-12">
              <h3 class="text-center">Budget</h3>
              <h4 class="text-center">Visualize your spending!</h4>
            </div>
          </div>
          <!-- END SUB HEADER -->

          <!-- INPUT AND CHART -->
          <div class="row sub-budget">
            <!-- INPUT -->
            <div class="col-md-6">
              <div>
                <span class="instruction">Enter another budget item:</span>
                <div>
                  <input type="color" id="color-picker" v-model="newExpenseItem.color" />
                  <input name="new-budget-item" id="new-budget-item" class="float-left new-budget" placeholder="New Item" v-model="newExpenseItem.label"/>
                  <input type="number" step="1" min="0" name="new-budget-amount" id="new-budget-amount" class="float-left new-budget"  placeholder="$ Amount" v-model="newExpenseItem.amount"/>
                  <button class="btn-success" v-on:click="addBudgetItem()">Add</button>
                </div>
              </div>
              <div>
                <span class="instruction">Enter the amount you spend <em><b>per month</b></em> for:</span>
                <div v-for="item in expenseItems" class="money-item-fields">
                  <div>
                    <div class="color" v-bind:style="{'background-color': item.color}"></div>
                    <b>{{item.label}}:</b>
                    <div class="money-item-amount float-right">
                      <button class="float-right btn-danger" type="button" name="delete" v-on:click="removeBudgetItem(item)">X</button>
                      $<input type="number" step="1" min="0" v-on:input="createChart(true)" v-model.number="item.amount" v-bind:placeholder="item.label"/>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- END INPUT -->

            <!-- CHART -->
            <div class="col-md-6 text-center" id="budget-chart">

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
        newExpenseItem: {label: '', amount: '', color: '#000000'},
        expenseItems: [
          {label: 'Utilities',       amount: 1, color: '#A60F2B'},
          {label: 'Rent',            amount: 0, color: '#648C85'},
          {label: 'Car',             amount: 0, color: '#B3F2C9'},
          {label: 'Groceries',       amount: 0, color: '#528C18'},
          {label: 'Entertainment',   amount: 0, color: '#C3F25C'}
        ]
      }
    },
    mounted (){
      this.createChart(false); //Create original chart, without removal of anything
    },
    methods: {
      isValidNewExpenseItem: function(newExpenseItem){
        return /\S/.test(newExpenseItem.label) //Check that it has atlease one letter in it
               && newExpenseItem.amount != null
               && /\S/.test(newExpenseItem.color)
      },
      addBudgetItem: function(){
        if(this.isValidNewExpenseItem(this.newExpenseItem)){
          this.expenseItems.push(this.newExpenseItem);
          this.newExpenseItem = {label: '', count: '', color: '#000000'};
        }
        this.createChart(true);
      },
      removeBudgetItem: function(item){
        var index = this.expenseItems.indexOf(item);
        if (index > -1) {
          this.expenseItems.splice(index, 1);
        }
        this.createChart(true);
      },
      createChart : function(isUpdate){
        if(isUpdate){
          d3.selectAll("svg").remove(); //Remove old chart, then recreate if true
        }
        const width = 360;
        const height = 360;
        var radius = Math.min(width, height) / 2;
        var colors = this.expenseItems.map(x => x.color);

        var color = d3.scaleOrdinal().range(colors);

        var svg = d3.select('#budget-chart')
                    .append('svg')
                    .attr('width', width)
                    .attr('height', height)
                    .append('g')
                    .attr('transform', 'translate(' + (width / 2) +  ',' + (height / 2) + ')');
        var arc = d3.arc()
                    .innerRadius(0)
                    .outerRadius(radius);
        var pie = d3.pie()
                    .value(function(d) { return d.amount; })
                    .sort(null);
       var path = svg.selectAll('path')
                     .data(pie(this.expenseItems))
                     .enter()
                     .append('path')
                     .attr('d', arc)
                     .attr('fill', function(d, i) {
                        return color(d.data.label);
                     });
      }
    }
  }

</script>

<style scoped>
  span{
    margin-left: 30px;
  }

  rect {
    stroke-width: 2;
  }
  input{
    width: 70%;
    float:right;
  }

  .instruction{
    display: inline-block;
    padding: 25px 0px ;
  }
  .money-item-fields{
    padding:5px 0px 5px 30px;
  }
  .money-item-amount{
    display: inline-block;
    text-align: right;
    margin-right:40px;
  }
  .legend {
    font-size: 12px;
  }
  .sub-budget{
    margin: 0px 40px;
    border-top: 1px solid lightgrey;
  }
  .new-budget{
    width:30%;
  }
  .color{
    display:inline-block;
    width:15px;
    height:15px;
    margin-left:5px;
  }
  .tooltip {
    background: #eee;
    box-shadow: 0 0 5px #999999;
    color: #333;
    display: none;
    font-size: 12px;
    left: 130px;
    padding: 10px;
    position: absolute;
    text-align: center;
    top: 95px;
    width: 80px;
    z-index: 10;
  }
  #budget-chart{
    height: 360px;
    width: 360px;
    position: relative;
  }
  #budgeting{
    background-color: whitesmoke;
    border: 1px solid lightgrey;
  }
  #new-budget-item{

    width:40%
  }
  #new-budget-amount{
    width:32%
  }
  #color-picker{
    width:5%;
    float:left;
    height:30px;
    margin-left:6%;
  }
</style>
