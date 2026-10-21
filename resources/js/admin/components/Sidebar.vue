<template>
    <aside
        :class="[
            'sidebar',
            {
                'sidebar-collapsed': colapsado,
            },
        ]"
    >

        <!-- ===================================================== -->
        <!-- ENCABEZADO                                            -->
        <!-- ===================================================== -->

        <div class="sidebar-header">

            <div class="brand-area">

                <div class="brand-icon">
                    <i class="fas fa-headset"></i>
                </div>

                <div
                    v-if="!colapsado"
                    class="brand-text"
                >
                    <h1>
                        Call Center
                    </h1>

                    <p>
                        Sistema de gestión
                    </p>
                </div>

            </div>

            <!-- BOTÓN CONTRAER / EXPANDIR -->

            <button
                type="button"
                class="sidebar-toggle"
                :title="colapsado ? 'Expandir menú' : 'Contraer menú'"
                @click="alternarSidebar"
            >
                <i
                    :class="[
                        'fas',
                        colapsado
                            ? 'fa-chevron-right'
                            : 'fa-chevron-left'
                    ]"
                ></i>
            </button>

        </div>


        <!-- ===================================================== -->
        <!-- USUARIO                                               -->
        <!-- ===================================================== -->

        <div class="sidebar-user">

            <div class="user-avatar">
                {{ iniciales }}
            </div>

            <div
                v-if="!colapsado"
                class="user-info"
            >
                <p class="user-name">
                    {{ nombreUsuario }}
                </p>

                <p class="user-role">
                    {{ nombreRol }}
                </p>
            </div>

        </div>


        <!-- ===================================================== -->
        <!-- NAVEGACIÓN                                            -->
        <!-- ===================================================== -->

        <nav class="sidebar-nav">

            <!-- Dashboard -->

            <router-link
                v-if="puedeVerDashboard"
                to="/dashboard"
                class="sidebar-link"
                active-class="active"
                title="Dashboard"
            >
                <i class="fas fa-chart-line sidebar-link-icon"></i>

                <span
                    v-if="!colapsado"
                    class="sidebar-link-text"
                >
                    Dashboard
                </span>
            </router-link>


            <!-- Llamadas -->

            <router-link
                v-if="puedeVerLlamadas"
                to="/llamadas"
                class="sidebar-link"
                active-class="active"
                title="Llamadas"
            >
                <i class="fas fa-phone-alt sidebar-link-icon"></i>

                <span
                    v-if="!colapsado"
                    class="sidebar-link-text"
                >
                    Llamadas
                </span>
            </router-link>


            <!-- Departamentos -->

            <router-link
                v-if="puedeVerDepartamentos"
                to="/departamentos"
                class="sidebar-link"
                active-class="active"
                title="Departamentos"
            >
                <i class="fas fa-building sidebar-link-icon"></i>

                <span
                    v-if="!colapsado"
                    class="sidebar-link-text"
                >
                    Departamentos
                </span>
            </router-link>


            <!-- Mi Departamento -->

            <router-link
                v-if="puedeVerMiDepartamento"
                to="/mi-departamento"
                class="sidebar-link"
                active-class="active"
                title="Mi Departamento"
            >
                <i class="fas fa-sitemap sidebar-link-icon"></i>

                <span
                    v-if="!colapsado"
                    class="sidebar-link-text"
                >
                    Mi Departamento
                </span>
            </router-link>


            <!-- Usuarios -->

            <router-link
                v-if="puedeVerUsuarios"
                to="/usuarios"
                class="sidebar-link"
                active-class="active"
                title="Usuarios"
            >
                <i class="fas fa-users sidebar-link-icon"></i>

                <span
                    v-if="!colapsado"
                    class="sidebar-link-text"
                >
                    Usuarios
                </span>
            </router-link>

        </nav>


        <!-- ===================================================== -->
        <!-- CERRAR SESIÓN                                         -->
        <!-- ===================================================== -->

        <div class="sidebar-footer">

            <button
                type="button"
                class="logout-button"
                title="Cerrar sesión"
                @click="cerrarSesion"
            >
                <i class="fas fa-sign-out-alt"></i>

                <span v-if="!colapsado">
                    Cerrar sesión
                </span>
            </button>

        </div>

    </aside>
</template>


<script>

import {
    getUser,
    cerrarSesion as cerrarSesionAuth,
} from '../../auth/auth';

import './styles/sidebar.css';


export default {

    name: 'Sidebar',


    data() {

        return {

            usuario: getUser(),

            colapsado: false,

        };

    },


    computed: {

        /*
        |--------------------------------------------------------------------------
        | Nombre del usuario
        |--------------------------------------------------------------------------
        */

        nombreUsuario() {

            if (!this.usuario) {

                return 'Usuario';

            }

            return `${this.usuario.nombre} ${this.usuario.apellido}`;

        },


        /*
        |--------------------------------------------------------------------------
        | Iniciales
        |--------------------------------------------------------------------------
        */

        iniciales() {

            if (!this.usuario) {

                return 'U';

            }

            const nombre =
                this.usuario.nombre || '';

            const apellido =
                this.usuario.apellido || '';


            const inicialNombre =
                nombre.charAt(0);

            const inicialApellido =
                apellido.charAt(0);


            return (
                inicialNombre +
                inicialApellido
            ).toUpperCase();

        },


        /*
        |--------------------------------------------------------------------------
        | Nombre del rol
        |--------------------------------------------------------------------------
        */

        nombreRol() {

            const roles = {

                super_admin:
                    'Super Administrador',

                supervisor:
                    'Supervisor',

                jefe_departamento:
                    'Jefe de departamento',

                recepcionista:
                    'Recepcionista',

            };


            return (
                roles[this.usuario?.role] ||
                'Usuario'
            );

        },


        /*
        |--------------------------------------------------------------------------
        | Dashboard
        |--------------------------------------------------------------------------
        */

        puedeVerDashboard() {

            return [

                'super_admin',
                'supervisor',
                'jefe_departamento',
                'recepcionista',

            ].includes(
                this.usuario?.role
            );

        },


        /*
        |--------------------------------------------------------------------------
        | Llamadas
        |--------------------------------------------------------------------------
        */

        puedeVerLlamadas() {

            return [

                'super_admin',
                'supervisor',
                'recepcionista',

            ].includes(
                this.usuario?.role
            );

        },


        /*
        |--------------------------------------------------------------------------
        | Departamentos
        |--------------------------------------------------------------------------
        */

        puedeVerDepartamentos() {

            return [

                'super_admin',
                'supervisor',

            ].includes(
                this.usuario?.role
            );

        },


        /*
        |--------------------------------------------------------------------------
        | Mi Departamento
        |--------------------------------------------------------------------------
        */

        puedeVerMiDepartamento() {

            return [

                'super_admin',
                'supervisor',
                'jefe_departamento',

            ].includes(
                this.usuario?.role
            );

        },


        /*
        |--------------------------------------------------------------------------
        | Usuarios
        |--------------------------------------------------------------------------
        */

        puedeVerUsuarios() {

            return (
                this.usuario?.role ===
                'super_admin'
            );

        },

    },


    methods: {

        /*
        |--------------------------------------------------------------------------
        | Contraer / expandir
        |--------------------------------------------------------------------------
        */

        alternarSidebar() {

            this.colapsado =
                !this.colapsado;

        },


        /*
        |--------------------------------------------------------------------------
        | Cerrar sesión
        |--------------------------------------------------------------------------
        */

        async cerrarSesion() {

            await cerrarSesionAuth();

            this.$router.push('/login');

        },

    },

};

</script>