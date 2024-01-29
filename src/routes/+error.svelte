<script>
	import { page } from '$app/stores';
</script>

<h1>{$page.status}: {$page.error.message}</h1>

<style>
    h1 {
        flex-grow: 3;
        width: 90%;
        text-align: center;
        margin: auto;

        color: red;
    }
</style>
