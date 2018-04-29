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
              <span id="instruction">Enter the amount you spend <em><b>per month</b></em> for:</span>
              <div v-for="item in expenseItems" class="money-item-fields">
                <div>
                  <b>{{item.label}}:</b>
                  <div class="money-item-amount float-right">
                    $<input type="number" step=".01" min="0" v-on:input="createChart(true)" v-model.number="item.count" v-bind:placeholder="item.label"/>
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
          {label: 'Utilities',       count: 1},
          {label: 'Rent/Mortgage',   count: 0},
          {label: 'Car',             count: 0},
          {label: 'Groceries',       count: 0},
          {label: 'Entertainment',   count: 0},
          {label: 'Miscellaneous',   count: 0}
        ]
      }
    },
    mounted (){
      this.createChart(false); //Create original chart, without removal of anything
    },
    methods: {
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
                                             '#528C18', '#C3F25C', '#FFB732',
                                             '#128518', '#5F6318', '#B266B2']);

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
        var legend = svg.selectAll('.legend')
                        .data(color.domain())
                        .enter()
                        .append('g')
                        .attr('class', 'legend')
                        .attr('transform', function(d, i) {
                          var height = legendRectSize + legendSpacing;
                          var offset =  height * color.domain().length / 2;
                          var horz = -2 * legendRectSize;
                          var vert = i * height - offset;
                          return 'translate(' + horz + ',' + vert + ')';
                        });
        legend.append('rect')
              .attr('width', legendRectSize)
              .attr('height', legendRectSize)
              .style('fill', color)
              .style('stroke', color);
        legend.append('text')
              .attr('x', legendRectSize + legendSpacing)
              .attr('y', legendRectSize - legendSpacing)
              .text(function(d) { return d; });
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

  .money-item-fields{
    padding:5px 0px 5px 30px;
  }
  .money-item-amount{
    display: inline-block;
    text-align: right;
    margin-right: 100px;
  }
  .legend {
    font-size: 12px;
  }
  .sub-budget{
    margin: 0px 40px;
    border-top: 1px solid lightgrey;
  }

  #budget-chart{
    height: 360px;
    width: 360px;
    position: relative;
  }
  #instruction{
    display: inline-block;
    padding: 25px 0px ;
  }
  #budgeting{
    background-color: whitesmoke;
    border: 1px solid lightgrey;
  }
</style>
