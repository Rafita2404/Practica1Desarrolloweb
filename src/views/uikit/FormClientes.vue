<script setup>
import { ref } from 'vue';

const nombre = ref('');
const apellidoPaterno = ref('');
const apellidoMaterno = ref('');
const fechaNacimiento = ref(null);
const rfcGenerado = ref('');

const generarRFC = () => {
    if (!nombre.value || !apellidoPaterno.value || !apellidoMaterno.value || !fechaNacimiento.value) {
        alert('Hace falta el llenado de uno o varios campos para generar el RFC');
        return;
    }

    const apPaterno = apellidoPaterno.value.trim().toUpperCase();
    const apMaterno = apellidoMaterno.value.trim().toUpperCase();
    const nombreUsuario = nombre.value.trim().toUpperCase();
    const fecha = new Date(fechaNacimiento.value);

    const primeraLetraAP = apPaterno.charAt(0);

    let primeraVocalAP = '';
    const vocales = 'AEIOU';
    for (const char of apPaterno.substring(1)) {
        if (vocales.includes(char)) {
            primeraVocalAP = char;
            break;
        }
    }

    const primeraLetraAM = apMaterno.charAt(0);

    const primeraLetraNombre = nombreUsuario.charAt(0);

    const year = String(fecha.getFullYear()).substring(2);
    const mes = String(fecha.getMonth() + 1).padStart(2, '0');
    const dia = String(fecha.getDate()).padStart(2, '0');

    const rfcBasico = `${primeraLetraAP}${primeraVocalAP}${primeraLetraAM}${primeraLetraNombre}${year}${mes}${dia}`;

    rfcGenerado.value = rfcBasico;
};
</script>

<template>
    <Fluid>
        <div class="flex flex-col md:flex-row gap-8">
            <div class="card flex flex-col gap-4 w-full">
                <div class="card flex flex-col gap-4">
                    <div class="font-semibold text-xl">GENERADOR DE RFC PARA UNA PERSONA</div>
                    <div class="flex flex-col gap-2">
                        <label for="name1">NOMBRE</label>
                        <InputText id="name1" type="text" v-model="nombre" />
                    </div>
                    <div>
                        <label for="name2">APELLIDO PATERNO</label>
                        <InputText id="name2" type="text" v-model="apellidoPaterno" />
                    </div>
                    <div>
                        <label for="name3">APELLIDO MATERNO</label>
                        <InputText id="name3" type="text" v-model="apellidoMaterno" />
                    </div>
                    <div class="flex flex-col gap-2">
                        <label for="birthdate">FECHA DE NACIMIENTO</label>
                        <Calendar id="fechaNacimiento" v-model="fechaNacimiento" dateFormat="dd/mm/yy" showIcon />
                    </div>
                    <div>
                        <label for="RFC">RFC</label>
                        <InputText id="RFC" type="text" v-model="rfcGenerado" readonly />
                    </div>
                    <div>
                        <Button label="GENERAR RFC" @click="generarRFC" :fluid="false"></Button>
                    </div>
                </div>
            </div>
        </div>
    </Fluid>
</template>
