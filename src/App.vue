<template>
  <div id="app">	  <div id="activityApp">
    <h1>New Vue Activity App</h1>	    <nav class="navbar is-white topNav">
      <div class="container">
        <div class="navbar-brand">
          <h1>{{fullAppName}}</h1>
        </div>
      </div>
    </nav>
    <nav class="navbar is-white">
      <div class="container">
        <div class="navbar-menu">
          <div class="navbar-start">
            <a class="navbar-item is-active" href="#">Newest</a>
            <a class="navbar-item" href="#">In Progress</a>
            <a class="navbar-item" href="#">Finished</a>
          </div>
        </div>
      </div>
    </nav>
    <section class="container">
      <div class="columns">
        <div class="column is-3">
          <a v-if="!isFormDisplayed" @click="toggleFormDisplay" class="button is-primary is-block is-alt is-large" href="#">New Activity</a>
          <div v-if="isFormDisplayed" class="create-form">
            <h2>Create Activity</h2>
            <form>
              <div class="field">
                <label class="label">Title</label>
                <div class="control">
                  <input v-model="newActivity.title" class="input" type="text" placeholder="Read a Book">
                </div>
              </div>
              <div class="field">
                <label class="label">Notes</label>
                <div class="control">
                  <textarea v-model="newActivity.notes" class="textarea" placeholder="Write some notes here"></textarea>
                </div>
              </div>
              <div class="field is-grouped">
                <div class="control">
                  <button v-bind:disabled ='!isFormValid' @click="createActivity" class="button is-link">Create Activity</button>
                </div>
                <div class="control">
                  <button class="button is-text" @click="toggleFormDisplay">Cancel</button>
                </div>
              </div>
            </form>
          </div>
        </div>
        <div class="column is-9">
          <div class="box content">
            <ActivityItem v-for="activity in activities"
                           :activity="activity"
                           :key="activity.id"></ActivityItem>
          </div>
        </div>
      </div>
    </section>
  </div>	  
</div>
</template>

<script>
import ActivityItem from '@/components/ActivityItem'
import {fetchActivities, fetchCategories, fetchUser} from '@/api/'
export default {
  name: 'app',
    components: {ActivityItem},
  data(){
    return{
      isFormDisplayed: false,
      isTextDisplayed: true,
      creator:'Raydoan Anik',
      appName:'Activity Plannner',
      newActivity: {
        title: '',
        notes: ''
      },
      items: {1: {name: 'Filip'}, 2: {name: 'John'}},
        user: {},
        activities:{},
        categories: {}
    }
  },
  created(){
    this.activities = fetchActivities()
    this.categories = fetchCategories()
    this.user = fetchUser()

    console.log(this.categories)
    console.log(this.user)
  },
  computed:{
    isFormValid() {
      return this.newActivity.title && this.newActivity.notes
    },
    fullAppName () {
      return this.appName + ' by ' + this.creator
    }
  },
  methods: {
     toggleTextDisplay () {
      this.isTextDisplayed = !this.isTextDisplayed
    },
    toggleFormDisplay () {
      this.isFormDisplayed = !this.isFormDisplayed
    },
    createActivity () {
      console.log(this.newActivity)
    },
    
  }
  
}
</script>

<style>
#activityApp{
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

html,
body {
  font-family: 'Open Sans', serif;
  background: #F2F6FA;
}

footer {
  background-color: #F2F6FA !important;
}

.topNav {
  border-top: 5px solid #3498DB;
}

.topNav .container {
  border-bottom: 1px solid #E6EAEE;
}

.container .columns {
  margin: 3rem 0;
}

.navbar-menu .navbar-item {
  padding: 0 2rem;
}

aside.menu {
  padding-top: 3rem;
}

aside.menu .menu-list {
  line-height: 1.5;
}

aside.menu .menu-label {
  padding-left: 10px;
  font-weight: 700;
}

.button.is-primary.is-alt {
  background: #00c6ff;
  background: -webkit-linear-gradient(to bottom, #0072ff, #00c6ff);
  background: linear-gradient(to bottom, #0072ff, #00c6ff);
  font-weight: 700;
  font-size: 14px;
  height: 3rem;
  line-height: 2.8;
}

.media-left img {
  border-radius: 50%;
}

.media-content p {
  font-size: 14px;
  line-height: 2.3;
  font-weight: 700;
  color: #8F99A3;
}

article.post {
  margin: 1rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #E6EAEE;
}

article.post:last-child {
  padding-bottom: 0;
  border-bottom: none;
}

.menu-list li {
  padding: 5px;
}

.navbar-brand>h1 {
  font-size: 31px;
  padding: 20px;
}
</style>
