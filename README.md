<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestión de Turnos</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f7f7f7;
        }

        header {
            background-color: #0078D4;
            color: white;
            padding: 10px 20px;
            text-align: center;
            font-size: 24px;
        }

        nav {
            display: flex;
            justify-content: center;
            background: #004085;
            padding: 10px;
        }

        nav a {
            margin: 0 15px;
            color: white;
            text-decoration: none;
            font-size: 18px;
            padding: 5px 10px;
            border-radius: 5px;
            transition: background-color 0.3s ease;
        }

        nav a:hover {
            background-color: #016db3;
        }

        section {
            padding: 20px;
        }

        .hidden {
            display: none;
        }

        .fecha-display {
            text-align: center;
            margin: 20px 0;
            padding: 15px;
            background: white;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            font-size: 1.2em;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .fecha-display span {
            margin: 0 10px;
            font-weight: bold;
            color: #004085;
        }

        .turnos {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 20px;
            padding: 15px;
        }

        .turno {
            background-color: #e0e0e0;
            text-align: left;
            padding: 15px;
            border-radius: 5px;
            cursor: pointer;
        }

        .turno.libre {
            background-color: #8bc34a;
        }

        .turno.ocupado {
            background-color: #ff5252;
            color: white;
            cursor: not-allowed;
        }

        .turno:hover:not(.ocupado) {
            background-color: #afe2af;
        }
        
        .turno.ocupado:hover {
            background-color: #e53935; /* Slightly darker red for hover on occupied */
        }


        .especialistas, .farmacias {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }

        .card {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 15px;
            background: white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .contador {
            margin-top: 10px;
            font-size: 18px;
        }
        
        .auth-modal { /* Generic class for login and registration modals */
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            padding: 20px;
            border-radius: 5px;
            display: none;
            z-index: 1000;
            width: 320px; /* Ensure consistent width */
        }
        
        .auth-modal.show {
            display: block;
        }

        #overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            display: none;
            z-index: 999;
        }

        #overlay.show {
            display: block;
        }

        .auth-modal input, .auth-modal select, .auth-modal button {
            width: calc(100% - 22px); /* Account for padding and border */
            margin: 8px 0;
            padding: 10px;
            box-sizing: border-box; /* Add box-sizing */
        }
        
        .auth-modal button {
            background-color: #0078D4;
            color: white;
            border: none;
            cursor: pointer;
        }
        .auth-modal button:hover {
            background-color: #016db3;
        }
        .auth-modal .secondary-action {
            background-color: #6c757d;
        }
        .auth-modal .secondary-action:hover {
            background-color: #5a6268;
        }
        .auth-modal a {
            display: block;
            text-align: center;
            margin-top: 10px;
            color: #0078D4;
            cursor: pointer;
            font-size: 0.9em;
        }
        .auth-modal a:hover {
            text-decoration: underline;
        }
        .auth-modal h2 {
            margin-top: 0;
            color: #004085;
            text-align: center;
        }

        #registrationModal { 
            background-color: #f0f0f0; 
        }
        #passwordRecoveryEmailModal, #passwordResetModal { /* Style for new recovery modals */
            background-color: #f0f0f0;
        }
        .password-requirements {
            font-size: 0.8em;
            color: #555;
            margin-top: 5px;
            margin-bottom: 10px;
            text-align: left;
            padding-left: 2px; 
        }

        .medicos-disponibles {
            margin-top: 20px;
            padding: 15px;
            background: white;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .medicos-disponibles h3 {
            color: #004085;
            margin-bottom: 15px;
        }

        .medicos-semana {
            border-left: 1px solid #ddd;
            padding-left: 20px;
        }

        .dia-medicos {
            margin-bottom: 15px;
        }

        .dia-medicos h4 {
            color: #0078D4;
            margin: 5px 0;
        }

        .dia-medicos p {
            margin: 3px 0;
            font-size: 0.9em;
        }

        .turno-info {
            display: none;
            margin-top: 10px;
        }

        .turno.ocupado .turno-info {
            display: block;
        }

        .turno-info input {
            width: 100%;
            margin: 5px 0;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 3px;
            box-sizing: border-box;
        }

        .turno-hora {
            font-weight: bold;
            color: #004085;
        }

        .dia-columna {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .dia-header {
            background-color: #004085;
            color: white;
            padding: 10px;
            text-align: center;
            border-radius: 5px;
            margin-bottom: 5px;
        }

        .turno-modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            z-index: 1001; /* Higher than auth modals */
            display: none;
            width: 300px;
        }

        .turno-modal.show {
            display: block;
        }

        .turno-modal input {
            width: calc(100% - 18px); /* Account for padding and border */
            margin: 10px 0;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }

        .turno-modal button {
            width: 100%;
            padding: 10px;
            background: #0078D4;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
        }

        .turno-modal button:hover {
            background: #016db3;
        }

        .turno-modal h3 {
            margin-top: 0;
            color: #004085;
        }

        .turno-modal label { 
            display: block;
            margin-top: 10px;
            margin-bottom: 3px;
            font-weight: bold;
            color: #004085;
        }

        .turno-modal textarea { 
            width: calc(100% - 18px);
            margin: 5px 0 10px 0;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
            resize: vertical;
            min-height: 80px; 
        }

        .modal-buttons {
            display: flex;
            gap: 10px;
        }

        .modal-buttons button {
            flex: 1;
        }

        .cancelar-btn {
            background: #dc3545 !important;
        }

        .cancelar-btn:hover {
            background: #c82333 !important;
        }

        /* Estilos para la mensajería */
        #section-mensajeria h2 {
            color: #004085;
            text-align: center;
            margin-bottom: 20px;
        }
        .mensajeria-container {
            display: flex;
            gap: 20px;
            height: 60vh; /* Altura fija para el contenedor de mensajería */
            background: white;
            padding: 15px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .lista-contactos {
            flex: 1;
            border-right: 1px solid #ddd;
            padding-right: 15px;
            overflow-y: auto;
        }
        .lista-contactos h3 {
            color: #0078D4;
            margin-top: 0;
        }
        #contactList {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        #contactList li {
            padding: 10px;
            cursor: pointer;
            border-bottom: 1px solid #eee;
            border-radius: 3px;
        }
        #contactList li:hover, #contactList li.active {
            background-color: #e9ecef;
        }
        .area-chat {
            flex: 3;
            display: flex;
            flex-direction: column;
        }
        .area-chat h3 {
            color: #0078D4;
            margin-top: 0;
            margin-bottom: 10px;
            padding-bottom: 10px;
            border-bottom: 1px solid #ddd;
        }
        .message-display {
            flex-grow: 1;
            overflow-y: auto;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            margin-bottom: 10px;
            background-color: #f9f9f9;
        }
        .message-item {
            padding: 8px 12px;
            margin-bottom: 8px;
            border-radius: 15px;
            max-width: 70%;
            word-wrap: break-word;
        }
        .message-item strong {
            display: block;
            font-size: 0.8em;
            margin-bottom: 3px;
            color: #555;
        }
        .message-item.sent {
            background-color: #0078D4;
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 5px;
        }
        .message-item.sent strong {
            color: #e0e0e0;
        }
        .message-item.received {
            background-color: #e0e0e0;
            color: #333;
            margin-right: auto;
            border-bottom-left-radius: 5px;
        }
        .message-item.receta {
            border-left: 4px solid #ffc107; 
            font-style: italic;
        }
        .message-item.archivo { 
            border-left: 4px solid #28a745; 
            background-color: #f0fff0; 
        }
        .message-item.archivo::before { 
            content: "📄 ";
            font-weight: bold;
            padding-right: 5px;
        }
        .message-input-area {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        #messageInput {
            width: calc(100% - 20px); /* Full width minus padding */
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            resize: vertical;
            min-height: 60px;
            box-sizing: border-box;
        }
        .message-buttons {
            display: flex;
            gap: 10px;
        }
        .message-buttons button {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            background-color: #0078D4;
            color: white;
        }
        .message-buttons button:hover {
            background-color: #016db3;
        }
        #sendRecetaBtn {
            background-color: #ffc107;
            color: #333;
        }
        #sendRecetaBtn:hover {
            background-color: #e0a800;
        }
        #sendArchivoBtn { 
            background-color: #17a2b8; 
            color: white;
        }
        #sendArchivoBtn:hover {
            background-color: #138496;
        }
    </style>
</head>
<body>
    <header>Aplicación de Gestión de Turnos</header>
    <nav>
        <a href="#" id="link-turnos">Turnos</a>
        <a href="#" id="link-especialistas">Especialistas</a>
        <a href="#" id="link-farmacias">Farmacias</a>
        <a href="#" id="link-mensajeria" class="hidden">Mensajería</a>
        <a href="#" id="link-pacientes" class="hidden">Mis Pacientes</a>
    </nav>

    <!-- Overlay for modals -->
    <div id="overlay"></div>

    <!-- Modal for Login -->
    <div id="loginModal" class="auth-modal">
        <h2>Iniciar Sesión</h2>
        <input type="text" id="username" placeholder="Nombre de Usuario">
        <input type="password" id="password" placeholder="Contraseña">
        <select id="userRoleLogin" style="width: 100%; margin: 10px 0; padding: 10px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box;">
            <option value="usuario">Usuario</option>
            <option value="medico">Médico</option>
            <option value="farmacia">Farmacia</option>
        </select>
        <button id="loginButton">Ingresar</button>
        <button id="showRegisterModalButton" class="secondary-action">Registrarse</button>
        <a href="#" id="forgotPasswordLink">¿Has olvidado tu contraseña?</a>
    </div>

    <!-- Modal for Registration -->
    <div id="registrationModal" class="auth-modal">
        <h2>Registrarse</h2>
        <input type="text" id="regUsername" placeholder="Nombre de Usuario">
        <input type="email" id="regEmail" placeholder="Correo Electrónico">
        <input type="password" id="regPassword" placeholder="Contraseña">
        <p class="password-requirements">La contraseña debe tener al menos 8 caracteres y contener al menos una letra mayúscula.</p>
        <input type="password" id="regConfirmPassword" placeholder="Confirmar Contraseña">
        <select id="regUserRole" style="width: 100%; margin: 10px 0; padding: 10px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box;">
            <option value="usuario">Usuario</option>
            <option value="medico">Médico</option>
            <option value="farmacia">Farmacia</option>
        </select>
        <button id="registerButton">Registrar</button>
        <button id="cancelRegisterButton" class="secondary-action">Cancelar</button>
    </div>

    <!-- Modal for Password Recovery Step 1: Email Input -->
    <div id="passwordRecoveryEmailModal" class="auth-modal">
        <h2>Recuperar Contraseña</h2>
        <input type="email" id="recoveryEmail" placeholder="Correo Electrónico Registrado">
        <button id="sendRecoveryLinkButton">Enviar Enlace de Recuperación</button>
        <button id="cancelRecoveryEmailButton" class="secondary-action">Cancelar</button>
    </div>

    <!-- Modal for Password Recovery Step 2: New Password Input -->
    <div id="passwordResetModal" class="auth-modal">
        <h2>Establecer Nueva Contraseña</h2>
        <input type="password" id="newPasswordRecovery" placeholder="Nueva Contraseña">
        <p class="password-requirements">La contraseña debe tener al menos 8 caracteres y contener al menos una letra mayúscula.</p>
        <input type="password" id="confirmNewPasswordRecovery" placeholder="Confirmar Nueva Contraseña">
        <button id="resetPasswordButton">Restablecer Contraseña</button>
        <button id="cancelResetPasswordButton" class="secondary-action">Cancelar</button>
    </div>

    <section id="section-turnos">
        <h2>Gestión de Turnos</h2>
        <div id="fechaNavegacion" class="fecha-display">
            <button id="prevWeekBtn" style="padding: 10px 15px; background-color: #0078D4; color: white; border: none; border-radius: 5px; cursor: pointer;">&lt; Semana Anterior</button>
            <span id="semanaActualDisplay"></span>
            <button id="nextWeekBtn" style="padding: 10px 15px; background-color: #0078D4; color: white; border: none; border-radius: 5px; cursor: pointer;">Semana Siguiente &gt;</button>
        </div>
        <p>(Haz clic en un turno libre para asignarlo y en un turno ocupado para cancelarlo)</p>
        <div class="turnos" id="calendario"></div>
        <div class="contador" id="contador"></div>
        <div class="medicos-disponibles">
            <div>
                <h3 id="medicosDiaTitulo">Médicos de turno:</h3>
                <div id="medicos-dia" class="especialistas"></div>
            </div>
            <div class="medicos-semana">
                <h3>Próximos días:</h3>
                <div id="medicos-proximos"></div>
            </div>
        </div>
    </section>

    <section id="section-especialistas" class="hidden">
        <h2>Especialistas</h2>
        <div class="especialistas">
            <div class="card">
                <h3>Dr. Juan Pérez</h3>
                <p>Profesión: Dermatólogo</p>
                <p>Edad: 45 años</p>
                <p>Teléfono: (123) 456-7890</p>
            </div>
            <div class="card">
                <h3>Dra. Ana Gómez</h3>
                <p>Profesión: Pediatra</p>
                <p>Edad: 38 años</p>
                <p>Teléfono: (987) 654-3210</p>
            </div>
            <!-- Más especialistas pueden ser añadidos aquí -->
        </div>
    </section>

    <section id="section-farmacias" class="hidden">
        <h2>Farmacias de Turno</h2>
        <div class="farmacias">
            <div class="card">
                <h3>Farmacia Central</h3>
                <p>Dirección: Av. Principal 123</p>
                <p>Horario: 24 horas</p>
            </div>
            <div class="card">
                <h3>Farmacia Nueva Vida</h3>
                <p>Dirección: Calle Secundaria 456</p>
                <p>Horario: 8:00 a 22:00</p>
            </div>
            <!-- Más farmacias pueden ser añadidas aquí -->
        </div>
    </section>

    <section id="section-mensajeria" class="hidden">
        <h2>Mensajería Interna</h2>
        <div class="mensajeria-container">
            <div class="lista-contactos">
                <h3>Contactos</h3>
                <ul id="contactList">
                    <!-- Los contactos se cargarán aquí dinámicamente -->
                </ul>
            </div>
            <div class="area-chat">
                <h3 id="chattingWith">Selecciona un contacto para chatear</h3>
                <div id="messageDisplay" class="message-display">
                    <!-- Los mensajes se mostrarán aquí -->
                </div>
                <div class="message-input-area">
                    <textarea id="messageInput" placeholder="Escribe tu mensaje o receta aquí..."></textarea>
                    <div class="message-buttons">
                        <button id="sendMessageBtn">Enviar Mensaje</button>
                        <button id="sendRecetaBtn" class="hidden">Enviar como Receta</button>
                        <button id="sendArchivoBtn" class="hidden">Enviar Archivo/Planilla</button> 
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section id="section-pacientes" class="hidden">
        <h2>Mis Pacientes Agendados</h2>
        <div id="lista-pacientes" class="especialistas" style="grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));">
            <!-- Patient cards will be generated here by JavaScript -->
        </div>
    </section>

    <div class="turno-modal" id="turnoModal">
        <h3>Datos del Turno</h3>
        <input type="text" id="nombrePaciente" placeholder="Nombre completo">
        <input type="text" id="direccionPaciente" placeholder="Dirección">
        <input type="tel" id="telefonoPaciente" placeholder="Teléfono">
        <label for="profesionalSelect" style="display: block; margin-top: 10px; margin-bottom: 3px; font-weight: bold; color: #004085;">Profesional:</label>
        <select id="profesionalSelect" style="width: calc(100% - 18px); margin: 5px 0 10px 0; padding: 8px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box;">
            <option value="">Seleccione un Profesional</option>
        </select>
        <div class="modal-buttons">
            <button id="guardarTurno">Guardar Turno</button>
            <button id="cancelarTurnoModal" class="cancelar-btn">Cancelar</button> 
        </div>
    </div>

    <!-- Modal for Receta -->
    <div id="recetaModal" class="turno-modal">
        <h3>Crear Receta Médica</h3>
        
        <label for="recetaFecha">Fecha:</label>
        <input type="text" id="recetaFecha" readonly>

        <label for="recetaMedico">Médico:</label>
        <input type="text" id="recetaMedico" readonly>

        <label for="recetaPaciente">Paciente:</label>
        <input type="text" id="recetaPaciente" placeholder="Nombre del Paciente">

        <label for="recetaDetalle">Detalle:</label>
        <textarea id="recetaDetalle" placeholder="Indicaciones, medicamentos, etc." rows="5"></textarea>

        <label for="recetaFirma">Firma del Médico:</label>
        <input type="text" id="recetaFirma" placeholder="Firma (Nombre del Médico)">

        <div class="modal-buttons">
            <button id="enviarRecetaModalBtn">Enviar Receta</button>
            <button id="cancelarRecetaModalBtn" class="cancelar-btn">Cancelar</button>
        </div>
    </div>

    <script>
        const secciones = {
            turnos: document.getElementById('section-turnos'),
            especialistas: document.getElementById('section-especialistas'),
            farmacias: document.getElementById('section-farmacias'),
            mensajeria: document.getElementById('section-mensajeria'),
            pacientes: document.getElementById('section-pacientes'), 
        };

        const enlaces = {
            turnos: document.getElementById('link-turnos'),
            especialistas: document.getElementById('link-especialistas'),
            farmacias: document.getElementById('link-farmacias'),
            mensajeria: document.getElementById('link-mensajeria'),
            pacientes: document.getElementById('link-pacientes'), 
        };

        let currentUser = null; 
        let currentWeekStartDate;
        const turnosAgendados = {}; 
        const turnos = []; 

        // Datos para mensajería y usuarios
        const messagesStore = {}; // { "userA_userB": [messageObj, ...], ... }
        let currentChatPartner = null;
        
        // Initial users with passwords. In a real app, this would come from a secure backend.
        const allKnownUsers = [ 
            { name: "Paciente Ejemplo", role: "usuario", password: "password123", email: "paciente@example.com" }, 
            { name: "Dr. Juan Pérez", role: "medico", especialidad: "Dermatología", password: "medicoPassword1", email: "juanperez@example.com" },
            { name: "Dra. Ana Gómez", role: "medico", especialidad: "Pediatría", password: "medicoPassword2", email: "anagomez@example.com" },
            { name: "Dr. Carlos Ruiz", role: "medico", especialidad: "Traumatología", password: "medicoPassword3", email: "carlosruiz@example.com"},
            { name: "Farmacia Central", role: "farmacia", direccion: "Av. Principal 123", password: "farmaciaPassword1", email: "farmaciacentral@example.com" },
            { name: "Farmacia Nueva Vida", role: "farmacia", direccion: "Calle Secundaria 456", password: "farmaciaPassword2", email: "farmacianuevavida@example.com" }
        ];

        const loginModal = document.getElementById('loginModal');
        const registrationModal = document.getElementById('registrationModal');
        const overlay = document.getElementById('overlay');
        const passwordRecoveryEmailModal = document.getElementById('passwordRecoveryEmailModal');
        const passwordResetModal = document.getElementById('passwordResetModal');
        let emailForPasswordReset = null; // To store email for password reset process

        function showLoginModal() {
            hideRegistrationModal(); // Ensure registration modal is hidden
            hidePasswordRecoveryEmailModal();
            hidePasswordResetModal();
            loginModal.classList.add('show');
            overlay.classList.add('show');
        }

        function hideLoginModal() {
            loginModal.classList.remove('show');
            if (!registrationModal.classList.contains('show') && 
                !turnoModal.classList.contains('show') && 
                !recetaModal.classList.contains('show') &&
                !passwordRecoveryEmailModal.classList.contains('show') &&
                !passwordResetModal.classList.contains('show')) {
               overlay.classList.remove('show');
            }
        }
        
        function showRegistrationModal() {
            hideLoginModal();
            hidePasswordRecoveryEmailModal();
            hidePasswordResetModal();
            registrationModal.classList.add('show');
            overlay.classList.add('show');
        }

        function hideRegistrationModal() {
            registrationModal.classList.remove('show');
             if (!loginModal.classList.contains('show') && 
                 !turnoModal.classList.contains('show') && 
                 !recetaModal.classList.contains('show') &&
                 !passwordRecoveryEmailModal.classList.contains('show') &&
                 !passwordResetModal.classList.contains('show')) {
               overlay.classList.remove('show');
            }
        }

        function showPasswordRecoveryEmailModal() {
            hideLoginModal();
            hideRegistrationModal();
            hidePasswordResetModal();
            passwordRecoveryEmailModal.classList.add('show');
            overlay.classList.add('show');
            document.getElementById('recoveryEmail').value = ''; // Clear field
        }

        function hidePasswordRecoveryEmailModal() {
            passwordRecoveryEmailModal.classList.remove('show');
            if (!loginModal.classList.contains('show') && 
                !registrationModal.classList.contains('show') && 
                !turnoModal.classList.contains('show') && 
                !recetaModal.classList.contains('show') &&
                !passwordResetModal.classList.contains('show')) {
               overlay.classList.remove('show');
            }
        }
        
        function showPasswordResetModal(email) {
            emailForPasswordReset = email;
            hidePasswordRecoveryEmailModal();
            passwordResetModal.classList.add('show');
            overlay.classList.add('show'); // Ensure overlay remains if it was hidden
            document.getElementById('newPasswordRecovery').value = '';
            document.getElementById('confirmNewPasswordRecovery').value = '';
        }

        function hidePasswordResetModal() {
            passwordResetModal.classList.remove('show');
            emailForPasswordReset = null;
            if (!loginModal.classList.contains('show') && 
                !registrationModal.classList.contains('show') && 
                !turnoModal.classList.contains('show') && 
                !recetaModal.classList.contains('show') &&
                !passwordRecoveryEmailModal.classList.contains('show')) {
               overlay.classList.remove('show');
            }
        }

        const loginButton = document.getElementById('loginButton');
        loginButton.addEventListener('click', () => {
            const usernameInput = document.getElementById('username');
            const passwordInput = document.getElementById('password');
            const roleSelect = document.getElementById('userRoleLogin'); // Use the correct ID
            
            const username = usernameInput.value.trim();
            const password = passwordInput.value;
            const role = roleSelect.value;

            if (!username || !password) {
                alert('Por favor introduce nombre de usuario y contraseña.');
                return;
            }

            const user = allKnownUsers.find(u => u.name === username && u.role === role);

            if (user && user.password === password) {
                currentUser = user; // Store the full user object
                alert(`Bienvenido, ${currentUser.name} (Rol: ${currentUser.role})`);
                hideLoginModal();
                initializeCalendarApp(); 

                enlaces.mensajeria.classList.add('hidden'); // Default to hidden
                enlaces.pacientes.classList.add('hidden'); // Default to hidden

                if (currentUser.role === 'medico') {
                    enlaces.mensajeria.classList.remove('hidden');
                    enlaces.pacientes.classList.remove('hidden'); // Show "Mis Pacientes" for doctors
                } else if (currentUser.role === 'farmacia') {
                    enlaces.mensajeria.classList.remove('hidden');
                }
                
                // Ensure sections are hidden if links are hidden
                if (enlaces.mensajeria.classList.contains('hidden') && !secciones.mensajeria.classList.contains('hidden')) {
                    secciones.mensajeria.classList.add('hidden');
                }
                if (enlaces.pacientes.classList.contains('hidden') && !secciones.pacientes.classList.contains('hidden')) {
                    secciones.pacientes.classList.add('hidden');
                }
                
                // Clear login form fields
                usernameInput.value = '';
                passwordInput.value = '';
                
            } else {
                alert('Nombre de usuario, contraseña o rol incorrectos.');
            }
        });
        
        const showRegisterModalButton = document.getElementById('showRegisterModalButton');
        showRegisterModalButton.addEventListener('click', showRegistrationModal);

        const registerButton = document.getElementById('registerButton');
        registerButton.addEventListener('click', () => {
            const regUsernameInput = document.getElementById('regUsername');
            const regEmailInput = document.getElementById('regEmail');
            const regPasswordInput = document.getElementById('regPassword');
            const regConfirmPasswordInput = document.getElementById('regConfirmPassword');
            const regUserRoleSelect = document.getElementById('regUserRole');

            const username = regUsernameInput.value.trim();
            const email = regEmailInput.value.trim();
            const password = regPasswordInput.value;
            const confirmPassword = regConfirmPasswordInput.value;
            const role = regUserRoleSelect.value;

            if (!username || !password || !confirmPassword || !email) { 
                alert('Por favor completa todos los campos, incluyendo el correo electrónico.');
                return;
            }
            // Basic email validation
            if (!email.includes('@') || !email.includes('.')) {
                alert('Por favor introduce un correo electrónico válido.');
                return;
            }
            if (password !== confirmPassword) {
                alert('Las contraseñas no coinciden.');
                return;
            }
            if (password.length < 8 || !/[A-Z]/.test(password)) {
                alert('La contraseña debe tener al menos 8 caracteres y contener al menos una letra mayúscula.');
                return;
            }
            if (allKnownUsers.some(u => u.name === username)) {
                alert('El nombre de usuario ya existe. Por favor elige otro.');
                return;
            }
            if (allKnownUsers.some(u => u.email === email)) {
                alert('Este correo electrónico ya está registrado. Por favor usa otro.');
                return;
            }

            const newUser = { name: username, role: role, password: password, email: email }; 
            // Add special properties based on role if needed for other parts of the app
            if (role === 'medico') newUser.especialidad = "General"; // Default or prompt for more info
            if (role === 'farmacia') newUser.direccion = "No especificada";

            allKnownUsers.push(newUser);
            alert(`Registro exitoso para ${username}. Se ha enviado un correo de verificación a ${email}.`);
            
            // Clear registration form fields
            regUsernameInput.value = '';
            regEmailInput.value = '';
            regPasswordInput.value = '';
            regConfirmPasswordInput.value = '';
            
            hideRegistrationModal();
            showLoginModal(); // Show login modal again
        });

        const cancelRegisterButton = document.getElementById('cancelRegisterButton');
        cancelRegisterButton.addEventListener('click', () => {
            hideRegistrationModal();
            showLoginModal(); // Go back to login modal
        });

        const forgotPasswordLink = document.getElementById('forgotPasswordLink');
        forgotPasswordLink.addEventListener('click', (e) => {
            e.preventDefault();
            showPasswordRecoveryEmailModal();
        });

        const sendRecoveryLinkButton = document.getElementById('sendRecoveryLinkButton');
        sendRecoveryLinkButton.addEventListener('click', () => {
            const recoveryEmailInput = document.getElementById('recoveryEmail');
            const email = recoveryEmailInput.value.trim();

            if (!email) {
                alert('Por favor, introduce tu correo electrónico.');
                return;
            }
            if (!email.includes('@') || !email.includes('.')) {
                alert('Por favor introduce un correo electrónico válido.');
                return;
            }

            const userExists = allKnownUsers.find(user => user.email === email);
            if (userExists) {
                alert(`Se ha encontrado una cuenta para ${email}. Serás redirigido para establecer una nueva contraseña.`);
                showPasswordResetModal(email);
            } else {
                alert('No se encontró ninguna cuenta asociada a este correo electrónico. Por favor, verifica el correo o regístrate.');
            }
        });

        const cancelRecoveryEmailButton = document.getElementById('cancelRecoveryEmailButton');
        cancelRecoveryEmailButton.addEventListener('click', () => {
            hidePasswordRecoveryEmailModal();
            showLoginModal();
        });

        const resetPasswordButton = document.getElementById('resetPasswordButton');
        resetPasswordButton.addEventListener('click', () => {
            if (!emailForPasswordReset) {
                alert('Error: No se ha especificado un correo para el reseteo. Por favor, intente de nuevo.');
                hidePasswordResetModal();
                showPasswordRecoveryEmailModal();
                return;
            }

            const newPasswordInput = document.getElementById('newPasswordRecovery');
            const confirmNewPasswordInput = document.getElementById('confirmNewPasswordRecovery');
            const newPassword = newPasswordInput.value;
            const confirmNewPassword = confirmNewPasswordInput.value;

            if (!newPassword || !confirmNewPassword) {
                alert('Por favor, completa ambos campos de contraseña.');
                return;
            }
            if (newPassword !== confirmNewPassword) {
                alert('Las nuevas contraseñas no coinciden.');
                return;
            }
            if (newPassword.length < 8 || !/[A-Z]/.test(newPassword)) {
                alert('La nueva contraseña debe tener al menos 8 caracteres y contener al menos una letra mayúscula.');
                return;
            }

            const userToUpdate = allKnownUsers.find(user => user.email === emailForPasswordReset);
            if (userToUpdate) {
                userToUpdate.password = newPassword; // Update password in memory
                alert('Tu contraseña ha sido restablecida exitosamente. Ahora puedes iniciar sesión con tu nueva contraseña.');
                hidePasswordResetModal();
                showLoginModal();
            } else {
                // This case should ideally not happen if emailForPasswordReset was validated before
                alert('Error: No se pudo encontrar el usuario para actualizar la contraseña. Intente de nuevo.');
                hidePasswordResetModal();
                showPasswordRecoveryEmailModal();
            }
        });

        const cancelResetPasswordButton = document.getElementById('cancelResetPasswordButton');
        cancelResetPasswordButton.addEventListener('click', () => {
            hidePasswordResetModal();
            showLoginModal(); // Or showPasswordRecoveryEmailModal() if you want them to go back one step
        });

        window.addEventListener('load', () => {
            showLoginModal();
        });

        function getStartOfWeek(date) {
            const d = new Date(date);
            const day = d.getDay();
            const diff = d.getDate() - day + (day === 0 ? -6 : 1); 
            return new Date(d.setDate(diff));
        }

        function formatDateForDisplay(date) {
            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = date.getFullYear();
            return `${day}/${month}/${year}`;
        }
        
        function getISODateString(date) {
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            return `${year}-${month}-${day}`;
        }

        function getDayNameFromDate(date) {
            const days = ['Domingo', 'Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado'];
            return days[date.getDay()];
        }

        function initializeCalendarApp() {
            currentWeekStartDate = getStartOfWeek(new Date());
            document.getElementById('prevWeekBtn').addEventListener('click', () => changeWeek(-7));
            document.getElementById('nextWeekBtn').addEventListener('click', () => changeWeek(7));
            renderCurrentWeekView();
        }

        function changeWeek(offsetDays) {
            currentWeekStartDate.setDate(currentWeekStartDate.getDate() + offsetDays);
            renderCurrentWeekView();
        }

        function renderCurrentWeekView() {
            updateWeekDisplay();
            populateCalendarGrid();
            updateDoctorInfoForDate(currentWeekStartDate); 
            actualizarContador();
        }

        function updateWeekDisplay() {
            const displaySpan = document.getElementById('semanaActualDisplay');
            const endDate = new Date(currentWeekStartDate);
            endDate.setDate(currentWeekStartDate.getDate() + 6);
            displaySpan.textContent = `Semana del ${formatDateForDisplay(currentWeekStartDate)} al ${formatDateForDisplay(endDate)}`;
        }
        
        const medicosPorDia = {
            'Lunes': [
                { nombre: 'Dr. Juan Pérez', especialidad: 'Dermatología' },
                { nombre: 'Dra. María López', especialidad: 'Cardiología' }
            ],
            'Martes': [
                { nombre: 'Dra. Ana Gómez', especialidad: 'Pediatría' },
                { nombre: 'Dr. Carlos Ruiz', especialidad: 'Traumatología' }
            ],
            'Miércoles': [
                { nombre: 'Dr. Luis Torres', especialidad: 'Oftalmología' },
                { nombre: 'Dra. Sandra Díaz', especialidad: 'Neurología' }
            ],
            'Jueves': [
                { nombre: 'Dr. Roberto García', especialidad: 'Psiquiatría' },
                { nombre: 'Dra. Carmen Silva', especialidad: 'Endocrinología' }
            ],
            'Viernes': [
                { nombre: 'Dr. Miguel Sánchez', especialidad: 'Ginecología' },
                { nombre: 'Dra. Patricia Muñoz', especialidad: 'Dermatología' }
            ],
            'Sábado': [],
            'Domingo': []
        };

        function updateDoctorInfoForDate(dateObj) {
            const dayName = getDayNameFromDate(dateObj);
            document.getElementById('medicosDiaTitulo').textContent = `Médicos de turno para ${dayName} ${formatDateForDisplay(dateObj)}:`;

            const medicosDiaDiv = document.getElementById('medicos-dia');
            const medicosProximosDiv = document.getElementById('medicos-proximos');
            
            medicosDiaDiv.innerHTML = '';
            medicosProximosDiv.innerHTML = '';
            
            const medicos = medicosPorDia[dayName] || [];
            if (medicos.length === 0) {
                medicosDiaDiv.innerHTML = '<p>No hay médicos programados para este día.</p>';
            } else {
                medicos.forEach(medico => {
                    const medicoCard = document.createElement('div');
                    medicoCard.className = 'card';
                    medicoCard.innerHTML = `
                        <h4>${medico.nombre}</h4>
                        <p>Especialidad: ${medico.especialidad}</p>
                    `;
                    medicosDiaDiv.appendChild(medicoCard);
                });
            }
            
            const diasSemana = ['Domingo', 'Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado'];
            const indexDiaActual = dateObj.getDay();

            for (let i = 1; i <= 4; i++) {
                const nextDayDate = new Date(dateObj);
                nextDayDate.setDate(dateObj.getDate() + i);
                const proximoDiaNombre = getDayNameFromDate(nextDayDate);
                const medicosDelDiaSiguiente = medicosPorDia[proximoDiaNombre] || [];
                
                if (medicosDelDiaSiguiente.length > 0) {
                    const diaDiv = document.createElement('div');
                    diaDiv.className = 'dia-medicos';
                    diaDiv.innerHTML = `<h4>${proximoDiaNombre} (${formatDateForDisplay(nextDayDate)}):</h4>`;
                    
                    medicosDelDiaSiguiente.forEach(medico => {
                        diaDiv.innerHTML += `
                            <p>${medico.nombre} - ${medico.especialidad}</p>
                        `;
                    });
                    medicosProximosDiv.appendChild(diaDiv);
                }
            }
        }

        const calendario = document.getElementById('calendario');
        const horarios = [
            '09:00', '10:00', '11:00', '12:00', 
            '13:00', '14:00', '15:00'
        ];

        function getTurnoInfoHTML(bookingData, role) {
            if (!bookingData) { 
                // Turno libre
                return `
                    <input type="text" placeholder="Nombre" class="turno-nombre" readonly value="">
                    <input type="text" placeholder="Dirección" class="turno-direccion" readonly value="">
                    <input type="tel" placeholder="Teléfono" class="turno-telefono" readonly value="">
                    <input type="text" placeholder="Profesional" class="turno-profesional" readonly value="">
                `;
            }

            // Turno ocupado
            const nombre = bookingData.nombrePaciente || '';
            let direccion = bookingData.direccionPaciente || '';
            let telefono = bookingData.telefonoPaciente || '';
            const profesional = bookingData.profesional || 'No asignado';

            // Determine if full details should be shown
            let showFullDetails = false;
            if (currentUser && currentUser.role === 'medico' && bookingData.profesional === currentUser.name) {
                showFullDetails = true;
            }

            if (!showFullDetails) {
                direccion = maskString(direccion);
                telefono = maskString(telefono);
            }

            return `
                <input type="text" placeholder="Nombre" class="turno-nombre" readonly value="${nombre}">
                <input type="text" placeholder="Dirección" class="turno-direccion" readonly value="${direccion}">
                <input type="tel" placeholder="Teléfono" class="turno-telefono" readonly value="${telefono}">
                <input type="text" placeholder="Profesional" class="turno-profesional" readonly value="${profesional}">
            `;
        }

        function populateCalendarGrid() {
            calendario.innerHTML = ''; 
            turnos.length = 0; 

            const currentDay = new Date(currentWeekStartDate);

            for (let i = 0; i < 7; i++) {
                const dayDate = new Date(currentDay);
                dayDate.setDate(currentDay.getDate() + i);
                const dayName = getDayNameFromDate(dayDate);
                const isoDateStr = getISODateString(dayDate);

                const diaColumna = document.createElement('div');
                diaColumna.className = 'dia-columna';
                
                const diaHeader = document.createElement('div');
                diaHeader.className = 'dia-header';
                diaHeader.textContent = `${dayName} ${formatDateForDisplay(dayDate)}`;
                diaColumna.appendChild(diaHeader);

                horarios.forEach((hora) => {
                    const turnoEl = document.createElement('div');
                    const dateTimeKey = `${isoDateStr}T${hora}`;
                    const booking = turnosAgendados[dateTimeKey];

                    turnoEl.innerHTML = `
                        <div class="turno-hora">${hora}</div>
                        ${getTurnoInfoHTML(booking, currentUser.role)}
                    `;
                    turnoEl.dataset.date = isoDateStr;
                    turnoEl.dataset.time = hora;

                    if (booking) {
                        turnoEl.className = 'turno ocupado';
                        turnoEl.dataset.estado = 'ocupado';
                        turnoEl.dataset.usuario = booking.usuario;
                    } else {
                        turnoEl.className = 'turno libre';
                        turnoEl.dataset.estado = 'libre';
                    }
                    
                    turnos.push(turnoEl); 
                    diaColumna.appendChild(turnoEl);
                });
                calendario.appendChild(diaColumna);
            }
            actualizarContador(); 
        }

        let turnoSeleccionado = null;
        const turnoModal = document.getElementById('turnoModal');
        const guardarTurnoBtn = document.getElementById('guardarTurno');
        const cancelarTurnoBtn = document.getElementById('cancelarTurnoModal');

        calendario.addEventListener('click', (e) => {
            if (!currentUser) {
                alert('Por favor inicia sesión primero.');
                return;
            }

            const turno = e.target.closest('.turno');
            if (!turno) return;

            const turnoDateStr = turno.dataset.date;
            const clickedDate = new Date(turnoDateStr + 'T00:00:00'); 
            updateDoctorInfoForDate(clickedDate);

            if (turno.dataset.estado === 'ocupado') {
                const dateTimeKey = `${turno.dataset.date}T${turno.dataset.time}`;
                const bookingData = turnosAgendados[dateTimeKey];

                // Check if the current user is the one who booked the appointment
                if (bookingData && bookingData.usuario === currentUser.name) {
                    if (confirm('¿Deseas cancelar este turno?')) {
                        delete turnosAgendados[dateTimeKey];
                        
                        turno.dataset.estado = 'libre';
                        turno.dataset.usuario = ''; // Clear the stored username
                        turno.className = 'turno libre';
                        // Clear patient info fields in the UI for this slot
                        turno.querySelector('.turno-nombre').value = '';
                        turno.querySelector('.turno-direccion').value = '';
                        turno.querySelector('.turno-telefono').value = '';
                        turno.querySelector('.turno-profesional').value = ''; // Clear professional
                        actualizarContador();
                        alert('Turno cancelado exitosamente.');
                    }
                } else {
                    // If not booked by current user, or if bookingData is somehow missing (shouldn't happen if state is 'ocupado')
                    alert('Este turno está ocupado por otro usuario o no tienes permiso para cancelarlo.');
                }
                return;
            }

            // Check if current user already has a booking in the visible week or any other rule
            const userHasBookingThisWeek = Object.values(turnosAgendados).some(
                (booking) => booking.usuario === currentUser.name
            );

            if (userHasBookingThisWeek) {
                alert('Solo puedes tener un turno agendado a la vez. Cancela tu turno actual si deseas tomar uno nuevo.');
                return;
            }

            turnoSeleccionado = turno;
            // Pre-fill patient name in modal if current user is a 'usuario'
            if (currentUser.role === 'usuario') {
                document.getElementById('nombrePaciente').value = currentUser.name;
            } else {
                document.getElementById('nombrePaciente').value = ''; // Clear for medico/farmacia
            }
            document.getElementById('direccionPaciente').value = '';
            document.getElementById('telefonoPaciente').value = '';

            // Populate professional selector
            const profesionalSelect = document.getElementById('profesionalSelect');
            profesionalSelect.innerHTML = '<option value="">Seleccione un Profesional</option>'; // Default option
            const medicos = allKnownUsers.filter(u => u.name !== currentUser.name && u.role === 'medico');
            medicos.forEach(medico => {
                const option = document.createElement('option');
                option.value = medico.name;
                option.textContent = medico.name + (medico.especialidad ? ` (${medico.especialidad})` : '');
                profesionalSelect.appendChild(option);
            });
            // Reset selected value for professional selector
            profesionalSelect.value = '';

            turnoModal.classList.add('show');
            overlay.classList.add('show'); // Ensure overlay is shown for turnoModal as well
        });

        guardarTurnoBtn.addEventListener('click', () => {
            const nombre = document.getElementById('nombrePaciente').value.trim();
            const direccion = document.getElementById('direccionPaciente').value.trim();
            const telefono = document.getElementById('telefonoPaciente').value.trim();
            const profesional = document.getElementById('profesionalSelect').value;

            if (!nombre || !direccion || !telefono || !profesional) {
                alert('Por favor completa todos los campos, incluyendo el profesional.');
                return;
            }

            const dateTimeKey = `${turnoSeleccionado.dataset.date}T${turnoSeleccionado.dataset.time}`;
            turnosAgendados[dateTimeKey] = {
                usuario: currentUser.name, // The user who booked (can be anyone)
                nombrePaciente: nombre,
                direccionPaciente: direccion,
                telefonoPaciente: telefono,
                profesional: profesional // The selected doctor
            };

            turnoSeleccionado.dataset.estado = 'ocupado';
            turnoSeleccionado.dataset.usuario = currentUser.name; // User who made the booking
            turnoSeleccionado.className = 'turno ocupado';
            
            turnoSeleccionado.querySelector('.turno-nombre').value = nombre;
            turnoSeleccionado.querySelector('.turno-direccion').value = direccion;
            turnoSeleccionado.querySelector('.turno-telefono').value = telefono;
            turnoSeleccionado.querySelector('.turno-profesional').value = profesional; // Display selected professional

            actualizarContador();
            cerrarModalTurno();
            alert(`Turno asignado a ${nombre} con ${profesional} para ${getDayNameFromDate(new Date(turnoSeleccionado.dataset.date  + 'T00:00:00'))} ${formatDateForDisplay(new Date(turnoSeleccionado.dataset.date + 'T00:00:00'))} a las ${turnoSeleccionado.dataset.time}`);
        });

        cancelarTurnoBtn.addEventListener('click', cerrarModalTurno);

        function cerrarModalTurno() {
            turnoModal.classList.remove('show');
            if (!loginModal.classList.contains('show') && 
                !registrationModal.classList.contains('show') && 
                !turnoModal.classList.contains('show') &&
                !passwordRecoveryEmailModal.classList.contains('show') &&
                !passwordResetModal.classList.contains('show')) {
                overlay.classList.remove('show');
            }
            document.getElementById('nombrePaciente').value = '';
            document.getElementById('direccionPaciente').value = '';
            document.getElementById('telefonoPaciente').value = '';
            document.getElementById('profesionalSelect').value = ''; // Reset professional selector
            turnoSeleccionado = null;
        }

        function actualizarContador() {
            const libres = turnos.filter((turnoEl) => turnoEl.dataset.estado === 'libre').length;
            const ocupados = turnos.filter((turnoEl) => turnoEl.dataset.estado === 'ocupado').length;
            const contador = document.getElementById('contador');
            contador.textContent = `Turnos libres (esta semana): ${libres} | Turnos ocupados: ${ocupados}`;
        }

        function initializeMessaging() {
            const contactList = document.getElementById('contactList');
            const chattingWithHeader = document.getElementById('chattingWith');
            const messageDisplay = document.getElementById('messageDisplay');
            const messageInput = document.getElementById('messageInput');
            const sendMessageBtn = document.getElementById('sendMessageBtn');
            const sendRecetaBtn = document.getElementById('sendRecetaBtn');
            const sendArchivoBtn = document.getElementById('sendArchivoBtn'); 

            contactList.innerHTML = ''; // Clear previous list
            chattingWithHeader.textContent = 'Selecciona un contacto para chatear';
            messageDisplay.innerHTML = '';
            sendRecetaBtn.classList.add('hidden'); // Hide buttons initially
            sendArchivoBtn.classList.add('hidden');
            currentChatPartner = null; // Reset current chat partner

            let eligibleContacts = [];
            if (currentUser.role === 'medico') {
                eligibleContacts = allKnownUsers.filter(user => 
                    user.name !== currentUser.name && (user.role === 'medico' || user.role === 'farmacia')
                );
            } else if (currentUser.role === 'farmacia') {
                eligibleContacts = allKnownUsers.filter(user => 
                    user.name !== currentUser.name && user.role === 'medico'
                );
            }

            eligibleContacts.forEach(user => {
                const contactListItem = document.createElement('li');
                contactListItem.textContent = `${user.name} (${user.role})`;
                contactListItem.dataset.userName = user.name;

                contactListItem.addEventListener('click', () => {
                    // Highlight active contact
                    document.querySelectorAll('#contactList li').forEach(li => li.classList.remove('active'));
                    contactListItem.classList.add('active');

                    currentChatPartner = allKnownUsers.find(u => u.name === user.name); // Get full user object
                    if (!currentChatPartner) return;

                    chattingWithHeader.textContent = `Chateando con ${currentChatPartner.name}`;
                    messageDisplay.innerHTML = '';
                    const chatKey = getChatKey(currentUser.name, currentChatPartner.name);

                    if (messagesStore[chatKey]) {
                        messagesStore[chatKey].forEach(message => {
                            const messageItem = document.createElement('div');
                            messageItem.classList.add('message-item');
                            if (message.sender === currentUser.name) {
                                messageItem.classList.add('sent');
                            } else {
                                messageItem.classList.add('received');
                            }
                            if (message.type === 'receta') {
                                messageItem.classList.add('receta');
                            } else if (message.type === 'archivo') { 
                                messageItem.classList.add('archivo');
                            }
                            messageItem.innerHTML = `<strong>${message.sender}</strong><p>${message.text}</p>`;
                            messageDisplay.appendChild(messageItem);
                        });
                    }
                    messageDisplay.scrollTop = messageDisplay.scrollHeight;

                    // Button visibility based on current user's role
                    sendRecetaBtn.classList.add('hidden');
                    sendArchivoBtn.classList.add('hidden');

                    if (currentUser.role === 'medico') {
                        sendRecetaBtn.classList.remove('hidden');
                    }
                    if (currentUser.role === 'medico' || currentUser.role === 'farmacia') {
                        sendArchivoBtn.classList.remove('hidden');
                    }
                });
                contactList.appendChild(contactListItem);
            });
        }

        // Receta Modal elements
        const recetaModal = document.getElementById('recetaModal');
        const recetaFechaInput = document.getElementById('recetaFecha');
        const recetaMedicoInput = document.getElementById('recetaMedico');
        const recetaPacienteInput = document.getElementById('recetaPaciente');
        const recetaDetalleTextarea = document.getElementById('recetaDetalle');
        const recetaFirmaInput = document.getElementById('recetaFirma');
        const enviarRecetaModalBtn = document.getElementById('enviarRecetaModalBtn');
        const cancelarRecetaModalBtn = document.getElementById('cancelarRecetaModalBtn');

        function showRecetaModal() {
            if (!currentUser || currentUser.role !== 'medico' || !currentChatPartner) {
                alert("Función de receta no disponible o no hay un chat activo.");
                return;
            }
            
            // Pre-fill modal fields
            recetaFechaInput.value = formatDateForDisplay(new Date());
            recetaMedicoInput.value = currentUser.name;
            // Try to prefill patient name. If chat partner is not 'usuario', doctor might need to change it.
            recetaPacienteInput.value = currentChatPartner.name; 
            
            // Transfer text from main message input to recipe detail
            const messageInput = document.getElementById('messageInput');
            recetaDetalleTextarea.value = messageInput.value;
            // messageInput.value = ''; // Optionally clear main input after transfer

            recetaFirmaInput.value = currentUser.name; // Pre-fill signature with doctor's name

            recetaModal.classList.add('show');
            overlay.classList.add('show');
        }

        function hideRecetaModal() {
            recetaModal.classList.remove('show');
            if (!loginModal.classList.contains('show') && 
                !registrationModal.classList.contains('show') && 
                !turnoModal.classList.contains('show') &&
                !passwordRecoveryEmailModal.classList.contains('show') &&
                !passwordResetModal.classList.contains('show')) {
                overlay.classList.remove('show');
            }
            // Clear fields
            recetaPacienteInput.value = '';
            recetaDetalleTextarea.value = '';
            recetaFirmaInput.value = '';
        }

        enviarRecetaModalBtn.addEventListener('click', () => {
            if (!currentChatPartner || !currentUser) return;

            const fecha = recetaFechaInput.value;
            const medico = recetaMedicoInput.value;
            const paciente = recetaPacienteInput.value.trim();
            const detalle = recetaDetalleTextarea.value.trim();
            const firma = recetaFirmaInput.value.trim();

            if (!paciente || !detalle || !firma) {
                alert("Por favor complete todos los campos de la receta (Paciente, Detalle, Firma).");
                return;
            }

            const formattedReceta = `
                **RECETA MÉDICA**<br>
                ------------------------------<br>
                Fecha: ${fecha}<br>
                Médico: ${medico}<br>
                Paciente: ${paciente}<br>
                ------------------------------<br>
                <strong>Detalle:</strong><br>
                ${detalle.replace(/\n/g, '<br>')}<br>
                ------------------------------<br>
                Firma: ${firma}
            `.trim();

            const chatKey = getChatKey(currentUser.name, currentChatPartner.name);
            const newRecetaMessage = {
                sender: currentUser.name,
                text: formattedReceta,
                type: 'receta'
            };

            if (!messagesStore[chatKey]) {
                messagesStore[chatKey] = [];
            }
            messagesStore[chatKey].push(newRecetaMessage);

            const messageDisplay = document.getElementById('messageDisplay');
            const messageItem = document.createElement('div');
            messageItem.classList.add('message-item', 'sent', 'receta');
            messageItem.innerHTML = `<strong>${newRecetaMessage.sender}</strong><p>${newRecetaMessage.text}</p>`;
            messageDisplay.appendChild(messageItem);
            messageDisplay.scrollTop = messageDisplay.scrollHeight;

            // Clear the main message input if its content was used for the recipe
            document.getElementById('messageInput').value = '';


            hideRecetaModal();
        });

        cancelarRecetaModalBtn.addEventListener('click', hideRecetaModal);

        // Helper function for generating a consistent chat key
        function getChatKey(user1Name, user2Name) {
            return [user1Name, user2Name].sort().join('_');
        }

        function renderPacientesSection() {
            if (!currentUser || currentUser.role !== 'medico') {
                secciones.pacientes.innerHTML = '<p>Acceso denegado. Esta sección es solo para médicos.</p>';
                return;
            }

            const listaPacientesDiv = document.getElementById('lista-pacientes');
            const misPacientes = [];

            for (const dateTimeKey in turnosAgendados) {
                const turno = turnosAgendados[dateTimeKey];
                // Filter appointments where the logged-in doctor is the assigned professional
                if (turno.profesional === currentUser.name) {
                    const [dateStr, timeStr] = dateTimeKey.split('T');
                    // Create a proper Date object for formatting, ensuring correct local timezone interpretation
                    const dateObj = new Date(dateStr + 'T00:00:00Z'); // Use Z for UTC to avoid timezone shifts from string
                    
                    misPacientes.push({
                        ...turno,
                        fecha: formatDateForDisplay(dateObj), 
                        hora: timeStr
                    });
                }
            }

            listaPacientesDiv.innerHTML = ''; // Clear previous content

            if (misPacientes.length === 0) {
                listaPacientesDiv.innerHTML = '<p>Aún no tienes pacientes agendados.</p>';
                return;
            }

            // Sort patients by date and time
            misPacientes.sort((a, b) => {
                const dateAParts = a.fecha.split('/');
                const dateBParts = b.fecha.split('/');
                const dateAStr = `${dateAParts[2]}-${dateAParts[1]}-${dateAParts[0]}T${a.hora}`;
                const dateBStr = `${dateBParts[2]}-${dateBParts[1]}-${dateBParts[0]}T${b.hora}`;
                const dateA = new Date(dateAStr);
                const dateB = new Date(dateBStr);
                return dateA - dateB;
            });

            misPacientes.forEach(paciente => {
                const pacienteCard = document.createElement('div');
                pacienteCard.className = 'card';
                pacienteCard.innerHTML = `
                    <h4>Paciente: ${paciente.nombrePaciente}</h4>
                    <p><strong>Fecha:</strong> ${paciente.fecha}</p>
                    <p><strong>Hora:</strong> ${paciente.hora}</p>
                    <p><strong>Teléfono:</strong> ${paciente.telefonoPaciente}</p> 
                    <p><strong>Dirección:</strong> ${paciente.direccionPaciente}</p>
                    <p><em>Reservado por: ${paciente.usuario}</em></p> 
                `;
                listaPacientesDiv.appendChild(pacienteCard);
            });
        }
    </script>
</body>
</html>
