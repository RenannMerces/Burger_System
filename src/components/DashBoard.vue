<template>
    <div id="burger-table">
        <MessageForm  :msg="msg" v-show="msg"/>
        <table>
            <thead id="burger-table-heading">
                <tr>
                    <th class="order-id">#</th>
                    <th>Cliente</th>
                    <th>Pão</th>
                    <th>Carne</th>
                    <th>Opcionais</th>
                    <th>Status</th>
                    <th>Ações</th>
                </tr>
            </thead>

            <tbody id="burger-table-rows">
                <tr class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                    <td class="order-number">{{ burger.id }}</td>
                    <td> {{ burger.name }} </td>
                    <td> {{ burger.pao }} </td>
                    <td> {{ burger.carne }} </td>

                    <!-- Opcionais -->
                    <td>
                        <ul>
                            <li v-for="(opcional, index) in burger.opcionais" :key="index">
                                {{opcional}}
                            </li>
                        </ul>
                    </td>

                    <!-- Status -->
                    <td>
                        <select name="status" class="status" @change="updateBurger($event, burger.id)">
                            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">
                                {{s.tipo}}
                            </option>
                        </select>
                    </td>

                    <td>
                        <button class="delete-btn" @click="deleteBurger(burger.id)">Excluir</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>

import MessageForm from './MessageForm.vue';

export default {
    name: "DashBoard",
    data(){
        return{
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        }
    },components: {
            MessageForm
    },
    methods: {
        async getPedidos(){

            const req = await fetch("http://localhost:3000/burgers")
            const data = await req.json();
            this.burgers = data;

            // resgatar os status
            this.getStatus();

        },

        async getStatus(){

            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();
            this.status = data;
        },

        async deleteBurger(id){
            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "DELETE"
            })

            const res = await req.json();

            // msg
            // colocar uma msg de sistema para o usuário, dizendo que o burger foi criado com sucesso ou se houve algum erro
            this.msg = `Pedido removido com sucesso!`;

            //limpar msg

            setTimeout(() => {this.msg = null;}, 5000);

            this.getPedidos();
        },

        async updateBurger(event, id){
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "content-type": "application/json"},
                body: dataJson
            });

            const res = await req.json();

                 // colocar uma msg de sistema para o usuário, dizendo que o burger foi criado com sucesso ou se houve algum erro
            this.msg = `O pedido N° ${res.id} foi atualizado para ${res.status}`;

            //limpar msg

            setTimeout(() => {this.msg = null;}, 5000);

            console.log(res);

        }

    }, mounted(){

        this.getPedidos();
    }
}


</script>

<style scoped>

    #burger-table{
        max-width: 1100px;
        margin: 60px auto;
        padding: 20px;
    }

    table{
        width: 100%;
        border-collapse: separate;
        border-spacing: 0 10px;
    }

    #burger-table-heading{
        background: #111;
    }

    th{
        color: #fff;
        padding: 16px 18px;
        text-align: left;
        font-size: 14px;
        font-weight: 600;
        letter-spacing: 0.5px;
    }

    .burger-table-row{
        background: #ffffff;
        box-shadow: 0 3px 8px rgba(0,0,0,0.06);
        transition: 0.2s;
    }

    .burger-table-row:hover{
        transform: scale(1.01);
        box-shadow: 0 5px 14px rgba(0,0,0,0.1);
    }

    td{
        padding: 18px;
        font-size: 14px;
        color: #444;
    }

    .order-number{
        font-weight: bold;
        color: #222;
    }

    ul{
        margin: 0;
        padding-left: 18px;
    }

    li{
        font-size: 13px;
        margin-bottom: 4px;
    }

    .status{
        padding: 8px 12px;
        border-radius: 6px;
        border: 1px solid #ddd;
        background: #f8f8f8;
        font-size: 13px;
        cursor: pointer;
        transition: 0.2s;
    }

    .status:hover{
        border-color: #999;
    }

    button{
        padding: 8px 14px;
        margin-right: 6px;
        border: none;
        border-radius: 6px;
        font-size: 13px;
        font-weight: 600;
        cursor: pointer;
        color: #000;
        transition: 0.2s;
    }

    button:hover{
        transform: translateY(-1px);
        box-shadow: 0 3px 6px rgba(0,0,0,0.15);
    }

    .delete-btn{
        background: #e63946;
        color: white;
    }

    .delete-btn:hover{
        background: #c1121f;
    }

</style>