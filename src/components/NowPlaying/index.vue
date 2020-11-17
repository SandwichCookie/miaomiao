<template>
    <div class="movie_body">
        <ul>
            <li v-for="item in movielist" :key="item.filmId">
                <div class="pic_show"><img :src="item.poster" /></div>
                <div class="info_list">
                    <span class="name">{{item.name}}</span>
                    <span class="item">{{item.item.name}}</span>
                    <p v-if="item.grade">观众评分 <span class="grade">{{item.grade}}</span></p>
                    <p v-else>暂无评分</p>
                    <p v-if="item.actors">主演:{{item.actors | actorfilter}}</p>
                    <p v-else>暂无主演</p>
                    <p>{{item.nation}} | {{item.runtime}}分钟</p>
                </div>
                <div class="btn_mall">
                    购票
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
import Vue from 'vue';

Vue.filter("actorfilter",(data)=>{
    var newlist = data.map(item => item.name)
    return newlist.join(" ")
})

export default {
    name: 'NowPlaying',
    data() {
        return {
            movielist: []
        }
    },
    mounted(){
        this.axios({
            url: 'https://m.maizuo.com/gateway?cityId=440100&pageNum=1&pageSize=10&type=1&k=3080438',
            headers:{
                'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"16047527771204983139663875","bc":"440100"}',
                'X-Host': 'mall.film-ticket.film.list'
            }
        }).then(res=>{
            var msg = res.data.msg;
            if(msg === 'ok'){
                this.movielist = res.data.data.films;
            }
        })
    }
}
</script>

<style>
    #content .movie_body{ flex:1; overflow:auto;}
    .movie_body ul{ margin:0 12px; overflow: hidden;}
    .movie_body ul li{ margin-top:12px; display: flex; align-items:center; border-bottom: 1px #e6e6e6 solid; padding-bottom: 10px;}
    .movie_body .pic_show{ width:64px; height: 90px;}
    .movie_body .pic_show img{ width:100%;}
    .movie_body .info_list { margin-left: 10px; flex:1; position: relative;}
    .movie_body .info_list h2{ font-size: 17px; line-height: 24px; width:150px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;}
    .movie_body .info_list p{ font-size: 13px; color:#666; line-height: 22px; width:200px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;}
    .movie_body .info_list .grade{ font-weight: 700; color: #faaf00; font-size: 15px;}
    .movie_body .btn_mall{ width:47px; height:27px; line-height: 28px; text-align: center; background-color: #f03d37; color: #fff; border-radius: 4px; font-size: 12px; cursor: pointer;}
    .movie_body .info_list .name{color: #191a1b;font-size: 16px;height: 22px;line-height: 22px;overflow: hidden;margin-right: 5px;}
    .movie_body .info_list .item{font-size: 9px;color: #fff;background-color: #d2d6dc;height: 14px;padding: 0 2px;}
</style>