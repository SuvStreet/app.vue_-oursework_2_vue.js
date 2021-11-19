<template>
  <div class="card container">
    <h2 class="title">Комментарии</h2>
  </div>

  <div
    class="card container"
    v-for="{ id, email, body, name } in commentsProps"
    :key="id"
  >
    <ul class="list">
      <li class="list-item">
        <div>
          <div class="person">
            <div class="avatar"></div>
            <small class="namePerson">{{ name }}</small>
          </div>
          <small>{{ body }}</small>
          <p>
            <a class="mail" :href="'mailto:' + email">{{ email }}</a>
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['commentsProps'],
}
</script>

<style scoped>
.title{
  margin: 0;
}

.namePerson {
  font-weight: bold;
  color: #00d170;
}

.namePerson:hover {
  cursor: pointer;
  text-decoration: underline;
}

.person {
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
  padding-bottom: 5px;
  border-bottom: 1px solid #eaecef;
}

.avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-image: url(https://i2.wp.com/www.cssscript.com/wp-content/uploads/2020/12/Customizable-SVG-Avatar-Generator-In-JavaScript-Avataaars.js.png?fit=438%2C408&ssl=1);
  background-size: auto 50px;
  display: inline-block;
  margin-right: 10px;
}

p {
  margin: 0;
  text-align: right;
}

.mail {
  font-weight: bold;
  color: #00000061;
  transition: 0.5s;
  margin: 0;
}

.mail:hover {
  font-weight: bold;
  color: #000000;
  transition: 0.5s;
  text-decoration: underline;
}
</style>
