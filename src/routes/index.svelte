<script lang="ts">
    import Auth from "../components/Auth.svelte";
</script>

<div class="page-container">
    <h1>Gaa Game Management</h1>
    <p>Please log in</p>

    <Auth on:auth={() => (window.location.href = "/live")} />
</div>
