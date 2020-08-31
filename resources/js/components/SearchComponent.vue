<template>
    <div>
        <input type="text" v-model="q" class="form-control mb-3" placeholder="Rechercher...">
        <div class="alert alert-secondary" v-if="this.q && getFilteredPosts.length >= 1">
            {{ getFilteredPosts.length }} articles trouvés
        </div>
        <div class="alert alert-secondary" v-if="this.q && getFilteredPosts.length === 0">
            Aucun article trouvé
        </div>
        <div class="loader" v-if="this.loading"></div>
        <div v-for="post in getFilteredPosts" v-bind:key="post.id">
            <div class="card mb-3">
                <div class="card-body">
                    <h5 class="card-title">{{ post.title }}</h5>
                    <p class="card-text">{{ post.description }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {

        data() {
            return {
                posts: [],
                q: '',
                loading: true
            }
        },

        methods: {
            fetchPosts() {
                axios.get('/api/posts')
                .then(response => {
                    this.posts = response.data;
                    this.loading = false;
                })
                .catch(errors => console.log(errors))
            }
        },

        computed: {
            getFilteredPosts() {
                return this.posts.filter(
                   post => {
                       return post.description.toLowerCase().includes(this.q.toLowerCase());
                   }
                );
            }
        },

        mounted() {
            this.fetchPosts();
        }
    }
</script>

<style scoped>
.loader {
  border: 16px solid #eeeeee; /* Light grey */
  border-top: 16px solid #3498db; /* Blue */
  border-radius: 50%;
  margin: 0 auto;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
