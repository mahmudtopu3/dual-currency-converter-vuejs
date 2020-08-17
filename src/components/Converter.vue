<template>
  <div class="container">
    <div class="row">
     
      <div class="col-md-12">
        
        <div class="row converter">
           <div class="col-md-12 text-center">
             <h3>Simple VueJS Dual Currency Converter</h3>
             <br>
           </div>
          <div class="col-md-6">
            <div class="form-group input-group-sm">
              <label for="exampleFormControlSelect1">Send </label>
              <select class="form-control" id="exampleFormControlSelect1" v-model="send">
                <option v-for="(currency,index) in currencies" :key="index">{{ currency }}</option>
              </select>
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group input-group-sm">
              <label for="exampleFormControlSelect1">Receive </label>
              <select class="form-control" id="exampleFormControlSelect1" v-model="receive">
                <option v-for="(currency,index) in currencies" :key="index">{{ currency }}</option>
              </select>
            </div>
          </div>
          <div class="col-md-6">
            <div class="input-group input-group-sm mb-3">
              <div class="input-group-prepend">
                <span class="input-group-text" id="inputGroup-sizing-sm">Insert</span>
              </div>
              <input v-model="input" type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
            </div>
            <button @click="result()" type="submit" class="btn btn-primary btn-sm">Submit</button>

          </div>
          <div class="col-md-6">
            <div class="input-group input-group-sm mb-3">
              <div class="input-group-prepend">
                <span class="input-group-text" id="inputGroup-sizing-sm">Result</span>
              </div>
              <input v-model="output" disabled type="text" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
            </div>
            
          </div>

          <div v-if="show" class="col-md-6">
            <br>
            <div class="alert alert-success alert-dismissible fade show" role="alert">
              <strong>Successfully Converted!</strong> Result: {{ output }}.
              <button type="button" class="close" @click="show=false">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
          </div>
          <br><br><br><br><br><br>

          <div class="col-md-6">
            <ul>
              <li v-for="rate in rates" :key="rate.id">{{rate.from}} To {{rate.to}} Rate: {{ rate.rate }}</li>
            </ul>
          </div>
          
        </div>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Converter',
  props: {
    msg: String
  },
  data(){
     return {
       send : 'USD',
       receive : 'BDT',
       currencies: [],
       input  : 1,
       output :null,
       show : false,
       rates : [
         {id:1,from:'USD',to:'BDT',rate:84.75},
         {id:2,from:'POUND',to: 'BDT',rate:110.86},
         {id:3,from:'CAD',to: 'BDT',rate:64.07},
       ]
     }
  },
  methods : {
    calc(val){
       
        let currency = this.rates.filter(x => x.from === this.send && x.to===this.receive);
        if(currency.length>0 && currency[0].from == this.send && currency[0].to == this.receive){
          console.log('bb')
          this.output = val*currency[0].rate
        }
        else if(this.send === this.receive){
          this.output = val
        }
        else {
          if(this.send!='BDT'){
              let send = this.rates.filter(x => x.from === this.send && x.to==='BDT');
              let receive = this.rates.filter(x => x.from === this.receive && x.to==='BDT');
              this.output = val*((1/receive[0].rate) / (1/send[0].rate))
          }
          else {
            let cur = this.rates.filter(x => x.from === this.receive && x.to==='BDT');
            this.output =  val*(1/cur[0].rate)
            console.log('bdt to')
          }
      
        }
      
    },
    result(){
      this.output!=null? this.show = true : this.show = false;
    }
  },
  watch : {
    input(val){
       this.calc(val);
    },
    send(){
      this.calc(this.input);
    },
    receive(){
      this.calc(this.input);
    }
   

  },
  created(){
    let a = []
    let b = []
    this.rates.forEach(element=>{
      
        a.push(element.to)
        b.push(element.from)

    })
    this.currencies = [...new Set([...a, ...b])];
    this.calc(this.input);
  }
}
</script>

<style scoped>
.converter{
  margin: 10% auto;
}
</style>
