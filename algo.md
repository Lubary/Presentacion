<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Creador de "Mis Estudios" para GitHub</title>
    <!-- Tailwind CSS para el diseño de la aplicación -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome para los iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- Marked.js para renderizar Markdown en tiempo real en la vista previa -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;700&display=swap');
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
        }
        .mono {
            font-family: 'JetBrains Mono', monospace;
        }
        /* Estilos personalizados para la vista previa simulada de GitHub */
        .github-preview {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            color: #1f2328;
        }
        .github-preview h1 {
            font-size: 2em;
            font-weight: 600;
            border-bottom: 1px solid #d0d7de;
            padding-bottom: 0.3em;
            margin-top: 24px;
            margin-bottom: 16px;
        }
        .github-preview h2 {
            font-size: 1.5em;
            font-weight: 600;
            border-bottom: 1px solid #d0d7de;
            padding-bottom: 0.3em;
            margin-top: 24px;
            margin-bottom: 16px;
        }
        .github-preview h3 {
            font-size: 1.25em;
            font-weight: 600;
            margin-top: 24px;
            margin-bottom: 16px;
        }
        .github-preview p {
            margin-top: 0;
            margin-bottom: 16px;
            line-height: 1.5;
        }
        .github-preview ul {
            padding-left: 2em;
            list-style-type: disc;
            margin-top: 0;
            margin-bottom: 16px;
        }
        .github-preview li {
            margin-top: 0.25em;
        }
        .github-preview blockquote {
            padding: 0 1em;
            color: #656d76;
            border-left: 0.25em solid #d0d7de;
            margin: 0 0 16px 0;
        }
        .github-preview a {
            color: #0969da;
            text-decoration: none;
        }
        .github-preview a:hover {
            text-decoration: underline;
        }
        .github-preview table {
            border-spacing: 0;
            border-collapse: collapse;
            width: 100%;
            margin-top: 0;
            margin-bottom: 16px;
        }
        .github-preview th, .github-preview td {
            padding: 6px 13px;
            border: 1px solid #d0d7de;
        }
        .github-preview tr {
            background-color: #ffffff;
            border-top: 1px solid #d0d7de;
        }
        .github-preview tr:nth-child(even) {
            background-color: #f6f8fa;
        }
        /* Temas de previsualización oscuros */
        .dark-mode-preview {
            color: #e6edf3 !important;
        }
        .dark-mode-preview h1, .dark-mode-preview h2 {
            border-bottom-color: #21262d !important;
        }
        .dark-mode-preview tr {
            background-color: #0d1117 !important;
            border-top-color: #21262d !important;
        }
        .dark-mode-preview tr:nth-child(even) {
            background-color: #161b22 !important;
        }
        .dark-mode-preview th, .dark-mode-preview td {
            border-color: #30363d !important;
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen text-slate-800 flex flex-col">

    <!-- Navbar -->
    <header class="bg-slate-900 text-white shadow-md px-6 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
        <div class="flex items-center gap-3">
            <div class="bg-indigo-600 p-2.5 rounded-lg text-white">
                <i class="fa-solid fa-graduation-cap text-2xl"></i>
            </div>
            <div>
                <h1 class="text-xl font-bold tracking-tight">Creador de Estudios para GitHub</h1>
                <p class="text-xs text-slate-400">Genera un estudios.md interactivo y profesional</p>
            </div>
        </div>
        <div class="flex gap-2">
            <button onclick="loadDemoData()" class="px-4 py-2 text-sm bg-slate-800 hover:bg-slate-700 rounded-lg font-medium transition flex items-center gap-2">
                <i class="fa-solid fa-wand-magic-sparkles"></i> Cargar Ejemplo
            </button>
            <button onclick="exportMarkdown()" class="px-4 py-2 text-sm bg-indigo-600 hover:bg-indigo-500 rounded-lg font-medium transition flex items-center gap-2 shadow-sm">
                <i class="fa-solid fa-copy"></i> Copiar Código MD
            </button>
        </div>
    </header>

    <!-- Main Workspace -->
    <main class="flex-grow grid grid-cols-1 lg:grid-cols-12 gap-6 p-6">
        
        <!-- EDITOR PANEL (Left) -->
        <section class="lg:col-span-5 flex flex-col gap-6 max-h-[85vh] overflow-y-auto pr-2">
            
            <!-- Datos Básicos -->
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                <div class="flex items-center gap-2 mb-4">
                    <span class="text-indigo-600 text-lg"><i class="fa-solid fa-user-gear"></i></span>
                    <h2 class="text-md font-bold text-slate-900">1. Datos Personales</h2>
                </div>
                <div class="grid grid-cols-1 gap-3">
                    <div>
                        <label class="text-xs font-semibold text-slate-500 block mb-1">Nombre Completo</label>
                        <input type="text" id="userName" placeholder="Juan Pérez" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none transition text-sm">
                    </div>
                    <div>
                        <label class="text-xs font-semibold text-slate-500 block mb-1">Título / Especialidad</label>
                        <input type="text" id="userTitle" placeholder="Estudiante de Desarrollo de Software / Autodidacta" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none transition text-sm">
                    </div>
                    <div>
                        <label class="text-xs font-semibold text-slate-500 block mb-1">Usuario de GitHub (Para insignias y contador)</label>
                        <input type="text" id="userGithub" placeholder="miusuario_github" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none transition text-sm">
                    </div>
                    <div>
                        <label class="text-xs font-semibold text-slate-500 block mb-1">Mini Presentación Académica</label>
                        <textarea id="userBio" rows="3" placeholder="Cuéntale brevemente a los reclutadores cuál es tu foco de estudio, tus motivaciones o tu transición laboral..." class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none transition text-sm"></textarea>
                    </div>
                </div>
            </div>

            <!-- Educación Formal (Historial) -->
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                <div class="flex justify-between items-center mb-4">
                    <div class="flex items-center gap-2">
                        <span class="text-indigo-600 text-lg"><i class="fa-solid fa-university"></i></span>
                        <h2 class="text-md font-bold text-slate-900">2. Estudios / Formación</h2>
                    </div>
                    <button onclick="addEducationField()" class="text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-600 font-semibold px-2.5 py-1.5 rounded transition flex items-center gap-1">
                        <i class="fa-solid fa-plus"></i> Añadir
                    </button>
                </div>
                <div id="educationContainer" class="flex flex-col gap-4">
                    <!-- Dinámico -->
                </div>
            </div>

            <!-- Certificaciones y Cursos -->
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                <div class="flex justify-between items-center mb-4">
                    <div class="flex items-center gap-2">
                        <span class="text-indigo-600 text-lg"><i class="fa-solid fa-certificate"></i></span>
                        <h2 class="text-md font-bold text-slate-900">3. Certificados y Cursos</h2>
                    </div>
                    <button onclick="addCertificateField()" class="text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-600 font-semibold px-2.5 py-1.5 rounded transition flex items-center gap-1">
                        <i class="fa-solid fa-plus"></i> Añadir
                    </button>
                </div>
                <div id="certificatesContainer" class="flex flex-col gap-4">
                    <!-- Dinámico -->
                </div>
            </div>

            <!-- Proyectos Académicos/Prácticas -->
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                <div class="flex justify-between items-center mb-4">
                    <div class="flex items-center gap-2">
                        <span class="text-indigo-600 text-lg"><i class="fa-solid fa-diagram-project"></i></span>
                        <h2 class="text-md font-bold text-slate-900">4. Proyectos Destacados</h2>
                    </div>
                    <button onclick="addProjectField()" class="text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-600 font-semibold px-2.5 py-1.5 rounded transition flex items-center gap-1">
                        <i class="fa-solid fa-plus"></i> Añadir
                    </button>
                </div>
                <div id="projectsContainer" class="flex flex-col gap-4">
                    <!-- Dinámico -->
                </div>
            </div>

        </section>

        <!-- PREVIEW PANEL (Right) -->
        <section class="lg:col-span-7 flex flex-col gap-4">
            
            <!-- Selector de Vista (Markdown vs Vista Previa) -->
            <div class="bg-white p-3 rounded-xl shadow-sm border border-slate-200 flex flex-wrap justify-between items-center gap-2">
                <div class="flex gap-2 bg-slate-100 p-1 rounded-lg">
                    <button onclick="switchTab('visual')" id="btnTabVisual" class="px-4 py-1.5 rounded-md text-sm font-semibold transition bg-white text-slate-900 shadow-sm">
                        <i class="fa-solid fa-eye"></i> Vista Previa (GitHub)
                    </button>
                    <button onclick="switchTab('code')" id="btnTabCode" class="px-4 py-1.5 rounded-md text-sm font-semibold transition text-slate-500 hover:text-slate-800">
                        <i class="fa-solid fa-code"></i> Código Markdown (.md)
                    </button>
                </div>

                <!-- Selector de Fondo de GitHub (Claro/Oscuro) -->
                <div id="previewThemeSelector" class="flex items-center gap-2">
                    <span class="text-xs text-slate-500 font-medium">Modo GitHub:</span>
                    <button onclick="toggleGithubTheme('light')" id="btnGithubLight" class="p-1.5 rounded-md bg-slate-200 border border-slate-300 text-slate-800" title="Modo Claro">
                        <i class="fa-solid fa-sun text-sm"></i>
                    </button>
                    <button onclick="toggleGithubTheme('dark')" id="btnGithubDark" class="p-1.5 rounded-md bg-slate-800 text-slate-400 hover:bg-slate-700" title="Modo Oscuro">
                        <i class="fa-solid fa-moon text-sm"></i>
                    </button>
                </div>
            </div>

            <!-- Contenido de la Previsualización -->
            <div class="flex-grow bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden flex flex-col max-h-[75vh]">
                
                <!-- Tab de Vista Previa -->
                <div id="tabVisualContent" class="p-8 overflow-y-auto flex-grow bg-white transition-colors duration-200">
                    <div id="githubRender" class="github-preview">
                        <!-- El Markdown renderizado aparecerá aquí -->
                    </div>
                </div>

                <!-- Tab de Código Markdown -->
                <div id="tabCodeContent" class="hidden p-4 bg-slate-950 text-slate-200 overflow-y-auto flex-grow font-mono text-sm relative">
                    <pre id="markdownCode" class="whitespace-pre-wrap select-all bg-transparent focus:outline-none focus:ring-0"></pre>
                    <button onclick="exportMarkdown()" class="absolute top-4 right-4 bg-indigo-600 hover:bg-indigo-500 text-white font-medium px-3 py-1.5 rounded-lg text-xs transition flex items-center gap-1.5">
                        <i class="fa-solid fa-copy"></i> Copiar Código
                    </button>
                </div>
            </div>

            <!-- Explicación de cómo subirlo a GitHub -->
            <div class="bg-indigo-50 border border-indigo-100 rounded-xl p-4 text-xs text-indigo-900 flex gap-3 items-start">
                <span class="text-indigo-600 text-lg mt-0.5"><i class="fa-solid fa-circle-info"></i></span>
                <div>
                    <h3 class="font-bold mb-1">¿Cómo uso este contenido en mi repositorio de GitHub?</h3>
                    <p class="leading-relaxed">
                        Copia el código de la pestaña <strong>"Código Markdown (.md)"</strong>, ve a tu repositorio en GitHub, crea un archivo llamado <code class="bg-indigo-100 px-1 rounded font-mono">estudios.md</code>, pega todo el contenido y guárdalo (Commit). ¡Listo, tendrás tu historial de estudios perfectamente estructurado!
                    </p>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="bg-slate-950 text-slate-500 text-xs py-4 text-center border-t border-slate-800">
        <p>Desarrollado para principiantes de GitHub. Edita, previsualiza y copia.</p>
    </footer>

    <!-- Script de Control Principal -->
    <script>
        let currentTab = 'visual';
        let currentGithubTheme = 'light';
        
        // Estructuras de datos dinámicos por defecto
        let educationList = [];
        let certificatesList = [];
        let projectsList = [];

        // Inicializador
        window.onload = function() {
            // Cargar datos por defecto para que no se vea vacío
            loadDemoData();
            
            // Listeners para inputs de texto plano
            ['userName', 'userTitle', 'userGithub', 'userBio'].forEach(id => {
                document.getElementById(id).addEventListener('input', updateContent);
            });
        };

        // Cambiar entre Pestaña Visual y Pestaña Código
        function switchTab(tab) {
            currentTab = tab;
            const btnVisual = document.getElementById('btnTabVisual');
            const btnCode = document.getElementById('btnTabCode');
            const contentVisual = document.getElementById('tabVisualContent');
            const contentCode = document.getElementById('tabCodeContent');

            if (tab === 'visual') {
                btnVisual.className = "px-4 py-1.5 rounded-md text-sm font-semibold transition bg-white text-slate-900 shadow-sm";
                btnCode.className = "px-4 py-1.5 rounded-md text-sm font-semibold transition text-slate-500 hover:text-slate-800";
                contentVisual.classList.remove('hidden');
                contentCode.classList.add('hidden');
                document.getElementById('previewThemeSelector').classList.remove('invisible');
            } else {
                btnCode.className = "px-4 py-1.5 rounded-md text-sm font-semibold transition bg-white text-slate-900 shadow-sm";
                btnVisual.className = "px-4 py-1.5 rounded-md text-sm font-semibold transition text-slate-500 hover:text-slate-800";
                contentCode.classList.remove('hidden');
                contentVisual.classList.add('hidden');
                document.getElementById('previewThemeSelector').classList.add('invisible');
            }
            updateContent();
        }

        // Alternar modo claro/oscuro de simulación GitHub
        function toggleGithubTheme(theme) {
            currentGithubTheme = theme;
            const renderBox = document.getElementById('tabVisualContent');
            const renderText = document.getElementById('githubRender');
            const btnLight = document.getElementById('btnGithubLight');
            const btnDark = document.getElementById('btnGithubDark');

            if (theme === 'dark') {
                renderBox.className = "p-8 overflow-y-auto flex-grow bg-[#0d1117] transition-colors duration-200";
                renderText.classList.add('dark-mode-preview');
                btnDark.className = "p-1.5 rounded-md bg-slate-700 text-yellow-400 border border-slate-600";
                btnLight.className = "p-1.5 rounded-md bg-slate-200 text-slate-500 hover:bg-slate-300";
            } else {
                renderBox.className = "p-8 overflow-y-auto flex-grow bg-white transition-colors duration-200";
                renderText.classList.remove('dark-mode-preview');
                btnLight.className = "p-1.5 rounded-md bg-slate-200 text-amber-600 border border-slate-300";
                btnDark.className = "p-1.5 rounded-md bg-slate-800 text-slate-400 hover:bg-slate-700";
            }
        }

        // Cargar Datos de Prueba (Demo)
        function loadDemoData() {
            document.getElementById('userName').value = "Camila Ramos";
            document.getElementById('userTitle').value = "Desarrolladora Web Jr. | Estudiante de Ingeniería";
            document.getElementById('userGithub').value = "camiladev";
            document.getElementById('userBio').value = "Me apasiona estructurar interfaces limpias y resolver problemas mediante código moderno. Actualmente estoy cursando mis estudios formales en tecnología y complementándolo con autoaprendizaje continuo sobre tecnologías frontend y backend.";

            // Educación Demo
            educationList = [
                { id: 1, career: "Ingeniería de Sistemas", school: "Universidad Tecnológica", period: "2023 - Presente", detail: "Enfoque en programación orientada a objetos, bases de datos y algoritmos avanzados. Miembro del club de desarrollo de software." },
                { id: 2, career: "Bootcamp Desarrollo Web Full Stack", school: "Academia WebTech", period: "2024 (6 meses)", detail: "Curso intensivo de más de 400 horas enfocado en el stack MERN (MongoDB, Express, React, Node.js)." }
            ];

            // Certificaciones Demo
            certificatesList = [
                { id: 1, name: "Responsive Web Design", provider: "freeCodeCamp", date: "Julio 2024" },
                { id: 2, name: "JavaScript Moderno y ES6", provider: "Platzi", date: "Septiembre 2024" }
            ];

            // Proyectos Demo
            projectsList = [
                { id: 1, name: "Plataforma de Tareas Educativas", tech: "React & Firebase", detail: "Proyecto de fin de Bootcamp. Permite a estudiantes organizar asignaturas y ver notas con gráficos de rendimiento." }
            ];

            renderFields();
            updateContent();
        }

        // Renderizar los campos del editor en base a los arrays
        function renderFields() {
            // Renderizar Educación
            const eduContainer = document.getElementById('educationContainer');
            eduContainer.innerHTML = '';
            educationList.forEach((item, index) => {
                eduContainer.innerHTML += `
                    <div class="bg-slate-50 p-4 rounded-lg border border-slate-200 relative group">
                        <button onclick="removeField('edu', ${index})" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 transition">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                        <div class="grid grid-cols-1 gap-2 text-xs">
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="font-bold text-slate-500 mb-1 block">Título u Grado</label>
                                    <input type="text" value="${item.career}" oninput="updateItemValue('edu', ${index}, 'career', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                                <div>
                                    <label class="font-bold text-slate-500 mb-1 block">Institución</label>
                                    <input type="text" value="${item.school}" oninput="updateItemValue('edu', ${index}, 'school', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                            </div>
                            <div class="grid grid-cols-3 gap-2">
                                <div class="col-span-1">
                                    <label class="font-bold text-slate-500 mb-1 block">Período</label>
                                    <input type="text" value="${item.period}" oninput="updateItemValue('edu', ${index}, 'period', this.value)" class="w-full px-2 py-1.5 border rounded bg-white" placeholder="2023 - 2025">
                                </div>
                                <div class="col-span-2">
                                    <label class="font-bold text-slate-500 mb-1 block">Detalles o Logros</label>
                                    <input type="text" value="${item.detail}" oninput="updateItemValue('edu', ${index}, 'detail', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            });

            // Renderizar Certificaciones
            const certContainer = document.getElementById('certificatesContainer');
            certContainer.innerHTML = '';
            certificatesList.forEach((item, index) => {
                certContainer.innerHTML += `
                    <div class="bg-slate-50 p-4 rounded-lg border border-slate-200 relative">
                        <button onclick="removeField('cert', ${index})" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 transition">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                        <div class="grid grid-cols-1 gap-2 text-xs">
                            <div class="grid grid-cols-3 gap-2">
                                <div class="col-span-2">
                                    <label class="font-bold text-slate-500 mb-1 block">Certificado / Curso</label>
                                    <input type="text" value="${item.name}" oninput="updateItemValue('cert', ${index}, 'name', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                                <div>
                                    <label class="font-bold text-slate-500 mb-1 block">Emisor</label>
                                    <input type="text" value="${item.provider}" oninput="updateItemValue('cert', ${index}, 'provider', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                            </div>
                            <div>
                                <label class="font-bold text-slate-500 mb-1 block">Fecha de Obtención</label>
                                <input type="text" value="${item.date}" oninput="updateItemValue('cert', ${index}, 'date', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                            </div>
                        </div>
                    </div>
                `;
            });

            // Renderizar Proyectos
            const projContainer = document.getElementById('projectsContainer');
            projContainer.innerHTML = '';
            projectsList.forEach((item, index) => {
                projContainer.innerHTML += `
                    <div class="bg-slate-50 p-4 rounded-lg border border-slate-200 relative">
                        <button onclick="removeField('proj', ${index})" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 transition">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                        <div class="grid grid-cols-1 gap-2 text-xs">
                            <div class="grid grid-cols-2 gap-2">
                                <div>
                                    <label class="font-bold text-slate-500 mb-1 block">Nombre del Proyecto</label>
                                    <input type="text" value="${item.name}" oninput="updateItemValue('proj', ${index}, 'name', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                                <div>
                                    <label class="font-bold text-slate-500 mb-1 block">Tecnologías Utilizadas</label>
                                    <input type="text" value="${item.tech}" oninput="updateItemValue('proj', ${index}, 'tech', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                                </div>
                            </div>
                            <div>
                                <label class="font-bold text-slate-500 mb-1 block">Detalles del desarrollo</label>
                                <input type="text" value="${item.detail}" oninput="updateItemValue('proj', ${index}, 'detail', this.value)" class="w-full px-2 py-1.5 border rounded bg-white">
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        // Gestión de campos dinámica (Añadir / Eliminar)
        function addEducationField() {
            educationList.push({ career: 'Nueva Carrera / Grado', school: 'Institución Educativa', period: 'Año de Inicio - Fin', detail: 'Escribe aquí algún proyecto final, promedio u orientación académica relevante.' });
            renderFields();
            updateContent();
        }

        function addCertificateField() {
            certificatesList.push({ name: 'Nombre del Curso / Certificación', provider: 'Udemy, Coursera, Platzi, etc.', date: 'Mes Año' });
            renderFields();
            updateContent();
        }

        function addProjectField() {
            projectsList.push({ name: 'Proyecto de Aprendizaje', tech: 'HTML, CSS, JS', detail: 'Describe qué creaste para demostrar tus capacidades autodidactas.' });
            renderFields();
            updateContent();
        }

        function removeField(type, index) {
            if (type === 'edu') educationList.splice(index, 1);
            if (type === 'cert') certificatesList.splice(index, 1);
            if (type === 'proj') projectsList.splice(index, 1);
            renderFields();
            updateContent();
        }

        function updateItemValue(type, index, field, value) {
            if (type === 'edu') educationList[index][field] = value;
            if (type === 'cert') certificatesList[index][field] = value;
            if (type === 'proj') projectsList[index][field] = value;
            updateContent();
        }

        // Generar el código Markdown final basándonos en los datos dinámicos
        function buildMarkdown() {
            const name = document.getElementById('userName').value || 'Mi Nombre';
            const title = document.getElementById('userTitle').value || 'Estudiante';
            const github = document.getElementById('userGithub').value || 'tu_usuario';
            const bio = document.getElementById('userBio').value || 'Mi bio académica.';

            let md = `# 🎓 Mis Estudios y Trayectoria Académica\n\n`;
            
            // Contador de visitas (estable y limpio)
            md += `![Visitas](https://komarev.com/ghpvc/?username=${github}&color=blue&style=flat-square) &nbsp;\n`;
            md += `![Estudios Badge](https://img.shields.io/badge/Formaci%C3%B3n-En%20Progreso-brightgreen?style=flat-square&logo=gitbook&logoColor=white)\n\n`;

            md += `### Hola, soy ${name} 👋\n\n`;
            md += `> ${title}\n\n`;
            md += `${bio}\n\n`;
            
            md += `***\n\n`;

            // Sección Educación
            md += `## 🏫 Historial Académico y Formación\n\n`;
            if (educationList.length === 0) {
                md += `*Actualmente editando formación...*\n\n`;
            } else {
                md += `| Título / Carrera | Institución / Centro | Período | Logros e Información clave |\n`;
                md += `| :--- | :--- | :--- | :--- |\n`;
                educationList.forEach(edu => {
                    md += `| **${edu.career}** | ${edu.school} | \`${edu.period}\` | ${edu.detail} |\n`;
                });
                md += `\n`;
            }

            // Sección Certificaciones
            md += `## 📜 Certificaciones y Cursos Complementarios\n\n`;
            if (certificatesList.length === 0) {
                md += `*Actualmente editando cursos complementarios...*\n\n`;
            } else {
                certificatesList.forEach(cert => {
                    md += `- **${cert.name}** — *${cert.provider}* (\`${cert.date}\`)\n`;
                });
                md += `\n`;
            }

            // Sección Proyectos
            md += `## 🛠️ Proyectos de Aprendizaje Relacionados\n\n`;
            if (projectsList.length === 0) {
                md += `*Actualmente editando proyectos...*\n\n`;
            } else {
                projectsList.forEach(proj => {
                    md += `### 🚀 ${proj.name}\n`;
                    md += `- **Tecnologías:** \`${proj.tech}\`\n`;
                    md += `- **Descripción:** ${proj.detail}\n\n`;
                });
            }

            md += `***\n\n`;
            md += `[⬅️ Volver a mi README Principal](./README.md)\n`;

            return md;
        }

        // Renderizar el contenido final
        function updateContent() {
            const markdown = buildMarkdown();
            
            // Actualizar vista previa HTML usando la librería Marked.js
            document.getElementById('githubRender').innerHTML = marked.parse(markdown);
            
            // Actualizar vista previa de código plano
            document.getElementById('markdownCode').innerText = markdown;
        }

        // Función de copiar al portapapeles con feedback visual
        function exportMarkdown() {
            const codeText = buildMarkdown();
            navigator.clipboard.writeText(codeText).then(() => {
                alert('¡Código Markdown copiado al portapapeles de manera exitosa! Listo para pegar en tu archivo "estudios.md" en GitHub.');
            }).catch(err => {
                alert('Hubo un error al copiar el código automáticamente. Por favor, selecciónalo manualmente desde la pestaña de código.');
            });
        }
    </script>
</body>
</html>
