<template>
  <div class="container mt-4">
    <h1>Listado de Personas</h1>
    
    <div class="row mb-3">
      <div class="col-md-6">
        <input 
          type="text" 
          class="form-control" 
          v-model="filtroNombre" 
          placeholder="Filtrar por nombre o apellido"
        >
      </div>
      <div class="col-md-6">
        <input 
          type="text" 
          class="form-control" 
          v-model="filtroDNI" 
          placeholder="Filtrar por DNI"
        >
      </div>
    </div>

    <div class="alert alert-warning" v-if="mostrarAdvertencia">
      Por favor, ingrese al menos 3 caracteres en alguno de los filtros
    </div>

    <table class="table">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Apellido</th>
          <th>DNI</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="persona in personasFiltradas" :key="persona.dni">
          <td>{{ persona.nombre }}</td>
          <td>{{ persona.apellido }}</td>
          <td>{{ persona.dni }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      personas: [
        { nombre: 'Brian', apellido: 'Wainer', dni: '12345678' },
        { nombre: 'Lionel', apellido: 'Messi', dni: '23948287' },
        { nombre: 'Martin', apellido: 'Palermo', dni: '30444287' }
      ],
      filtroNombre: '',
      filtroDNI: ''
    }
  },
  computed: {
    mostrarAdvertencia() {
      return (this.filtroNombre.length > 0 && this.filtroNombre.length < 3) || 
             (this.filtroDNI.length > 0 && this.filtroDNI.length < 3)
    },
    personasFiltradas() {
      if (!this.filtroNombre && !this.filtroDNI) return this.personas

      return this.personas.filter(persona => {
        const nombreCompleto = `${persona.nombre} ${persona.apellido}`.toLowerCase()
        const cumpleFiltroNombre = !this.filtroNombre || 
          (this.filtroNombre.length >= 3 && 
           nombreCompleto.includes(this.filtroNombre.toLowerCase()))
        
        const cumpleFiltroDNI = !this.filtroDNI || 
          (this.filtroDNI.length >= 3 && 
           persona.dni.includes(this.filtroDNI))

        return cumpleFiltroNombre && cumpleFiltroDNI
      })
    }
  }
}
</script> 