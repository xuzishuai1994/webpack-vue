<template>
    <div>
        <button
            v-if="$route.path !=='/time-entries/log-time'"
            v-link="'/time-entries/log-time'"
        class="btn btn-primary">
        创建            
        </button>

        <div v-if="$route.path === '/time-entries/log-time'">
            <h3>创建</h3>
        </div>
        <hr />

        <router-view></router-view>

        <div class="time-entries">
            <p v-if="!timeEntries.length"><stong>还没有任何任务</stong></p>
            <div class="list-group">
                <a class="list-group-item" v-for="timeEntry in timeEntries">
                    <div class="row">
                        <div class="col-sm-2 user-details">
                            <img src="https://avatars1.githubusercontent.com/u/10184444?v=3&s=460" class="avatar img-circle img-responsive" />
                            <p class="text-center">
                                <storng>
                                    二哲
                                </storng>
                            </p>
                        </div>

                        <div class="col-sm-2 text-center time-block">
                        <h3 class="list-group-item-text total-time">
                            <i class="glyphicon glyphicon-time"></i>
                            {{timeEntry.totalTime}}
                        </h3>
                        <p class="label label-primary text-center">
                            <i class="glyphicon glyphicon-calendar"></i>
                            {{timeEntry.date}}
                        </p> 
                        </div>

                        <div class="col-sm-7 comment-section">
                            <p>{{timeEntry.comment}}</p>
                        </div>

                        <div class="col-sm-1">
                            <button
                            class="btn btn-xs btn-danger delete-button"
                            @click="deleteTimeEntry(timeEntry)">
                            X
                            </button>
                        </div>
                    </div>
                </a>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    route:{
        data(){
            this.$http.get('http://localhost:8888/time-entries').then(function(ret){
                this.timeEntries = ret.data;
            }).then(function(err){
                console.log(err);
            })
        }
    },
    data () {
      return {
        timeEntries: []
      }
    },
    methods: {
      deleteTimeEntry (timeEntry) {
        let index = this.timeEntries.indexOf(timeEntry)
        let _id = this.timeEntries[index]._id

        if (window.confirm('确定要删除吗?')) {
            this.$http.delete('http://localhost:8888/delete/'+_id).then(function(ret){
                console.log(ret);
            }).then(function(err){
                console.log(err);
            })
          this.timeEntries.splice(index, 1)
          this.$dispatch('deleteTime', timeEntry)
        }
      }
    },
    events: {
      timeUpdate (timeEntry) {
        this.timeEntries.push(timeEntry)
        return true
      }
    }
  }
</script>

<style>
    .avatar{
        height:75px;
        margin:0 auto;
        margin-top:10px;
        margin-bottom:10px;
    }
    .user-details{
        background-color:#f5f5f5;
        border-right: 1px solid #ddd;
        margin:-10px 0;
    }
    .time-block{
        padding:10px;
    }
    .comment-section{
        padding:20px
    }
</style>
