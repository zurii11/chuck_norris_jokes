<template>
    <div class="wrapper">
        <div class="header">
            <h1>CHUCK NORRIS JOKES</h1>
        </div>
        <div class="func">
            <select class="sl" v-model="selectedCategory">
                <option value="">-- Select category --</option>
                <option value="all">Any category</option>
				<option v-for="category in categories" :key="category" :value="category">{{ category.charAt(0).toUpperCase()+category.slice(1) }}</option>
            </select>
            <button class="btn" @click="getJoke(selectedCategory)">
                Give me a joke!
            </button>
        </div>
		<div class="joke" v-show="joke" key="1">
			<p>{{ joke }}</p>
		</div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent(
{
    name: 'Head',

    data()
    {
        return {
            selectedCategory: "" as string,
			joke: "" as string,
			categories: [] as string[]
        }
    },

	mounted()
	{
		this.getCategories();
	},
    
    methods: {
		getCategories()
		{
			axios.get('https://api.chucknorris.io/jokes/categories').then(res => {
				this.categories = res.data;
				console.log(this.categories[0]);
			});
		},

        getJoke(category: string)
        {
            if (category == "" || category == "all")
            {
				console.log(category);
                axios.get('https://api.chucknorris.io/jokes/random').then(res => {
					console.log(res.data);
					this.joke = res.data['value'];
					console.log(this.joke);
				});
            }
			else
			{
				axios.get(`https://api.chucknorris.io/jokes/random?category=${category}`).then(res => {
					this.joke = res.data['value'];
				});
			}
        }
    }
});

</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Akronim&family=Comfortaa:wght@300&display=swap');

.wrapper {
    padding: 5px;
    width: 500px;
    margin: auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

h1 {
	font-family: 'Akronim', cursive;
	font-size: 44px;
}

.func {
    width: 350px;
    display: flex;
    justify-content: space-around;
}

.sl {
    width: 200px;
	background-color: #ede5ea;
	border: 1px solid #e6e1e4;
	border-radius: 5px;
	font-size: 16px;
	padding: 4px 8px;
	cursor: pointer;
}

.btn {
	background-color: #2ab261;
	border: none;
	border-radius: 5px;
	color: #fff;
	padding: 5px 10px;
	text-align: center;
	text-decoration: none;
	font-size: 16px;
	display: inline-block;
	cursor: pointer;
}

.joke {
	margin-top: 20px;
	padding: 4px;
	padding-left: 10px;
	text-align: left;
	width: 500px;
	min-height: 100px;
	border-radius: 5px;
	box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
	font-family: 'Comfortaa', cursive;
	transition: height 1s;
}

</style>
