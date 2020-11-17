<template>
    <div class="cinema_body">
        <ul>
            <li v-for="item in cinemalist" :key="item.cinemaId">
                <div>
                    <span>{{item.name}}</span>
                    <span class="q"><span class="price">{{item.lowPrice / 100}}</span>元起</span>
                </div>
                <div class="address">
                    <span>{{item.address}}</span>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'CiList',
    data() {
        return {
            cinemalist: []
        }
    },
    mounted(){
        this.axios({
            url: 'https://m.maizuo.com/gateway?cityId=330300&ticketFlag=1&k=4535029',
            headers: {
                'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"16047527771204983139663875","bc":"330300"}',
                'X-Host': 'mall.film-ticket.cinema.list'
            }
        }).then(res=>{
            var msg = res.data.msg;
            if(msg === 'ok'){
                this.cinemalist = res.data.data.cinemas
            }
        })
    }
}
</script>

<style>
    #content .cinema_body{ flex:1; overflow:auto;}
    .cinema_body ul{ padding:20px;}
    .cinema_body li{  border-bottom:1px solid #e6e6e6; margin-bottom: 20px;}
    .cinema_body div{ margin-bottom: 10px;}
    .cinema_body .q{ font-size: 11px; color:#f03d37;}
    .cinema_body .price{ font-size: 18px;}
    .cinema_body .address{ font-size: 13px; color:#666;}
    .cinema_body .address span:nth-of-type(2){ float:right; }
</style>