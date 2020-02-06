<template>
  
  <div id="main">
    <h1>Welcome to Syfon Github finder</h1>
    <h2>Your Username is : {{username}}</h2>
    <input type="text" name="gh-username" placeholder="Search for a GitHub username..." v-model="username" @keyup="search" :class="{ 'has-error': error }">

    <span class="loading" v-if="loading">Searching GitHub for "{{ username }}"...</span>

    <div class="box" v-if="results && !loading || error && !loading">

        <div v-if="results">
            <img v-if="results.avatar_url" :src="results.avatar_url" :alt="'GitHub Avatar for ' + username ">
            <h4 v-if="results.name">Name:{{ results.name }}</h4>
            <p v-if="results.login">Username:{{ results.login }}</p>
            <i v-if="results.bio">Bio:{{ results.bio }}</i>
            <p v-if="results.public_repos">Public Repo:{{ results.public_repos }}</p>
            <p v-if="results.followers">Followers:{{ results.followers }}</p>
            <p v-if="results.following">Following:{{ results.following }}</p>
            <p v-if="results.curl">Commits:{{ results.curl}}</p>
            <p v-if="results.location">Location:{{ results.location }}</p>
        </div>

        <span v-if="error">Sorry, Syfon Github can't find user called "<i>{{ username }}</i>"!</span>

    </div>
  </div>
  

  

</template>

<script>
import axios from 'axios';
export default {
  name: 'Finder',
data () {
            return {
                username: '',
                results: '',
                error: '',
                loading: false,
            }
        },
        watch: {
            username: function () {
                this.error = '';
            }
        },
        methods:  {
            search: function() {
                if( this.username ){
                    this.loading = true;
                    // console.log("Searching for... " + this.username );
                    axios.get('https://api.github.com/users/' + this.username)
                        .then(response => {
                            // console.log( response.data );
                            this.results = response.data;
                            this.error = '';
                            this.loading = false;
                        }).catch(error => {
                            this.results = '';
                            this.error = error;
                            this.loading = false;
                        });
                } else {
                    this.results = '';
                    this.error = '';
                    this.loading = false;
                }
            }
        }

}
</script>

<style scoped>
div {
   font-family: "HelveticaNeue-Light", "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial, "Lucida Grande", sans-serif;
   font-weight: 300;
   font-size: 16px; line-height: 1.3;
}
h1,h2{
  width: 500px;
  max-width: 90%;
    padding: 25px;
    margin: 15px auto;
    text-align: center;
}
input {
    display: block;
    width: 450px;
    max-width: 74%;
    padding: 15px 25px;
    margin: 45px auto;
}
.has-error {
    border: 1px solid #96281B;
    outline: none;
}
.box {
    display: block;
    width: 450px;
    max-width: 74%;
    padding: 25px;
    margin: 15px auto;
    background: rgb(226, 226, 226);
    border: 1px solid #D2D7D3;
    text-align: center;
}
.box img {
    display: block;
    width: 200px;
    height: auto;
    margin: 0 auto 15px;
}
.loading {
    display: block;
    text-align: center;
    color: rgb(112, 130, 150);
}
#main{
  background: rgb(235, 233, 233);
  height: auto;
  min-height: 100vh;
  padding: 15px 0px;
}
</style>
