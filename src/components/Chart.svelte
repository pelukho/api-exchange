<script>
    import Line from "svelte-chartjs/src/Line.svelte"
    import { getExchangeData } from '../services/ApiService.svelte';
    import { onMount } from 'svelte';

    let dataPerWeek = {
        loaded: false,
        data: []
    }

    let dataSetDate = [],
        dataSetRate = [];

    onMount(async () => {
        getExchangeData().then(resp => {
            dataPerWeek.loaded = true;
            dataPerWeek.data = resp.reverse();

            resp.map(item => {
                dataSetDate.push(item.exchangedate)
                dataSetRate.push(item.rate)
            });
        })
    });

    let dataLine = {
        labels: [],
        datasets: [
            {
                label: '',
                fill: true,
                lineTension: 0.3,
                backgroundColor: "rgba(184, 185, 210, .3)",
                borderColor: "rgb(35, 26, 136)",
                borderCapStyle: "butt",
                borderDash: [],
                borderDashOffset: 0.0,
                borderJoinStyle: "miter",
                pointBorderColor: "rgb(35, 26, 136)",
                pointBackgroundColor: "rgb(255, 255, 255)",
                pointBorderWidth: 5,
                pointHoverRadius: 5,
                pointHoverBackgroundColor: "rgb(0, 0, 0)",
                pointHoverBorderColor: "rgba(220, 220, 220, 1)",
                pointHoverBorderWidth: 2,
                pointRadius: 1,
                pointHitRadius: 10,
                data: []
            }
        ]
    };

    let options = {
        responsive: true,
        legend: false,
        scales: {
            yAxes: [{
                display: true,
                ticks: {
                    min: 20,
                    max: 30
                }
            }]
        },
    }

    $: if (dataPerWeek.loaded) {
        dataLine.labels = dataSetDate
        dataLine.datasets[0].data = dataSetRate
    }
</script>
{#if dataPerWeek.loaded}
    <Line data={dataLine} {options}/>
{/if}