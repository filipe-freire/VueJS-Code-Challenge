<template>
  <div id="app">
    <section>
      <div class="head">
        <h2>Next events</h2>
        <input type="text" placeholder="Search" />
      </div>
      <!-- CardList Container -->
      <div>
        <!-- Each card Here -->
        <div class="service-card">
          <div class="service-card-head">
            <div>
              <h3>Passeio de Canoa</h3>
              <p class="date">10.03.2021 from 15.00 to 19.00 (4h)</p>
            </div>
            <div>
              <div class="availability-wrapper">
                <p>Availability</p>
                <p>23/52</p>
              </div>
              <button class="btn regular">Open</button>
            </div>
          </div>
          <div class="service-card-body">
            <p>
              Lorem ipsum dolor sit amet consectetur adipisicing elit. Ut non eum mollitia
              dignissimos, libero blanditiis corporis ipsa molestias molestiae reiciendis qui earum
              ipsam quo quaerat labore saepe sint consequatur! Rerum.
            </p>
            <div class="btn-container">
              <button class="btn edit"><i class="far fa-edit"></i>Edit</button>
              <button class="btn delete"><i class="far fa-trash-alt"></i>Delete</button>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: "App",
  components: {
    // HelloWorld
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap");
/* Custom Properties */
:root {
  /* COLORS */
  --availability-high: #6ac951;
  --availability-medium: #ffc764;
  --availability-low: #ff5533;

  /* BUTTONS */
  --bg-save: #6ac951;
  --bg-save-border: rgba(74, 82, 106, 0.35);

  --bg-regular: #f1f1f8;
  --bg-regular-border: #d3d7db;

  --bg-delete: #f16c64;
  --bg-delete-border: rgba(241, 108, 100, 0.35);

  --bg-edit: #00a9f0;
  --bg-edit-border: rgba(0, 169, 240, 0.4);

  /* FONTS */
  --font-family-primary: "Roboto", sans-serif;
}

/* Styling reset */
*,
::before,
::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: var(--font-family-primary);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

section {
  margin: 0 auto;
  max-width: 800px;
  padding: 1rem 2rem;
}

.head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1em;
}

.head > input {
  flex-grow: 1;
  max-width: 200px;
  padding: 1ch;
}

/* Services Card Styling */

.service-card {
  background-color: #f5f8fa;
  padding: 1rem 2rem;
  border-left: 7px solid var(--availability-medium);
  border-bottom: 1px solid #3e3e3e54;
}

.service-card-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.service-card-head > div:last-child {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-grow: 1;
  max-width: 194px;
}

.date {
  font-size: 10pt;
  margin-top: 0.8ch;
  color: #979797;
}

.availability-wrapper {
  text-align: right;
}

.availability-wrapper > p:nth-child(1) {
  color: #4b536b;
  font-size: 12pt;
}

.availability-wrapper > p:nth-child(2) {
  margin-top: 0.2ch;
  font-weight: 500;
}

/* SERVICE CARD BODY (description and buttons) */

.service-card-body {
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}

.service-card-body > p {
  max-width: 38ch;
  font-size: 10pt;
}

.btn-container {
  flex-grow: 1;
  max-width: 250px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

/* BUTTONS */
.far {
  margin-right: 0.7ch;
}

.btn {
  padding: 0.7rem 1.5rem;
  border-radius: 7px;
  font-family: inherit;
  font-weight: 400;
  color: #fff;
}
.btn + .btn {
  margin-left: 1ch;
}
.btn.regular {
  background-color: var(--bg-regular);
  border: 1px solid var(--bg-regular-border);
  color: #4b536b;
}
.btn.edit {
  background-color: var(--bg-edit);
  border: 1px solid var(--bg-edit-border);
}
.btn.delete {
  background-color: var(--bg-delete);
  border: 1px solid var(--bg-delete-border);
}
.btn.save {
  background-color: var(--bg-save);
  border: 1px solid var(--bg-save-border);
}
</style>
