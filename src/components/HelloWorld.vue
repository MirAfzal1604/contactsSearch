<template>
  <div class="box">
    <div class="header">
      <h4 class="title">Live User Filter</h4>
      <small class="subtitle"> Search by name and/or location </small>
      <input type="text" id="filter" placeholder="Search" v-model="keyword" />
    </div>

    <div class="loading" v-if="loading">
      <h3>Loading...</h3>
    </div>

    <ul id="result" class="user-list">
      <li v-for="user in searchInfo" :key="user">
        <img :src="user.picture.large" :alt="user.name.first" />
        <div class="user-info">
          <h4>
            <span v-html="user.name.first"></span>
            {{ user.name.last }}
            <!-- {{ user.name.last | replace('cse','computer science') }} -->
          </h4>
          <p class="forMargin">
            <span v-html="user.location.city"></span>
            <span> | </span>
            <span v-html="user.location.country"></span>
          </p>
          <p>age {{ user.dob.age }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  async mounted() {
    this.loading = true;

    const res = await fetch("https://randomuser.me/api?results=5000");
    const { results } = await res.json();
    this.list = results;

    this.loading = false;
  },

  data() {
    return {
      loading: false,
      keyword: "",
      list: [],
    };
  },

  computed: {
    searchInfo() {
      return this.list.filter((user) => {
        var fio = user.name;
        var loc = user.location;

        return (
          fio.first.toLowerCase().includes(this.keyword.toLowerCase()) ||
          fio.last.toLowerCase().includes(this.keyword.toLowerCase()) ||
          loc.city.toLowerCase().includes(this.keyword.toLowerCase()) ||
          loc.country.toLowerCase().includes(this.keyword.toLowerCase())
        );
      });
    },
  },
};
</script>

<style scoped >
.loading {
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.7);
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
#result li b {
  font-weight: normal;
  background: yellow;
  border-radius: 3px;
}

.box {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

title {
  margin: 0;
}

.subtitle {
  display: inline-block;
  margin: 5px 0 20px;
  opacity: 0.8;
}

.header {
  background-color: #3e57db;
  color: #fff;
  padding: 30px 20px;
}

.header input {
  background-color: rgba(0, 0, 0, 0.3);
  border: 0;
  border-radius: 50px;
  color: #fff;
  font-size: 14px;
  padding: 10px 15px;
  width: 100%;
}

.header input:focus {
  outline: none;
}

.user-list {
  background-color: #fff;
  list-style-type: none;
  margin: 0;
  padding: 0;
  height: 80%;
  overflow-y: auto;
}

.user-list li {
  display: flex;
  align-items: center;
  padding: 20px;
}

.user-list img {
  border-radius: 50%;
  object-fit: cover;
  height: 75px;
  width: 75px;
}

.user-list .user-info {
  margin-left: 30px;
}

.user-list .user-info h4 {
  margin: 0;
}

.user-list .user-info p {
  font-size: 12px;
}

.user-list .user-info .forMargin {
  margin: 7px 0;
}

.user-list li:not(:list-of-type) {
  border-bottom: 1px solid #eee;
}
</style>
