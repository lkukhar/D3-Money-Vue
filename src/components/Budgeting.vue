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
                    .data(pie(this.moneyItems))
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
      var tooltip = d3.select('#chart')
                      .append('div')
                      .attr('class', 'tooltip');
      tooltip.append('div')
             .attr('class', 'label');
      tooltip.append('div')
             .attr('class', 'count');
      tooltip.append('div')
             .attr('class', 'percent');
      legend.append('rect')
            .attr('width', legendRectSize)
            .attr('height', legendRectSize)
            .style('fill', color)
            .style('stroke', color);
      legend.append('text')
            .attr('x', legendRectSize + legendSpacing)
            .attr('y', legendRectSize - legendSpacing)
            .text(function(d) { return d; });
    },
    methods: {
      updateChart : function(){
        d3.selectAll("svg").remove();
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
                      .data(pie(this.moneyItems))
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
        var tooltip = d3.select('#chart')
                        .append('div')
                        .attr('class', 'tooltip');
        tooltip.append('div')
               .attr('class', 'label');
        tooltip.append('div')
               .attr('class', 'count');
        tooltip.append('div')
               .attr('class', 'percent');
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
  .money-item-fields{
    padding:5px 0px 5px 0px;
  }
  .money-item-amount{
    display: inline-block;
    text-align: right;
    margin-right: 100px;
  }
  .legend {
    font-size: 12px;
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
  rect {
    stroke-width: 2;
  }
  input{
    width: 70%;
    float:right;
  }
  #budget-chart{
    height: 360px;
    width: 360px;
    position: relative;
  }
</style>
