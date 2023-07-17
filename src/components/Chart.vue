<template>
    <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />
</template>
  
<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
    name: 'BarChart',
    components: { Bar },
    props: ['search'],
    data(props) {
        console.log(props.search);
        return {
            chartData: {
                labels: ['January', 'February', 'March'],
                datasets: [{ data: [40, 20, 12] }]
            },
            chartOptions: {
                responsive: true
            },
            listItems: [],
        }
    },
    methods: {
        async getData(search) {
            const res = await fetch(`https://api.openweathermap.org/data/2.5/forecast?q=${search}&appid=${process.env.VUE_APP_API}&units=metric`);
            const finalRes = await res.json();
            if (finalRes.cod == '200') {
                console.log(finalRes);
                this.listItems.push(finalRes.list);
            }
        }
    },
    beforeMount(props) {
        console.log(props);
        this.getData(props.search)
    },
}
</script>
  