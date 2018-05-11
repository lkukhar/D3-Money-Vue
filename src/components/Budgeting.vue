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
                  <input type="color" id="color-picker" />
                  <input name="new-budget-item" id="new-budget-item" class="float-left new-budget" placeholder="New Item"/>
                  <input name="new-budget-amount" id="new-budget-amount" class="float-left new-budget" placeholder="$ Amount"/>
                  <button>Add</button>
                </div>
              </div>
              <div>
                <span class="instruction">Enter the amount you spend <em><b>per month</b></em> for:</span>
                <div v-for="item in expenseItems" class="money-item-fields">
                  <div>
                    <div class="color"></div>
                    <b>{{item.label}}:</b>
                    <div class="money-item-amount float-right">
                      <button class="float-right" type="button" name="delete" v-on:click="removeBudgetItem(item)">X</button>
                      $<input type="number" step="1" min="0" v-on:input="createChart(true)" v-model.number="item.count" v-bind:placeholder="item.label"/>
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
        expenseItems: [
          {label: 'Utilities',       count: 0, color: '#A60F2B'},
          {label: 'Rent',            count: 0, color: '#648C85'},
          {label: 'Car',             count: 0, color: '#B3F2C9'},
          {label: 'Groceries',       count: 0, color: '#528C18'},
          {label: 'Entertainment',   count: 0, color: '#C3F25C'}
        ]
      }
    },
    mounted (){
      this.createChart(false); //Create original chart, without removal of anything
    },
    methods: {
      removeBudgetItem: function(item){
        var index = this.expenseItems.indexOf(item);
        if (index > -1) {
          this.expenseItems.splice(index, 1);
        }
      },
      createChart : function(isUpdate){
        if(isUpdate){
          d3.selectAll("svg").remove(); //Remove old chart, then recreate if true
        }
        const width = 360;
        const height = 360;
        const donutWidth = 50;
        const legendRectSize = 18;
        const legendSpacing = 4;
        var radius = Math.min(width, height) / 2;

        var color = d3.scaleOrdinal().range(['#A60F2B', '#648C85', '#B3F2C9',
                                             '#528C18', '#C3F25C']);

        var svg = d3.select('#budget-chart')
                    .append('svg')
                    .attr('width', width)
                    .attr('height', height)
                    .append('g')
                    .attr('transform', 'translate(' + (width / 2) +  ',' + (height / 2) + ')');
        var arc = d3.arc()
                    .innerRadius(radius - donutWidth)
                    .outerRadius(radius);
        var pie = d3.pie()
                    .value(function(d) { return d.count; })
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
    background-color: black;
    margin-left:5px;
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
