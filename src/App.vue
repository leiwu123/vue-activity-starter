<template>
  <div id="activityApp">
    <nav class="navbar is-white topNav">
      <div class="container">
        <div class="navbar-brand">
          <h1>{{ fullAppName }}</h1>
        </div>
      </div>
    </nav>
    <TheNavBar />
    <section class="container">
      <!-- <div class="example-wrapper">
        <div v-if="isTextDisplayed">
          <h1 v-bind:title="titleMessage">{{message}}</h1>
        </div>
        <div v-else>for=""
          Hidden Message
        </div>

        {{items}}

        <div v-for="item in items">
          {{item.name}}
        </div>
        <button @click="toggleTextDisplay">Hide Message</button>
      </div> -->
      <div class="columns">
        <div class="column is-3">
          <!-- Activity Form -->
          <activityCreate :categories="categories" @activityCreated="addActivity" />
          <!-- Activity Form END -->
        </div>
        <div class="column is-9">
          <div class="box content" :class="{fetching: isFetching, 'has-error': error}">
            <div v-if="error">
              {{ error }}
            </div>
            <div v-else>
              <div v-if="isFetching">
                Loading...
              </div>
              <ActivityItem
                v-for="activity in activities"
                :key="activity.id"
                :activity="activity"
              />
            </div>
            <div v-if="!isFetching">
              <div class="activity-length">Currently {{ activityLength }} activities</div>
              <div class="activity-motivation">{{ activityMotivation }}</div>
            </div>
            
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Vue from 'vue'
import ActivityItem from './components/ActivityItem'
// import ActivityItem from '@/components/ActivityItem' //@ points to src folder
import ActivityCreate from '@/components/ActivityCreate'
import TheNavBar from '@/components/TheNavBar'
import  { fetchActivities, fetchCategories, fetchUser } from './api'   // short for ./api/index
// import { fetchActivities } from '@/api'   // short for ./api/index


export default {
  name: 'App',
  components: {ActivityItem, ActivityCreate, TheNavBar},
  data(){
    return {
      creator: 'Leiwu123',
      appName: 'Activity Planner',
      isFetching: false,
      error: null,
      user: {
      },
      activities: {
      },
      categories: {
      }        
    }
    
  },
  computed: {
    isFormValid() {
      return this.newActivity.title && this.newActivity.notes
    },
    fullAppName() {
      return this.appName + ' by ' + this.creator
    },
    activityLength() {
      // debugger
      // const activitiesKeyArray = Object.keys(this.activities)
      // console.log(activitiesKeyArray)
      // const activityLength = activitiesKeyArra // isFetching: false,
      // error: null,y.length
      // return activityLength

      return Object.keys(this.activities).length
    },
    activityMotivation() {
      if (this.activityLength && this.activityLength <5) {
        return 'Nice to see some goals (:'
      } else if (this.activityLength >= 5) {
        return 'So many activities! Good Job!'
      } else {
        return 'No activities, so sad!'
      }
    }
  },
  // watch: {
  //   creator(val) {
  //     debugger
  //     console.log(val)
  //     this.watchedAppName = this.appName + ' by ' + val
  //   },
  //   appName(val) {
  //     debugger
  //     console.log(val)
  //     this.watchedAppName = val + ' by ' + this.creator
  //   }
  // },
  created () {
    this.isFetching = true
    fetchActivities()
      .then(activities => {
        this.activities = activities
        this.isFetching = false
      })
      .catch(err => {
        this.error = err
        this.isFetching = false
      })
    this.user = fetchUser(),
    this.categories = fetchCategories()
    // console.log(this.user);
    // console.log(this.categories),
    // console.log(this.activities)
  },
  methods: {
    addActivity(newActivity) {
      // debugger
      // this.activities[newActivity.id] = newActivity
      // console.log(this.activities)
      Vue.set(this.activities, newActivity.id, newActivity)
      
    }
          // toggleTextDisplay(){
          //   this.isTextDisplayed = !this.isTextDisplayed
          // },
          // toggleFormDisplay(){
          //   this.isFormDisplayed = !this.isFormDisplayed
          // },
          // yell: function() {
          //   alert('meow!!!')
          // }, 
          // createActivity() {
          //   console.log(this.newActivity)
          // },
          // isFormValid() {
          //   return this.newActivity.title && this.newActivity.notes
          // }
        }

}
</script>

<style>
#activityApp {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}

html,
body {
  font-family: "Open Sans", serif;
  background: #f2f6fa;
}
footer {
  background-color: #f2f6fa !important;
}
.fetching {
  border: 2px solid orange;
}
.has-error {
  border: 2px solid red;
}
.activity-motivation {
  float: right;
}
.activity-length {
  display: inline-block;
}
.example-wrapper {
  margin-left: 30px;
}
.topNav {
  border-top: 5px solid #3498db;
}
.topNav .container {
  border-bottom: 1px solid #e6eaee;
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
  color: #8f99a3;
}
article.post {
  margin: 1rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #e6eaee;
}
article.post:last-child {
  padding-bottom: 0;
  border-bottom: none;
}
.menu-list li {
  padding: 5px;
}

.navbar-brand > h1 {
  font-size: 31px;
  padding: 20px;
}

</style>
