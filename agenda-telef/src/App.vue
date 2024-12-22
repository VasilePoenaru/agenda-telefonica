<template>
  <div class="app-container">
    <h1 class="title">Agenda telefonică</h1>
    <h2 class="subtitle">Bine ai venit la agenda ta cu numerele de telefon ale prietenilor și cunoștințelor!</h2>
    <div class="form-container">
      <input type="text" v-model="newName" placeholder="Nume" />
      <input
        type="text"
        v-model="newPhone"
        placeholder="Număr de telefon"
        @input="validatePhone"
      />
      <button class="add-contact" @click="addContact">Adaugă contact</button>
    </div>
    <ul>
      <li v-for="contact in sortedContacts" :key="contact.id">
        {{ contact.name }} – {{ contact.phone }}
        <div class="button-group">
          <button class="edit-button" @click="editContact(contact)">Modifică</button>
          <button class="delete-button" @click="deleteContact(contact.id)">Șterge</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newName: "",
      newPhone: "",
      contacts: [
        { id: 1, name: "Anamaria Poenaru", phone: "0787888999" },
        { id: 2, name: "Vasile Poenaru", phone: "0787784349" },
        { id: 3, name: "Michiela Poenaru", phone: "0783033509" },
      ],
      editingContact: null,
    };
  },
  computed: {
    sortedContacts() {
      return [...this.contacts].sort((a, b) =>
        a.name.localeCompare(b.name)
      );
    },
  },
  methods: {
    validatePhone(event) {
      const value = event.target.value;
      if (!/^\d*$/.test(value)) {
        this.newPhone = value.replace(/\D/g, "");
      }
    },
    addContact() {
      if (!this.newName || !this.newPhone) {
        alert("Te rog să completezi atât numele cât și numărul de telefon!");
        return;
      }

      if (this.editingContact) {
        this.editingContact.name = this.newName;
        this.editingContact.phone = this.newPhone;
        this.editingContact = null;
      } else {
        const newId = this.contacts.length
          ? this.contacts[this.contacts.length - 1].id + 1
          : 1;
        this.contacts.push({
          id: newId,
          name: this.newName,
          phone: this.newPhone,
        });
      }

      this.newName = "";
      this.newPhone = "";
    },
    editContact(contact) {
      this.newName = contact.name;
      this.newPhone = contact.phone;
      this.editingContact = contact;
    },
    deleteContact(id) {
      this.contacts = this.contacts.filter((contact) => contact.id !== id);
    },
  },
};
</script>

<style>
.app-container {
  width: 66%;
  margin: 0 auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

.title {
  color: red;
  text-shadow: 2px 2px 2px lightgray;
}

.subtitle {
  color: darkblue;
  margin-bottom: 20px;
}

.form-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  gap: 10px;
}

.add-contact {
  background-color: lightyellow;
  color: darkgreen;
  border: 1px solid darkgreen;
  padding: 5px 10px;
  cursor: pointer;
  box-shadow: 1px 1px 3px gray;
}

.add-contact:hover {
  background-color: yellow;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 5px 0;
}

.button-group {
  display: flex;
  gap: 10px;
}

.edit-button {
  background-color: lightblue;
  border: 1px solid blue;
  padding: 5px 10px;
  cursor: pointer;
  box-shadow: 1px 1px 3px gray;
}

.edit-button:hover {
  background-color: deepskyblue;
  color: white;
}

.delete-button {
  background-color: lightcoral;
  border: 1px solid red;
  padding: 5px 10px;
  cursor: pointer;
  box-shadow: 1px 1px 3px gray;
}

.delete-button:hover {
  background-color: red;
  color: white;
}
</style>
