<template>
    <div class="modal">
        <h2>To add another city delete one of the previous ones.</h2>
        <button class="buttons" @click='passEvent("close")'>Got it</button>
    </div>
</template>

<style>
.modal {
    border: 2px solid black;
    border-radius: 10px;
    height: 600px;
    width: 800px;
    position: absolute;
    top: 50%;
    right: 50%;
    transform: translate(50%, -50%);
    background-color: aliceblue;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.buttons {
    background-color: white;
    border: 1px solid black;
    border-radius: 2px;
    height: 40px;
    width: 200px;
    font-size: 20px;
}

.buttons:hover {
    background-color: lightgray;
}
</style>

<script>
export default {
    methods: {
        passEvent(evt) {
            this.$emit("modal2", evt)
        }
    }
}
</script>