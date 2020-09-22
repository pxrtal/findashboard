<template>
  <div class="hello">
    <div class="container">
      <form class="navbar-form m-2">
        <div class="input-group no border">
          <input type="text" value="" class="form-control" placeholder="Buscar..." v-model="stock">
          <button @click="update(stock)" class="btn btn-white btn-round bn-just-icon">
            <i class="material-icons al-3">search</i>
          </button>

        </div>
      </form>

      <!-- Informacion basica-->
      <div class="div" v-if="price !==''">
      <div class="row">
        <div class="col-md-3">
          <div class="card card-stats card-background">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">content_copy</i>
              </div>
              <p class="card-category">Beta</p>
              <h4 class="card-title"> {{this.beta}} </h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
           <div class="card card-stats card-background">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">store</i>
              </div>
              <p class="card-category">CEO</p>
              <h4 class="card-title"> {{this.ceo}} </h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
           <div class="card card-stats card-background">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">info</i>
              </div>
              <p class="card-category">Compañía</p>
              <h4 class="card-title"> {{this.companyName}} </h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">storage</i>
              </div>
              <p class="card-category">Sector</p>
              <h4 class="card-title"> {{this.sector}} </h4>
            </div>
          </div>
        </div>
      </div>  

      
      <!-- OTRA INFORMACION -->
      <div class="row">
        <div class="col-md-12">
          <md-list>
            <md-list-item class="col-md-5">
              <md-icon>timeline</md-icon>
              <span class="left">Price {{this.price}}</span>
              <span class="right">P/B {{this.priceBookValueRatio}}</span>
            </md-list-item>
            
            <md-list-item class="col-md-5">
              <md-icon>attach_money</md-icon>
              <span class="left">ROA {{this.returnOnAssets}}%</span>
              <span class="right">P/S {{this.priceToSalesRatio}}</span>
            </md-list-item>

            <md-list-item class="col-md-5">
              <md-icon>attach_money</md-icon>
              <span class="left">ROE {{this.returnOnEquity}}%</span>
              <span class="right">P/E {{this.priceEarningsRatio}}</span>
            </md-list-item>

            <md-list-item class="col-md-5">
              <md-icon>bar_chart</md-icon>
              <span class="">Gross Profit Mar {{this.grossProfitMargin}}%</span>
            </md-list-item>
          </md-list>
        </div>
      </div>

    <div class="row mt-5">
      <div class="col-md-4">
        <div class="card card-profile border-0">
          <div class="card-avatar">
            <img class="img" :src="image" alt="">
          </div>
          <div class="card-body">
            <h4 class="card-title no-outline">{{this.companyName}}</h4>
            <p class="card-description">{{this.description}}</p>
          </div>
        </div>
      </div>
      <div class="col-md-8">
        <div id='CandleStick'></div>
      </div>
    </div>

  <div class="row">
    <div class="col-md-4">
      <div id="Assets">

      </div>
    </div>

    <div class="col-md-4">
      <div id="Liabilities">

      </div>
    </div>

     <div class="col-md-4">
      <div id="Total">

      </div>
    </div>
  </div>

<div class="row">
  <div class="col-md-12 mt-3 align-center">
  <h4> Income Statement</h4>
  </div>
</div>
<div class="row">
  <div class="card-body table-responsive">
    <table class="table table-hover">
      <thead class="headingcustomtab"> 
        <th>In Millions</th>
        <th>{{ISdate[0]}}</th>
        <th>{{ISdate[1]}}</th>
        <th>{{ISdate[2]}}</th>
      </thead>
      <tbody>
        <tr>
          <td>Revenue</td>
          <td>$ {{revenue[0]}}</td>
          <td>$ {{revenue[1]}}</td>
          <td>$ {{revenue[2]}}</td>
        </tr>
        <tr>
          <td>COGS</td>
          <td>$ {{cogs[0]}}</td>
          <td>$ {{cogs[1]}}</td>
          <td>$ {{cogs[2]}}</td>
        </tr>
        <tr>
          <td>OpInc</td>
          <td>$ {{Opinc[0]}}</td>
          <td>$ {{Opinc[1]}}</td>
          <td>$ {{Opinc[2]}}</td>
        </tr>
        <tr>
          <td>Net Income</td>
          <td>$ {{Netinc[0]}}</td>
          <td>$ {{Netinc[1]}}</td>
          <td>$ {{Netinc[2]}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Plotly from 'plotly.js-dist'
export default {
  name: 'Stockinfo',
  props: {
    msg: String
  },

  data() {
    return {
      stock : '',
      beta : '',
      infoCompany: '',
      companyName: '',
      sector:'',
      ceo:'',
      price:'',
      marketCap:'',
      image:'',
      returnOnAssets:'',
      returnOnEquity:'',
      grossProfitMargin:'',
      priceBookValueRatio:'',
      priceToSalesRatio:'',
      priceEarningsRatio:'',
      description:'',
      open:[],
      close:[],
      high:[],
      low:[],
      x:[],
      opentemp:'',
      xi:'',
      trace1:{},
      cash:'',
      receivables:'',
      inventory:'',
      propertyPlantEquipmentNet:'',
      godwill:'',
      ltinvestment:'',
      payables:'',
      shortTermDebt:'',
      longTermDebt:'',
      defRevenue:'',
      deflia:'',
      revenue : [],
      GrossProfit : [],
      Opinc : [],
      Netinc : [],
      ISdate : [],
      cogs : []


    }
  },

  methods:{
    update(stock) {
      this.getInfo(stock)
      this.getValue(stock)
      this.getChart(stock)
      this.getChartPie(stock)
      this.getTable(stock)
    },
    getTable(stock){
      this.revenue = [],
      this.cogs=[],
      this.GrossProfit = [],
      this.Opinc = [],
      this.Netinc = [],
      this.ISdate = [],

      axios.get('https://financialmodelingprep.com/api/v3/income-statement/'+ stock +'?period=quarter&limit=400&apikey=e73c056ec41ee9a200a1948d18e62e9d')
      .then(response => {
        this.IStemp = response.data
        this.IStemp = this.IStemp.slice(0,4)
        for (this.xi of this.IStemp){
          this.revenue.push(this.xi.revenue/1000000)
          this.cogs.push(this.xi.costOfRevenue/1000000)
          this.GrossProfit.push(this.xi.grossProfit/1000000)
          this.Opinc.push(this.xi.operatingIncome/1000000)
          this.Netinc.push(this.xi.netIncome/1000000)
          this.ISdate.push(this.xi.date)
        }
      })
      .catch(error => console.log(error))
    },
    getChartPie(stock){
      axios.get('https://financialmodelingprep.com/api/v3/balance-sheet-statement/'+ stock +'?period=quarter&limit=400&apikey=e73c056ec41ee9a200a1948d18e62e9d')
      .then(response => {
        this.cash = response.data[0].cashAndCashEquivalents
        this.receivables = response.data[0].netReceivables
        this.inventory = response.data[0].inventory
        this.propertyPlantEquipmentNet = response.data[0].propertyPlantEquipmentNet
        this.godwill = response.data[0].goodwillAndIntangibleAssets
        this.ltinvestment = response.data[0].longTermInvestments
        this.payables = response.data[0].accountPayables
        this.shortTermDebt = response.data[0].shortTermDebt
        this.longTermDebt = response.data[0].longTermDebt
        this.defRevenue = response.data[0].deferredRevenue
        this.deflia = response.data[0].deferredTaxLiabilitiesNonCurrent
      })
      .catch(error => console.log(error))

      var data = [{
        values: [this.cash, this.receivables, this.inventory, this.propertyPlantEquipmentNet,
         this.godwill, this.ltinvestment],
        labels: ['Cash', 'Receivables', 'Inventory', 'PPE',
        'Godwill Assets', 'LT Investments'],
        type: 'pie',
        textinfo:"label+percent",
        textposition:"outside",
        title:"Assets",
        hole:.4,
        automargin:true
      }];

      var layout = {
        height: 500,
        width: 400,
        showlegend: false
    }

var dataliab = [{
        values: [this.payables, this.shortTermDebt, this.longTermDebt, this.defRevenue,
         this.deflia],
        labels: ['Payables', 'ST Debt', 'LT Debt', 'deferred Revenue',
        'TaxLiab'],
        type: 'pie',
        textinfo:"label+percent",
        textposition:"outside",
        title:"Liabilities",
        hole:.4,
        automargin:true
      }];

var total = [{
        values: [this.cash, this.receivables, this.inventory, this.propertyPlantEquipmentNet,
         this.godwill, this.ltinvestment, this.payables, this.shortTermDebt, this.longTermDebt, this.defRevenue,
         this.deflia],
        labels: ['Cash', 'Receivables', 'Inventory', 'PPE',
        'Godwill Assets', 'LT Investments', 'Payables', 'ST Debt', 'LT Debt', 'deferred Revenue',
        'TaxLiab'],
        type: 'pie',
        textinfo:"label",
        textposition:"outside",
        title:"Total",
        hole:.4,
        automargin:true
      }];

Plotly.newPlot('Assets', data, layout);
Plotly.newPlot('Liabilities', dataliab, layout);
Plotly.newPlot('Total', total, layout);
    }
    ,
    getChart(stock){
      axios.get('https://financialmodelingprep.com/api/v3/historical-price-full/'+ stock +'?apikey=e73c056ec41ee9a200a1948d18e62e9d')
      .then(response => {
        this.open = []
        this.close = []
        this.high = []
        this.low = []
        this.x = []
        this.xi = ''
        this.opentemp = response.data.historical
        for (this.xi of this.opentemp){
          this.open.push(this.xi.open)
          this.close.push(this.xi.close)
          this.high.push(this.xi.high)
          this.low.push(this.xi.low)
          this.x.push(this.xi.date)
        }

      })
      .catch(error => console.log(error))
     

     var trace1 = {
  
      x: this.x,
      close: this.close,
      decreasing: {line: {color: '#dc3545'}}, 

      high: this.high,

      increasing: {line: {color: '#28a745'}}, 
      
      line: {color: 'rgba(31,119,180,1)'}, 
      
      low: this.low,
      open: this.open,
      type: 'candlestick', 
      xaxis: 'x', 
      yaxis: 'y'
    };

    var data = [trace1];

    var layout = {
      dragmode: 'zoom', 
      margin: {
        r: 10, 
        t: 20, 
        b: 20, 
        l: 30
      }, 
      showlegend: false, 
      xaxis: {
        autorange: true, 
        rangeslider: {range: [this.x[0], this.x[1000]]}, 
        title: 'Date', 
        type: 'date'
      }, 
      yaxis: {
        autorange: true, 
        range: [Math.min(...this.close)-10, Math.max(...this.close)+20],
        type: 'linear'
      }
  };
  Plotly.newPlot('CandleStick', data, layout);


    },
    
    getValue(stock){
      axios.get('https://financialmodelingprep.com/api/v3/ratios/' + stock + '?limit=40&apikey=e73c056ec41ee9a200a1948d18e62e9d')
      .then(response => {
        this.returnOnAssets = parseFloat(response.data[0].returnOnAssets *100).toFixed(2)
        this.returnOnEquity = parseFloat(response.data[0].returnOnEquity *100).toFixed(2)
        this.grossProfitMargin = parseFloat(response.data[0].grossProfitMargin *100).toFixed(2)
        this.priceBookValueRatio = parseFloat(response.data[0].priceBookValueRatio).toFixed(2)
        this.priceToSalesRatio = parseFloat(response.data[0].priceToSalesRatio).toFixed(2)
        this.priceEarningsRatio = parseFloat(response.data[0].priceEarningsRatio).toFixed(2)
        })
        .catch(error => console.log(error))
    },
    
    getInfo(stock) {
       axios.get('https://financialmodelingprep.com/api/v3/profile/' + stock + '?apikey=e73c056ec41ee9a200a1948d18e62e9d')
      .then(response => {
        this.infoCompany = response.data[0].address
        this.beta = parseFloat(response.data[0].beta).toFixed(2)
        this.companyName = response.data[0].companyName
        this.sector = response.data[0].sector
        this.ceo = response.data[0].ceo
        this.price = response.data[0].price
        this.marketCap = response.data[0].mktCap
        this.image = response.data[0].image
        this.description = response.data[0].description
        })
        .catch(error => log(error))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.card-background {
  background-color: rgba(21, 1, 26, 0.774);
  color:white;
}

.md-list {
  max-width: 100%;
  display: inline-block;
  vertical-align: top;
  border:1px solid rgba(#000,.12)
}

.left{
  margin-right:7px;
  margin-top:0px;
}

.right{
  margin-left:0px;
  margin-top:px;
}

.headingcustomtab{
  color: rgba(42, 11, 51, 0.801)

}


</style>
