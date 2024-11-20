<template>
    <div class="report-data mt-4 card-types">
        <div class="header">
            <div class="row">
                <div class="col-6">
                    <h3 class="title">Total de clientes</h3>
                </div>
                <div class="col-lg-6">
                    <div class="interval">
                        <label>{{ intervalSelected === "oneMonth" ? "No último" : "Nos últimos" }}</label>
                        <select class="form-select float-end text-end" v-model="intervalSelected" @change="updateChart()">
                            <option v-for="(interval, index) in intervals" :key="index" :value="index">{{ interval }}</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
        <div class="row card-data">
            <div class="col-sm-3">
                <div class="chart">
                    <div class="label">
                        <h4 class="number">{{ generalData[intervalSelected]?.current_period_count || 0 }}</h4>
                        <p class="text">Clientes totais</p>
                        <span class="card-percent" 
                            :class="(generalData[intervalSelected]?.percentage_increase || 0) > 0 ? 'icon-var-up' : 'icon-var-down'">
                            {{ Math.abs(generalData[intervalSelected]?.percentage_increase || 0) }}%
                        </span>
                    </div>
                    <canvas id="doughnutChart" @mouseleave="resetColorChart()"></canvas>
                </div>
            </div>
            <div class="col-sm-9 d-flex align-items-center">
                <div class="row">
                    <div class="col d-flex list-cards" v-for="(data, index) in dataCustomers" :key="index">
                        <div class="card" :id="'card-'+index" @mouseover="setColorChart(index)" @mouseleave="resetColorChart()">
                            <h3 class="title">
                                <span class="legend" :style="{ backgroundColor: baseColors[index] }"></span>
                                {{ typeLabels[index].name }}
                            </h3>
                            <p class="description">{{ typeLabels[index].description }}</p>
                            <h4 class="number">
                                {{ data?.stats[intervalSelected]?.current_period_count }}
                                <span class="card-percent" :class="(data?.stats[intervalSelected]?.percentage_increase || 0) > 0 ? 'icon-var-up' : 'icon-var-down'">
                                    {{ Math.abs(data?.stats[intervalSelected]?.percentage_increase) }}%
                                </span>
                            </h4>
                            <hr>
                            <p v-if="data?.stats[intervalSelected]?.current_period_count " class="other-data">
                                {{ Math.round(data?.stats[intervalSelected]?.current_period_count / countCustomers * 100) }}
                                % do total de vendas
                            </p>
                            <p v-else class="other-data">100 % do total de vendas</p>
                            <p class="other-data">
                                R$ {{ data?.average_ticket.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 }) }} 
                                ticket médio
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import store from '@/store';
    import { shallowRef } from 'vue';
    import Chart from 'chart.js/auto';
    import ChartDataLabels from 'chartjs-plugin-datalabels'

    export default {
        name: "CardTypes",
        data() {
            return {
                intervalSelected: "7_days",
                countCustomers: 0,
                reportChart: [],
                showData: [],
                baseColors: {
                    new_customers: "rgba(153,154,242,0.6)", 
                    frequent_customers: "rgba(24,226,28,0.6)", 
                    occasional_customers: "rgba(58,170,244,0.6)"
                },
                baseColorsHover: {
                    new_customers: "rgba(153,154,242,1)", 
                    frequent_customers: "rgba(24,226,28,1)", 
                    occasional_customers: "rgba(58,170,244,1)"
                },
                typeLabels: {
                    new_customers: {
                        name: "Novos clientes",
                        description: "Clientes que pediram pela primeira vez na sua loja."
                    },
                    frequent_customers: {
                        name: "Clientes frequentes",
                        description: "Clientes que realizaram mais de 4 pedidos na sua loja."
                    },
                    occasional_customers: {
                        name: "Clientes ocasionais",
                        description: "Clientes que pediram até 4 vezes na sua loja."
                    }
                },
                reportData: [],
                generalData: [],
            }
        },
        props: {
            intervals: Object
        },
        methods: {
            countTotal(data) {
                this.countCustomers = data[this.intervalSelected].current_period_count ;
            },
            createChart() {                
                var data = {
                    labels: ["new_customers", "frequent_customers", "occasional_customers"],
                    datasets: [
                        {
                            label: "Clientes",
                            borderWidth: 0,
                            borderSkipped: false,
                            backgroundColor: Object.values(this.baseColors),
                            hoverBackgroundColor: Object.values(this.baseColorsHover),
                            data: [ 
                                this.reportData?.new_customers?.stats[this.intervalSelected]?.current_period_count | 0,
                                this.reportData?.frequent_customers?.stats[this.intervalSelected]?.current_period_count | 0,
                                this.reportData?.occasional_customers?.stats[this.intervalSelected]?.current_period_count | 0,
                            ]
                        }
                    ]
                };

                this.showData = shallowRef(data);
                
                var options = { 
                    maintainAspectRatio: false,
                    cutout: '70',
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            enabled: false,
                            usePointStyle: false
                        },
                        datalabels: {
                            display: false
                        }
                    },
                    onHover: (event, elements) => {
                        if (elements.length) {
                            const index = elements[0].index;
                            const items = Object.keys(this.baseColors);
                            this.reportChart.data.datasets[0].backgroundColor = ["rgba(241, 244, 244,1)", "rgba(241, 244, 244,1)", "rgba(241, 244, 244,1)"];
                            this.reportChart.data.datasets[0].backgroundColor[index] = this.baseColorsHover[items[index]];
                            this.reportChart.update();
                            document.querySelector('#card-new_customers').classList.remove('active-new_customers');
                            document.querySelector('#card-frequent_customers').classList.remove('active-frequent_customers');
                            document.querySelector('#card-occasional_customers').classList.remove('active-occasional_customers');
                            document.querySelector('#card-'+items[index]).classList.add('active-'+items[index]);
                        } 
                    }
                };
                
                this.reportChart = shallowRef(
                    new Chart("doughnutChart", {
                        type: "doughnut",
                        plugins: [ChartDataLabels],
                        options: options,
                        data: data
                    })
                );
            },
            updateChart() {
                const data = [ 
                    this.reportData?.new_customers?.stats[this.intervalSelected]?.current_period_count | 0,
                    this.reportData?.frequent_customers?.stats[this.intervalSelected]?.current_period_count | 0,
                    this.reportData?.occasional_customers?.stats[this.intervalSelected]?.current_period_count | 0,
                ];
                this.reportChart.data.datasets[0].data = data;
                this.reportChart.update();
            },
            resetColorChart() {
                this.reportChart.data.datasets[0].backgroundColor = Object.values(this.baseColors);
                this.reportChart.update();
                document.querySelector('#card-new_customers').classList.remove('active-new_customers');
                document.querySelector('#card-frequent_customers').classList.remove('active-frequent_customers');
                document.querySelector('#card-occasional_customers').classList.remove('active-occasional_customers');
            },
            setColorChart(type) {
                this.reportChart.data.datasets[0].backgroundColor = ["rgba(241, 244, 244,1)", "rgba(241, 244, 244,1)", "rgba(241, 244, 244,1)"];
                let index = Object.keys(this.baseColors).indexOf(type);
                this.reportChart.data.datasets[0].backgroundColor[index] = this.baseColorsHover[type];
                this.reportChart.update();
                document.querySelector('#card-new_customers').classList.remove('active-new_customers');
                document.querySelector('#card-frequent_customers').classList.remove('active-frequent_customers');
                document.querySelector('#card-occasional_customers').classList.remove('active-occasional_customers');
                document.querySelector('#card-'+type).classList.add('active-'+type);
            }
        },
        async mounted() {            

            this.selectCategory = this.category;
            const userResponse = await axios.get('https://api.prattuapp.com.br/api/users/me', {
                headers: {
                    'Authorization': `Bearer ${store.state.token}`
                }
            });
            const restaurantId = userResponse.data.restaurant_id;
            const customerAnalysisResponse = await axios.get(`https://api.prattuapp.com.br/api/customer-analysis/${restaurantId}`, {
                headers: {
                    'Authorization': `Bearer ${store.state.token}`
                }
            });
            this.reportData = customerAnalysisResponse.data;
            this.dataCustomers = Object.keys(this.reportData).reduce((acc, key) => {
                if (key !== 'total_customers') {
                    acc[key] = this.reportData[key];
                }
                return acc;
            }, {});

            this.generalData = this.reportData?.total_customers?.stats;
            this.countTotal(this.reportData?.total_customers?.stats);
            this.createChart();
        }
    };
</script>

<style lang="scss" scoped>
    .interval {
        width: 235px;
        float: right;
        position: relative;
        border-bottom: 1px solid $gray-line !important;
        padding-bottom: 2px;
        margin-bottom: 8px;
        height: 40px !important;
        label {
            position: absolute;
            margin-top: 7px;
            font-size: 16px;
            font-weight: 500;
            text-align: left;
        }
        select {
            background-color: transparent;
            border: none;
        }
    }

    .card-types {
        margin-top: 35px !important;
        .header {
            border-bottom: 1px solid $gray-line;
            margin-bottom: 20px;
            padding-bottom: 5px;
        }
        h3.title {
            font-size: 16px;
            font-weight: 500;
            margin-top: 10px;
        }
        .chart {
            height: 280px;
            border-right: 1px solid $gray-line;
            padding-right: 20px;
        }
    }

    .chart {
        position: relative;
        .label {
            position: absolute;
            text-align: center;
            top: calc(50% - 40px);
            left: calc(50% - 52px);
            .number {
                font-size: 28px;
                font-weight: 700;
                margin: 0;
                padding: 0;
                line-height: 1;
            }
            .text {
                font-size: 12px;
                font-weight: 400;
                margin: 0;
                padding: 0;
                line-height: 1;
                margin-bottom: 7px;
            }
        }
    }

    .active-new_customers {
        background-color: rgba(198, 199, 248,0.6) !important;
    }
    .active-frequent_customers {
        background-color: rgba(24,226,28,0.3) !important;
    }
    .active-occasional_customers {
        background-color: rgba(130, 201, 248,0.4) !important;
    }

    .card-data {
        margin-top: 40px !important;
        margin-bottom: 20px !important;
    }

    .card {
        border: none;
        border-radius: 16px;
        padding: 20px 25px;
        background-color: #F1F4F4; //COLOCAR A COR CCERTA
        .title {
            font-size: 16px !important;
            font-weight: 500 !important;
            margin-bottom: 15px;
            .legend {
                width: 9px;
                height: 9px;
                display: inline-block;
                border-radius: 50%;
                margin-right: 5px;
                transform: translateY(-1px);
            }
        }
        .description {
            font-size: 14px !important;
            font-weight: 400;
            line-height: 1.5
        }
        .number {
            font-size: 28px !important;
            font-weight: 700;
            .card-percent {
                transform: translateY(-8px);
                margin-left: 3px;
            }
        }
        hr {
            border: none;
            border-bottom: 1px solid #DCDCDC; //COLOCAR A COR CCERTA
            box-shadow: none;
            opacity: 1;
            margin: 5px 0 10px 0;
        }
        .other-data {
            margin: 0;
            font-size: 14px;
            font-weight: 400;
            line-height: 1.5
        }
    }
    .description {
  
    font-weight: 400;
    line-height: 1.5;
    min-height: 50px; // Define a altura mínima para o campo description
}

</style>