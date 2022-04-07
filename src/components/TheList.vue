<template>
    <ul class="list">
        <li class="list-item" v-for="task in list" :key="task.id" v-show="(task.active && filter !== 'Finished') || (!task.active && filter !== 'Active')">
            <div class="card-item">
                <p :class="task.active ? 'strong' : 'done'" @click="$emit('finish', task.id)">{{task.text}}</p>
                <button class="btn danger" @click="$emit('remove', task.id)">Удалить</button>
            </div>
        </li>
    </ul>
</template>

<script>
export default {
    props: ['list', 'filter'],
    emits: ['finish', 'remove']
}
</script>