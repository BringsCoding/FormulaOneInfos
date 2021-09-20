container
<template>
  <div class="text">
    Formel 1<br />
    info Seite
  </div>
  <section class="infos-section">
    <details class="container-info">
      <summary class="infos"><strong>Information</strong></summary>
      <p>
        Hier können sie die Fragen stellen, <br />wer von 1950 bis jetzt Formel
        1<br />
        Weltmeister geworden ist und<br />
        mit wie vielen Punkten! <br />Des Weiteren wird abgefragt<br />
        wie viele Rennen <br />es in der jeweiligen Session gegeben hat!
      </p>
    </details>
    <h3 class="font-medium text-xl">Hol dir die Formel 1 Infos</h3>
    <input
      class="text-input"
      type="number"
      v-model="name"
      placeholder="Suche"
    />
    <button @click="newName = name">Press me</button>
  </section>

  <ul>
    <li v-for="lib in data" :key="lib.name">
      Es gab <strong>{{ data.MRData.total }}</strong> Rennen im Jahr
      <strong>{{ newName }}</strong>
    </li>

    <li v-for="lib in dataDriver" :key="lib.name">
      Weltmeister wurde
      <strong>{{
        dataDriver.MRData.StandingsTable.StandingsLists[0].DriverStandings[0]
          .Driver.givenName
      }}</strong
      >&#160;<strong>{{
        dataDriver.MRData.StandingsTable.StandingsLists[0].DriverStandings[0]
          .Driver.familyName
      }}</strong>
      mit
      <strong>{{
        dataDriver.MRData.StandingsTable.StandingsLists[0].DriverStandings[0]
          .points
      }}</strong>
      Punkten
    </li>
  </ul>
</template>

<script>
import { ref, watchEffect, reactive, toRefs } from "vue";
export default {
  name: "App",
  watch: {
    newName() {},
  },
  setup() {
    const name = ref(null);
    const newName = ref(null);
    const state = reactive({ data: [] });
    const stateDrive = reactive({ dataDriver: [] });
    watchEffect(() => {
      if (newName.value)
        fetch(`https://ergast.com/api/f1/${newName.value}.json`)
          .then((response) => response.json())
          .then((data) => {
            state.data = data;
            console.log("data", data.MRData.total);
            name.value = "";
          });
      if (newName.value)
        fetch(`https://ergast.com/api/f1/${newName.value}/driverStandings.json`)
          .then((response) => response.json())
          .then((dataDriver) => {
            stateDrive.dataDriver = dataDriver;
            console.log(
              "data",
              dataDriver.MRData.StandingsTable.StandingsLists[0]
                .DriverStandings[0].Driver.code
            );
            name.value = "";
          });
    });
    return {
      name,
      newName,
      ...toRefs(state),
      ...toRefs(stateDrive),
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
html {
  background-color: beige;
}

.infos-section {
  display: inline-table;
}

.infos {
  position: relative;
  right: 3.5625rem;
  margin: 1.875rem;
  font-size: 1em;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
h3 {
  position: relative;
  right: 0.3125rem;
}

p {
  position: relative;
  left: 1.875rem;
  text-align: justify;
}

.text-input {
  border-radius: 20px;
  outline: none;
}

.text {
  border-radius: 3px;
  font-size: 4em;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  text-transform: uppercase;
  font-weight: 800;
  transition: color 400ms linear;
  color: rgb(248, 248, 248);
  text-shadow: 1px 1px 1px #0661bd, 1px 2px 1px #0661bd, 1px 3px 1px #0661bd,
    1px 4px 1px #0661bd, 1px 5px 1px #0661bd, 1px 6px 1px #0661bd,
    1px 7px 1px #0661bd, 1px 8px 1px #0661bd, 1px 9px 1px #0661bd,
    1px 10px 1px #0661bd, 1px 18px 6px rgba(16, 16, 16, 0.4),
    1px 12px 10px rgba(16, 16, 16, 0.4), 1px 25px 35px rgba(16, 16, 16, 0.4),
    1px 30px 60px rgba(16, 16, 16, 0.4);
}

.text:hover {
  color: rgb(161, 161, 161);
  transition: color 700ms linear;
}
</style>

/* https://api.github.com/users/${newName.value}/repos
https://ergast.com/api/f1/${newName.value}/driverStandings.json */
