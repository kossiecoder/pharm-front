<template>
    <b-container>
        <div class="d-flex my-3">
            <h1 class="flex-grow-1">Member Statistics</h1>
            <b-button
                v-if="currentYear"
                variant="primary"
                @click="getStat(null)"
            >By Year</b-button>
        </div>


        <BarChart
            v-if="!loading"
            :chart-data="chartdata"
            :options="options"
        />
        <LoadingSpinner v-else />
    </b-container>
</template>

<script>
    import BarChart from "@/BarChart";
    import axios from 'axios';
    import LoadingSpinner from "./LoadingSpinner";

    export default {
        components: {
            BarChart,
            LoadingSpinner,
        },

        data() {
            return {
                currentYear: null,
                loading: true,
                chartdata: null,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    'onClick': (e, item) => {
                        if (this.currentYear === null) {
                            this.currentYear = item[0]._view.label;
                            this.getStat(item[0]._view.label);
                        } else {
                            this.currentYear = null;
                            this.getStat();
                        }
                    },
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true
                            }
                        }]
                    }
                },
            }
        },

        async created() {
            await this.getStat();
            this.loading = false;
        },

        methods: {
            updateChartData(labels, data) {
                this.chartdata = {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Number of Sign-up Members' + (this.currentYear ? ` by Month in ${this.currentYear}`: ' by Year'),
                            backgroundColor: '#f87979',
                            data: data
                        }
                    ]
                };
            },

            async getStat(year = null) {
                try {
                    const res = await axios.get('http://localhost:8000/api/members-stat', {
                        params: {
                            year
                        }
                    });

                    const result = res.data.result;
                    const labels = Object.keys(result).sort();
                    const data = [];

                    labels.forEach(function(v) {
                        data.push(result[v]);
                    });

                    if (year === null) {
                        this.currentYear = null;
                    }

                    this.updateChartData(labels, data);

                } catch (e) {
                    console.log(e)
                }
            }
        }
    }
</script>
