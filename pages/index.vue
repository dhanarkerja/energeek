<template>
  <div class="container">
    <img src="../assets/energeek2 1.png" class="text-center">
    <form>
      <h5>Jabatan</h5>
      <select v-model="jabatan" class="form-control form-control-sm">
        <option v-for="list in listjabatan" :value="list.name" :key="list.id">{{list.name}}</option>
      </select>
      <h5>Telepon</h5>
      <input v-model="telepon" type="text" class="form-control">
      <h5>Email</h5>
      <input v-model="email" type="email" class="form-control">
      <h5>Tanggal Lahir {{thl}}</h5>
      <b-form-datepicker id="example-datepicker" :show-decade-nav="true" v-model="thl" class="mb-2"></b-form-datepicker>
      <h5>Skill Set ({{skill}})</h5>
      <select class="form-control form-control-sm" @change="getskill($event)">
        <option v-for="list in listskill" :value="list.name" :key="list.id">{{list.name}}</option>
      </select>
      <button type="button" @click="submit()" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data(){
    return {
      jabatan:"",
      listjabatan:[],
      listskill:[],
      telepon:"",
      email:"",
      thl:"",
      skill:[]
    }
  },
  async created(){
    this.initJabatan()
    this.initSkill()
  },
  methods:{
    getskill(id){
      console.log(id.target.value)
      this.skill.push(id.target.value)
      console.log(this.skill)
    },
    submit(){
      var getJobId = 0
      var getSkillID = []
      var loopjob = []
      var loopskil = []
      console.log(this.skill)
      for ( let i =0 ; i<this.listjabatan.length;i++) {
        if(this.listjabatan[i].name === this.jabatan) {
          console.log("aaaa")
          getJobId = this.listjabatan[i].id
        }
      }

      for ( let i =0 ; i<this.listskill.length;i++) {
        for ( let k =0 ; k<this.skill.length;k++){
          if(this.listskill[i].name === this.skill[k]) {
            console.log("bb")
            getSkillID.push(this.listskill[k].id)
          }
        }
      }
      console.log(getSkillID)
    },
    async initSkill(){
      const config = {
        headers: {
            Accept : "application/json"
        }
      };
      try {
        const res = await axios.get(`http://demo73.energeek.co.id/energeek-frontend-test/public/api/select_list/skill?search=`, config);
        this.listskill = res.data.data.skills
        console.log(this.listskill)
      } catch (err) {
          console.log(err);
      } 
    },
    async initJabatan(){
      const config = {
        headers: {
            Accept : "application/json"
        }
      };
      try {
        const res = await axios.get(`http://demo73.energeek.co.id/energeek-frontend-test/public/api/select_list/job?search`, config);
        this.listjabatan = res.data.data.jobs
        console.log(this.listjabatan)
      } catch (err) {
          console.log(err);
      } 
    }
  },
}
</script>
<style >

</style>