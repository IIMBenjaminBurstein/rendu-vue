<template>
<div id="form">
        <div id="align">
            <h2 id="error">Erreur</h2>
            <div>
                <div class="mb-3">
                    <label class="form-label">Titre de la Page</label>
                    <input v-if="$route.name != 'create'"  type="text" class="form-control" v-model.trim="newTitle" disabled="disabled">
                    <input v-else  type="text" class="form-control" v-model.trim="title">
                </div>
                <div class="mb-3">
                    <label class="form-label">Meta Title</label>
                    <input v-if="$route.name != 'create'" v-model.trim="newMetaTitle"   type="text" class="form-control">
                    <input v-else  type="text" class="form-control" v-model.trim="metaTitle" >
                </div>
                <div class="mb-3">
                    <label class="form-label">Meta Description</label>
                    <input v-if="$route.name != 'create'" v-model.trim="newMetaDesc"   type="text" class="form-control">
                    <input v-else  type="text" class="form-control" v-model.trim="metaDesc" >
                </div>
            </div>
            <div id="img">
                <label>Image par défault</label>
                <img  src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Pas_d%27image_disponible.svg/1200px-Pas_d%27image_disponible.svg.png"
                    alt="image blog">
                <label>Modifier l'image via un url</label>
                <input v-if="$route.name != 'create'" v-model.trim="newImg"   type="text" />
                <input v-else v-model.trim="img" type="text" />
            </div>
        </div>
        <div class="mb-3" id="bottom">
            <label class="form-label">Content</label>
            <textarea v-if="$route.name != 'create'" v-model.trim="newContent" class="form-control"></textarea>
            <textarea v-else  v-model.trim="content" class="form-control"></textarea>
        </div>   
        <button v-if="$route.name === 'create'" @click="add" class="btn btn-primary">Créer la page</button>
        <button v-else  @click="put" class="btn btn-primary">Modifier le poste</button>
</div>
</template>

<script>

export default {
    data() {
        if(this.$route.name != 'create'){
             return {
            'newTitle' : this.$store.state.posts[this.$route.params.id].title,
            'newMetaTitle': this.$store.state.posts[this.$route.params.id].metaTitle,
            'newMetaDesc' : this.$store.state.posts[this.$route.params.id].metaDesc,
            'newContent':  this.$store.state.posts[this.$route.params.id].content,
            'newImg' :  this.$store.state.posts[this.$route.params.id].img,
            'newDate' : this.$store.state.posts[this.$route.params.id].date,
            }   
        }
    },
   methods: {
       add(){
           this.$store.dispatch('verifyPost', { 
            'title' : this.title, 
            'metaTitle' :  this.metaTitle, 
            'metaDesc': this.metaDesc, 
            'content' : this.content, 
            'img' : this.img,
            'date' : this.dataDate,
            });
        },
       put() {
            this.$store.dispatch('verifyModifPost', [{ 
            'title' : this.newTitle, 
            'metaTitle' :  this.newMetaTitle, 
            'metaDesc': this.newMetaDesc, 
            'content' : this.newContent, 
            'img' : this.newImg,
            'date' : this.newDate,
            }, this.$route.params.id]);
           
       }
   },
   computed: {
       posts() {
           return this.$store.state.posts;
       },
        dataDate() {
                var date = new Date();
                let days = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"]
                let months = ["Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Aout", "Septembre",
                    "Octobre", "Novembre", "Decembre"
                ]

                var dayNumber = date.getDate();
                var actualDay = days[date.getDay()]
                var actualMonth = months[date.getMonth()]

                return actualDay + " " + dayNumber + " " + actualMonth + " "
   },
}
}
</script>

<style scoped>
    #form {
        margin-top: 50px;
        width: 40%;
        border: solid 1px black;
    }

    #align {
        display: flex;
        justify-content: space-around;
        margin-top: 50px;
    }
    #align input {
        width: 60%;
        text-align: center;
    }
    #align .mb-3 {
        display: flex;
        justify-content: space-between;
    }

    #img {
        width: 30%;
        display: grid;
    }

    #img img {
        width: 80%;
        margin: 0 auto;
        border: black 1px solid;
        height: auto;
    }
    #img input {
       margin: 20px auto;
    }
    #metaDes {
        width: 80%;
        margin: 0 auto;
    }
    #bottom {
        display: flex;
        width: 90%;
        justify-content: space-between;
        margin: 0 auto;
        align-items: center;
    }
    #bottom textarea {
        width: 75%;
    }
    button {
        margin-bottom: 50px;
        margin-top: 25px;
    }
    #error{
        color: red;
        display: none;
    }
</style>