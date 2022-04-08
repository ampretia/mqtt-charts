<template>
  <div class="home">
    <h1 class="title">Light Sensor Display</h1>
    <div class="section">
 
        <div class="column">
          <b-progress
            size="is-large"
            :type="uvilabel"
            :value="uvi"
            show-value
            max="11"
          >
            UV Index {{ uvi }}
          </b-progress>
        </div>
             <div class="columns is-mobile">
        <div class="column">
          <div id="chart">
            <apexchart
              type="radialBar"
              height="250"
              :options="clrchartOptions"
              :series="clrpc"
            ></apexchart>
          </div>
        </div>
        <div class="column">
          <div id="chart">
            <apexchart
              type="radialBar"
              height="250"
              :options="nirchartOptions"
              :series="nirpc"
            ></apexchart>
          </div>
        </div>        
      </div>
    </div>
    <div class="section">
      <div id="chart">
        <apexchart
          type="bar"
          height="380"
          :options="chartOptions"
          :series="series"
        ></apexchart>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//const labels=["415mm","445nm","480nm","515nm","555nm","590nm","638nm","680nm"]
import "vue-json-pretty/lib/styles.css";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      // series: [
      //   {
      //     data: [400, 430, 448, 470, 540, 580, 690, 1100],
      //   },
      // ],
      nirchartOptions: {
        chart: {
          height: 150,
          type: "radialBar"
        },
        plotOptions: {
          radialBar: {
            hollow: {
              size: "50%"
            },
            dataLabels: {
              value: {
                formatter: function(val) {
                  return val;
                }
              }
            }
          }
        },
        fill: {},
        labels: ["Near-IR Count"]
      },
      clrchartOptions: {
        chart: {
          height: 150,
          type: "radialBar"
        },
        plotOptions: {
          radialBar: {
            hollow: {
              size: "50%"
            },
            dataLabels: {
              value: {
                formatter: function(val) {
                  return val*20;
                }
              }
            }
          }
        },
        labels: ["Clear Light Count"]
      },
      uvichartOptions: {
        chart: {
          height: 150,
          type: "radialBar"
        },
        plotOptions: {
          radialBar: {
            hollow: {
              size: "50%"
            },
            dataLabels: {
              value: {
                formatter: function(val) {
                  return val*100;
                }
              }
            }
          }
        },
        labels: ["UVIndex"]
      },
      chartOptions: {
        chart: {
          type: "bar",
          height: 380
        },
        plotOptions: {
          bar: {
            barHeight: "100%",
            distributed: true,
            horizontal: false,
            dataLabels: {
              position: "bottom"
            }
          }
        },
        colors: [
          "#8f00ff",
          "#4b0082",
          "#0000ff",
          "#00ffff",
          "#00ff00",
          "#ffff00",
          "#ffa500",
          "#ff0000"
        ],
        legend: {
          show: false
        },
        stroke: {
          width: 1,
          colors: ["#fff"]
        },
        xaxis: {
          categories: [
            "415mm",
            "445nm",
            "480nm",
            "515nm",
            "555nm",
            "590nm",
            "638nm",
            "680nm"
          ]
        },
        yaxis: {
          labels: {
            show: false
          }
        },
        title: {
          text: "Wavelength counts",
          align: "center",
          floating: true
        },

        tooltip: {
          theme: "dark",
          x: {
            show: false
          },
          y: {
            title: {
              formatter: function() {
                return "";
              }
            }
          }
        }
      }
    };
  },
  computed: {
    uvilabel: function() {
      switch (this.$store.state.uvi) {
        case 0:
        case 1:
        case 2:
          return "is-success";
        case 3:
        case 4:
        case 5:
        case 6:
          return "is-warning";
        case 7:
        case 8:
        case 9:
          return "is-danger";
        case 10:
        case 11:
          return "is-primary";
        default:
          return "is-info";
      }
    },
    uvi: function() {
      return this.$store.state.uvi;
    },
    nir: function() {
      return [this.$store.state.nir];
    },
    nirpc: function() {
      return [this.$store.state.nir / 10];
    },
    clr: function() {
      return [this.$store.state.clr];
    },
    clrpc: function() {
      return [this.$store.state.clr / 20];
    },
    series: function() {
      //  ? console.log(this.$store.state.wlcount);
      // return  [{data: [400, 430, 448, 470, 540, 580, 690, 1100, 1200, 1380]}];
      return this.$store.getters.wlcount;
    }
  },
  methods: {}
};
</script>
<style scoped>
.inner-text {
  margin-top: 70px;
}
</style>
