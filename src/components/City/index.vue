<template>
    <div>
        <mt-index-list ref="mylist">
            <div class="city_hot">
			<h2>热门城市</h2>
				<ul class="clearfix">
					<li>上海</li>
					<li>北京</li>
					<li>广州</li>
					<li>深圳</li>
				</ul>
		</div>
            <mt-index-section :index="data.index" v-for="data in datalist" :key="data.index">
                <div v-for="city in data.list" :key="city.cityId" @click="handleClick(city.cityId)">
                    <mt-cell :title="city.name"></mt-cell>
                </div>
            </mt-index-section>
        </mt-index-list>
    </div>
</template>

<script>
export default {
    name: 'City',
    data() {
        return {
            datalist: []
        }
    },
    mounted(){
        this.$refs.mylist.$el.style.height = document.documentElement.clientHeight - 150 + 'px';
        this.axios({
            url: 'https://m.maizuo.com/gateway?k=750395',
            headers: {
                'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"16047527771204983139663875","bc":"330300"}',
                'X-Host': 'mall.film-ticket.city.list'
            }
        }).then(res=>{
            var msg = res.data.msg;
            if(msg === 'ok'){
                 this.datalist = this.handleCity(res.data.data.cities)
            }
        })
    },
    methods:{
        handleCity(datalist){
            var letterArr = [];
            // 遍历一遍A-Z的数组，fromCharCode() = 将 Unicode 编码转为指定的字符
            for(var i=65;i<91;i++){
                letterArr.push(String.fromCharCode(i));
            }
            var newlist = [];
            for(var j=0;j<letterArr.length;j++){
                var arr = datalist.filter(item=>item.pinyin.substring(0,1) === letterArr[j].toLowerCase())
                if(arr.length>0){
                    newlist.push({
                        index:letterArr[j],
                        list:arr
                    })
                }
            }
            return newlist
        },
        handleClick(id){
            localStorage.setItem('cityId',id)
        }
    }
}
</script>

<style scoped>
    .city_hot{ margin-top: 20px;}
    .city_hot h2{ padding-left: 15px; line-height: 30px; font-size: 14px; color:gray; font-weight: normal;}
    .city_hot ul{margin-bottom:20px;}
    .city_hot ul li{ float: left; background: #f0f0f0; width: 29%; height: 33px; margin-top: 15px; margin-left: 3%; padding: 0 4px; border: 1px solid #f0f0f0; border-radius: 3px; line-height: 33px; text-align: center; box-sizing: border-box;}
</style>