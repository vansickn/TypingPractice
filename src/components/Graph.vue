<template>
        <div class="container flex flex-col min-w-full">
            <div class="container flex flex-row min-w-full justify-center">
                <!-- <LineChart class="w-10/12" v-bind="barChartProps" /> -->
                <LineChart v-if="showGraph" class="sm:w-8/12 h-64" :chart-data="chartData" :options="options"/>
            </div>
        </div>

</template>

<script>
import { BarChart, useBarChart } from "vue-chart-3";
import { LineChart, useLineChart } from "vue-chart-3";
import { ref, computed, defineComponent } from "vue";

// export default{
//     data(){

//     }
// }

export default{
    components: {
        BarChart,
        LineChart,
      },
    props: ['word_list','wpm_array','correctness_array','generate'], 
    data(){
        return {
            chartData: {
               labels: ["Paris", "Nîmes", "Toulon", "Perpignan", "Autre", "Autre", "Autre", "Autre"],
                datasets: [
                    {
                    data: [30, 40, 60, 70],
                    backgroundColor: [
                        "#22c55e",
                        "#22c55e",
                        "#22c55e",
                        "#22c55e",
                        "#22c55e",
                        
                    ],
                    },
                ], 
            },
            options: {
                scales: {
                    y: {
                    beginAtZero: true,
                    },
                },
                plugins: {
                    legend: {
                        display: false,
                    }
                },
                padding: 10,
                legendTop: false,
                font: {
                    family: 'Avenir',
                }
            },
            showGraph: false,
        }
    },
    methods: {
        generateChartData(words,wpm){
            this.chartData = {
                labels: words,
                datasets: [
                    {
                        data: wpm,
                        backgroundColor: this.generateBackgroundColors(this.correctness_array),
                        borderColor: '#6b7280',
                        borderWidth: 1,
                        tension: 0.3,
                    }
                ],
            }
        },
        generateBackgroundColors(array){
            var colors = [];
            for (let i = 0; i < array.length; i++) {
                if(array[i] == true){
                    colors.push("#22c55e");
                }else{
                    colors.push("#ef4444")
                }
            }
            return colors;
        }
    },
    watch: {
        generate: function(newv){
            if(newv && this.wpm_array != null){

                var wpm_copy = [...this.wpm_array]

                this.generateChartData(this.word_list,wpm_copy);
                this.showGraph = true;
            }
        }
    }

}

// export default defineComponent({
//   name: "App",
//   components: {
//     BarChart,
//     LineChart,
//   },
//   props: ['word_list'],
//   setup() {
//     const data = ref([30, 40, 60, 70]);
//     const legendTop = ref(true);
//     const imgData = ref(null);

//     const options = computed(() => ({
//       scales: {
//         y: {
//           beginAtZero: false,
//         },
//       },
//       plugins: {
//           legend: {
//               display: false,
//           }
//       },
//       padding: 10,
//       legendTop: false,
//     }));

//     const chartData = computed(() => ({
//       labels: ["Paris", "Nîmes", "Toulon", "Perpignan", "Autre", "Autre", "Autre", "Autre"],
//       datasets: [
//         {
//           data: data.value,
//           backgroundColor: [
//             "#22c55e",
//             "#22c55e",
//             "#22c55e",
//             "#22c55e",
//             "#22c55e",
            
//           ],
//         },
//       ],
//     }));

//     const { barChartProps, barChartRef } = useBarChart({
//       chartData,
//       options,
//     });

//     return {barChartProps, barChartRef };
//   },
// });
</script>

<style>

</style>
