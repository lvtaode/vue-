<template>
    <div>
        <Header></Header>
        <div id="visitor"
        v-loading="loading"
        element-loading-text="拼命加载中"
        element-loading-spinner="el-icon-loading">
            <!-- <visitor-pie :pieData="pieData"></visitor-pie> -->
        </div>
    </div>
</template>
<script>
    import Header from './Header';
    import echarts from 'echarts/lib/echarts';
    // 引入柱状图
    import 'echarts/lib/chart/pie';
    import 'echarts/lib/component/title';
    import 'echarts/lib/component/legend';
     export default {
         data(){
             return{
                pieData:{},
                loading:true
             }
         },
         components:{
            Header
        },
        mounted(){
            this.$http.get('https://elm.cangdu.org/v1/user/city/count').then(res=>{
                    this.pieData = res.data.user_city;
                    this.loading=false;
                })
            this.myChart = echarts.init(document.getElementById('visitor'));
            this.initData();
        },
        methods: {
            initData(){
                console.log(this.pieData);
                const option = {
                    title : {
                        text: '用户分布',
                        subtext: '',
                        x:'center'
                    },
                    tooltip : {
                        trigger: 'item',
                        formatter: "{a} <br/>{b} : {c} ({d}%)"
                    },
                    legend: {
                        orient: 'vertical',
                        left: 'left',
                        data: ['北京','上海','深圳','杭州','其他']
                    },
                    series : [
                        {
                            name: '访问来源',
                            type: 'pie',
                            radius : '55%',
                            center: ['50%', '60%'],
                            data:[
                                {value:this.pieData.beijing, name:'北京'},
                                {value:this.pieData.shanghai, name:'上海'},
                                {value:this.pieData.shenzhen, name:'深圳'},
                                {value:this.pieData.hangzhou, name:'杭州'},
                                {value:this.pieData.qita, name:'其他'}
                            ],
                            itemStyle: {
                                emphasis: {
                                    shadowBlur: 10,
                                    shadowOffsetX: 0,
                                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                                }
                            }
                        }
                    ]
                };        
                this.myChart.setOption(option);
            }
        },
        watch: {
            pieData: function (){
                this.initData()
            }
        }
    }
</script>
<style>
    @import '../style/common.css';
    #visitor{
        width: 90%;
        height: 450px;
        margin: 30px auto 0;
    }
</style>
