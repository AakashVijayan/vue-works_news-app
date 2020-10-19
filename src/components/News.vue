<template>
    <div class = "container">
        <div class = "searchbar">
            <form action="">
                <input class="search-input" typt="text" placeholder="Search here..."/> 
            </form>
            <div class = "search-icon">
                <i class="fas fa-search" ></i>
                <i class="fas fa-times" ></i>
            </div>
        </div>
        <div class="result-list">
            <article class ="article-class" v-for="(article,index) in articles" :key="index">
                <header>
                    <img class = ".article-header-img" v-if="article.urlToImage" :src="article.urlToImage" alt="">
                    <i v-else class = "fas fa-image article-header-i"></i>
                </header>
                <section class ="article-section" v-html="article.title"></section>
                <footer class ="article-footer">
                    <i class="fas fa-chevron-right" ></i>
                </footer>
            </article>
        </div>
    </div>
</template>


<script>
export default {
    props:['api_key'],
    data(){
        console.log("inside data");
        return{
            apiUrl:'',
            isBusy:false,
            showloader:false,
            currentPage:1,
            totalResults:0,
            maxPerPage:20,
            searchString:'',
            articles:[],
            country: 'us'
        }
    },
    methods:{
        fetchTopNews(){
            console.log(this.api_key);
            this.apiUrl ='https://newsapi.org/v2/top-headlines?country='+this.country+'&apiKey='
                    +this.api_key+'&pageSize='+this.maxPerPage;
            this.isBusy=true;
            this.searchString='';
            this.resetData();
            this.fetchData();
        },
        resetData(){
            this.currentPage=1,
            this.articles=[]
        },
        fetchData(){
          console.log(this.apiKey);
            let reqst = new Request(this.apiUrl+'&page='+this.currentPage);
            fetch(reqst).then((resp)=>resp.json())
            .then((data)=>{
                this.totalResults= data.totalResults;
                console.log(data);
                data.articles.forEach(element =>{
                    this.articles.push(element);
                });
                this.isBusy=false;
                this.showloader = false;
            }).catch((error)=>{
                console.log(error);
            })
        },
    },
        created(){
            this.fetchTopNews();
            console.log("inside created");
        },
}
</script>

<style>

.container{
    position: relative;
}
.searchbar{
    position: absolute;
    top:0;
    left:0;
    right:0;
    width:100%;
    height:60px;
    font-size: 1.6rem;
}

.search-input{
        padding: 0 100px 0 20px;
        margin: 0;
        width:calc(100% 120px);
        height:60px;
        border:none;
        font-size:2rem;
        color: #fff;
        background-color: #222;
    }

.search-input :focus{
    outline: none;
}   

.search-icon{
position: absolute;
right:20px;
top:20px;
color: #fff;
}

i{
   margin-left: 15px;
   cursor:pointer; 
}

.result-list {
      padding-top: 60px;
    }

.article-class {
      display: grid;
      grid-template-columns: 200px auto 40px;
      grid-template-rows: 100px;
      border-bottom: 1px solid #ccc;
      overflow: hidden;
      cursor: pointer;
}
.article-class .article-header {
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
}

.article-class .article-header .article-header-img {
          max-width: 100%;
          height: auto;
        }
.article-class .article-header .article-header-i{
     font-size: 2rem;
}

.article-class .article-header .article-section {
        margin: 0;
        padding: 10px;
        height: 100px;
}

.article-class .article-footer {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.6rem;
        color: #888;
      }
        
</style>