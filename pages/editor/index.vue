<template>
  <div class="wrap">
    <header>
      <div class="logo">
        <h1>Editor</h1>
      </div>
      <div class="button-area">
        <button class="save-button" id="save" @click="save">SAVE</button>
      </div>
    </header>
    <section class="body">
      <div class="left">
        <p>Item</p>
        <ul>
          <li v-for="item in contentList">{{item}}</li>
        </ul>
      </div>
      <div class="right">
        <textarea name="" ref="contents" cols="30" rows="10"></textarea>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "index",

  data() {
    return {
      dbName: "the_name",
      db: null,
      contentList: []
    }
  },

  methods: {
    save() {
      console.log("save")
      const self =this;

      let request = window.indexedDB.open(this.dbName, 2);
      const contents = this.$refs.contents.value
      const customerData =
        {ssn: contents, name: "Bill2", age: 35, email: "bill2@company.com", contents: contents}
      ;

      request.onerror = function (event) {
        // Handle errors.
        console.log(event)
      };
      request.onsuccess = function (event) {
        // Handle errors.
        const db = event.target.result;

        let customerObjectStore = db.transaction("customers", "readwrite").objectStore("customers");

        let b = customerObjectStore.add(customerData);
        b.onsuccess = e => {
          console.log(e)
          self.allRetrieve(event)
        }
        b.onerror = e => {
          console.log(e)
        }
      };

    },

    allRetrieve(event) {
      console.log("allRetrieve", event)
      const db = event.target.result;

      let transaction = db.transaction("customers", "readonly");
      let store = transaction.objectStore("customers");

      let index = store.index('name');
      this.contentList.length=0;
      index.openCursor().onsuccess = function (event) {
        let cursor = event.target.result;
        if (cursor) {
          let data = cursor.value;
          console.log(JSON.stringify(cursor.value));
          this.contentList.push(data?.ssn)
          cursor.continue();
          // data.cursor.continue(); // while (data.cursor.continue()) { // console.log(cursor.value.name); // }
        }
      }.bind(this)

    }
  },

  mounted() {

    const self = this;
    // This is what our customer data looks like.
    const customerData = [
      {ssn: "444-44-4444", name: "Bill", age: 35, email: "bill@company.com"},
      {ssn: "555-55-5555", name: "Donna", age: 32, email: "donna@home.org"}
    ];

    const request = window.indexedDB.open(this.dbName, 2);

    request.onerror = function (event) {
      // Handle errors.
    };
    request.onupgradeneeded = function (event) {
      self.db = event.target.result;

      // Create an objectStore to hold information about our customers. We're
      // going to use "ssn" as our key path because it's guaranteed to be
      // unique - or at least that's what I was told during the kickoff meeting.
      const objectStore = self.db.createObjectStore("customers", {keyPath: "ssn"});

      // Create an index to search customers by name. We may have duplicates
      // so we can't use a unique index.
      objectStore.createIndex("name", "name", {unique: false});

      // Create an index to search customers by email. We want to ensure that
      // no two customers have the same email, so use a unique index.
      objectStore.createIndex("email", "email", {unique: false});

      // Use transaction oncomplete to make sure the objectStore creation is
      // finished before adding data into it.
      objectStore.transaction.oncomplete = function (event) {
        // Store values in the newly created objectStore.
        const customerObjectStore = self.db.transaction("customers", "readwrite").objectStore("customers");
        customerData.forEach(function (customer) {
          customerObjectStore.add(customer);
        });
      };
    };
    request.onsuccess = event => {
      this.allRetrieve(event)
      console.log(event)
    }
  }
}
</script>


<style scoped>
.wrap {
  display: flex;
  width: 100vw;
  height: 100vh;
  flex-direction: column;
}

header {
  background-color: #35495e;
  width: 100%;
  height: 10%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.logo {
  width: 20%;
}

.button-area {
  background-color: darkkhaki;
  width: 30%;
  justify-content: flex-end;
  display: flex;
  padding: 10px;
}

.save-button {
  position: relative;
  /*top:10px;*/
  /*left: 10px;*/
  /*right: 0px;*/
  /*bottom: 0px;*/
}

.body {
  display: flex;
  flex-direction: row;
  height: 100%;
}

.left {
  width: 30%;
  height: 100%;
  background-color: #526488;
}

.right {
  width: 70%;
  height: 100%;
  background-color: gray;
}

.right > textarea {
  width: 100%;
  height: 100%;
}

.right > textarea:focus {
  outline: none;
}

</style>
