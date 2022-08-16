<template lang="">
	<nav class="navbar navbar-expand-lg bg-light">
		<div class="container-fluid">
			<router-link class="navbar-brand" :to="{ name: 'Home' }"
				>DKU19JAM</router-link
			>

			<ul class="navbar-nav me-auto mb-2 mb-lg-0">
				<li class="nav-item">
					<router-link class="nav-link active" :to="{ name: 'Todos' }"
						>Todos</router-link
					>
				</li>
			</ul>
		</div>
	</nav>
</template>
<script>
export default {};
</script>
<style lang=""></style>
