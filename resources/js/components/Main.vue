<template>
    <main class="container">
        <h1>Elenco post:</h1>

        <div class="row">
            <div class="card d-flex col-4" v-for="post in posts" :key="post.id" style="width: 18rem;">
                <div class="card-body d-flex flex-column">
                    <h5 class="card-title">{{post.title}}</h5>
                    <p class="card-text">{{post.content}}</p>
                    <a href="#" class="btn btn-primary">Mostra dettagli post</a>
                </div>
            </div>
        </div>
        <nav aria-label="Page navigation example">
            <ul class="pagination">
                <li class="page-item" :class="currentPage==1 ? 'd-none' : '' ">
                    <button class="page-link" @click="changePagePosts(currentPage - 1)">Previous</button>
                </li>
                <li class="page-item" :class="currentPage==lastPage ? 'd-none' : '' ">
                    <button class="page-link" @click="changePagePosts(currentPage + 1)">Next</button>
                </li>
            </ul>
        </nav>
        
    </main>
</template>

<script>
    export default {
        name : "Main",

        data(){
            return{
                posts: [],
                currentPage: 1,
                lastPage: null
            }
        },

        methods:{
            changePagePosts(PaginaDaCambiare){
                axios.get("/api/posts", {
                    "params": {
                        "page": PaginaDaCambiare
                    }
                }).then( (response) => {
                    console.log(response);
                    this.currentPage = response.data.results.current_page;
                    this.posts = response.data.results.data;
                    console.log(this.posts);
                    this.lastPage = response.data.results.last_page;
                });
            }
        },

        created(){
            this.changePagePosts(this.currentPage);
        }
    }
</script>

<style>

</style>