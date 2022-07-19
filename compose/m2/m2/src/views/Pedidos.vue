<template>
  
                <div class="main-container" >
                  <Message :msg="msg" v-show="msg" />
                    <div id="pedido-table">
                        <div>
                            <div id="pedido-table-heading">
                                <div id="order-id">#:</div>
                                <div>Internet:</div>
                                <div>TV:</div>
                                <div>Telefone:</div>
                                <div>Ações:</div>
                            </div>
                        </div>
                        <div id="pedido-table-rows" v-for="pedido in pedido" :key="pedido.id">
                            <div class="pedido-table-row" >
                                <div class="order-number">{{pedido.id}}</div>
                                <div>{{pedido.nets}}</div>
                                <div>{{pedido.tvs}}</div>
                                <div>{{pedido.telefones}}</div>
                                <div>
                                    <select name="status" class="status" @change="updatePedido($event, pedido.id)">
                                        <option value="">Selecione</option>
                                        <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="pedido.status == s.tipo">{{s.tipo}}</option>
                                    </select>
                                    <button class="delete-btn" @click="deletePedido(pedido.id)">Cancelar</button>
                                </div>
                            </div>
                        </div>
                    </div>
                      
                </div>
                
                
                
         
</template>

<script>
  export default {
    name: "Form",
    data() {
      return {
        pedido: null,
        status: null
      }
    },
    methods: {
      async getPedido() {
        const req = await fetch('http://localhost:3000/pedido');
        const data = await req.json();
        this.pedido = data;
        console.log(this.pedido);
        // Resgata os status de pedidos
        this.getStatus()
      },
      async getStatus() {
        const req = await fetch('http://localhost:3000/status');
        const data = await req.json();
        this.status = data;
        console.log(data);
      },
      async deletePedido(id){
        const req = await fetch(`http://localhost:3000/pedido/${id}`, {
          method: "DELETE"
        });

        const ras = await req.json();

        //msg

        this.getPedido();
      },
       async updatePedido(event, id) {
        const option = event.target.value;
        const dataJson = JSON.stringify({status: option});
        const req = await fetch(`http://localhost:3000/pedido/${id}`, {
          method: "PATCH",
          headers: { "Content-Type" : "application/json" },
          body: dataJson
        });
        const res = await req.json()
        console.log(res)
      }
    },
    mounted () {
    this.getPedido()
    }
  }
</script>

<style>
/* Pedidos */

#pedido-table-heading,
#pedido-table-rows,
#pedido-table-row{
    display: flex;
    flex-wrap: wraps;
}

#pedido-table-heading{
    font-weight: bold;
    padding: 10px;
    border-bottom: 3px solid #333;
}

#pedido-table-heading div,
.pedido-table-row div {
    width: 23%;
    
}

.pedido-table-row div{
  float: left;
}

.pedido-table-row {
    width: 100%;
    padding: 12px;
    border: 1px solid #ccc;
}

#pedido-table-heading .order-id,
.pedido-table-row .order-number{
    width: 5%;
}

select{
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>
