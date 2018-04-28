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
            <div class="col-md-6">
              <span>Enter the amount you spend <em><b>per month</b></em> for:</span>
              <div v-for="item in moneyItems" class="money-item-fields">
                <div>
                  <b>{{item.label}}:</b>
                  <div class="money-item-amount float-right">
                    $<input type="number" step=".01" min="0" v-on:input="updateChart" v-model.number="item.count" v-bind:placeholder="item.label"/>
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
        moneyItems: [
          {label: 'Credit Card',     count: 1},
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
    },
    mounted (){
      const width = 360;
      const height = 360;
      var radius = Math.min(width, height) / 2;

      var color = d3.scaleOrdinal().range(['#A60F2B', '#648C85', '#B3F2C9',
                                           '#528C18', '#C3F25C', '#D3435C',
                                           '#128518', '#5F6318', '#C23448']);

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
                  .value(function(d) { return d.count; })
                  .sort(null);
      var path = svg.selectAll('path')
                    .data(pie(this.moneyItems))
                    .enter()
                    .append('path')
                    .attr('d', arc)
                    .attr('fill', function(d, i) {
                       return color(d.data.label);
                    });
    },
    methods: {
      updateChart : function(){
        d3.selectAll("svg").remove();
        const width = 360;
        const height = 360;
        var radius = Math.min(width, height) / 2;

        var color = d3.scaleOrdinal().range(['#A60F2B', '#648C85', '#B3F2C9',
                                             '#528C18', '#C3F25C', '#D3435C',
                                             '#128518', '#5F6318', '#C23448']);

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
                    .value(function(d) { return d.count; })
                    .sort(null);
        var path = svg.selectAll('path')
                      .data(pie(this.moneyItems))
                      .enter()
                      .append('path')
                      .attr('d', arc)
                      .attr('fill', function(d, i) {
                         return color(d.data.label);
                      });
      }
    },
  }

</script>

<style scoped>
  .money-item-fields{
    padding:5px 0px 5px 0px;
  }
  .money-item-amount{
    display: inline-block;
    text-align: right;
    margin-right: 100px;
  }
  input{
    width: 70%;
    float:right;
  }
</style>
