<template>
    <div class="modal">
        <h2>Are you sure you want to delete this city?</h2>
        <div>
            <button class="buttons" id="yes" @click='passEvent("yes")'>Yes</button>
            <button class="buttons" id="no" @click='passEvent("no")'>No</button>
        </div>
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

#yes {
    color: green;
}

#no {
    color: red;
    margin-left: 2px;
}
</style>

<script>
export default {
    methods: {
        passEvent(evt) {
            this.$emit("modal", evt)
        }
    }
}
</script>