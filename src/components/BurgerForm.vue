<template>
    <div>
        <div>
            <form id="burger-form" @submit="CreateBurger">

                <div class="input-container">
                    <label for="name">Nome do Cliente: </label>
                    <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome" required>
                </div>

                <div class="input-container">
                    <label for="pao">Escolha o pão: </label>
                    <select name="pao" id="pao" v-model="pao" required>
                        <option value="">Selecione o seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                    </select>
                </div>

                <div class="input-container">
                    <label for="carne">Escolha a carne: </label>
                    <select name="carne" id="carne" v-model="carne" required>
                        <option value="">Selecione a carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
                    </select>
                </div>

                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Escolha os opcionais: </label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>

                <div class="input-container">
                    <input class="submit-btn" type="submit" name="submit-btn" value="Criar meu Burger!" id="">
                </div>

            </form>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'BurgerForm',
        data() {
            return {
                //Dados que vem da API
                paes: null,
                carnes: null,
                opcionaisdata: null,

                //Dados enviados para a API
                name: null,
                pao: null,
                carne: null,
                opcionais: [],
                msg: null
            }
        },
        methods:{
            async getIndegredients(){
                try {
                    const req = await fetch('http://localhost:3000/ingredientes');
                    const data = await req.json();
                    this.paes = data.paes;
                    this.carnes = data.carnes;
                    this.opcionaisdata = data.opcionais;
                } catch (error) {
                    console.error('Erro ao buscar ingredientes:', error);
                }
            },

            async getCarnes(){
                try {
                    const req = await fetch('http://localhost:3000/ingredientes/carnes');
                    const data = await req.json();
                    this.carnes = data.carnes;
                } catch (error) {
                    console.error('Erro ao buscar carnes:', error);
                }
            },

            async getOpcionais(){
                try {
                    const req = await fetch('http://localhost:3000/ingredientes/opcionais');
                    const data = await req.json();
                    this.opcionaisdata = data.opcionais;
                } catch (error) {
                    console.error('Erro ao buscar opcionais:', error);
                }
            },

            async CreateBurger(e){ // Função para criar o burger e enviar os dados para o backend

                e.preventDefault(); // Evita o comportamento padrão do formulário

                const data = {
                    name: this.name,
                    pao: this.pao,
                    carne: this.carne,
                    opcionais: Array.from(this.opcionais),
                    status: "Solicitado"
                }

                const dataJson = JSON.stringify(data); // Converte os dados para JSON

                const req = await fetch('http://localhost:3000/burgers', {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: dataJson
                });

                const res = await req.json();

                // colocar uma msg de sistema para o usuário, dizendo que o burger foi criado com sucesso ou se houve algum erro


                // limpar os campos do formulário após o envio
                this.name = "";
                this.pao = "";
                this.carne = "";
                this.opcionais = "";
            }
        },
        mounted() {
            this.getIndegredients();
            this.getCarnes();
            this.getOpcionais();
        }
    }
</script>

<style scoped>

div{
    font-family: Arial, Helvetica, sans-serif;
}

#burger-form{
    max-width: 450px;
    margin: 40px auto;
    padding: 30px;
    background: #ffffff;
    border-radius: 12px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.12);
    transition: 0.3s;
}

#burger-form:hover{
    box-shadow: 0 10px 30px rgba(0,0,0,0.18);
}

.input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label{
    margin-bottom: 6px;
    font-weight: bold;
    color: #444;
    font-size: 14px;
}

input, select{
    padding: 12px;
    width: 100%;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 14px;
    background-color: #fafafa;
    transition: all 0.25s ease;
}

input:focus, select:focus{
    outline: none;
    border-color: #fcba26;
    background-color: #fff;
    box-shadow: 0 0 6px rgba(252,186,38,0.4);
}

#opcionais-container{
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
}

#opcionais-title{
    grid-column: span 2;
    margin-bottom: 5px;
}

.checkbox-container{
    display: flex;
    align-items: center;
    background: #f5f5f5;
    padding: 10px;
    border-radius: 6px;
    transition: 0.2s;
    cursor: pointer;
}

.checkbox-container:hover{
    background: #ffe8a6;
}

.checkbox-container input{
    margin-right: 8px;
}

.checkbox-container span{
    font-weight: 500;
}

.submit-btn{
    width: 100%;
    background: linear-gradient(135deg,#fcba26,#f39c12);
    color: #222;
    font-weight: bold;
    border: none;
    padding: 14px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
    transition: all 0.25s ease;
}

.submit-btn:hover{
    transform: translateY(-2px);
    box-shadow: 0 6px 14px rgba(0,0,0,0.2);
}

@media (max-width: 500px){
    #burger-form{
        padding: 20px;
        margin: 20px;
    }

    .checkbox-container{
        width: 100%;
    }
}

</style>