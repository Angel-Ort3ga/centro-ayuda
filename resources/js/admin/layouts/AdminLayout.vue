<template>

    <div class="admin-layout">

        <!-- Sidebar -->
        <Sidebar />

        <!-- Contenido principal -->
        <main class="admin-content">

            <div class="admin-content-inner">

                <router-view />

            </div>

        </main>

    </div>

</template>


<script>

import Sidebar from '../components/Sidebar.vue';


export default {

    name: 'AdminLayout',


    components: {

        Sidebar,

    },

};

</script>


<style scoped>

.admin-layout {
    min-height: 100vh;

    display: flex;

    background: #f3f4f6;
}


.admin-content {
    flex: 1;

    min-width: 0;

    min-height: 100vh;
}


.admin-content-inner {
    padding: 24px;
}


@media (max-width: 768px) {

    .admin-content-inner {
        padding: 16px;
    }

}

</style>