<template>
    <div id="appWrapper">
        <div class="container">
            <div class="search-box">
                <input type="text" name="q" placeholder="Escribe algo..." class="search-input" id="q" v-model="query"
                    @input="search">
                <ul class="result-list" :class="resultVisibility">
                    <li v-for="post in posts" class="result-item">
                        <a href="#" class="result-link">
                            <div class="result-title">{{ post.title }}</div>
                            <div class="result-content">{{ post.content.substr(1, 40) }}...</div>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            query: '',
            posts: []
        }
    },
    computed: {
        resultVisibility() {
            return (this.query.length > 0) ? 'show' : 'hide';
        }
    },
    methods: {
        search() {
            if (this.query.length >= 3) {
                axios.post('/posts/search', {
                    q: this.query
                }).then(res => {
                    this.posts = res.data.posts;
                }).catch(error => {
                    console.log(error.response);
                });
            }
        }
    }
}
</script>
<style>
body {
    font-family: sans-serif;
}

::-webkit-scrollbar {
    width: 10px;
}

::-webkit-scrollbar-track {
    background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
    background: #888;
}

::-webkit-scrollbar-thumb:hover {
    background: #555;
}

.container {
    padding: 30px;
}

.search-box {
    position: relative;
    display: flex;
    align-items: center;
    flex-direction: column;
}

.search-input {
    width: 600px;
    height: 30px;
    border-radius: 10px;
    border: 0;
    background: #eee;
    padding: 10px 20px;
    font-size: 18px;
    outline: none;
}

.result-list.show {
    position: absolute;
    width: 640px;
    max-height: 250px;
    overflow-y: auto;
    list-style: none;
    background: #fff;
    padding: 0px;
    top: 40px;
    border-radius: 10px;
    box-shadow: 1px 2px 8px 0px #b5b5b5;
}

.result-list.hide {
    display: none;
}

.result-item {
    border-bottom: 1px solid #ececec;
}

.result-link {
    text-decoration: none;
    color: #333;
    display: block;
    padding: 10px 15px;
}

.result-link:hover {
    background: #f9f9f9;
}

.result-title {
    font-size: 20px;
    font-weight: 600;
}

.result-content {
    font-size: 18px;
}
</style>