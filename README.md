<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>SwiftSend | Tu paquete en tiempo real</title>
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <!-- FontAwesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <!-- SweetAlert2 -->
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #e9edf2 100%);
            min-height: 100vh;
        }

        .header {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: white;
            padding: 20px 5%;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }

        .nav-container {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 20px;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo i {
            font-size: 32px;
            color: #f97316;
        }

        .logo h1 {
            font-size: 24px;
            font-weight: 700;
        }

        .logo span {
            color: #f97316;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 20px;
            background: rgba(255,255,255,0.1);
            padding: 8px 20px;
            border-radius: 50px;
        }

        .user-info i {
            font-size: 20px;
            color: #f97316;
        }

        .btn-logout {
            background: transparent;
            border: 1px solid #f97316;
            color: #f97316;
            padding: 6px 16px;
            border-radius: 30px;
            cursor: pointer;
            transition: 0.3s;
        }

        .btn-logout:hover {
            background: #f97316;
            color: white;
        }

        .hero {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            padding: 50px 5% 70px;
            text-align: center;
            color: white;
        }

        .hero h2 {
            font-size: 38px;
            margin-bottom: 15px;
        }

        .hero p {
            font-size: 18px;
            opacity: 0.9;
            max-width: 600px;
            margin: 0 auto;
        }

        .main-container {
            max-width: 1200px;
            margin: -40px auto 40px;
            padding: 0 20px;
        }

        .card {
            background: white;
            border-radius: 28px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 20px 35px -10px rgba(0,0,0,0.1);
        }

        .card h2 {
            font-size: 24px;
            margin-bottom: 20px;
            color: #0f172a;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .card h2 i {
            color: #f97316;
        }

        .form-group {
            margin-bottom: 20px;
        }

        input, textarea, select {
            width: 100%;
            padding: 14px 18px;
            border: 2px solid #e2e8f0;
            border-radius: 18px;
            font-size: 15px;
            transition: 0.3s;
            font-family: 'Inter', sans-serif;
        }

        input:focus, textarea:focus {
            outline: none;
            border-color: #f97316;
            box-shadow: 0 0 0 3px rgba(249,115,22,0.1);
        }

        .btn-primary {
            background: linear-gradient(135deg, #f97316, #ea580c);
            color: white;
            border: none;
            padding: 14px 28px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.3s;
            width: 100%;
            font-size: 16px;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(249,115,22,0.3);
        }

        .btn-secondary {
            background: #334155;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
        }

        .two-columns {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }

        @media (max-width: 768px) {
            .two-columns {
                grid-template-columns: 1fr;
            }
        }

        .estado-pedido {
            background: #f8fafc;
            border-radius: 20px;
            padding: 20px;
            margin-top: 20px;
        }

        .badge {
            display: inline-block;
            padding: 8px 18px;
            border-radius: 40px;
            font-size: 13px;
            font-weight: 600;
        }

        .badge-pendiente { background: #fef3c7; color: #d97706; }
        .badge-preparando { background: #dbeafe; color: #2563eb; }
        .badge-enviado { background: #e0f2fe; color: #0891b2; }
        .badge-en-ruta { background: #ede9fe; color: #7c3aed; }
        .badge-entregado { background: #dcfce7; color: #16a34a; }

        .map {
            height: 320px;
            border-radius: 20px;
            overflow: hidden;
            border: 1px solid #eef2ff;
            margin-top: 20px;
        }

        .progress-bar-container {
            height: 10px;
            background: #e2e8f0;
            border-radius: 10px;
            overflow: hidden;
            margin: 15px 0;
        }

        .progress-fill {
            width: 0%;
            height: 100%;
            background: linear-gradient(90deg, #f97316, #ea580c);
            transition: width 0.5s ease;
            border-radius: 10px;
        }

        .timeline {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        .timeline-step {
            text-align: center;
            flex: 1;
            min-width: 70px;
        }

        .timeline-step .dot {
            width: 40px;
            height: 40px;
            background: #e2e8f0;
            border-radius: 50%;
            margin: 0 auto 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            color: #64748b;
        }

        .timeline-step.active .dot {
            background: #f97316;
            color: white;
        }

        .timeline-step.completed .dot {
            background: #22c55e;
            color: white;
        }

        .timeline-step .label {
            font-size: 11px;
            font-weight: 500;
            color: #475569;
        }

        .pedido-item {
            background: #f8fafc;
            border-radius: 18px;
            padding: 15px;
            cursor: pointer;
            transition: 0.2s;
            border: 1px solid #e2e8f0;
            margin-bottom: 12px;
        }

        .pedido-item:hover {
            transform: translateX(5px);
            border-color: #f97316;
        }

        .notification-bell {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background: #f97316;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 5px 20px rgba(0,0,0,0.2);
            z-index: 100;
        }

        .notification-bell i {
            font-size: 28px;
            color: white;
        }

        .notification-badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background: red;
            color: white;
            border-radius: 50%;
            width: 22px;
            height: 22px;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .modal-content {
            background: white;
            border-radius: 32px;
            padding: 30px;
            max-width: 400px;
            width: 90%;
        }

        footer {
            background: #0f172a;
            color: #94a3b8;
            text-align: center;
            padding: 30px;
            margin-top: 60px;
        }
    </style>
</head>
<body>

<div class="header">
    <div class="nav-container">
        <div class="logo">
            <i class="fas fa-boxes"></i>
            <h1>Swift<span>Send</span></h1>
        </div>
        <div id="userInfoHeader" style="display: none;">
            <div class="user-info">
                <i class="fas fa-user-circle"></i>
                <span id="userNameDisplay"></span>
                <button class="btn-logout" id="logoutBtn"><i class="fas fa-sign-out-alt"></i> Salir</button>
            </div>
        </div>
        <div id="authButtons">
            <button class="btn-logout" id="openLoginBtn" style="background:#f97316; color:white; border:none;"><i class="fas fa-sign-in-alt"></i> Iniciar sesión</button>
            <button class="btn-logout" id="openRegisterBtn" style="margin-left:10px;"><i class="fas fa-user-plus"></i> Registrarse</button>
        </div>
    </div>
</div>

<section class="hero">
    <h2>Rastrea tu paquete en tiempo real</h2>
    <p>Regístrate, crea tu pedido y recibe notificaciones cuando llegue a su destino</p>
</section>

<div class="main-container">
    <div id="appContent" style="display: none;">
        <div class="two-columns">
            <div class="card">
                <h2><i class="fas fa-box-open"></i> Solicitar envío</h2>
                <div class="form-group">
                    <input type="text" id="destinatarioNombre" placeholder="Nombre del destinatario" required>
                </div>
                <div class="form-group">
                    <input type="text" id="destinatarioDireccion" placeholder="Dirección de entrega" required>
                </div>
                <div class="form-group">
                    <input type="text" id="destinatarioTelefono" placeholder="Teléfono de contacto" required>
                </div>
                <div class="form-group">
                    <textarea id="observaciones" rows="3" placeholder="Observaciones adicionales (ej: dejar en portería)"></textarea>
                </div>
                <button class="btn-primary" id="crearPedidoBtn"><i class="fas fa-paper-plane"></i> Crear pedido</button>
            </div>

            <div class="card">
                <h2><i class="fas fa-truck"></i> Mis pedidos activos</h2>
                <div id="listaPedidosContainer">
                    <p style="color: #64748b; text-align: center;">No tienes pedidos activos. Crea uno nuevo.</p>
                </div>
            </div>
        </div>

        <div class="card" id="detallePedidoCard" style="display: none;">
            <h2><i class="fas fa-info-circle"></i> Seguimiento del pedido #<span id="pedidoIdSeleccionado"></span></h2>
            <div id="detallePedido"></div>
            <div id="mapaPedido" class="map"></div>
        </div>
    </div>

    <div id="welcomeMessage" class="card" style="text-align: center;">
        <i class="fas fa-user-friends" style="font-size: 50px; color: #f97316; margin-bottom: 20px;"></i>
        <h2>¡Bienvenido a SwiftSend!</h2>
        <p style="margin: 20px 0;">Regístrate o inicia sesión para crear tu pedido y hacer seguimiento en tiempo real.</p>
        <button class="btn-primary" id="welcomeRegisterBtn" style="width: auto; padding: 12px 30px;">Registrarse gratis</button>
    </div>
</div>

<div class="notification-bell" id="notificationBell" style="display: none;">
    <i class="fas fa-bell"></i>
    <div class="notification-badge" id="notificationCount">0</div>
</div>

<div id="loginModal" class="modal">
    <div class="modal-content">
        <h2 style="margin-bottom: 20px;"><i class="fas fa-sign-in-alt"></i> Iniciar sesión</h2>
        <input type="email" id="loginEmail" placeholder="Correo electrónico" style="margin-bottom: 15px;">
        <input type="password" id="loginPassword" placeholder="Contraseña" style="margin-bottom: 20px;">
        <button class="btn-primary" id="confirmLoginBtn">Ingresar</button>
        <p style="margin-top: 15px; text-align: center;">¿No tienes cuenta? <a href="#" id="switchToRegister">Regístrate aquí</a></p>
        <button class="btn-secondary" id="closeLoginModal" style="margin-top: 10px; width: 100%;">Cancelar</button>
    </div>
</div>

<div id="registerModal" class="modal">
    <div class="modal-content">
        <h2 style="margin-bottom: 20px;"><i class="fas fa-user-plus"></i> Crear cuenta</h2>
        <input type="text" id="regNombre" placeholder="Nombre completo" style="margin-bottom: 15px;">
        <input type="email" id="regEmail" placeholder="Correo electrónico" style="margin-bottom: 15px;">
        <input type="text" id="regTelefono" placeholder="Teléfono" style="margin-bottom: 15px;">
        <input type="password" id="regPassword" placeholder="Contraseña" style="margin-bottom: 20px;">
        <button class="btn-primary" id="confirmRegisterBtn">Registrarse</button>
        <p style="margin-top: 15px; text-align: center;">¿Ya tienes cuenta? <a href="#" id="switchToLogin">Inicia sesión aquí</a></p>
        <button class="btn-secondary" id="closeRegisterModal" style="margin-top: 10px; width: 100%;">Cancelar</button>
    </div>
</div>

<footer>
    <p>© 2026 SwiftSend - Seguimiento inteligente de paquetes</p>
</footer>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
    // ==================== DATOS GLOBALES ====================
    let usuarios = [];
    let pedidos = [];
    let usuarioActual = null;
    let pedidoSeleccionado = null;
    let mapaPedido = null;
    let marcadorPedido = null;
    let intervalosSimulacion = {};

    // ==================== INICIALIZAR ====================
    function cargarDatos() {
        const usuariosGuardados = localStorage.getItem('swiftsend_usuarios');
        if (usuariosGuardados) {
            usuarios = JSON.parse(usuariosGuardados);
        } else {
            usuarios = [];
        }

        const pedidosGuardados = localStorage.getItem('swiftsend_pedidos');
        if (pedidosGuardados) {
            pedidos = JSON.parse(pedidosGuardados);
        } else {
            pedidos = [];
        }

        const sesion = localStorage.getItem('swiftsend_sesion');
        if (sesion) {
            usuarioActual = JSON.parse(sesion);
            actualizarUIUsuario();
        }
    }

    function guardarUsuarios() {
        localStorage.setItem('swiftsend_usuarios', JSON.stringify(usuarios));
    }

    function guardarPedidos() {
        localStorage.setItem('swiftsend_pedidos', JSON.stringify(pedidos));
    }

    function guardarSesion() {
        if (usuarioActual) {
            localStorage.setItem('swiftsend_sesion', JSON.stringify(usuarioActual));
        } else {
            localStorage.removeItem('swiftsend_sesion');
        }
    }

    // ==================== COORDENADAS ====================
    function obtenerCoordenadasPorEstado(estado, pedidoId) {
        const base = {
            'pendiente': [4.7110, -74.0721],
            'preparando': [4.7000, -74.0650],
            'enviado': [4.6850, -74.0550],
            'en_ruta': [4.6650 + ((pedidoId % 10) * 0.002), -74.0450],
            'entregado': [4.6500 + ((pedidoId % 8) * 0.003), -74.0300]
        };
        return base[estado] || base['pendiente'];
    }

    // ==================== MAPA ====================
    function actualizarMapaPedido(pedido) {
        if (!mapaPedido || !pedido) return;
        if (marcadorPedido) mapaPedido.removeLayer(marcadorPedido);
        
        const coords = obtenerCoordenadasPorEstado(pedido.estado, pedido.id);
        const estadoTexto = {
            'pendiente': 'Pendiente',
            'preparando': 'Preparando',
            'enviado': 'Enviado',
            'en_ruta': 'En ruta',
            'entregado': '✅ Entregado'
        };
        
        marcadorPedido = L.marker(coords).addTo(mapaPedido)
            .bindPopup(`<b>Pedido #${pedido.id}</b><br>${estadoTexto[pedido.estado]}<br>${pedido.destinatarioDireccion}`)
            .openPopup();
        mapaPedido.setView(coords, 14);
    }

    // ==================== SIMULACIÓN ====================
    function iniciarSimulacionEnvio(pedido) {
        if (intervalosSimulacion[pedido.id]) {
            clearInterval(intervalosSimulacion[pedido.id]);
        }
        
        const estados = ['pendiente', 'preparando', 'enviado', 'en_ruta', 'entregado'];
        let estadoIndex = estados.indexOf(pedido.estado);
        
        intervalosSimulacion[pedido.id] = setInterval(() => {
            if (estadoIndex < estados.length - 1) {
                estadoIndex++;
                pedido.estado = estados[estadoIndex];
                pedido.fechaActualizacion = new Date().toISOString();
                guardarPedidos();
                
                if (pedidoSeleccionado && pedidoSeleccionado.id === pedido.id) {
                    mostrarDetallePedido(pedido);
                }
                actualizarListaPedidos();
                
                if (pedido.estado === 'entregado') {
                    clearInterval(intervalosSimulacion[pedido.id]);
                    delete intervalosSimulacion[pedido.id];
                    
                    // NOTIFICACIÓN AL USUARIO
                    Swal.fire({
                        title: '🎉 ¡Paquete entregado!',
                        html: `Tu pedido <strong>#${pedido.id}</strong> ha llegado a su destino.<br>Destinatario: ${pedido.destinatarioNombre}`,
                        icon: 'success',
                        confirmButtonColor: '#f97316',
                        timer: 5000,
                        toast: true,
                        position: 'top-end',
                        showConfirmButton: false,
                        timerProgressBar: true
                    });
                    
                    // Sonido opcional (pequeño beep)
                    const audio = new Audio('data:audio/wav;base64,U3RlYWx0aCBzb3VuZA==');
                    audio.play().catch(e => console.log('Sonido no soportado'));
                }
            } else {
                clearInterval(intervalosSimulacion[pedido.id]);
                delete intervalosSimulacion[pedido.id];
            }
        }, 7000);
    }

    // ==================== CREAR PEDIDO ====================
    function crearPedido() {
        if (!usuarioActual) {
            Swal.fire('Inicia sesión', 'Debes iniciar sesión para crear un pedido', 'warning');
            return;
        }
        
        const destinatarioNombre = document.getElementById('destinatarioNombre').value.trim();
        const destinatarioDireccion = document.getElementById('destinatarioDireccion').value.trim();
        const destinatarioTelefono = document.getElementById('destinatarioTelefono').value.trim();
        const observaciones = document.getElementById('observaciones').value.trim();
        
        if (!destinatarioNombre || !destinatarioDireccion || !destinatarioTelefono) {
            Swal.fire('Campos incompletos', 'Completa todos los campos del destinatario', 'error');
            return;
        }
        
        const nuevoId = pedidos.length > 0 ? Math.max(...pedidos.map(p => p.id)) + 1 : 1000;
        
        const nuevoPedido = {
            id: nuevoId,
            usuarioId: usuarioActual.id,
            destinatarioNombre: destinatarioNombre,
            destinatarioDireccion: destinatarioDireccion,
            destinatarioTelefono: destinatarioTelefono,
            observaciones: observaciones || 'Sin observaciones',
            estado: 'pendiente',
            fechaCreacion: new Date().toISOString(),
            fechaActualizacion: new Date().toISOString()
        };
        
        pedidos.push(nuevoPedido);
        guardarPedidos();
        
        document.getElementById('destinatarioNombre').value = '';
        document.getElementById('destinatarioDireccion').value = '';
        document.getElementById('destinatarioTelefono').value = '';
        document.getElementById('observaciones').value = '';
        
        actualizarListaPedidos();
        
        Swal.fire({
            title: '✅ Pedido creado',
            text: `Pedido #${nuevoId} registrado. El seguimiento comenzará automáticamente.`,
            icon: 'success',
            confirmButtonColor: '#f97316'
        });
        
        iniciarSimulacionEnvio(nuevoPedido);
        mostrarDetallePedido(nuevoPedido);
    }

    // ==================== MOSTRAR DETALLE ====================
    function mostrarDetallePedido(pedido) {
        pedidoSeleccionado = pedido;
        document.getElementById('detallePedidoCard').style.display = 'block';
        document.getElementById('pedidoIdSeleccionado').innerText = pedido.id;
        
        const estadoTexto = {
            'pendiente': 'Pendiente de confirmación',
            'preparando': 'Preparando paquete',
            'enviado': 'Enviado a transporte',
            'en_ruta': 'En ruta hacia destino',
            'entregado': 'Entregado'
        };
        
        const estadoClass = `badge-${pedido.estado.replace('_', '-')}`;
        
        const progresoMap = {
            'pendiente': 0,
            'preparando': 25,
            'enviado': 50,
            'en_ruta': 75,
            'entregado': 100
        };
        const progreso = progresoMap[pedido.estado] || 0;
        
        const html = `
            <div class="estado-pedido">
                <p><strong><i class="fas fa-user"></i> Destinatario:</strong> ${pedido.destinatarioNombre}</p>
                <p><strong><i class="fas fa-map-marker-alt"></i> Dirección:</strong> ${pedido.destinatarioDireccion}</p>
                <p><strong><i class="fas fa-phone"></i> Teléfono:</strong> ${pedido.destinatarioTelefono}</p>
                <p><strong><i class="fas fa-comment"></i> Observaciones:</strong> ${pedido.observaciones}</p>
                <p><strong><i class="fas fa-tag"></i> Estado:</strong> <span class="badge ${estadoClass}">${estadoTexto[pedido.estado]}</span></p>
            </div>
            <div class="progress-bar-container">
                <div class="progress-fill" style="width: ${progreso}%;"></div>
            </div>
            <div class="timeline">
                <div class="timeline-step ${progreso >= 0 ? 'completed' : ''} ${pedido.estado === 'pendiente' ? 'active' : ''}">
                    <div class="dot"><i class="fas fa-clock"></i></div>
                    <div class="label">Pendiente</div>
                </div>
                <div class="timeline-step ${progreso >= 25 ? 'completed' : ''} ${pedido.estado === 'preparando' ? 'active' : ''}">
                    <div class="dot"><i class="fas fa-box"></i></div>
                    <div class="label">Preparando</div>
                </div>
                <div class="timeline-step ${progreso >= 50 ? 'completed' : ''} ${pedido.estado === 'enviado' ? 'active' : ''}">
                    <div class="dot"><i class="fas fa-paper-plane"></i></div>
                    <div class="label">Enviado</div>
                </div>
                <div class="timeline-step ${progreso >= 75 ? 'completed' : ''} ${pedido.estado === 'en_ruta' ? 'active' : ''}">
                    <div class="dot"><i class="fas fa-truck"></i></div>
                    <div class="label">En ruta</div>
                </div>
                <div class="timeline-step ${progreso >= 100 ? 'completed' : ''} ${pedido.estado === 'entregado' ? 'active' : ''}">
                    <div class="dot"><i class="fas fa-check-circle"></i></div>
                    <div class="label">Entregado</div>
                </div>
            </div>
        `;
        
        document.getElementById('detallePedido').innerHTML = html;
        
        if (mapaPedido) {
            mapaPedido.remove();
        }
        mapaPedido = L.map('mapaPedido').setView([4.7110, -74.0721], 12);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '© OpenStreetMap'
        }).addTo(mapaPedido);
        actualizarMapaPedido(pedido);
    }

    // ==================== LISTAR PEDIDOS ====================
    function actualizarListaPedidos() {
        if (!usuarioActual) return;
        
        const pedidosUsuario = pedidos.filter(p => p.usuarioId === usuarioActual.id);
        const container = document.getElementById('listaPedidosContainer');
        
        if (pedidosUsuario.length === 0) {
            container.innerHTML = '<p style="color: #64748b; text-align: center;">No tienes pedidos. Crea uno nuevo.</p>';
            return;
        }
        
        let html = '';
        pedidosUsuario.forEach(p => {
            const estadoClass = `badge-${p.estado.replace('_', '-')}`;
            const estadoTexto = {
                'pendiente': 'Pendiente',
                'preparando': 'Preparando',
                'enviado': 'Enviado',
                'en_ruta': 'En ruta',
                'entregado': 'Entregado'
            };
            html += `
                <div class="pedido-item" onclick='mostrarDetallePedido(JSON.parse(JSON.stringify(pedidos.find(ped => ped.id === ${p.id}))))'>
                    <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap;">
                        <div>
                            <strong><i class="fas fa-hashtag"></i> #${p.id}</strong>
                            <span style="margin-left: 10px;"><i class="fas fa-user"></i> ${p.destinatarioNombre}</span>
                        </div>
                        <span class="badge ${estadoClass}">${estadoTexto[p.estado]}</span>
                    </div>
                    <div style="font-size: 13px; color: #64748b; margin-top: 8px;">
                        <i class="fas fa-map-marker-alt"></i> ${p.destinatarioDireccion.substring(0, 50)}${p.destinatarioDireccion.length > 50 ? '...' : ''}
                    </div>
                </div>
            `;
        });
        container.innerHTML = html;
    }

    // ==================== REGISTRO ====================
    function registrarUsuario() {
        const nombre = document.getElementById('regNombre').value.trim();
        const email = document.getElementById('regEmail').value.trim();
        const telefono = document.getElementById('regTelefono').value.trim();
        const password = document.getElementById('regPassword').value;
        
        if (!nombre || !email || !telefono || !password) {
            Swal.fire('Error', 'Completa todos los campos', 'error');
            return;
        }
        
        if (usuarios.find(u => u.email === email)) {
            Swal.fire('Error', 'El correo ya está registrado', 'error');
            return;
        }
        
        const nuevoUsuario = {
            id: usuarios.length + 1,
            nombre: nombre,
            email: email,
            telefono: telefono,
            password: password
        };
        
        usuarios.push(nuevoUsuario);
        guardarUsuarios();
        
        Swal.fire('Éxito', 'Cuenta creada correctamente. Ahora inicia sesión.', 'success');
        cerrarModales();
        abrirLoginModal();
    }

    // ==================== LOGIN ====================
    function loginUsuario() {
        const email = document.getElementById('loginEmail').value.trim();
        const password = document.getElementById('loginPassword').value;
        
        const usuario = usuarios.find(u => u.email === email && u.password === password);
        
        if (!usuario) {
            Swal.fire('Error', 'Correo o contraseña incorrectos', 'error');
            return;
        }
        
        usuarioActual = usuario;
        guardarSesion();
        cerrarModales();
        actualizarUIUsuario();
        
        Swal.fire('Bienvenido', `Hola ${usuario.nombre}`, 'success');
    }

    // ==================== UI ====================
    function actualizarUIUsuario() {
        if (usuarioActual) {
            document.getElementById('appContent').style.display = 'block';
            document.getElementById('welcomeMessage').style.display = 'none';
            document.getElementById('userInfoHeader').style.display = 'block';
            document.getElementById('authButtons').style.display = 'none';
            document.getElementById('userNameDisplay').innerText = usuarioActual.nombre;
            actualizarListaPedidos();
        } else {
            document.getElementById('appContent').style.display = 'none';
            document.getElementById('welcomeMessage').style.display = 'block';
            document.getElementById('userInfoHeader').style.display = 'none';
            document.getElementById('authButtons').style.display = 'flex';
        }
    }

    function logout() {
        usuarioActual = null;
        guardarSesion();
        actualizarUIUsuario();
        document.getElementById('detallePedidoCard').style.display = 'none';
        if (mapaPedido) {
            mapaPedido.remove();
            mapaPedido = null;
        }
        Swal.fire('Sesión cerrada', 'Hasta pronto', 'info');
    }

    // ==================== MODALES ====================
    function abrirLoginModal() {
        document.getElementById('loginModal').style.display = 'flex';
        document.getElementById('loginEmail').value = '';
        document.getElementById('loginPassword').value = '';
    }
    
    function abrirRegisterModal() {
        document.getElementById('registerModal').style.display = 'flex';
        document.getElementById('regNombre').value = '';
        document.getElementById('regEmail').value = '';
        document.getElementById('regTelefono').value = '';
        document.getElementById('regPassword').value = '';
    }
    
    function cerrarModales() {
        document.getElementById('loginModal').style.display = 'none';
        document.getElementById('registerModal').style.display = 'none';
    }

    // ==================== EVENTOS ====================
    document.getElementById('openLoginBtn').addEventListener('click', abrirLoginModal);
    document.getElementById('openRegisterBtn').addEventListener('click', abrirRegisterModal);
    document.getElementById('welcomeRegisterBtn').addEventListener('click', abrirRegisterModal);
    document.getElementById('confirmLoginBtn').addEventListener('click', loginUsuario);
    document.getElementById('confirmRegisterBtn').addEventListener('click', registrarUsuario);
    document.getElementById('closeLoginModal').addEventListener('click', cerrarModales);
    document.getElementById('closeRegisterModal').addEventListener('click', cerrarModales);
    document.getElementById('logoutBtn').addEventListener('click', logout);
    document.getElementById('crearPedidoBtn').addEventListener('click', crearPedido);
    
    document.getElementById('switchToRegister').addEventListener('click', (e) => {
        e.preventDefault();
        cerrarModales();
        abrirRegisterModal();
    });
    
    document.getElementById('switchToLogin').addEventListener('click', (e) => {
        e.preventDefault();
        cerrarModales();
        abrirLoginModal();
    });
    
    // Inicializar
    cargarDatos();
</script>
</body>
</html>
