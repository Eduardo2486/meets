<script>
    import {slide} from 'svelte/transition';
</script>
<style>
    span{
        display: inline;
        margin: 0 0.25rem;
        border-radius: 3px;
        border: 1px solid #cf0056;
        background: #cf0056;
        padding: 0 0.50rem;
    }
</style>
<span transition:slide><slot></slot></span>