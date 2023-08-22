<script setup lang="ts">
    import {ref} from "vue";

    const yourObject = "YOUR_OBJECT_NAME"

    const sourceUrl = ref(`http://localhost:8080/o/c/${yourObject}`)
    const targetUrl = ref(`http://localhost:8088/o/c/${yourObject}`)
    const usernameSource = ref("test@liferay.com")
    const passwordSource = ref("test1234")
    const usernameTarget = ref("test@liferay.com")
    const passwordTarget = ref("test1234")
    const statusMessageExport = ref("")
    const statusMessageImport = ref("")
    const processStarted = ref(false)
    const batchImport = ref(false)

    async function fetchData(url, auth) {
      const headers = new Headers();
      headers.set("Authorization", "Basic " + btoa(auth));

      const response = await fetch(url, { headers });
      return response.json();
    }

    async function updateData(item, auth) {
      const headers = new Headers();
      headers.set("Content-Type", "application/json");
      headers.set("Authorization", "Basic " + btoa(auth));

      await fetch(`${targetUrl.value}/by-external-reference-code/${item.externalReferenceCode}`, {
        method: "PUT",
        headers: headers,
        body: JSON.stringify(item),
      });
    }

    async function updateDataBatch(targetAuth: string, data: any[]) {
      const headers = new Headers();
      headers.set("Content-Type", "application/json");
      headers.set("Authorization", "Basic " + btoa(targetAuth));

      await fetch(`${targetUrl.value}/batch`, {
        method: "POST",
        headers: headers,
        body: JSON.stringify(data),
      });
    }

    async function transferData() {
      console.log("transferData")

      processStarted.value = true;

      const sourceAuth = usernameSource.value + ":" + passwordSource.value;
      const targetAuth = usernameTarget.value + ":" + passwordTarget.value;
      const pageSize = 100; // Dies kann angepasst werden, je nachdem wie viele Ergebnisse pro Seite gewünscht sind.

      let data = [];
      let page = 1;

      const initialResult = await fetchData(`${sourceUrl.value}?page=${page}&pageSize=${pageSize}`, sourceAuth);

      if (!initialResult.totalCount) {
        console.error('Invalid source data, totalCount is missing.');
        return;
      }

      const totalPages = Math.ceil(initialResult.totalCount / pageSize);

      for (let currentPage = 1; currentPage <= totalPages; currentPage++) {
        statusMessageExport.value = `Fetching page ${currentPage} of ${totalPages}`;
        const result = await fetchData(`${sourceUrl.value}?page=${currentPage}&pageSize=${pageSize}`, sourceAuth);

        if (result.items) {
          for (const item of result.items) {
            delete item.actions;
            delete item.id;
            data.push(item);
          }
        }
      }

      if(batchImport.value){
        statusMessageImport.value = `Batch Importing ${data.length} items`;
        await updateDataBatch(targetAuth, data);
      }else{
        //for each data item call the targetUrl
        for (const item of data) {
          const i = data.indexOf(item);
          statusMessageImport.value = `Updating ${i+1}/${data.length}: ${item.externalReferenceCode}`;
          await updateData(item, targetAuth);
        }
      }
    }
</script>

<template>
  <div>
    <div class="row">
      <label>Source URL: </label>
      <input v-model="sourceUrl" />
    </div>
    <div class="row">
      <label>Username: </label>
      <input v-model="usernameSource" />
    </div>
    <div class="row">
      <label>Password: </label>
      <input v-model="passwordSource" type="password" />
    </div>
    <div class="row mt-5">
      <label>Target URL: </label>
      <input v-model="targetUrl" />
    </div>
    <div class="row">
      <label>Username: </label>
      <input v-model="usernameTarget" />
    </div>
    <div class="row">
      <label>Password: </label>
      <input v-model="passwordTarget" type="password" />
    </div>
      <div class="row mt-5">
      <label>Try Batch Import:</label>
      <input type="checkbox" v-model="batchImport" />
    </div>
    <button @click="transferData">Transfer Data</button>
  </div>
  <div class="mt-5">
    <h3>Status:</h3>
    <ul v-if="processStarted">
      <li>{{statusMessageExport}}</li>
      <li>{{statusMessageImport}}</li>
    </ul>
  </div>
</template>

<style scoped>
.mt-5{
  margin-top: 5rem;
}

label{
  display: inline-block;
  width: 150px;
}
input{
  width: 300px;
}
.row{
  margin-bottom: 10px;
}

button{
  background-color: #9960a0;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

</style>
