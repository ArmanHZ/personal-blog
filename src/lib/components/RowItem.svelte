<style lang="scss">

    .row-item {
        border-left: 0.3rem solid;
        border-image-slice: 1;
        border-image-source: linear-gradient(to bottom, #ef9af2, #6df1d8, #b8aec8);
        padding-left: 1rem;
    }

    p {
        color: rgb(248, 248, 255, 0.9);
    }

    span {
        color: #ef9af2;
    }

</style>

<script lang="ts">

    export let title: string;
    export let link = "#"
    export let content: string;

</script>

<div class="row-item">
    <h2><span>{"> "}</span><a href={link}>{title}</a></h2>
    <p>{content}</p>
</div>
