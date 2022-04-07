<template>
    <div class="row">
        <div v-for="nameButton in namesButtons" :key="nameButton.pvsevdo">
            <button class="btn" :class=" nameButton.pvsevdo === filterStatus ? 'active' : '' "
                    @click="$emit('show', nameButton.pvsevdo)">
                {{ nameButton.name }}
            </button>
        </div>
    </div>
    <hr>
</template>

<script>
export default {
    props: ['namesButtons', 'filterStatus'],
    emits: ['show']
}
</script>