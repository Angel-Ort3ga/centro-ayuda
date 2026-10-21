<template>

    <div class="llamada-form">

        <!-- =========================================================
             ENCABEZADO
        ========================================================== -->

        <div class="llamada-form-header">

            <div class="header-content">

                <div class="header-icon">
                    <i class="fas fa-phone-alt"></i>
                </div>

                <div>
                    <h2>Registrar nueva llamada</h2>

                    <p>
                        Captura la información del ciudadano y canaliza
                        correctamente la llamada.
                    </p>
                </div>

            </div>

            <button
                type="button"
                class="btn-close-form"
                @click="cancelar"
                :disabled="guardando"
                title="Cerrar"
            >
                <i class="fas fa-times"></i>
            </button>

        </div>


        <!-- =========================================================
             PASOS
        ========================================================== -->

        <div class="llamada-steps">

            <div
                v-for="paso in pasos"
                :key="paso.numero"
                class="llamada-step"
                :class="{
                    'active': pasoActual === paso.numero,
                    'completed': pasoActual > paso.numero
                }"
            >

                <div class="step-number">

                    <i
                        v-if="pasoActual > paso.numero"
                        class="fas fa-check"
                    ></i>

                    <span v-else>
                        {{ paso.numero }}
                    </span>

                </div>

                <div class="step-info">

                    <span class="step-title">
                        {{ paso.titulo }}
                    </span>

                    <span class="step-description">
                        {{ paso.descripcion }}
                    </span>

                </div>

            </div>

        </div>


        <!-- =========================================================
             ERROR GENERAL
        ========================================================== -->

        <div
            v-if="error"
            class="llamada-error"
        >

            <div class="error-icon">
                <i class="fas fa-exclamation-circle"></i>
            </div>

            <div>
                <strong>No se pudo registrar la llamada</strong>

                <p>
                    {{ error }}
                </p>
            </div>

        </div>


        <!-- =========================================================
             CONTENIDO
        ========================================================== -->

        <div class="llamada-form-content">


            <!-- =====================================================
                 PASO 1 - CIUDADANO
            ====================================================== -->

            <div
                v-if="pasoActual === 1"
                class="form-step-content"
            >

                <div class="section-heading">

                    <div class="section-icon">
                        <i class="fas fa-user"></i>
                    </div>

                    <div>
                        <h3>Datos del ciudadano</h3>

                        <p>
                            Registra los datos básicos de la persona que
                            realiza la llamada.
                        </p>
                    </div>

                </div>


                <div class="form-grid">


                    <!-- =================================================
                         TELÉFONO
                    ================================================== -->

                    <div class="form-group">

                        <label for="telefono">
                            Teléfono
                            <span class="required">*</span>
                        </label>

                        <div
                            class="phone-input"
                            :class="{
                                'has-error': errores.telefono
                            }"
                        >

                            <!-- LADA FIJA -->

                            <span class="country-code">
                                <i class="fas fa-phone"></i>
                                +52
                            </span>


                            <!-- NÚMERO -->

                            <input
                                id="telefono"
                                type="tel"
                                v-model="form.telefono"
                                @input="limpiarTelefono"
                                placeholder="10 dígitos"
                                maxlength="10"
                                minlength="10"
                                inputmode="numeric"
                                autocomplete="tel-national"
                                aria-label="Número telefónico"
                            />

                        </div>

                        <div class="field-hint">
                            <i class="fas fa-info-circle"></i>

                            <span>
                                Ingresa el número telefónico.
                            </span>

                            <span class="phone-counter">
                                {{ form.telefono.length }}/10
                            </span>
                        </div>

                        <span
                            v-if="errores.telefono"
                            class="field-error"
                        >
                            {{ errores.telefono }}
                        </span>

                    </div>


                    <!-- =================================================
                         NOMBRE(S)
                    ================================================== -->

                    <div class="form-group">

                        <label for="nombre">
                            Nombre(s)
                            <span class="required">*</span>
                        </label>

                        <div class="input-wrapper">

                            <i class="fas fa-user"></i>

                            <input
                                id="nombre"
                                type="text"
                                v-model.trim="form.nombre"
                                @input="limpiarNombre"
                                placeholder="Nombres"
                                maxlength="100"
                                autocomplete="given-name"
                                :class="{
                                    'has-error': errores.nombre
                                }"
                            />

                        </div>

                        <div class="field-hint">
                            <i class="fas fa-info-circle"></i>

                            <span>
                                Ingresa el nombre.
                            </span>
                        </div>
 
                        <span
                            v-if="errores.nombre"
                            class="field-error"
                        >
                            {{ errores.nombre }}
                        </span>

                    </div>


                    <!-- =================================================
                         APELLIDOS
                    ================================================== -->

                    <div class="form-group">

                        <label for="apellidos">
                            Apellidos
                            <span class="required">*</span>
                        </label>

                        <div class="input-wrapper">

                            <i class="fas fa-user-tag"></i>

                            <input
                                id="apellidos"
                                type="text"
                                v-model.trim="form.apellidos"
                                @input="limpiarApellidos"
                                placeholder="Apellidos"
                                maxlength="155"
                                autocomplete="family-name"
                                :class="{
                                    'has-error': errores.apellidos
                                }"
                            />

                        </div>

                        <div class="field-hint">
                            <i class="fas fa-info-circle"></i>

                            <span>
                                Ingresa los apellidos.
                            </span>
                        </div>

                        <span
                            v-if="errores.apellidos"
                            class="field-error"
                        >
                            {{ errores.apellidos }}
                        </span>

                    </div>

                </div>


                <!-- =====================================================
                     INFORMACIÓN
                ====================================================== -->

                <div class="step-information-card">

                    <div class="information-icon">
                        <i class="fas fa-lightbulb"></i>
                    </div>

                    <div>
                        <strong>Información importante</strong>

                        <p>
                            El folio, fecha, hora, usuario y estado de la
                            llamada serán generados automáticamente por
                            el sistema.
                        </p>
                    </div>

                </div>

            </div>


            <!-- =====================================================
                 PASO 2 - INFORMACIÓN
            ====================================================== -->

            <div
                v-if="pasoActual === 2"
                class="form-step-content"
            >

                <div class="section-heading">

                    <div class="section-icon">
                        <i class="fas fa-comments"></i>
                    </div>

                    <div>
                        <h3>Información de la llamada</h3>

                        <p>
                            Registra el motivo y clasifica correctamente
                            la solicitud.
                        </p>
                    </div>

                </div>


                <div class="form-grid form-grid-single">


                    <!-- =================================================
                         MOTIVO
                    ================================================== -->

                    <div class="form-group">

                        <div class="label-row">

                            <label for="motivo">
                                Motivo de la llamada
                                <span class="required">*</span>
                            </label>

                            <span class="character-counter">
                                {{ form.motivo.length }}/1000
                            </span>

                        </div>

                        <textarea
                            id="motivo"
                            v-model.trim="form.motivo"
                            rows="5"
                            maxlength="1000"
                            placeholder="Describe brevemente el motivo de la llamada..."
                            :class="{
                                'has-error': errores.motivo
                            }"
                        ></textarea>

                        <span
                            v-if="errores.motivo"
                            class="field-error"
                        >
                            {{ errores.motivo }}
                        </span>

                    </div>


                    <!-- =================================================
                         CATEGORÍA
                    ================================================== -->

                    <div class="form-group">

                        <label>
                            Categoría
                            <span class="required">*</span>
                        </label>

                        <div class="category-options">


                            <!-- INFORMACIÓN -->

                            <button
                                type="button"
                                class="category-option"
                                :class="{
                                    selected:
                                        form.categoria === 'informacion'
                                }"
                                @click="seleccionarCategoria('informacion')"
                            >

                                <span
                                    class="category-icon information"
                                >
                                    <i class="fas fa-info-circle"></i>
                                </span>

                                <span class="category-text">

                                    <strong>
                                        Información
                                    </strong>

                                    <small>
                                        Consulta general
                                    </small>

                                </span>

                                <span
                                    v-if="
                                        form.categoria === 'informacion'
                                    "
                                    class="category-check"
                                >
                                    <i class="fas fa-check"></i>
                                </span>

                            </button>


                            <!-- QUEJA -->

                            <button
                                type="button"
                                class="category-option"
                                :class="{
                                    selected:
                                        form.categoria === 'queja'
                                }"
                                @click="seleccionarCategoria('queja')"
                            >

                                <span
                                    class="category-icon complaint"
                                >
                                    <i class="fas fa-comment-alt"></i>
                                </span>

                                <span class="category-text">

                                    <strong>
                                        Queja
                                    </strong>

                                    <small>
                                        Inconformidad
                                    </small>

                                </span>

                                <span
                                    v-if="
                                        form.categoria === 'queja'
                                    "
                                    class="category-check"
                                >
                                    <i class="fas fa-check"></i>
                                </span>

                            </button>


                            <!-- TRÁMITE -->

                            <button
                                type="button"
                                class="category-option"
                                :class="{
                                    selected:
                                        form.categoria === 'tramite'
                                }"
                                @click="seleccionarCategoria('tramite')"
                            >

                                <span
                                    class="category-icon procedure"
                                >
                                    <i class="fas fa-file-alt"></i>
                                </span>

                                <span class="category-text">

                                    <strong>
                                        Trámite
                                    </strong>

                                    <small>
                                        Gestión o procedimiento
                                    </small>

                                </span>

                                <span
                                    v-if="
                                        form.categoria === 'tramite'
                                    "
                                    class="category-check"
                                >
                                    <i class="fas fa-check"></i>
                                </span>

                            </button>


                            <!-- SOPORTE -->

                            <button
                                type="button"
                                class="category-option"
                                :class="{
                                    selected:
                                        form.categoria === 'soporte'
                                }"
                                @click="seleccionarCategoria('soporte')"
                            >

                                <span
                                    class="category-icon support"
                                >
                                    <i class="fas fa-headset"></i>
                                </span>

                                <span class="category-text">

                                    <strong>
                                        Soporte
                                    </strong>

                                    <small>
                                        Ayuda o asistencia
                                    </small>

                                </span>

                                <span
                                    v-if="
                                        form.categoria === 'soporte'
                                    "
                                    class="category-check"
                                >
                                    <i class="fas fa-check"></i>
                                </span>

                            </button>

                        </div>

                        <span
                            v-if="errores.categoria"
                            class="field-error"
                        >
                            {{ errores.categoria }}
                        </span>

                    </div>

                </div>

            </div>


            <!-- =====================================================
                 PASO 3 - DEPARTAMENTO
            ====================================================== -->

            <div
                v-if="pasoActual === 3"
                class="form-step-content"
            >

                <div class="section-heading">

                   

                    <div>
                        <h3>Destino de la llamada</h3>

                        <p>
                            Selecciona el departamento responsable de
                            atender la solicitud.
                        </p>
                    </div>

                </div>


                <!-- CARGANDO -->

                <div
                    v-if="cargandoDepartamentos"
                    class="department-loading"
                >

                    <div class="loading-spinner">
                        <i class="fas fa-spinner fa-spin"></i>
                    </div>

                    <div>

                        <strong>
                            Cargando departamentos...
                        </strong>

                        <p>
                            Estamos obteniendo los departamentos
                            disponibles.
                        </p>

                    </div>

                </div>


                <!-- SELECT -->

                <div
                    v-else
                    class="form-group"
                >

                    <label for="departamento_id">
                        Departamento
                        <span class="required">*</span>
                    </label>

                    <div class="select-wrapper">

                        <i class="fas fa-building"></i>

                        <select
                            id="departamento_id"
                            v-model="form.departamento_id"
                            :class="{
                                'has-error':
                                    errores.departamento_id
                            }"
                        >

                            <option value="">
                                Selecciona un departamento
                            </option>

                            <option
                                v-for="departamento in departamentos"
                                :key="departamento.id"
                                :value="departamento.id"
                            >
                                {{ departamento.nombre }}
                            </option>

                        </select>

                        <i
                            class="fas fa-chevron-down select-arrow"
                        ></i>

                    </div>

                    <span
                        v-if="errores.departamento_id"
                        class="field-error"
                    >
                        {{ errores.departamento_id }}
                    </span>

                </div>


                <!-- SIN DEPARTAMENTOS -->

                <div
                    v-if="
                        !cargandoDepartamentos &&
                        departamentos.length === 0
                    "
                    class="department-empty"
                >

                    <div class="empty-icon">
                        <i class="fas fa-building"></i>
                    </div>

                    <div>

                        <strong>
                            No hay departamentos disponibles
                        </strong>

                        <p>
                            Verifica que existan departamentos activos
                            registrados en el sistema.
                        </p>

                    </div>

                </div>


                <!-- DEPARTAMENTO SELECCIONADO -->

                <div
                    v-if="departamentoSeleccionado"
                    class="department-selected"
                >

                    <div class="department-selected-header">

                        <div class="department-main-icon">
                            <i class="fas fa-building"></i>
                        </div>

                        <div>

                            <span class="department-label">
                                Departamento seleccionado
                            </span>

                            <h4>
                                {{ nombreDepartamento }}
                            </h4>

                        </div>

                    </div>


                    <div class="department-details">

                        <div
                            v-if="
                                departamentoSeleccionado.extension
                            "
                            class="department-detail"
                        >

                            <span class="detail-icon">
                                <i class="fas fa-phone"></i>
                            </span>

                            <div>

                                <small>
                                    Extensión
                                </small>

                                <strong>
                                    {{
                                        departamentoSeleccionado.extension
                                    }}
                                </strong>

                            </div>

                        </div>


                        <div
                            v-if="
                                departamentoSeleccionado.telefono
                            "
                            class="department-detail"
                        >

                            <span class="detail-icon">
                                <i class="fas fa-mobile-alt"></i>
                            </span>

                            <div>

                                <small>
                                    Teléfono
                                </small>

                                <strong>
                                    {{
                                        departamentoSeleccionado.telefono
                                    }}
                                </strong>

                            </div>

                        </div>


                        <div
                            v-if="
                                departamentoSeleccionado.correo
                            "
                            class="department-detail"
                        >

                            <span class="detail-icon">
                                <i class="fas fa-envelope"></i>
                            </span>

                            <div>

                                <small>
                                    Correo
                                </small>

                                <strong>
                                    {{
                                        departamentoSeleccionado.correo
                                    }}
                                </strong>

                            </div>

                        </div>

                    </div>

                </div>

            </div>


            <!-- =====================================================
                 PASO 4 - OBSERVACIONES / RESUMEN
            ====================================================== -->

            <div
                v-if="pasoActual === 4"
                class="form-step-content"
            >

                <div class="section-heading">

                    <div class="section-icon">
                        <i class="fas fa-clipboard-check"></i>
                    </div>

                    <div>

                        <h3>
                            Revisión y registro
                        </h3>

                        <p>
                            Verifica la información antes de registrar
                            la llamada.
                        </p>

                    </div>

                </div>


                <!-- RESUMEN -->

                <div class="call-summary">

                    <div class="summary-header">

                        <div class="summary-icon">
                            <i class="fas fa-phone-alt"></i>
                        </div>

                        <div>

                            <strong>
                                Resumen de la llamada
                            </strong>

                            <span>
                                Revisa los datos capturados.
                            </span>

                        </div>

                    </div>


                    <div class="summary-grid">


                        <!-- NOMBRE COMPLETO -->

                        <div class="summary-item">

                            <span class="summary-label">
                                <i class="fas fa-user"></i>
                                Ciudadano
                            </span>

                            <strong>
                                {{
                                    nombreCompleto ||
                                    'No especificado'
                                }}
                            </strong>

                        </div>


                        <!-- TELÉFONO -->

                        <div class="summary-item">

                            <span class="summary-label">
                                <i class="fas fa-phone"></i>
                                Teléfono
                            </span>

                            <strong>
                                +52
                                {{
                                    form.telefono ||
                                    'No especificado'
                                }}
                            </strong>

                        </div>


                        <!-- CATEGORÍA -->

                        <div class="summary-item">

                            <span class="summary-label">
                                <i class="fas fa-tag"></i>
                                Categoría
                            </span>

                            <strong>
                                {{
                                    categoriaTexto ||
                                    'No especificada'
                                }}
                            </strong>

                        </div>


                        <!-- DEPARTAMENTO -->

                        <div class="summary-item">

                            <span class="summary-label">
                                <i class="fas fa-building"></i>
                                Departamento
                            </span>

                            <strong>
                                {{
                                    nombreDepartamento ||
                                    'No seleccionado'
                                }}
                            </strong>

                        </div>


                        <!-- MOTIVO -->

                        <div
                            class="summary-item summary-item-full"
                        >

                            <span class="summary-label">
                                <i class="fas fa-comment"></i>
                                Motivo
                            </span>

                            <p>
                                {{
                                    form.motivo ||
                                    'No especificado'
                                }}
                            </p>

                        </div>

                    </div>

                </div>


                <!-- OBSERVACIONES -->

                <div
                    class="form-group observations-group"
                >

                    <div class="label-row">

                        <label for="observaciones">

                            Observaciones

                            <span class="optional">
                                Opcional
                            </span>

                        </label>

                        <span class="character-counter">
                            {{ form.observaciones.length }}/2000
                        </span>

                    </div>

                    <textarea
                        id="observaciones"
                        v-model.trim="form.observaciones"
                        rows="5"
                        maxlength="2000"
                        placeholder="Agrega información adicional que pueda ser útil para el seguimiento..."
                        :class="{
                            'has-error':
                                errores.observaciones
                        }"
                    ></textarea>

                    <span
                        v-if="errores.observaciones"
                        class="field-error"
                    >
                        {{ errores.observaciones }}
                    </span>

                </div>

            </div>

        </div>


        <!-- =========================================================
             FOOTER
        ========================================================== -->

        <div class="llamada-form-footer">

            <div class="footer-left">

                <button
                    v-if="pasoActual > 1"
                    type="button"
                    class="btn-secondary"
                    @click="pasoAnterior"
                    :disabled="guardando"
                >
                    <i class="fas fa-arrow-left"></i>
                    Anterior
                </button>

            </div>


            <div class="footer-right">

                <button
                    type="button"
                    class="btn-cancel"
                    @click="cancelar"
                    :disabled="guardando"
                >
                    Cancelar
                </button>


                <button
                    v-if="pasoActual < pasos.length"
                    type="button"
                    class="btn-primary"
                    @click="siguientePaso"
                    :disabled="guardando"
                >

                    Continuar

                    <i class="fas fa-arrow-right"></i>

                </button>


                <button
                    v-else
                    type="button"
                    class="btn-success"
                    @click="guardar"
                    :disabled="guardando"
                >

                    <i
                        v-if="guardando"
                        class="fas fa-spinner fa-spin"
                    ></i>

                    <i
                        v-else
                        class="fas fa-save"
                    ></i>

                    {{
                        guardando
                            ? 'Registrando...'
                            : 'Registrar llamada'
                    }}

                </button>

            </div>

        </div>

    </div>

</template>


<script>

import axios from 'axios';

export default {

    name: 'LlamadaFormModal',


    data() {

        return {

            pasoActual: 1,


            pasos: [

                {
                    numero: 1,
                    titulo: 'Ciudadano',
                    descripcion: 'Datos básicos'
                },

                {
                    numero: 2,
                    titulo: 'Información',
                    descripcion: 'Motivo y categoría'
                },

                {
                    numero: 3,
                    titulo: 'Departamento',
                    descripcion: 'Destino de la llamada'
                },

                {
                    numero: 4,
                    titulo: 'Observaciones',
                    descripcion: 'Revisión y registro'
                }

            ],


            form: {

                telefono: '',

                nombre: '',

                apellidos: '',

                motivo: '',

                categoria: '',

                departamento_id: '',

                observaciones: ''

            },


            departamentos: [],

            cargandoDepartamentos: false,

            guardando: false,

            errores: {},

            error: null

        };

    },


    computed: {

        /* =========================================================
           DEPARTAMENTO SELECCIONADO
        ========================================================== */

        departamentoSeleccionado() {

            if (
                !this.form.departamento_id ||
                !Array.isArray(this.departamentos)
            ) {
                return null;
            }


            return this.departamentos.find(
                departamento => {

                    if (
                        !departamento ||
                        departamento.id === null ||
                        departamento.id === undefined
                    ) {
                        return false;
                    }


                    return String(departamento.id) ===
                        String(this.form.departamento_id);

                }
            ) || null;

        },


        /* =========================================================
           NOMBRE DEL DEPARTAMENTO
        ========================================================== */

        nombreDepartamento() {

            if (
                !this.departamentoSeleccionado ||
                !this.departamentoSeleccionado.nombre
            ) {
                return '';
            }


            return this.departamentoSeleccionado.nombre;

        },


        /* =========================================================
           NOMBRE COMPLETO
        ========================================================== */

        nombreCompleto() {

            return [
                this.form.nombre,
                this.form.apellidos
            ]
                .filter(valor => valor && valor.trim())
                .join(' ')
                .trim();

        },


        /* =========================================================
           CATEGORÍA
        ========================================================== */

        categoriaTexto() {

            const categorias = {

                informacion: 'Información',

                queja: 'Queja',

                tramite: 'Trámite',

                soporte: 'Soporte'

            };


            return categorias[this.form.categoria] || '';

        }

    },


    mounted() {

        this.cargarDepartamentos();

    },


    methods: {

        /* =========================================================
           TELÉFONO
        ========================================================== */

        limpiarTelefono(event) {

            let valor = event.target.value || '';


            /*
             * Solo números.
             * Elimina letras, espacios, guiones,
             * paréntesis y cualquier otro carácter.
             */

            valor = valor.replace(/\D/g, '');


            /*
             * Máximo 10 dígitos.
             */

            valor = valor.substring(0, 10);


            this.form.telefono = valor;

        },


        /* =========================================================
           NOMBRE
        ========================================================== */

        limpiarNombre(event) {

            let valor = event.target.value || '';


            /*
             * Permite letras, espacios, acentos y ñ.
             * No permite números ni símbolos extraños.
             */

            valor = valor.replace(
                /[^A-Za-zÁÉÍÓÚáéíóúÑñÜü\s]/g,
                ''
            );


            /*
             * Evita espacios repetidos.
             */

            valor = valor.replace(/\s{2,}/g, ' ');


            this.form.nombre = valor;

        },


        /* =========================================================
           APELLIDOS
        ========================================================== */

        limpiarApellidos(event) {

            let valor = event.target.value || '';


            valor = valor.replace(
                /[^A-Za-zÁÉÍÓÚáéíóúÑñÜü\s]/g,
                ''
            );


            valor = valor.replace(/\s{2,}/g, ' ');


            this.form.apellidos = valor;

        },


        /* =========================================================
           CATEGORÍA
        ========================================================== */

        seleccionarCategoria(categoria) {

            this.form.categoria = categoria;


            if (this.errores.categoria) {

                this.$delete(
                    this.errores,
                    'categoria'
                );

            }

        },


        /* =========================================================
           CARGAR DEPARTAMENTOS
        ========================================================== */

        async cargarDepartamentos() {

            this.cargandoDepartamentos = true;


            try {

                const response = await axios.get(
                    '/api/admin/departamentos'
                );


                let departamentos = [];


                if (Array.isArray(response.data)) {

                    departamentos = response.data;

                } else if (
                    response.data &&
                    Array.isArray(response.data.data)
                ) {

                    departamentos =
                        response.data.data;

                } else if (
                    response.data &&
                    response.data.data &&
                    Array.isArray(
                        response.data.data.data
                    )
                ) {

                    departamentos =
                        response.data.data.data;

                }


                this.departamentos =
                    departamentos.filter(
                        departamento => {

                            return (
                                departamento &&
                                typeof departamento ===
                                    'object' &&
                                departamento.id !== null &&
                                departamento.id !== undefined
                            );

                        }
                    );


            } catch (error) {

                console.error(
                    'Error al cargar departamentos:',
                    error
                );


                this.departamentos = [];


                if (error.response) {

                    if (
                        error.response.status === 401
                    ) {

                        this.error =
                            'Tu sesión ha expirado. Inicia sesión nuevamente.';

                    } else if (
                        error.response.status === 403
                    ) {

                        this.error =
                            'No tienes permisos para consultar los departamentos.';

                    } else {

                        this.error =
                            'No fue posible cargar los departamentos.';

                    }

                } else {

                    this.error =
                        'No fue posible conectar con el servidor.';

                }


            } finally {

                this.cargandoDepartamentos = false;

            }

        },


        /* =========================================================
           SIGUIENTE PASO
        ========================================================== */

        siguientePaso() {

            if (!this.validarPasoActual()) {
                return;
            }


            if (
                this.pasoActual <
                this.pasos.length
            ) {

                this.pasoActual++;

                this.error = null;

            }

        },


        /* =========================================================
           PASO ANTERIOR
        ========================================================== */

        pasoAnterior() {

            if (this.pasoActual > 1) {

                this.pasoActual--;

                this.error = null;

            }

        },


        /* =========================================================
           VALIDAR PASO ACTUAL
        ========================================================== */

        validarPasoActual() {

            this.errores = {};

            let valido = true;


            /* -----------------------------------------
               PASO 1
            ------------------------------------------ */

            if (this.pasoActual === 1) {


                if (!this.form.telefono) {

                    this.errores.telefono =
                        'El teléfono es obligatorio.';

                    valido = false;

                } else if (
                    !/^\d{10}$/.test(
                        this.form.telefono
                    )
                ) {

                    this.errores.telefono =
                        'El teléfono debe contener exactamente 10 dígitos.';

                    valido = false;

                }


                if (!this.form.nombre) {

                    this.errores.nombre =
                        'El nombre es obligatorio.';

                    valido = false;

                } else if (
                    this.form.nombre.trim().length < 2
                ) {

                    this.errores.nombre =
                        'Ingresa un nombre válido.';

                    valido = false;

                }


                if (!this.form.apellidos) {

                    this.errores.apellidos =
                        'Los apellidos son obligatorios.';

                    valido = false;

                } else if (
                    this.form.apellidos.trim().length < 2
                ) {

                    this.errores.apellidos =
                        'Ingresa los apellidos.';

                    valido = false;

                }

            }


            /* -----------------------------------------
               PASO 2
            ------------------------------------------ */

            if (this.pasoActual === 2) {

                if (!this.form.motivo) {

                    this.errores.motivo =
                        'El motivo de la llamada es obligatorio.';

                    valido = false;

                }


                if (!this.form.categoria) {

                    this.errores.categoria =
                        'Selecciona una categoría.';

                    valido = false;

                }

            }


            /* -----------------------------------------
               PASO 3
            ------------------------------------------ */

            if (this.pasoActual === 3) {

                if (!this.form.departamento_id) {

                    this.errores.departamento_id =
                        'Selecciona un departamento.';

                    valido = false;

                }

            }


            return valido;

        },


        /* =========================================================
           PASO COMPLETADO
        ========================================================== */

        pasoCompletado(numeroPaso) {

            if (numeroPaso === 1) {

                return (
                    !!this.form.telefono &&
                    !!this.form.nombre &&
                    !!this.form.apellidos
                );

            }


            if (numeroPaso === 2) {

                return (
                    !!this.form.motivo &&
                    !!this.form.categoria
                );

            }


            if (numeroPaso === 3) {

                return !!this.form.departamento_id;

            }


            if (numeroPaso === 4) {

                return true;

            }


            return false;

        },


        /* =========================================================
           GUARDAR
        ========================================================== */

        async guardar() {

            if (!this.validarTodosLosPasos()) {
                return;
            }


            this.guardando = true;

            this.error = null;


            try {


                /*
                 * El backend actualmente utiliza un solo
                 * campo "nombre".
                 *
                 * Aquí combinamos:
                 *
                 * nombre + apellidos
                 */

                const nombreCompleto =
                    this.nombreCompleto;


                const payload = {

                    telefono:
                        this.form.telefono,

                    nombre:
                        nombreCompleto,

                    motivo:
                        this.form.motivo,

                    categoria:
                        this.form.categoria,

                    departamento_id:
                        this.form.departamento_id,

                    observaciones:
                        this.form.observaciones || null

                };


                console.log(
                    'Payload de nueva llamada:',
                    payload
                );


                const response =
                    await axios.post(
                        '/api/admin/llamadas',
                        payload
                    );


                this.$emit(
                    'guardado',
                    response.data
                );


                this.resetear();


            } catch (error) {

                console.error(
                    'Error al registrar llamada:',
                    error
                );


                if (
                    error.response &&
                    error.response.status === 422
                ) {

                    this.errores =
                        error.response.data.errors ||
                        {};

                    this.error =
                        error.response.data.message ||
                        'Revisa los datos ingresados.';


                    this.pasoActual =
                        this.detectarPasoConError();


                } else if (
                    error.response &&
                    error.response.status === 401
                ) {

                    this.error =
                        'Tu sesión ha expirado. Inicia sesión nuevamente.';


                } else if (
                    error.response &&
                    error.response.status === 403
                ) {

                    this.error =
                        'No tienes permisos para registrar llamadas.';


                } else {

                    this.error =
                        'Ocurrió un error al registrar la llamada.';

                }


            } finally {

                this.guardando = false;

            }

        },


        /* =========================================================
           VALIDAR TODO
        ========================================================== */

        validarTodosLosPasos() {

            this.errores = {};

            let valido = true;


            /* TELÉFONO */

            if (!this.form.telefono) {

                this.errores.telefono =
                    'El teléfono es obligatorio.';

                valido = false;

            } else if (
                !/^\d{10}$/.test(
                    this.form.telefono
                )
            ) {

                this.errores.telefono =
                    'El teléfono debe contener exactamente 10 dígitos.';

                valido = false;

            }


            /* NOMBRE */

            if (!this.form.nombre) {

                this.errores.nombre =
                    'El nombre es obligatorio.';

                valido = false;

            }


            /* APELLIDOS */

            if (!this.form.apellidos) {

                this.errores.apellidos =
                    'Los apellidos son obligatorios.';

                valido = false;

            }


            /* MOTIVO */

            if (!this.form.motivo) {

                this.errores.motivo =
                    'El motivo de la llamada es obligatorio.';

                valido = false;

            }


            /* CATEGORÍA */

            if (!this.form.categoria) {

                this.errores.categoria =
                    'Selecciona una categoría.';

                valido = false;

            }


            /* DEPARTAMENTO */

            if (!this.form.departamento_id) {

                this.errores.departamento_id =
                    'Selecciona un departamento.';

                valido = false;

            }


            if (!valido) {

                this.pasoActual =
                    this.detectarPasoConError();

            }


            return valido;

        },


        /* =========================================================
           DETECTAR PASO CON ERROR
        ========================================================== */

        detectarPasoConError() {

            if (
                this.errores.telefono ||
                this.errores.nombre ||
                this.errores.apellidos
            ) {

                return 1;

            }


            if (
                this.errores.motivo ||
                this.errores.categoria
            ) {

                return 2;

            }


            if (
                this.errores.departamento_id
            ) {

                return 3;

            }


            return this.pasoActual;

        },


        /* =========================================================
           CANCELAR
        ========================================================== */

        cancelar() {

            if (this.guardando) {
                return;
            }


            this.$emit('cancelar');

        },


        /* =========================================================
           RESET
        ========================================================== */

        resetear() {

            this.pasoActual = 1;


            this.form = {

                telefono: '',

                nombre: '',

                apellidos: '',

                motivo: '',

                categoria: '',

                departamento_id: '',

                observaciones: ''

            };


            this.errores = {};

            this.error = null;

        }

    }

};

</script>