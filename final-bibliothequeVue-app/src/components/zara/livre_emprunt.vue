<template lang="">
   <div v-for="doc in docs" :key="doc.id" class="doc">
        <div>
            <p><b>Titre :</b> {{doc.titre}}</p>
        </div>
        <div>
            <p><b>Auteur : </b>{{doc.auteur}}</p>
        </div>
        <div>
            <p><b>Date d'apparution :</b> {{doc.dateParution}}</p>
        </div>
        <div>
            <p><b>Catégorie :</b> {{doc.categorie}}</p>
    
        </div>
        <div>
            <p><b>A propos :</b> {{doc.apropos}}</p>
        </div>
        <div>
            <button class="btn btn-outline-primary" @click="emprunter($event,doc._id)">Emprunter</button>
        </div>
  </div>
</template>
<script >
import DocumentItem from "../../types/DoucumentItem";
import config from "../../types/config";
import Singleton from "@/types/Singleton";
export default {
  data() {
    return {
      docs: [
        //lorsque la base distant n'est pas disponible, décommenter cette ligne de code
        // { id: 1, title : 'titre1', auteur : 'Auteur1', dateParution : 'Date1', categorie : 'Categorie1', apropos : 'c\'est un livre qui raconte la vie de Monsieur Jacques qui est un homme courageuxc\'est un livre qui raconte la vie de Monsieur Jacques qui est un homme courageuxc\'est un livre qui raconte la vie de Monsieur Jacques qui est un homme courageux'},
        // { id: 2, title : 'titre1', auteur : 'Auteur1', dateParution : 'Date1', categorie : 'Categorie1', apropos : 'c\'est un livre qui raconte la vie de Monsieur Jacques qui est un homme courageux'},
        // { id: 3, title : 'titre1', auteur : 'Auteur1', dateParution : 'Date1', categorie : 'Categorie1', apropos : 'c\'est un livre qui raconte la vie de Monsieur Jacques qui est un homme courageux'},
        // { id: 4, title : 'titre4', auteur : 'Auteur4', dateParution : 'Date4', categorie : 'Categorie4', apropos : 'c\'est un livre qui raconte la vie de Monsieur Jacques qui est un homme courageux'}
      ],
    };
  },
  methods: {
    // rechercher() {

    // },
    emprunter(event, id) {
      fetch(config + "/api/bookPhysique/emprunter", {
        method: "POST",
        mode: "cors",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify({
          newBookManagement: {
            idPhysicalBook: id,
            idUser: Singleton.Utilisateur.id,
          },
        }),
      })
        .then((response) => response.text())
        .then((responseJson) => {
          event.target.disabled = true;
          console.log(responseJson);
          if (responseJson == "already exist") {
            alert("vous avez déjà emprunter ce livre");
          }
          console.log(responseJson);
        });
    },
  },
  mounted() {
    fetch(config + "/api/bookPhysique")
      .then((response) => response.json())
      .then((result) => {
        this.docs = result;
      });
  },
};
</script>
<style lang="scss" scoped>
.doc {
  padding: 10px;
  margin: 12px;
  box-shadow: 1px 5px 24px rgba(0, 0, 0, 0.5);
  border-radius: 12px;
  width: 50%;
  margin: 0 auto 15px auto;
}
</style>
