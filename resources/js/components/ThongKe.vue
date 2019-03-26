<template>
<div>
    <div class="row" v-if="$gate.isGiamDoc()">
        <div class="col-md-12">
          <div class="tile">
            <h3 class="tile-title" style="text-align:center;">THỐNG KÊ DOANH THU</h3>
            <!--div class="embed-responsive embed-responsive-16by9"-->
            <div style="margin-left: 40%;font-size: 20px;" class="form-group">
                 <label for="text">Chọn năm cần lọc: </label>
                <select name="selYear" id="selYear" class="form-" @change="layTheoNam">
                    <option :value="year.year" v-for="(year, index) in years" :key="index">{{ year.year }}</option>
                </select>
            </div>
            <div>
              <line-chart :chart-data="urlData" :height="100" currency="VNĐ"></line-chart>
            </div>
          </div>
        </div>
    </div>

    <div class="mb-5" v-else>
        <not-found></not-found>
    </div>
</div>
</template>
<script>
import LineChart from '../charts/lineChart.js'
export default {
    components: {
        LineChart
    },
    data() {
        return {
            urlData: {},
            years: {}
        }
    },
    methods: {
        layThongKe(){
            if(this.$gate.isGiamDoc()){
                axios.get('/api/thongke_laynam').then((response) => {
                    this.years = response.data
                });
                axios.get('/api/ThongKe').then((response) => {
                    this.urlData = response.data
                });
            }
        },
        layTheoNam(){
            axios.get('/api/thongke_laytheonam?y=' + event.target.value).then((response) => {
                this.urlData = response.data
            });
        }
    },
    mounted() {
        this.layThongKe()
    },
}
</script>