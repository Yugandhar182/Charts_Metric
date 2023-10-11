<script>
    import { onMount } from 'svelte';
    import Metric from "$lib/metric/metric.svelte";
    import { Chart, BarSeries, Category } from '@syncfusion/ej2-charts';
    import { Browser } from '@syncfusion/ej2-base';
    Chart.Inject(BarSeries, Category);
  
    let chartData = []; // Initialize with an empty array
  
    onMount(async () => {
      // Fetch data from the API
      const apiUrl = 'https://api.recruitly.io/api/dashboard/ats/data/rejectreasons?start=01%2F01%2F2023&end=11%2F10%2F2023&apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77';
  
      try {
        const response = await fetch(apiUrl);
        if (response.ok) {
          const jsonData = await response.json();
          // Transform API data into the format suitable for the chart
          chartData = jsonData.map(item => ({
            x: item.reason || 'Unknown', // Use 'Unknown' if reason is null
            y: item.count
          }));
          // Refresh the chart with the updated data
          console.log(chartData);
  
          updateChart();
        } else {
          console.error('Failed to fetch data from the API.');
        }
      } catch (error) {
        console.error('An error occurred while fetching data:', error);
      }
    });
  
    // Function to create or update the chart
    function updateChart() {
      // Create and append the chart with the updated data source
      const chart = new Chart({
        primaryXAxis: {
          valueType: 'Category',
          majorGridLines: { width: 0 }
        },
       
        primaryYAxis: {
          
          edgeLabelPlacement: 'Shift',
          majorTickLines: { width: 0 },
          minorTickLines: { width: 0 },
          lineStyle: { width: 0 },
         
        },
        width: '1090px',
        height:"350px",
       
        series: [
          {
            type: 'Bar',
            dataSource: chartData,
            xName: 'x',
            width: 2,
            yName: 'y',
            columnSpacing: 0.1,
           
          },
        ],
      });
  
      chart.appendTo('#container');
    }
  </script>
  
  <div class="main-container">
<Metric/>
   <div class="container2">
    <h1 class="h1" >Rejected Reasons</h1>
    <div id='container'></div>
  </div>
  </div>
  
  <style>
    .main-container {
    
    }
    .container2 {
     width: 1400px;
     height: 420px;
   background-color:white;
   border: 4px solid rgb(234, 234, 245);
   margin-top: 70px;
   margin-left:60px;
    }
  .h1{
  
    font-size: 20px;
   
    font-weight: bold;
  }
  
  </style>
  