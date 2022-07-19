<template>
    
        <div> 
            <Message :msg="msg" v-show="msg" />
            <form id="Forms" @submit="createConcluir">
                <div class="input-container">
                    <p>Escolha sua Internet</p>
                    <div class="separador">
                        <input type="radio" id="1" v-model="nets" name="nets" value="100mb">
                        <label for="100mb">100 MB <p>R$??,??</p></label>
                    </div>

                    <div class="separador">
                        <input type="radio" id="2" v-model="nets" name="nets" value="200mb">
                        <label for="200mb">200 MB <p>R$??,??</p></label>
                    </div>

                    <div class="separador">
                        <input type="radio" id="3" v-model="nets" name="nets" value="300mb">
                        <label for="300mb">300 MB <p>R$??,??</p></label>
                    </div>  
                </div>
            




                <div class="tv">
                <div class="input-container"  v-show='(nets == "100mb") || (nets == "200mb") || (nets == "300mb")'>
                    <p>Escolha sua TV</p>
                    <div class="separadorTv" >
                        <input type="radio" id="ultimatehd" v-model="tvs" name="tvs" value="ultimatehd">
                        <label for="ultimatehd">Ultimade HD <p>R$??,??</p></label>
                    </div>

                    <div class="separadorTv" >
                        <input type="radio" id="fullhd" v-model="tvs" name="tvs" value="fullhd">
                        <label for="fullhd">Full HD <p>R$??,??</p></label>
                    </div>
  
                </div>
                </div>


                <div class="telefone">
                <div class="input-container" v-show='(nets == "100mb") || (nets == "200mb") || (nets == "300mb")'>
                    <p>Escolha seu Telefone</p>
                    <div class="separadorTelefone" >
                        <input type="radio" id="100mb" v-model="telefones" name="telefones" value="ilimitado fixo">
                        <label for="ilimitadofixo">Ilimitado Fixo <p>R$??,??</p></label>
                    </div>

                </div>
                </div>

                <div class="input-container">
                    <input class="submit-btn" type="submit" value="concluir"/>
                </div>

              
            </form>
            
        </div>

    
</template>

<script>
import Message from './Message'
export default {
  name: "Form",
  data() {
    return {
        id: null,
        nets: null,
        tvs: null,
        telefones: null,
        status:"Solicitado"
    }
  },
  methods: {
    async getPlano() {
      const req = await fetch('http://localhost:3000/plano')
      const data = await req.json()
      this.nets = data.nets
      this.tvs = data.tvs
      this.telefones = data.telefones

    },
    async createConcluir(e) {
      e.preventDefault();
      console.log("Concluiu");
      const data = {
        nets: this.nets,
        tvs: this.tvs,
        telefones: this.telefones,
        status: "Solicitado"
      }
      console.log(data);

      const dataJson = JSON.stringify(data)    
      const req = await fetch("http://localhost:3000/pedido", {
        method: "POST",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      });
      const res = await req.json()
      console.log(res)
      //Colocando msg no sistema
      this.msg = "Pedido realizado com sucesso!"
      // clear message
      setTimeout(() => this.msg = "", 3000)
      // limpar campos
      this.nets = ""
      this.tvs = ""
      this.telefones = ""
      
    }
  },
  mounted () {
    this.getPlano()
  },
  components: {
    Message
  }
}
</script>

<style scoped>
.submit-btn{
    margin: 0 46%;
}
p{
    text-align: center;
    font-size: 20px;
    color: #a32cc4;
    margin: 30px 0;
}
.separador{
    text-align: center;
    font-size: 20px;
    color: #a32cc4;
    width: 90px;
    height: 90px;
    border: 2px solid #a32cc4;
    float: left;
}

.separadorTv{
    text-align: center;
    font-size: 20px;
    color: #a32cc4;
    width: 90px;
    height: 90px;
    border: 2px solid #a32cc4;
    float: left;
}

.separadorTelefone{
    text-align: center;
    font-size: 20px;
    color: #a32cc4;
    width: 90px;
    height: 90px;
    border: 2px solid #a32cc4;
    float: left;
}

/*
@media screen and (max-width: 1329px) {
    .separador{
        margin: 0 140px;
    }
}
*/


/* NET */
@media screen and (max-width: 1440px) {
    .separador{
        margin: 0 6%;
        width: 20%;
    }
}

@media screen and (max-width: 768px) {
    .separador{
        margin: 30px 20%;
        width: 60%;
    }
}

@media screen and (max-width: 644px) {
    .separador{
        margin: 30px 0;
        width: 100%;
    }
}

/* TV */

@media screen and (max-width: 1440px) {
    .separadorTv{
        margin: 0 6%;
        width: 20%;
    }
}

@media screen and (max-width: 768px) {
    .separadorTv{
        margin: 30px 20%;
        width: 60%;
    }
}

@media screen and (max-width: 644px) {
    .separadorTv{
        margin: 30px 0;
        width: 100%;
    }
}

/* Telefone */

@media screen and (max-width: 1440px) {
    .separadorTelefone{
        margin: 0 6% 30px 6%;
        width: 20%;
    }
}

@media screen and (max-width: 768px) {
    .separadorTelefone{
        margin: 30px 20%;
        width: 60%;
    }
}

@media screen and (max-width: 644px) {
    .separadorTelefone{
        margin: 30px 0;
        width: 100%; 
    }
}

.telefone p {
    width: 100%;
    height: auto;
    float: left;
}





</style>