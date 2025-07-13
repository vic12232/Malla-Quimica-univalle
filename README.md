// script.js

const cursos = [
  // Semestre 1
  { codigo: '116009C', nombre: 'Introducción a las ciencias naturales', semestre: 1, requisitos: [] },
  { codigo: '106002C', nombre: 'Introducción a la experimentación científica', semestre: 1, requisitos: [] },
  { codigo: '111001C', nombre: 'Matemática fundamental', semestre: 1, requisitos: [] },

  // Semestre 2
  { codigo: '116013C', nombre: 'Química general', semestre: 2, requisitos: ['116009C', '106002C', '111001C'] },
  { codigo: '111013C', nombre: 'Cálculo I', semestre: 2, requisitos: ['111001C'] },
  { codigo: '102052C', nombre: 'Biología general', semestre: 2, requisitos: ['116009C', '106002C'] },
  { codigo: '106001C', nombre: 'Física fundamental I', semestre: 2, requisitos: ['116009C', '106002C', '111013C'] },

  // Semestre 3
  { codigo: '116015C', nombre: 'Química analítica', semestre: 3, requisitos: ['116013C', '111001C'] },
  { codigo: '111014C', nombre: 'Cálculo II', semestre: 3, requisitos: ['111013C'] },
  { codigo: '106008C', nombre: 'Física II', semestre: 3, requisitos: ['111013C', '106001C'] },
  { codigo: '111002C', nombre: 'Intro. al modelamiento matemático', semestre: 3, requisitos: ['111013C', '106001C', '102052C', '116013C'] },
  { codigo: '116003C', nombre: 'Experimentación científica', semestre: 3, requisitos: ['106002C', '116013C', '116015C'] },

  // Semestre 4
  { codigo: '111032C', nombre: 'Matemáticas especiales I', semestre: 4, requisitos: ['111014C'] },
  { codigo: '116002C', nombre: 'Estructura atómica y enlace', semestre: 4, requisitos: ['111014C', '106008C', '116013C'] },
  { codigo: '106016C', nombre: 'Física III', semestre: 4, requisitos: ['111014C', '106008C'] },
  { codigo: '116018C', nombre: 'Análisis instrumental I', semestre: 4, requisitos: ['116015C'] },
  { codigo: '116028C', nombre: 'Laboratorio de química analítica', semestre: 4, requisitos: ['116015C', '116003C'] },
  { codigo: '116023C', nombre: 'Estadística química', semestre: 4, requisitos: ['116003C', '116015C'] },

  // Semestre 5
  { codigo: '116024C', nombre: 'Fisicoquímica I', semestre: 5, requisitos: ['116002C', '111032C', '106016C'] },
  { codigo: '111036C', nombre: 'Matemáticas especiales II', semestre: 5, requisitos: ['111032C'] },
  { codigo: '116039C', nombre: 'Química orgánica I', semestre: 5, requisitos: ['116002C'] },
  { codigo: '116031C', nombre: 'Química inorgánica I', semestre: 5, requisitos: ['116002C'] },
  { codigo: '116021C', nombre: 'Análisis instrumental II', semestre: 5, requisitos: ['116018C', '116002C'] },
  { codigo: '116027C', nombre: 'Laboratorio de análisis instrumental', semestre: 5, requisitos: ['116018C'] },

  // Semestre 6
  { codigo: '116025C', nombre: 'Fisicoquímica II', semestre: 6, requisitos: ['116024C', '111036C'] },
  { codigo: '116040C', nombre: 'Química orgánica II', semestre: 6, requisitos: ['116039C'] },
  { codigo: '116032C', nombre: 'Química inorgánica II', semestre: 6, requisitos: ['116031C', '116024C'] },
  { codigo: '116034C', nombre: 'Lab. de fisicoquímica I', semestre: 6, requisitos: ['116024C'] },
  { codigo: '116036C', nombre: 'Lab. de química orgánica I', semestre: 6, requisitos: ['116039C', '116027C'] },
  { codigo: '116029C', nombre: 'Lab. de química inorgánica I', semestre: 6, requisitos: ['116031C', '116027C'] },

  // Semestre 7
  { codigo: '116043C', nombre: 'Bioquímica', semestre: 7, requisitos: ['116025C', '116040C', '116027C'] },
  { codigo: '116057C', nombre: 'Química macromolecular', semestre: 7, requisitos: ['116040C', '116032C'] },
  { codigo: '116055C', nombre: 'Seminario', semestre: 7, requisitos: ['116025C', '116040C', '116032C'] },
  { codigo: '116035C', nombre: 'Lab. de fisicoquímica II', semestre: 7, requisitos: ['116025C', '116034C'] },
  { codigo: '116037C', nombre: 'Lab. de química orgánica II', semestre: 7, requisitos: ['116040C', '116027C', '116036C'] },
  { codigo: '116030C', nombre: 'Lab. de química inorgánica II', semestre: 7, requisitos: ['116032C', '116027C', '116029C'] },

  // Semestre 8
  { codigo: 'BIO-MOL', nombre: 'Biología molecular', semestre: 8, requisitos: ['116043C'] },
  { codigo: '116033C', nombre: 'Identificación de compuestos', semestre: 8, requisitos: ['116040C', '116032C', '116021C'] },
  { codigo: '116054C', nombre: 'Química verde y ambiental', semestre: 8, requisitos: ['116040C', '116032C'] },

  // Semestre 9
  { codigo: 'TG-I', nombre: 'Trabajo de grado I', semestre: 9, requisitos: ['BIO-MOL', '116033C'] },

  // Semestre 10
  { codigo: 'TG-II', nombre: 'Trabajo de grado II', semestre: 10, requisitos: ['TG-I'] },
];

const aprobados = new Set();

function crearMalla() {
  const grid = document.getElementById('grid');
  const semestres = [...new Set(cursos.map(c => c.semestre))].sort((a, b) => a - b);

  semestres.forEach(sem => {
    const label = document.createElement('div');
    label.className = 'semester-label';
    label.textContent = `Semestre ${sem}`;
    grid.appendChild(label);

    cursos.filter(c => c.semestre === sem).forEach(curso => {
      const div = document.createElement('div');
      div.className = 'course locked';
      div.id = curso.codigo;
      div.textContent = curso.nombre;
      div.onclick = () => toggleCurso(curso);
      grid.appendChild(div);
    });
  });

  actualizarEstado();
}

function toggleCurso(curso) {
  const div = document.getElementById(curso.codigo);
  if (div.classList.contains('locked')) return;

  if (aprobados.has(curso.codigo)) {
    aprobados.delete(curso.codigo);
  } else {
    aprobados.add(curso.codigo);
  }

  actualizarEstado();
}

function actualizarEstado() {
  cursos.forEach(curso => {
    const div = document.getElementById(curso.codigo);
    const requisitosAprobados = curso.requisitos.every(r => aprobados.has(r));

    if (requisitosAprobados || curso.requisitos.length === 0) {
      div.classList.remove('locked');
    } else {
      div.classList.add('locked');
    }

    if (aprobados.has(curso.codigo)) {
      div.classList.add('approved');
    } else {
      div.classList.remove('approved');
    }
  });
}

crearMalla();
