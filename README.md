// Configuración de materias con sus requisitos:
const materias = [
    { codigo: "GEV-001", nombre: "Gestión del Entorno de Aprendizaje", prereq: [] },
    { codigo: "MEE-101", nombre: "Fundamentos Psicológicos y Pedagógicos de la Educación Especial", prereq: [] },
    { codigo: "MEE-103", nombre: "Neurociencia y Aprendizaje", prereq: [] },
    { codigo: "MM-101", nombre: "Metodología de la Investigación", prereq: [] },

    { codigo: "MEE-201", nombre: "Desarrollo Evolutivo", prereq: [] },
    { codigo: "MEE-202", nombre: "Desviaciones en el Aprendizaje y Desarrollo Intelectual", prereq: [] },
    { codigo: "MEE-203", nombre: "Desviaciones Auditivas", prereq: [] },
    { codigo: "MEE-204", nombre: "Desviaciones Visuales", prereq: [] },

    { codigo: "MEE-301", nombre: "Trastornos Afectivos y Conductuales", prereq: ["MEE-201"] },
    { codigo: "MEE-302", nombre: "Trastorno Generalizado del Desarrollo", prereq: ["MEE-201"] },
    { codigo: "MEE-303", nombre: "Discapacidad Físico-Motoras", prereq: ["MEE-201"] },
    { codigo: "MEE-304", nombre: "Trastorno del Lenguaje", prereq: ["MEE-201"] },

    { codigo: "MEE-402", nombre: "Evaluación, Diagnóstico e Intervención", prereq: ["MEE-101"] },
    { codigo: "MEE-403", nombre: "Familia, Comunidad y Psicopedagogía Escolar", prereq: [] },
    { codigo: "MEE-501", nombre: "Tecnología Aplicada a la Atención a la Diversidad", prereq: [] },

    { codigo: "MEE-502", nombre: "Seminario de Actualización", prereq: [] },
    { codigo: "MEE-503", nombre: "Pasantía", prereq: [] },
    { codigo: "MSI-102", nombre: "Seminario de Investigación II", prereq: ["MSI-101"] },

    { codigo: "MEE-600", nombre: "Trabajo Final", prereq: [] },

    { codigo: "MSI-101", nombre: "Seminario de Investigación I", prereq: ["MM-101"] }
];

let estadoMaterias = {};

// Función para crear el HTML:
function generarMalla() {
    const contenedor = document.getElementById("malla");
    contenedor.innerHTML = "";

    materias.forEach(materia => {
        const div = document.createElement("div");
        div.className = "caja bloqueada";
        div.id = materia.codigo;
        div.innerHTML = <strong>${materia.codigo}</strong><br>${materia.nombre};

        div.onclick = () => aprobarMateria(materia.codigo);
        contenedor.appendChild(div);

        estadoMaterias[materia.codigo] = false;
    });

    actualizarBloqueo();
}

// Aprobar una materia:
function aprobarMateria(codigo) {
    if (document.getElementById(codigo).classList.contains("bloqueada")) {
        alert("Debes aprobar los requisitos primero.");
        return;
    }

    estadoMaterias[codigo] = !estadoMaterias[codigo];

    actualizarEstadoVisual();
    actualizarBloqueo();
}

function actualizarEstadoVisual() {
    for (const codigo in estadoMaterias) {
        const div = document.getElementById(codigo);
        if (estadoMaterias[codigo]) {
            div.classList.add("aprobada");
        } else {
            div.classList.remove("aprobada");
        }
    }
}

function actualizarBloqueo() {
    materias.forEach(materia => {
        const div = document.getElementById(materia.codigo);
        const cumplidos = materia.prereq.every(req => estadoMaterias[req]);
        if (materia.prereq.length === 0 || cumplidos) {
            div.classList.remove("bloqueada");
        } else {
            div.classList.add("bloqueada");
        }
    });
}

window.onload = generarMalla;
