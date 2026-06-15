<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'

const route = useRoute()
const cui = ref(route.params.cui)
const estudiante = ref('')
const email = ref('')
const matriculas = ref([])
const cargando = ref(true)

// Formatear fecha actual de emisión de forma automática
const obtenerFechaActual = () => {
  const hoy = new Date()
  const dia = String(hoy.getDate()).padStart(2, '0')
  const mes = String(hoy.getMonth() + 1).padStart(2, '0')
  const anio = hoy.getFullYear()
  return `${dia}/${mes}/${anio}`
}

const fechaEmision = obtenerFechaActual()

const cargarDatos = async (codigoCui) => {
  cargando.value = true
  try {
    // Agregamos encabezados para evitar problemas de caché y mejorar la petición cross-origin
    const respuesta = await axios.get(`/api/certificate?cui=${codigoCui}`, {
  headers: {
    'Accept': 'application/json'
  }
})
    
    if (respuesta.data && respuesta.data.results && respuesta.data.results.length > 0) {
      const data = respuesta.data.results[0]
      
      // Validamos exhaustivamente la estructura que devuelve Vercel
      if (data.student) {
        estudiante.value = data.student.full_name || 'Estudiante sin nombre'
        email.value = data.student.email || ''
      } else {
        estudiante.value = 'Datos de estudiante no disponibles'
        email.value = ''
      }

      matriculas.value = respuesta.data.results
    } else {
      // Si la API responde vacío pero sin error (CUI no registrado)
      estudiante.value = ''
      email.value = ''
      matriculas.value = []
    }
  } catch (error) {
    console.error("Error al consumir la API del profesor:", error)
    // Si la API falla por CORS en Netlify, le damos un aviso al usuario en vez de dejarlo vacío
    estudiante.value = 'Error al conectar con el servidor (API)'
    email.value = 'Intente recargar la página'
    matriculas.value = []
  } finally {
    cargando.value = false
  }
}

onMounted(() => {
  cargarDatos(cui.value)
})

watch(() => route.params.cui, (nuevoCui) => {
  cui.value = nuevoCui
  cargarDatos(nuevoCui)
})
</script>

<template>
  <div class="contenedor-principal">
    <div v-if="cargando" class="cargando">Cargando constancia...</div>
    
    <div v-else class="documento-tarjeta">
      <div class="encabezado">
        <h1>CONSTANCIA DE MATRÍCULA DE LABORATORIO</h1>
        <h2>Escuela Profesional de Ingeniería de Sistemas EPIS</h2>
        <p class="fecha">Emitido el: {{ fechaEmision }}</p>
        <p class="autor">Generado por: Deyci Andrea Camargo Ramos</p>
      </div>
      
      <hr class="separador" />

      <div class="seccion-titulo">DATOS DEL ALUMNO</div>
      <div class="datos-alumno-grid">
        <div class="fila-dato">
          <span class="etiqueta">C.U.I.:</span>
          <span class="valor">{{ cui }}</span>
        </div>
        <div class="fila-dato">
          <span class="etiqueta">Nombre completo:</span>
          <span class="valor">{{ estudiante || 'No encontrado' }}</span>
        </div>
        <div class="fila-dato">
          <span class="etiqueta">Email:</span>
          <span class="valor">{{ email }}</span>
        </div>
      </div>

      <div class="seccion-titulo" style="margin-top: 25px;">ASIGNATURAS MATRICULADAS</div>
      
      <table class="tabla-matricula">
        <thead>
          <tr>
            <th style="width: 5%; text-align: center;">N°</th>
            <th style="width: 12%;">Código</th>
            <th style="width: 35%;">Curso</th>
            <th style="width: 10%;">Año</th>
            <th style="width: 10%;">Grupo</th>
            <th style="width: 13%;">Laboratorio</th>
            <th style="width: 15%;">Docente</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, indice) in matriculas" :key="item.id">
            <td style="text-align: center;">{{ indice + 1 }}</td>
            <td>{{ item.workload.course.code }}</td>
            <td>
              <strong>{{ item.workload.course.name }}</strong>
              <span class="alias-curso"> 
                ({{ item.workload.course.acronym }})
              </span>
            </td>
            <td>{{ item.workload.course.year_display }}</td>
            <td>{{ item.workload.group }}</td>
            <td>{{ item.workload.laboratory }}</td>
            <td>{{ item.workload.teacher.full_name }}</td>
          </tr>
          <tr v-if="matriculas.length === 0">
            <td colspan="7" style="text-align: center; padding: 30px; color: #a0aec0; font-style: italic;">
              No se encontraron registros de matrícula para este CUI.
            </td>
          </tr>
        </tbody>
      </table>

      <div class="total-cursos">
        <strong>Total de cursos matriculados:</strong> {{ matriculas.length }}
      </div>

      <div class="pie-pagina">
        Documento generado digitalmente por el Sistema de Matrícula de Laboratorio EPIS.
      </div>
    </div>
  </div>
</template>

<style scoped>
.contenedor-principal {
  background-color: #f3f4f6;
  min-height: 100vh;
  width: 100%;
  padding: 40px 20px;
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  box-sizing: border-box;
}

.cargando {
  font-size: 1.2rem;
  color: #4b5563;
  margin-top: 50px;
  text-align: center;
  width: 100%;
}

.documento-tarjeta {
  background-color: #ffffff;
  width: 100%;
  max-width: 1000px;
  border: 1px solid #e5e7eb;
  padding: 40px 50px;
  box-sizing: border-box;
}

.encabezado {
  text-align: center;
}

.encabezado h1 {
  color: #0d47a1;
  font-size: 1.5rem;
  margin: 0 0 8px 0;
  font-weight: bold;
}

.encabezado h2 {
  color: #333333;
  font-size: 1.1rem;
  margin: 0 0 12px 0;
  font-weight: bold;
}

.encabezado .fecha {
  color: #666666;
  font-size: 0.85rem;
  margin: 0 0 4px 0;
}

.encabezado .autor {
  color: #4b5563;
  font-size: 0.9rem;
  font-weight: bold;
  margin: 0;
}

.separador {
  border: 0;
  border-top: 1px solid #ccc;
  margin: 20px 0;
}

.seccion-titulo {
  background-color: #eee;
  color: #111;
  font-weight: bold;
  font-size: 0.9rem;
  padding: 8px 12px;
  border-left: 4px solid #0d47a1;
  margin-bottom: 15px;
}

.datos-alumno-grid {
  padding: 5px 12px;
}

.fila-dato {
  display: flex;
  margin-bottom: 8px;
  font-size: 0.95rem;
}

.etiqueta {
  font-weight: bold;
  color: #111;
  width: 160px;
}

.valor {
  color: #333;
}

.tabla-matricula {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  font-size: 0.9rem;
}

.tabla-matricula th {
  border: 1px solid #e5e7eb;
  padding: 10px 8px;
  background-color: #f9fafb;
  color: #111;
  font-weight: bold;
  text-align: left;
}

.tabla-matricula td {
  border: 1px solid #e5e7eb;
  padding: 12px 8px;
  color: #333;
}

.alias-curso {
  font-weight: normal;
  color: #555;
}

.total-cursos {
  margin-top: 25px;
  font-size: 0.95rem;
  color: #111;
}

.pie-pagina {
  text-align: center;
  margin-top: 45px;
  font-size: 0.8rem;
  color: #999;
  font-style: italic;
}
</style>