<template>
	<div>
		<h1> {{book.name.toUpperCase()}} </h1>
		<h2> {{book.author}} </h2>
		<div v> </div>
	</div>
</template>

<script>
export default {
	name: 'MyComponent',
	data(){
		return {
			book: {
				name: "Example for a D3 Graph",
				author: "Richard Uribe"
			}
		}
	}
}
</script>

<style>

</style>