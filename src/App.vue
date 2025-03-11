<template>
  <div class="container">
    <div class="input-container">
      <h3>JSON blocos</h3>
      <textarea
        v-model="jsonInput1"
        rows="10"
        placeholder="JSON dos blocos aqui..."
      ></textarea>
    </div>
    <div class="input-container">
      <h3>JSON opções</h3>
      <textarea
        v-model="jsonInput2"
        rows="10"
        placeholder="JSON de opções aqui..."
      ></textarea>
    </div>
    <div>
      <button @click="loadJson">Começar</button>
      <button @click="saveChanges">Salvar alterações</button>
    </div>
    <div class="output-container">
      <div v-if="editorData1.length > 0">
        <div v-for="(item, index) in editorData1" :key="index" class="item">
          <div class="number"><strong>{{ item.key }}</strong></div>
          <div class="itens">

          <p>
                <p class="before">
                  Atual - 
                  <input :placeholder="initialContentPlaceholders[item.key]" class=" disabled input" disabled>
            </p>
            Novo -
            <input v-model="item.value" class="after input" />
            </p>
            <template v-if="editorData2[item.key]">
            <div class="group">
              <div
              v-for="(groupItem, groupIndex) in editorData2[item.key]"
              :key="groupIndex"
              class="group-item"
              >
              <input
              type="checkbox"
              v-model="groupItem.ATIVO"
              true-value="Y"
              false-value="N"
              :id="'ativo_' + item.key + '_' + groupIndex"
              class="ativo "
              />
              <input v-model="groupItem.ACEITA" class="input-checkbox input" />
              <label :for="'ativo_' + item.key + '_' + groupIndex" >{{ groupItem.CONTEUDO }}</label>
                      </div>
                      </div>
          </template>
        </div>
      </div>
      </div>
      <div v-else class="else">
        <p id="noData">Sem dados para mostrar.</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      jsonInput1: "",
      jsonInput2: "",
      editorData1: [],
      editorData2: {},
      initialContentPlaceholders: {}
    };
  },
  methods: {

    loadJson() {
      try {
        const parsedJson1 = JSON.parse(this.jsonInput1);
        this.editorData1 = Object.keys(parsedJson1).map((key) => ({
          key: key,
          value: parsedJson1[key],
        }));

        const parsedJson2 = JSON.parse(this.jsonInput2);
        this.editorData2 = parsedJson2;
    this.editorData1.forEach(item => {
      if (!this.initialContentPlaceholders[item.key]) {
        
        this.initialContentPlaceholders[item.key] = item.value;
      }
    });
  }
    catch (error) {
        alert("JSON em Formato inválido");
      }
  }, 
    saveChanges() {
      this.jsonInput1 = JSON.stringify(
        this.editorData1.reduce((acc, item) => {
          acc[item.key] = item.value;
          return acc;
        }, {}),
        null,
        2
      );

      this.jsonInput2 = JSON.stringify(this.editorData2, null, 2);
      loadJson()
    },
  },
};
</script>

<style scoped>
.number{
  width: 100vh;
  text-align: start;
  margin-left: 23vh;
}


.else{
  text-align: center;
}
  
.itens{
  margin-left: 2vh;
}

.input-checkbox{
  margin-left: 4vh;
  margin-right: 1vh;
}

.disabled{
  background-color: rgba(255, 255, 255, 0.11);
  margin-bottom: 1vh;
  color: white;
  width: 70vh;
}

.before{
  /* margin-left:  6vh; */
  text-align: start;
  width: 90vh;
}

#noData {
  margin-top: 4vh;
}

.ativo {
  margin-top: 2vh;
  margin-left: 1vh;
  width: 3vh;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  width: 200vh;
}

.input-container {
  width: 100vh;
  margin-top: 3vh;
}

textarea {
  width: 100%;
  height: 200px;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
}

button {
  margin-top: 2vh;
  margin-bottom: 2vh;
  padding: 10px 20px;
  margin-left: 1vh;
  margin-right: 1vh;
  background-color: #2c2c2c;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #3e3e3e;
}

.output-container {
  width: 100%;
  text-align: start;
}

.item {
  padding-bottom: 6vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.group {
  margin-bottom: 2vh;
}

.group-item {
  width: 90vh;
  /* margin-left: 6vh; */
  text-align: start;
}

h3 {
  margin-top: 0;
}

.after {
  width: 90%;
  }

.input{
    border: 1px solid #ccc;
    border-radius: 3px;
    max-width: 70vh;
    padding: 5px;

}
</style>
