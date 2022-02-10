<template>
        <div v-if="showGraph" class="container flex flex-col min-w-full items-center">
            <h1 class="text-4xl text-gray-500">WPM: {{this.wpm_total}}</h1>
            <div class="container flex flex-row min-w-full justify-center">
                <!-- <LineChart class="w-10/12" v-bind="barChartProps" /> -->
                <LineChart v-if="showGraph" class="w-10/12 max-h-64" :chart-data="chartData" :options="options"/>
            </div>
            <div class="container flex flex-row justify-center gap-2">
               <h1 class="text-lg text-gray-500">Time: {{this.time}}</h1> 
               <h1 class="text-lg text-gray-500">Accuracy: {{this.correct_words}}/{{this.total_words}}</h1> 
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
    props: ['word_list','wpm_array','correctness_array','generate','wpm_total','time','correct_words','total_words'], 
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
                        ticks: {
                            color: '#6b7280',
                            font: {
                                size: 12,
                                family: 'Avenir',
                            }
                        }
                    },
                    x: {
                        ticks: {
                            color: '#6b7280',
                            font: {
                                size: 12,
                                family: 'Avenir',
                            }
                        }
                    }
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
                },
                maintainAspectRatio: false,
                responsive: true,
                elements: {
                    point: {
                        radius: 4,
                        borderWidth: 0,
                        hoverRadius: 6,
                    }
                },
            },
            showGraph: false,
            wpm_copy: null,
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
                        borderWidth: 2,
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
            if(this.wpm_array == null){
                return
            }
            var regen = (JSON.stringify(this.wpm_array) == JSON.stringify(this.wpm_copy))
            if(newv && !regen && this.wpm_array.length == this.total_words){
                this.wpm_copy = [...this.wpm_array]
                this.generateChartData(this.word_list,this.wpm_copy);
                this.showGraph = true;
            }
        }
    },

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
