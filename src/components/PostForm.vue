<script>

export default {
    props:{
        id: String,
        post: Object,
    },
    data() {
        return {
            formData: {
                title: this.post?.title || "",
                content: this.post?.content || ""
            },            
            editable: Boolean(this.post),

            buttonName: Boolean(this.post) === true ? "Salvar" : "Criar"
        }
    },
    methods: {
        handleCreatePost(event) {
            event.preventDefault()

            if (!this.formData.title || !this.formData.content) {
                alert("preencha os campos...")
                return
            }

            let now = new Date()
            let dataDaPostagem = `${now.getDate()}/0${now.getMonth() + 1}/${now.getFullYear()} - ${now.getHours().toString().padStart(2, "0")}:${now.getMinutes().toString().padStart(2, "0")}`

            /* PASSA OBJETO NO EVENTO */
            const obj = {
                title: this.formData.title,
                content: this.formData.content,
                datetime: dataDaPostagem
            }

            this.editable ? this.$emit("edit-post", obj, this.id) : this.$emit("create-post", obj)
            
            this.$router.push("/")
        },
    }
}
</script>

<template>
    <div class="container">
        <form id="form" action="">
            <input 
                v-model="formData.title" 
                placeholder="Titulo"
            >

            <textarea 
                v-model="formData.content" 
                placeholder="Mensagem" rows="15">
            </textarea>

            <button type="submit" @click="handleCreatePost">
                {{buttonName}}
            </button>
        </form>
    </div>
</template>

<style>
form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    margin-top: 10px;
    margin-bottom: 10px;
}

form>* {
    background-color: white;
    box-shadow: 3px 3px 15px lightgray;
    border: none;

    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;

    width: 500px;
    padding: 10px;
    margin-top: 5px;

    border: 3px;
}

form button {
    background-color: rgb(53, 53, 53);
    color: whitesmoke;
    font-weight: 800;
    width: 100px;
    border-radius: 10px;

    transition: 1s;
}

form button:hover {
    background-color: #2b1a1a;
    box-shadow: 3px 3px 0px lightcoral;
}

@media (max-width: 600px) {
    form>* {
        width: 85vw;
    }
}
</style>