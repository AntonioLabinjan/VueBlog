<template>
  <div>
    <center><h2>VueBlog</h2></center>
    <input type="file" @change="uploadImages" multiple>
    <textarea v-model="postText" placeholder="Unesite tekstualni post"></textarea>
    <button @click="addPost">Dodaj post</button>
    <div v-for="(item, index) in items" :key="index">
      <div v-if="item.type === 'image'">
        <img :src="item.url" alt="Prenesena slika">
        <p>Datum i vrijeme postavljanja: {{ item.date }}</p>
        <div>
          <p v-if="item.description">{{ item.description }}</p>
          <input
            type="text"
            v-model="item.editDescription"
            placeholder="Unesite ili uredite opis slike"
          >
          <button @click="saveDescription(index)">Spremi opis</button>
          <button @click="deleteDescription(index)">Obriši opis</button>
        </div>
        <button @click="deleteImage(index)">Obriši sliku</button>
      </div>
      <div v-else>
        <p>Datum i vrijeme postavljanja: {{ item.date }}</p>
        <p>{{ item.text }}</p>
        <button @click="editText(index)">Uredi tekst</button>
        <button @click="deleteText(index)">Obriši tekst</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [], // Pohranjuje slike, tekstualne postove i njihove informacije
      postText: "", // Tekstualni post
    };
  },
  methods: {
    uploadImages(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        if (file) {
          const reader = new FileReader();
          reader.onload = () => {
            const date = new Date(); // trenutno vrijeme
            this.items.push({
              type: 'image',
              url: reader.result,
              date: date.toLocaleString(),
              description: "", // Početni prazni opis slike
              editDescription: "" // Polje za uređivanje opisa
            });
          };
          reader.readAsDataURL(file);
        }
      }
    },
    addPost() {
      if (this.postText.trim() !== "") {
        const date = new Date(); // trenutno vrijeme
        this.items.push({
          type: 'text',
          date: date.toLocaleString(),
          text: this.postText
        });
        this.postText = ""; // Resetiranje unosa teksta
      }
    },
    deleteImage(index) {
      this.items.splice(index, 1);
    },
    saveDescription(index) {
      // Spremi opis slike
      this.items[index].description = this.items[index].editDescription.trim();
      this.items[index].editDescription = ""; // Resetiraj uređivanje opisa
    },
    deleteDescription(index) {
      // Obriši opis slike
      this.items[index].description = "";
      this.items[index].editDescription = ""; // Resetiraj uređivanje opisa
    },
    editText(index) {
      // Omogućava uređivanje teksta posta
      const editedText = prompt("Uredi tekst posta", this.items[index].text);
      if (editedText !== null) {
        this.items[index].text = editedText.trim();
      }
    },
    deleteText(index) {
      // Omogućava brisanje teksta posta
      if (confirm("Jeste li sigurni da želite obrisati ovaj tekst?")) {
        this.items.splice(index, 1);
      }
    }
  }
};
</script>
