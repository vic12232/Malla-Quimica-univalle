// script.js

const cursos = [
  {
    codigo: '116009C', nombre: 'Introducción a las ciencias naturales', semestre: 1, requisitos: []
  },
  {
    codigo: '106002C', nombre: 'Introducción a la experimentación científica', semestre: 1, requisitos: []
  },
  {
    codigo: '111001C', nombre: 'Matemática fundamental', semestre: 1, requisitos: []
  },
  {
    codigo: '116013C', nombre: 'Química general', semestre: 2, requisitos: ['116009C', '106002C', '111001C']
  },
  {
    codigo: '111013C', nombre: 'Cálculo I', semestre: 2, requisitos: ['111001C']
  },
  {
    codigo: '102052C', nombre: 'Biología general', semestre: 2, requisitos: ['116009C', '106002C']
  },
  {
    codigo: '106001C', nombre: 'Física fundamental I', semestre: 2, requisitos: ['116009C', '106002C', '111013C']
  },
  {
    codigo: '116015C', nombre: 'Química analítica', semestre: 3, requisitos: ['116013C', '111001C']
  },
  {
    codigo: '111014C', nombre: 'Cálculo II', semestre: 3, requisitos: ['111013C']
  },
  {
    codigo: '106008C', nombre: 'Física II', semestre: 3, requisitos: ['111013C', '106001C']
  },
  {
    codigo: '111002C', nombre: 'Intro. al modelamiento matemático', semestre: 3, requisitos: ['111013C', '106001C', '102052C', '116013C']
  },
  {
    codigo: '116003C', nombre: 'Experimentación científica', semestre: 3, requisitos: ['106002C', '116013C']
  },
  // ... añadir el resto de cursos aquí
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
