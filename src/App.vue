<template>
    <div class="container">
        <header>
            <nav>
                <ul>
                    <li>
                        <button @click="selectedMenu = 'Post'" :class="{ active: selectedMenu === 'Post' }">Post</button>
                    </li>
                    <li>
                        <button @click="selectedMenu = 'Todos'" :class="{ active: selectedMenu === 'Todos' }">Todos</button>
                    </li>
                </ul>
            </nav>
        </header>
        <div class="content">
            <div v-if="selectedMenu === 'Post'" class="post-section">
                <select v-model="selectedUser" @change="fetchPosts">
                    <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
                </select>
                <div v-if="posts.length > 0">
                    <h2>Postingan User: {{ selectedUserName }}</h2>
                    <ul>
                        <li v-for="post in posts" :key="post.id">
                            <h3>{{ post.title }}</h3>
                            <p>{{ post.body }}</p>
                        </li>
                    </ul>
                </div>
                <div v-else>
                    <p>Loading...</p>
                </div>
            </div>
            <div v-else-if="selectedMenu === 'Todos'" class="todos-section">
                <Todos :todos="todos" />
            </div>
        </div>
    </div>
</template>
  
  <script>
  import Todos from './Todos.vue'; // Sesuaikan dengan path file komponen Todos Anda
  
  export default {
	components: {
	  Todos
	},
	data() {
	  return {
		selectedMenu: 'Post', // Default menu selection
		users: [],
		selectedUser: null,
		selectedUserName: '',
		posts: [],
		todos: [] // Isi dengan data Todos yang telah Anda dapatkan
	  };
	},
	methods: {
	  fetchUsers() {
		fetch('https://jsonplaceholder.typicode.com/users')
		  .then(response => response.json())
		  .then(data => {
			this.users = data;
		  })
		  .catch(error => {
			console.error('Error fetching users:', error);
		  });
	  },
	  fetchPosts() {
		fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`)
		  .then(response => response.json())
		  .then(data => {
			this.posts = data;
		  })
		  .catch(error => {
			console.error('Error fetching posts:', error);
		  });
		// Get selected user name
		const selectedUser = this.users.find(user => user.id === parseInt(this.selectedUser));
		if (selectedUser) {
		  this.selectedUserName = selectedUser.name;
		}
	  }
	},
	watch: {
	  selectedUser() {
		this.fetchPosts();
	  }
	},
	created() {
	  this.fetchUsers();
	}
  };
  </script>
  
  <style>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

header {
    background-color: #3A82EE;
    padding: 5px;
	display: block;
	width: 9.928%;
	border-radius: 0.5rem;
}

nav ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

nav ul li {
    display: inline-block;
    margin-right: 20px;
    color: #fff;
    cursor: pointer;
}

nav ul li.active {
    font-weight: bold;
}

button {
    background-color: #ff00ff;
    color: #fff;
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
}

button:hover {
    background-color: #cc00cc;
}

.content {
    max-width: 800px;
}

.post-section,
.todos-section {
    margin-top: 20px;
}
</style>
  